# iPXE build

git clone
git checkout v1.21.1

Edit file src/config/general.h

   * Ensure that all DOWNLOAD_PROTO options are defined except for FILE
   * Enable the following IMAGE types (leave any others enabled):
      * BZIMAGE
      * COMBOOT
      * ELF
      * MULTIBOOT
      * PXE
      * SCRIPT
   * Enable all of the commands listed with the ```_CMD``` suffix

### Script

The purpose of the iPXE script is straightforward - it will repeatedly try to contact the TFTP server until it gets a response. This allows for retries of boot/network connectivity if there is an issue.

```
#!ipxe

:retry_dhcp
dhcp || goto retry_dhcp

:retry_menu
chain http://[server]/boot.php?mac=${net0/mac}&asset=${asset:uristring}&serial=${serial}&hostname=${hostname} || goto retry_menu
```

### Build

```
make bin/undionly.kpxe EMBED=../../ipxe-script.txt
cp   bin/undionly.kpxe ../../undionly-custom.kpxe
```

In DHCP servers, configure this as the boot filename

```
  next-server 192.168.100.1;
  filename "/ipxe/undionly-custom.kpxe";
```
