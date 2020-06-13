---
description: A tokenization protocol on top of Factom chains and entries
---

# FAT Tokenization

[Factom Asset Tokens](https://www.fatprotocol.org/) \(FAT\) are a collection of open source, data-only blockchain tokenization standards. FAT Tokens open up affordable, secure crypto tokens to the world.

{% embed url="https://www.youtube.com/watch?v=bdp7DsrX7hY" %}

## FAT Features

#### **Low Fixed Cost**

Fixed at $0.001 USD per transaction and $0.011 USD per new token issuance, FAT has 99% lower transaction and issuance costs than Ethereum Tokens with [similar features](https://github.com/Factom-Asset-Tokens/FAT/blob/master/FAQ#is-fat-comparable-with-ethereum?) and speed.

#### **Universal**

FAT standards are data only, and are written in universal formats that most programmers already know, and anyone can learn \(JSON\). There is no need to learn contract languages or other specialized tools.

#### **Extensible**

FAT standards and tokens are composable and extensible by design, and are made to be extended to the limit of creativity. Metadata is embeddable in issuances and transactions for unlimited flexibility.

#### **Safe**

Sending and issuing FAT tokens does not require buying crypto from exchanges. The tokens used to pay for FAT transactions & issuances, [Entry Credits](https://www.factom.com/devs/tokens/entry-credits), are not securities and are [purchasable online instantly](https://shop.factom.com/).

#### **Secure**

FAT is built on [Factom](https://github.com/Factom-Asset-Tokens/FAT/blob/master), a decentralized Blockchain run by 100s of community members and over 40 verified enterprise companies worldwide. Every 10 Minutes Factom's data is recorded into the Bitcoin Blockchain to prove it's existence and secure it forever.

#### Smart Contracts \(Alpha\)

FAT supports smart contracts written in WebAssembly which reside at FAT token addresses. Contracts are written in WebAssembly, a bytecode format programmable in multiple popular languages such as C, C++, Rust, Typescript.

Smart contracts can receive, send, create tokens programmatically.

#### **Multiple Token Types**

FAT features several token types and is always adding more. You can create your own too!:

[**FAT-0**](https://github.com/Factom-Asset-Tokens/FAT/blob/master/fatips/0.md)**:** A fungible asset token standard.

* Most alike an ERC-20 token \(TRON, Golem, Ziliqa\):
  * Tradable in fractions of a token
* Supports smart contracts written in WebAssembly\(Alpha\)

[**FAT-1**](https://github.com/Factom-Asset-Tokens/FAT/blob/master/fatips/1.md)**:** A non-fungible asset token standard.

* Most alike an ERC-721 token \(CryptoKitties\)
  * Tradable in whole number token increments
  * Every token is unique, and can have unique properties\(metadata\)
  * Tokens have a history of the addresses they've resided at

## **Getting Started**

Check out our [**Getting Started Guide**](https://docs.google.com/document/d/1dh3NrjKLqqjr737A-UI1EezE_TROHIzxw7W5djK21sM/edit) for the Factom Testnet to issue your first token!

### FAT Software

#### FAT Daemon

A daemon written in Golang that discovers new Factom Asset Tokens chains and maintains their current state. The daemon provides a JSON-RPC 2.0 API for accessing data about token chains. Comes packaged with a command line interface for issuing and sending tokens.

[FAT Daemon Github](https://github.com/Factom-Asset-Tokens/fatd)

#### **FAT Wallet**

A user facing wallet for receiving and sending fungible and non-fungible FAT tokens. The wallet is available as an executable for Windows, Mac, or Linux.

[FAT Wallet Github](https://github.com/Factom-Asset-Tokens/wallet)

#### FAT Javascript/NodeJS Library

Create and sign token issuances and transactions in Javascript. Interact with a FAT Daemon in NodeJS or from the browser. Supports all existing FAT Daemon RPC endpoints.

[FAT-JS Github](https://github.com/Factom-Asset-Tokens/fat-js)

### FAT Courtesy Nodes

A courtesy node is a FAT Daemon hosted for public access on the cloud that allows developers to use FAT without needing to run or maintain their own FAT Daemon. Courtesy nodes can be used with the FAT Wallet, FAT Daemon CLI, and FAT-JS projects to access data about and send FAT tokens.

#### Testnet

0.fat.testnet.dbgrow.com:8078

#### Mainnet

0.mainnet.fat.dbgrow.com:8078

#### 

### FAQ

**What Is The FAT Wallet?**

The FAT Wallet is a piece of software that keeps track of transactions of your selected FAT tokens and allows you to check token balances and send tokens. It's just like the the Bitcoin and Ethereum wallets you're used to. It features a user friendly interface to allow anyone to receive FAT tokens securely on their personal computer.  


**What Types of Tokens Work With The Wallet?**

Both FAT-0 and FAT-1 are supported. Future token standards will be implemented as deemed appropriate and beneficial for the community.  


**What Do I Need To Receive Tokens?**

No more than 1 minute of your time. Simply follow the instructions when the wallet starts up for the first time to create an address you can then share with others. This address generated is a Factom address that can also accept Factoids. You can also import a Factoid address if you have one already.  


**What Do I Need to Send tokens?**

You'll need to purchase some Entry Credits so you can enter data into the Factom blockchain.

You can acquire Entry Credits from 3rd party purchasers \([Defacto](https://ec.de-facto.pro/) & [Factom Inc.](https://shop.factom.com/)\).

Alternatively, you can acquire Factoids from an exchange and burn them into Entry Credits and avoid the markups of 3rd party services. You'll always get a good deal, since the burn ratio between Factoids to Entry Credits is stabilized to give Entry Credits a constant value of $0.001 USD.

Once your Entry Credit address is funded you can send FAT tokens by burning entry credits. It costs 1 Entry Credits to send a transaction \($0.001 USD\).

Pending transactions should show up in the wallet within 30 seconds after successful entry into Factom.  


**What Do I Need to Issue a New FAT Token?**

To issue a new FAT token you'll need a [Factom Digital Identity](https://github.com/FactomProject/FactomDocs/blob/master/Identity.md).

Registering a new identity costs around 30 Entry Credits \($0.03\). You'll need to wait up to 10 minutes for the block to complete to view and use your new identity.  
****

**Issuing A FAT Token**

Using the wallet you can issue new tokens for your application or organization's needs. Parameters are flexible to allow FAT tokens to fit your use case. You can define basic properties like Display Name & Symbol, Token Type, and rules like Maximum Token Supply.

Once you've set your parameters you can add an initial set of recipients to receive the newly created tokens. After issuance, you can sell your tokens using traditional business processes or integrate them into your application's functionality to implement token economies.

## FAT Resources

* [Factom Asset Tokens Website](https://www.fatprotocol.org/)
  * [Factom Asset Tokens Testnet Explorer](https://www.fatprotocol.org/explorer)
  * [Factom Asset Tokens Contact Us](mailto:contact@fatprotocol.org?subject=FAT%20Protocol%20Contact%20Us)
* [Factom Asset Token Specifications Repository](https://github.com/Factom-Asset-Tokens/FAT)
* [Factom Asset Tokens Testnet Explorer](https://explorer.factoid.org/data?type=fat-tokens) \(TFA\)
* * [Factom Asset Token Discord Community](https://discord.com/invite/8ADPfSc)



