---
title: "3.2 Mastering System-Wide Environment Profiles on Linux: A Comprehensive Guide"
datePublished: Sat Aug 12 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsox9ujw000p08l384apcsc5
slug: 32-mastering-system-wide-environment-profiles-on-linux-a-comprehensive-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708103800923/c9e6329f-4643-49d0-8e99-5bae55a4c4ee.png
tags: linux, devops, linux-for-beginners, shell-script, shubhamlondhe, trainwithshubham, linuxfordevops, powertocloud

---

# Introduction:

In the dynamic landscape of Linux system administration, managing system-wide environment profiles is a fundamental task. From setting global environment variables to monitoring command history, understanding how to effectively manage these profiles is essential for maintaining system integrity and optimizing workflow. In this detailed blog post, we'll explore the commands `printenv` and `history`, providing insights and examples on how to master system-wide environment profiles on Linux.

## **Printing Environment Variables (**`printenv`):

### **Printing all environment variables:**

* ```bash
    $ printenv
    ```
    
* **Printing the value of a specific environment variable:**
    
    ```bash
    $ printenv PATH
    ```
    
* **Printing the values of multiple environment variables:**
    
    ```bash
    $ printenv USER HOME
    ```
    
* **Using** `grep` to filter specific environment variables:
    
    ```bash
    $ printenv | grep -i term
    ```
    
* **Redirecting output to a file:**
    
    ```bash
    $ printenv > environment_variables.txt
    ```
    

### **Viewing Command History (**`history`):

1. * **Displaying command history:**
        
        ```bash
        $ history
        ```
        
    * **Limiting the number of displayed commands:**
        
        ```bash
        $ history 10
        ```
        
    * **Clearing command history:**
        
        ```bash
        $ history -c
        ```
        
    * **Searching command history:**
        
        ```bash
        $ history | grep ssh
        ```
        
    * **Using** `!` to execute a specific command from history:
        
        ```bash
        $ !100
        ```
        
2. **Saving Command History to a File (**`history`):
    
    * **Saving command history to a file:**
        
        ```bash
        $ history > command_history.txt
        ```
        
    * **Appending command history to an existing file:**
        
        ```bash
        $ history >> command_history.txt
        ```
        
    * **Filtering command history before saving:**
        
        ```bash
        $ history | grep ssh > ssh_commands.txt
        ```
        
    * **Excluding line numbers from command history:**
        
        ```bash
        $ history -w command_history.txt
        ```
        
    * **Exporting command history for use in another session:**
        
        ```bash
        $ history -a
        ```
        
3. **Customizing Environment Profiles:**
    
    * **Editing the** `.bashrc` file to set environment variables:
        
        ```bash
        $ nano ~/.bashrc
        ```
        
    * **Adding a new environment variable:**
        
        ```bash
        export MY_VAR="value"
        ```
        
    * **Sourcing the updated** `.bashrc` file to apply changes:
        
        ```bash
        $ source ~/.bashrc
        ```
        
    * **Creating system-wide environment variables in** `/etc/environment`:
        
        ```bash
        $ sudo nano /etc/environment
        ```
        
    * **Reloading environment variables without rebooting:**
        
        ```bash
        $ source /etc/environment
        ```
        
4. **Managing User-Specific Environment Profiles:**
    
    * **Editing the** `.bash_profile` file for user-specific environment settings:
        
        ```bash
        $ nano ~/.bash_profile
        ```
        
    * **Exporting environment variables in the** `.bash_profile` file:
        
        ```bash
        export PATH="$PATH:/usr/local/bin"
        ```
        
    * **Reloading the** `.bash_profile` without logging out:
        
        ```bash
        $ source ~/.bash_profile
        ```
        
    * **Creating aliases for commonly used commands in** `.bashrc`:
        
        ```bash
        alias ll='ls -alF'
        ```
        

Go through these cmd's to get grep on managing environment variables and history.

```bash
#Users current evnvironment
printenv
#or
env
PATH=/home/aaron/.local/bin:/home/aaron/bin:/usr/local/bin:/usr/local/sbin
:/usr/bin:/usr/sbin
HISTSIZE=1000
GJS_DEBUG_TOPICS=JS ERROR;JS LOG
SESSION_MANAGER=local/unix:@/tmp/.ICE-unix/2260,unix/unix:/tmp/.ICE-
unix/2260

#Changing Env Variable HISTSIZE value
#to persist after reboot edit in .bashrc file
HISTSIZE=2000

#Add custom environment variables
cat .bashrc
# .bashrc
# Source global definitions
if [ -f /etc/bashrc ]; then
. /etc/bashrc
fi
# User specific environment
if ! [[ "$PATH" =~ "$HOME/.local/bin:$HOME/bin:" ]]
then
PATH="$HOME/.local/bin:$HOME/bin:$PATH"
fi

#Adding env var for all users
sudo vi /etc/environment
#Create a template for the users' environment
#Example:Inform all new users about some default policy
#Create ReadMe in /etc/skel dir
sudo vim /etc/skel/README
	Please don't run CPU-intensive
	processes between 8AM and 10PM.

#Adding a Env Variable just for a specific User
sudo vim /home/trinity/.bashrc
	PATH="$HOME/.local/bin:$HOME/bin:$PATH"
	#add /opt/bin	to path
	PATH="$HOME/.local/bin:$HOME/bin:/opt/bin:$PATH"

echo $PATH
/home/trinity/.local/bin:/home/trinity/bin:/usr/local/bin:/usr/bin:/usr
/local/sbin:/usr/sbin
```

## Conclusion:

Effective management of system-wide environment profiles is crucial for optimizing workflows and ensuring system stability on Linux. By mastering commands like `printenv` and `history`, along with various customization techniques, administrators can gain greater control over environment variables and command histories. Experiment with these commands and examples in your Linux environment to streamline your system administration tasks and enhance productivity.