---
title: "5.6 Enhancing Security with SSH: Configuring SSH Servers and Clients"
datePublished: Wed Sep 13 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsp1qvnk00040ajldnpfgusn
slug: 56-enhancing-security-with-ssh-configuring-ssh-servers-and-clients
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708111785225/376c7a98-893a-4527-b0f8-eb1516875797.png
tags: linux, cloud-computing, devops, linux-for-beginners, 90daysofdevops, shubhamlondhe, trainwithshubham, lfcs, powertocloud

---

# **Introduction:**

Secure Shell (SSH) is a cryptographic network protocol that facilitates secure communication between two systems over an insecure network. It provides a secure avenue for accessing and managing remote machines, making it a crucial component of modern computing environments. This guide outlines the process of configuring SSH servers and clients (`sshd.service`), focusing on strengthening security measures and optimizing performance.

**Configuring SSH Servers (**`sshd_config`): SSH server configuration is essential for controlling access and ensuring secure communication. Follow these steps to configure the `sshd_config` file:

1. **Accessing Configuration Files:** Locate the SSH server configuration file at `/etc/ssh/sshd_config` and open it for editing.
    
    ```bash
    sudo vi /etc/ssh/sshd_config
    ```
    
2. **Key Settings:**
    
    * **Port Configuration:** Specify the port number on which the SSH daemon listens for incoming connections. The default port is 22.
        
        ```bash
        Port 22
        ```
        
    * **AddressFamily:** Define which address family should be used by sshd. Options include `any`, `inet` (IPv4 only), or `inet6` (IPv6 only).
        
        ```bash
        AddressFamily yes
        ListenAddress 0.0.0.0
        ```
        
    * **PermitRootLogin:** Determine whether root can log in using SSH. Options include `yes`, `prohibit-password`, `forced-commands-only`, or `no`.
        
        ```bash
        PermitRootLogin yes
        ```
        
    * **PasswordAuthentication:** Control whether password authentication is allowed. Set to `no` to enforce SSH key-based authentication.
        
        ```bash
        PasswordAuthentication no
        ```
        
    * **X11Forwarding:** Specify whether X11 forwarding is permitted for running graphical applications remotely.
        
        ```bash
        X11Forwarding yes
        ```
        
3. **Reloading Configuration Changes:** After modifying the configuration file, reload the SSH daemon to apply the changes.
    
    ```bash
    sudo systemctl reload sshd.service
    ```
    

**SSH Key Management:** SSH keys provide a secure method for authenticating users and establishing secure connections. Follow these steps to manage SSH keys:

1. **Generating SSH Keys:** Use `ssh-keygen` to generate a public/private RSA key pair.
    
    ```bash
    ssh-keygen
    ```
    
2. **Copying Public Key to Remote Server:** Use `ssh-copy-id` to securely copy the public key to the remote server, enabling passwordless authentication.
    
    ```bash
    ssh-copy-id aaron@10.9.8.0
    ```
    
3. **Removing Existing Keys:** If needed, remove existing fingerprints or keys using `ssh-keygen -R` for a specific hostname or IP address, or remove all fingerprints by deleting the `known_hosts` file.
    
    ```bash
    ssh-keygen -R hostname/IPAddress
    rm known_hosts
    ```
    

## **Conclusion:**

Configuring SSH servers and clients is essential for establishing secure and efficient communication channels in computing environments. By following the steps outlined in this guide, administrators can enhance security by enforcing key-based authentication, restricting root login, and controlling access to SSH services. Additionally, proper SSH key management practices ensure secure authentication and streamlined remote access to servers and services. With these configurations in place, organizations can bolster their security posture and mitigate potential risks associated with unauthorized access and data breaches.