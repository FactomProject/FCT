---
description: Factom "Hello World" in JavaScript
---

# JavaScript

In this tutorial we will be using the [factom.js](https://www.npmjs.com/package/factom) library to write a simple Node.js application using Factom. Along the way we will learn the important concepts of Factom and all the fundamental operations to interact with the blockchain.

## Requirements

* Node.js 8+ 
* npm \(or yarn\) 
* Docker \(if using Factom Dev Stack to run the local Factom blockchain\)

## Setup

### Setting up a local Factom blockchain

For this tutorial we will be running our own local Factom blockchain to make our development experience smoother:

* We have a faster block time \(few seconds instead of 10 minutes on mainnet/testnet\).
* We have immediate access to Factoids to interact with the blockchain \(for free\).
* We avoid potential availability/connectivity/latency inconveniences.

In this tutorial we'll be using the simplest solution, Factom Dev Stack \(FDS\), that only requires you to have [Docker installed](https://docs.docker.com/install/). There exist other ways to set up a developer sandbox which can you can [lookup here](https://developers.factomprotocol.org/start/developer-sandbox-setup-guide). 

Install Factom Dev Stack globally on your machine:

```bash
npm install -g factom-dev-stack
```

{% hint style="info" %}
You may need to use `sudo` in font of the command above to install FDS globally.
{% endhint %}

And then start FDS:

```bash
factom-dev-stack start
```

{% hint style="info" %}
The first time you launch FDS it will download the Docker images of `factomd` and `factom-walletd` which may take some time.
{% endhint %}

{% hint style="info" %}
Launching FDS without extra argument like above creates an ephemeral blockchain that will be cleaned up once FDS is stopped. [See here](https://github.com/PaulBernier/factom-dev-stack#blockchain-and-wallet-persistence) for more info.
{% endhint %}

Once FDS is started the control panel of your running Factom node is available on port `8090`, you can visualize it by opening [http://localhost:8090/](http://localhost:8090/) in a browser. The control panel can be used to see the state of your blockchain and also as an explorer. Find [more detailed info here](https://developers.factomprotocol.org/start/factomd-control-panel) about the control panel if you are interested.

You now have an instance of `factomd` \(Factom daemon\) and `factom-walletd` \(wallet daemon\) running that you can interact with. By default `factomd` exposes a JSON-RPC API on port `8088` and `factom-walletd` exposes a JSON-RPC API on port `8089`. Those are the set of APIs we are going to use to interact with our Factom blockchain.

### Setting up our project

Let's set up a basic Node.js project with factom.js as a dependency:

```bash
mkdir factom-hello-world
cd factom-hello-world
npm init
npm install -S factom
touch index.js
```

## Factom JS development 101

{% hint style="info" %}
The complete technical doc of factom.js is available online at [https://factomjs.luciap.ca/](https://factomjs.luciap.ca/
)
{% endhint %}

### Instantiating FactomCli

A `FacomCli` instance allows you to interact easily with both `factomd` and `factom-walletd`. 

```javascript
const { FactomCli } = require('factom')
const cli = new FactomCli()
```

Without any argument passed to the constructor the instance will connect by default to local instances of `factomd` and `factomd-walletd`, which is what we want in this tutorial. If you wanted to specify the endpoints \(to connect to a mainnet node for instance\) you can pass options to the constructor:

```javascript
const cli = new FactomCli({
    factomd: {
         host: 'api.factomd.net',
         port: 443,
         protocol: 'https'
    },
    walletd: {
         host: 'localhost',
         user: 'paul',
         password: 'pass'
     }
})
```

All the options available are described here: [https://factomjs.luciap.ca/\#connectionoptions](https://factomjs.luciap.ca/#connectionoptions)

{% hint style="info" %}
In the rest of the tutorial we will directly reference the `cli` variable without re-defining it every time.
{% endhint %}

### Dual token system, addresses and balances

Factom has a dual token system:

* Factoid \(FCT\) is the token to transfer value. It's possible to send and receive Factoids and they are traded on multiple crypto-currency exchanges with a fluctuating value relative to USD or BTC.
* Entry Credit \(EC\) is the token to insert data in the Factom blockchain. The only way to create Entry Credits is to burn Factoids and ECs cannot be transferred once created. They can only be used \(burned\) to pay for writing data. The conversion rate from FCT to EC is always adjusted such that 1 EC costs ~0.001 USD at all time.

Because there are two tokens there are two sets of addresses. Addresses have a public and private part, and the prefix \(2 first letters\) is different for each type. Here's a summary:

|  | Private address | Public address |
| :--- | :--- | :--- |
| Factoid | **Fs**3E9gV6DXsYzf7Fqx1fVBQPQXV695eP3k5XbmHEZVRLkMdD9qCK | **FA**2jK2HcLnRdS94dEcU27rF3meoJfpUcZPSinpb7AwQvPRY6RL1Q |
| Entry Credit | **Es**32PjobTxPTd73dohEFRegMFRLv3X5WZ4FXEwNN8kE2pMDfeMym | **EC**2vXWYkAPduo3oo2tPuzA44Tm7W6Cj7SeBr3fBnzswbG5rrkSTD |

When starting a new local network the genesis block will fund the address `FA2jK2HcLnRdS94dEcU27rF3meoJfpUcZPSinpb7AwQvPRY6RL1Q` for which we luckily know the corresponding private address, `Fs3E9gV6DXsYzf7Fqx1fVBQPQXV695eP3k5XbmHEZVRLkMdD9qCK`, so we will be able to use those funds. Let's check the balance of this address:

```javascript
const balance = await cli.getBalance('FA2jK2HcLnRdS94dEcU27rF3meoJfpUcZPSinpb7AwQvPRY6RL1Q')
```

It should return a value of `2000000000000` which is the amount in _factoshis_. So we are starting we 20,000 FCT on this address.

{% hint style="info" %}
1 factoshi = $$10^{-8}$$ FCT
{% endhint %}

To get other addresses to work with we have two options:

```javascript
// 1. Generate completely random addresses
const { generateRandomFctAddress, generateRandomEcAddress} = require('factom')
const fctAddress = generateRandomFctAddress()
const ecAddress = generateRandomEcAddress()

// 2. Generate addresses from the wallet which are derived from a seed
// We are using here a direct call to a factom-walletd API: https://docs.factom.com/api#generate-ec-address
// The private part of the address is automatically stored in walletd
const walletFctAddress = await cli.walletdApi('generate-factoid-address')
const walletEcAddress = await cli.walletdApi('generate-ec-address')
```

{% hint style="info" %}
Generally it is a better idea to derive the addresses from a seed as it allows you to backup that seed and restore all the addresses from it later if needed.
{% endhint %}

{% hint style="info" %}
Similarly to the method `walletdApi` you have access to a method `factomdApi` that allows you to make calls directly against [factomd API](https://docs.factom.com/api).
{% endhint %}

### Making transactions

#### Sending Factoids

Let's see how to transfer some of those FCT to another address. Factom.js has a convenient method `createFactoidTransaction` for SISO \(Single Input Single Output\) transactions:

```javascript
const recipient = await cli.walletdApi('generate-factoid-address')
const transaction = await cli.createFactoidTransaction(
        'Fs3E9gV6DXsYzf7Fqx1fVBQPQXV695eP3k5XbmHEZVRLkMdD9qCK',
        recipient.public,
        100000000)
const transactionId = await cli.sendTransaction(transaction)
```

{% hint style="info" %}
1. The amount is in _factoshis_ \(and not factoids\).
2. As the input of the transaction we use the private part of the source address. If the address was imported in the wallet we could have use the public address and the library would have fetched the private counterpart from the wallet seamlessly.
{% endhint %}

You can search for the transaction id \(after the next block is produced\) in your node control panel and should see something like this:

![Factoid transaction](../../.gitbook/assets/image%20%286%29.png)

#### Converting FCT to EC

To insert chains and entries in the Factom blockchain we will need Entry Credits. ECs can be obtained by burning Factoids. We can use the `createEntryCreditPurchaseTransaction`  method for that:

```javascript
const recipient = await cli.walletdApi('generate-ec-address')
const transaction = await cli.createEntryCreditPurchaseTransaction(
        'Fs3E9gV6DXsYzf7Fqx1fVBQPQXV695eP3k5XbmHEZVRLkMdD9qCK',
        recipient.public,
        1000)
// You can check how many factoshis it's going to cost you to buy those 1000 ECs
console.log(transaction.totalInputs);
const txId = await cli.sendTransaction(transaction)
```

{% hint style="info" %}
Note that the amount here is the number of Entry Credits to convert to. The library will compute for you how much factoshis it will cost.
{% endhint %}

#### Complex transactions

In the previous sections we used the helper methods `createFactoidTransaction` and`createEntryCreditPurchaseTransaction`  to craft valid transactions for us \(including fees etc\). The library allows you to craft yourself complex transactions if you wanted to. Examples can be [found here](https://github.com/PaulBernier/factomjs#multi-inputsoutputs-transaction).

### Creating chains and entries

Now that we have some Entry Credits we can write data in the blockchain. In Factom we create chains which can then contain entries. Entries can contain any arbitrary data. The structure of chains allows to logically organize your data. Some important facts to know about chains and entries:

* One entry is composed of a list of external IDs and a content. Any of those can be empty.
* One entry can contain up to 10kb of data.
* The cost of writing an entry is 1 EC/kb \(so between 1 EC and 10 EC\).
* The cost of creating a chain is 10 EC + the cost the first entry \(so between 11 EC and 20 EC\).
* A chain is simply defined by its first entry. The chain ID is derived from the external IDs of that first entry.

{% hint style="warning" %}
Remember that anybody can read all the entries of any chain. Also anybody can add an entry to any chain. **Chains are not secret or private.** If you want to learn more about some patterns to deal with this you can read [Design patterns on Factom](https://medium.com/@luciap_tech/design-patterns-on-factom-160ece591e85). 
{% endhint %}

#### Creating our first chain

```javascript
const { Entry, Chain } = require('factom')
const firstEntry = Entry.builder()
    .extId('6d79206578742069642031') // If no encoding parameter is passed as 2nd argument, 'hex' is used
    .extId('my ext id 2', 'utf8') // Explicit the encoding. Or you can pass directly a Buffer
    .content('Hello world', 'utf8')
    .build()

const chain = new Chain(firstEntry)
// EC35pi8hBPst6JosVSRNvQ5ysXZXN6QX1pTDWhUDy3cnDThpJMeL is the EC address generated and funded 
// in the previous section and paying for the chain insertion.
const { chainId, entryHash } = await cli.add(chain, 'EC35pi8hBPst6JosVSRNvQ5ysXZXN6QX1pTDWhUDy3cnDThpJMeL')
```

{% hint style="info" %}
Be aware that the default string encoding of the external ids and the content is `hex`. You can specify an encoding as a second argument or pass directly an instance of `Buffer`.
{% endhint %}

Search for the chain ID in the control panel \(after the next block has been produced\):

![Our first chain](../../.gitbook/assets/image%20%284%29.png)

#### Adding an entry to an existing chain

```javascript
const { Entry, } = require('factom')
// Add to chain db1e44f55fb346cdfa14a0ed483c580f5be2bcc5b5d03fece013666657cc106f
// which is the chain previously created (if you didn't modify the first entry)
const secondEntry = Entry.builder()
    .chainId('db1e44f55fb346cdfa14a0ed483c580f5be2bcc5b5d03fece013666657cc106f')
    .extId('something', 'utf8')
    .content('A second entry', 'utf8')
    .build()

const { chainId, entryHash } = await cli.add(secondEntry, 'EC35pi8hBPst6JosVSRNvQ5ysXZXN6QX1pTDWhUDy3cnDThpJMeL')
```

![Second entry added to our chain](../../.gitbook/assets/image%20%287%29.png)

### Reading chains and entries

#### Reading entries

Now that we have inserted data into Factom we want to be able to read that data from the blockchain. You can query an entry directly by its entry hash:

```javascript
const entry = await cli.getEntry('fc72f3f72e2ebf7d92b33b2b1a8e10f0b7cac43e2492c2d103e815e946d1f9a9')
```

The external IDs and content of the entry are returned as `Buffer` and it is up to your application to interpret/decode that raw data. 

{% hint style="info" %}
The entry returned by `getEntry` does not contain a timestamp and a block context \(details about which block the entry was included in\). If you need that data you can use `getEntryWithBlockContext` instead, but be aware that this method is more expensive as it makes 3 API calls.
{% endhint %}

#### Reading chains

If we want to retrieve all the entries of a given chain we can simply do so:

```javascript
const entries = await cli.getAllEntriesOfChain('db1e44f55fb346cdfa14a0ed483c580f5be2bcc5b5d03fece013666657cc106f')
```

This method will rewind the whole chain and retrieve all its entries. However if the chain is long this is not a good approach. Another method allows you to control the rewind of the chain and process the entries one by one:

```javascript
await cli.rewindChainWhile(
    'db1e44f55fb346cdfa14a0ed483c580f5be2bcc5b5d03fece013666657cc106f', 
    (entry) => true, 
    function (entry) {
        console.log(entry.content.toString('utf8'))
})
```

The second argument is a predicate that allows you to halt the loop. If that predicate never evaluates to false the rewind will stop once the whole chain has been processed.

## Let's build a factomizer

We now have all the components we need write our first Factom application. This will be a simple "factomizer": an app that allows to store the hash of a file on Factom and then later verify the integrity of the file against the data stored in the entry.

{% hint style="warning" %}
This basic implementation of a factomizer is not to be considered suitable for real world usage. For the sake of simplicity we didn't address some flaws, the biggest one being the authentication of the data recorded \(remember that anyone could write in your chain\).
{% endhint %}

### Factomizing operation

Our main function has 3 steps:

* We build a new `Chain` object that contains information about the file we want to secure on Factom.
* We buy enough entry credits on the fly to pay for writing that data.
* We add the chain to Factom.

{% code title="factomize.js" %}
```javascript
// Our main function that factomizes our input file
async function factomize(filePath) {
    const cli = new FactomCli();

    const chain = buildChain(filePath);
    const { ecAddress } = await getEntryCredits(cli, chain.ecCost());
    const { entryHash, chainId } = await cli.add(chain, ecAddress);

    console.log(`File ${path.resolve(filePath)} secured on Factom blockchain.`);
    console.log(`Chain id: ${chainId}`);
    console.log(`Entry hash: ${entryHash}`);
}
```
{% endcode %}

Here we are creating a new a new chain per file. Another approach could be to create one chain where you will append new entries for all your factomized files. We put some metadata in the external ids \(filename, timestamp\) and the SHA512 of the file in the content of the entry. We also added 'simple-factomized' in the first external ID as a simple marker for easier identification of the nature of the entry,.

{% code title="factomize.js" %}
```javascript
function buildChain(filePath) {
    const data = fs.readFileSync(filePath);
    const filename = path.basename(filePath);
    const hash = sha512(data);

    const entry = Entry.builder()
        .extId('simple-factomizer', 'utf8')
        .extId(filename, 'utf8')
        .extId(Date.now().toString(), 'utf8')
        .content(hash)
        .build();

    return new Chain(entry);
}
```
{% endcode %}

We have a function to buy entry credits for the specific amount needed for the sake of re-using what we learned previously. But in practice you will often use an EC address that already have enough ECs and that is regularly topped up to stay with a large positive balance.

{% code title="factomize.js" %}
```javascript
// FCT address funded from the genesis block
const FUNDED_FCT_ADDRESS = 'Fs3E9gV6DXsYzf7Fqx1fVBQPQXV695eP3k5XbmHEZVRLkMdD9qCK';

async function getEntryCredits(cli, amount) {
    const { public: ecAddress } = await cli.walletdApi('generate-ec-address');

    const transaction = await cli.createEntryCreditPurchaseTransaction(FUNDED_FCT_ADDRESS, ecAddress, amount);
    const txId = await cli.sendTransaction(transaction);

    console.log(`Transaction ID of the entry credit purchase: ${txId}`);

    return { ecAddress };
}
```
{% endcode %}

Everything together gives us our simple factomizer:

{% code title="factomize.js" %}
```javascript
const fs = require('fs'),
    path = require('path'),
    { sha512 } = require('./crypto'),
    { FactomCli, Entry, Chain } = require('factom');

// FCT address funded from the genesis block
const FUNDED_FCT_ADDRESS = 'Fs3E9gV6DXsYzf7Fqx1fVBQPQXV695eP3k5XbmHEZVRLkMdD9qCK';

async function factomize(filePath) {
    const cli = new FactomCli();

    const chain = buildChain(filePath);
    const { ecAddress } = await getEntryCredits(cli, chain.ecCost());
    const { entryHash, chainId } = await cli.add(chain, ecAddress);

    console.log(`File ${path.resolve(filePath)} secured on Factom blockchain.`);
    console.log(`Chain id: ${chainId}`);
    console.log(`Entry hash: ${entryHash}`);
}

async function getEntryCredits(cli, amount) {
    const { public: ecAddress } = await cli.walletdApi('generate-ec-address');
    const transaction = await cli.createEntryCreditPurchaseTransaction(FUNDED_FCT_ADDRESS, ecAddress, amount);
    const txId = await cli.sendTransaction(transaction);

    console.log(`Transaction ID of the entry credit purchase: ${txId}`);

    return { ecAddress };
}

function buildChain(filePath) {
    const data = fs.readFileSync(filePath);
    const filename = path.basename(filePath);
    const hash = sha512(data);

    const entry = Entry.builder()
        .extId('simple-factomizer', 'utf8')
        .extId(filename, 'utf8')
        .extId(Date.now().toString(), 'utf8')
        .content(hash)
        .build();

    return new Chain(entry);
}

factomize(process.argv[2])
```
{% endcode %}

Usage:

```bash
$ node factomize.js my-file.pdf 
Transaction ID of the entry credit purchase: 4e01008711ccc67c75a64ffa446ed55eec90073e0735ba94ec8627f427b02acf
File /my-file.pdf secured on Factom blockchain.
Chain id: c82ef9af79d3ad483494164ed53be677f06c294c3dff4322847cc8045a377252
Entry hash: e750680a6b5f0f64fcab757f28134fbe5ddb2a861d9b20a448ec2425af8bd552
```

### Verification operation

Now that we have secured a document in the blockchain we need an operation to be able to verify at a later time the integrity of that document. The steps are straightforward:

* Compute the hash of the file we want to check the integrity.
* Fetch the hash recorded in the corresponding Factom entry.
* Compare both hashes.

{% code title="verify.js" %}
```javascript
async function verify(filePath, entryHash) {
    const fileHash = getFileHash(filePath);
    const recordedHash = await getRecordedHash(entryHash);

    if (fileHash.equals(recordedHash)) {
        console.log(`Integrity of file ${path.resolve(filePath)} verified against Factom record.`);
    } else {
        console.log('Hash of file *DOES NOT* match the proof recorded on Factom.');
    }
}
```
{% endcode %}

{% code title="verify.js" %}
```javascript
async function getRecordedHash(entryHash) {
    const cli = new FactomCli();
    const entry = await cli.getEntry(entryHash);

    if (entry.extIds[0].toString() !== 'simple-factomizer') {
        throw new Error(`Entry [${entryHash}] is not a simple factomizer entry`);
    }

    return entry.content;
}
```
{% endcode %}

Everything together:

{% code title="verify.js" %}
```javascript
const fs = require('fs'),
    path = require('path'),
    { sha512 } = require('./crypto'),
    { FactomCli } = require('factom');

async function verify(filePath, entryHash) {
    const fileHash = getFileHash(filePath);
    const recordedHash = await getRecordedHash(entryHash);

    if (fileHash.equals(recordedHash)) {
        console.log(`Integrity of file ${path.resolve(filePath)} verified against Factom record.`);
    } else {
        console.log('Hash of file *DOES NOT* match the proof recorded on Factom.');
    }
}

function getFileHash(filePath) {
    const fileData = fs.readFileSync(filePath);
    return sha512(fileData);
}

async function getRecordedHash(entryHash) {
    const cli = new FactomCli();
    const entry = await cli.getEntry(entryHash);

    if (entry.extIds[0].toString() !== 'simple-factomizer') {
        throw new Error(`Entry [${entryHash}] is not a simple factomizer entry`);
    }

    return entry.content;
}

verify(process.argv[2], process.argv[3]);
```
{% endcode %}

Usage:

```bash
$ node verify.js my-file.pdf e750680a6b5f0f64fcab757f28134fbe5ddb2a861d9b20a448ec2425af8bd552
Integrity of file /my-file.pdf verified against Factom record.
```

### Complete source code

The complete source code of this simple factomizer is available online at [https://github.com/PaulBernier/simple-factomizer](https://github.com/PaulBernier/simple-factomizer)

## Conclusion

You should know have all the fundamentals to start building your own application on top of Factom. To go further you can explore the complete [documentation of factom.js](https://factomjs.luciap.ca/) and read the article [Design patterns on Factom](https://medium.com/@luciap_tech/design-patterns-on-factom-160ece591e85). Have fun!

