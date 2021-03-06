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

For this, we create a dedicated FreeDOS directory under the tftpboot root and then a subdirectory for ROMs to be stored.

```
mkdir -p /var/lib/tftpboot/freedos/live/roms
```

Into this directory, you should create a structure which allows for all the various ROMS you will need to flash.

Be sure to make the filenames 8.3 friendly, as the resulting image will be FAT16 for compatibility's sake.

2 Create a ROM update image and inject these files into it

   * This will create a 512MB image to be attached as a secondary disk to the FreeDOS bootable image.

```
truncate updates.img --size 512MB

sfdisk updates.img << EOF
2048,,C,*
EOF
```

   * Create a FAT filesystem

Note that we set a variable, loopid, to avoid an issue where you may already have a loop device mounted as loop0. You can modify this to suit.

If you are only looking to update the image with new files using the commands in the step below, you only need to run the first 3 lines of the below commands.

```
export LOOPID=0
losetup /dev/loop${LOOPID} updates.img
partx -u /dev/loop${LOOPID}
mkfs.vfat -F 16 /dev/loop${LOOPID}p1 
```

   * Sync the files to the image file

This step of the process is safe to run again and again as you add more files to the 

```
mkdir -p /tmp/updates
mount /dev/loop${LOOPID}p1 /tmp/updates
rsync -av ./ --exclude updates.img /tmp/updates/
```
 
   * Unmount everything

```
umount /tmp/updates
losetup -d /dev/loop${LOOPID}
```

3 Download the FreeDOS bootable floppy image

```
cd /var/lib/tftpboot/freedos/live
wget http://www.freedos.org/download/download/FD12FLOPPY.zip
```

   * Place the following block in your ipxe menu file to allow the new image to be booted alongside FreeDOS

```
:freedos_live
set base-url http://${server_ip}/tftpboot/freedos/live
sanhook ${base-url}/roms/updates.img || goto failed
kernel ${memdisk} raw
initrd ${base-url}/FD12FLOPPY.zip
boot || goto failed
```

# References

   * With thanks to https://possiblelossofprecision.net/?p=2312 for the best approach to the live FreeDOS target
