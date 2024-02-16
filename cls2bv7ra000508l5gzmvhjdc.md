---
title: "1.5 Understanding and Managing Hard Links"
datePublished: Tue Jul 18 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cls2bv7ra000508l5gzmvhjdc
slug: 15-understanding-and-managing-hard-links
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708107863655/da573395-2a2c-49a2-8f98-31528ecaec23.png
tags: cloud, linux, devops, linux-for-beginners, 90daysofdevops

---

In the world of Unix-like operating systems, file management is a core concept that every user and administrator needs to understand. Among the many features of Unix file systems is the concept of links, specifically 'hard links'.

A hard link is essentially an additional name for an existing file on Linux or other Unix-like operating systems. Any number of hard links, and thus any number of names, can be created for any file. Hard links can be created for other hard links. However, they cannot be created for directories, and they cannot cross file system boundaries. This means you cannot create a hard link on one disk for a file on another disk.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706654888978/623fa26f-bdba-4196-8b0b-7d61bcb0d809.png align="center")

When we talk about hard links, it's important to note that there is no concept of the original file and the link in terms of the file system. Both the original and the hard link point to the same inode (the data structure that stores all the information about a file, except its name and its actual data).

To create a hard link, we use the `ln` command. Here's the basic syntax:

```bash
ln /path/to/file /path/to/hardlink
ln -s /path/to/file /path/to/softlink
```

After the hard link is created, you can use either name to edit the content of the file. Any changes made will be reflected, regardless of which name you use to access the file.

One key characteristic of a hard link is that it remains valid even if the original file is deleted. This happens because the hard link still points to the inode containing the data. The data will only be deleted when all hard links to the inode are deleted.

In conclusion, hard links are a powerful tool in Unix-like operating systems for managing and accessing files. They provide a way to organize files in a way that makes sense to the user, without duplicating data.  

| **BASIS FOR COMPARISON** | **<mark>HARD LINK</mark>** | **<mark>SOFT LINK</mark>** |
| --- | --- | --- |
| Basic | A file can be accessed through many different names known as hard links. | A file can be accessed through different references pointing to that file is known as a soft link. |
| Link validation, when the original file is deleted | Still valid and file can be accessed. | Invalid |
| Command used for creation | `ln` | `ln -s` |
| inode number | Same | Different |
| Can be linked | To its own partition. | To any other file system even networked. |
| Memory consumption | Less | More |
| Relative Path | Not applicable | Allowed |