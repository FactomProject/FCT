---
description: Command reference for the Factom CLI
---
# Factom CLI Commands

```text
factom-cli [OPTIONS] SUBCOMMAND [OPTIONS]
```
<i>factom-cli</i> is a command line interface program for interacting with <i>factomd</i>  and <i>factom-walletd</i>.

## Flags

### -f

```text
$ echo hello | factom-cli addchain -f -n moe -n larry \
 EC2DKSYyRcNWf7RS963VFYgMExoHRYLHVeCfQ9PGPmNzwexample
```

The f flag, used with the `addchain`, `addentry`, `buyec`, `composechain`, `composeentry`, `sendfct`, `sendtx`, and `signtx` subcommands, tells factom-cli to continue on with processing without waiting for acknowledgment of the success of the subcommand to be generated. This can be useful for scripts where you can execute a long string of subcommands in a fraction of the time it would take if you had to wait for an acknowledgment of each command individually.

### -q

```text
$ echo goodbye | factom-cli addentry -q -n moe -n larry -e curly \
 EC2DKSYyRcNWf7RS963VFYgMExoHRYLHVeCfQ9PGPmNzwexample
```

The q flag specifies "quiet" execution of the `addchain`, `addentry`, `addtxecoutput`, `addtxfee`,
`addtxinput`, `addtxoutput`, `buyec`, `newtx`, `sendfct`, `sendtx`, `signtx`, and `subtxfee` subcommands. This means that no output will be returned back to the user. This again can be useful for scripts where there is no need for feedback from factom-cli.

### -e -x

```text
$ factom-cli addentry [-fq] [-n NAME1 -h HEXNAME2 ...|-c CHAINID] \
[-e EXTID1 -e EXTID2 -x HEXEXTID ...] [-CET] ECADDRESS <STDIN>
```

The `addentry` subcommands support the `-e` and `-x` flags for adding external ids to the Entries they create. Multiple `-e` and `-x` flags may be used, and -e and -x may be used in combination. `-e` string will encode the string as binary and set it as the next external id. `-x` hexstring will decode the hexstring into binary and set it as the next external id.

### -n -h

```text
$ factom-cli get chainhead -n test -h 3031
```

The `get firstentry`, `get chainhead`, and `get allentries` subcommands support the `-n` and `-h` flags for using Chain Names as an alternative for providing a ChainID. The Chain Name is the combination of External IDs on the first Entry in the Chain.

### -r

```text
$ factom-cli sendfct -r $my_factoid_address factom.michaeljbeam.me
```

The r flag tells factom-cli to try and resolve a public Factoid or Entry Credit Address from a DNS name registered through Netki.

### -C

```text
$ echo hello | factom-cli addchain -n moe -n larry -C \
 EC2DKSYyRcNWf7RS963VFYgMExoHRYLHVeCfQ9PGPmNzwexample
```

The C flag, used with the `addchain` subcommand, limits the subcommand output to a single field, the ChainID with no headers. This means that a variable can be assigned to the value of the executed command directly.

### -E

```text
$ echo goodbye | factom-cli addentry -n moe -n larry -e curly -E \
 EC2DKSYyRcNWf7RS963VFYgMExoHRYLHVeCfQ9PGPmNzwexample
```

The E flag, used with the `addchain` and `addentry` subcommands, limits the subcommand output to a single field, the Entry Hash with no headers. This means that a variable can be assigned to the value of the executed command directly.

### -T

```text
$ factom-cli get pendingtransactions -T
```

The T flag, used with the `addchain`, `addentry`, `buyec`, `get pendingtransactions`, `listtxs address`, `listtxs` subcommands, limits the subcommand output to a single field, the Entry Hash with no headers. This means that a variable can be assigned to the value of the executed command directly.

## Commands

### addchain

```text
factom-cli addchain [-fq] [-n NAME1 -n NAME2 -h HEXNAME3 ] [-CET] \
ECADDRESS <STDIN>
```

Create a new Factom Chain. Read data for the First Entry from stdin. Use the Entry Credits from the specified address.
 
Optional output flags will display only specific information used for scripting
 
 **-C**	display only the ChainID
 
 **-E**	display only the Entry Hash

 **-T**	display only the Transaction ID

 
### addentry

```text
factom-cli addentry [-fq] [-n NAME1 -h HEXNAME2 ...|-c CHAINID] \
 [-e EXTID1 -e EXTID2 -x HEXEXTID ...] [-CET] ECADDRESS <STDIN>
```
Create a new Factom Entry. Read data for the Entry from stdin. Use the Entry Credits from the specified address.

Optional output flags will display only specific information used for scripting
 
 **-C**	display only the ChainID
 
 **-E**	display only the Entry Hash

 **-T**	display only the Transaction ID

### addtxecoutput

```text
factom-cli addtxecoutput [-rq] TXNAME ADDRESS AMOUNT
```
Add an Entry Credit output to a transaction in the wallet. `-r` Netki DNS resolve. `-q` quiet.

### addtxfee

```text
factom-cli addtxfee [-q] TXNAME ADDRESS
```
Add the transaction fee to an input of a transaction in the wallet. `-q` quiet.

### addtxinput

```text
factom-cli addtxinput [-q] TXNAME ADDRESS AMOUNT
```
Add a Factoid input to a transaction in the wallet. `-q` quiet.

### addtxoutput

```text
factom-cli addtxoutput [-rq] TXNAME ADDRESS AMOUNT
```
Add a Factoid output to a transaction in the wallet. `-r` Netki DNS resolve. `-q` quiet.

### backupwallet

```text
factom-cli backupwallet
```
Backup the running wallet

### balance

```text
factom-cli balance [-r] ADDRESS
```
If this is an EC Address, returns the number of Entry Credits. If this is a Factoid Address, returns the Factoid balance.

### balancetotals

```text
factom-cli balancetotals [-FS -FA -ES -EA]
```
This is the total number of Factoids and Entry Credits in the wallet

Optional output flags will display only specific information used for scripting

**-EA**	Display only the ackEC value

**-ES** Display only the savedEC value

**-FA** Display only the ackFCT value

**-FS** Display only the savedFCT value

### buyec

```text
factom-cli buyec [-fqrT] FCTADDRESS ECADDRESS ECAMOUNT
```
Buy ECAMOUNT number of entry credits. -f force. -q quiet. `-r` Netki DNS resolve. `-T` TxID.

### composechain

```text
$ factom-cli composechain [-f] [-n NAME1 -n NAME2 -h HEXNAME3 ] \
ECADDRESS <STDIN>
```
Create API calls to create a new Factom Chain. Read data for the First Entry from stdin. Use the Entry Credits from the specified address.

### composeentry

```text
$ factom-cli composeentry [-f] [-n NAME1 -h HEXNAME2 ...|-c CHAINID] \
[-e EXTID1 -e EXTID2 -x HEXEXTID ...] ECADDRESS <STDIN>
```
Create API calls to create a new Factom Entry. Read data for the Entry from stdin. Use the Entry Credits from the specified address.

### composetx

```text
$ factom-cli composetx TXNAME
```
Compose a wallet transaction into a JSON RPC object

### diagnostics
```text
$ factom-cli diagnostics [server|network|sync|election|authset]
```
Get diagnostic information about the Factom network

### diagnostics authset
```text
$ factom-cli diagnostics authset
```
Get diagnostic information about the Factom authorized servers

### diagnostics election
```text
$ factom-cli diagnostics election {-PVFIR]
```
Get diagnostic information about the Factom network election process

Optional output flags will display only specific information used for scripting

**-F**	display only the Federated Index

**-I**	display only the Federated ID

**-P**	display only the progress status

**-R**	display only the current round

**-V**	display only the VM Index

### diagnostics network
```text
$ factom-cli diagnostics network [-LMDPHTB]
```
Get diagnostic information about the Factom network

Optional output flags will display only specific information used for scripting

**-B**	display only the last block from the DBState

**-D**	display only the current minute duration

**-H**	display only the balance hash

**-L**	display only the Leader height

**-M**	display only the current minute

**-P**	display only the previous minute duration

**-T**	display only the temporary balance hash


### diagnostics server
```text
$ factom-cli diagnostics server [-NIKR]
```
Get diagnostic information about the Factom API server

Optional output flags will display only specific information used for scripting

**-I**	display only the ID of the API server

**-K**	display only the public key of the API server

**-N**	display only the name of the API server

**-R**	display only the role of the API server

### diagnostics sync
```text
$ factom-cli diagnostics sync [-SRE]
```
Get diagnostic information about the network syncing

Optional output flags will display only specific information used for scripting

**-E**	display only the expected status

**-R**	display only the received status

**-S**	display only the syncing status

### ecrate

```text
factom-cli ecrate
```
It takes this many Factoids to buy an Entry Credit. Displays the larger between current and future rates. Also used to set Factoid fees.

### exportaddresses

```text
factom-cli exportaddresses
```
List the private addresses stored in the wallet

### exportidentitykeys

```text
factom-cli exportidentitykeys
```
List the identity key pairs (public and private) stored in the wallet

### get

```text
factom-cli get allentries|authorities|chainhead|currentminute|ablock|dblock|eblock|ecblock|fblock|
entry|firstentry|head|heights|pendingentries|pendingtransactions|raw|tps|walletheight

```
Get data about Factom Chains, Entries, and Blocks

### get abheight 
```text
factom-cli get abheight HEIGHT -r (to suppress Raw Data)
```
Get Admin Block by height

### get ablock
```text 
factom-cli get ablock [-RDBPL] HEIGHT|KEYMR
```
Get an Admin Block from factom by its Key Merkel Root or by its Height

Optional output flags will display only specific information used for scripting

**-B**	display only the Backreference Hash

**-D**	display only the Directory Block height

**-L**	display only the Lookup Hash

**-P**	display only the Previous Backreference Hash

**-R**	display the hex encoding of the raw Directory Block

### get allentries

```text
factom-cli get allentries [-n NAME1 -N HEXNAME2 ...] CHAINID
```
Get all of the Entries in a Chain

### get authorities

```text
factom-cli get authorities
```
Get information about the authority servers on the Factom network

### get chainhead

```text
factom-cli get chainhead [-n NAME1 -N HEXNAME2 ...] CHAINID
```
Get ebhead by chainid

### get currentminute

```text
$ factom-cli get currentminute [-BDFLMNRSTX]
LeaderHeight: 8596
DirectoryBlockHeight: 8596
Minute: 9
CurrentBlockStartTime: 1572635328110996891
CurrentMinuteStartTime: 1572635382109911982
CurrentTime: 1572635387202893194
DirectoryBlockInSeconds: 60
StallDetected: false
FaultTimeout: 30
RoundTimeout: 30

$ factom-cli get currentminute -L
8596   

$ factom-cli get currentminute -M
9
```
Get information about the current minute and other properties of the factom network.

Optional output flags will display only specific information used for scripting

**-L** display only the Leader height

**-D** display only the DirectoryBlock height

**-M** display only the Current minute

**-B** display only the Block start time

**-N** display only the Minute start time

**-T** display only the Current time

**-S** display only the Directorty Block in seconds

**-X** display only the Stall Detected value

**-F** display only the Fault timeout

**-R** display only the Round timeout

### get dbheight
```text 
factom-cli get dbheight HEIGHT -r (to suppress Raw Data)
```
Get Directory Block by height

### get dblock

```text
factom-cli get dblock [-RHKAVNBPFTDC] HEIGHT|KEYMR
```
Get a Directory Block from factom by its Key Merkel Root or by its Height

Optional output flags will display only specific information used for scripting

**-A**	display only the Directory Block Header Hash

**-B**	display only the Directory Block Body Merkel Root

**-C**	display only the Directory Block Count

**-D**	display only the Directory Block Height

**-F**	display only the Previous Directory Block Full Hash

**-H**	display only the Directory Block Hash

**-K**	display only the Directory Block Key Merkel Root

**-N**	display only the Network ID

**-P**	display only the Previous Directory Block Key Merkel Root

**-R**	display the hex encoding of the raw Directory Block

**-T**	display only the Directory Block Timestamp

**-V**	display only the Directory Block Header Version

### get eblock

```text
factom-cli get eblock KEYMR
```
Get eblock by Merkle root

### get echeight
```text 
factom-cli get echeight HEIGHT -r (to suppress Raw Data)
```
Get Entry Credit Block by height

### get ecblock

```text
factom-cli get ecblock [-RBPLDAHF] HEIGHT|KEYMR
```
Get ecblock by Key Merkle root or by height

Optional output flags will display only specific information used for scripting

**-A**	display only the Head Expansion Area

**-B**	display only the Body Hash

**-D**	display only the Directory Block Height

**-F**	display only the Full Hash

**-H**	display only the Header Hash

**-L**	display only the Previous Full Hash

**-P**	display only the Previous Header Hash

**-R**	display the hex encoding of the raw Entry Credit Block

### get entry

```text
factom-cli get entry [-RC] HASH
```
Get Entry by Hash. -R raw entry. -C ChainID

### get fbheight
```text 
factom-cli get fbheight HEIGHT -r (to suppress Raw Data)
```
Get Factoid Block by height

### get fblock
```text 
factom-cli get fblock [-RBPLED] KEYMR|HEIGHT
```
Get a Factoid Block by its Key Merkle Root or Height
Optional output flags will display only specific information used for scripting

**-B**	display only the Body Merkel Root

**-D**	display only the Directory Block Height

**-E**	display only the Exchange Rate

**-L**	display only the Previous Ledger Key Merkel Root

**-P**	display only the Previous Key Merkel Root

**-R**	display the hex encoding of the raw Factoid Block

### get firstentry

```text
factom-cli get firstentry [-n NAME1 -N HEXNAME2 ...] CHAINID [-REC]
```
Get the first Entry in a Chain. -R RawEntry. -E EntryHash. -C ChainID

### get head

```text
factom-cli get head [-RHKAVNBPFTDC]
```
Get the latest completed directory block

Optional output flags will display only specific information used for scripting

**-A**	display only the Directory Block Header Hash

**-B**	display only the Directory Block Body Merkel Root

**-C**	display only the Directory Block Count

**-D**	display only the Directory Block Height

**-F**	display only the Previous Directory Block Full Hash

**-H**	display only the Directory Block Hash

**-K**	display only the Directory Block Key Merkel Root

**-N**	display only the Network ID

**-P**	display only the Previous Directory Block Key Merkel Root

**-R**	display the hex encoding of the raw Directory Block

**-T**	display only the Directory Block Timestamp

**-V**	display only the Directory Block Header Version

### get heights

```text
$ factom-cli get heights [-DLBE]
DirectoryBlockHeight: 10000
LeaderHeight: 10001
EntryBlockHeight: 10000
EntryHeight: 10000

$ factom-cli get heights -D
10000
```
Get the current heights of various blocks in factomd

Optional output flags will display only specific information used for scripting
**-D** display only the DirectoryBlock height

**-L** display only the Leader height

**-B** display only the EntryBlock height

**-E** display only the Entry height

### get pendingentries

```text
factom-cli get pendingentries [-E]
```
Get all pending entries, which may not yet be written to blockchain. -E EntryHash.

### get pendingtransactions
```text
factom-cli get pendingtransactions [-T]
```
Get all pending factoid transacitons, which may not yet be written to blockchain. -T TxID.

### get raw
```text
factom-cli get raw HASH
```
Returns a raw hex representation of a block, transaction, entry, or commit

### get tps
```text
factom-cli get tps [-IT]
```
Get the current instant and total average rate of Transactions Per Second.

Optional output flags will display only specific information used for scripting

**-I**	display only the instant TPS rate

**-T**	display only the total averaged TPS rate

### get walletheight
```text
factom-cli get walletheight
```
Get the number of factoid blocks factom-walletd has cached

### identity

```text
factom-cli identity addchain|addkeyreplacement|addattribute| \
addattributeendorsement|composechain|composekeyreplacement| \
composeattribute|composeattributeendorsement|getactivekeys|getactivekeysatheight
```
Used with subcommands to create/manage Factom Identity Chains, their currently valid keys, attributes, and attribute endorsements

### identity addchain
```text
factom-cli identity addchain [-fq] [-n NAME1 -n NAME2] \
[-k PUBKEY1 -k PUBKEY2] [-CET] ECADDRESS
```

Create a new Identity Chain. Use the Entry Credits from the specified address.

Using the optional output flags allows you to get back information about the newly created chain and the transaction that created it. Those flags are: `-C` ChainID. `-E` EntryHash. `-T` TxID.

### identity addkeyreplacement
```text
factom-cli identity addkeyreplacement [-fq] \
 [-c CHAINID | -n NAME1 -n NAME2 ... -n NAMEN] \
 --oldkey PUBKEY --newkey PUBKEY --signerkey PUBKEY ECADDRESS [-CET]
```

Create a new Identity Key Replacement Entry using the Entry Credits from the specified address. The oldkey is replaced by the newkey, and signerkey (same or higher priority as oldkey) authorizes the replacement.

Using the optional output flags allows you to get back information about the newly created entry and the transaction that created it. Those flags are: `-C` ChainID. `-E` EntryHash. `-T` TxID.

### identity addattribute
```text
factom-cli identity addattribute [-fq] -c CHAINID \
-creceiver CHAINID -csigner CHAINID -signerkey PUBKEY \
-attribute ATTRIBUTE_JSON_ARRAY ECADDRESS [-CET]
```

Create a new Identity Attribute Entry using the Entry Credits from the specified address.

Using the optional output flags allows you to get back information about the newly created entry and the transaction that created it. Those flags are: `-C` ChainID. `-E` EntryHash. `-T` TxID.

The `-attribute` flag must be followed by a JSON array of sub-attributes (key:value mappings) surrounded by backticks like so:

```-attribute `[{"key":KEY1,"value":VALUE1},{"key":KEY2,"value":VALUE2},{"key":KEYN,"value":VALUEN}]` ```

### identity addattributeendorsement
```text
factom-cli identity addattributeendorsement [-fq] -c CHAINID \
-csigner CHAINID -signerkey PUBKEY -entryhash ENTRYHASH ECADDRESS [-CET]
```

Create a new Endorsement Entry for the Identity Attribute at the given entry hash. Uses the Entry Credits from the specified address.

Using the optional output flags allows you to get back information about the newly created entry and the transaction that created it. Those flags are: `-C` ChainID. `-E` EntryHash. `-T` TxID.

### identity composechain
```text
factom-cli identity composechain [-f] [-n NAME1 -n NAME2] \
[-k PUBKEY1 -k PUBKEY2] ECADDRESS
```

Create API calls to create a new Factom Identity Chain. Use the Entry Credits from the specified address.

### identity composekeyreplacement
```text
factom-cli identity composekeyreplacement [-f] \
[-c CHAINID | -n NAME1 -n NAME2 ... -n NAMEN] \
--oldkey PUBKEY --newkey PUBKEY --signerkey PUBKEY ECADDRESS
```

Create API calls to create a new Identity key replacement entry using the Entry Credits from the specified address. The oldkey is replaced by the newkey, and signerkey (same or higher priority as oldkey) authorizes the replacement.

### identity composeattribute
```text
factom-cli identity composeattribute [-f] -c CHAINID \
-creceiver CHAINID -csigner CHAINID -signerkey PUBKEY \
-attribute ATTRIBUTE_JSON_ARRAY ECADDRESS
```

Create API calls to create a new Identity Attribute Entry using the Entry Credits from the specified address.

The `-attribute` flag must be followed by a JSON array of sub-attributes (key:value mappings) surrounded by backticks like so:

```-attribute `[{"key":KEY1,"value":VALUE1},{"key":KEY2,"value":VALUE2},{"key":KEYN,"value":VALUEN}]` ```

### identity composeattributeendorsement
```text
factom-cli identity composeattributeendorsement [-f] \
-c CHAINID -csigner CHAINID -signerkey PUBKEY -entryhash ENTRYHASH ECADDRESS
```

Compose API calls to create a new Endorsement Entry for the Identity Attribute at the given entry hash. Uses the Entry Credits from the specified address.

### identity getactivekeys
```text
factom-cli identity getactivekeys [-c CHAINID | -n NAME1 -n NAME2 ... -n NAMEN]
```

Gets the set of identity public keys that are active for the given identity chain at the highest known block height.

### identity getactivekeysatheight
```text
factom-cli identity getactivekeysatheight [-c CHAINID | -n NAME1 -n NAME2 ... -n NAMEN] HEIGHT
```

Gets the set of identity public keys that were active for the given identity chain at the specified block height.

### importaddress

```text
factom-cli importaddress ADDRESS [ADDRESS...]
```
Import one or more secret keys into the wallet

### importidentitykeys

```text
factom-cli importidentitykeys SECKEY [SECKEY...]
```
Import one or more identity keys into the wallet from the specified idsec keys

### importwords

```text
factom-cli importwords '12WORDS'
```
Import 12 words from Koinify sale into the Wallet

### listaddresses

```text
factom-cli listaddresses
```
List the addresses stored in the wallet

### listidentitykeys

```text
factom-cli listidentitykeys
```
List the public identity keys stored in the wallet

### listtxs

```text
factom-cli listtxs [tmp|all|address|id|range]
```
List transactions from the wallet or the Factoid Chain

### listtxs address

```text
factom-cli listtxs address [-T] ECADDRESS|FCTADDRESS
```
List the transaction from the Factoid Chain with a specific address. This command will only list confirmed transactions.`-T` TxID.

### listtxs [all]

```text
factom-cli listtxs [all] [-T]
```
List all transactions from the Factoid Chain. This command will only list confirmed transactions.`-T` TxID.

### listtxs id

```text
factom-cli listtxs id [-T] TXID
```
List transaction from the Factoid Chain. This command will only list confirmed transactions.`-T` TxID.

### listtxs name

```text
factom-cli listtxs name [-T] TXID
```
Show a current working transaction in the wallet. `-T` TxID.

### listtxs range

```text
factom-cli listtxs range [-T] START END
```
List the transactions from the Factoid Chain within the specified range. This command will only list confirmed transactions.

### listtxs tmp

```text
factom-cli listtxs tmp
```
List current working transactions in the wallet

### newecaddress

```text
factom-cli newecaddress
```
Generate a new Entry Credit Address in the wallet

### newfctaddress

```text
factom-cli newfctaddress
```
Generate a new Factoid Address in the wallet

### newidentitykey

```text
factom-cli newidentitykey
```
Generates a new identity key in the wallet

### newtx

```text
factom-cli newtx [-q] TXNAME
```
Create a new transaction in the wallet

### properties

```text
factom-cli properties [-CFAWL]
```
Get version information about facotmd and the factom wallet

Optional output flags will display only specific information used for scripting

**-A**	display only the factomd API version

**-C**	display only the CLI version

**-F**	display only the factomd version

**-L**	display only the wallet API version

**-W**	display only the factom-wallet version

### receipt

```text
factom-cli receipt ENTRYHASH
```
Returns a Receipt for a given Entry

### replaydbstates

```text
factom-cli replaydbstates STARTHEIGHT ENDHEIGHT
```
Emit DBStateMsgs over the LiveFeed API between two specifed block heights

### rmaddress

```text
factom-cli rmaddress ADDRESS
```
Removes the public and private key from the wallet for the address specified.

### rmidentitykey

```text
factom-cli rmidentitykey PUBKEY
```
Removes the identity key pair from the wallet for the specified idpub key.

### rmtx

```text
factom-cli rmtx TXNAME
```
Remove a transaction in the wallet

### sendfct

```text
factom-cli sendfct [-fqrT] FROMADDRESS TOADDRESS AMOUNT
```
Send Factoids between 2 addresses. `-f` force. `-q` quiet. `-r` Netki DNs resolve. `-T` TxID. 

### sendtx

```text
factom-cli sendtx [-fqT] TXNAME
```
Send a Transaction to Factom. `-f` force. `-q` quiet. `-T` TxID.

### signtx

```text
factom-cli signtx [-fqT] TXNAME
```
Sign a transaction in the wallet. `-f` force. `-q` quiet. `-T` TxID.

### status

```text
factom-cli status [-TSUD] TxID|FullTx
```
Returns information about a factoid transaction, or an entry / entry credit transaction

Optional output flags will display only specific information used for scripting

**-D**	display the time of the transaction

**-S**	display the transaction status only

**-T**	display the transaction id only

**-U**	display the unix time of the transaction

### subtxfee

```text
factom-cli subtxfee TXNAME ADDRESS
```
Subtract the transaction fee from an output of a transaction in the wallet

### unlockwallet

```text
 factom-cli unlockwallet [-v] "passphrase" <seconds-to-unlock>
```
Unlock the wallet for some number of seconds; must be an encrypted wallet. -v verbose. (Note: it is recommended that you run the command with a leading space to prevent writing the password to the command line history.)
