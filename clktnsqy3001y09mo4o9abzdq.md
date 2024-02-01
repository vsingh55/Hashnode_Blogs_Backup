---
title: "M.1: Essential Commands"
datePublished: Thu Jul 13 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clktnsqy3001y09mo4o9abzdq
slug: basic-linux-commands
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706784548014/6ea51545-3313-4262-86f6-79ffa7f40f2d.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1706737406294/4d317c87-bd3a-457e-be88-85ec44c3d80e.png
tags: linux-for-beginners, devops-articles, linux-commands, 90daysofdevops

---

Here's a basic introduction to some common Linux commands:

1. `pwd`: Stands for "Print Working Directory." It shows you the current directory you are in.
    
2. `ls`: Lists files and directories in the current directory.
    
    * `ls -a`: Lists all files and directories, including hidden files.
        
    * `ls -l`: Displays detailed information about files and directories, such as permissions, owner, size, and modification date.
        
    * `ls -h`: Shows file sizes in a human-readable format (e.g., KB, MB, GB).
        
3. `cd`: Stands for "Change Directory." It allows you to navigate between directories.
    
    * `cd <directory_name>`: Moves you to the specified directory.
        
    * `cd ..`: Moves you up one level to the parent directory.
        
    * `cd ~`: Takes you to your home directory.
        
4. `mkdir`: Stands for "Make Directory." It is used to create new directories.
    
    * `mkdir <directory_name>`: Creates a new directory with the given name.
        
5. `rmdir`: Stands for "Remove Directory." It is used to remove empty directories.
    
    * `rmdir <directory_name>`: Removes the specified empty directory.
        
6. `rm`: Stands for "Remove." It is used to delete files and directories.
    
    * `rm <file_name>`: Removes the specified file.
        
    * `rm -r <directory_name>`: Removes a directory and its contents recursively (use with caution).
        
7. `cp`: Stands for "Copy." It is used to copy files or directories.
    
    * `cp <source_file> <destination>`: Copies the source file to the destination.
        
    * `cp -r <source_directory> <destination>`: Copies a directory and its contents recursively.
        
8. `mv`: Stands for "Move." It is used to move or rename files and directories.
    
    * `mv <source> <destination>`: Moves the source file or directory to the destination.
        
    * `mv <old_name> <new_name>`: Renames a file or directory.
        
9. `cat`: Stands for "Concatenate." It is used to display the contents of a file.
    
    * `cat <file_name>`: Displays the content of the specified file.
        
10. `echo`: Prints a message or value to the terminal.
    
    * `echo <message>`: Displays the message on the terminal.
        
11. `man`: Stands for "Manual." It is used to display the manual page for a command.
    
    * `man <command>`: Shows the manual for the specified command. Press 'q' to exit the manual page.
        

| **<mark>Command</mark>** | **<mark>Description</mark>** |
| --- | --- |
| `ls` | Lists all files and directories in the present working directory |
| `ls -R` | Lists files in sub-directories as well |
| `ls -a` | Lists hidden files as well |
| `ls -al` | Lists files and directories with detailed information like permissions,size, owner, etc. |
| `cd or cd ~` | Navigate to HOME directory |
| `cd ..` | Move one level up |
| `cd` | To change to a particular directory |
| `cd /` | Move to the root directory |
| `cat > filename` | Creates a new file |
| `cat filename` | Displays the file content |
| `cat file1 file2 > file3` | Joins two files (file1, file2) and stores the output in a new file (file3) |
| `mv file "new file path"` | Moves the files to the new location |
| `mv filename new_file_name` | Renames the file to a new filename |
| `sudo` | Allows regular users to run programs with the security privileges of the superuser or root |
| `rm filename` | Deletes a file |
| `man` | Gives help information on a command |
| `history` | Gives a list of all past commands typed in the current terminal session |
| `clear` | Clears the terminal |
| `mkdir directoryname` | Creates a new directory in the present working directory or a at the specified path |
| `rmdir` | Deletes a directory |
| `mv` | Renames a directory |
| `pr -x` | Divides the file into x columns |
| `pr -h` | Assigns a header to the file |
| `pr -n` | Denotes the file with Line Numbers |
| `lp -nc , lpr c` | Prints “c” copies of the File |
| `lp-d lp-P` | Specifies name of the printer |
| `apt-get` | Command used to install and update packages |
| `mail -s 'subject'   -c 'cc-address'   -b 'bcc-address'   'to-address'   ` | Command to send email |
| `mail -s "Subject"   to-address < Filename   ` | Command to send email with attachment |

These are just some of the basic Linux commands to get you started. As you become more familiar with the Linux environment, you'll find many more commands and options to explore!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690976246671/4d7041ee-ba8e-4a51-995f-04e030a7be51.png align="right")

[**Introduction to Devops**](https://hashnode.com/post/clkso3bpx000009k275r554xg)

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Stay updated for more such bloggs. Happy Learning</div>
</div>