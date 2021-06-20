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

### Script

```
#!ipxe

:retry_dhcp
dhcp || goto retry_dhcp

:retry_menu
chain http://[server]/boot.php?mac=${net0/mac}&asset=${asset:uristring}&serial=${serial}&hostname=${hostname} || goto retry_menu
```

### Build

