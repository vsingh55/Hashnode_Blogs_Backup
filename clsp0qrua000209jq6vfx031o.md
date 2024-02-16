---
title: "4.6 Navigating Static IP Routing in Linux with ip route and nmtui"
datePublished: Wed Aug 30 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsp0qrua000209jq6vfx031o
slug: 46-navigating-static-ip-routing-in-linux-with-ip-route-and-nmtui
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708109970017/6e090e20-769e-4040-a525-3be31213db9f.png
tags: linux, cloud-computing, devops, networking, linux-for-beginners, 90daysofdevops, shubhamlondhe, trainwithshubham, powertocloud

---

# **Introduction:**

In networking, static routing plays a vital role in directing IP traffic efficiently from source to destination. Unlike dynamic routing, where routing tables are automatically updated using routing protocols, static routing requires manual configuration by network administrators. In this guide, we'll explore how to statically route IP traffic in Linux using commands like `ip route` and graphical tools like `nmtui`, understanding the concepts and commands involved.

**Understanding Static Routing:** Static routing involves manually configuring routing tables on network devices to define the path that data packets must follow to reach their destination. Unlike dynamic routing, where routers communicate with each other to update routing tables, static routes remain unchanged unless modified by the administrator. This method is ideal for small, stable networks with predictable traffic patterns.

**Using** `ip route` Command: The `ip route` command is a powerful tool for managing static routes in the routing table of Linux systems. Here's how to use it to add, delete, and manage static routes:

1. **Adding Static Routes:**
    
    ```bash
    sudo ip route add 192.168.0.0/24 via 10.0.0.100
    ```
    
    This command adds a static route to network `192.168.0.0/24` via the router at `10.0.0.100`.
    
2. **Specifying Interface:**
    
    ```bash
    sudo ip route add 192.168.0.0/24 via 10.11.12.100 dev enp0s3
    ```
    
    If multiple network interfaces are present, specify the interface (`enp0s3` in this example) where the route should be applied.
    
3. **Deleting Routes:**
    
    ```bash
    sudo ip route del 192.168.0.0/24
    ```
    
    Use this command to delete a previously configured static route.
    
4. **Adding Default Route:**
    
    ```bash
    sudo ip route add default via 10.0.0.100
    ```
    
    This command adds a default route, directing all traffic to the specified router.
    
5. **Making Routes Permanent:**
    
    ```bash
    sudo nmcli connection modify enp0s3 +ipv4.routes "192.168.0.0/24 10.0.0.100"
    ```
    
    To persist routes after boot, use this command to modify network manager connections (`enp0s3` in this example) and add static routes.
    

```bash
#Let’s say you have two different networks, 
#each with its router: the network 192.168.0.x and network 10.0.0.100. 
#The 10.0.0.100 (network A) and 192.168.0.0 (network B) want to connect between them
sudo ip route add 192.168.0.0/24 via 10.0.0.100

#There are multiple network-based interfaces on the system
#so also specify the interface name where we want to apply this route
sudo ip route add 192.168.0.0/24 via 10.11.12.100 dev enp0s3

#Delete previous route
sudo ip route del 192.168.0.0/24

#Adding a default route
sudo ip route add default via 10.0.0.100

#Deleting default route
sudo ip route del default via 10.0.0.100

#Make route commands permanent or persist after boot
#1st check what network manager cconnection are active
nmcli connection show
NAME                 UUID                             TYPE             DEVICE
enp0s3    fadff03a-8b55-4b81-b582-3e84b50fa8f5        ethernet         enp0s3
#Makig permanent
sudo nmcli connection modify enp0s3 +ipv4.routes "192.168.0.0/24 10.0.0.100"

#To immediatly apply new routes
sudo nmcli device reapply enp0s3
Connection successfully reapplied to device 'enp0s3'

#Remove the route
sudo nmcli connection modify enp0s3 -ipv4.routes "192.168.0.0/24 10.0.0.100"
```

**Using** `nmtui` for Configuration: `nmtui` (Network Manager Text User Interface) provides a user-friendly way to configure network settings, including static routes. Here's how to use it:

1. **Accessing** `nmtui`:
    
    ```bash
    sudo nmtui
    ```
    
    Run this command to access the `nmtui` interface.
    
2. **Navigating to Static Routes:** Use the arrow keys to navigate to the "Edit a connection" option, select the desired connection, and navigate to "IPv4 Routes" to add or remove static routes.
    
3. **Applying Changes:** After configuring static routes, apply the changes and reapply the connection to immediately apply the new routes.
    

## **Conclusion:**

Statically routing IP traffic in Linux involves manually configuring routing tables to define the path data packets should follow. By using commands like `ip route` and graphical tools like `nmtui`, administrators can efficiently manage static routes, ensuring optimal network performance and connectivity. Whether adding, deleting, or persisting routes, understanding the concepts and commands involved is essential for effective network administration in Linux environments.