---
title: "M.1: Essential Commands"
datePublished: Wed Jul 19 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cls2fov41000009kz3ibkgtll
slug: m1-essential-commands-1-1-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706743294648/f891bae9-4ac3-4402-93e2-c39659cd6b51.png
tags: linux, vim, devops, linux-for-beginners, 90daysofdevops

---

*There are a number of text editors available in Linux such as vi, vim, Nano etc. but we will focus on vi because it’s the most popular one.*

*Vi editor comes installed by default in most operating systems. Run the “vi” command and specify the file name to open it e.g. —* ***vi index.html***

The terminal opens the file and you’ll now be inside the editor. The vi editor has 3 modes of operation, the command mode, insert mode and last line mode. When you open a file in the vi editor, you are by default in the command mode and in this mode, you can issue commands such as to copy and paste lines or delete a line or word or to navigate between lines etc. but you can’t write contents to the file. To write contents to the file, you must switch to the **insert mode**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706744491371/524c33bb-d4dc-4917-9d01-8bf500ef52fb.png align="center")

Vi (Visual) is a powerful text editor included in most Linux systems. It has a rich set of features and commands that make it a go-to choice for many developers and system administrators. It’s known for its efficiency and ability to handle complex editing tasks with just a few keystrokes.

To edit or create new files such as .txt, .html or .sh, you need to master an editor. Run any of the following command as per need to open the editor and start writing.

```bash
$ sudo vi file.txt
$ sudo vim file.txt
$ sudo vi script.sh
$ sudo vim script.sh
```

Below is a brief overview of some of the most commonly used Vi commands:

**vi editing commands**

| **<mark>Command</mark>** | **<mark>Description</mark>** |
| --- | --- |
| `i` | Insert at cursor (goes into insert mode) |
| `a` | Write after cursor (goes into insert mode) |
| `A` | Write at the end of line (goes into insert mode) |
| `ESC` | Terminate insert mode |
| `u` | Undo last change |
| `U` | Undo all changes to the entire line |
| `o` | Open a new line (goes into insert mode) |
| `dd` | Delete line |
| `3dd` | Delete 3 lines |
| `D` | Delete contents of line after the cursor |
| `C` | Delete contents of a line after the cursor and insert new text. Press ESC key to end insertion. |
| `dw` | Delete word |
| `4dw` | Delete 4 words |
| `cw` | Change word |
| `x` | Delete character at the cursor |
| `r` | Replace character |
| `R` | Overwrite characters from cursor onward |
| `s` | Substitute one character under cursor continue to insert |
| `S` | Substitute entire line and begin to insert at the beginning of the line |
| `~` | Change case of individual character |

These are just a few of the powerful commands available in Vi. Remember, Vi operates in different modes, mainly command mode and insert mode. Command mode is where you enter all your commands, while insert mode is where you can insert text.

Mastering Vi can take some time, but once you get the hang of it, you'll find it an incredibly powerful tool for your text editing needs.