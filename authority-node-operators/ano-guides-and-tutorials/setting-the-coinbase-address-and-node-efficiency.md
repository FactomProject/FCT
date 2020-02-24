# Setting the coinbase address and node efficiency

[![](http://img.youtube.com/vi/irlboOrHHcc/0.jpg)](http://www.youtube.com/watch?v=irlboOrHHcc)

{% hint style="info" %}
This tutorial is fully covered in the video shown above.
{% endhint %}

This tutorial will guide you through the process of setting your coinbase address and node efficiency. If you've followed the previous tutorials you should have a system with Factomd running as well as a wallet with some Entry Credits in it. 

### Requirements

First off you will need two USB flash drives ready. 

* One that will be used to boot a temporary Linux to perform the operations. This USB stick will be referenced as BOOT stick in the tutorial.
* The second one will be used to store data related to the operations. This USB flash drive will be referenced as DATA stick in the tutorial. Copy this tutorial onto the DATA stick to be able to follow the instructions on the offline system as well. 

Additionally, you will need your Factom identity information ready:

* Your identity root chain ID \(starts with `888888`\)
  * You will find this in the important.conf file you made when generating your server identity 
* Your server management subchain ID \(starts with `888888`\)
* Your secret level 1 identity key \(starts with `SK1` \)

The value of the server management subchain ID can be found by looking at the identity root chain ID in a Factom explorer: it is the value of the 3rd external id of the entry whose 2nd external id is equal to 'Register Server Management'.

You will also need a valid main-net Entry Credit address funded with ECs \(maybe a hundred to be safe\). Write down the corresponding private EC address \(that starts with `Es`\) or you can store it on the DATA stick. You also need a main-net public Factoid address \(that starts with `FA` \) that will be used as the receiving coinbase address.

### The steps

First create the Ubuntu 18.04 bootable BOOT stick. Google "create Ubuntu 18.04 bootable USB flash drive" if you're unsure how to.

Then, boot Ubuntu 18.04 from the BOOT stick and connect to the internet.

Open a terminal and run the following commands \(this will install node.js 10 and the factom-identity CLI\):

```bash
sudo apt-get update
sudo apt install -y git curl
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
git clone https://github.com/PaulBernier/factom-identity-cli.git
cd factom-identity-cli
npm install
```

Now turn **off** the internet connection. From now on your Ubuntu should _never_ get connected to the Internet. You can double check by typing `ping 8.8.8.8`, or you can unplug your wifi/Ethernet if you really want to be sure.

Proceed by generating the update scripts by running the following in a terminal:

**Updating the coinbase address**:

```bash
# Parameters: <Identity root chain ID> <FCT public address> <SK1 private key> <Paying private EC address>
# Example:
node bin/factom-identity-cli update-coinbase-address --offline -s https://api.factomd.net/v2 --id 8888889822cf1d5889aa8dc11ad210b67d582812152de568fabc5f8505989c0f --fctaddress FA3HZDE4MdXAthauFoA3aKYpx33U4fT2kAABmfwk7NBqyLT2zed5 --sk1 sk12tdaziBoFyBHG56Ery3bPFFBDpy7Y3VymduGPfoj66cGhH4mHZrw --secaddress Es3ytEKt6t5Jm9juC4kR7EgKQSX8BpRnM4WADtgFoq7j1WgbeEGW
```

{% hint style="danger" %}
Be sure to replace the parameters in the command above with your own:

* Identity root chain ID
* FCT public address
* SK1 private key
* Paying private EC address
{% endhint %}

**Updating the node efficiency**:

```bash
# Parameters: <Identity root chain ID> <Efficiency> <SK1 private key> <Paying private EC address> <Server Management Subchain ID>
# Example:
node bin/factom-identity-cli update-efficiency --offline -s https://api.factomd.net/v2 --id 8888889822cf1d5889aa8dc11ad210b67d582812152de568fabc5f8505989c0f --efficiency 50.1 --sk1 sk12tdaziBoFyBHG56Ery3bPFFBDpy7Y3VymduGPfoj66cGhH4mHZrw --secaddress Es3ytEKt6t5Jm9juC4kR7EgKQSX8BpRnM4WADtgFoq7j1WgbeEGW --smchainid 8888887c01c12c72052f9c99b45782013feadb20c46ca86dc6e3a9730835848a
```

{% hint style="danger" %}
Be sure to replace the parameters in the command above with your own:

* Identity root chain ID
* Efficiency
* SK1 private key
* Paying private EC address
* Server Management Subchain ID

Value of the _Server Management Subchain ID_ can be found by looking at the identity root chain id in a Factom explorer: it is the value of the 3rd external id of the entry whose 2nd external id is equal to 'Register Server Management'.
{% endhint %}

Those two commands will create two scripts, if successful:

* `update-coinbase-address.<short_id>.sh`
* `update-efficiency.<short_id>.sh`

{% hint style="info" %}
 &lt;_short\_id&gt;_ is the short id of your identity: the first 6 characters after the leading 888888. 
{% endhint %}

Copy these scripts to your DATA stick, eject it and shut down and destroy the Ubuntu session. 

{% hint style="danger" %}
 **Important:** the scripts generated contain timestamped data which means they have a limited time of validity. You must execute the scripts **within 1 hour** of their creation to be certain that the entries contained in them will be accepted by the network.
{% endhint %}

Continue now on the previous system you were working on and plugin the DATA stick. Locate the files and make sure they are executable:

```bash
chmod +x update-coinbase-address.<short_id>.sh
chmod +x update-efficiency.<short_id>.sh
```

Now execute both bash scripts. 

```bash
./update-coinbase-address.<short_id>.sh
./update-efficiency.<short_id>.sh
```

The scripts simply use `curl` command lines that should be available on most unix based systems. The scripts will make the requests to the Factom Open Node \(`https://api.factomd.net/v2`\). 

{% hint style="info" %}
You can edit the scripts to choose another endpoint by changing the  `HOST=` parameter at the top to localhost or a test-net node for instance.
{% endhint %}

After executing, the scripts should output success messages if done correctly. You can check in an explorer that your identity root chain and server management subchain have been updated after 10 minutes \(next block\).

Finally, format the boot stick, or even better, do a secure erase with specialized tools. 

