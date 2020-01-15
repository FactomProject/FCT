# Installation

[![Factom protocol: Getting started with the basics](http://img.youtube.com/vi/luJucKvHsB4/0.jpg)​](https://www.youtube.com/watch?v=luJucKvHsB4)​

{% hint style="info" %}
This complete tutorial is fully covered in the video shown above.
{% endhint %}

## Before you begin

Deployment of the testnet software will be done using a program called «Docker».

Docker is compatible with Linux, MacOS and Windows, as well as with AWS-/Azure-cloud services. It creates a virtual container that contains and runs the software provided by Factom.

Step by step instructions will be provided for downloading and installing an ordinary node on Linux, MacOS and Windows, as well as for the Linux version of the Authority server.

The main support channel is in the Factom Community [Discord channel](https://discord.gg/S4ghP9C) under the category "COMMUNITY-TESTNET".

If you are an experienced user you may skip these instructions, and head directly to the [instructions on the factomd-testnet-toolkit GitHub page](https://github.com/FactomProject/factomd-testnet-toolkit).

## Install Ubuntu Server edition

Download [Ubuntu Server 18.04 LTS](https://www.ubuntu.com/download/server).

{% hint style="info" %}
If you are on a 32-bit system you need the 32-bit installer.
{% endhint %}

Make a bootable USB by using a tool like [Etcher ](https://www.balena.io/etcher/)or [Rufus](https://rufus.ie/). 

Boot from the USB-stick you made, and install Ubuntu 

{% hint style="info" %}
Include a graphical interface if you are not Linux savvy \(Ubuntu-desktop or GNOME\).
{% endhint %}

## Install Docker

 Follow the excellent Docker install-guide [here](https://docs.docker.com/install/linux/docker-ce/ubuntu/). The guide involves removing any old versions of docker, adding the docker-CE repository and then installing it.

 After the installation is complete you need to grant your Ubuntu-user permission to actually use the docker program. This is done by executing the following commands:

```bash
sudo usermod -aG docker $USER
```

{% hint style="info" %}
After running the commands above **you need to log out of Ubuntu and log back in** **for the change to take effect.** 
{% endhint %}

Verify that the command was successful by opening the terminal and running:

```bash
docker run hello-world
```

This should then generate the «hello-world» image.

## Securing your node

[![Factom protocol: Securing your node with SSH and iptables](http://img.youtube.com/vi/VIr9OT7ZRp0/0.jpg)​](https://www.youtube.com/watch?v=VIr9OT7ZRp0)​

{% hint style="info" %}
This part of the tutorial is covered in the video shown above.
{% endhint %}

### Firewall

In order to have your node join the swarm \(or even function properly\), you need to expose a couple of ports. The set up depends on whether you use an external firewall \(or NAT\), such as AWS or hosting at home, or rely on the node's own firewall to secure it \(most VPS-services\). If you do not want to join the authority set, only port 8110 needs to be opened to the public.

#### External firewall / NAT

No firewall needs to be configured on the node itself, but it can still be set up for added security in case your external firewall gets compromised. You need to allow access to port 2376, 2222 and 8088 _ONLY TO_ 54.171.68.124. Failure to do this properly can compromise your node. Port 8090 to public is beneficial for testnet debugging. Port 8110 is required to be open to the public, this is the port the network communicates on. The steps to do this varies greatly by your individual set up \(NAT or not, firewall/router model, etc..\)

#### Internal firewall

In order to join the swarm, first ensure that your firewall rules allow access on the following ports. All swarm communications occur over a self-signed TLS certificate. Due to the way iptables and docker work you cannot use the `INPUT` chain to block access to apps running in a docker container as it's not a local destination but a `FORWARD` destination. By default when you map a port into a docker container it opens up to `any` host. To restrict access we need to add our rules in the `DOCKER-USER` chain [reference](https://docs.docker.com/network/iptables/).

* TCP port `2376` _only to_ `54.171.68.124` for secure Docker engine communication. This port is required for Docker Machine to work. Docker Machine is used to orchestrate Docker hosts. As this is a local service we use the `INPUT` chain.

In addition, the following ports must be opened for factomd to function which we add to the `DOCKER-USER` chain:

* `2222` to `54.171.68.124`, which is the SSH port used by the `ssh` container
* `8088` to `54.171.68.124`, the factomd API port
* `8090` to `0.0.0.0`, the factomd Control panel
  * Keeping this open to the world is beneficial on testnet for debugging purposes
* `8110` to `0.0.0.0`, the factomd testnet port
* **OPTIONAL**: `8088` to `178.62.125.252`, the factomd API port to [fct.tools](https://fct.tools/) monitoring server

An example using `iptables`with the optional setting at line 5:

```bash
sudo iptables -A INPUT ! -s 54.171.68.124/32 -p tcp -m tcp --dport 2376 -m conntrack --ctstate NEW,ESTABLISHED -j REJECT --reject-with icmp-port-unreachable
sudo iptables -A DOCKER-USER ! -s 54.171.68.124/32  -i <external if> -p tcp -m tcp --dport 8090 -j REJECT --reject-with icmp-port-unreachable
sudo iptables -A DOCKER-USER ! -s 54.171.68.124/32  -i <external if> -p tcp -m tcp --dport 2222 -j REJECT --reject-with icmp-port-unreachable
sudo iptables -A DOCKER-USER ! -s 54.171.68.124/32  -i <external if> -p tcp -m tcp --dport 8088 -j REJECT --reject-with icmp-port-unreachable
sudo iptables -A DOCKER-USER ! -s 178.62.125.252/32  -i <external if> -p tcp -m tcp --dport 8088 -j REJECT --reject-with icmp-port-unreachable
sudo iptables -A DOCKER-USER -p tcp -m tcp --dport 8110 -j ACCEPT
```

{% hint style="info" %}
Replace `<external if>` with the name of the interface you use to connect to the internet eg. `eth0` or `ens0`. To see interfaces use **`ip addr list`**
{% endhint %}

{% hint style="warning" %}
Don't forget to [save](https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands#saving-rules) the rules!
{% endhint %}

### SSH key-pair based login

#### 1. Create an authentication key-pair

**Linux**

This is done on your local computer, not your node, and will create a 4096-bit RSA key-pair. During creation, you will be given the option to encrypt the private key with a passphrase. This means that it cannot be used without entering the passphrase, unless you save it to your local desktop’s keychain manager. We suggest you use the key-pair with a passphrase, but you can leave this field blank if you don’t want to use one.

```text
ssh-keygen -b 4096
```

Press Enter to use the default names `id_rsa` and `id_rsa.pub` in `/home/your_username/.ssh` before entering your passphrase.

Now copy your key to your node \(replace the username and ip with appropriate values\)

```bash
ssh-copy-id USERNAME@IP
```

Exit and log back in to your node. If you specified a passphrase, you need to enter it here.

**Windows**

Download and install [PuTTy ](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)\(use the MSI installer as it includes puttygen\).

Select RSA and increase to a 4096-bits key in the bottom right field and generate a key. Type in a passphrase \(optional, recommended\). Now save your keys.

Now copy the entire public key, it starts with `ssh-rsa` and ends with `==` followed by the key comment. On your node, create your .ssh folder if it does not already exist. Now create and/or edit the file `./ssh/authorized_keys` and paste your key here.

The next time you use PuTTy to connect, go to your Connection -&gt; SSH -&gt; Auth setting and browse to the PRIVATE key you saved earlier. Save the connection and try to connect. You should now be able to connect using your SSH key instead of password.

#### 2. SSH Daemon options

Edit /etc/ssh/sshd\_config using your favorite editor:

```text
sudo nano /etc/ssh/sshd_config
```

Below are a handful of settings we recommend setting:

```text
AddressFamily inet                  # listen only on IPv4
PermitRootLogin no                  # the most important setting, do not allow root login
PasswordAuthentication no           # disable password login
PubkeyAthentication yes             # enable keypair login
AuthorizedKeysFile .ssh/authorized_keys # keyfile location
```

## Setting up and installing factomd

[![Factom protocol: Choose VPS and join the Docker Swarm](http://img.youtube.com/vi/Qghv05RCMcE/0.jpg)​](https://www.youtube.com/watch?v=Qghv05RCMcE)​

{% hint style="info" %}
This part of tutorial is covered in the video shown above.
{% endhint %}

### Storing the docker swarm certificate and key

Make sure you store the docker swarm testnet key and certificate on your system.  The files can be found at [here](https://github.com/FactomProject/factomd-testnet-toolkit/tree/master/tls). You can store these files in the directory /etc/docker for instance:

You can store these files in the directory /etc/docker for instance: 

```bash
sudo mkdir -p /etc/docker
sudo wget https://raw.githubusercontent.com/FactomProject/factomd-testnet-toolkit/master/tls/cert.pem -O /etc/docker/factom-testnet-cert.pem
sudo wget https://raw.githubusercontent.com/FactomProject/factomd-testnet-toolkit/master/tls/key.pem -O /etc/docker/factom-testnet-key.pem
sudo chmod 644 /etc/docker/factom-testnet-cert.pem
sudo chmod 440 /etc/docker/factom-testnet-key.pem
sudo chgrp docker /etc/docker/*.pem
```

Now you should  have the files with the correct permissions set.

{% hint style="warning" %}
**Please note** that in the rest of this tutorial it's assumed you stored the files using the `/etc/docker` location and with the above names. If not, please adjust the commands below involving the certificate and keys.
{% endhint %}

### Configuring and running the docker engine

Configure the docker daemon using a default config file, located at `/etc/docker/daemon.json` . Create this file if it doesn't exist. Copy the following into the file:

{% code title="/etc/docker/daemon.json" %}
```bash
{
  "tls": true,
  "tlscert": "/etc/docker/factom-testnet-cert.pem",

  "tlskey": "/etc/docker/factom-testnet-key.pem",
  "hosts": ["tcp://0.0.0.0:2376", "unix:///var/run/docker.sock"]
}
```
{% endcode %}

{% hint style="info" %}
If your system has multiple IP addresses you can select which IP it should listen on by editing `"hosts".` 
{% endhint %}

Now you'll need to replace the standard docker start command. Run the following command:

```bash
sudo systemctl edit docker.service
```

 The above command creates an override directory at `/etc/systemd/system/docker.service.d/` and an override file called `override.conf` \(which is open on your terminal now\).  Copy and paste the following:

{% code title="override.conf" %}
```bash
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd
```
{% endcode %}

Exit and save the file. Now reload the docker configuration and the `docker.service`:

```bash
sudo systemctl daemon-reload
```

Docker should now be configured and ready. You can test if it runs correctly with the following:

```bash
sudo systemctl restart docker
sudo systemctl status docker
```

It should restart with no errors appearing and you should see that the `override.conf` file has been loaded. 

### Creating the Factomd volumes

Factomd relies on two volumes, `factom_database` and `factom_keys`. Please create these before joining the swarm:

```bash
docker volume create factom_database
docker volume create factom_keys
```

These volumes are there to make sure that you can remove or delete the container itself but the database and the keys are still persistent on the system.

For running a main-net authority node we strongly recommend syncing the database from scratch. If you do already have a recently synced main-net node and would like to avoid re-syncing, run:

```bash
sudo cp -r <path to your database> /var/lib/docker/volumes/factom_database/_data .
```

The directory in \_data after the copy should be custom-database, as the volume is mounted at `$HOME/.factom/m2`.

### Joining the docker swarm

Finally, to join the swarm, run the following command:

```bash
docker swarm join --token SWMTKN-1-0bv5pj6ne5sabqnt094shexfj6qdxjpuzs0dpigckrsqmjh0ro-87wmh7jsut6ngmn819ebsqk3m 54.171.68.124:2377
```

{% hint style="info" %}
Joining as a worker means you have no ability to control containers on another node.
{% endhint %}

Once you have joined the swarm network, you will be issued a control panel login by the testnet adminstrator. Please submit this [form](https://docs.google.com/forms/d/e/1FAIpQLSd-t33chnGOyLZ6kJ-QC-L0EgOExzY7GQ8y9e0I0E4AIbdKBQ/viewform) and a staff member will contact you as soon as possible.

{% hint style="danger" %}
Only accept logins at [https://testnet.federation.factomd.com](https://testnet.federation.factomd.com/). Any other login endpoints are fraudulent and not to be trusted.
{% endhint %}

### Starting the factomd container

{% hint style="warning" %}
**Please note**: There is a version for the Factom software in the next command. Make sure you run the correct and latest announced version from the Discord \#operators-announcement channel
{% endhint %}

Run the following command _exactly:_

```bash
docker run -d --name "factomd" -v "factom_database:/root/.factom/m2" -v "factom_keys:/root/.factom/private" -p "8088:8088" -p "8090:8090" -p "8110:8110" -l "name=factomd" factominc/factomd:v6.3.1-rc1-alpine -broadcastnum=16 -network=CUSTOM -customnet=fct_community_test -startdelay=600 -faulttimeout=120 -config=/root/.factom/private/factomd.conf
```

{% hint style="info" %}
If you want the Factomd container to start at system boot \(reboots\) you can add the following parameter to the command above:

`--restart unless-stopped`
{% endhint %}

After this your node will be started. You can check for the existence of a Factom container using the command `docker ps`. 

**You're now almost ready to be included in the testnet**. Stop the `factomd` container with `docker stop factomd` and download the `factomd.conf` file [here](https://github.com/FactomProject/factomd-testnet-toolkit/blob/master/factomd.conf.EXAMPLE), or run:

```bash
wget -O factomd.conf https://raw.githubusercontent.com/FactomProject/factomd-testnet-toolkit/master/factomd.conf.EXAMPLE
```

This will download and save the file to your current folder. Now place the config file in `/var/lib/docker/volumes/factom_keys/_data`  by running \(if the file is where you're currently at\):

```bash
sudo mv factomd.conf /var/lib/docker/volumes/factom_keys/_data/factomd.conf
```

Now you're free to start the `factomd` container again with `docker start factomd`. 

{% hint style="info" %}
If you check the currently running docker containers you'll see a container named `factominc/filebeat:m3-debug` , this is generally a good sign as it means the portainer system has successfully connected and started a container remotely. 
{% endhint %}

{% hint style="warning" %}
Please wait for your node to be fully synced by checking the control panel node sync statuses to be 100% before performing any next steps. Please also regard the initial wait period of 20 minutes before doing anything with your node.
{% endhint %}

## Generating your server identity

[![Factom protocol: Generating your server identity](http://img.youtube.com/vi/g9FzNtSB7I4/0.jpg)​](https://www.youtube.com/watch?v=g9FzNtSB7I4)​

{% hint style="info" %}
This part of the tutorial is covered in the video shown above.
{% endhint %}

This tutorial assumes that you've setup Ubuntu that is running the _Factom Daemon_. If you haven't, please check out the _getting started with the basics_ tutorial first.

You can check if the daemon is up by running `docker ps` . A container with the image `factominc/factomd:vX.xx` should show up where `vX.xx` is your current Factomd version. 

{% hint style="info" %}
Latest version of the Factom Daemon can be found [here.](https://github.com/FactomProject/distribution/releases)
{% endhint %}

To be able to join the testnet as an authority server you will need a «personal» server identity. The identity is generated using the serveridentity program. Entry Credits are required to create your identity to the blockchain.

### Create and fund a new Test Credit address

#### **Using the commandline**

* Start`factom-walletd`
* Create a new TC address `factom-cli -s=localhost:port newecaddress`
* Visit the [Faucet ](https://faucet.factoid.org/)and input your generated address.
* Verify that it has been funded: `factom-cli -s=localhost:port balance ECXXXXXXXXXXXXXX`
* Export your address: `factom-cli -s=localhost:port exportaddresses`

{% hint style="info" %}
Take note of your private TC address for the next step, beginning with ****`Esxxxxxxxxxxxxxxxx`
{% endhint %}

#### **Using the Enterprise Wallet:**

* Create a new Entry Credit address in your Address Book.
* Visit the [Faucet ](https://faucet.factoid.org/)and input your generated address
* Verify that it has been funded by checking the balance in the wallet
* Click the pencil-icon on your EC wallet and click "Display Private Key", you will need this for the next step.
* Also create a new Factoid address to set up efficiency and payout

### Building the serveridentity tool

The identity is generated using the serveridentity tool, but first you need to build said tool.

Start off by installing _git_, _golang-go_ and _golang-glide_:

```bash
sudo apt-get install git golang-go golang-glide
```

next up edit `~/.profile` with for example nano:

```bash
nano .profile
```

and add the following lines to the bottom:

{% code title=".profile" %}
```bash
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```
{% endcode %}

{% hint style="danger" %}
Open a new terminal for these changes to take effect. You might even need to re-log.
{% endhint %}

Next up clone the serveridentity tool:

```bash
mkdir -p $GOPATH/src/github.com/FactomProject/
cd $GOPATH/src/github.com/FactomProject/
git clone https://github.com/FactomProject/serveridentity.git
cd serveridentity
glide install
go install
cd signwithed25519
go install
```

Now you should have two files in the `go/bin/`folder named `serveridentity` and `signedwithed25519`. These are the files you need to generate your server identity. 

### Create the server identity

Now run the following:

```bash
./serveridentity full elements Esxxxxxxxxxxxxxxxxxxxxx  -n={OPTIONAL_FILENAME} -f
```

This will generate numerous keys which are printed to stdout, two files will also be produced:

* Script to add the Identity to the blockchain
  * Script that utilizes factom-cli
  * Name of script is by default `fullidentity.sh` or `{OPTIONAL_FILENAME}.sh` if provided.
* Config file needed for the server
  * Place in `~/factom/m2` and rename to `factomd.conf`
  * Name of config is by default `fullidentity.conf` or `{OPTIONAL_FILENAME}.conf` if provided

Record the private keys printed out to the screen on paper or long term storage. These are used to control your identity in the future. Level 4 is the highest security and level 1 will be used to do more operations. 

Make sure factomd is running and run `factom-walletd` in a terminal window.  The factom-cli commands in important.sh need to be run. Change all lines with `factom-cli …` to now read `factom-cli -s=localhost:port ...`. Import the EC address to your wallet: 

```bash
factom-cli importaddress Esxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Check the balance of your addresses:

```bash
 factom-cli listaddresses
```

Run the important.sh script:

```bash
chmod +x important.sh
./important.sh
```

Check the explorer that the new identity chains were created 10 minutes later. You can now paste the contents of the `important.conf` file into your `factomd.conf`, normally located at `/var/docker/volumes/factom_keys/_data/factomd.conf`.

