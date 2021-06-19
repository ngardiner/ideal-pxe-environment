# PLD Linux

## Introduction

PLD Linux is a useful rescue CD which, while getting a bit old (last release was 2013), still has value in the list of Rescue images.

We won't create an installer boot entry for PLD because it isn't a distro we are likely to deploy, but we will create entries both for the PLD Stable (2011-02-12) and beta (2013-03-12) images. These are particularly useful if you need a rescue CD with an older kernel version due to any compatibility issue, as Stable uses kernel version x and beta uses kernel version y.

Both can be downloaded from http://rescuecd.pld-linux.org/. 

## Download

Download both images to the tftpboot path, /var/lib/tftpboot/Linux/Rescue, as we are going to extract the files within the ISO and use them directly.

```
wget http://rescuecd.pld-linux.org/beta/RCDx64_13_03_12.iso
wget http://rescuecd.pld-linux.org/download/2011-02-12/x86_64/RCDx64_11_02.iso
```

Mount each of the ISO images

```
mkdir PLD-20110212
mkdir -p /tmp/pld
mount RCDx64_11_02.iso /tmp/pld
rsync -av /tmp/pld/ PLD-20110212/
umount /tmp/pld
rm RCDx64_11_02.iso
```

```
mkdir PLD-20130312
mkdir -p /tmp/pld
mount RCDx64_13_03_12.iso /tmp/pld
rsync -av /tmp/pld/ PLD-20110212/
umount /tmp/pld
rm RCDx64_11_02.iso
```

## Live Image

For the PLD image, we boot it by fetching the kernel and initrd over HTTP. The following definitions show the 

PLD Linux Stable (2011-02-12)
```
:pld-20110212
set path http://${server_ip}/tftpboot/Linux/Rescue/PLD-20110212
kernel ${path{/boot/isolinux/vmlinuz6
initrd ${path}/rescue6.cpi
boot || goto fail
```

PLD Linux Beta (2013-03-12)
```
:pld-20130312
set path http://${server_ip}/tftpboot/Linux/Rescue/PLD-20130312
kernel ${path}/boot/isolinux/vmlinuz6
initrd ${path}/rescue6.cpi
boot || goto fail
```
