# [WIP] C720P × Chrome OS × Arch Linux

Setting up the Acer C720P Chromebook with Chrome OS and Arch Linux

## Introduction

This was done specifically on an `Acer C720P 29554003aii` with:

- Intel Celeron 2955
- 4 GB RAM
- 32 GB SSD
- 11.6" touchscreen display

I do not guarantee that this guide will work on other C720(P) variants (though it probably will).

This is a guide to:

- Repartition the Chromebook's for use with Chrome OS and Arch Linux (dual boot)
- Install Arch Linux on the newly created partition
- Set up the GRUB bootloader
- Configure touchpad and video drivers for the C720P

This guide will be straightforward. If you want to understand more of what we'll be doing (which I highly recommend), I'll be linking the appropriate Arch Wiki entry for further reading.

## Pre-installation

### Enable Developer mode [Arch Wiki](https://wiki.archlinux.org/index.php/Chrome_OS_devices#Enabling_developer_mode)

- Enter `Recovery mode` by holding `Esc + Refresh` and pressing the `Power` button.
- Press `Ctrl + D` and confirm

### Access root shell [Arch Wiki](https://wiki.archlinux.org/index.php/Chrome_OS_devices#Accessing_the_superuser_shell)

- From fresh install:
  - `Ctrl + Alt + Forward Arrow`
  - Username: `chronos`
  - Open root shell with `sudo bash`
- From configured Chrome OS system:
  - Open crosh with `Ctrl + Alt + T`
  - Open bash shell with `shell`
  - Open root shell with `sudo bash`

#### Enable SeaBIOS

From root shell, run:

```
crossystem dev_boot_usb=1 dev_boot_legacy=1
```

## Installation

### Repartition 



## Resources

This guide was heavily based on the Arch Linux wiki

- https://wiki.archlinux.org/index.php/Acer_C720_Chromebook
- https://wiki.archlinux.org/index.php/Chrome_OS_devices

TODO:
- Write protect screw
- Resetting Chromebook
- Partition SSD
- Repair Chrome OS
- Installing Arch
- Setting up GRUB
- Post installation
