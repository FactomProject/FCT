---
description: This page describes how to install Factom Federation.
---

# Installing Factom

Here is a step by step guide on how to install FF binaries on Mac, Windows, and Linux.

1. Download the installer for Mac, Windows, or Linux on [GitHub](https://github.com/FactomProject/distribution/releases). There are various versions, make sure to select the one best suited for your system.
2. Save it to your desktop or downloads folder on your local hard drive.
3. Follow the instructions for your OS.

## Mac

This installer is for `factomd`, `factom-walletd`, `factom-cli`, the three binaries will be installed in `/Applications/Factom/` together with a `.factom` folder in the root of the local user Home Folder. These are all required to run Factom via command line.

Before you start, go to System Preferences/Security & Privacy, click on the lock at the bottom left of the window, type your username and password, then select “Allow apps downloaded from: Anywhere.” At the prompt click on “Allow From Anywhere.”

![Security &amp; Privacy](https://docs.factom.com/images/wallet_005.png)

Note that after running the Factom installer it is recommended you revert your Security & Privacy settings to their original state.

Next, locate the `factom.mpkg.zip` file you just downloaded. Double-click to unzip it, then double-click the `factom.mpkg` file to run the installer.

The installer will open, click continue.

![Installer Step 1](https://docs.factom.com/images/wallet_006.png)

Then click install.

![Installer Step 2](https://docs.factom.com/images/wallet_007.png)

The installer will prompt for your username and password. You need to have Admin privileges on the Mac \(means you need to be able to install applications on your Mac\).

Enter your username and password then click Install Software.

![Installer Step 3](https://docs.factom.com/images/wallet_008.png)

The installer will proceed with the installation and once finished it will prompt with “The installation was successful.” Then click close.

![Installer Step 4](https://docs.factom.com/images/wallet_009.png)

## Windows

This installer is for `factomd`, `factom-walletd`, `factom-cli`, the three binaries will be installed in `c:\Program Files (x86)\Factom\ or c:\Program Files\Factom\` \(depending on your system\) together with a `.factom` folder in the root of the local user Home Folder. These are all required to run Factom via command line.

Next, locate the `FactomInstall-amd64.msi` or `FactomInstall-i386.msi` file you just downloaded in Step 1.

Double click the `.msi` installer to run it, but you may be prompted with the following message.

![Windows Prompt 1](https://docs.factom.com/images/wallet_010.png)

Click on `More info` to expand it then select “Run anyway.”

![Windows Prompt 2](https://docs.factom.com/images/wallet_011.png)

The Installer will open, click “Next.”

![Installer Step 1](https://docs.factom.com/images/wallet_012.png)

Select `I accept the terms in the License Agreement` and click `Next` to continue.

![Installer Step 2](https://docs.factom.com/images/wallet_013.png)

Make sure Factom is installed into `c:\Program Files (x86)\Factom\` folder and click `Next` to continue.

![Installer Step 3](https://docs.factom.com/images/wallet_014.png)

Then click `Install`.

![Installer Step 4](https://docs.factom.com/images/wallet_015.png)

Select `Yes` to continue if you get the following message.

![Windows Prompt 3](https://docs.factom.com/images/wallet_016.png)

When the installation is over select `Finish` to exit the installer..

![Installer Step 5](https://docs.factom.com/images/wallet_017.png)

## Linux

This installer is for `factomd`, `factom-walletd`, `factom-cli`, the three binaries will be installed on your local drive together with a `.factom` folder in the root of the local user Home Folder `~/.factom`. These are all required to run Factom via command line or the Factom Foundation Wallet.

Download the `factom-amd64.deb` or `factom-i386.deb` installer that suits your system as per Step 1 then run the following command to install.

```bash
sudo dpkg -i ./factom-amd64.deb
```

or

```bash
sudo dpkg -i ./factom-i386.deb
```

We are aware Linux users are hardcore, so we made sure one command is all they need to be ready to [install Enterprise Wallet](https://docs.factomprotocol.org/wallets/enterprise-wallet)!

## Docker

If you’ve made it this far, you may not have an operating system. This will make it very difficult to run Factom Federation since, at this point, we do still require you to use a computing device. However, if you’re simply looking for a way to get started in ANY environment, you’ve come to the right place.

Because we are a very modern organization, we’ve made arrangements for you to get started in a containerized environment. All you need is [Docker](https://www.docker.com/) at minimum v17 and a clone of the [factomd repo](https://github.com/FactomProject/factomd).

### **Build**

From wherever you have cloned the factomd repo, run:

```bash
docker build -t factomd_container .
```

{% hint style="info" %}
You can replace **factomd\_container** with whatever you want to call the container. e.g. **factomd**, **foo**, etc.
{% endhint %}

### **Cross-Compile**

To cross-compile for a different target, you can pass in a `build-arg` :

```bash
docker build -t factomd_container --build-arg GOOS=darwin .
```

### **Run**

**No Persistence**

```bash
docker run --rm -p 8090:8090 factomd_container
```

* This will start up **factomd** with no flags.
* The Control Panel is accessible at port 8090**..**
* When the container terminates, all data will be lost

{% hint style="info" %}
In the above, replace **factomd\_container** with whatever you called it when you built it - e.g. **factomd**, **foo**, etc.
{% endhint %}

**With Persistence**

```bash
docker volume create factomd_volume
docker run --rm -v $(PWD)/factomd.conf:/source -v factomd_volume:/destination busybox /bin/cp /source /destination/factomd.conf
docker run --rm -p 8090:8090 -v factomd_volume:/root/.factom/m2 factomd_container
```

* This will start up **factomd** with no flags.
* The Control Panel is accessible at port 8090
* When the container terminates, the data will remain persisted in the volume **factomd\_volume**
* The above copies **factomd.conf** from the local directory into the container. Put _your_ version in there, or change the path appropriately.

{% hint style="info" %}
In the above**:**

* replace **factomd\_container** with whatever you called it when you built it - e.g. **factomd**, **foo**, etc.
* replace **factomd\_volume** with whatever you might want to call it - e.g. **myvolume**, **barbaz**, etc.
{% endhint %}

### **Additional Flags**

In all cases, you can startup with additional flags by passing them at the end of the docker command, e.g.

```bash
docker run --rm -p 8090:8090 factomd_container -port 9999
```

### **Copy**

```text
docker run --rm --entrypoint='' \
    -v <FULLY_QUALIFIED_PATH_TO_TARGET_DIRECTORY>:/destination factomd_container \
    /bin/cp /go/bin/factomd /destination
```

> The following will copy the binary to `/tmp/factomd`

```text
docker run --rm --entrypoint='' \
-v /tmp:/destination factomd_container \
/bin/cp /go/bin/factomd /destination
```

So yeah, you want to get your binary _out_ of the container. To do so, you basically mount your target into the container and copy the binary over as shown. You should replace **factomd\_container** with whatever you called it in the [build](https://developers.factomprotocol.org/start/enterprise-wallet/installation#build) section above e.g. **factomd**, **foo**, etc.

**Cross-Compile Copy**

> Cross-Compile copy

```text
docker run --rm --entrypoint='' \
-v <FULLY_QUALIFIED_PATH_TO_TARGET_DIRECTORY>:/destination \
factomd_container \
/bin/cp /go/bin/darwin_amd64/factomd /destination
```

> To copy the darwin\_amd64 version of the binary to `/tmp/factomd`…

```text
docker run --rm --entrypoint='' 
-v /tmp:/destination factomd_container \
/bin/cp /go/bin/darwin_amd64/factomd /destination
```

If you cross-compiled to a different target, your binary will be in `/go/bin/<target>/factomd`. e.g. If you built with `--build-arg GOOS=darwin`, then you can copy out the binary using the commands shown on the right.You should replace **factomd\_container** with whatever you called it in the **build** section above e.g. **factomd**, **foo**, etc.

