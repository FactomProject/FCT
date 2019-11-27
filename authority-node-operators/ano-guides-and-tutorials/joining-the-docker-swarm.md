# Joining the docker swarm

[![Factom protocol: Choose VPS and join the Docker Swarm](http://img.youtube.com/vi/Qghv05RCMcE/0.jpg)](https://www.youtube.com/watch?v=Qghv05RCMcE)

{% hint style="info" %}
This tutorial is fully covered in the video shown above. Some parts may differ from the text version, such as the swarm certificates and keys, these have been updated since the making of this video.
{% endhint %}

## Installing docker

First you need to install docker. Please follow the instructions [here](https://docs.docker.com/install/linux/docker-ce/ubuntu/) to install docker-CE \(Community Edition\) to your system. Once it's installed run the following:

```bash
usermod -aG docker $USER
```

{% hint style="warning" %}
Make sure you logout / login again, as otherwise your current terminal session will not be updated.
{% endhint %}

## Storing the docker swarm certificate and key

The Docker Swarm certificate and key is used for authenticating with the docker swarm. These will verify clients with the certificate, as well as encrypt communication with the Docker API using TLS. 

{% hint style="info" %}
If you're trying to join the test-net swarm the keys can be found [here](https://github.com/FactomProject/factomd-testnet-toolkit/tree/master/tls).
{% endhint %}

You can store these files in the directory `/etc/docker` for instance: 

```bash
sudo mkdir -p /etc/docker
sudo wget https://raw.githubusercontent.com/FactomProject/factomd-authority-toolkit/master/tls/cert_exp_5-14-21.pem -O /etc/docker/factom-mainnet-cert_exp_5-14-21.pem
sudo wget https://raw.githubusercontent.com/FactomProject/factomd-authority-toolkit/master/tls/key_exp_5-14-21.pem -O /etc/docker/factom-mainnet-key_exp_5-14-21.pem
sudo wget https://raw.githubusercontent.com/FactomProject/factomd-authority-toolkit/master/tls/ca_exp_5-14-21.pem -O /etc/docker/factom-mainnet-ca_exp_5-14-21.pem
sudo chmod 644 /etc/docker/factom-mainnet-cert_exp_5-14-21.pem
sudo chmod 440 /etc/docker/factom-mainnet-key_exp_5-14-21.pem /etc/docker/factom-mainnet-ca_exp_5-14-21.pem
sudo chgrp docker /etc/docker/*.pem
```

Now you should  have the files with the correct permissions set.

{% hint style="warning" %}
**Please note** that in the rest of this tutorial it's assumed you stored the files using the `/etc/docker` location and with the above names. If not, please adjust the commands below involving the certificate and keys.
{% endhint %}

## Configuring and running the docker engine

Configure the docker daemon using a default config file, located at `/etc/docker/daemon.json` . Create this file if it doesn't exist. Copy the following into the file:

{% code title="/etc/docker/daemon.json" %}
```bash
{
  "tlsverify": true,
  "tlscert": "/etc/docker/factom-mainnet-cert_exp_5-14-21.pem",
  "tlskey": "/etc/docker/factom-mainnet-key_exp_5-14-21.pem",
  "tlscacert":"/etc/docker/factom-mainnet-ca_exp_5-14-21.pem",
  
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

## Creating the Factomd volumes

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

The directory in \_data after the copy should be main-database, as the volume is mounted at `$HOME/.factom/m2`.

## Joining the docker swarm

Finally, to join the swarm, run the following command:

```bash
docker swarm join --token SWMTKN-1-5ct5plmbn1ombbjqp8ql8hq93jkof6246suzast5n1gfwa083b-1ui6w6fupe45tizz0tv6syzrs 52.48.130.243:2377
```

{% hint style="info" %}
As a reminder, joining as a worker means you have no ability to control containers on another node.
{% endhint %}

Once you have joined the network, you will be issued a control panel login by a Factom employee after messaging one of the Factom engineers on discord. You should private message the following for each node:

* NodeID \(found by running `docker info` \)
* IP address 
* Docker engine listening port \(2376\)

{% hint style="danger" %}
Only accept logins at federation.factomd.com. Any other login endpoints are fraudulent and not to be trusted.
{% endhint %}

## Starting Factomd Container from the Docker CLI

{% hint style="warning" %}
**Please note**: There is a version for the Factom software in the next command. Make sure you run the correct and latest announced version from the Discord \#operators-announcement channel
{% endhint %}

Run the following command _exactly:_

`docker run -d --name "factomd" -v "factom_database:/root/.factom/m2" -v "factom_keys:/root/.factom/private" -p "8088:8088" -p "8090:8090" -p "8108:8108" -l "name=factomd" factominc/factomd:v6.3.1-alpine -startdelay=600 -faulttimeout=120 -config=/root/.factom/private/factomd.conf`

{% hint style="info" %}
If you want the Factomd container to start at system boot \(reboots\) you can add the following parameter to the command above:

`--restart unless-stopped`
{% endhint %}

After this your node will be started. You can check for the existence of a Factom container using the command `docker ps` . 

You're now almost ready to be included in the main-net. Stop the `factomd` container with `docker stop factomd` and download the `factomd.conf` file [here](https://raw.githubusercontent.com/FactomProject/factomd/master/factomd.conf). 

There are some required edits that are needed, among them you're required to enter a few special peers here. You will get more information about this if you're accepted as an ANO.

In the `important.conf` file generated earlier you'll find the following three lines:

```bash
IdentityChainID
LocalServerPrivKey
LocalServerPublicKey
```

Paste these into the `factomd.conf` and save. Now place the config file in `/var/lib/docker/volumes/factom_keys/_data`  by running \(if the file is where you're currently at\):  
  
`sudo mv factomd.conf /var/lib/docker/volumes/factom_keys/_data/factomd.conf`

Now you're free to start the `factomd` container again with `docker start factomd` . 

If you check the currently running docker containers you'll see a container named `factominc/filebeat:m3-debug` , this is generally a good sign as it means the portainer system has successfully connected and started a container remotely. 

{% hint style="warning" %}
Please wait for your node to be fully synced by checking the control panel node sync statuses to be 100% before performing any next steps. Please also regard the initial wait period of 20 minutes before doing anything with your node. Also note that the `factomd:v6.3.1-alpine` version may have changed since the time of writing.
{% endhint %}

