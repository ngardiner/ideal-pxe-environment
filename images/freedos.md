# FreeDOS

## Installer

It's possible to install FreeDOS over the network using an installer image

## Live Image

A FreeDOS live image is particularly useful 

There are two options for providing a freedos live environment:

   * Map a Virtual Machine to an iSCSI LUN, and install FreeDOS on it
      * This can cause issues if you have multiple machines

   * Download a FreeDOS floppy image, and insert files into it

### FreeDOS over iSCSI

I'll document this if/when I set it up

### FreeDOS Floppy

The approach that we will take for this is a 2-part process:

1 Create a directory where we place all of our BIOS updates - for our machine types, NIC cards, Video Cards, SATA controllers etc.

For this, we create a dedicated FreeDOS directory under the tftpboot root and then a subdirectory for ROMs

```
mkdir -p /var/lib/tftpboot/freedos/live/roms
```

Into this directory, you should create a structure which allows for all the various ROMS you will need to flash.

1 Download a FreeDOS floppy image and inject these files into it
