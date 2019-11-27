# Getting started

[![Factom protocol: Getting started with the basics](http://img.youtube.com/vi/luJucKvHsB4/0.jpg)](https://www.youtube.com/watch?v=luJucKvHsB4)

{% hint style="info" %}
This tutorial is fully covered in the video shown above.
{% endhint %}

In this tutorial you will learn how to setup:

* A full factomd node with a wallet daemon
* Factom-cli 
* Docker

You need these to be able to get started with interacting with the Factom Blockchain. This tutorial is relevant for both mainnet and testnet and assumes that you've already setup an updated latest LTS version of Ubuntu. Minimum specifications for a node is listed below.

##  Minimum system requirements

* A modern CPU
* 16 GB RAM
* At least 50 GB storage
* 20 Mbit/s synchronous
* Up to 1TB / month data transfer
* Static IP-address

{% hint style="info" %}
The specifications are subject to change at any time as the network matures.
{% endhint %}

## Setting up a factomd node

Setting up a factomd node is very easy, download the latest release by running the following:

```bash
wget https://github.com/FactomProject/distribution/releases/download/v6.1.0/factom-amd64.deb
```

{% hint style="danger" %}
The version in the above command may be outdated, be sure to check what the [latest release](https://github.com/FactomProject/distribution/releases) is. 
{% endhint %}

After the download is finished, install the package:

```bash
sudo dpkg -i factom-amd64.deb
```

Once installation is complete you have access to the Factom daemon, wallet daemon and the Factom CLI. 

To start the Factom daemon simply run:

```bash
factomd
```

You also have access to the wallet daemon and CLI tools:

```bash
factom-walletd
factom-cli
```

### The Docker way

You can use the above deb package to interact with the protocol and complete the tutorial, but as an ANO your Factom daemon will run within a Docker container. Install Docker by following [the official Docker guides](https://docs.docker.com/install/linux/docker-ce/ubuntu/). Make sure to allow your non-root user to use Docker:

```bash
sudo usermod -aG docker <username>
```

Create the Factomd volumes:

```bash
docker volume create factom_keys
docker volume create factom_database
```

Place the `factomd.conf` in the correct folder:

```bash
wget https://raw.githubusercontent.com/FactomProject/factomd/master/factomd.conf
sudo cp factomd.conf /var/lib/docker/volumes/factom_keys/_data/factomd.conf
```

Run `factomd` in Docker:

```bash
docker run -d --name "factomd" -v "factom_database:/root/.factom/m2" -v "factom_keys:/root/.factom/private" -p "8088:8088" -p "8090:8090" -p "8108:8108" -l "name=factomd" factominc/factomd:v6.3.1-alpine -startdelay=600 -faulttimeout=120 -config=/root/.factom/private/factomd.conf
```

{% hint style="danger" %}
The version in the above command may be outdated, be sure to check what the [latest release](https://github.com/FactomProject/distribution/releases) is.
{% endhint %}

## Setting up the wallet daemon

Now we want a wallet daemon running. You can easily just run factom-walletd above, but you might want to run it as a system service:

{% code title="/etc/systemd/system/factom-walletd.service" %}
```bash
[Unit]
Description=Run the Factom Wallet service
Documentation=https://github.com/FactomProject/factom-walletd
After=network-online.target

[Service]
User=<username>
Group=<username>
EnvironmentFile=-/etc/default/factom-walletd
ExecStart=/usr/bin/factom-walletd $FACTOM_WALLETD_OPTS
KillMode=control-group
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
{% endcode %}

```bash
sudo systemctl daemon-reload
sudo systemctl enable factom-walletd
sudo systemctl start factom-walletd
```

{% hint style="success" %}
Congratulations, you now have a basic Factom daemon running in a docker container. You should be able to use this to complete the rest of the tutorial.
{% endhint %}

