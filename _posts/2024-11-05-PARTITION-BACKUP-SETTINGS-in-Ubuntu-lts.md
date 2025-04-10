---
title: "Partition and Backup"
date: 2024-11-05 00:00:00 +0800
categories: [Linux]
image: assets/images/ubuntu-linux.jpg
tags: [Partition,Backup]
---
# PARTITION SETTING

*The setup below is about how to create new partitions and create partitions of btrfs file system*

Go to root user
```
sudo su
```

List Block Devices:

This command is used to list block devices, showing information about disks and partitions.

```
lsblk
```

Detailed Disk Information:

This command provides detailed information about disks and their partitions.

```
fdisk -l
```

Partitioning (/dev/sda) with fdisk:

```
/dev/sda
	-fdisk /dev/sda #to create partiton
	 -p print the partition table
	 -g create anew empty GPT partion table
		 -gpt
	 -n add a new partition 
		 -partition number
		 -start sector 1024
		 -end sector +7200G (7)
		-p print the partition table
		-w write table to disk and exit

```

Create Btrfs File System:

This command formats the newly created partition `/dev/sda1` as a Btrfs file system.

```
  /dev/sda
	 -/dev/sda1
		 -mkfs.btrfs /dev/sda1 #make sta1 as btrfs
		
```
Create a Directory for Mounting:

This command creates a directory named "data" to be used as the mount point.

```
 -mkdir /data	
```

Mount the Btrfs File System:

Mounts the Btrfs file system on `/dev/sda1` to the `/data` directory.

```
mount /dev/sda1 /data

```
Check Btrfs Space Usage:

Displays the space usage of the Btrfs file system mounted on `/data`.
Shows information about the Btrfs file system, including the UUID.

```
btrfs fi usage /data
#to show the file data and information
btrfs fi show /data
	#note uuid
	-uuid:3c65d967-2d4e-4ee4-9155-a983c2f4f319 
	
```

Mount sdc1 to Root (/):

Mounts the `/dev/sdc1` partition to the root directory ("/").

```
/dev/sdb
/dev/sdc
 -/dev/sdc1 / #mount as root
```
Edit `/etc/fstab` for Persistent Mounting:

The name "fstab" stands for "file system table." The `/etc/fstab` file is crucial for the system's proper functioning because it defines the associations between various block devices (such as hard drives, SSDs, or partitions) and their respective mount points in the file system.

Through editor ,specify the uuid and mount point 

```
nano /etc/fstab  
    => /dev/disk/by-uuid/3c65d967-2d4e-4ee4-9155-a983c2f4f319 /data btrfs defaults 0 1 => Restart required
```

After modifying `/etc/fstab`, a system restart is recommended for the changes to take effect.


# Extend an LVM partition

1.**Check Available Space on Physical Volume**:

```
sudo pvs
```

2.**Check the Logical Volume Details**: Before extending, it’s a good idea to see the current setup of the logical volume you want to expand.
```
sudo lvdisplay
```

3.**Extend the Logical Volume**: Use the `lvextend` command to add the free space to the logical volume. Replace `<volume_group>` and `<logical_volume>` with your volume group and logical volume names, and allocate the additional space.

- **To add all available free space**:
```
sudo lvextend -l +100%FREE /dev/<volume_group>/<logical_volume>
```

 -  **To add a specific amount of space (e.g., 1 TB)**:
```
sudo lvextend -L +1T /dev/<volume_group>/<logical_volume>
```

# BACKUP SETTINGS
rsync - it is used  for mirroring ,backup , migration on data in servers

- `-a`: Archive mode, which preserves permissions, ownership, timestamps, and recursive copying.
- `-z`: Enables compression during the transfer, reducing the amount of data sent over the network.
- `-v`: Verbose mode, which shows the files being copied.
- `-P`: Combines `--partial` (allows resuming of partially transferred files) and `--progress` (shows progress during the transfer).

```
rsync -azvP /data/backup bitnmc@10.10.111.15:/data
```

now backup to required hard disk 
