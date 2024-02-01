---
title: "M.2: Operation of Running Systems"
datePublished: Wed Aug 02 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cls2lwm1j000109lb9ndf9huj
slug: m2-operation-of-running-systems-1-1-1-1-1-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706755132213/e3898c0d-1611-4ec8-8939-6af891ebb40b.png
tags: linux, devops, linux-for-beginners, linux-commands, 90daysofdevops

---

Managing software on a Linux system is a crucial task for system administrators and users alike. DNF (Dandified YUM) is a package manager used in Fedora and other RPM-based Linux distributions. In this guide, we'll explore various commands and operations to efficiently handle software using `dnf`.

### Checking and Upgrading Software

* **Check for upgrades:**
    
    ```bash
    dnf check-upgrade
    ```
    
* **Upgrade installed packages:**
    
    ```bash
    sudo dnf upgrade
    ```
    

### Repository Management

* **Check repository list:**
    
    ```bash
    sudo dnf repolist
    ```
    
    or
    
    ```bash
    sudo dnf repolist -v
    ```
    
* **List all repositories (enabled and disabled):**
    
    ```bash
    sudo dnf repolist --all
    ```
    
* **Enable/Disable a repository:**
    
    ```bash
    sudo dnf config-manager --enable repo_name
    sudo dnf config-manager --disable repo_name
    ```
    
* **Add a new repository:**
    
    ```bash
    sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    ```
    
* **Remove a repository:**
    
    ```bash
    sudo rm /etc/yum.repos.d/docker-ce.repo
    ```
    

### Package Management

* **Search for packages:**
    
    ```bash
    sudo dnf search packageName
    ```
    
    For more accuracy:
    
    ```bash
    sudo dnf search 'packageName'
    ```
    
* **Get information about a package:**
    
    ```bash
    sudo dnf info packageName
    ```
    
* **Install a package:**
    
    ```bash
    sudo dnf install packageName
    ```
    
* **Reinstall a package:**
    
    ```bash
    sudo dnf reinstall packageName
    ```
    
* **Uninstall or remove a package:**
    
    ```bash
    sudo dnf remove packageName
    ```
    
* **Group Operations:**
    
    * **Install a group of packages:**
        
        ```bash
        sudo dnf group install 'packageName'
        ```
        
    * **List available groups:**
        
        ```bash
        sudo dnf group list
        sudo dnf group list --hidden
        ```
        
    * **Remove a group of packages:**
        
        ```bash
        sudo dnf group remove 'packageName'
        ```
        

### Local Package Operations

* **Install a local RPM file:**
    
    ```bash
    sudo dnf install ./packageName.rpm
    ```
    
* **Remove a locally installed package:**
    
    ```bash
    sudo dnf remove packageName.rpm
    ```
    

### Dependency Management

* **Remove unnecessary dependencies:**
    
    ```bash
    sudo dnf autoremove
    ```
    

### File Management

* **Identify file origin (for troubleshooting):**
    
    ```bash
    dnf provides /etc/anacrontab
    ```
    
    After identifying an issue with a file (e.g., bad edits), reinstall the package:
    
    ```bash
    sudo rm /etc/anacrontab
    dnf reinstall cronie-anacron
    ```
    
* **View all files in a package (installed or not):**
    
    ```bash
    dnf repoquery -l packageName
    ```
    

This comprehensive guide should empower users and administrators to efficiently manage software on their Linux systems using the versatile `dnf` package manager.