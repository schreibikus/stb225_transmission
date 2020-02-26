# stb225 buildroot
This is makefile for building the Linux image for a STB(Set Top Box) based on PNX8335(stb225).
The main target of this image: to use STB only as Linux server.

## How to choose needed the Linux kernel version

### Currently, the default Linux kernel is version 4.19.x

### For using Linux 4.4.214 by default(Notice: RTL WiFi does not work)
Make the following configuration changes:
* Change "Toolchain" -> "Custom kernel headers series" to "4.4.x"
* Change "Kernel" -> "Kernel version" to "4.4.214"
* Change "Kernel" -> "Custom kernel patches" to "$(TOPDIR)/linux/pnx8335/linux-4.4-*.patch"

### For using Linux 4.9.214 by default
Make the following configuration changes:
* Change "Toolchain" -> "Custom kernel headers series" to "4.9.x"
* Change "Kernel" -> "Kernel version" to "4.9.214"
* Change "Kernel" -> "Custom kernel patches" to "$(TOPDIR)/linux/pnx8335/linux-4.9-*.patch"

### For using Linux 4.14.171 by default
Make the following configuration changes:
* Change "Toolchain" -> "Custom kernel headers series" to "4.14.x"
* Change "Kernel" -> "Kernel version" to "4.14.171"
* Change "Kernel" -> "Custom kernel patches" to "$(TOPDIR)/linux/pnx8335/linux-4.19-*.patch"

### For using Linux 4.20.17 by default(Notice: RTL WiFi does not work)
Make the following configuration changes:
* Change "Toolchain" -> "Custom kernel headers series" to "4.20.x"
* Change "Kernel" -> "Kernel version" to "4.20.17"
* Change "Kernel" -> "Custom kernel patches" to "$(TOPDIR)/linux/pnx8335/linux-4.20-*.patch"

### For using Linux 5.0.21 by default
Make the following configuration changes:
* Change "Toolchain" -> "Custom kernel headers series" to "5.0.x"
* Change "Kernel" -> "Kernel version" to "5.0.21"
* Change "Kernel" -> "Custom kernel patches" to "$(TOPDIR)/linux/pnx8335/linux-4.20-*.patch"

### For using Linux 5.1.21 by default(Notice: RTL WiFi does not work)
Make the following configuration changes:
* Change "Toolchain" -> "Custom kernel headers series" to "5.1.x"
* Change "Kernel" -> "Kernel version" to "5.1.21"
* Change "Kernel" -> "Custom kernel patches" to "$(TOPDIR)/linux/pnx8335/linux-4.20-*.patch"

### For using Linux 5.2.21 by default(Notice: RTL WiFi does not work)
Make the following configuration changes:
* Change "Toolchain" -> "Custom kernel headers series" to "5.2.x"
* Change "Kernel" -> "Kernel version" to "5.2.21"
* Change "Kernel" -> "Custom kernel patches" to "$(TOPDIR)/linux/pnx8335/linux-5.2-*.patch"

### For using Linux 5.3.18 by default(Notice: RTL WiFi does not work)
Make the following configuration changes:
* Change "Toolchain" -> "Custom kernel headers series" to "5.3.x"
* Change "Kernel" -> "Kernel version" to "5.3.18"
* Change "Kernel" -> "Custom kernel patches" to "$(TOPDIR)/linux/pnx8335/linux-5.2-*.patch"

## Build Linux image
To build the image run make in this directory.
Please find image at buildroot-*/output/images

## Configure u-boot and Writing image to NAND flash
Load by Y-modem Linux kernel image in to RAM using command:

loady 0x80800000 115200

Change the NAND flash partitioning with following command:

setenv mtdparts 'mtdparts=gen_nand:16k(Boot0),560k(Boot1),3M(Ker0),3M(Ker1),64k(EnvFs),20M(Kernel),-(Filesystem)'

Write Linux image to Flash:

nand erase Kernel

nand write 0x80800000 Kernel 0x1400000

Modify last variable

setenv bootcmd 'nand read 0x80800000 Kernel; run setargs; bootm 0x80800000'

saveenv

## Preparation in Linux:
To get access to transmission web interface open /mnt/nand/transmission/settings.json file by vi editor
and change line from

"rpc-whitelist": "127.0.0.1",

to

"rpc-whitelist": "127.0.0.1,192.168.*.*",

For mounting FAT USB-flash drive add following line in /mnt/nand/fstab file:

/dev/sda1 /mnt/downloads   vfat  iocharset=utf8,uid=1000,gid=1000 0 0

or if you have ext4 USB-Flash:

/dev/sda1 /mnt/downloads   ext4  defaults 0 0

To configure WIFI, you must create the configuration file /mnt/nand/wpa_supplicant.conf.
You can also copy the existing /etc/wpa_supplicant.conf file to the /mnt/nand directory and edit it.

To configure login through ssh, you must set the root password and edit /etc/ssh/sshd_config by adding the following line:

PermitRootLogin yes
