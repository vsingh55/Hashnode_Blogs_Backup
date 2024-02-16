---
title: "6.7 Configuring Systems to Mount File Systems on Demand with autofs"
datePublished: Tue Oct 17 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsp6luhm000109ju0d08d0gr
slug: 67-configuring-systems-to-mount-file-systems-on-demand-with-autofs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708119755023/729bacd7-390c-4149-99a8-acd1fa76c710.png
tags: cloud, linux, devops, linux-for-beginners, 90daysofdevops, shubhamlondhe, trainwithshubham, lfcs

---

# Introduction:

In Linux, mounting file systems permanently using the `/etc/fstab` configuration can sometimes lead to resource inefficiencies, especially when the mounted file system is rarely accessed. An alternative approach to conserving system resources is to utilize the `autofs` service, which allows file systems to be mounted and unmounted automatically on-demand. This guide will walk you through the process of setting up and configuring `autofs` to mount file systems such as NFS, AFS, SMBFS, CIFS, and local file systems.

## Introduction to `autofs`

The `autofs` service is a kernel-based tool that automatically mounts and unmounts file systems on demand. By utilizing `autofs`, system resources can be conserved as file systems are only mounted when they are accessed, reducing the overhead of permanently mounted file systems.

## Setting Up NFS Server

Before configuring `autofs`, ensure that the NFS server is installed and running on your system. You can install the NFS server and start the service using the following commands:

```bash
sudo dnf install nfs-utils
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
```

Once the NFS server is running, you can define the exported file systems in the `/etc/exports` configuration file. For example, to export the `/etc` directory to a specific IP address with read-only access, add the following line to `/etc/exports`:

```bash
/etc 127.0.0.1(ro)
```

After updating the `/etc/exports` file, reload the NFS server configuration:

```bash
sudo systemctl reload nfs-server.service
```

## Installing and Configuring `autofs`

Next, install the `autofs` package on your system:

```bash
sudo dnf install autofs
```

Start the `autofs` service and enable it to start automatically at boot:

```bash
sudo systemctl start autofs.service
sudo systemctl enable autofs.service
```

## Configuring `autofs`

### Editing the `/etc/auto.master` File

The `/etc/auto.master` file is the main configuration file for `autofs` and defines the directories to be automatically mounted. Edit this file to specify the mount points and their corresponding configuration files:

```bash
sudo vi /etc/auto.master
```

Add an entry in the `/etc/auto.master` file to define the directory for automounting and specify the location of the configuration file for `autofs` options. For example:

```bash
/shares/ /etc/auto.shares --timeout=400
```

In this example, `/shares/` is the directory for automounting, and `/etc/auto.shares` is the configuration file containing `autofs` options. The `--timeout=400` option sets the timeout value for unmounting the file systems after a period of inactivity.

### Creating the Configuration File for `autofs`

Create the configuration file specified in the `/etc/auto.master` file:

```bash
sudo vi /etc/auto.shares
```

In this configuration file, define the mount points and their corresponding file system locations. For example:

```bash
mynetworkshare -fstype=auto 127.0.0.1:/etc
```

In this example, `mynetworkshare` is the name of the mount point, `-fstype=auto` specifies the file system type to be automatically determined, and `127.0.0.1:/etc` is the location of the NFS share to be mounted on-demand.

### Reloading the `autofs` Service

After configuring `autofs`, reload the service to apply the changes:

```bash
sudo systemctl reload autofs.service
```

## Conclusion

By configuring `autofs` to mount file systems on demand, you can efficiently manage system resources and reduce overhead associated with permanently mounted file systems. This guide has provided step-by-step instructions for setting up and configuring `autofs` to automatically mount file systems such as NFS shares on-demand, enhancing the flexibility and performance of your Linux system.