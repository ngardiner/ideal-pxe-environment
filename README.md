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

This is the ISO mountpoint. 

## PHP Interface

## Images and Resources

* Live Environments
   * [FreeDOS](images/freedos.md) - Provide the ability to flash BIOS and ROM images over the network

* Installers
   * [FreeDOS](freedos.md)
