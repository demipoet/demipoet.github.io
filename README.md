 <link rel="shortcut icon" type="image/x-icon" href="favicon.png">

# Beginners' Guide - How to Setup a Quilibrium CeremonyClient node
Created by Demipoet. Let's connect at [Farcaster](https://warpcast.com/demipoet)! ... Don't have Farcaster account yet? Create your FC account [here](https://warpcast.com/~/invite-page/272717?id=1f9087d2).<br /><br />
Current Version: 1.4.1 (Sunset) as of March 3, 2024<br />
Want to refer to the old PDF Guide: [link](https://drive.google.com/file/d/1atQ2Gb8vLzqxiS2cqRAp9ojFNDJup3TU/view?usp=sharing)<br />

## Table of Contents
I. [Secure your Node hardware (VPS)](#i-secure-your-node-hardware-vps)<br />
II. [SSH for the first time to your new VPS](#ii-ssh-for-the-first-time-to-your-new-vps)<br />
III. [Prerequisite software](#iii-prerequisite-software)<br />
IV. [Configure Linux network device settings](#iv-configure-linux-network-device-settings)<br />
V. [Clone the Quilibrium CeremonyClient Repository](#v-clone-the-quilibrium-ceremonyclient-repository)<br />
VI. [Import your Voucher Hex](#vi-import-your-voucher-hex-optional) - Save your Peer ID / Q Wallet<br />
VI.B. [Run `go run` once to Create your Q Wallet and `.config` folder](#vib-run--go-run--once-to-create-your-q-wallet-and-config-folder)<br/>
VII. [Configure your Node Network Firewall](#vii-configure-your-node-network-firewall)<br />
VIII. [Configure your config.yml](#viii-configure-your-configyml)<br />
&emsp; 1. [Enable gRPC to enable gRPC Function Calls for your Node](#1-enable-grpc-to-enable-grpc-function-calls-for-your-node)<br />
&emsp; 2. [Enable Stats Collection by Opt-In](#2-enable-stats-collection-by-opt-in)<br/>
IX. [Safekeep the `Q Wallet` Private Key and Encryption Key](#ix-safekeep-the-q-wallet-private-key-and-encryption-key)<br/>
&emsp; 1. [keys.yml](#1-keysyml)<br/>
&emsp; 2. [config.yml](#2-configyml)<br/>
X. [Build the `node` Binary in `/root/go/bin` Folder](#x-build-the-node-binary-in-rootgobin-folder)<br/>
XI. [Create System Service for your Q Node](#xi-create-system-service-for-your-q-node)<br/>
&emsp; 1. [Limit Node CPU Usage](#1-limit-node-cpu-usage)<br/>
XII. [Monitor trace/info logs of your Q Node service](#xii-monitor-traceinfo-logs-of-your-q-node-service)<br/>
XIII. [Control your Q Node via service commands](#xiii-control-your-q-node-via-service-commands)<br/>
&emsp; 1. [Start your Q Node](#1-start-your-q-node)<br/>
&emsp; 2. [Stop your Q Node](#2-stop-your-q-node)<br/>
&emsp; 3. [Look at status of your Q Node](#3-look-at-status-of-your-q-node)<br/>
XIV. [Upgrading your Q Node to latest release](#xiv--upgrading-your-q-node-to-latest-release)<br/>
&emsp; 1. [Complete upgrade code script](#complete-upgrade-code-script)<br/>
XV.  [Purging your node, keeping the same Peer ID](#xv--purging-your-node-keeping-the-same-peer-id)<br/>
XVI. [Using gRPCurl For More Information on Q Network](#xvi--using-grpcurl-for-more-information-on-q-network)<br/>
XVII. [FAQ](#xvii--faq)<br/>

## I. Secure your Node hardware (VPS)
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br />
### 1. Get a VPS running Ubuntu 22.04

#### TOP RECOMMENDATION

##### Cherryservers

After Cassie has done some intensive testing, and working with the folks over at CherryServers, We are happy to announce that they are offering customized server instances for Q node runners. <br /><br />

CherryServers is a Europe-based crypto-friendly bare metal and cloud provider. We are proud to be one of the few service providers who openly embrace web3 as we believe in the principles of free internet, decentralization, and the freedom of choice without being tied down to any particular vendor. With over 20 years of experience and a strong reputation, we have established several successful partnerships within the blockchain field. We're excited to announce our latest partnership with Quilibrium to assist their node running community by offering stable infrastructure to enable high performance node hosting. We are excited to offer the following configurations, which have been validated for Quilibrium node hosting:<br /><br />

Sign-up link: <b>[link](https://www.cherryservers.com/?affiliate=5I763YKW)</b><br /><br />

* <b>Cloud VDS 4</b>: CPU: 4 x Intel Gold 6230R @ 2.10Ghz (8 vCores), RAM: 32GB, Disk Storage: NVMe 200GB, Backup: 50GB, Bandwidth: 1Gbps uplink, Egress Traffic: 10TB, Ingress unmetered, <b>96.12 USD/monthly</b><br />
* <b>E3-1240V3</b>: CPU: E3-1240v3, 4c/8t - 3.4GHz, RAM: 16GB ECC DDRIII , Disk Storage: 2x SSD 250GB, OS Disk: SSD 250GB, Backup: 100GB, Bandwidth: 1Gbps uplink, Egress Traffic: 10TB, Ingress unmetered, <b>106.92 USD/monthly</b><br />
* <b>E3-1240V5</b>: CPU: E3-1240v5, 4c/8t - 3.5GHz, RAM: 32GB ECC DDR4, Disk Storage: 2x SSD 250GB, OS Disk: SSD 250GB, Backup: 100GB, Bandwidth: 3Gbps uplink, Egress Traffic: 30TB, Ingress unmetered, <b>106.81 USD/monthly</b><br />
* <b>E5-1620V4</b>: CPU: E5-1620v4, 4c/8t - 3.5GHz, RAM: 32GB ECC DDR4, Disk Storage: 2x SSD 250GB, OS Disk: SSD 250GB, Backup: 100GB, Bandwidth: 3Gbps uplink, Egress Traffic: 30TB, Ingress unmetered, <b>124.20 USD/monthly</b><br />
* <b>E5-1650V3</b>: CPU: E5-1650v3, 6c/12t - 3.5GHz, RAM: 64GB Registered ECC DDR4, Disk Storage: 2x SSD 250GB, OS Disk: SSD 250GB, Backup: 100GB, Bandwidth: 1Gbps uplink, Egress Traffic: 30TB, Ingress unmetered, <b>125.66 USD/monthly</b><br />
* <b>E5-1650V4</b>: CPU: E5-1650v4, 6c/12t - 3.6GHz, RAM: 64GB Registered ECC DDR4, Disk Storage: 2x SSD 500GB, OS Disk: SSD 500GB, Backup: 100GB, Bandwidth: 3Gbps uplink, Egress Traffic: 30TB, Ingress unmetered, <b>132.68 USD/monthly</b><br />
* <b>E5-1650V3</b>: CPU: E5-1650v3, 6c/12t - 3.5GHz, RAM: 64GB Registered ECC DDR4, Disk Storage: 2x SSD 250GB, OS Disk: SSD 250GB, Backup: 100GB, Bandwidth: 1Gbps uplink, Egress Traffic: 30TB, Ingress unmetered, <b>125.66 USD/monthly</b><br />


#### System Requirements

For the Dawn phase, a server must have a minimum of:
> 16GB of RAM, preferably 32 GB,
> 250GB of storage, preferably via SSD,
> 50MBps symmetric bandwidth, ie 50 download, 50 upload
> For Intel/AMD, the baseline processor is a Skylake processor @ 3.4GHz with 12 dedicated cores.
> For ARM, the M1 line of Apple is a good reference, but this has to be a dedicated machine.

With Dusk, these minimum requirements will reduce significantly.

### 2. Connect to your VPS 
Downloading and install PuTTY: http://putty.org/  or just use your Terminal if you are running MacOS or Linux <br />
Put your IP address into terminal client & click open Click "Accept" Type "root" & click enter Type your password & click enter <br />
SSH into your VPS <br />

## II. SSH for the first time to your new VPS
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br /><br />
To SSH into a VPS, run:
```
ssh root@<ip_address_of_VPS>
```
> The authenticity of host (<ip_address>) can't be established.<br />
> ED25519 key fingerprint is SHA256:xxxxxxx<br />
> This key is not known by another names<br />
> Are you sure you want to continue connecting (yes/no/\[fingerprint\])?

```
yes
```

> Warning: Permanently added '<ip_address>' (ED25519) to the list of known hosts.
> Connection closed by <ip_address> port 22.

Enter the root password
SSH into your VPS again:
```
ssh root@<ip_address_of_VPS>
```
Enter the root password.

## III. Prerequisite software
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br />
### 1. Use root user for this installation
Make sure you are using the root user as this guide is tailored 

### 1. Update 
Run apt update to fetch the latest version of the package list from Ubuntu's software repository
```
sudo apt -q update
```

### 2. Install Git

```
sudo  apt  install  git  -y
```
```
git --version
```
It must show:
> git version 2.34.1

### 3. Install GO

The release right now (as of 1.2.14) does not support its project builds with go1.21.x nor go1.19.x. It has to be go1.20.x.

#### Download the distribution source
```
wget  https://go.dev/dl/go1.20.14.linux-amd64.tar.gz
```
OR
```
wget  https://go.dev/dl/go1.20.14.linux-arm64.tar.gz
```

The project support both AMD and ARM.<br/><br/>

<b>KNOWN ISSUE FOR CherryServers</b>, For some of their IP addresses, Google.com blocks them and this will prevent you from downloading (wget) the go1.20.14 gzip file from the URL above. To solve this, unfortunately, the only way is to cancel your new VDS and order a new one so that you get a new Primary IP address. I know this because I experienced this when I purchased my Cloud VDS4.

#### Extract the zipped (gz) file
```
sudo tar -xvf go1.20.14.linux-amd64.tar.gz 
```
#### Move the GO folder to usr folder
```
sudo  mv  go  /usr/local
```
#### Delete (remove) the downloaded GO zipped file
```
sudo  rm  go1.20.14.linux-amd64.tar.gz
```
#### Permanently set GO environment variables
```
sudo vim  ~/.bashrc
```
Press `i` to start inserting text into ~/.bashrc file<br />
Scroll until the end of the file, and insert the following in new lines at the end:<br />
```
GOROOT=/usr/local/go
GOPATH=$HOME/go
PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```
Press `esc` to stop the insert-text mode<br />
Press `shift` + `:wq`, and press `enter` or `return` on the keyboard<br />
Run:
```
source  ~/.bashrc
go version
```
It must show:
> go version go1.20.14 linux/amd64

## IV. Configure Linux network device settings
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br /><br />
Purpose: to optimize throughput (bandwidth) and latency for large parallel jobs typical of networks like Q<br />

Run:
```
sudo vim  /etc/sysctl.conf
```
Press `i` to start inserting text into /etc/sysctl.conf file<br />
Scroll until the end of the file, and insert the following in new lines at the end:<br />
```
# Increase buffer sizes for better network performance
net.core.rmem_max=600000000
net.core.wmem_max=600000000
```
Press `esc` to stop the insert-text mode<br />
Press `shift` + `:wq`, and press `enter` or `return` on the keyboard<br />
Run:
```
sudo  sysctl  -p
```
Reboot your VPS
```
reboot
```
You will be disconnected from your VPS as the VPS reboots<br />
SSH into your VPS again:
```
ssh root@<ip_address_of_VPS>
```
Enter the root password<br />

## V. Clone the Quilibrium CeremonyClient Repository
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br /><br />
Run:
```
cd ~
git  clone  https://github.com/QuilibriumNetwork/ceremonyclient.git
```
Go to the ceremonyclient/node folder
```
cd ceremonyclient/node
```
## VI. Import your voucher hex (optional)
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br /><br />
<b>Note:</b> Only applicable for those who has an offline voucher (from April 2023 offline declaration for the Freedom of Internet ceremony).<br />
If you do not have a voucher, skip this section and do [Section VI.B.](#vib-run--go-run--once-to-create-your-q-wallet-and-config-folder)<br/>
Run:
```
sudo vim /root/voucher.hex
```
Press `i` to start inserting text into voucher.hex file<br />
Copy-paste the 228-character voucher hex into the file<br />
Press `esc` to stop the insert-text mode<br />
Press `shift` + `:wq`, and press `enter` or `return` on the keyboard<br />
Run:
```
GOEXPERIMENT=arenas  go  run  ./...  -import-priv-key  `cat /root/voucher.hex`
```
Take note of your Peer ID. The Peer ID may act as your `Q Wallet` later on.<br />
It will be one of the last lines in the response, starts with 'Qm' with a label Peer ID.

## VI.B. Run " `go run` " once to Create your Q Wallet and `.config` folder
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br /><br />
<b>Note:</b> Only applicable if you skipped [Section VI](#vi-import-your-voucher-hex-optional), because you do not have an offline voucher, and you are installing a fresh Q Node.
Run:
```
cd ~/ceremonyclient/node
GOEXPERIMENT=arenas go run ./...
```
<br/>

As this will trigger for your Q Node to starup, all you really need right now is for the startup script to create you the `.config` folder inside `~/ceremonyclient/node` - to have the 2 files - `config.yml` and `keys.yml` created.
<br/><br/>

Once you see the logs start to trail, you would want to stop the node for now. <br/>
You need to open a new terminal window and SSH into your VPS again as root. <br/>
Run:
```
ps aux
```
and look for the PID of the `/root/go/bin/node`.<br/>
Once you know the PID of the `node` app, run:
```
kill -9 <PID>
```

This will kill the process for the `node` app. You may proceed to the next section.


## VII. Configure your Node Network Firewall
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br /><br />
Run:
```
sudo ufw enable
```
Type `y` and press `enter` or `return` on keyboard
Run:
```shell
sudo ufw allow 22
sudo ufw allow 8336
sudo ufw allow 443
sudo ufw status
```

Response for the status command should be:
```
> To            Action            From
> --            ------            -----
> 22            ALLOW             Anywhere
> 8336          ALLOW             Anywhere
> 443           ALLOW             Anywhere
> 22 (v6)       ALLOW             Anywhere (v6)
> 8336 (v6)     ALLOW             Anywhere (v6)
> 443 (v6)      ALLOW             Anywhere (v6)
```

## VIII. Configure your config.yml
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)
### 1. Enable gRPC to enable gRPC Function Calls for your Node<br/>

<b>Note</b>: This interface, while read-only, is unauthenticated and not rate-limited. It is recommended that you only enable them if you are properly controlling access via firewall or only query via localhost (i.e. if port 8337 is used for gRPC calls, best not to allow it on your firewall configuration later and only trigger gRPC calls on localhost).<br/><br/>

Go to ceremonyclient/node folder. <br/>
```
cd ~/ceremonyclient/node
```
Run:
```shell
sudo vim  .config/config.yml
```
Press `i` to start inserting text into config.yml file<br />
On the line, right about the end of file, there is a field `listenGrpcMultiaddr: “”`, replace it with 
```shell
listenGrpcMultiaddr: /ip4/127.0.0.1/tcp/8337
```
Press `esc` to stop the insert-text mode<br />
Press `shift` + `:wq`, and press `enter` or `return` on the keyboard<br />

### 2. Enable Stats Collection by Opt-In<br/>
Go to ceremonyclient/node folder. <br/>
```
cd ~/ceremonyclient/node
```
Run:
```shell
sudo vim  .config/config.yml
```
Press `i` to start inserting text into config.yml file<br />
On the line, right about the middle part of file, there is a field `engine`, append a sub-field called `statsMultiaddr`
```shell
engine:
  statsMultiaddr: "/dns/stats.quilibrium.com/tcp/443"
```
Press `esc` to stop the insert-text mode<br />
Press `shift` + `:wq`, and press `enter` or `return` on the keyboard<br />

## IX. Safekeep the `Q Wallet` Private Key and Encryption Key
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br/>

### 1. keys.yml
Go to ceremonyclient/node folder. <br/>
```
cd ~/ceremonyclient/node
```
Run:
```shell
sudo vim  .config/keys.yml
```
Copy the following lines on an external file or note on your personal desktop/laptop.
```shell
default-proving-key:
  id: default-proving-key
  type: 0
  privateKey: abcde258a29d0eabcde8eaed9741c404133c35f4d781d5c16dff83d4b55efghije77092b91308b6c5b03f39866e01891e643583e5fb00f3f81f3538e19bc25edb931c6ce5d30df1d76de2b4714d2256f49f8e3a141a114ff049d9b1f2c2d3bfc43ba027bac9077thisisafakeprivatekeyc6c5962c1975fe82980fac3b4aa85c1a8bd8cdf4f5163f01405abcde
  publicKey: abcde0486a744fc63thisisafakeprivatekeyd8df16af8e287f8bb7fbc9206e24cb7f054a53cacda84fca62337f9e2b3dca20f9121cbxxxxx
```
Press `shift` + `:q`, and press `enter` or `return` on the keyboard<br />

### 2. config.yml

Go to ceremonyclient/node folder. <br/>
```
cd ~/ceremonyclient/node
```
Run:
```shell
sudo vim  .config/config.yml
```
Copy all text, found in the config.yml file, on an external file or note on your personal desktop/laptop.
```shell
key:
  keyManagerType: file
  keyManagerFile:
    path: .config/keys.yml
    createIfMissing: false
    encryptionKey: abcde4fbef10d6b75c8a5d3xxxxx8c9298e0561c6xxxxxxef4ad345e279xxxxx

rest of the config.yml file
```
Press `shift` + `:q`, and press `enter` or `return` on the keyboard<br />

## X. Build the `node` Binary in `/root/go/bin` Folder
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br/>
Run:
```
GOEXPERIMENT=arenas  go  install  ./...
```
This will build and compile the `node` app binary inside `/root/go/bin/` folder<br/>
Verify that the node binary is built by running:
```
ls /root/go/bin
```
It must show
> node

## XI. Create System Service for your Q Node
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br/><br/>

Doing this step allows your node to be run as a system service, whereas in case your node shuts down or gets signal killed for whatever reason, the service will enable auto-restarting your node.<br/><br/>

The name of the system service we will make is called `ceremonyclient`<br/>
Run:
```
sudo vim /lib/systemd/system/ceremonyclient.service
```
Press `i` to start inserting text into ceremonyclient.service file<br />
Copy and paste the following lines inside the file
```text
[Unit]
Description=Ceremony Client Go App Service

[Service]
Type=simple
Restart=always
RestartSec=5s
WorkingDirectory=/root/ceremonyclient/node
Environment=GOEXPERIMENT=arenas
ExecStart=/root/go/bin/node ./...

[Install]
WantedBy=multi-user.target
```
Press `esc` to stop the insert-text mode<br />
Press `shift` + `:wq`, and press `enter` or `return` on the keyboard<br />


### 1. Limit Node CPU Usage

<b>Rationale</b>: The v1.4.2 update on Sunset has led to an increase in CPU usage. For everyone who is using VPS (including myself), please follow the instructions below to instruct the Q Node to only use a % of provisioned CPU cores
Open the `ceremonyclient.service` file<br/>
Run:
```
sudo vim /lib/systemd/system/ceremonyclient.service
```
Press `i` to start inserting text into ceremonyclient.service file<br />
Under [Service] add the following lines
```text
[Unit]
Description=Ceremony Client Go App Service

[Service]
CPUQuota=720%

...rest of file
```
> <b>Note</b>:The value for CPUQuota above is calculated by multiplying 80% or 90% by the number of CPU cores.<br/>
> E.g. If your VPS has 8 cores, you can set it as 8 * 90% = 720%<br/>

Press `esc` to stop the insert-text mode<br />
Press `shift` + `:wq`, and press `enter` or `return` on the keyboard<br />

## XII. Monitor trace/info logs of your Q Node service
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br/><br/>

Run:
```
sudo journalctl -u ceremonyclient.service -f --no-hostname -o cat
```
While the `ceremonyclient` system service is not started, nothing will be shown and that is okay<br/>
Just maintain this terminal window while you use another window to start your service (in next [Section XIII.1](#1-start-your-q-node))

## XIII. Control your Q Node via service commands
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br/>
### 1. Start your Q Node
Run:
```
service ceremonyclient start
```
<b>Note #1</b>: If you have the monitoring window on [Section XII](#xii-monitor-traceinfo-logs-of-your-q-node-service) open, you will notice after executing this start command that your Q Node will start up<br/><br/>

<b>Note #2</b>: If you skipped [Section VI](#vi-import-your-voucher-hex-optional) above, please take note of your Peer ID on the logs shown in monitoring window<br/><br/>

### 2. Stop your Q Node
Run:
```
service ceremonyclient stop
```

### 3. Look at status of your Q Node
Run:
```
service ceremonyclient status
```

## XIV.  Upgrading your Q Node to latest release
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br/>
First, run:
```
service ceremonyclient status
```
Press `ctrl` + `c` on your keyboard twice, to exit the status section in the terminal<br/>
If the response says service is Inactive, proceed to git fetch command below. <br/>
If response says service is Active, run: 
```
service ceremonyclient stop
```
<br/><br/>
Go to ceremonyclient folder. <br/>
```
cd ~/ceremonyclient
```
Check whether some files are updated in the ceremonyclient git repo, run:
```
git fetch origin
```
If there are files shown that are changed or different from your local copy, run:
```
git merge origin
```
Go to ceremonyclient/node folder. <br/>
```
cd ~/ceremonyclient/node
```
Next, do a `go clean` command to clear all previous build files. Run:
```
GOEXPERIMENT=arenas go clean -v -n -a ./...
```
Next, remove the compiled go binary file `node`, run:
```
rm /root/go/bin/node
ls /root/go/bin
```
The `ls` command should respond empty<br/>

<br/><br/>
Next, make a new build compiled binary file `node`, run:
```
GOEXPERIMENT=arenas  go  install  ./...
```
Verify that the `node` binary is built again, run:
```
ls /root/go/bin
```
Response should show
> node

Lastly, start your Q Node via the service command, run:
```
service ceremonyclient start
```

### Complete upgrade code script
```
service ceremonyclient stop
cd ~/ceremonyclient
git fetch origin
git merge origin
cd ~/ceremonyclient/node
GOEXPERIMENT=arenas go clean -v -n -a ./...
rm /root/go/bin/node
ls /root/go/bin
GOEXPERIMENT=arenas  go  install  ./...
ls /root/go/bin
service ceremonyclient start
```

## XV.  Purging your node, keeping the same Peer ID
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br/>

> The motivation for doing this, during this period as of March 12, 2024, is when your Q Node is seemingly stuck at a certain frame during syncing. This can be the case for the very suspect frame _*4067*_.
> I added this section for those folks who has finally reached the limits of their patience with frame 4067 and want to start a fresh node, built up using the latest Q release (at the moment, 1.4.5).
>
> Additional insights:
> I did this experiment myself and is showing good results. The new node never got stuck at 4067, but gets stuck in other frame numbers. Nevertheless, whenever I see this, I only need to stop and start the node and after some time, it moves on to higher frames.

<b>Note</b>: The steps below assume you have followed this guide to setup your Q node initially. Otherwise, the steps below may not be applicable to your node.<br/><br/>

<b>Here are the steps</b>:<br/><br/>

1. Copy your config.yml and keys.yml into your `/root` folder. command is
```
cp /root/ceremonyclient/node/.config/config.yml /root
cp /root/ceremonyclient/node/.config/keys.yml /root
```

2. Go to root folder.<br/>
```
cd /root
```

3. Next, delete the whole ceremonyclient folder.<br/>
<b>IMPORTANT Note</b>: Please make sure that you have the 2 .yml files inside `/root` folder by doing an `ls /root` command.
```
rm -rf /root/ceremonyclient
```

4. Delete the node binary file.
```
rm /root/go/bin/node
```

5. Git clone the repository again.
```
git  clone  https://github.com/QuilibriumNetwork/ceremonyclient.git
```

6. Go to node folder.
```
cd ceremonyclient/node
```
7. Create a .config directory
```
mkdir .config
```
8. Copy the config.yml and keys.yml into the newly created .config folder
```
cp /root/keys.yml /root/ceremonyclient/node/.config/
cp /root/config.yml /root/ceremonyclient/node/.config/
```
9. Compile the node binary file.
```
GOEXPERIMENT=arenas go install ./...
```
10. Start the service.
```
service ceremonyclient start
```
<br/>
To reiterate, this to me is not the final solution. I hope the next releases for Quilibrium will not have these issues anymore. However, if you still notice your node(s) get stuck in a certain frame, after following the steps above, do try to `service ceremonyclient stop` and `service ceremonyclient start` and do further monitoring.


## XVI.  Using gRPCurl For More Information on Q Network
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br/>

<b>Note</b>: This section is not part of the installation or setup process for the Q Node. The commands within this section are used only after you have successfully completed the steps above and you just want to query for more information about your Q Node.

<b>Reference</b>: https://github.com/mscurtescu/ceremonyclient/wiki/gRPCurl-How-To

### Installation

There are many ways to install gRPCurl (brew, docker, Go) and since we already have installed Go you may install gRPCurl from the source package<br/>
Run:
```
go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest
```
The command above takes care of installing the command in the bin sub-folder of $GOPATH<br/>
Verify your installation works, run:
```
grpcurl -help
```
As long as the terminal does not say unknown command, but something else, you have successfully installed gRPCurl

### Available gRPC Functions to call on your Q Node

#### Get Node Info
Run: 
```
grpcurl -plaintext localhost:8337 quilibrium.node.node.pb.NodeService.GetNodeInfo
```
Response:
```
{
  "peerId": "QmBgOwjQo6a12345Aq1fvc6wJ3xnTXNTzOLDLwFHyabcde==",
  "maxFrame": "7658"
}
```

#### Get Token Info (works for some nodes)
Run: 
```
grpcurl -plaintext localhost:8337 quilibrium.node.node.pb.NodeService.GetTokenInfo
```
Response:
```
{
  "confirmedTokenSupply": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACsuy4PqzEAA=",
  "unconfirmedTokenSupply": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACsuxtRfqwAA=",
  "ownedTokens": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=",
  "unconfirmedOwnedTokens": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA="
}
```
Values are encoded, but the ownedTokens and unconfirmedTokens are for sure equal to 0. Needs more decoding studies here.

#### Count of all Q Nodes
Run: 
```
grpcurl -plaintext -max-msg-sz 5000000 localhost:8337 quilibrium.node.node.pb.NodeService.GetPeerInfo | grep peerId | wc -l
```
Response:
```
11000
```

#### Get Peer Info
Run: 
```
grpcurl -plaintext -max-msg-sz 5000000 localhost:8337 quilibrium.node.node.pb.NodeService.GetPeerInfo | less
```
Response:
```
{
  "peerInfo": [
    {
      "peerId": "EiCIlJTLaQ+GE0ElmoCTb1wK/OWfWTzDA9hC1AyAmNNCew==",
      "multiaddrs": [
        ""
      ],
      "maxFrame": "1439",
      "timestamp": "1708984499614",
      "version": "AQIP",
      "signature": "uOXC/WFc3tCsaqJKplle2q/oeUDGlwZf/ZrDdjWqn/Z3ZBUZZy2jwbB78vhFrNsArVuEw+x9eSWAVTq7ugE4ME5bUNUB4+7Gl6BtV8vPNwqh5NWbfNooKbHDNyees6MEi+VsM04wS4wf147SLiGPxicA",
      "publicKey": "gMTEXXTwVMd9JGBTdJ3+eh/qIGuYapU+PGe7kg3HUgdlj4Ff8NBMVhHxoN/dYLnmpaquGOcqgKcA",
      "totalDistance": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAET2HAgKu7BSR42UPY8a2MEk/qOTrCjR9shges9bZvO/Ng=="
    },
    {
      "peerId": "EiA3Xnx1esdF2BJXwEuVFXw+gj5g5ss4ajJBQ3TNXWiAxA==",
      "multiaddrs": [
        ""
      ],
      "maxFrame": "6783",
      "timestamp": "1704564573411",
      "version": "AQIB",
      "signature": "JOJoCRUmzizAlFjf3dXB8wAeEvFXsXCIl66A/sLu4hjIJktYzvZPAc83abLeG3Zm8WHhuhtnHH4AXHitiWGlqwtFbWCRE6TuDFioemJBKhRgwS3bLF3KIC0yWEcBSTM5hgJCDWe7oCSI2NV8n7mufRUA",
      "publicKey": "prlfQeX4gLPLOz87HChd1zOWihxXeoVdUBMdBTjNmSYsYAGnJuMmGDdgZoipZWsrgsEbhnjKfa2A",
      "totalDistance": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAHW0BTyd89JROjwc23I0r7qOJ7ROny+L+EWD1n6OHytKKw=="
    },
```

## XVII.  FAQ
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br/>

#### What's my balance? Why am I still at 0?
> This question is the most frequently asked of them all. Depending on when you joined in on the project, the answer will vary. At this point, fully-synchronized nodes should start accumulating a balance. We are working to improve synchronization so it's not an incredibly long slog to get there. Everyone who has been participating should see some balance when Dusk launches.

#### How do I know my node is operating properly?
> You should see the frame number your node is processing continually increase. This may be slow at first due to heavy fork reconciliation – think of each node with a prover key as being able to send valid frames, but the timereel as a means to choose between them. Because we have had many iterations and lots of early networking issues, we may be initial forks that your node must churn through. We're working on improving sync to be smarter about this so you don't have to reconcile all of them.

#### Where do I run a node?
> Don't run it on IaaS/PaaS providers like AWS/GCP/Azure. The egress fees will eat you alive. If you don't want to/can't run on your own hardware, VPS/ bare metal hosts do well. Be advised: Q uses a lot of bandwidth in this stage.

#### How do I run a node?
> If you have git and go 1.20 installed, it's just these three steps:
```
git clone https://github.com/QuilibriumNetwork/ceremonyclient.git
cd ceremonyclient/node
GOEXPERIMENT=arenas go run ./...
```

> If you don't, follow whatever guide for your operating system to install go 1.20 (MUST be 1.20.X, 1.19- won't work, 1.21+ won't either).

#### I'm getting cannot load arena: malformed module path "arena" when I run it, what am I doing wrong?
> Wrong version of golang. Make sure you installed 1.20 (and it has priority on your PATH environment variable)

#### I'm getting the error below when I run the -balance command flag, what am I doing wrong?

```
panic: error getting token info: rpc error: code = Unknown desc = get token info : get highest candidate data clock frame: item not found
goroutine 1 [running]:
main.main()
/root/ceremonyclient/node/main.go:101 +0x745
exit status 2
```
> Nothing. 
##### -balance tag: 
> The -balance command tag when calling the GOEXPERIMENT=arenas go run ./... -balance has been deprecated since Dawn v1.2.9. Unless your node was upgraded to the latest version, yet upgraded from a version <1.2.9 (e.g. 1.2.7) (i.e. the very first version your node ran with was one older than 1.2.9), the -balance command tag will not work anymore and will face the error below.

##### GetTokenInfo gRPCurl function: 
> The same error shows for the GetTokenInfo function if you were to call using gRpcUrl library (see image below)

##### Solution: 
> You can still see your balance in the logs of a running node. In later releases, it is certain there will be a friendlier way to see a node’s $QUIL balance.

#### Someone is offering me QUIL OTC, is this legit?
> No. Tokens do not unlock until Dusk, nobody can transfer you QUIL or buy it from you.

#### Who is developing Quilibrium?
> Officially the core Quil dev team is just Cassie as of right now (making this an oddly third person statement), plus many folks who have submitted PRs to the core project, as well as related projects like Agost with quilibrium-rs, and Sir0uk with the nodekeeper dashboard. If you'd like to join in, there's loads of things to work on, please DM!

#### Wen balance
> See 1st question


