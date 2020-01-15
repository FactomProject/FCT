# Maintenance

## Updating your node

{% hint style="info" %}
If you verify that you are not a federated server \(by checking [http://fct.tools/](http://fct.tools/)\) you are able to do an update at any time, but write a notice about it in the discord channel \#testnet-chat first.
{% endhint %}

Updating your node basically just means pulling an updated docker build from the Factom repository and restarting your node:

```bash
docker stop factomd
docker rm factomd
```

Now run the startup command again, change the version to the up-to-date one at `factominc/factomd:v<version>` in the line below if it's outdated:

`docker run -d --name "factomd" -v "factom_database:/root/.factom/m2" -v "factom_keys:/root/.factom/private" -p "8088:8088" -p "8090:8090" -p "8110:8110" -l "name=factomd" factominc/factomd:v6.3.1-rc1-alpine -broadcastnum=16 -network=CUSTOM -customnet=fct_community_test -startdelay=600 -faulttimeout=120 -config=/root/.factom/private/factomd.conf`

{% hint style="danger" %}
The factomd version in the above command may be outdated, be sure to check what the [latest release](https://hub.docker.com/r/factominc/factomd/tags) is. 
{% endhint %}

## Brainswapping

### What is a brainswap

Brainswapping is the idea of having two nodes switch identities at the same time. We call it "brainswapping" because a node's identity dictates how it behaves.

It was implemented as a way to update the network without having to bring it down, as federated servers can "brainswap" with standby nodes that have already been updated with the new code.

The network will not perceive this as a node going offline, as the identity \(and thus the associated federated server\) is still online.

After transferring the Authority identity the node operator can now shutdown their original node, perform necessary updates, bring it back online and finally brainswap the identity back into the original server.

The procedure can also be used for migrating the Authority identity to a new physical server, by not performing the brainswap a second time to reverse the first swap .

### Preparing the brain swap

{% hint style="info" %}
For experienced and seasoned brainswappers there's an automated brain swap script made by Stamp-IT found [here](https://github.com/Stamp-IT-io/brainswap). However, **do not try this if this is the first time you're doing a brainswap, as it's important to understand the process in case something goes wrong.**
{% endhint %}

To perform the brain swap you will need one standby node that is ready. The standby node should be running the most recent Factomd software version if you've completed the previous steps.

#### Definitions

* **Federated Node** is the node that holds your authority identity, and needs to be updated. 
* **Standby Node** is a follower node on the network that you control.

#### Determining if you Standby Node is ready

If your standby node is not in sync with the network, performing the brainswap will result in your authority server going offline and likely to be demoted, so it is crucial to first check the health of the standby node.

* Check if the `DBHeight` matches that of the network.
  * This is done by comparing `DBHeight` in your control panel \(localhost:8090\) with the `DBHeight` of your federated server.
* Check if the minutes are following that of the network.
  * This is done by comparing the control panel of the Stanby node to that of the Federated. On the summary tab of the more Detailed Node Information you will see something similar to this:

    ```bash
      ===SummaryStart===
     FNode04[f0b7e3] L___vm01  0/ 0  0.0%  0.000 165[e0b9f8] 163/166/167  7/ 7 0/0/0/0 43400/0/0/0    0 0 2/40/100 0/0/0 0.07/0.00 0/0 - 309415
    ```

      The `7/7` means you are on minute 7. You will want to make sure this number is the same on the Standby `(_/7)` and the Federated `(7/7)`
* Check the process list for , that indicates some network instability. 
  * Process list is located in the control panel \(localhost:8090\) -&gt; "more detailed node information". If any entries show you should not move on with the brainswap.

Once you've confirmed that the standby node is ready, it's time to start the swap process.

### Performing the Brain Swap

{% hint style="danger" %}
If you haven't done a brain swap before, it is highly recommended that you read through this tutorial and watch the complete video before proceeding with the actual process.
{% endhint %}

The swap requires both config files \(located on Federated Node and Standby Node\) to be modified.

Open both config files in parallel in a text editor of your choice. The config files are located at `/var/lib/docker/volumes/factom_keys/_data`. Some examples with vim and nano: 

**vim:** `vi /var/lib/docker/volumes/factom_keys/_data/factomd.conf` 

**nano:** `nano /var/lib/docker/volumes/factom_keys/_data/factomd.conf`

Edit the files per the instructions below, save and exit.

Swap the following lines in the two config files:

```bash
IdentityChainID         = FA1E000000000000000000000000000000000000000000000000000000000000
LocalServerPrivKey      = 4c38c72fc5cdad68f13b74674d3ffb1f3d63a112710868c9b08946553448d26d
LocalServerPublicKey    = cc1985cdfae4e32b5a454dfda8ce5e1361558482684f3367649c3ad852c8e31a
```

{% hint style="info" %}
If you prefer to do a brain-transfer instead of a swap, you can just comment out the lines in your Federated node by placing a `;` in front of these lines.
{% endhint %}

Then add an additional line: `ChangeAcksHeight = 0`

This additional line is the brainswap logic. You will want to set the `ChangeAcksHeight` to some block height in the future \(remember the block height on the control panel is the last saved height, not the current working height!\).

**The safe height is the one you see in the control panel + 4**. \(localhost:8090\). If you know how to read the more detailed page, you can get away with a closer number.

Once you set the `ChangeAcksHeight` to `DBHeight+4`, save both files.

At the block height `ChangeAcksHeight` you should see both nodes change identities. If none of your nodes crash, and the identities change, the swap was successful.

## Moving server identity

This is only required if you want to create a new node, using your existing identity.

#### Notes:

* Replace your appropriate username
* Replace the IP of your hosts as appropriate
* The docker volumes need to have already been created.

{% hint style="danger" %}
IMPORTANT: before starting your factomd-node on your "new" host \("Host B"\), you MUST stop the factomd-node on your "old" host \("Host A"\).
{% endhint %}

### **Option 1: Single step**

This can be used if you have root access to the remote host. ****On your "old" host, copy the file from host A to host B -- \(scp\)

```bash
scp /var/lib/docker/volumes/factom_keys/_data/factomd.conf root@REMOTE-IP:/var/lib/docker/volumes/factom_keys/_data/factomd.conf
```

### **Option 2: Two-step**

On your "old" host, copy the file from host A to host B -- \(scp\):

```bash
 scp /var/lib/docker/volumes/factom_keys/_data/factomd.conf USER@REMOTE-IP:/home/USER/factomd.conf
```

On your "new" host, place the file in the appropriate folder:

```bash
 cp factomd.conf /var/lib/docker/volumes/factom_keys/_data/factomd.conf
```

