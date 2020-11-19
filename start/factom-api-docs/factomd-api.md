# Factomd

This is the API reference for the factomd process. The API is designed for outside application to process transactions and interact with the Factom federated servers. It’s listening port can be configured and runs on 8088 by default.

All these APIs use JSON-RPC, which is a remote procedure call protocol encoded in JSON. It is a very simple protocol \(and very similar to XML-RPC\), defining only a handful of data types and commands.

They can be invoked in your terminal as:

`curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": "METHOD_HERE", "params": {"PARAM_1":"PARAM_DATA_1"}}' -H 'content-type:text/plain;' http://localhost:8088/v2`

The output will also be JSON.

The right panel will contain the curl commands to be run in your terminal, as well as the JSON structures of the request and response. The JSON structs detail the required parameters for each call.

## ablock-by-height

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method":
"ablock-by-height", "params": {"height":1}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "ablock": {
      "header": {
        "prevbackrefhash": "77e4fb398e228ec9710c20988647a01e2259a40ab77e27c005baf7f2deae3415",
        "dbheight": 14460,
        "headerexpansionsize": 0,
        "headerexpansionarea": "",
        "messagecount": 4,
        "bodysize": 516,
        "adminchainid": "000000000000000000000000000000000000000000000000000000000000000a",
        "chainid": "000000000000000000000000000000000000000000000000000000000000000a"
      },
      "abentries": [
        {
          "identityadminchainid": "888888e238492b2d723d81f7122d4304e5405b18bd9c7cb22ca6bcbc1aab8493",
          "prevdbsig": {
            "pub": "0186ad82617edf3565d944aa104590eb6adb338e92ee6fcd750c2ab2b2707e25",
            "sig": "5796cd49835088ea0d6b8e4a75611ebc674fb791d6e9ebc7f6e5bb1a5e86fc25a8a7742e8f60870e2cb8523fd122ef54bb95ac94b3676b81e07c921ed2196508"
          }
        },
        {
          "identityadminchainid": "888888fc37fa418395eeccb95ab0a4c64d528b2aeefa0d1632c8a116a0e4f5b1",
          "prevdbsig": {
            "pub": "c845f47df202a649e2262d3da0e35556aab62e361425ad7d2e7813a215c8f277",
            "sig": "a5c976c4d18814916fc893f7b4dee78120d20e0deab2b04df2e3b67c2ea1123224db28559ca6d022822388a5ce41128bf5a09ccbbd02b1c5b17a4152183a3d06"
          }
        },
        {
          "identityadminchainid": "88888815ac8a1ab6b8f57cee67ba15aad23ab7d8e70ffdca064200738c201f74",
          "prevdbsig": {
            "pub": "f18512813300d8c1d11e78216d0640ddcc35156a20b53d5ced351a7d5ad90010",
            "sig": "1051c165d7ad33e1f764bb96e5e661053da381ebd708c8ac137da2a1b6847eac07e83472d4fa6096768c7904760c821e45b5ebe23a691cc5bad1b61937f9e303"
          }
        },
        {
          "identityadminchainid": "888888271203752870ae5e6fa0cf96f93cf14bd052455ad476ab26de1ad2c077",
          "prevdbsig": {
            "pub": "4f2d34f0417297e2e985e0cc6e4cf3d0814416d09f37af7375517ea236786ed3",
            "sig": "01206ff2963af7df29bb6749a4c29bc1eb65a48bd7b3ec6590c723e11c3a3c5342e72f9b079a58d77a2562c25289d799fadfc5205f1e99c4f1d5c3ce85432906"
          }
        }
      ],
      "backreferencehash": "1786de6a72311dd4b60c6608d60c2b9367642fb1ee6b867b2c9f4c57c87b8cba",
      "lookuphash": "574e7d6178e04c92879601f0cb84a619f984eb2617ff9e76ee830a9f614cc9a0"
    },
    "rawdata": "000000000000000000000000000000000000000000000000000000000000000a77e4fb398e228ec9710c20988647a01e2259a40ab77e27c005baf7f2deae34150000387c00000000040000020401888888e238492b2d723d81f7122d4304e5405b18bd9c7cb22ca6bcbc1aab84930186ad82617edf3565d944aa104590eb6adb338e92ee6fcd750c2ab2b2707e255796cd49835088ea0d6b8e4a75611ebc674fb791d6e9ebc7f6e5bb1a5e86fc25a8a7742e8f60870e2cb8523fd122ef54bb95ac94b3676b81e07c921ed219650801888888fc37fa418395eeccb95ab0a4c64d528b2aeefa0d1632c8a116a0e4f5b1c845f47df202a649e2262d3da0e35556aab62e361425ad7d2e7813a215c8f277a5c976c4d18814916fc893f7b4dee78120d20e0deab2b04df2e3b67c2ea1123224db28559ca6d022822388a5ce41128bf5a09ccbbd02b1c5b17a4152183a3d060188888815ac8a1ab6b8f57cee67ba15aad23ab7d8e70ffdca064200738c201f74f18512813300d8c1d11e78216d0640ddcc35156a20b53d5ced351a7d5ad900101051c165d7ad33e1f764bb96e5e661053da381ebd708c8ac137da2a1b6847eac07e83472d4fa6096768c7904760c821e45b5ebe23a691cc5bad1b61937f9e30301888888271203752870ae5e6fa0cf96f93cf14bd052455ad476ab26de1ad2c0774f2d34f0417297e2e985e0cc6e4cf3d0814416d09f37af7375517ea236786ed301206ff2963af7df29bb6749a4c29bc1eb65a48bd7b3ec6590c723e11c3a3c5342e72f9b079a58d77a2562c25289d799fadfc5205f1e99c4f1d5c3ce85432906"
  }
}
```

Retrieve administrative blocks for any given height.

The admin block contains data related to the identities within the factom system and the decisions the system makes as it builds the block chain. The ‘abentries’ \(admin block entries\) in the JSON response can be of various types, the most common is a directory block signature \(DBSig\). A majority of the federated servers sign every directory block, meaning every block after m5 will contain 5 DBSigs in each admin block.

## ack

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, 
"method":"ack", "params":{"hash":
"e96cca381bf25f6dd4dfdf9f7009ff84ee6edaa3f47f9ccf06d2787482438f4b", "chainid":"f9164cd66af9d5773b4523a510b5eefb9a5e626480feeb6671ef2d17510ca300", "fulltransaction":""}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

This API call is used to find the status of a transaction, whether it be a factoid, reveal entry, or commit entry. When using this, you must specify the type of the transaction by giving the `chainid` field 1 of 3 values:

* `f` for factoid transactions
* `c` for entry credit transactions \(commit entry/chain\)
* `################################################################` for reveal entry/chain
  * Where `#` is the ChainID of the entry

The status types returned are as follows:

* “_Unknown_” : Not found anywhere
* “_NotConfirmed_” : Found on local node, but not in network \(Holding Map\)
* “_TransactionACK_” : Found in network, but not written to the blockchain yet \(ProcessList\)
* “_DBlockConfirmed_” : Found in Blockchain

You may also provide the full marshaled transaction, instead of a hash, and it will be hashed for you.

The responses vary based on the type:

### Entries

> Entry Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "committxid":"debbbb6b902de330bfaa78c6c9107eb0a451e10cd4523e150a8e8e6d5a042886",
      "entryhash":"1a6c96162e81d429de92b2f18a0ba9b428e505de0077a5d16ad5707f0f8a73b2",
      "commitdata":{  
         "status":"DBlockConfirmed"
      },
      "entrydata":{  
         "status":"DBlockConfirmed"
      }
   }
}
```

Requesting an entry requires you to specify if the hash you provide is a commit or an entry hash. The `chainid` field is used to specify this. If you are searching for a commit, put `c` as the chainid field, otherwise, put the chainid that the entry belongs too.

For commit/reveal acks, the response has 2 sections, one for the commit, one for the reveal. If you provide the entryhash and chainid, both will be filled \(if found\). If you only provide the commit txid and `c` as the chainid, then only the commitdata is guaranteed to come back with data. The `committxid` and `entryhash` fields correspond to the `commitdata`and `entrydata` objects.

{% hint style="info" %}
Why `c`? It is short for:`000000000000000000000000000000000000000000000000000000000000000c`

which is the chainid for all entry credit blocks. All commits are placed in the entry credit block \(assuming they are valid and are properly paid for\).  
{% endhint %}

### Factoid Transactions

> Factoid Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "txid":"f1d9919829fa71ce18caf1bd8659cce8a06c0026d3f3fffc61054ebb25ebeaa0",
      "transactiondate":1441138021975,
      "transactiondatestring":"2015-09-01 15:07:01",
      "blockdate":1441137600000,
      "blockdatestring":"2015-09-01 15:00:00",
      "status":"DBlockConfirmed"
   }
}
```

The `hash` field for a factoid transaction is equivalent to `txid`. To indicate the hash is a factoid transaction, put `f` in the chainid field and the `txid` in the hash field.

The response will look different than entry related ack calls.

{% hint style="info" %}
Why `f`? It is short for:

`000000000000000000000000000000000000000000000000000000000000000f`

which is the chainid for all factoid blocks. All factoid transactions are placed in the factoid \(assuming they are valid\).
{% endhint %}

## admin-block

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": 
"admin-block", "params": {"keymr":"cc03cb3558b6b1acd24c5439fadee6523dd2811af82affb60f056df3374b39ae"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "ablock":{  
         "header":{  
            "prevbackrefhash":"452201ee821c28ff601cd70a20c492d3f2a84a543c492b97b631d8f5e575c712",
            "dbheight":100,
            "headerexpansionsize":0,
            "headerexpansionarea":"",
            "messagecount":2,
            "bodysize":131,
            "adminchainid":"000000000000000000000000000000000000000000000000000000000000000a",
            "chainid":"000000000000000000000000000000000000000000000000000000000000000a"
         },
         "abentries":[  
            {  
               "identityadminchainid":"0000000000000000000000000000000000000000000000000000000000000000",
               "prevdbsig":{  
                  "pub":"0426a802617848d4d16d87830fc521f4d136bb2d0c352850919c2679f189613a",
                  "sig":"50ed8fb2440b1d1e2036f617f64224a347c797898d7433e1934db47b1e057f250af772404c9d595a1f86d44d5034f4985ecdddc01c23e5945efe1fc866ea8505"
               }
            },
            {  
               "minutenumber":1
            }
         ],
         "backreferencehash":"7cbd0f9b59e3ebe5f34836c6d7d0d42b9cafafb8cb348d71cb7c66b6de782d7a",
         "lookuphash":"cc03cb3558b6b1acd24c5439fadee6523dd2811af82affb60f056df3374b39ae"
      },
      "rawdata":"000000000000000000000000000000000000000000000000000000000000000a452201ee821c28ff601cd70a20c492d3f2a84a543c492b97b631d8f5e575c712000000640000000002000000830100000000000000000000000000000000000000000000000000000000000000000426a802617848d4d16d87830fc521f4d136bb2d0c352850919c2679f189613a50ed8fb2440b1d1e2036f617f64224a347c797898d7433e1934db47b1e057f250af772404c9d595a1f86d44d5034f4985ecdddc01c23e5945efe1fc866ea85050001"
   }
}
```

Retrieve a specified admin block given its merkle root key.

## anchors

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": 
"anchors", "params": {"height":200000}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
   "jsonrpc":"2.0",
   "id":0,
   "result":{
      "directoryblockheight":200000,
      "directoryblockkeymr":"ce86fc790dd1462aea255adaa64e2f21c871995df2c2c119352d869fa1d7269f",
      "bitcoin":{
         "transactionhash":"6d2d1e506528ae3b476d70fb05517bbbb152a4698a23ff78b4d87249027f53ca",
         "blockhash":"0000000000000000000234e270b3fa6de63caad8a319731db5643ddb16b80cdf"
      },
      "ethereum":{
         "recordheight":200001,
         "dbheightmax":200000,
         "dbheightmin":199001,
         "windowmr":"935480547a2545161438da05136ec4238d88e7f8e1075687d8292fcafc1d0b22",
         "merklebranch":[
            {
               "left":"a92a4460a9555c8b57a282e7ad4514d0f5aa8a612963da79db0e7cede6299bcd",
               "right":"ce86fc790dd1462aea255adaa64e2f21c871995df2c2c119352d869fa1d7269f",
               "top":"bbbf494fff20d8fbada47980498a96e49fba920fd1ca4e0586af4961e4318354"
            },
            {
               "left":"b41ffe5f9b710edf6bcf1a6d3e92685155411111d640f2b2224ab15f04c46161",
               "right":"bbbf494fff20d8fbada47980498a96e49fba920fd1ca4e0586af4961e4318354",
               "top":"ca1e7d59dc295c48ebddf6c068826584fb5adb38798f5cac7f964509627825f5"
            },
            {
               "left":"c919c0fb94900f3016c75a7f9f992c79aa7db126db6c6c3b1af92bbee174bda6",
               "right":"ca1e7d59dc295c48ebddf6c068826584fb5adb38798f5cac7f964509627825f5",
               "top":"3c8e97e32b4dbc0e4d82f066217492ca72ca722b91dbc764c311c39a9196bdda"
            },
            {
               "left":"3c8e97e32b4dbc0e4d82f066217492ca72ca722b91dbc764c311c39a9196bdda",
               "right":"3c8e97e32b4dbc0e4d82f066217492ca72ca722b91dbc764c311c39a9196bdda",
               "top":"5713abb24c8a8480f5c04bfdb8668a93e8b0a24a0562afbc7f362d665938a89e"
            },
            {
               "left":"5713abb24c8a8480f5c04bfdb8668a93e8b0a24a0562afbc7f362d665938a89e",
               "right":"5713abb24c8a8480f5c04bfdb8668a93e8b0a24a0562afbc7f362d665938a89e",
               "top":"a7ab3a72f5e754f3e10f04b9f7f2e50f36c688776b521918a755e62fd6c2da34"
            },
            {
               "left":"5e42d2f5ba59338b317369bbc53fbb70c094364ae8c15c9a291dad65dec9d839",
               "right":"a7ab3a72f5e754f3e10f04b9f7f2e50f36c688776b521918a755e62fd6c2da34",
               "top":"bc5c336d7e53b6d9f2832038c69a78eb7300bd539b6002d44f450a4d2c979aa3"
            },
            {
               "left":"31041410eb7bd6a9fabe974a0f4eabe477ffbbf5806fdaee5da98faff63b8142",
               "right":"bc5c336d7e53b6d9f2832038c69a78eb7300bd539b6002d44f450a4d2c979aa3",
               "top":"f9b5967e2acadfac93bc2141ee9f85b0649f89452e44e4f8b1cbe877457a0da4"
            },
            {
               "left":"04aa1b2d35ea99becd92bcfccc7406aea37287a58425b83349d0fc8cc7bec443",
               "right":"f9b5967e2acadfac93bc2141ee9f85b0649f89452e44e4f8b1cbe877457a0da4",
               "top":"77eca087628e243bf2914162be6c4aefaf736ed56e8c125b955cef3c48a1ab4b"
            },
            {
               "left":"be8394e3f3a205ad37afb8af778695e2914f071b3d215e586a93288fd692eb00",
               "right":"77eca087628e243bf2914162be6c4aefaf736ed56e8c125b955cef3c48a1ab4b",
               "top":"12aa5f990bd209dfaa83d9ea050e96e3b44e54ffee02252028fe969da3ab690d"
            },
            {
               "left":"2bdead51afe10d07c5dc5b29351fedd4a13849261359a6f22100dcc7ff50543a",
               "right":"12aa5f990bd209dfaa83d9ea050e96e3b44e54ffee02252028fe969da3ab690d",
               "top":"935480547a2545161438da05136ec4238d88e7f8e1075687d8292fcafc1d0b22"
            }
         ],
         "contractaddress":"0xfac701d9554a008e48b6307fb90457ba3959e8a8",
         "txid":"0xd57492c9d505e3052c454acdfc3768bc3eb8859c91829654346503ef2dcb6a23",
         "blockhash":"0x49e8f3c394079e6c3964fdf943ea9759475d4ca5aec90c9a695be07efdd88d32",
         "txindex":31
      }
   }
}
```

Look up the anchors for a specific directory block by either its height or its hash. Returns both Bitcoin and Ethereum anchor entries if they exist. If a block doesn't have an anchor record for Bitcoin or Ethereum, it will return "false" in that place.

## chain-head

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method":
"chain-head", "params":{"chainid":"5a77d1e9612d350b3734f6282259b7ff0a3f87d62cfef5f35e91a5604c0490a3"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "chainhead": "f65f67774139fa78344dcdd302631a0d646db0c2be4d58e3e48b2a188c1b856c",
    "chaininprocesslist":false
  }
}
```

Return the keymr of the head of the chain for a chain ID \(the unique hash created when the chain was created\).

## commit-chain

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": "commit-chain", "params":
{"message":
"00015507b2f70bd0165d9fa19a28cfaafb6bc82f538955a98c7b7e60d79fbf92655c1bff1c76466cb3bc3f3cc68d8b2c111f4f24c88d9c031b4124395c940e5e2c5ea496e8aaa2f5c956749fc3eba4acc60fd485fb100e601070a44fcce54ff358d606698547340b3b6a27bcceb6a42d62a3a8d02a6f0d73653215771de243a63ac048a18b59da2946c901273e616bdbb166c535b26d0d446bc69b22c887c534297c7d01b2ac120237086112b5ef34fc6474e5e941d60aa054b465d4d770d7f850169170ef39150b"}}' \ 
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "message":"Chain Commit Success",
      "txid":"76e123d133a841fe3e08c5e3f3d392f8431f2d7668890c03f003f541efa8fc61",
      "entryhash": "f5c956749fc3eba4acc60fd485fb100e601070a44fcce54ff358d60669854734",
      "chainid": "  f9164cd66af9d5773b4523a510b5eefb9a5e626480feeb6671ef2d17510ca300"
   }
}
```

Send a Chain Commit Message to factomd to create a new Chain. The commit chain hex encoded string is documented [here](https://developers.factomprotocol.org/start/factom-data-structures/user-elements#chain-commit).

The commit-chain API takes a specifically formated message encoded in hex that includes signatures. If you have a factom-walletd instance running, you can construct this commit-chain API call with [compose-chain](https://developers.factomprotocol.org/start/factom-api-docs/factom-walletd-api#compose-chain) which takes easier to construct arguments.

The [compose-chain](https://developers.factomprotocol.org/start/factom-api-docs/factom-walletd-api#compose-chain) api call has two api calls in it’s response: commit-chain and [reveal-chain](https://developers.factomprotocol.org/start/factom-api-docs/factomd-api#reveal-chain). To successfully create a chain, the reveal-chain must be called after the commit-chain.

{% hint style="warning" %}
It is possible to be unable to send a commit, if the commit already exists \(if you try to send it twice\). This is a mechanism to prevent you from double spending. If you encounter this error, just skip to the [reveal-chain](https://developers.factomprotocol.org/start/factom-api-docs/factomd-api#reveal-chain). The error format can be found here: [repeated-commit](https://developers.factomprotocol.org/start/factom-api-docs/factomd-api#repeated-commit)  
{% endhint %}

## commit-entry

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": "commit-entry", "params":
{"message":"00015507C1024BF5C956749FC3EBA4ACC60FD485FB100E601070A44FCCE54FF358D60669854734013B6A27BCCEB6A42D62A3A8D02A6F0D73653215771DE243A63AC048A18B59DA29F4CBD953E6EBE684D693FDCA270CE231783E8ECC62D630F983CD59E559C6253F84D1F54C8E8D8665D493F7B4A4C1864751E3CDEC885A64C2144E0938BF648A00"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "message": "Entry Commit Success",
    "txid": "bf12150038699f678ac2314e9fa2d4786dc8984d9b8c67dab8cd7c2f2e83372c",
    "entryhash": "f5c956749fc3eba4acc60fd485fb100e601070a44fcce54ff358d60669854734"
    }
}
```

Send an Entry Commit Message to factom to create a new Entry. The entry commit hex encoded string is documented [here](https://developers.factomprotocol.org/start/factom-data-structures/user-elements#entry-commit).

The commit-entry API takes a specifically formated message encoded in hex that includes signatures. If you have a factom-walletd instance running, you can construct this commit-entry API call with [compose-entry](https://developers.factomprotocol.org/start/factom-api-docs/factom-walletd-api#compose-entry) which takes easier to construct arguments.

The compose-entry api call has two api calls in it’s response: commit-entry and [reveal-entry](https://developers.factomprotocol.org/start/factom-api-docs/factomd-api#reveal-entry). To successfully create an entry, the reveal-entry must be called after the commit-entry.

{% hint style="warning" %}
It is possible to be unable to send a commit, if the commit already exists \(if you try to send it twice\). This is a mechanism to prevent you from double spending. If you encounter this error, just skip to the [reveal-entry](https://developers.factomprotocol.org/start/factom-api-docs/factomd-api#reveal-entry). The error format can be found here: [repeated-commit](https://developers.factomprotocol.org/start/factom-api-docs/factomd-api#repeated-commit)
{% endhint %}

## current-minute

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": "current-minute"}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
    "jsonrpc": "2.0",
    "id": 0,
    "result": {
        "leaderheight": 4418,
        "directoryblockheight": 4418,
        "minute": 1,
        "currentblockstarttime": 1535639576826240687,
        "currentminutestarttime": 1535639590186099105,
        "currenttime": 1535639593259007218,
        "directoryblockinseconds": 100,
        "stalldetected": false,
        "faulttimeout": 120,
        "roundtimeout": 30
    }
}
```

The _current-minute_ API call returns:

* `leaderheight` returns the current block height.
* `directoryblockheight` returns the last saved height.
* `minute` returns the current minute number for the open entry block.
* `currentblockstarttime` returns the start time for the current block.
* `currentminutestarttime` returns the start time for the current minute.
* `currenttime` returns the current nodes understanding of current time.
* `directoryblockinseconds` returns the number of seconds per block.
* `stalldetected` returns if factomd thinks it has stalled.
* `faulttimeout` returns the number of seconds before leader node is faulted for failing to provide a necessary message.
* `roundtimeout` returns the number of seconds between rounds of an election during a fault.

## dblock-by-height

Retrieves a directory block given only its height.

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": 
"dblock-by-height", "params":{"height":14460}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "dblock": {
      "header": {
        "version": 0,
        "networkid": 4203931043,
        "bodymr": "7716df6083612597d4ef18a8076c40676cd8e0df8110825c07942ca5d30073b4",
        "prevkeymr": "fa7e6e2d37b012d71111bc4e649f1cb9d6f0321964717d35636e4637699d8da2",
        "prevfullhash": "469c7fdce467d222363d55ac234901d8acc61fd4045ae26dd54dac17c556ef86",
        "timestamp": 24671414,
        "dbheight": 14460,
        "blockcount": 3,
        "chainid": "000000000000000000000000000000000000000000000000000000000000000d"
      },
      "dbentries": [
        {
          "chainid": "000000000000000000000000000000000000000000000000000000000000000a",
          "keymr": "574e7d6178e04c92879601f0cb84a619f984eb2617ff9e76ee830a9f614cc9a0"
        },
        {
          "chainid": "000000000000000000000000000000000000000000000000000000000000000c",
          "keymr": "2a10f1678b9736f213ef3ac76e4f8aa910e5fed66733aa30dafdc91245157b3b"
        },
        {
          "chainid": "000000000000000000000000000000000000000000000000000000000000000f",
          "keymr": "cbadd7e280377ad8360a4b309df9d14f56552582c05100145ca3367e50adc497"
        }
      ],
      "dbhash": "aa7d881d23aad83425c3f10996999d31c76b51b14946f7aca204c150e81bc6d6",
      "keymr": "18509c431ee852edbe1029d676217a0d9cb4fcc11ef8e9aef27fd6075167120c"
    },
    "rawdata": "00fa92e5a37716df6083612597d4ef18a8076c40676cd8e0df8110825c07942ca5d30073b4fa7e6e2d37b012d71111bc4e649f1cb9d6f0321964717d35636e4637699d8da2469c7fdce467d222363d55ac234901d8acc61fd4045ae26dd54dac17c556ef86017874b60000387c00000003000000000000000000000000000000000000000000000000000000000000000a574e7d6178e04c92879601f0cb84a619f984eb2617ff9e76ee830a9f614cc9a0000000000000000000000000000000000000000000000000000000000000000c2a10f1678b9736f213ef3ac76e4f8aa910e5fed66733aa30dafdc91245157b3b000000000000000000000000000000000000000000000000000000000000000fcbadd7e280377ad8360a4b309df9d14f56552582c05100145ca3367e50adc497"
  }
}
```

## directory-block

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": 
"directory-block", "params": {"keymr":"7ed5d5b240973676c4a8a71c08c0cedb9e0ea335eaef22995911bcdc0fe9b26b"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "header":{  
         "prevblockkeymr":"7d15d82e70201e960655ce3e7cf475c9da593dfb82c6dca6377349bd148bf001",
         "sequencenumber":72497,
         "timestamp":1484858820
      },
      "entryblocklist":[  
         {  
            "chainid":"000000000000000000000000000000000000000000000000000000000000000a",
            "keymr":"3faa880a97ef6ce1feca643cffa015dd6be6a597b3f9260e408c5ac9351d1f8d"
         },
         {  
            "chainid":"000000000000000000000000000000000000000000000000000000000000000c",
            "keymr":"5f8c98930a1874a46b47b65b9376a02fbff65b760f6866519799d69e2bc019ee"
         },
         {  
            "chainid":"000000000000000000000000000000000000000000000000000000000000000f",
            "keymr":"8c6fed0f41317cc45201b5b170a9ac5bc045029e39a90b6061211be2c0678718"
         }
      ]
   }
}
```

Every directory block has a KeyMR \(Key Merkle Root\), which can be used to retrieve it. The response will contain information that can be used to navigate through all transactions \(entry and factoid\) within that block. The header of the directory block will contain information regarding the previous directory block’s keyMR, directory block height, and the timestamp.

## directory-block-head

> Example Request

```text
curl -X POST --data-binary \
'{"jsonrpc": "2.0", "id": 0, "method": "directory-block-head"}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "keymr":"7ed5d5b240973676c4a8a71c08c0cedb9e0ea335eaef22995911bcdc0fe9b26b"
   }
}
```

The directory block head is the last known directory block by factom, or in other words, the most recently recorded block. This can be used to grab the latest block and the information required to traverse the entire blockchain.

## ecblock-by-height

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method":
"ecblock-by-height", "params": {"height":10000}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "ecblock":{  
         "header":{  
            "bodyhash":"4d1693377deddda4f23c8c2523a5c47a78b607312845e319a4131c82c6a44819",
            "prevheaderhash":"d7dede56d2e7d46d879827fc6e4a3aca1be2816c9dca640686ec5f63cbb1fd9f",
            "prevfullhash":"2bc12a0c7d5dd282fe62dd220d575f6834f188b587091f4ab50815bca1dcfd99",
            "dbheight":10000,
            "headerexpansionarea":"",
            "objectcount":13,
            "bodysize":296,
            "chainid":"000000000000000000000000000000000000000000000000000000000000000c",
            "ecchainid":"000000000000000000000000000000000000000000000000000000000000000c"
         },
         "body":{  
            "entries":[  
               {  
                  "serverindexnumber":0
               },
               {  
                  "version":0,
                  "millitime":"0150f0bb0c15",
                  "entryhash":"b87f8623b15a4574527699f594c1d77986dc140dbd4f4b277f35331a1f22f92e",
                  "credits":1,
                  "ecpubkey":"4bcbc1c5ab90e432bd407a51eaa513b4050eecda1fd42bbf6b7050a1d96f94b7",
                  "sig":"99c8d4b12e6dfc2ad5b1ee0da12dd25becd43808ded49dfa12f5667ba6ad07e8e44478912a219dfccd02f1110531f8b43a21d09c4af455b5c4ac398cb5926902"
               },
               {  
                  "number":1
               },
               {  
                  "number":2
               },
               {  
                  "number":3
               },
               {  
                  "number":4
               },
               {  
                  "version":0,
                  "millitime":"0150f0bf8808",
                  "entryhash":"5759b2609abf0d224250db124559ff51fcd739b29dbb3b32a619c5cc74a0318a",
                  "credits":1,
                  "ecpubkey":"3b6a27bcceb6a42d62a3a8d02a6f0d73653215771de243a63ac048a18b59da29",
                  "sig":"848506d790ce31287a3b97933631290f0e75cadf8be1cdf67a7e362bcdf15fc1fd50988ae001c683ac133f23c9a9af60521688870e96d78038e27d683960810a"
               },
               {  
                  "number":5
               },
               {  
                  "number":6
               },
               {  
                  "number":7
               },
               {  
                  "number":8
               },
               {  
                  "number":9
               },
               {  
                  "number":10
               }
            ]
         }
      },
      "rawdata":"000000000000000000000000000000000000000000000000000000000000000c4d1693377deddda4f23c8c2523a5c47a78b607312845e319a4131c82c6a44819d7dede56d2e7d46d879827fc6e4a3aca1be2816c9dca640686ec5f63cbb1fd9f2bc12a0c7d5dd282fe62dd220d575f6834f188b587091f4ab50815bca1dcfd990000271000000000000000000d0000000000000128000003000150f0bb0c15b87f8623b15a4574527699f594c1d77986dc140dbd4f4b277f35331a1f22f92e014bcbc1c5ab90e432bd407a51eaa513b4050eecda1fd42bbf6b7050a1d96f94b799c8d4b12e6dfc2ad5b1ee0da12dd25becd43808ded49dfa12f5667ba6ad07e8e44478912a219dfccd02f1110531f8b43a21d09c4af455b5c4ac398cb5926902010101020103010403000150f0bf88085759b2609abf0d224250db124559ff51fcd739b29dbb3b32a619c5cc74a0318a013b6a27bcceb6a42d62a3a8d02a6f0d73653215771de243a63ac048a18b59da29848506d790ce31287a3b97933631290f0e75cadf8be1cdf67a7e362bcdf15fc1fd50988ae001c683ac133f23c9a9af60521688870e96d78038e27d683960810a01050106010701080109010a"
   }
}
```

Retrieve the entry credit block for any given height. These blocks contain entry credit transaction information.

## entry

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method":"entry","params":
{"hash":"24674e6bc3094eb773297de955ee095a05830e431da13a37382dcdc89d73c7d7"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
 "jsonrpc":"2.0",
 "id":0,
 "result":{  
  "chainid":"df3ade9eec4b08d5379cc64270c30ea7315d8a8a1a69efe2b98a60ecdd69e604",
  "content":"...",
  "extids":[  
     "466163746f6d416e63686f72436861696e"
  ]
 }
}
```

Get an Entry from factomd specified by the Entry Hash.

## entry-ack

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, 
"method":"entry-ack", "params":{"txid":
"9228b4b080b3cf94cceea866b74c48319f2093f56bd5a63465288e9a71437ee8"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "committxid":"e5b5be39a41df43a3c46beaa238dc5e6f7bb11115a8da1a9b45cd694e257935a",
      "entryhash":"9228b4b080b3cf94cceea866b74c48319f2093f56bd5a63465288e9a71437ee8",
      "commitdata":{  
         "transactiondate":1449547801861,
         "transactiondatestring":"2015-12-07 22:10:01",
         "blockdate":1449547800000,
         "blockdatestring":"2015-12-07 22:10:00",
         "status":"DBlockConfirmed"
      },
      "entrydata":{  
         "blockdate":1449547800000,
         "blockdatestring":"2015-12-07 22:10:00",
         "status":"DBlockConfirmed"
      }
   }
}
```

{% hint style="danger" %}
**Deprecated**: Please refer to [ack](https://developers.factomprotocol.org/start/factom-api-docs/factomd-api#Ack)
{% endhint %}

Entry Acknowledgements will give the current status of a transaction. “DBlockConfirmed” is the highest level of confirmation you can obtain. This means the entry has made it into Factom.

## entry-block

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method":"entry-block", 
"params":{"keymr":"041c3fed14469a3d0f1a022e3d5321583065e691edb9223605c86766ff881883"}}'\
 -H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
 "jsonrpc":"2.0",
 "id":0,
 "result":{  
  "header":{  
   "blocksequencenumber":4211,
   "chainid":"0caff62ea5b5aa015c706add7b2463a5be07e1f0537617f553558090f23c7f56",
   "prevkeymr":"f7588bc41f03220a5ba4128432dc58b2027c05f432c91d79e6213ecdd5c923b3",
   "timestamp":1450147800,
   "dbheight":15000
  },
  "entrylist":[  
   {  
    "entryhash":"0ae2ab2cf543eed52a13a5a405bded712444cc8f8b6724a00602e1c8550a4ec2",
    "timestamp":1450147980
   }
  ]
 }
}
```

Retrieve a specified entry block given its merkle root key. The entry block contains 0 to many entries

## entry-credit-balance

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": 
"entry-credit-balance", "params":
{"address":"EC2DKSYyRcNWf7RS963VFYgMExoHRYLHVeCfQ9PGPmNzwrcmgm2r"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "balance": 2000
  }
}
```

Return its current balance for a specific entry credit address.

## entrycredit-block

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": 
"entrycredit-block", "params": {"keymr":"2050b16701f29238d6b99bcf3fb0ca55d6d884139601f06691fc370cda659d60"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "ecblock":{  
         "header":{  
            "bodyhash":"5eb9fea36690f9279360019d141faabef23b733030c2c6972f7870f38c22c385",
            "prevheaderhash":"cd62ba005d20e9384e28a29da742b6fc3d6b12a2458dbb5d9be3e9daf743036c",
            "prevfullhash":"848b20a3c499d9ade0fcc879e0febcc9fd107c6b6411627c262e44ce19499872",
            "dbheight":1998,
            "headerexpansionarea":"",
            "objectcount":14,
            "bodysize":433,
            "chainid":"000000000000000000000000000000000000000000000000000000000000000c",
            "ecchainid":"000000000000000000000000000000000000000000000000000000000000000c"
         },
         "body":{  
            "entries":[  
               {  
                  "serverindexnumber":0
               },
               {  
                  "number":1
               },
               {  
                  "version":0,
                  "millitime":"014fd1eafcba",
                  "entryhash":"5710aeb50e7ad82b3ad469e91019bc86b360796cfd4c037168ffe9040b7dfec7",
                  "credits":1,
                  "ecpubkey":"17ef7a21d1a616d65e6b73f3c6a7ad5c49340a6c2592872020ec60767ff00d7d",
                  "sig":"7cd6b1df3d3f9cf519a2437e799ee8acd815a2aeec7d5743295ff476632b070cb4459300a2bef1055e6fbb0c7ff5c62a9695f41be246c566051dd172816d7b06"
               },
               {  
                  "version":0,
                  "millitime":"014fd1eafcbc",
                  "entryhash":"ae8088e67f5bc7023f1b9dd84022c41067e8777e6436f73fc2384511ae637122",
                  "credits":1,
                  "ecpubkey":"17ef7a21d1a616d65e6b73f3c6a7ad5c49340a6c2592872020ec60767ff00d7d",
                  "sig":"5b126ac909fff2f0553e6b02d559478e95be3af80926b710e1fa086b177fb5ae2b065737d58375924653f70527695a3c1c84823acc6d83000347fbecf49eca0a"
               },
               {  
                  "version":0,
                  "millitime":"014fd1eafcd7",
                  "entryhash":"5685ab402185def2ee2639be7a119d1fbf66114ce923c585bf20920c839ed1a7",
                  "credits":1,
                  "ecpubkey":"17ef7a21d1a616d65e6b73f3c6a7ad5c49340a6c2592872020ec60767ff00d7d",
                  "sig":"0213cf5675124b211d16cb3c0c35f3cf87fa83e6970d0e7eb1feadd49bbd774878edc18066afe75b32fe525be4720326f622df0fa13afc5ad6653e6a0cf0e607"
               },
               {  
                  "number":2
               },
               {  
                  "number":3
               },
               {  
                  "number":4
               },
               {  
                  "number":5
               },
               {  
                  "number":6
               },
               {  
                  "number":7
               },
               {  
                  "number":8
               },
               {  
                  "number":9
               },
               {  
                  "number":10
               }
            ]
         }
      },
      "rawdata":"000000000000000000000000000000000000000000000000000000000000000c5eb9fea36690f9279360019d141faabef23b733030c2c6972f7870f38c22c385cd62ba005d20e9384e28a29da742b6fc3d6b12a2458dbb5d9be3e9daf743036c848b20a3c499d9ade0fcc879e0febcc9fd107c6b6411627c262e44ce19499872000007ce00000000000000000e00000000000001b1000001010300014fd1eafcba5710aeb50e7ad82b3ad469e91019bc86b360796cfd4c037168ffe9040b7dfec70117ef7a21d1a616d65e6b73f3c6a7ad5c49340a6c2592872020ec60767ff00d7d7cd6b1df3d3f9cf519a2437e799ee8acd815a2aeec7d5743295ff476632b070cb4459300a2bef1055e6fbb0c7ff5c62a9695f41be246c566051dd172816d7b060300014fd1eafcbcae8088e67f5bc7023f1b9dd84022c41067e8777e6436f73fc2384511ae6371220117ef7a21d1a616d65e6b73f3c6a7ad5c49340a6c2592872020ec60767ff00d7d5b126ac909fff2f0553e6b02d559478e95be3af80926b710e1fa086b177fb5ae2b065737d58375924653f70527695a3c1c84823acc6d83000347fbecf49eca0a0300014fd1eafcd75685ab402185def2ee2639be7a119d1fbf66114ce923c585bf20920c839ed1a70117ef7a21d1a616d65e6b73f3c6a7ad5c49340a6c2592872020ec60767ff00d7d0213cf5675124b211d16cb3c0c35f3cf87fa83e6970d0e7eb1feadd49bbd774878edc18066afe75b32fe525be4720326f622df0fa13afc5ad6653e6a0cf0e60701020103010401050106010701080109010a"
   }
}
```

Retrieve a specified entrycredit block given its merkle root key. The numbers are minute markers.

## entry-credit-rate

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, 
"method": "entry-credit-rate"}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "rate": 95369
  }
}
```

Returns the number of Factoshis \(Factoids \*10^-8\) that purchase a single Entry Credit. The minimum factoid fees are also determined by this rate, along with how complex the factoid transaction is.

## factoid-ack

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0,
"method":"factoid-ack", "params":{
"txid":"f1d9919829fa71ce18caf1bd8659cce8a06c0026d3f3fffc61054ebb25ebeaa0"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "txid":"f1d9919829fa71ce18caf1bd8659cce8a06c0026d3f3fffc61054ebb25ebeaa0",
      "transactiondate":1441138021975,
      "transactiondatestring":"2015-09-01 15:07:01",
      "blockdate":1441137600000,
      "blockdatestring":"2015-09-01 15:00:00",
      "status":"DBlockConfirmed"
   }
}
```

{% hint style="danger" %}
**Deprecated**: Please refer to [ack](https://developers.factomprotocol.org/start/factom-api-docs/factomd-api#Ack)
{% endhint %}

Factoid Acknowledgements will give the current status of a transaction. “DBlockConfirmed” is the highest level of confirmation you can obtain. This means the transaction has completed.

## factoid-balance

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": 
"factoid-balance", "params":{"address":"FA2jK2HcLnRdS94dEcU27rF3meoJfpUcZPSinpb7AwQvPRY6RL1Q"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "balance": 966582271
  }
}
```

This call returns the number of Factoshis \(Factoids \*10^-8\) that are currently available at the address specified.

## factoid-block

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": 
"factoid-block", "params": {"keymr":"1df843ee64f4b139047617a2df1007ea4470fabd097ddf87acabc39813f71480"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "fblock":{  
         "bodymr":"1118c35047d58381f363c69a2501cfab34b5dca2f1c2806530c22af8bf7b3f59",
         "prevkeymr":"694e5ea2cd1feb49ff6ecb5b77739f4885352143b389db7c7559a91b5df7eb20",
         "prevledgerkeymr":"581914c0e606a95e0df75596a75de62d828f5e215a6380a62db90025f0516d6d",
         "exchrate":120000,
         "dbheight":41915,
         "transactions":[  
            {  
               "txid":"a1dfd97c32df3f060a56436a661e2e997e2954198294c351380c1029c9029f4b",
               "blockheight":0,
               "millitimestamp":1466346000464,
               "inputs":[  

               ],
               "outputs":[  

               ],
               "outecs":[  

               ],
               "rcds":[  

               ],
               "sigblocks":[  

               ]
            },
            {  
               "txid":"6e0c010d3977e59734f4cc5f520092af7916420a2b03fb37c80d7d364261d164",
               "blockheight":0,
               "millitimestamp":1466346049257,
               "inputs":[  
                  {  
                     "amount":6990000000,
                     "address":"c825bc4e3309b97d8e0f1af0f0f0a71930a151488af886fad792e775928a7d5a",
                     "useraddress":"FA3VE46DUpZEgC1vRsZiWEcbjNMB6jbfQQ1nS66PSE7Jt5ADAQ8Z"
                  },
                  {  
                     "amount":1560000,
                     "address":"330fd717584445ac866dc2facd8b856e63bdb8b15b5ed46c0b053b2c6c5c5c3f",
                     "useraddress":"FA2MZs5wASMo9cCiKezdiQKCd8KA6Zbg2xKXKGmYEZBqon9J3ZKv"
                  }
               ],
               "outputs":[  
                  {  
                     "amount":6990000000,
                     "address":"330fd717584445ac866dc2facd8b856e63bdb8b15b5ed46c0b053b2c6c5c5c3f",
                     "useraddress":"FA2MZs5wASMo9cCiKezdiQKCd8KA6Zbg2xKXKGmYEZBqon9J3ZKv"
                  }
               ],
               "outecs":[  

               ],
               "rcds":[  
                  "0143a73f3b72b34c281f51b23e3861fb08dde5195f37ed2dd6e1dcf4df6924cdc7",
                  "012c94f2bbe49899679c54482eba49bf1d024476845e478f9cce3238f612edd761"
               ],
               "sigblocks":[  
                  {  
                     "signatures":[  
                        "3a58153da5a9a8cb000689983e2532168b22a1c29c197379a263d9e4c844c6ffb2cd2d79d9a33782e66b66cccd749b9b20fd81215d891ff7c601d625a96dcd0c"
                     ]
                  },
                  {  
                     "signatures":[  
                        "b3a9554231d940c288cabbbf056067c64c7478dc0264da1451ceeb1c1a28ab63a7ac71a2a08075f742fdfe0f80012ca3614af92c2c5f757d3aac762a0dfb2d08"
                     ]
                  }
               ]
            },
            {  
               "txid":"6d966064383b0da8df1e8db6ec319c01fcf0a119439936bc6ed86afe4769a252",
               "blockheight":0,
               "millitimestamp":1466346444191,
               "inputs":[  
                  {  
                     "amount":46535848,
                     "address":"1b81ea64ac318395390caf9acb3efbece402b03cd20aa7540f1172f2e235b656",
                     "useraddress":"FA2BCCVgPD7ZGFpYpzbWXbqtmJroL6wQFcJgPSME6coUqh9EUng9"
                  }
               ],
               "outputs":[  
                  {  
                     "amount":45095848,
                     "address":"dcb8222e77488961e3a5516de51a5831708f10ddb62b1464d1d87fc10354d01e",
                     "useraddress":"FA3eHXyQ7ibbX75pbhcAJqkaWhLYqfNptG1ifxjogDgADLgKXrcX"
                  }
               ],
               "outecs":[  

               ],
               "rcds":[  
                  "0112cb1fc2b1d5e8a82f6727a02731ced20e09ba04eaf5762ab1b8f6b0c280fbe1"
               ],
               "sigblocks":[  
                  {  
                     "signatures":[  
                        "c0ef56d70389388d70464478750cde51deabb6b1d19a4e3766937163d337e0c6ba1c9d71c7d4e6ef37efd66512463fc61ed5b93eced32d1d2c9b2bec0c232b0b"
                     ]
                  }
               ]
            }
         ],
         "chainid":"000000000000000000000000000000000000000000000000000000000000000f",
         "keymr":"1df843ee64f4b139047617a2df1007ea4470fabd097ddf87acabc39813f71480",
         "ledgerkeymr":"f6b05c36fe0a7c91f3d96f41fab2987250398350499f8369a1905357a1bb31e9"
      },
      "rawdata":"000000000000000000000000000000000000000000000000000000000000000f1118c35047d58381f363c69a2501cfab34b5dca2f1c2806530c22af8bf7b3f59694e5ea2cd1feb49ff6ecb5b77739f4885352143b389db7c7559a91b5df7eb20581914c0e606a95e0df75596a75de62d828f5e215a6380a62db90025f0516d6d000000000001d4c00000a3bb000000000300000200020155690850500000000002015569090ee90201009a858bdf00c825bc4e3309b97d8e0f1af0f0f0a71930a151488af886fad792e775928a7d5adf9b40330fd717584445ac866dc2facd8b856e63bdb8b15b5ed46c0b053b2c6c5c5c3f9a858bdf00330fd717584445ac866dc2facd8b856e63bdb8b15b5ed46c0b053b2c6c5c5c3f0143a73f3b72b34c281f51b23e3861fb08dde5195f37ed2dd6e1dcf4df6924cdc73a58153da5a9a8cb000689983e2532168b22a1c29c197379a263d9e4c844c6ffb2cd2d79d9a33782e66b66cccd749b9b20fd81215d891ff7c601d625a96dcd0c012c94f2bbe49899679c54482eba49bf1d024476845e478f9cce3238f612edd761b3a9554231d940c288cabbbf056067c64c7478dc0264da1451ceeb1c1a28ab63a7ac71a2a08075f742fdfe0f80012ca3614af92c2c5f757d3aac762a0dfb2d08000000000000020155690f159f0101009698a9281b81ea64ac318395390caf9acb3efbece402b03cd20aa7540f1172f2e235b65695c0b728dcb8222e77488961e3a5516de51a5831708f10ddb62b1464d1d87fc10354d01e0112cb1fc2b1d5e8a82f6727a02731ced20e09ba04eaf5762ab1b8f6b0c280fbe1c0ef56d70389388d70464478750cde51deabb6b1d19a4e3766937163d337e0c6ba1c9d71c7d4e6ef37efd66512463fc61ed5b93eced32d1d2c9b2bec0c232b0b000000"
   }
}
```

Retrieve a specified factoid block given its merkle root key.

## factoid-submit

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": "factoid-submit", "params":
{"transaction":"0201565d109233010100b0a0e100646f3e8750c550e4582eca5047546ffef89c13a175985e320232bacac81cc428afd7c200ce7b98bfdae90f942bc1fe88c3dd44d8f4c81f4eeb88a5602da05abc82ffdb5301718b5edd2914acc2e4677f336c1a32736e5e9bde13663e6413894f57ec272e28dc1908f98b79df30005a99df3c5caf362722e56eb0e394d20d61d34ff66c079afad1d09eee21dcd4ddaafbb65aacea4d5c1afcd086377d77172f15b3aa32250a"}}' \
 -H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "message":"Successfully submitted the transaction",
      "txid":"aa8bac391e744340140ea0d95c7b37f9cc8a58601961bd751f5adb042af6f33b"
   }
}
```

Submit a factoid transaction. The transaction hex encoded string is documented [here](https://developers.factomprotocol.org/start/factom-data-structures/user-elements#factoid-transaction)**.**

The factoid-submit API takes a specifically formatted message encoded in hex that includes signatures. If you have a factom-walletd instance running, you can construct this factoid-submit API call with [compose-transaction](https://developers.factomprotocol.org/start/factom-api-docs/factom-walletd-api#compose-transaction) which takes easier to construct arguments.

## fblock-by-height

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method":
 "fblock-by-height", "params": {"height":1}}' \
 -H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "fblock":{  
         "bodymr":"16a82932aa64e6ad45b2749f2abb871fcf3353ab9d4e163c9bd90e5bbd745b59",
         "prevkeymr":"a164ccbb77a21904edc4f2bb753aa60635fb2b60279c06ae01aa211f37541736",
         "prevledgerkeymr":"2fb170f73c3961d4218ff806dd75e6e348ca1798a5fc7a99d443fbe2ff939d99",
         "exchrate":666600,
         "dbheight":1,
         "transactions":[  
            {  
               "txid":"e321605afa458333cdded91644b0d9a21b4325bb3340b85a943974bf70aa1e99",
               "blockheight":0,
               "millitimestamp":1441137675547,
               "inputs":[  

               ],
               "outputs":[  

               ],
               "outecs":[  

               ],
               "rcds":[  

               ],
               "sigblocks":[  

               ]
            },
            {  
               "txid":"f1d9919829fa71ce18caf1bd8659cce8a06c0026d3f3fffc61054ebb25ebeaa0",
               "blockheight":0,
               "millitimestamp":1441138021975,
               "inputs":[  
                  {  
                     "amount":207999200,
                     "address":"7d4f56c528ab09da5bbf7b37b0b453f43db303730e28e9ebe02657dff431d4f7",
                     "useraddress":"FA2vGRwutdPdTHQa7kkpX3LkSgqKQ1MS2nur4UqbxqP5MGHcziWa"
                  }
               ],
               "outputs":[  

               ],
               "outecs":[  
                  {  
                     "amount":200000000,
                     "address":"17ef7a21d1a616d65e6b73f3c6a7ad5c49340a6c2592872020ec60767ff00d7d",
                     "useraddress":"EC1whAxbYYsfQoAFLuzHCsz4Qz29WePBcrrGj5MqMQ1PR43wjiBH"
                  }
               ],
               "rcds":[  
                  "01a5be79b6ada79c0af4d6b7f91234ff321f3b647ed01e02ccbbc0fe9dcc632934"
               ],
               "sigblocks":[  
                  {  
                     "signatures":[  
                        "82f22455b9756ee4b4db411a5d00e31b689c1bd1abe1d1e887cf4c52e67fc51fe4d9594c24643a91009c6ea91701b5b6df240248c2f39453162b61d71b982701"
                     ]
                  }
               ]
            }
         ],
         "chainid":"000000000000000000000000000000000000000000000000000000000000000f",
         "keymr":"aa100f203f159e4369081bb366f6816b302387ec19a4f8b9c98495d97fbe3527",
         "ledgerkeymr":"5810ed83155dfb7b6039323b8a5572cd03166a37d1c3e86d4538c99907a81757"
      },
      "rawdata":"000000000000000000000000000000000000000000000000000000000000000f16a82932aa64e6ad45b2749f2abb871fcf3353ab9d4e163c9bd90e5bbd745b59a164ccbb77a21904edc4f2bb753aa60635fb2b60279c06ae01aa211f375417362fb170f73c3961d4218ff806dd75e6e348ca1798a5fc7a99d443fbe2ff939d9900000000000a2be8000000010000000002000000c702014f8a7fcd1b00000002014f8a851657010001e397a1607d4f56c528ab09da5bbf7b37b0b453f43db303730e28e9ebe02657dff431d4f7dfaf840017ef7a21d1a616d65e6b73f3c6a7ad5c49340a6c2592872020ec60767ff00d7d01a5be79b6ada79c0af4d6b7f91234ff321f3b647ed01e02ccbbc0fe9dcc63293482f22455b9756ee4b4db411a5d00e31b689c1bd1abe1d1e887cf4c52e67fc51fe4d9594c24643a91009c6ea91701b5b6df240248c2f39453162b61d71b98270100000000000000000000"
   }
}
```

Retrieve the factoid block for any given height. These blocks contain factoid transaction information.

## heights

> Example Request

```text
curl -X POST --data-binary \
'{"jsonrpc": "2.0", "id": 0, "method": "heights"}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "directoryblockheight":72498,
      "leaderheight":72498,
      "entryblockheight":72498,
      "entryheight":72498
   }
}
```

Returns various heights that allows you to view the state of the blockchain. The heights returned provide a lot of information regarding the state of factomd, but not all are needed by most applications. The heights also indicate the most recent block, which could not be complete, and still being built. The heights mean as follows:

* directoryblockheight : The current directory block height of the local factomd node.
* leaderheight : The current block being worked on by the leaders in the network. This block is not yet complete, but all transactions submitted will go into this block \(depending on network conditions, the transaction may be delayed into the next block\)
* entryblockheight : The height at which the factomd node has all the entry blocks. Directory blocks are obtained first, entry blocks could be lagging behind the directory block when syncing.
* entryheight : The height at which the local factomd node has all the entries. If you added entries at a block height above this, they will not be able to be retrieved by the local factomd until it syncs further.

A fully synced node should show the same number for all, \(except between minute 0 and 1, when leaderheight will be 1 block ahead.\)

## multiple-ec-balances

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method":"multiple-ec-balances", 
"params":{"addresses":["EC293AbTn3VScgC2m86xTh2kFKAMNnkgoLdXgywpPa66Jacom5ya","EC3ExcVhmGRJmavCf1LCMu8YiHCyU2CWVh5DmXRz6jfPHMbzJSCz"]}}'
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "currentheight": 1674,
    "lastsavedheight": 1673,
    "balances": [
        {
          "ack": 5,
          "saved": 5,
          "err": ""
        },
        {
            "ack": 14,
            "saved": 14,
            "err": ""
        }
    ]
  }
}
```

The _multiple-ec-balances_ API is used to query the acknowledged and saved balances for a list of entry credit addresses.

* `currentheight` is the current height that factomd was loading.
* `lastsavedheight` is the height last saved to the database.
* In `balances` it returns `"ack"`, `"saved"` and `"err"`.
  * `ack` is the balance after processing any in-flight transactions known to the Factom node responding to the API call
  * `saved` is the last saved to the database
  * `err` is just used to display any error that might have happened during the request. If it is `""` that means there was no error.
* If the syntax of the parameters is off e.g. missing a quote, a comma, or a square bracket, it will return: {“jsonrpc”:“2.0”,“id”:null,“error”:{“code”:-32600,“message”:“Invalid Request”}}
* If the parameters are labeled incorrectly the call will return: “{“code”:-32602,“message”:“Invalid params”,“data”:“ERROR! Invalid params passed in, expected 'addresses’”}”
* If factomd is not loaded up all the way to the last saved block it will return: {“currentheight”:0,“lastsavedheight”:0,“balances”:\[{“ack”:0,“saved”:0,“err”:“Not fully booted”}\]}
* If the list of addresses contains an incorrectly formatted address the call will return: {“currentheight”:0,“lastsavedheight”:0,“balances”:\[{“ack”:0,“saved”:0,“err”:“Error decoding address”}\]}
* If an address in the list is valid but has never been part of a transaction the call will return: “balances”:\[{“ack”:0,“saved”:0,“err”:“Address has not had a transaction”}\]“

{% hint style="warning" %}
**Referring to the example request:** This Example is for simulation, these addresses may not work or have the same value for mainnet or testnet.
{% endhint %}

## multiple-fct-balances

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method":"multiple-fct-balances", 
"params":{"addresses":["FA3uMAv9htC5y5u3ayzxNQKZNDpgrJVf49kJSKdVNxcYoNBbSLXc","FA3umgJaXdHjpSQyBUPC2uMFuoW9nM5Ymm8Sa2f2VKGSqsyx79nf"]}}'
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
    "jsonrpc": "2.0",
    "id": 0,
    "result": {
        "currentheight": 1678,
        "lastsavedheight": 1677,
        "balances": [
            {
                "ack": 69896000,
                "saved": 69896000,
                "err": ""
            },
            {
                "ack": 245272000,
                "saved": 245272000,
                "err": ""
            }
        ]
    }
}
```

The _multiple-fct-balances_ API is used to query the acknowledged and saved balances in factoshis \(a factoshi is 10^8 factoids\) not factoids\(FCT\) for a list of FCT addresses.

* `currentheight` is the current height that factomd was loading.
* `lastsavedheight` is the height last saved to the database.
* In `balances` it returns `"ack"`, `"saved"` and `"err"`.
  * `ack` is the balance after processing any in-flight transactions known to the Factom node responding to the API call
  * `saved` is the last saved to the database
  * `err` is just used to display any error that might have happened during the request. If it is `""` that means there was no error.
* If the syntax of the parameters is off e.g. missing a quote, a comma, or a square bracket, it will return: `{"jsonrpc”:“2.0”,“id”:null,“error”:{“code”:-32600,“message”:“Invalid Request”}}`
* If the parameters are labeled incorrectly the call will return: `{“code”:-32602,“message”:“Invalid params”,“data”:“ERROR! Invalid params passed in, expected 'addresses’”}`
* If factomd is not loaded up all the way to the last saved block it will return: `{“currentheight”:0,“lastsavedheight”:0,“balances”:[{“ack”:0,“saved”:0,“err”:“Not fully booted”}]}`
* If the list of addresses contains an incorrectly formatted address the call will return: `{“currentheight”:0,“lastsavedheight”:0,“balances”:[{“ack”:0,“saved”:0,“err”:“Error decoding address”}]}`
* If an address in the list is valid but has never been part of a transaction it will return: `“balances”:[{“ack”:0,“saved”:0,“err”:“Address has not had a transaction”}]`“

{% hint style="warning" %}
**Referring to the example request:** This Example is for simulation, these addresses may not work or have the same value for mainnet or testnet.
{% endhint %}

## pending-entries

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method":
"pending-entries", "params": {}}' -H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
 "jsonrpc":"2.0",
 "id":0,
 "result":[  
    {  
       "entryhash":"dde3f69025780f58da583b6961cf17291004f733fb2fa1a69738e0a7768387e4",
       "chainid":"5c7a44a37870ca729d03820339379955781a863bc461545a9df06bbc15110bdb",
       "status":"TransactionACK"
    },
    {  
       "entryhash":"45da41a07c6157839a735147a555ba4b009b72a9a7fe126c0d418413743f1683",
       "chainid":"f6df2b40bf16be03deddc169a6429804a67a761ed5f2d5499f7e56a7202f854b",
       "status":"TransactionACK"
    }
 ]
}
```

Returns an array of the entries that have been submitted but have not been recorded into the blockchain.

## pending-transactions

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": "pending-transactions", "params":
{"address":"FA2jK2HcLnRdS94dEcU27rF3meoJfpUcZPSinpb7AwQvPRY6RL1Q"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
 "jsonrpc":"2.0",
 "id":0,
 "result":[
  {
    "transactionid": "337a32712f14c5df0b57a64bd6c321a043081688ecd4f33fd8319470da2256b1",
    "status": "TransactionACK",
    "inputs":[
      {
        "amount":100012000,
        "address":"646f3e8750c550e4582eca5047546ffef89c13a175985e320232bacac81cc428",
        "useraddress":"FA2jK2HcLnRdS94dEcU27rF3meoJfpUcZPSinpb7AwQvPRY6RL1Q"
      }
    ],
    "outputs":[
      {
        "amount":100000000,
        "address":"27c0a471c6c9b6da3fce0af7b1119ac2cbc68723657a9e2860d83c6776bbd6ff",
        "useraddress":"FA2GaytuGnYuphP3uz3V9HuN9ECLAHRy73Hu7xhbydatJFx414j9"
      }
    ],
    "ecoutputs":[],
    "fees":12000
  }
 ]
}
```

Returns an array of factoid transactions that have not yet been recorded in the blockchain, but are known to the system.

## properties

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": 
"properties"}' -H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "factomdversion": "0.4.0.0",
    "factomdapiversion": "2.0"
  }
}
```

Retrieve current properties of the Factom system, including the software and the API versions.

## raw-data

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": 
"raw-data", "params":{"hash":"0ae2ab2cf543eed52a13a5a405bded712444cc8f8b6724a00602e1c8550a4ec2"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "data":"00f9164cd66af9d5773b4523a510b5eefb9a5e626480feeb6671ef2d17510ca30000900040e5a5fa49cf19015475fb1b5c8e4269b4586f04ec98e0bb60dd32c214e201c9cf97febe09afca45549db35a9f61ca930855ce6d753853fb270490bcafb65e43020015466163746f6d20496e63204261736520436861696e0014466163746f6d697a652045766572797468696e67000c42656c6c2041766572616765001142656c6c204176657261676520446174617b22736f75726365223a227777772e62656c6c617665726167652e636f6d222c2271756f74655f64617465223a22323031372d30312d31392031373a33333a3031222c2264617461223a5b7b22474f4c44223a22313231332e3134227d2c7b2253494c564552223a2231372e3139227d5d7d"
   }
}
```

Retrieve an entry or transaction in raw format, the data is a hex encoded string.

## receipt

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": 
"receipt", "params":{"hash":"0ae2ab2cf543eed52a13a5a405bded712444cc8f8b6724a00602e1c8550a4ec2"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "receipt":{  
         "entry":{  
            "entryhash":"0ae2ab2cf543eed52a13a5a405bded712444cc8f8b6724a00602e1c8550a4ec2"
         },
         "merklebranch":[  
            {  
               "left":"0ae2ab2cf543eed52a13a5a405bded712444cc8f8b6724a00602e1c8550a4ec2",
               "right":"0000000000000000000000000000000000000000000000000000000000000003",
               "top":"d2f3554d8394dc59932383de36dd742449d1c25ef1d21cdab07c4b9f044356af"
            },
            {  
               "left":"6adbc7ba1758f2850c92e4a09dea8ba9c6d844cdd4d596db2421b0f843c3e530",
               "right":"d2f3554d8394dc59932383de36dd742449d1c25ef1d21cdab07c4b9f044356af",
               "top":"041c3fed14469a3d0f1a022e3d5321583065e691edb9223605c86766ff881883"
            },
            {  
               "left":"0caff62ea5b5aa015c706add7b2463a5be07e1f0537617f553558090f23c7f56",
               "right":"041c3fed14469a3d0f1a022e3d5321583065e691edb9223605c86766ff881883",
               "top":"434ea8fc39fa1686035b4c993ef2c62ee67f67332fc59da1d373f33140d7c2b5"
            },
            {  
               "left":"434ea8fc39fa1686035b4c993ef2c62ee67f67332fc59da1d373f33140d7c2b5",
               "right":"1deef358d7e02adfe353f8137d1535e34f112d7a3b9fbf06b5c64dee5e4b2042",
               "top":"9482ce541c7198e21947c3043f12dddda6184bb4928f01ae2c3f885b2ed3d5bb"
            },
            {  
               "left":"9482ce541c7198e21947c3043f12dddda6184bb4928f01ae2c3f885b2ed3d5bb",
               "right":"3d93579d9f5b22387cb46c1ad280da9ba9e1fe753972ef08d9e9f1c938a765c0",
               "top":"6c83c08ee21925ce8c958a3b0b5da21bb04d66465f6f1366783f637bcdbb9d47"
            },
            {  
               "left":"9f5d7d533e66e920a68d9fd63d0cee1517fb6de713946968453cfd2cb3081dfd",
               "right":"6c83c08ee21925ce8c958a3b0b5da21bb04d66465f6f1366783f637bcdbb9d47",
               "top":"e1d971bedc93c03963d7808b570744d3853e8c521b21a5e3985f2651b85b5362"
            },
            {  
               "left":"e1d971bedc93c03963d7808b570744d3853e8c521b21a5e3985f2651b85b5362",
               "right":"f62391cfbdb158a4b20f301e2800f0b3933ba23417bf1aeb630ff968765ddb30",
               "top":"5858cdfc5418ed2d6d2f023f91786d6169e420d7ac3f5f3fc4d5121cd7848509"
            },
            {  
               "left":"3000e97b6045299a108c04b656a9965d2ac5e7a9ae76718afb6009c4098d320b",
               "right":"5858cdfc5418ed2d6d2f023f91786d6169e420d7ac3f5f3fc4d5121cd7848509",
               "top":"4358041d6773351dd0a42a8d16778c6544b1196a03c6c41645340cd076a29b6b"
            }
         ],
         "entryblockkeymr":"041c3fed14469a3d0f1a022e3d5321583065e691edb9223605c86766ff881883",
         "directoryblockkeymr":"4358041d6773351dd0a42a8d16778c6544b1196a03c6c41645340cd076a29b6b",
         "bitcointransactionhash":"469a96c847cf8bf1f325b6eec850f46488ed671930d62b54ed186a8031477a7d",
         "bitcoinblockhash":"000000000000000001edf3adf719dfc1263661d2c4b0ed779d004a2cbb7cca32"
      }
   }
}
```

Retrieve a receipt providing cryptographically verifiable proof that information was recorded in the factom blockchain and that this was subsequently anchored in the bitcoin blockchain.

## reveal-chain

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": "reveal-chain", "params":
{"entry":
"007E18CCC911F057FB111C7570778F6FDC51E189F35A6E6DA683EC2A264443531F000E0005746573745A0005746573745A48656C6C6F20466163746F6D21"}}' \
 -H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "message": "Entry Reveal Success",
    "entryhash": "f5c956749fc3eba4acc60fd485fb100e601070a44fcce54ff358d60669854734",
    "chainid": "  f9164cd66af9d5773b4523a510b5eefb9a5e626480feeb6671ef2d17510ca300"
  }
}
```

Reveal the First Entry in a Chain to factomd after the Commit to complete the Chain creation. The reveal-chain hex encoded string is documented [here](https://developers.factomprotocol.org/start/factom-data-structures/user-elements#entry).

The reveal-chain API takes a specifically formatted message encoded in hex that includes signatures. If you have a factom-walletd instance running, you can construct this reveal-chain API call with [compose-chain](https://developers.factomprotocol.org/start/factom-api-docs/factom-walletd-api#compose-chain) which takes easier to construct arguments.

The compose-chain api call has two api calls in its response: [commit-chain](https://developers.factomprotocol.org/start/factom-api-docs/factomd-api#commit-chain) and [reveal-chain](https://developers.factomprotocol.org/start/factom-api-docs/factomd-api#reveal-chain). To successfully create a chain, the reveal-chain must be called after the commit-chain.

## reveal-entry

> Example Request

```text
curl -X POST --data '{"jsonrpc": "2.0", "id": 0, "method": "reveal-entry", "params":
{"entry":"007E18CCC911F057FB111C7570778F6FDC51E189F35A6E6DA683EC2A264443531F000E0005746573745A0005746573745A48656C6C6F20466163746F6D21"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "message": "Entry Reveal Success",
    "entryhash": "f5c956749fc3eba4acc60fd485fb100e601070a44fcce54ff358d60669854734",
    "chainid": "  f9164cd66af9d5773b4523a510b5eefb9a5e626480feeb6671ef2d17510ca300"
  }
}
```

Reveal an Entry to factomd after the Commit to complete the Entry creation. The reveal-entry hex encoded string is documented [here](https://developers.factomprotocol.org/start/factom-data-structures/user-elements#entry).

The reveal-entry API takes a specifically formatted message encoded in hex that includes signatures. If you have a factom-walletd instance running, you can construct this reveal-entry API call with [compose-entry](https://developers.factomprotocol.org/start/factom-api-docs/factom-walletd-api#compose-entry) which takes easier to construct arguments.

The compose-entry api call has two api calls in it’s response: [commit-entry](https://developers.factomprotocol.org/start/factom-api-docs/factomd-api#commit-entry) and [reveal-entry](https://developers.factomprotocol.org/start/factom-api-docs/factomd-api#reveal-entry). To successfully create an entry, the reveal-entry must be called after the commit-entry.

## send-raw-message

Send a raw hex encoded binary message to the Factom network. This is mostly just for debugging and testing.

### To Check Commit Chain Example

> Example Commit Chain Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": "send-raw-message", "params":
{"message":"00015fc6dbeaccfab82063af4a2890f89c243a9a3db2cce041e9352a1df32731d302917c38b229985e890c7d0d4c76e84a283011ba165ccee3524dd91fb417c2550c6d1c42d3bd23af5f7c05a89c0097eed7378c60b8bcc89a284094a81da85fb8faab7b2972470cb64dfb9c542844a0724222d53b86c85baa6fe49cc01fb5e8d26e08ce4690b0e3933bf1f6c5c15b28a33eb504f87c07f7bb51691b90cb3326d62b4b97802db3c6dccc9b0108f2c06cac0b7968e9f1f6aabb126f9aa58bc8eae21f2383729cb703"}}' -H 'content-type:text/plain;' htt
p://localhost:8088/v2
```

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "method": "send-raw-message",
  "params": {
    "message": "00015fc6dbeaccfab82063af4a2890f89c243a9a3db2cce041e9352a1df32731d302917c38b229985e890c7d0d4c76e84a283011ba165ccee3524dd91fb417c2550c6d1c42d3bd23af5f7c05a89c0097eed7378c60b8bcc89a284094a81da85fb8faab7b2972470cb64dfb9c542844a0724222d53b86c85baa6fe49cc01fb5e8d26e08ce4690b0e3933bf1f6c5c15b28a33eb504f87c07f7bb51691b90cb3326d62b4b97802db3c6dccc9b0108f2c06cac0b7968e9f1f6aabb126f9aa58bc8eae21f2383729cb703"
  }
}
```

> Example Commit Chain Response

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "message": "Successfully sent the message"
  }
}
```

Entry Hash : 23af5f7c05a89c0097eed7378c60b8bcc89a284094a81da85fb8faab7b297247

### To Check Reveal Chain Example

> Example Reveal Chain Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": "send-raw-message", "params":
{"message":"00d05f245ae96f954dbb4da9c5dc82eea40df0a11823efa47733e374237d626146000e0005614c4855720005487554754e68644351744c636152614972556e517062765863554e6b704d4551446c5573737163776a654371714a5a5a68554654614745554f6b7341747843634875524e665a4549706e4f736d49534c4a73656f6e6673594c41634e5964767552454b4f595242696d6d6643705666655242726847704a4a5a6c61446a43776248446a52644f77674a75494648766d5343626b524158576d4343616842456b7564665a4f6f5565794f69686e5a4b5a52625653515357695350557668597759514579526467554376706d724172796a71494566736173626d4161636b496145485a5267764643664f44434849576769544e764b67726c42585854684b444947697350716556664655507a565171477474617558456b7a47747973537a55656c46624a445957794b4274675a746f48795341715274465649666556644468436562796248584e4b714e627a6c7159667264424e74446c4857545666694b4348524556486d785a535057575579734551536f77775275745952594b76514b59477147756d625a6f485a736b496f6e74556b514a7463484b6f53626553466c666176546e446172784c714a5846586f657945446376696c5157596d56796859614544734b535058596a6346736c50616b6d79635a456e4c4b50424a75627155554246636972494973595155434d59564673517579567a5161535a57737161414d51754c674b6b6b5a4244486f53555453456e6356595a47536e4c676a666c6d57754953444d4756794f444a4f4975724d4f6f4f72664672477a6a63664c4d5a6d53654a55615a5a646241644c496548644e75427a57756b486d566c5a4b6750486e4e46655751784c71594b50516b585554655856765170766e526e43624377496c7853507545487064414a414b637a7956456b74434d516d6166734b7a695444566a6e784c7741486a7059735562466472534465657644536e6c6b716a52424a626742536f7a635073747843656a754152594767494965566a5452566c545669735a4d444f4662727752707566484d48496f544465705257566d75717446646b4d434c41724850666366614d6f6a6d75574b4d4b747263456645755a53656d506d745442654d48476667744257525174446473414b5775535377444643576965537375677a664c54456679715463454b6e494276794462434f696755654f527943716244714f5843646c726b565a6b455a7952456f6c54586d76507877416d5a6144776b6a42694b454a6a7265765153535967514a48697871714d57546667627748794251476841756843754e746442787849706a5a6850504148714a4c474b794c7a6858726a4e7852594c43775842504851686a486f5962436843576f61415263436869464e59766c6b797862684f5a727869655868586143426c546379627967717373726a4b"}}' -H 'content-type:text/plain;' http://localhost:8088/v2
```

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "method": "send-raw-message",
  "params": {
    "message": "00d05f245ae96f954dbb4da9c5dc82eea40df0a11823efa47733e374237d626146000e0005614c4855720005487554754e68644351744c636152614972556e517062765863554e6b704d4551446c5573737163776a654371714a5a5a68554654614745554f6b7341747843634875524e665a4549706e4f736d49534c4a73656f6e6673594c41634e5964767552454b4f595242696d6d6643705666655242726847704a4a5a6c61446a43776248446a52644f77674a75494648766d5343626b524158576d4343616842456b7564665a4f6f5565794f69686e5a4b5a52625653515357695350557668597759514579526467554376706d724172796a71494566736173626d4161636b496145485a5267764643664f44434849576769544e764b67726c42585854684b444947697350716556664655507a565171477474617558456b7a47747973537a55656c46624a445957794b4274675a746f48795341715274465649666556644468436562796248584e4b714e627a6c7159667264424e74446c4857545666694b4348524556486d785a535057575579734551536f77775275745952594b76514b59477147756d625a6f485a736b496f6e74556b514a7463484b6f53626553466c666176546e446172784c714a5846586f657945446376696c5157596d56796859614544734b535058596a6346736c50616b6d79635a456e4c4b50424a75627155554246636972494973595155434d59564673517579567a5161535a57737161414d51754c674b6b6b5a4244486f53555453456e6356595a47536e4c676a666c6d57754953444d4756794f444a4f4975724d4f6f4f72664672477a6a63664c4d5a6d53654a55615a5a646241644c496548644e75427a57756b486d566c5a4b6750486e4e46655751784c71594b50516b585554655856765170766e526e43624377496c7853507545487064414a414b637a7956456b74434d516d6166734b7a695444566a6e784c7741486a7059735562466472534465657644536e6c6b716a52424a626742536f7a635073747843656a754152594767494965566a5452566c545669735a4d444f4662727752707566484d48496f544465705257566d75717446646b4d434c41724850666366614d6f6a6d75574b4d4b747263456645755a53656d506d745442654d48476667744257525174446473414b5775535377444643576965537375677a664c54456679715463454b6e494276794462434f696755654f527943716244714f5843646c726b565a6b455a7952456f6c54586d76507877416d5a6144776b6a42694b454a6a7265765153535967514a48697871714d57546667627748794251476841756843754e746442787849706a5a6850504148714a4c474b794c7a6858726a4e7852594c43775842504851686a486f5962436843576f61415263436869464e59766c6b797862684f5a727869655868586143426c546379627967717373726a4b"
  }
}
```

> Example Reveal Chain Response

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "message": "Successfully sent the message"
  }
}
```

Entry Hash : 23af5f7c05a89c0097eed7378c60b8bcc89a284094a81da85fb8faab7b297247

Chain ID : d05f245ae96f954dbb4da9c5dc82eea40df0a11823efa47733e374237d626146

## transaction

> Example Request

```text
curl -X POST --data-binary '{"jsonrpc": "2.0", "id": 0, "method": "transaction", "params":
{"hash":"64251aa63e011f803c883acf2342d784b405afa59e24d9c5506c84f6c91bf18b"}}' \
-H 'content-type:text/plain;' http://localhost:8088/v2
```

> Example Response

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "factoidtransaction":{  
         "millitimestamp":1443537161594,
         "inputs":[  
            {  
               "amount":1000000000,
               "address":"ab87d1b89117aba0e6b131d1ee42f99c6e806b76ca68c0823fb65c80d694b125",
               "useraddress":""
            }
         ],
         "outputs":[  
            {  
               "amount":992000800,
               "address":"648f374d3de5d1541642c167a34d0f7b7d92fd2dab4d32f313776fa5a2b73a98",
               "useraddress":""
            }
         ],
         "outecs":[  
         ],
         "rcds":[  
            "10560cc304eb0a3b0540bc387930d2a7b2373270cfbd8448bc68a867cefb9f74"
         ],
         "sigblocks":[  
            {  
               "signatures":[  
                  "d68d5ce3bee5e69f113d643df1f6ba0dd476ada40633751537d5b840e2be811d4734ef9f679966fa86b1777c8a387986b4e21987174f9df808c2081be2c04a08"
               ]
            }
         ],
         "blockheight":0
      },
      "includedintransactionblock":"786dd9b5d3c3b2d9c40c538fa99277fe10c16cda32b5642d63bd7f60804aa11d",
      "includedindirectoryblock":"d6b4b0988a3a777a88cebf4e6c34ccc16363f48560cd81c446da73461c4e965c",
      "includedindirectoryblockheight":3999
   }
}
```

```text
{  
   "jsonrpc":"2.0",
   "id":0,
   "result":{  
      "entry":{
         "version":0,
         "chainid":"f920ae8d6a1d8e5e0978406ff801cc598a8d29d913dcf63101d9cbb709e3e885",
         "extids":[
            "437a68615a",
            "7747575a70" 
         ],       "content":"4e69586c4d596463765579774859556a49776c4c7a4d6a5a6f7672726b4a4847634d4a42777952764d79546d716f68776f455773795268795261786144504b4a534c4b626a4e4654477156794d584978524e70576861664b444f4f4c696a68757a636a656f7a645165416b527857464d50576f52716f704e5456687a6465757357547171724a4c58735a4353655a666b42476a576272455970524b70756f517a63546b5565514a6756684d4843696c78675a536655506a70774a7a45535a6d797973686161695761756e6973526c6357756e714454556d614c794462794c42445343586a6578686a434a5a7265646e6d7a4c6e4d595a6c47717a4b53696671547770485254714651464a6b71617179484c6b6e614966757556594b57696a644861706d6d55417a6b4c444c42414f7752797676436c704471676b6865734e7a4f4c4c65796f7a6f4f7a7176616c644b76666d61445449497169564a704c47497072766d6b6c497150517953684e4b5a6879796877664650764146424f4949554541466b694843594c5a4e6c7368784d736b7243627257586253424e4a4742504c7a475643586c74477654436c4e514a53486a517544644f68524b4b6d735a564f76507571464979615465574b7a717278587474676b79564f4f415a7769754169564466724a46524f5a61727a4f557173627163484172547646634a52714f7a6758484c7573676e6d6450716857537652464753616f685676506e65596b7563626148797750694e517544705a4177794e7750575759796a6d6e6b78764d4759737259594341685a6171584d6e6e6661634e7177797046654659554b53747545564b524c7878686454495368584d73766a436f7159666d4d756e70546a516a6143485271504b434b537570547146466c7a42585579737a4473687552786f5647596b7161646e435475577944796e584b796b446c525a4954666e484757654c7148536e6a51574e5a4864674b79474c6c756b5469744d54615866716241667579496a4f4f6b667465414156655a4e7054427756484f6a6e54617a72714d75504466586b747344436b7a477843497a756641766e4457706d426f766b54586e4d797665654c6654547851686674686b616d6f64594b466765546a6c435a5876785a524e4c445242666e6d41634b496d6943625a654c796d584b556c56715a556369545076685a4372517977674247584b416b764b57466b474d5977714f556a4a72744b6b754457474765444a795a6a737376445857675476436b654d56626d4d764d43424351594e544a5a72774f5554417479756d786b58444d486e7171774466594a717a4c6672744b6b495a4e72447249594550724f61636c6875496755754c57574f4a6457524b6f4f427a4b6967714a4f486846666a6363686e45646d426149"},"includedintransactionblock":"73a5dc5a5c06eaeeebf2db8a57eac10b977fc5fbf47e8325e3ad0485bcfec7c6", 
         "includedindirectoryblock":"b50de1d4b6a923c3c7051bf04af2b1a7fa74d5992dea9dac816fb6f783b40539", 
         "includedindirectoryblockheight":64574
      }
   }
}
```

Retrieve details of a factoid transaction using a transaction’s hash \(or corresponding transaction id\).

Note that information regarding the

* directory block height
* directory block keymr
* transaction block keymr

are also included.

The "_blockheight_” parameter in the response will always be 0 when using this call, refer to “_includedindirectoryblockheight_” if you need the height.

{% hint style="info" %}
This call will also accept an entry hash as input, in which case the returned data concerns the entry. The returned fields and their format are shown in the 2nd Example Response at right.

If the input hash is non-existent, the returned fields will be as follows:

* `“includedintransactionblock”:“”`
* `“includedindirectoryblock”:“”`
* `“includedindirectoryblockheight”:-1`
{% endhint %}

## Errors

There are various errors you can get back from the API. Here we will detail what they mean, and sometimes how to handle them.

Error types:

* Parse error
* Invalid Request
* Invalid params
* Internal error
* Method not found
* Repeated Commit

### Parse Error

> Parse Error

```text
{
  "jsonrpc": "2.0",
  "id": null,
  "error": {
    "code": -32700,
    "message": "Parse error"
  }
}
```

**What does this mean?**  
The server had an error parsing the JSON

**What to do?**  
Check the JSON you provided and ensure it is valid.

### Invalid Request

> Invalid Request

```text
{
  "jsonrpc": "2.0",
  "id": null,
  "error": {
    "code": -32600,
    "message": "Invalid Request"
  }
}
```

**What does this mean?**  
The JSON sent is not a valid Request object.

**What to do?**  
Ensure that your request matches our JSON-RPC standard.

### Invalid params

> Invalid params

```text
{
  "jsonrpc": "2.0",
  "id": null,
  "error": {
    "code": -32602,
    "message": "Invalid params"
  }
}
```

**What does this mean?**  
The params sent do not match the expected parameters

**What to do?**  
Ensure that your parameters are correct for the API endpoint you are trying to call. Also, ensure that you have all the required fields.

### Internal error

> Internal error

```text
{
  "jsonrpc": "2.0",
  "id": null,
  "error": {
    "code": -32603,
    "message": "Internal error"
  }
}
```

**What does this mean?**  
There was an internal error when processing your request.

**What to do?**  
This error is hard to fix from the clientside, many times it is related to database lookups.

### Method not found

> Method not found

```text
{
  "jsonrpc": "2.0",
  "id": 0,
  "error": {
    "code": -32601,
    "message": "Method not found"
  }
}
```

**What does this mean?**  
If you provide a method that is not a supported API method, this error will be returned.

**What to do?**  
Check the method you provided for a typo, and ensure that your URL you provide is correct \(factomd API call to factomd, wallet API call to factom-walletd\).

### Repeated Commit

> Repeated Commit

```text
{  
   "jsonrpc":"2.0",
   "id":7,
   "error":{  
      "code":-32011,
      "message":"Repeated Commit",
      "data":{  
         "entryhash":"018a03d4afb83347a546e6bf15c32ff7087f584bbb7248a0ab9b9bda92f12338",
         "info":"A commit with equal or greater payment already exists"
      }
   }
}
```

**What does this mean?**  
This occurs if you send a commit to an entry that has already been paid for. If your commit is paying more than the current one, yours will go through.

**What to do?**  
If this error occurs, just send the reveal, as the entry has already been paid for.

In Factom every entry is paid for by the commit, and the data is given by the reveal. So if the commit is already paid for, you do not need to send another. This prevents you from double spending for an entry. Now if your entry requires more entry credits than the existing commit, you must send a new commit with more funds, and that will override the existing commit \(but both commits will spend, so get it right the first time\).

If you wish to find the existing commit, you can use the [ack](https://developers.factomprotocol.org/start/factom-api-docs/factomd-api#Ack) call, using the entryhash and the chainid.

