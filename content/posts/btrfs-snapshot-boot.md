+++
title = "Btrfs Snapshot: A Guide to System Rollbacks on Arch Linux"
date = "2024-12-02T16:13:11+07:00"
author = "Minh Thuan Tran"
authorTwitter = "" #do not include @
cover = "https://cdn.discordapp.com/attachments/538969606836977665/1313075601896243200/OCVet0y.png?ex=674ed073&is=674d7ef3&hm=a8298d621e7dbb0c160d89d22a626a512b3d45b00a623efd39c6161ab6cb8430&"
tags = ["Btrfs", "Arch Linux", "Snapshots", "System Rollback"]
keywords = ["Btrfs snapshots", "Arch Linux rollback", "GRUB boot menu", "system recovery"]
description = "Learn how to set up automatic Btrfs snapshots and add them to the GRUB boot menu for easy system rollbacks on Arch Linux."
showFullContent = false
readingTime = true
hideComments = false
+++
# BTRFS snapshots and system rollbacks on Arch Linux

Set up automatic snapshots of a BTRFS root subvolume (supposed you've installed your system using btrfs), add these snapshots to the GRUB boot menu, and gain the ability to rollback an [Arch Linux](https://www.dwarmstrong.org/tags/arch/) system to an earlier state.

# Let's go

## 1. Install btrfs assistant

Open the terminal and install [btrfs-assistant](https://github.com/nexusriot/btrfs-assistant) from the AUR using your favorite AUR helper
For this demo I'm gonna use [yay (yet another yogurt)](https://github.com/Jguer/yay)

```shell
yay -S btrfs-assistant
```

## 2. Enable snapshot in btrfs assitant

Open the btrfs-assitant, you can see there's many features such as Scrub, Balance, ... but today we will focus on enabling automatic snapshot service

[Btrfs Assistant Menu](https://cdn.discordapp.com/attachments/538969606836977665/1313076221193748560/aAAheQ6.png?ex=674ed106&is=674d7f86&hm=70338f717e685547607481f19efb2af7bee8e62324dda8275849d5eaa8485cb7&)

- Head to the **Snapper Setting** tab
- Click on **New config**
- Select **Backup path**
    - Select the sub directory you want to store in restoration
    - Note that if that sub volumn already covered by another backup config, it won't work
- Set the Backup interval:
    - For example setting 3 on daily will be the same as make 1 backup every 8 hours
- **Save** your configuration and **Apply systemd changes**

### Congrats on having a snapshot service now

---

## 3. Install grub-btrfs

Having snapshot service is a thing, what we want is to be able to boot from it

### We can achieve this by installing grub-btrfs 

```shell
sudo pacman -S grub-btrfs
```

Set the location of the directory containing the `grub.cfg` file in `/etc/default/grub-btrfs/config`.

Example: My `grub.cfg` is located in `/efi/grub` ...

At the next boot, there is an submenu in GRUB for `Arch Linux snapshots`.
