---
title: "4.3 Automating Network Services in Linux: A Guide to nmcli"
datePublished: Thu Aug 24 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsozpm8j000209jm7ces7s1q
slug: 43-automating-network-services-in-linux-a-guide-to-nmcli
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708108234942/975e68f4-f085-477b-971a-f20eac691f24.png
tags: cloud, linux, devops, networking, linux-for-beginners, 90daysofdevops, shubhamlondhe, trainwithshubham, lfcs

---

# **Introduction:**

Ensuring that essential network services start automatically at boot is crucial for maintaining connectivity and accessibility in Linux systems. `nmcli`, a command-line tool for managing NetworkManager, provides a straightforward way to configure network services to start automatically at boot. In this comprehensive guide, we'll explore how to use `nmcli` to enable network services, check their status, and ensure seamless connectivity after system reboot.

**Checking and Starting NetworkManager Service:** Before configuring network services to start at boot, it's essential to ensure that the NetworkManager service is running:

1. **Checking Service Status (**`systemctl status NetworkManager.service`):
    
    ```bash
    systemctl status NetworkManager.service
    ```
    
    This command checks the status of the NetworkManager service to determine if it's active or inactive.
    
2. **Starting the Service (**`sudo systemctl start NetworkManager.service`):
    
    ```bash
    sudo systemctl start NetworkManager.service
    ```
    
    If the NetworkManager service is inactive, use this command to start it manually.
    
3. **Enabling Service at Boot (**`sudo systemctl enable NetworkManager.service`):
    
    ```bash
    sudo systemctl enable NetworkManager.service
    ```
    
    This command enables the NetworkManager service to start automatically at boot, ensuring continuous network connectivity.
    

**Configuring Network Services to Start Automatically:** `nmcli` provides a convenient way to configure network services to start automatically at boot:

1. **Listing Configured Connections (**`nmcli connection show`):
    
    ```bash
    nmcli connection show
    ```
    
    Use this command to list all connections configured for the system, along with their names, UUIDs, types, and devices.
    
2. **Identifying the Device Name:** From the list of connections, identify the device name associated with the network interface you want to configure.
    
3. **Modifying Connection Settings (**`sudo nmcli connection modify <device_name> autocnnect yes`):
    
    ```bash
    sudo nmcli connection modify enp0s3 autocnnect yes
    ```
    
    Replace `<device_name>` with the name of the network interface you identified. This command modifies the connection settings to enable automatic connection at boot.
    

## **Conclusion:**

Automating network services to start at boot is essential for maintaining uninterrupted connectivity and accessibility in Linux systems. With `nmcli`, managing NetworkManager connections and configuring them to start automatically is straightforward and efficient. By following the steps outlined in this guide, you can ensure that essential network services are enabled and ready to go after each system reboot, minimizing downtime and enhancing productivity in your Linux environment.