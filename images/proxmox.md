# Proxmox Installer

## Introduction

The Proxmox installer IOS file can be served over iPXE to enable netboot installation of Proxmox servers without the need for a USB key.

The quickest way to serve the files is via HTTP, however this can be modified to also work over TFTP as well.

## Download Proxmox Installer ISO

1 Download the Proxmox VE installer ISO image

```
cd /var/www/html/iso
wget http://download.proxmox.com/iso/proxmox-ve_6.4-1.iso
```

2 Mount the image and extract the tftp files

```
mkdir -p /var/lib/tftpboot/Linux/Proxmox/6.4
mkdir -p /tmp/pxmx
mount proxmox-ve_6.4-1.iso /tmp/pxmx
cp /tmp/pxmx/boot/linux26 /var/lib/tftpboot/Linux/Proxmox/6.4/
cp /tmp/pxmx/boot/initrd.img /var/lib/tftpboot/Linux/Proxmox/6.4/
```

3 Define the iPXE menu entry for Proxmox


## References

  * https://github.com/morph027/pve-iso-2-pxe
