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

2 Create a ROM update image and inject these files into it

   * This will create a 32MB image to be attached as a secondary disk to the FreeDOS bootable

```
truncate updates.img --size 32MB

sfdisk updates.img << EOF
63,,b
EOF
```

   * Create a FAT filesystem

```
losetup /dev/loop0 updates.img
mkfs.vfat /dev/loop0p1 
```

3 Define a FreeDOS boot 

```
cd /var/lib/tftpboot/freedos/live
wget http://www.freedos.org/download/download/FD12FLOPPY.zip
```

   * Place the following block in your ipxe menu file to allow the new image to be booted alongside FreeDOS

```
:freedos_live
set base-url http://${server_ip}/tftpboot/freedos/live
sanhook ${base-url}/roms/updates.img || goto failed
kernel memdisk raw
initrd ${base-url}/FD12FLOPPY.zip
boot || goto failed
```

# References

   * With thanks to https://possiblelossofprecision.net/?p=2312 for the best approach to the live FreeDOS target
