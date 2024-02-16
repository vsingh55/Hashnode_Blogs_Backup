---
title: "6.2 Disk Partitioning in Linux: A Comprehensive Guide"
datePublished: Sun Oct 01 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsp5ky2z00010alc3rmtf05d
slug: 62-disk-partitioning-in-linux-a-comprehensive-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708116821904/b7950d1b-06dc-4c40-944d-199d28a0600d.png
tags: linux, devops, linux-for-beginners, linux-basics, 90daysofdevops, shubhamlondhe, trainwithshubham, linuxforcloud

---

# Introduction:

Partitioning is a fundamental aspect of managing disk space in Linux systems. It involves dividing a disk into one or more logical units to organize data effectively. This comprehensive guide provides an in-depth exploration of disk partitioning using tools such as `fdisk`, `cfdisk`, `parted`, and `gparted`, covering the creation, deletion, and modification of partitions, as well as understanding partition tables and their types.

## Understanding Disk Partitioning

### Overview

Disk partitioning is the process of dividing a disk into smaller segments known as partitions. These partitions allow for better organization and utilization of disk space, facilitating data storage and management.

### Key Concepts

In Linux systems, disks are identified as `/dev/sda`, `/dev/sdb`, etc., in physical servers and `/dev/vda`, `/dev/vdb`, etc., in virtual machines. Partitions are named based on the disk name followed by a number, such as `/dev/sda1`, `/dev/sda2`, etc.

Partition tables, such as MBR (Master Boot Record) and GPT (GUID Partition Table), define the organization of partitions and data on the disk. Understanding these partition table types is crucial for effective disk management.

### MBR vs. GPT Partition Tables

| Properties | MBR | GPT |
| --- | --- | --- |
| Max Capacity | 2TB | 9.7ZB (~9.7 billion terabytes) |
| Max Partitions | 26 | 128 |
| Data Location | Beginning of drive | Throughout the drive |
| BIOS Type | Legacy BIOS | UEFI |

## Managing Partitions with `fdisk`

### Overview

`fdisk` is a command-line tool for managing disk partitions in Linux. It offers a text-menu driven interface for creating, deleting, and modifying partitions.

### Basic Commands

* `m`: Display the menu
    
* `p`: List the partition table
    
* `n`: Create a new partition
    
* `d`: Delete a partition
    
* `t`: Change a partition type
    
* `w`: Write the new partition table information and exit
    
* `q`: Quit without making changes
    

### Example Usage

```bash
sudo fdisk /dev/sdb
sudo fdisk -l /dev/sda
```

<mark>Historically, two commands exist to manipulate disks and partitions:&nbsp;</mark> **<mark>fdisk</mark>** <mark>&nbsp;and&nbsp;</mark> **<mark>parted</mark>**<mark>.</mark>

`fdisk` command doesn’t handle **GPT** partition tables.

Recently, a new tool called **gdisk** has been created to deal with **GPT** partition tables, offering an alternative to the **parted** command.

```bash
#Listing current partition system
lsblk
NAME           MAJ:MIN    RM    SIZE    RO    TYPE    MOUNTPOINT
vda              8:0       0     20G     0    disk
├─vda1           8:1       0      1G     0    part        /boot   #partition
└─vda2           8:2       0     19G     0    part                #partition
	├─cs-root    253:0       0     17G     0    lvm         /
	└─cs-swap    253:1       0      2G     0    lvm        [SWAP]

#list of partitions on block device /dev/sda
sudo fdisk --list /dev/sda
Disk /dev/sda: 20 GiB, 21474836480 bytes, 41943040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical) : 512 bytes / 512 bytes
I/0 size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: exb65442dd
Device      Boot    Start          End     Sectors   Size   Id        Type
/dev/sda1     *      2048      2099199     2097152     1G   83       Linux
/dev/sda2         2099200     41943039    39843840    19G   8e   Linux LVM
```

Fortunately, no actual changes are made until you write the partition table to the disk by entering `w`. It is therefore important to verify your partition table is correct (with `p`) before writing to disk with `w`. If something is wrong, you can jump out safely with `q`.

The system will not use the new partition table until you reboot. However, you can use the following command:

```bash
sudo partprobe -s
```

to try and read in the revised partition table. However, this doesn't always work reliably, and it is best to reboot before doing things like formatting new partitions, etc., as mixing up partitions can be catastrophic.

At any time, you can run the following command:

```bash
cat /proc/partitions
```

to examine what partitions the operating system is currently aware of.

You can display the partition table and take no action with the following command:

```bash
sudo fdisk -l /dev/sda
```

## Exploring Partitioning with `cfdisk`

### Overview

`cfdisk` provides a text-based "graphical" interface for partitioning disks. It allows users to create, delete, and modify partitions in a user-friendly manner.

`cfdisk` command is used to create, delete, and modify partitions on a disk device. It displays or manipulates the disk partition table by providing a text-based “graphical” interface.

```bash
sudo cfdisk /dev/sda
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708117606438/c20dd218-81a8-408e-9db1-b1c590b07c94.png align="center")

Pick *gpt* from the list (dos for MBR). Now you will see a partition table like this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708117705680/0ecab761-62b0-4b2e-87df-aaf4aabd0fb7.png align="center")

See the available free space. Here we have 20 GB. Select **NEW** and create a new partition. Use up-down arrow keys to navigate and enter to select. You can do many things with the free space, if you are installing a new system with a command line interface, you can see an option of using the selected space as **primary partition.**  **Example:** Select the size 2GB. Enter -&gt; and select **primary**. Similarly we can do a logical partition also.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708117908636/04c7b5f4-adc8-4b85-91fc-cd7bfa2e42dd.png align="center")

After sizing the partition, select what type do you want, in my case, I am choosing **Linux Swap**.

* After selecting the size and type **write** to the disk:
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708118075910/590700ef-3cd1-4149-9a2f-dcb89594cc27.png align="center")
    

## Understanding `parted` and `gparted`

### `parted`

`parted` is a command-line utility for partitioning disks. It offers more features compared to `fdisk`, including support for GPT partition tables.

#### Basic Commands

* `mklabel`: Create a new partition table
    
* `mkpart`: Create a new partition
    
* `rm`: Remove a partition
    
* `resize`: Resize a partition
    
* `print`: Display partition information
    
* `quit`: Quit `parted`
    

#### Example Usage

```bash
sudo parted /dev/sdc
```

### `gparted`

`gparted` is a graphical partition editor that provides a user-friendly interface for managing partitions. It is based on `parted` but offers a visual representation of disk partitions.

#### Features

* Create, delete, resize, move, copy, and paste partitions
    
* Support for various file systems
    
* Graphical representation of disk partitions
    

#### Example Usage

```bash
sudo gparted
```

## Conclusion

Mastering disk partitioning is essential for effective disk management in Linux environments. With tools like `fdisk`, `cfdisk`, `parted`, and `gparted`, users can confidently create, delete, and modify partitions to optimize disk usage and organize data efficiently. Understanding the differences between MBR and GPT partition tables empowers users to choose the appropriate partitioning scheme for their specific needs. Whether through the command line or graphical interfaces, disk partitioning remains a crucial skill for Linux administrators and users alike.