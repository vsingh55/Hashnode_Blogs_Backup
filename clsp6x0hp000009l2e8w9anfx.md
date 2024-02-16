---
title: "6.9 Manage and configure LVM (Logical Volume Manager) storage"
datePublished: Sat Oct 21 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsp6x0hp000009l2e8w9anfx
slug: 69-manage-and-configure-lvm-logical-volume-manager-storage
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708120438540/abb0a412-8190-48a0-b733-78cb3d20b553.png
tags: cloud, linux, devops, 90daysofdevops, trainwithshubham, lfcs, powertocloud

---

# Introduction:

LVM (Logical Volume Manager) is a powerful storage management solution for Linux systems that allows administrators to manage disk space more flexibly by abstracting physical storage devices into logical volumes. Here's an overview of some common LVM commands and their usage:

1. **lvmdiskscan**: This command scans all disks visible to the system and provides information about them, including the device name, size, and type of partition table. It's useful for identifying available disks to be used with LVM.
    
    ```bash
    lvmdiskscan
    ```
    
2. **pvcreate**: This command initializes physical volumes (disks or partitions) to be used by LVM.
    
    ```bash
    pvcreate /dev/sdb1
    ```
    
3. **vgcreate**: This command creates a volume group, which is a collection of physical volumes. Volume groups are used as a pool of storage from which logical volumes are created.
    
    ```bash
    vgcreate myvg /dev/sdb1
    ```
    
4. **lvcreate**: This command creates logical volumes within a volume group. Logical volumes are like partitions but with more flexibility in terms of resizing and management.
    
    ```bash
    lvcreate -n mylv -L 10G myvg
    ```
    
5. **pvs**: This command displays information about physical volumes, including their size, free space, and volume group membership.
    
    ```bash
    pvs
    ```
    
6. **vgs**: This command displays information about volume groups, including their size, free space, and associated physical volumes.
    
    ```bash
    vgs
    ```
    
7. **lvs**: This command displays information about logical volumes, including their size, associated volume group, and mount point (if applicable).
    
    ```bash
    lvs
    ```
    

These commands provide the basic building blocks for managing and configuring LVM storage. With LVM, administrators can dynamically resize logical volumes, add or remove physical volumes from volume groups, and perform various other storage management tasks without downtime.