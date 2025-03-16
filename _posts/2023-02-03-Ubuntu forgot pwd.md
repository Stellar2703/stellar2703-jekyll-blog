---
title: "Reset Ubuntu Pwd"
date: 2023-02-03 00:00:00 +0800
categories: [Linux]
tags: [Password Reset]
---
### Method 1: Reset Password Using Recovery Mode

#### 1.1 Boot into Recovery Mode

- Restart your computer and hold down  ‘Shift’ key to access the GRUB 
- Select ‘Advanced options for Ubuntu,’ 
- Choose the ‘Recovery mode’ option.

#### 1.2 Remount Filesystem with Write Access

 - select ‘root – Drop to root shell prompt.’ 
 - remount the filesystem with write access using : `mount -o remount,rw /`

#### 1.3 Reset Password

To reset the password, type `passwd <username>` (replace `<username>` with your actual username) and press ‘Enter.’ 

#### 1.4 Reboot and Log In

Type `reboot` and press ‘Enter.’ Log in using your new password.



### Method 2: Reset Password Using Live USB/CD

#### 2.1 Boot from Live USB/CD

Open a terminal in ubuntu try without installing ubuntu
#### 2.3 Identify and Mount Root Partition

- Use the command `sudo fdisk -l` to identify root partition. 
- Mount the root partition using `sudo mount /dev/sdXY /mnt` 

#### 2.4 Chroot and Reset Password

- Change the root to the mounted partition with `sudo chroot /mnt`.
- Reset the password using `passwd <username>`

#### 2.5 Reboot and Log In

- Exit the chroot environment by typing `exit`.
- Unmount the partition with `sudo umount /mnt`. 
- Remove the Live USB/CD and reboot your computer. 
