# NODE-RED-ON-NANO-PI-R2-OPEN-WRT


THIS IS AN OPEN PROJECT

AT THIS STAGE THE INSTALL WORKS HOWEVER I AM ENDING UP WITH A BLANK DASHBOARD PAGE--- THIS IS NOT COMPLETE ---
NODE-RED-ON-NANO-PI-R2-OPEN-WRT


# openwrt-node-red on Nano PI R2S

[![platform](https://img.shields.io/badge/platform-Node--RED-red)](https://nodered.org)

A packaging up of Node-RED for OpenWRT.

The `dist` directory contains the .ipk file you need to download and install. For example

    opkg install node-red_1.0.6.ipk


### Build scripts

Install / Burn OPEN-WRT onto an sd card  8-16 or 32 GB preferably by using Balena Etcher (example) and setup as your prefered configuration.

Flashing Utility: win32diskimager.rar Windows utility. Under Linux users can use "dd" BALENA ETCHER https://www.balena.io 

You may download the OPEN-WRT packages from :

OPEN WRT WEBSITE https://openwrt.org/toh/friendlyarm/friendlyarm_nanopi_r2s

Image Files: http://downloads.openwrt.org/snapshots/targets/rockchip/armv8/openwrt-rockchip-armv8-friendlyarm_nanopi-r2s-ext4-sysupgrade.img.gz

Upgrade File: http://downloads.openwrt.org/snapshots/targets/rockchip/armv8/openwrt-rockchip-armv8-friendlyarm_nanopi-r2s-squashfs-sysupgrade.img.gz

Once you have booted the NANO PI R2S log into open-wrt via SSH 
192.168.1.1 
ssh root@192.168.1.1
root , no password

Run the installer to install the LUCI web interface

opkg install luci

when complete open a web browser to 192.168.1.1

Login to openwrt

Configure password and configure your network as necessary, then update .

Turn off your system and remove the SD card.

Place the card in another Linux system, use G-Parted to resize the partitions so that they use the full card or enlarge the root partition and create another ext4 partition for SMB perhaps.
Regardless enlarge the partition as the standard partition will be of a small size. 

Once complete place the card back in the Nano PI and boot up your system.

Log into the LUCI ..your ip.. default 192.168.1.1

run the software updater and install:
node
node.npm
git
git-http

ssh into the router and run these commands:

npm install -g --unsafe-perm node-red

git clone https://github.com/node-red/node-red.git

npm install

npm run build

npm start


you will then see in terminal 
[info] Starting flows
[info] Started flows
[info] Server now running at http://127.0.0.1:1880/

You may then log into Node Red on your site -default- 192.168.1.1:1880

http://<hostname>:1880. You can find the IP address by running hostname -I 




Im my particular case i have installed node-red onto a USB disk suing these commands as well as following the above installation.


USB MOUNTING
Log into your Open-wrt system

 ssh root@192.168.1.1

opkg update

mkdir -p /mnt/usb
mount -t vfat /dev/sda1 /mnt/usb

cd /mnt/usb/

INSTALL AS PER THE ABOVE INSTRUCTIONS 
FINALLY YOU WILL BE IN THIS DIRECTORY:
 
\/mnt/usb/node-red#

npm start 

and you should have node red running.

At this stage when you reboot or after a reboot you will have to run :

root@OpenWrt:~# cd /mnt/usb/node-red
root@OpenWrt:/mnt/usb/node-red# npm start



The scripts can help you to recreate this yourself.

 - **buildNR.sh** - installs node.js and then Node-RED from npm and then several extra nodes into `/etc/node-red`. This is where you can add your customised nodes.
 - **ipk-pack.sh** - removes un-needed cruft, creates needed control files, and then creates the .ipk file
 - **packit** - a helper script that can be run on your host machine to automate the packaging by copying the necessary files to the target device, then calling ipk-pack, and then copying the created file back to your host.

### Other files

 - **flows.json, flows_cred.json** - Default flow file to load - Customise this if you want to start with something special.
 - **node-red** - the init.d script that starts and stops Node-RED as a service.

### OpenWRT admin menu - Luci files

OpenWRT uses Luci as it's admin web UI. The files in the `luci` directory can be used to add Node-RED to the admin menu so you can easily see the log (usually at `/var/log/node-red.log`).

The `addluci.sh` script will copy them into the correct place (as of Oct 2018), you will need to restart luci (or reboot).

The `status.lua` file currently just copies over the existing one. This may cause problems in the future if the order of items in the default file changes. There doesn't seem to be a good way to contribute an entry dynamically on install. Thus we don't automatically install these files for you. You may need to merge them manually.
