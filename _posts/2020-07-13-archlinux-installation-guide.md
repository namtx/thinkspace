---
layout: post
title: "Arch Linux Installation guide"
description: "Arch Linux Installation guide"
comments: true
keywords: "k3s"
---

# Verify boot mode

```bash
ls /sys/firmware/efi/efivars
```

If the command shows the directory without error, then the system is booted in UEFI mode. If the directory does not exist, the system may be booted in BIOS (or CSM) mode. If the system did not boot in the mode you desired, refer to your motherboard's manual.

# Internet connect

```bash
ip link
```

```bash
ping archlinux.org
```

# Partition the disk

![archwiki]({{site.url}}/assets/images/arch-k3s__Running__and_Installation_guide_-_ArchWiki.jpg)

```bash
fdisk /dev/sda
```

As we're booted in BIOS so, we need `/dev/sda1` for `/mnt` (Linux) and `/dev/sda2` for `swap` (Linux Swap)

![./assets/images/arch-k3s__Running_.jpg]({{site.url}}/assets/images/arch-k3s__Running_.jpg)

```bash
mkfs.ext4 /dev/sda1

# make swap
mkswap /dev/sda2
swapon /dev/sda2

#mount /dev/sda1 to /mnt
mount /dev/sda1 /mnt
```

```bash
genfstab -U /mnt >> /mnt/etc/fstab
cat /mnt/etc/fstab
```

![./assets/images/arch-k3s__Running__and_Arch_Linux_Installation_guide.jpg]({{site.url}}/assets/images/arch-k3s__Running__and_Arch_Linux_Installation_guide.jpg)

# Set timezone

```bash
https://ipapi.co/timezone
Asia/Ho_Chi_Minh

timedatectl set-timezone Asia/Ho_Chi_Minh
```

# Change root

```bash
arch-chroot /mnt
```

# Set locale

```bash
# Open /etc/locale.gen file and uncomment `en_US.UTF-8 UTF-8 line and then
local-gen
```

# Install dhcpcd

```bash
pacman -Syu dhcpcd
systemctl enable dhcpcd
```

# Install Bootloader

```bash
pacman -S grub os-prober

grub-install /dev/sda

grub-mkconfig -o /boot/grub/grub.cfg
```

# Done

```bash
exit

reboot
```
