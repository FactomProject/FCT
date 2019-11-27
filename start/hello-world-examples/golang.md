---
description: Factom "Hello world" in golang.
---

# Golang

## Prerequisites

* A valid Entry Credit address with a few EC \(see how to do that [here](https://developers.factomprotocol.org/wallets/enterprise-wallet)\)
* A working Go environment \(get that [here](https://golang.org/doc/install)\)
* A copy of the Factom package \(run: `go get github.com/FactomProject/factom` \)
* Internet connection

## Getting Started with Development

### Connect to a factomd server node

If you want to setup your own factomd node, head over to the [developer sandbox setup guide](https://developers.factomprotocol.org/start/developer-sandbox-setup-guide) and follow the instructions there, but for the sake of simplicity, it'd be easier to use the courtesy node from Factom Inc. They host a remote node for the community to access.

Create a new `.go` file and import the FactomProject/factom package. Now in the `init()` function, you'll set the url for the live `factomd` instance you want to work with.

{% code title="hello-world-factom" %}
```go
package main


import(
    "github.com/FactomProject/factom"
)

func init() {
    // Connect to Factom Open Node
    factom.SetFactomdServer("https://api.factomd.net")
}
```
{% endcode %}

### Writing data to a chain

Bitcoin has a single longest chain that everyone agrees to build upon. In Factom there isn't just one big stack everyone keeps throwing information on top of, but rather, many smaller independent chains that are organized into one large chain \(which is anchored into Bitcoin every 10 minutes\). The organizational layers/concepts in the Factom system are:

1. Directory Layer -- Organizes the Merkle Roots of Entry Blocks
2. Entry Block Layer -- Organizes references to Entries
3. Entries -- Contains an Application's raw data or a hash of its private data
4. Chains -- Grouping of entries specific to an Application

To access a specific chain, you usually will use its ChainName or ChainID. Here's a sample ChainID:`18de01b1ba2cea239c701501df630e93936c658561a33d1c0d08ee5281790aee`

Copy that and go to the [factom explorer](https://explorer.factom.com/) to search for it. You'll find

[![Block Explorer](https://github.com/sambarnes/awesome-factom/raw/master/introduction/screenshot-1.png)](https://github.com/sambarnes/awesome-factom/blob/master/introduction/screenshot-1.png)

Click the first entry in the chain. This is what the Chain Name points to. The ChainName is the set of External IDs of the first entry in a chain, this set of ExtIDs must be unique among all other first entries in Factom. The ChainID is formed by a hashing the series of ChainName segments, then those hashes are concatenated, and are hashed again into a single 32 byte ChainID. \(see the [data structures doc](https://developers.factomprotocol.org/start/factom-data-structures) for more info.\)

ChainID = _SHA256\( SHA256\(ChainName\[0\]\) \| SHA256\(ChainName\[1\]\) \| ... \| SHA256\(ChainName\[n\]\) \)_

[![First Entry](https://github.com/sambarnes/awesome-factom/raw/master/introduction/screenshot-2.png)](https://github.com/sambarnes/awesome-factom/blob/master/introduction/screenshot-2.png)

To write your own data to the chain, construct a new Factom Entry with the following:

* ChainID - the chain you want to write to
* ExtIDs - optional IDs or tags that are interpretted client side, in some application specific manner
* Content - the raw or hashed data you want to put on the blockchain

See [here](https://developers.factomprotocol.org/start/factom-data-structures/user-elements#entry) for more specific details on what makes up an Entry.

{% code title="hello-world-factom" %}
```go
...


func main() {
    // TODO: don't store key in plaintext, use a more secure way
    ecSecret := "YOUR EC PRIVATE KEY HERE"
    ecAddress, err := factom.GetECAddress(ecSecret)
    if err != nil {
        log.Fatal(err)
    }

    // create a new Factom entry
    e := factom.Entry{}
    e.ChainID = "18de01b1ba2cea239c701501df630e93936c658561a33d1c0d08ee5281790aee"
    e.ExtIDs = [][]byte{[]byte("ExampleTag")}
    e.Content = []byte("Hello, Factom!")
}
```
{% endcode %}

{% hint style="info" %}
Note that:

* ExtIDs & Content are optional for Entries
* Content is optional for Chains
{% endhint %}

Now you'll have to commit the Entry. This is signed with the specified EC address and broadcast to the network. At this point, no one else in the factom network knows what your Entry actually contains, they can only see that you paid for the ability to publish content and that you provided a hash of that content. When you reveal the Entry, your content then becomes public and able to be checked against what you committed to publishing just before.

{% code title="hello-world-factom" %}
```go
func main() {
    ...

    // Commit the entry, then reveal it
    txID, err := factom.CommitEntry(&e, ecAddress)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Transaction ID: %s", txID)

    if _, err := factom.RevealEntry(&e); err != nil {
        log.Fatal(err)
    }
}
```
{% endcode %}

Build and run the program, and it should print your transaction ID.

```bash
go build
go ./introduction
```

Copy the transaction ID and paste it into the block explorer. It'll show up in the next directory block within 10 minutes or so.

### Creating your own chain

Just like we did for writing something to an existing chain, we'll use the same commit-reveal scheme. Create an Entry, and give it a set of ExtIDs. Remember from earlier that since this is the first entry of a chain, the ExtIDs are used as the ChainName, which is in turn used to calculate the ChainID.

{% code title="hello-world-factom" %}
```go
func main() {
    // TODO: don't store key in plaintext, use a more secure way
    ecSecret := "YOUR EC PRIVATE KEY HERE"
    ecAddress, err := factom.GetECAddress(ecSecret)
    if err != nil {
        log.Fatal(err)
    }

    chainName := []string{"Factom Tutorial", "Intro", "YOUR TEST CHAIN NAME"}
    chainID := ConstructChainID(chainName)

    // create a new Factom entry
    e := factom.Entry{}
    e.ChainID = chainID
    e.Content = []byte("Something important...")

    if !factom.ChainExists(chainID) {
        // Chain doesn't exist yet. Create, commit, and reveal it.
        for _, nameSegment := range chainName {
            e.ExtIDs = append(e.ExtIDs, []byte(nameSegment))
        }
        c := factom.NewChain(&e)

        if _, err := factom.CommitChain(c, ecAddress); err != nil {
            log.Fatal(err)
        }
        if _, err := factom.RevealChain(c); err != nil {
            log.Fatal(err)
        }
        fmt.Printf("Chain Created: %s", chainID)
        return
    }

    // The chain exists already. So ExtID is no longer the chain name.
    // Treat them however you want. Tags, flags, names, priorities, etc.
    e.ExtIDs = [][]byte{[]byte("ExampleTag")}

    // Commit the entry, then reveal it
    txID, err := factom.CommitEntry(&e, ecAddress)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Transaction ID: %s", txID)

    if _, err := factom.RevealEntry(&e); err != nil {
        log.Fatal(err)
    }
}

// ConstructChainID takes the ChainName as a string array and returns its ChainID
// see: https://developers.factomprotocol.org/start/factom-data-structures
func ConstructChainID(chainName []string) string {
    e := factom.Entry{}
    for _, nameSegment := range chainName {
        e.ExtIDs = append(e.ExtIDs, []byte(nameSegment))
    }
    chain := factom.NewChain(&e)
    return chain.ChainID
}
```
{% endcode %}

Pick your own ChainName, run it a first time, and you should be able to [find your newly created chain](https://developers.factomprotocol.org/start/factom-explorer/usage#search-for-a-factom-chain) at the ChainID that is printed. If you run it a second time, another entry will be added to the chain that was just created.

{% hint style="success" %}
Congrats! You've learned the basic building blocks of Factom in go and are ready to start building applications with tamper-proof data.
{% endhint %}

{% hint style="info" %}
If you want to continue with a more in-depth tutorial, check out the following great tutorials that build on the one you just went through:

* [Proof of Life: an Intro to Client-Side Validation](https://github.com/sambarnes/awesome-factom/blob/master/proof-of-life-pt1/proof-of-life-pt1.md)
* [Proof of Life \(part 2\): Structured Chains](https://github.com/sambarnes/awesome-factom/blob/master/proof-of-life-pt2/proof-of-life-pt2.md)
{% endhint %}



