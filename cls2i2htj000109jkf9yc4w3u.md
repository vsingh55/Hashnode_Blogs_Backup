---
title: "M.1: Essential Commands"
datePublished: Sat Jul 22 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cls2i2htj000109jkf9yc4w3u
slug: m1-essential-commands-1-1-1-1-1-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706748526638/21cc85d7-ab50-457e-b6bd-08d909fdf7be.png
tags: linux, devops, linux-for-beginners, linux-basics, 90daysofdevops

---

# Comparing and Manipulating File Content with Sort and Uniq

Continuing our exploration of command-line file manipulation, this part of the blog focuses on the `sort` and `uniq` commands. We'll delve into how these commands, when paired with regular expressions, can efficiently compare and manipulate file content.

## Sort Command

The `sort` command is instrumental in arranging lines within a file, be it alphabetically or numerically. Regular expressions come into play when defining custom sorting criteria.

**Example 1: Sort lines in a file alphabetically**

```bash
sort filename.txt
```

**Example 2: Reverse sort lines based on the second column (numerically)**

```bash
sort -k2,2nr data.txt
```

**Example 3: Sort lines based on a custom pattern (e.g., month abbreviation)**

```bash
sort -t"-" -k2,2M -k3,3n dates.txt
```

**Example 4: Sort lines based on the last word in each line**

```bash
sort -t" " -kNF file.txt
```

**Example 5: Sort lines ignoring leading whitespaces**

```bash
sort -b data.txt
```

## Uniq Command

The `uniq` command, as the name suggests, is designed to identify and filter out repeated lines within a file. Regular expressions add depth to its functionality.

**Example 1: Display only unique lines in a sorted file**

```bash
sort data.txt | uniq
```

**Example 2: Count and display the number of occurrences of each line**

```bash
sort logfile.txt | uniq -c
```

**Example 3: Display only repeated lines in a sorted file**

```bash
sort data.txt | uniq -d
```

**Example 4: Display only unique lines, ignoring the first 5 characters**

```bash
sort file.txt | uniq -s 5
```

**Example 5: Display only the first occurrence of each repeated line**

```bash
sort data.txt | uniq -u
```

By combining `sort` and `uniq` with regular expressions, you gain precise control over how data is ordered and filtered. These commands, when used in conjunction with the previously discussed ones, provide a comprehensive toolkit for efficient file content manipulation on the command line.

Understanding these examples and experimenting with various patterns will empower you to handle a wide range of file manipulation tasks effectively. Regular expressions, as the common thread, tie together these commands into a cohesive and powerful set for text processing and analysis.