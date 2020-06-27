# Developer Sandbox

There are three options for running a sandbox.

1. [**Run only a local sandbox server**](https://developers.factomprotocol.org/start/developer-sandbox-setup-guide#setup-a-local-sandbox-factom-server)\*\*\*\*
   1. Minimal setup
   2. good for quickly testing APIs
   3. Better feedback metrics
2. [**Run a test server and client**](https://developers.factomprotocol.org/start/developer-sandbox-setup-guide#setup-a-factom-remote-server)\*\*\*\*
   1. Much closer to production experience
   2. Needed if multiple clients are used
   3. Factom setup needed on two different computers
3. [**Run a Dockerized Factom sandbox**](https://developers.factomprotocol.org/start/developer-sandbox-setup-guide#run-a-dockerized-factom-sandbox)\*\*\*\*
   1. Zero setup
   2. Good if you are already familiar with Docker
   3. Almost the same as running a local sandbox server

## Setup a Local Sandbox Factom Server

### **Install Factom Binaries**

Download the appropriate Factom binary package from factom.org. Installers for for Windows, Mac, and Linux are hosted at [https://github.com/FactomProject/distribution](https://github.com/FactomProject/distribution). Directions for installing Factom Federation may be found [here](https://developers.factomprotocol.org/start/factom-cli-docs/installation#install-factom-federation-ff).

Install the binaries like you would any other for your OS. The install directions walk you through various operating systems. Do not run them yet, as you will be making your own fresh blockchain instead of using the public one.

{% hint style="info" %}
 If you did run `factomd` before setup, follow the directions found [here ](https://developers.factomprotocol.org/start/developer-sandbox-setup-guide#resetting-the-blockchain)to reset the blockchain.
{% endhint %}

Three programs are installed: `factomd`, `factom-walletd`, and `factom-cli`. You may also wish to install the GUI [Enterprise Wallet](https://developers.factomprotocol.org/wallets/enterprise-wallet/installation#install-enterprise-wallet).

* `Factomd`is the main program. It manages the blockchain, connects to the public network, and enforces the network rules.
* `factom-walletd` is an application for holding private keys. It builds Factoid transactions and handles crypto related operations to add user data into Factom.
* `Factom-cli` is a program for users to interface with `factomd` and `factom-walletd`. It may be used to create Chains, Entries, and Factoid transactions.

### **Configure Factomd for Sandbox use**

Create a folder in your user home folder. The folder should be called `.factom`. If factomd has been run on your computer before this folder will already exist and may already contain part of the Factom blockchain.

In a terminal on Linux or  Mac, run: 

```bash
mkdir -p ~/.factom/m2
```

or Windows: 

```bash
mkdir %HOMEDRIVE%%HOMEPATH%\.factom\m2
```

Save the configuration file [factomd.conf](https://raw.githubusercontent.com/FactomProject/factomd/master/factomd.conf) to your `.factom/m2` directory. Open `factomd.conf` and edit the following:

* Change the line `Network` from `MAIN` to `LOCAL`
  * This will cause `factomd` to run on its own local network rather than the Factom main network.
* Change the line `NodeMode` from `FULL` to `SERVER`
  * This will make `factomd` create a blockchain.
* Optionally, adjust `DirectoryBlockInSeconds`. 600 gives 10 minute blocks, which is more realistic
  * The default is for 6 second blocks which is easier to develop with, but may causesome strange behavior and errors.
* For easier debugging, change the `logLevel` to `debug`. This exports pointers to data which is added into Factom to the `~/.factom/data/export/` directory. Adding new entries will add new files to the directory with the specified ChainID.

### **Run Factomd in sandbox mode**

In a terminal window, run:

```bash
factomd -network=CUSTOM -customnet="mycustomnet" -exclusive=true
```

In a new terminal window, run:

```bash
factom-walletd
```

In a new terminal window, run:

```bash
factom-cli properties
```

Result should be similar to:

```bash
% factom-cli properties
CLI Version: 0.2.0.2
Factomd Version: 0.4.0.2
Factomd API Version: 2.0
Wallet Version: 0.2.0.2
Wallet API Version: 2.0
```

{% hint style="info" %}
Windows users have a desktop shortcut for both first and second command, for the last one you might have to browse to the install location of `factom-cli`, or it might be in the path.
{% endhint %}

Once `factomd` is running you can use an internet browser and navigate to view the `factomd`Control Panel, found at [http://localhost:8090](http://localhost:8090).

### **Charge an Entry Credit key**

We use a Factoid key which has a balance of 40000 in the genesis block. It has since been depleted on main net, but can be used on sandbox systems. 

The private key is: `Fs3E9gV6DXsYzf7Fqx1fVBQPQXV695eP3k5XbmHEZVRLkMdD9qCK` 

The public key is: `FA2jK2HcLnRdS94dEcU27rF3meoJfpUcZPSinpb7AwQvPRY6RL1Q`

To import the address, run:

```bash
factom-cli importaddress Fs3E9gV6DXsYzf7Fqx1fVBQPQXV695eP3k5XbmHEZVRLkMdD9qCK
```

Now lets generate a new Entry Credit address:

```bash
factom-cli newecaddress
```

To verify that you've imported and generated your addresses you can list them:

```bash
factom-cli listaddresses
```

Result should be similar to:

```text
FA2jK2HcLnRdS94dEcU27rF3meoJfpUcZPSinpb7AwQvPRY6RL1Q 40000
EC2kMjtY5jB5sLLzFtfzaA2rU92fqdNeiWNWjvtKmYQoG7fXFTmG 0

% Named the addresses fa1 and ec1
% fa1=FA2jK2HcLnRdS94dEcU27rF3meoJfpUcZPSinpb7AwQvPRY6RL1Q
% ec1=EC2kMjtY5jB5sLLzFtfzaA2rU92fqdNeiWNWjvtKmYQoG7fXFTmG
```

Then purchase Entry Credits using our Factoid address.

```bash
factom-cli buyec $fa1 $ec1 10000
```

Result:

```bash
TxID: ce18d00316508dd44a02d20bb3d9ee15f909bd9a099dd1b9b4576e2688f8f42a
Status: TransactionACK
```

If you want to check your EC balance run:

```bash
factom-cli balance $ec1
```

### **Make Entries into Factom**

All Entries in Factom need to be in a Chain. First, make a Chain for your entries to live in.

```bash
% echo "This is the payload of the first Entry in my chain" | factom-cli addchain -n thisIsAChainName -n moreChainNameHere $ec1
CommitTxID: e8950db51f6c15e10fdc6cc928cd40a5b9438b775708bdf4cabb266b0c1ffb5a
ChainID: 23985c922e9cdd5ec09c7f52a7c715bc9e26295778ead5d54e30a0a6215783c8
Entryhash: e8a838f95c1fe873e0c7faae401cef31d6273644c32aa2324946613a594c0c77
```

It makes the chain with `ChainID`: `23985c922e9cdd5ec09c7f52a7c715bc9e26295778ead5d54e30a0a6215783c8`

Check the status of the Chain.

```bash
factom-cli get chainhead 23985c922e9cdd5ec09c7f52a7c715bc9e26295778ead5d54e30a0a6215783c8
```

Result:

```bash
EBlock: c18bf1688d20b145d53f6e995ff8dfe91c2d73a422a625eb68c712ccc7988cd2
BlockSequenceNumber: 0
ChainID: 23985c922e9cdd5ec09c7f52a7c715bc9e26295778ead5d54e30a0a6215783c8
PrevKeyMR: 0000000000000000000000000000000000000000000000000000000000000000
Timestamp: 1489718700
DBHeight: 130
EBEntry {
	Timestamp 1489718940
	EntryHash e8a838f95c1fe873e0c7faae401cef31d6273644c32aa2324946613a594c0c77
}
```

You can now place Entries into this Chain.

```bash
echo "This is the payload of an Entry" | factom-cli addentry -e newextid -e anotherextid -c 23985c922e9cdd5ec09c7f52a7c715bc9e26295778ead5d54e30a0a6215783c8 $ec1
```

Result:

```bash
CommitTxID: 0e6513528212b476297355207d619807f1b6bf2379f707c84cb0066ad2d4a920
ChainID: 23985c922e9cdd5ec09c7f52a7c715bc9e26295778ead5d54e30a0a6215783c8
Entryhash: 0a9fee6636ab71466a8716ba3715cc01582174d8ffab9e830e9f4ce4f1ea8890
```

Query your entries in Factom:

```bash
factom-cli get entry 0a9fee6636ab71466a8716ba3715cc01582174d8ffab9e830e9f4ce4f1ea8890
```

Result:

```text
EntryHash: 0a9fee6636ab71466a8716ba3715cc01582174d8ffab9e830e9f4ce4f1ea8890
ChainID: 23985c922e9cdd5ec09c7f52a7c715bc9e26295778ead5d54e30a0a6215783c8
ExtID: newextid
ExtID: anotherextid
Content:
"This is the payload of an Entry"
```

And get information about the chain and its entries:

```bash
% factom-cli get firstentry 23985c922e9cdd5ec09c7f52a7c715bc9e26295778ead5d54e30a0a6215783c8
```

Result:

```text
EntryHash: e8a838f95c1fe873e0c7faae401cef31d6273644c32aa2324946613a594c0c77
ChainID: 23985c922e9cdd5ec09c7f52a7c715bc9e26295778ead5d54e30a0a6215783c8
ExtID: thisIsAChainName
ExtID: moreChainNameHere
Content:
"This is the payload of the first Entry in my chain"
```

and:

```bash
factom-cli get allentries 23985c922e9cdd5ec09c7f52a7c715bc9e26295778ead5d54e30a0a6215783c8
```

Result:

```text
Entry [0] {
EntryHash: e8a838f95c1fe873e0c7faae401cef31d6273644c32aa2324946613a594c0c77
ChainID: 23985c922e9cdd5ec09c7f52a7c715bc9e26295778ead5d54e30a0a6215783c8
ExtID: thisIsAChainName
ExtID: moreChainNameHere
Content:
"This is the payload of the first Entry in my chain"

}
Entry [1] {
EntryHash: 0a9fee6636ab71466a8716ba3715cc01582174d8ffab9e830e9f4ce4f1ea8890
ChainID: 23985c922e9cdd5ec09c7f52a7c715bc9e26295778ead5d54e30a0a6215783c8
ExtID: newextid
ExtID: anotherextid
Content:
"This is the payload of an Entry"
}
```

### **Evaluating Success**

You can get some diagnostic info from the control panel \(which incidentally doesn't control anything\). Browse to [http://localhost:8090](http://localhost:8090/).

{% hint style="success" %}
Your Factom sandbox container is now setup, you're now ready to interact with the Factom blockchain! Why not follow one of the hello-world examples for your favorite client? Head over to the [Hello-world examples](https://developers.factomprotocol.org/start/hello-world-examples) section for an overview.
{% endhint %}

## **Setup a Factom Remote Server**

On a remote machine, setup a Factom server. Both the client and server must be run of different machines.  
  
Use the same directions as [Install Factom Binaries](https://developers.factomprotocol.org/start/developer-sandbox-setup-guide#install-factom-binaries) and [Configure Factomd for Sandbox use](https://developers.factomprotocol.org/start/developer-sandbox-setup-guide#configure-factomd-for-sandbox-use). Run `factomd`. Make sure that port 8110 is open on the server. Only factomd needs to be running on the remote server.

### **Setup Client to use Sandbox Server**

Create a folder in your user home folder. The folder should be called `.factom/m2`. If factomd has been run on your computer before this folder will already exist and may already contain part of the Factom blockchain.

In a terminal on Linux or  Mac, run: 

```bash
mkdir -p ~/.factom/m2
```

or Windows: 

```bash
mkdir %HOMEDRIVE%%HOMEPATH%\.factom\m2
```

Save the configuration file [factomd.conf](https://raw.githubusercontent.com/FactomProject/factomd/master/factomd.conf) to your `.factom/m2` directory. Open `factomd.conf` and edit the following:

* Change the line `ServerPubKey` from `"0426a802617848d4d16d87830fc521f4d136bb2d0c352850919c2679f189613a"` to `"8cee85c62a9e48039d4ac294da97943c2001be1539809ea5f54721f0c5477a0a"`
  * This will make factomd recognize the sandbox public key instead of the official public key.
* Make sure `NodeMode` is set to `FULL` for the client node. 

### **Connect Local Factomd to Sandbox Server**

The local machine should be set as a client in the `factomd.conf`, which is default. Run `factomd` this way, but use the remote Factom server's IP address.

```bash
factomd -network=CUSTOM -customnet="mycustomnet" -exclusive=true -peers="SERVERIP:8110" -prefix="notaserver"
```

{% hint style="success" %}
Your Factom sandbox container is now setup, you're now ready to interact with the Factom blockchain! Why not follow one of the hello-world examples for your favorite client? Head over to the [Hello-world examples](https://developers.factomprotocol.org/start/hello-world-examples) section for an overview.
{% endhint %}

## Run a Dockerized Factom sandbox

### Setting up the factom sandbox

A current ANO maintains a docker image for a Factom sandbox. This tutorial assumes you know how to use [docker](https://docs.docker.com/) and that it's installed.  To run the Factom sandbox container without any modifications, simply run one of the two below depending on what you need:

{% tabs %}
{% tab title="Quick start" %}
```bash
docker run -d --name fct-sandbox laende/fct-sandbox
```
{% endtab %}

{% tab title="Quick start with ports exposed" %}
```bash
docker run -d --name fct-sandbox -p "8088:8088" -p "8089:8089" -p "8090:8090" laende/fct-sandbox
```
{% endtab %}
{% endtabs %}

The commands above run a Factom node with the following configuration:

* `Network` is set to `LOCAL`
  * This will cause factomd to run on its own local network rather than the Factom main network.
* `NodeMode` is set to `SERVER`
  * This will make `factomd` create a blockchain.
* `DirectoryBlockInSeconds` is set to `20`
  * Mainnet runs with 10 minute blocks \(600 seconds\). Easier to develop with shorter block times, but may cause some strange behavior and errors. 
* `logLevel` is set to `debug`
  * This exports pointers to data which is added into Factom to the `~/.factom/data/export/`directory. Adding new entries will add new files to the directory with the specified `ChainID`.

{% hint style="info" %}
To make Factom's data persist it is recommended to mount a data volume at /root/.factom:  
  
`docker run -d -v /root/.factom --name fct-sandbox-volume laende/fct-sandbox`
{% endhint %}

**Alternatively**, ****if you wish to do modifications to the `factomd.conf` file before running you can clone the repository from github:

```bash
git clone https://github.com/Laende/fct-docker.git
```

 Navigate to the `fct-docker` directory, which contains five files. To configure your local installation of `factomd` and `factomwalletd` edit the `factomd.conf` file. Once you’re happy with the changes, run:

```bash
docker build --tag=fct-sandbox .
```

and:

```bash
docker run -d -p "8088:8088" -p "8089:8089" -p "8090:8090" fct-sandbox
```

### Importing wallet with funds to the sandbox container

Now we need a Factom wallet with some funds to start recording entries. We use a Factoid address which has a balance of 20000 FCT in the genesis block. It has since been depleted on mainnet, but can be used on sandbox systems.

The private key is: `Fs3E9gV6DXsYzf7Fqx1fVBQPQXV695eP3k5XbmHEZVRLkMdD9qCK` 

The public key is: `FA2jK2HcLnRdS94dEcU27rF3meoJfpUcZPSinpb7AwQvPRY6RL1Q`

Let's import this address to our sandbox container:

```bash
docker exec -it fct-sandbox factom-cli importaddress Fs3E9gV6DXsYzf7Fqx1fVBQPQXV695eP3k5XbmHEZVRLkMdD9qCK
```

This command should return the public key mentioned above. Note it down somewhere as we will be using it later. We now need an Entry Credit address which we’ll generate:

```bash
docker exec -it fct-sandbox factom-cli newecaddress
```

You should see an EC address returned \(it starts with `EC`\). Write down this address as well, as we’ll be using it later.

{% hint style="success" %}
Your Factom sandbox container is now setup, you're now ready to interact with the Factom blockchain! Why not follow one of the hello-world examples for your favorite client? Head over to the [Hello-world examples](https://developers.factomprotocol.org/start/hello-world-examples) section for an overview..
{% endhint %}

## Resetting the Blockchain

By default, Factom holds all the blockchain, wallet, etc data in the user's home directory in a folder called `.factom/m2/custom-database`. This may be a hidden folder, so make sure to display hidden folders on your OS.

To reset the blockchain, first close `factomd` and `factom-walletd`. Delete all the folders and files in `.factom/m2/custom-database` except `factomd.conf`.

