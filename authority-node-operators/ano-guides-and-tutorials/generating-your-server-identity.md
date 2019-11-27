# Generating your server identity

[![Factom protocol: Generating your server identity](http://img.youtube.com/vi/g9FzNtSB7I4/0.jpg)](https://www.youtube.com/watch?v=g9FzNtSB7I4)

{% hint style="info" %}
This tutorial is fully covered in the video shown above.
{% endhint %}

This tutorial assumes that you've setup Ubuntu that is running the _Factom Daemon_. If you haven't, please check out the _getting started with the basics_ tutorial first.

You can check if the daemon is up by running `docker ps` . A container with the image `factominc/factomd:vX.xx` should show up where `vX.xx` is your current Factomd version. 

{% hint style="info" %}
Latest version of the Factom Daemon can be found [here.](https://github.com/FactomProject/distribution/releases)
{% endhint %}

Furthermore you will need Entry Credits to create your identity to the blockchain, so make sure you have at least 24 EC in your Factom wallet before moving on with the tutorial.

Lastly, it is **highly** suggested that you have two USB flash drives ready:

* One that will be used to boot a temporary Linux to perform the operations. This USB stick will be referenced as BOOT stick in the tutorial.
* The second one will be used to store data related to the operations. This USB flash drive will be referenced as DATA stick in the tutorial. Copy this tutorial onto the DATA stick to be able to follow the instructions on the offline system as well. 

{% hint style="danger" %}
The keys generated as part of this tutorial are highly sensitive, do not EVER share any of the keys you hold or generate. Losing these keys means you will lose your server. Keep these secure and safe, preferably written down on a piece of paper.  

Thus, doing parts of this tutorial on an offline computer greatly reduces the risk of these keys being compromised by either key loggers,  screen capture software, viruses or by other malicious means.
{% endhint %}

## Building the serveridentity tool

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

{% hint style="warning" %}
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

Copy the files `serveridentity` and `signedwithed25519` into your USB DATA flash drive and continue the process on your offline system. 

## Creating the server identity 

For the next steps you will need some Entry Credits as you'll be storing your identities on the blockchain itself.  

{% tabs %}
{% tab title="Using the Enterprise wallet" %}
* Create a new Entry Credit address in your Address Book
* Verify that it has been funded by checking the balance in the wallet
* Click the pencil-icon on your EC wallet and click "Display Private Key", you will need this for the next step.
* If you dont have one then create a new Factoid address to set up efficiency and payouts.
{% endtab %}

{% tab title="Using the CLI wallet" %}
&lt;temp&gt;
{% endtab %}
{% endtabs %}

First, make sure the files are executable:

```bash
chmod +x serveridentity
chmod +x signwithed25519
```

Now run the following:

`./serveridentity full elements {ENTRY_CREDIT_PRIVATE_KEY} -n={OPTIONAL_FILENAME}`

This will generate numerous keys which are printed to stdout, two files will also be produced:

* Script to add the Identity to the blockchain
  * Script that utilizes factom-cli
  * Name of script is by default `fullidentity.sh` or `{OPTIONAL_FILENAME}.sh` if provided.
* Config file needed for the server
  * Place in `~/factom/m2` and rename to `factomd.conf`
  * Name of config is by default `fullidentity.conf` or `{OPTIONAL_FILENAME}.conf` if provided

Write down the keys that are printed in `stdout` and copy the two new files to the DATA stick, eject it and shut down and destroy the Ubuntu session. Continue the tutorial on your server again.

## Importing your server identity

Copy your `fullidentity.sh` file to your server, make it executable and run it. Make sure `signedwithed25519` is available on the system.

```bash
factom-cli importaddress {ENTRY_CREDIT_PRIVATE_KEY}
chmod +x fullidentity.sh
./fullidentity.sh
```

Your server identity chain is now generated. You can now paste the contents of the `important.conf` file into your `factomd.conf`, normally located at `/var/docker/volumes/factom_keys/_data/factomd.conf` 

