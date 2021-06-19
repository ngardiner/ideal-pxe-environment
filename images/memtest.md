# Memtest86+

## Introduction

Memtest86+ is an essential diagnostics tool for testing memory within a machine.

## Download

The following steps will download various Memtest86+ images 

## iPXE Configuration

### Memtest86+ v2.00

```
:memtest-200
set util_file images2/Memtest86+_V2.00
goto boot_image
```

### Memtest86+ v4.20

```
:memtest-420
set util_file images2/Memtest86+_V4.20
goto boot_image
```

### Memtest86+ v5.01

```
:memtest-501
set util_file images2/Memtest86+_V5.01
goto boot_image
```

### Memtest86+ v4.20 ELF

```
:memtest-420-elf
set util_file images/memtest86+.elf
goto boot_image
```

## Notes

There are two versions of the Memtest86+ image available. One is a zImage version that can be loaded as a kernel on some platforms (such as Virtualbox) but will often fail on hardware platforms with the error ```Requested memory not available```

A workaround is to use the ELF version of the image, which works on most platforms.
