---
title: "1.3 Mastering the Basics: Unveiling the Power of Essential Linux Commands Part-2"
datePublished: Fri Jul 14 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clku6njoo000009jmc9op2pya
slug: day-3-basic-linux-commands
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708107824089/e42a5f00-0726-4dd4-b143-4b8da89edc62.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1706737795945/137705e0-7f08-4785-8185-3a3abc399af9.png
tags: linux-for-beginners, linux-basics, linux-commands, 90daysofdevops, shubhamlondhe, trainwithshubham

---

In this article, we will explore various **file and directory manipulation tasks** in a Linux-based terminal.

* *Viewing File Contents:* To view what's written in a file (fruits.txt)
    

```bash
[~]$ cat fruits.txt
```

*Changing Access Permissions:* To change the access permissions of files (for example, make the file executable only for the owner)

```bash
[~]$ chmod u+rwx,g-rwx,o-rwx filename # Replace 'filename' with the actual file name
[~]$ chmod 700 filename # Replace 'filename' with the actual file name
```

| <mark>Bit</mark> | <mark>Purpose</mark> | <mark>Octal Value</mark> |
| --- | --- | --- |
| r | Read | 4 |
| w | Write | 2 |
| x | Execute | 1 |

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">while using octal code like <strong>xyz</strong>, where <strong>x</strong> gives permission for the <strong>user</strong> or <strong>owner</strong>, <strong>y</strong> talks about <strong>group</strong> &amp; <strong>z</strong> tells about <strong>others</strong>.</div>
</div>

Examples for more clarity:

* Full access for Owner, add read, remove execute for group and no access for others
    

```bash
[~]$ chmod u+rwx,g+r-x,o-rwx test-file
```

* Provide full access to Owners, group and others
    

```bash
[~]$ chmod 777 test-file
```

* Provide Read and execute access to Owners,groups and others
    

```bash
[~]$ chmod 555 test-file
```

* Read and Write access for Owner and Group, No access for others.
    

```bash
[~]$ chmod 660 test-file
```

* Full access for Owner, read and execute for group and no access for others.
    

```bash
[~]$ chmod 750 test-file
```

---

* *Checking Command History:* To check which commands you have run till now
    

```bash
[~]$ history
```

* *Removing a Directory/Folder:* To remove a directory/folder (for example, remove a directory named 'my\_folder')
    

```bash
[~]$ rm -r my_folder
```

* *Creating and Viewing fruits.txt:*
    

```bash
[~]$ echo "Apple Mango Banana Cherry Kiwi Orange Guava" > fruits.txt
[~]$ cat fruits.txt
```

* *Adding Content to devops.txt:* Add content in devops.txt (One in each line) - Apple, Mango, Banana, Cherry, Kiwi, Orange, Guava:
    

```bash
[~]$ echo -e "Apple\nMango\nBanana\nCherry\nKiwi\nOrange\nGuava" > devops.txt
```

* *Showing Top Three Fruits:*
    

```bash
[~]$ head -n 3 devops.txt
```

* *Showing Bottom Three Fruits*
    

```bash
[~]$ tail -n 3 devops.txt
```

* *Creating and Viewing Colors.txt*
    

```bash
[~]$ echo -e "Red\nPink\nWhite\nBlack\nBlue\nOrange\nPurple\nGrey" > Colors.txt

[~]$ cat Colors.txt
```

* *Adding Content to Colors.txt:* Add content in Colors.txt (One in each line) - Red, Pink, White, Black, Blue, Orange, Purple, Grey:
    

```bash
[~]$ echo -e "Red\nPink\nWhite\nBlack\nBlue\nOrange\nPurple\nGrey" > Colors.txt
```

* *Finding Differences Between Files:* To find the difference between fruits.txt and Colors.txt file
    

```bash
[~]$ diff fruits.txt Colors.txt
```

> "Choose a job you love, and you will never have to work a day in your life." - <mark>Confucius</mark>

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Happy learning!!!</div>
</div>

%[https://hashnode.com/post/clkso3bpx000009k275r554xg] 

%[https://hashnode.com/post/clktnsqy3001y09mo4o9abzdq]