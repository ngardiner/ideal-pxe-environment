# iPXE build

## Introduction

iPXE is an excellent open-source PXE ROM replacement.

There are many ways to get machines to boot using iPXE, but we take the simplest approach in this instance, by having the existing PXE ROM boot 

## Setting up iPXE

### Fetch Sources

Fetch the latest ipxe sources:

```
git clone https://github.com/ipxe/ipxe
git checkout v1.21.1
```

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

Use the following commands to build the iPXE image and place it in the tftpboot directory.

```
make bin/undionly.kpxe EMBED=../../ipxe-script.txt
cp   bin/undionly.kpxe ../../undionly-custom.kpxe
```

In DHCP servers, configure this as the boot filename

```
  next-server 192.168.100.1;
  filename "/ipxe/undionly-custom.kpxe";
```
