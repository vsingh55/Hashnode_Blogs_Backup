---
title: "6.8 Evaluate and compare the basic file system features and options (findmnt, remount)"
datePublished: Tue Oct 17 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsp6r5ih00010ajv1tx32hrq
slug: 68-evaluate-and-compare-the-basic-file-system-features-and-options-findmnt-remount
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708120147958/2b0a3727-4f53-4b7e-87c4-ad609ac07b48.png
tags: cloud, linux, devops, linux-for-beginners, 90daysofdevops, shubhamlondhe, trainwithshubham

---

# Introduction:

The `findmnt` command is a useful tool for listing all mounted file systems on a Linux system and provides information about their targets, sources, file system types, and mount options. Below, I'll summarize the basic features and options of `findmnt`, as well as demonstrate how to use the `remount` command to change mount options on a mounted file system.

### Basic Features and Options of `findmnt`:

* **TARGET:** Shows the target mount point.
    
* **SOURCE:** Shows the source device or location.
    
* **FSTYPE:** Shows the type of file system.
    
* **OPTIONS:** Shows the mount options for each file system.
    

### Examples of `findmnt` Usage:

1. List all mounted file systems:
    
    ```bash
    findmnt
    ```
    
2. List mounted file systems of specific types (e.g., xfs):
    
    ```bash
    findmnt -t xfs
    ```
    
3. Change mount options of a mounted file system (e.g., remount as read-only):
    
    ```bash
    sudo mount -o remount,ro /dev/vdb2 /mnt
    ```
    
4. View mounted file systems after changing mount options:
    
    ```bash
    findmnt -t xfs
    ```
    
5. Apply mount options persistently using `/etc/fstab`:
    
    ```bash
    sudo vim /etc/fstab
    # Add or update the entry for /dev/vdb2 to include desired mount options
    /dev/vdb2 /mnt xfs ro,noexec,nosuid 0 0
    ```
    
6. Reboot the system to apply changes made in `/etc/fstab`:
    
    ```bash
    sudo systemctl reboot
    ```
    
7. Verify changes to mount options after reboot:
    
    ```bash
    findmnt -t xfs
    ```
    

### Using `remount` to Change Mount Options:

The `remount` option is used with the `mount` command to change mount options on a mounted file system without unmounting it first. This is useful for applying changes to mount options while the file system is in use.

```bash
sudo mount -o remount,rw,noexec,nosuid /dev/vdb2 /mnt
```

This command remounts the `/dev/vdb2` file system at the `/mnt` mount point with the specified mount options (`rw`, `noexec`, `nosuid`). It allows for dynamic changes to mount options without requiring the file system to be unmounted and remounted.