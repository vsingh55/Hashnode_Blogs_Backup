---
title: "3.1 Mastering Local Group Management in Linux: A Comprehensive Guide"
datePublished: Thu Aug 10 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsowblhc000b08jx68b9des7
slug: 31-mastering-local-group-management-in-linux-a-comprehensive-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708101803104/1cd36bae-aaea-4119-a8b0-f1caa84728b1.png
tags: cloud, linux, devops, linux-for-beginners, 90daysofdevops, trainwithshubham

---

# Introduction:

In the realm of Linux system administration, managing user groups and their memberships is an essential task. Whether you're setting up permissions for file access or configuring services, <mark>understanding how to create, delete, and modify local groups and group memberships is crucial</mark>. In this comprehensive guide, we'll explore various commands and techniques to streamline group management on Linux systems.

1. **Creating Local Groups (**`groupadd`):
    
    * Basic group creation:
        
        ```bash
        $ sudo groupadd marketing
        ```
        
    * Specifying a specific GID (Group ID) for the group:
        
        ```bash
        $ sudo groupadd -g 1001 sales
        ```
        
    * Creating a system group:
        
        ```bash
        $ sudo groupadd -r admin
        ```
        
    * Setting group password (rarely used):
        
        ```bash
        $ sudo groupadd -p 'encrypted_password' finance
        ```
        
    * Verifying group creation and details:
        
        ```bash
        $ sudo cat /etc/group
        $ sudo getent group marketing
        ```
        
2. **Managing Group Memberships (**`usermod`):
    
    * Adding a user to an existing group:
        
        ```bash
        $ sudo usermod -aG marketing john
        ```
        
    * Removing a user from a group:
        
        ```bash
        $ sudo deluser john marketing
        ```
        
    * Setting primary group for a user:
        
        ```bash
        $ sudo usermod -g sales sarah
        ```
        
    * Adding multiple supplementary groups for a user:
        
        ```bash
        $ sudo usermod -aG finance,hr sarah
        ```
        
    * Expiring user account:
        
        ```bash
        $ sudo usermod -e 2025-12-31 sarah
        ```
        
    
3. **Deleting Local Groups (**`groupdel`):
    
    * Deleting a group:
        
        ```bash
        $ sudo groupdel marketing
        ```
        
    * Deleting a group along with its files:
        
        ```bash
        $ sudo groupdel -f marketing
        ```
        
    * Verifying group deletion:
        
        ```bash
        $ sudo cat /etc/group
        ```
        
    * Deleting a system group:
        
        ```bash
        $ sudo groupdel -r admin
        ```
        
    * Forcing deletion of a non-existent group:
        
        ```json
        $ sudo groupdel -f non_existent_group
        ```
        
4. **Managing Users (**`useradd`):
    
    * Adding a new user with default settings:
        
        ```bash
        $ sudo useradd john
        ```
        
    * Specifying a custom home directory for the user:
        
        ```bash
        $ sudo useradd -m -d /home/john_custom john
        ```
        
    * Setting a specific UID (User ID) for the user:
        
        ```bash
        $ sudo useradd -u 1001 john
        ```
        
    * Specifying login shell for the user:
        
        ```bash
        $ sudo useradd -s /bin/bash john
        ```
        
    * Creating a user with specific primary group:
        
        ```bash
        $ sudo useradd -g sales john
        ```
        
5. **Modifying User Attributes (**`usermod`):
    
    * Changing username:
        
        ```bash
        $ sudo usermod -l newname john
        ```
        
    * Changing home directory:
        
        ```bash
        $ sudo usermod -d /home/newhome john
        ```
        
    * Locking user account:
        
        ```bash
        $ sudo usermod -L john
        ```
        
    * Unlocking user account:
        
        ```bash
        $ sudo usermod -U john
        ```
        
    * Modifying expiration date of user account:
        
        ```bash
        $ sudo usermod -e 2023-06-30 john
        ```
        

```bash
#Take a look to know more about cmd's

sudo useradd john

#default setting of useradd in system
useradd -D #or (--default)
GROUP=100
HOME=/home
INACTIVE=-1
EXPIRE=
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=yes

#other defaults
cat /etc/login.defs

#to set a password for users
sudo passwd john
Changing password for user john.
New password:

#deleting the user (home dir will remain)
sudo userdel john
#remove user john completely with home dir
sudo userdel -r john #or (--remove)

#changing shell and directory (other than defaults)
#(-s -> --shell) (-d -> --home-dir)
sudo useradd -s /bin/othershell -d /home/otherdirectory/ john

#changing only shell
sudo useradd -s /bin/othershell john

#adding user without any home dir
#(-M -> --no-create-home)
sudo useradd -M john

#adding user to multiple groups
sudo useradd -G grp1 grp2 grp3 john
#add existing user to groups
sudo useradd -aG grp2 grp3 jane

#Create a User with an Account Expiry Date (-e)
useradd -e 2021-08-27 aparna
#Create a User with Password Expiry Date (-f)
useradd -e 2014-04-27 -f 45 mansi

#information about users and groups are stored in /etc/passwd file
cat /etc/passwd
john:x:1001:1001::/home/otherdirectory/:/bin/othershell
#username:encryptedPasswd:UID:GID:UserDescription:HomeDir:DefShell

#creating user with different UID
sudo useradd -u 1100 smith #or (--uid)

#If we want to see what username and group owns files or directories
ls -l /home/
drwx------. 16 aaron aaron 4096 Dec 16 10:01 aaron
drwx------. 4 jane jane 113 Dec 16 13:00 jane
drwx------. 3 john john 78 Oct 19 19:39 john
drwx------. 3 smith smith 78 Oct 19 19:39 smith

#see the numeric IDs of the user and group owners
ls -ln /home/
drwx------. 16 1000 1000 4096 Dec 16 10:01 aaron
drwx------. 4 1001 1001 13 Dec 16 13:00 jane
drwx------. 3 1002 1002 78 Oct 19 19:39 john
drwx------. 3 1100 1100 78 Oct 19 19:39 smith

#command assigns a user to a group with some security criteria
#Add user to Group
#(--a -> --add)
sudo gpasswd --a john developers

#Removing user from a group
#(-d -> --delete)
sudo gpasswd -d john developers

#User can have only one Primary Group and multiplle secondary groups

#Changing users Primary or Login group
#(-g -> --gid)
sudo usermod -g developers john


#check UID,GID, groups and other info of curr user
id
uid=1000(aaron) gid=1000(aaron) groups=1000(aaron),
10(wheel),1005(family) 
context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

#check current username
whoami
aaron

#creating system accounts using --system
#numeric ID of system acc are less than 1000
#created for programs hence no home dir created
#usually daemons use system accounts
sudo useradd --system sysacc

#changing home dir of existing user
#(--home -> -d) (-m -> --move-home)
sudo usermod -d /home/otherdirectory -m john

#Changing username of an existing user (john to jane)
#(-l -> --login)
sudo usermod -l jane john

#Change existing users login shell
#(-s - > -shell)
sudo usermod -s /bin/othershell jane

#Disabling account without deleting it
#(-L -> --lock)
sudo usermod -L jane

#Unlocks the account
#(-U -> --unlock)
sudo usermod -U jane

#Setting a date for user acc to expire
#(-e -> --expiredate)
sudo usermod -e 2021-12-10 jane
# Date format: YEAR-MONTH-DAY

#Remove expiration date using empty date
sudo usermod -e "" jane

#chage = change age
#To immediately set the password as expired
#it forces user to change the password
#(-d -> --lastdate)
sudo chage -d 0 jane
#Cancelling or unexpire the password
sudo chage -d -1 jane

#Changing user password every 30 days
#(-M -> --maxdays)
sudo chage -M 30 jane
#Password never expires
sudo chage -M -1 jane

#To see when a users password expires
#(-l -> --list)
sudo chage -l jane

#Rename a group (programmers to developers)
#(-n -> --new-name)
sudo groupmod -n developers programmers

#Deleting a group
sudo groupdel programmers
```

## Conclusion:

Effective management of local groups and group memberships is fundamental for maintaining security and access control on Linux systems. With the commands discussed in this guide, you can confidently create, delete, and modify local groups, as well as manage user memberships with precision and ease. Experiment with these commands in your Linux environment to gain proficiency and streamline your system administration tasks.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">To know more about Linux, follow <mark>Linux series</mark> of <strong><mark>PowerToCloud</mark></strong><mark>.</mark></div>
</div>