# ideal-pxe-environment
Documents an ideal PXE environment containing a web interface for control

## Introduction

## TFTP Server

The TFTP server provides both TFTP and HTTP download for boot files and ISO images, as well as 

### Mounts

There are two key mounts on the tftp server, for boot and TFTP related files (ipxe, memdisk, images) and ISO files.

In my ideal TFTP environment, these are kept separate because the ISO files are reusable on other platforms (Proxmox, ESXi). It would be possible however to have your ISO directory under the tftpboot directory, there is only minor modification required for this.

The mount locations are:

   * /var/lib/tftpboot

This is the TFTP root directory, in the default directory that tftpd-hpa expects it to be. Apache is configured to map this same path to the virtual directory /tftpboot, to allow HTTP transfer of larger files which is much faster than TFTP.

  * /var/www/html/iso

This is the ISO mountpoint. By mounting it directly here, we avoid having to create another alias in Apache, however it could absolutely be set up the way that tftpboot is.

## PHP Interface

The PHP interface has 2 components

## admin.php

To be defined

## boot.php

This is the file that ipxe clients request in order to load the PXE menu. It has 2 key roles:

   * Identify if we know this machine and if this machine has a specific PXE configuration defined
      * If so, the machine will boot directly into its PXE target

  * Identify if the machine needs any variables set - for example is it on a network from which we should use a different IP to reach the tftp server.

## Images and Resources

The following files describe how images can be set up for each of these environments.

* Live Environments
   * [FreeDOS](images/freedos.md) - Provide the ability to flash BIOS and ROM images over the network
   * [Ubuntu](images/ubuntu.md) - The Ubuntu Live CD provides both the ability to install Ubuntu and a Rescue Environment

* Installers
   * [FreeDOS](images/freedos.md)
   * [Ubuntu](images/ubuntu.md)

* Diagnostics
   * [Hardware Diagnostics Tool](images/hdt.md)
   * [MemTest86+](images/memtest.md)

* Rescue Environments
   * [PLD Linux Rescue](images/pld.md) - PLD Linyx Rescue is an (older) Linux Rescue CD based on PLD Linux
