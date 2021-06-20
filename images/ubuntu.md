# Ubuntu

## Download

The creation of the Ubuntu TFTP environment requires:

   * NFS for mounting the ubuntu distribution for installation

### Desktop

1 Download the desktop release ISO

```
cd /var/www/html/iso
wget https://releases.ubuntu.com/20.04/ubuntu-20.04.2.0-desktop-amd64.iso
```

2 Create necessary directories and mount the ISO

```
mkdir -p /tmp/ubuntu
mount ubuntu-20.04.2.0-desktop-amd64.iso /tmp/ubuntu
mkdir -p /var/lib/tftpboot/Linux/Ubuntu/20.04/Desktop/
rsync -av /tmp/ubuntu/ /var/lib/tftpboot/Linux/Ubuntu/20.04/Desktop/
```

3 Clean up

```
umount /tmp/ubuntu
rmdir /tmp/ubuntu
```

4 Define iPXE boot commands for Ubuntu Desktop 20.04

```
ubuntu2004dt:
set server_ip 192.168.100.1
set nfs_path /tftpboot/Linux/Ubuntu/20.04/Desktop/
kernel nfs://${server_ip}${nfs_path}/casper/vmlinuz.efi || read void
initrd nfs://${server_ip}${nfs_path}/casper/initrd.lz || read void
imgargs vmlinuz.efi initrd=initrd.lz root=/dev/nfs boot=casper netboot=nfs nfsroot=${server_ip}:${nfs_path} ip=dhcp splash quiet -- || read void
boot || read void
```

### Server

1 Download the server release ISO

```
cd /var/www/html/iso
wget https://releases.ubuntu.com/20.04/ubuntu-20.04.2-live-server-amd64.iso
```

2 Create necessary directories and mount the ISO

```
mkdir -p /tmp/ubuntu
mount ubuntu-20.04.2-live-server-amd64.iso /tmp/ubuntu
mkdir -p /var/lib/tftpboot/Linux/Ubuntu/20.04/Server/
rsync -av /tmp/ubuntu/ /var/lib/tftpboot/Linux/Ubuntu/20.04/Server/
```

3 Clean up

```
umount /tmp/ubuntu
rmdir /tmp/ubuntu
```

## Installer

## Live Environment

The Ubuntu ISO is useful as a rescue CD, providing a useful live environment

## References

   * https://ipxe.org/appnote/ubuntu_live
