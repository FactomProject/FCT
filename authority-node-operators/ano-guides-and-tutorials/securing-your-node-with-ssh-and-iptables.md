# Securing your node with SSH and iptables

[![](http://img.youtube.com/vi/7JBx-ZmBpz4/0.jpg)](http://www.youtube.com/watch?v=7JBx-ZmBpz4)

{% hint style="info" %}
This tutorial is fully covered in the video shown above.
{% endhint %}

Now it's time to secure your node by hardening SSH access and setting up the firewall. This tutorial covers what is considered minimum level of security for your node, or any production server in general \(subjective opinion\). 

## SSH key-pair based login

First off you should be using a SSH key-pair when you log into your node. Go ahead and make a folder called ssh and a file named `authorized_keys`:

```bash
mkdir .ssh
nano .ssh/autorized_keys
```

Now generate a public key, with for example PuTTY Key generator, and paste it into the `authorized_keys` file. 

{% hint style="info" %}
For added security you can limit the SSH access by IP as well, preferably you'll be managing your node from only one IP address. Instead of pasting your key into the file do the following:

`from="YOUR_IP" SSH-KEY`
{% endhint %}

Once you have your keys setup continue with editing the SSH config file.

```bash
nano /etc/ssh/sshd_config
```

Recommended changes are:

* `AddressFamily` set to `inet` , this will prevent your node from listening on IPv6.
* `PermitRootLogin` set to `no`. Self-explanatory, denies root login.
* `PasswordAuthentication` set to `no` , once you have setup key-based login you won't be login in with username/password. 

## Setting up the firewall with iptables

All docker swarm communications occur over TLS using a self-signed TLS certificate. Due to the way iptables and docker work you can't use the `INPUT` chain to block access to apps running in a docker container as it's not a local destination but a `FORWARD` one. By default when you map a port into a docker container it opens up to `any` host. To restrict access we need to add our rules in the `DOCKER-USER` chain. More info can be found [here](https://docs.docker.com/network/iptables/). 

Open TCP port `2376` _only to_ `52.48.130.243` & `18.203.51.247` for secure Docker engine communication. This port is required for Docker Machine to work. Docker Machine is used to orchestrate Docker hosts, as this is a local service we use the `INPUT`chain.

In addition, the following ports must be opened for `factomd` to function which we add to the `DOCKER-USER` chain:

* `2222` to `52.48.130.243` & `18.203.51.247`, which is the SSH port used by the `ssh` container
* `8088` to `52.48.130.243` & `18.203.51.247`, the `factomd` API port
* `8090` to `52.48.130.243` & `18.203.51.247`, the `factomd` Control panel
* `8108` to the world, the `factomd` main-net port

In the above video, we changed the INPUT policy to drop and included a management IP, this would create the basic ruleset:

```bash
sudo iptables -A INPUT -s 52.48.130.243/32 -p tcp -m tcp --dport 2376 -j ACCEPT
sudo iptables -A INPUT -s 18.203.51.247/32 -p tcp -m tcp --dport 2376 -j ACCEPT
sudo iptables -A INPUT -s <management-ip>/32 -p tcp -m tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -P INPUT DROP

sudo iptables -F DOCKER-USER
sudo iptables -A DOCKER-USER -p tcp -m tcp --dport 8108 -j ACCEPT
sudo iptables -A DOCKER-USER -s <management-ip>/32 -j ACCEPT
sudo iptables -A DOCKER-USER -s 52.48.130.243/32 -p tcp -m tcp --dport 8090 -j ACCEPT
sudo iptables -A DOCKER-USER -s 52.48.130.243/32 -p tcp -m tcp --dport 2222 -j ACCEPT
sudo iptables -A DOCKER-USER -s 52.48.130.243/32 -p tcp -m tcp --dport 8088 -j ACCEPT
sudo iptables -A DOCKER-USER -s 18.203.51.247/32 -p tcp -m tcp --dport 8090 -j ACCEPT
sudo iptables -A DOCKER-USER -s 18.203.51.247/32 -p tcp -m tcp --dport 2222 -j ACCEPT
sudo iptables -A DOCKER-USER -s 18.203.51.247/32 -p tcp -m tcp --dport 8088 -j ACCEPT
sudo iptables -A DOCKER-USER -p tcp -m tcp --dport 8090 -j DROP
sudo iptables -A DOCKER-USER -p tcp -m tcp --dport 2222 -j DROP
sudo iptables -A DOCKER-USER -p tcp -m tcp --dport 8088 -j DROP
sudo iptables -A DOCKER-USER -j RETURN
```

Last, but not least don't forget to [**save the rules**](https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands#saving-rules).

