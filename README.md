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

### Enable Developer mode ([Arch Wiki](https://wiki.archlinux.org/index.php/Chrome_OS_devices#Enabling_developer_mode))

- Enter `Recovery mode` by holding `Esc + Refresh` and pressing the `Power` button.
- Press `Ctrl + D` and confirm.

### Access root shell ([Arch Wiki](https://wiki.archlinux.org/index.php/Chrome_OS_devices#Accessing_the_superuser_shell))

- From fresh install:
  - `Ctrl + Alt + Forward Arrow`.
  - Username: `chronos`.
  - Open root shell with `sudo bash`.
- From configured Chrome OS system:
  - Open crosh with `Ctrl + Alt + T`.
  - Open bash shell with `shell`.
  - Open root shell with `sudo bash`.

#### Enable SeaBIOS ([Arch Wiki](https://wiki.archlinux.org/index.php/Chrome_OS_devices#Enabling_SeaBIOS))

- From root shell, run `crossystem dev_boot_usb=1 dev_boot_legacy=1`.

## Installation

### Repartition internal SSD ([Arch Wiki](https://wiki.archlinux.org/index.php/Chrome_OS_devices#Re-partition_the_drive))

- Login as a user.
- Download `chrubuntu-seabios-install.txt` from this repository into the Downloads folder (default).
- Open crosh `Ctrl + Alt + T` and open a shell with `shell`.
- Run the script with `sudo bash chrubuntu-seabios-intsall.txt`.

When prompted, give the number of GB you want your Arch Linux partiton to be.

I allocated 22 GB for Arch Linux and 10 GB for Chrome OS.

I initially allocated 8 GB for Chrome OS, but as I was using it, I kept getting Low Disk Space prompts, hence the 10 GB allocation.

### Repair Chrome OS filesystem ([Arch Wiki](https://wiki.archlinux.org/index.php/Chrome_OS_devices#Fixing_the_filesystem))

- Reboot and enter Chrome OS with `Ctrl + D`.
- Let Chrome OS repair itself.

## Arch Linux installaton

### Boot into Arch Linux

- Insert Arch Linux install media.
- Boot into SeaBIOS with `Ctrl + L`.
- Hit `Esc` and select the disk with Arch Linux.

### Create the ext4 partition to be used by Arch ([Arch Wiki](https://wiki.archlinux.org/index.php/Chrome_OS_devices#Continue_the_installation_process))

- Run `fdisk -l` and find the partition with the same size as specified in the earlier script. (We will refer to this as `/dev/sda7` for the rest of this guide. Your partition number may vary.)
- Run `mkfs.ext4 /dev/sda7` to create the filesystem for this partition.

### Create the BIOS boot partition ([Arch Wiki](https://wiki.archlinux.org/index.php/GRUB#GUID_Partition_Table_.28GPT.29_specific_instructions))

- Start gdisk with `gdisk /dev/sda`.
- Hit `n` to start creating a new partition.
- Select the default partition number by pressing `Enter`.
- When prompted for a start sector, select the default by pressing `Enter`.
- For the end sector, specify `+1MB` and hit `Enter`.
- Specify `ef02` as the partition type.
- Write the changes with `w`.

### Install Arch Linux ([Arch Wiki](https://wiki.archlinux.org/index.php/installation_guide))

- Install Arch Linux as you normally would, specifying `/dev/sda7` to mount to `/mnt`.

### Install GRUB

- Install `grub`
- Run `grub-install /dev/sda`
- Run `grub-mkconfig -o /boot/grub/grub.cfg`

## Post-installation

### Video drivers ([Arch Wiki](https://wiki.archlinux.org/index.php/intel_graphics))

- Install `xf86-video-intel` and `mesa`.
- Add the contents of `20-intel.conf` in this repository into `/etc/X11/xorg.conf.d/20-intel.conf`.

### Trackpad configs ([Arch Wiki](https://wiki.archlinux.org/index.php/Acer_C720_Chromebook#Touchpad_Configuration))

- Install `xf86-input-synaptics`.
- Add the contents of `50-cros-touchpad.conf` in this repository into `/etc/X11/xorg.conf.d/50-cros-touchpad.conf`.

### Disable power button shutdown ([Reddit](https://www.reddit.com/r/archlinux/comments/40mm4v/power_button_is_really_close_to_the_backspace/))

Since the power button is directly above the backspace, you can accidentally hit the power button by mistake, shutting down your system.

Not sure how this is handled by other window managers, but for i3, it turns of the machine without any warning.

To disable this, edit `/etc/systemd/logind.conf`, uncomment the line with `HandlePowerKey`, and the value to `ignore`.

### Fix wakeup from suspend on lid close ([Arch Wiki](https://wiki.archlinux.org/index.php/Acer_C720_Chromebook#Fix_wakeup_from_suspend_on_lid_close))

- Refer to `disable-touchpad-wakeup.conf` in this repository.
- Save the contents of the file to `/etc/tmpfiles.d/disable-touchpad-wakeup.conf`.

## Resources

This guide was heavily based on the Arch Linux wiki.

- https://wiki.archlinux.org/index.php/Acer_C720_Chromebook
- https://wiki.archlinux.org/index.php/Chrome_OS_devices
