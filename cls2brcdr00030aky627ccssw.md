---
title: "1.4 Understanding File Permissioning (SUID, SGID, and Sticky Bit in Linux)"
datePublished: Sun Jul 16 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cls2brcdr00030aky627ccssw
slug: m1-essential-commands-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706815885880/d66a6528-a60f-4d3f-b334-d9fc961dd93b.png
tags: cloud, linux, devops, linux-for-beginners, 90daysofdevops, trainwithshubham

---

In the world of Linux file permissions, there are a few special permissions that are incredibly useful, yet often misunderstood. These are the Set User ID (SUID), Set Group ID (SGID), and Sticky Bit. Let's dive into what these permissions are, and how they can be beneficial for your system.

## Set-user Identification (SUID)

Have you ever thought, how a non-root user can change his own password when he does not have write permission to the /etc/shadow file. To understand the trick check for the permission of `/usr/bin/passwd` command:

```bash
$ ls -lrt /usr/bin/passwd
-r-sr-sr-x   1 root     sys        31396 Jan 20  2014 /usr/bin/passwd
```

If you check carefully, you will find the `2 S’s` in the permission field.

* The `1st s` stands for the `SUID` and
    
* The `2nd s` stands for `SGID`
    
* When a command or script with SUID bit set is run, its effective UID becomes that of the owner of the file, rather than of the user who is running it.
    

Another good example of SUID is the su command:

```bash
$ ls -l /bin/su 
-rw**s**r-xr-x-x 1 root user 16384 Jan 12 2014 /bin/su
```

The setuid permission displayed as an “s” in the owner’s execute field.

### How to set SUID on a file?

```bash
$ chmod 4555 path/to/file
```

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text"><strong>Note: </strong>If a capital “<code>S</code>” appears in the owner’s execute field, it indicates that the <code>setuid</code> bit is on, and the execute bit “<code>x</code>” for the owner of the file is off or denied.</div>
</div>

## Set-group identification (SGID)

### SGID permission on executable file

SGID permission is similar to the SUID permission, only difference is when the script or command with SGID on is run, it runs as if it were a member of the same group in which the file is a member.

```bash
$ ls -l /usr/bin/write
-r-xr-sr-x 1 root tty 11484 Jan 15 17:55 /usr/bin/write
```

The setgid permission displays as an “s” in the group’s execute field.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text"><strong>Note:</strong> If a lowercase letter “l” appears in the group’s execute field, it indicates that the setgid bit is on, and the execute bit for the group is off or denied.</div>
</div>

### How to set GUID on a file?

```bash
$ chmod 2555 path/to/file
```

### SGID on a directory

* When SGID permission is set on a directory, files created in the directory belong to the group of which the directory is a member.
    
* For example, if a user having write permission in the directory creates a file there, that file is a member of the same group as the directory and not the user’s group. This is very useful in creating shared directories.
    

### How to set SGID on a directory

```bash
$ chmod g+s path/to/directory
```

## Sticky Bit

* The sticky bit is primarily used on shared directories.
    
* It is useful for shared directories such as **/var/tmp** and **/tmp** because users can create files, read and execute files owned by other users, but are not allowed to remove files owned by other users.
    
* For example, if user bob creates a file named /tmp/bob, other user tom can not delete this file even when the /tmp directory has permission of 777.
    
    * If sticky bit is not set then tom can delete /tmp/bob, as the /tmp/bob file inherits the parent directory permissions.
        
    * Root user (Off course!) and owner of the files can remove their own files.
        

### Example of sticky bit:

```bash
$ ls -ld /var/tmp
drwxrwxrw**t** 2 sys sys 512 Jan 26 11:02 /var/tmp
```

* T refers to when the execute permissions are off.
    
* t refers to when the execute permissions are on.
    

### How to set sticky bit permission?

```bash
chmod +t [path_to_directory]
chmod 1777 [path_to_directory]
```

These special permissions, while powerful, can pose security risks if misused. It's critical to understand them thoroughly and use them judiciously to maintain a secure and efficient Linux system.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706656265199/8ac3c61a-6aed-4317-98eb-3474bb77a46c.png align="left")

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Please like and share your thoughts.</div>
</div>