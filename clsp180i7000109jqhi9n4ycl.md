---
title: "5.2 Streamlining DNS Resolution with BIND: Configuring a Caching DNS Server"
datePublished: Mon Sep 04 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsp180i7000109jqhi9n4ycl
slug: 52-streamlining-dns-resolution-with-bind-configuring-a-caching-dns-server
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708110825245/0e772b6a-b846-4e70-9e89-6ccdfb3457dc.png
tags: linux, cloud-computing, devops, linux-for-beginners, 90daysofdevops, shubhamlondhe, trainwithshubham

---

# **Introduction:**

The Domain Name System (DNS) serves as the backbone of the Internet, translating human-readable domain names into their corresponding IP addresses. By configuring a caching DNS server using BIND (Berkeley Internet Name Domain), administrators can optimize DNS resolution, enhance network performance, and reduce latency. In this guide, we'll delve into the process of setting up a caching DNS server with BIND on Linux-based systems, exploring its benefits and configuration steps.

**Understanding DNS Cache and BIND:** DNS caching, or resolver cache, temporarily stores DNS records of previously visited domain names on a device, facilitating quicker access to frequently accessed sites. BIND, a widely used nameserver service, is responsible for performing domain name-to-IP address conversion and managing DNS resolution on Linux-based DNS servers.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708110921460/065b60ea-73dd-4f10-ac18-795c4dc3a70e.png align="center")

**Configuration Steps with** `named.conf`: Configuring a caching DNS server with BIND involves editing the `/etc/named.conf` file to define server settings and permissions. Here's a breakdown of the configuration steps:

1. **Install BIND Packages:**
    
    ```bash
    sudo dnf install bind bind-utils
    ```
    
    Begin by installing the BIND package and associated utilities on your Linux system.
    
2. **Configure** `/etc/named.conf`:
    
    * Adjust the `listen-on` directive to specify the IP addresses on which BIND should listen for DNS queries.
        
    * Set the `allow-query` directive to determine which hosts or networks are allowed to query the DNS server.
        
    * Enable recursion to allow BIND to fetch DNS data from external servers when necessary.
        
3. **Start and Enable BIND Service:**
    
    ```bash
    sudo systemctl start named.service
    sudo systemctl enable named.service
    ```
    
    Initiate the BIND service and ensure it starts automatically at system boot.
    
4. **Configure Firewall Rules:**
    
    ```bash
    sudo firewall-cmd --add-service=dns --permanent
    sudo firewall-cmd --reload
    ```
    
    Add firewall rules to allow incoming DNS traffic to the server, ensuring connectivity from external networks.
    
5. **Testing BIND Configuration:**
    
    ```bash
    dig @127.0.0.1 google.com
    ```
    
    Verify the functionality of the caching DNS server by querying a domain name. Subsequent queries should demonstrate improved response times due to cached DNS records.
    

```bash
#1st check current IP of system
ip a
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
UP group default qlen 1000
link/ether 08:00:27:6b:d7:87 brd ff:ff:ff:ff:ff:ff
inet 192.168.0.17/24 brd 192.168.0.255 scope global dynamic
noprefixroute enp0s3

#192.168.0.17 – DNS server address
#any – matches every IP address
sudo vim /etc/named.conf
listen-on port 53 { 127.0.0.1; };
#port 53 , 127.0.0.1 : bind is configured 
#to listen on port 53 and this IP. 
#Its called a loop back addr i.e. 
#it will only accept incoming connections from the programs 
#running on the same OS but not from the outside world

#To accept connections from other servers in our internal network, 
#change the line and tell bind to listen 
#on the IP address it has on that network.
listen-on port 53 { 127.0.0.1; 192.168.0.17; };

#If we want to accept incoming connections 
#on any network attached to our server
listen-on port 53 { any; };

#BIND will only allow local hosts to query, 
#and ask for information from its DNS database
#That means that only programs running on the 
#same operating system can request DNS data from BIND.
allow-query { localhost; };

#If we also want to let devices on our 192.168.0.0 network, 
#add the following on config line
allow-query { localhost; 192.168.0.0/24; };

#answer to queries coming from any IP address
allow-query { any; };

#Another important config option is the recursion option
#This lets our BIND server get DNS data from 
#other DNS servers on the internet,
#when it does not have it available in its cache
recursion yes;

#Start and enable Bind
sudo systemctl start named.service
sudo systemctl enable named.service

#by default, firewall blocks any incoming connection
#Add new rule If we want external computers
#to connect to our DNS daemon
sudo firewall-cmd --add-service=dns
#making rule permanent
sudo firewall-cmd --add-service=dns --permanent

#test if Bind is running correctly
dig @127.0.0.1 google.com
;; Query time: 82 msec
#data is not cached so it took 82msec
#but run it again and data is already cached
dig @127.0.0.1 google.com
;; Query time: 0 msec
```

## **Conclusion:**

Configuring a caching DNS server with BIND offers significant advantages, including enhanced network performance, reduced latency, and improved DNS resolution. By following the steps outlined in this guide, administrators can deploy a robust DNS caching solution on Linux-based systems, leveraging BIND's capabilities to optimize DNS query processing and enhance overall network efficiency.