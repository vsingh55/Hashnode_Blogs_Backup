---
title: "4.5 Mastering Packet Filtering with firewall-cmd in Linux"
datePublished: Sat Aug 26 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clsp0hr5o000308ky46y25fix
slug: 45-mastering-packet-filtering-with-firewall-cmd-in-linux
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708108263360/7e90f1c8-adde-4369-a544-0e2ebebc70f0.png
tags: linux, devops, networking, linux-for-beginners, 90daysofdevops, shubhamlondhe, trainwithshubham, powertocloud

---

# **Introduction:**

Packet filtering is a critical aspect of network security, allowing administrators to control incoming and outgoing network traffic based on predefined rules. In Linux, `firewall-cmd` serves as a powerful tool for implementing packet filtering using the `firewalld` daemon, which acts as a frontend for the iptables packet filtering system. In this comprehensive guide, we'll explore how to use `firewall-cmd` to manage packet filtering rules, configure zones, and ensure network security.

**Understanding Zones:** The `firewalld` daemon organizes packet filtering rules into groups called "zones." Each zone represents a level of trust in the networks your computer is connected to. By assigning network interfaces to specific zones, you can dictate the firewall behavior and determine which traffic is allowed or denied. Here are the predefined zones within `firewalld`:

* **drop**: Lowest level of trust, where all incoming connections are dropped without reply.
    
* **block**: Similar to drop, but incoming requests are rejected with an ICMP message.
    
* **public**: Represents public, untrusted networks, allowing selected incoming connections.
    
* **external**: Used for external networks, configured for NAT masquerading.
    
* **internal**: Used for internal networks with more trusted computers and additional services.
    
* **dmz**: For isolated computers in a DMZ, allowing certain incoming connections.
    
* **work** and **home**: For work and home environments, respectively, with varying levels of trust.
    
* **trusted**: Trusts all machines in the network, allowing the most open access.
    

**Implementing Packet Filtering:** `firewall-cmd` provides various commands to implement packet filtering rules and manage zones effectively:

1. **Viewing Available Zones:**
    
    ```bash
    firewall-cmd --get-zones
    ```
    
    This command lists all available zones that can be assigned to network interfaces.
    
2. **Setting Default Zone:**
    
    ```bash
    firewall-cmd --set-default-zone=external
    ```
    
    Use this command to set the default zone for the firewall. For example, setting it to `external` for networks accessed through external gateways.
    
3. **Adding Services and Ports:**
    
    ```bash
    sudo firewall-cmd --add-service=http
    sudo firewall-cmd --add-port=80/tcp
    ```
    
    These commands allow traffic to specific services (e.g., HTTP) or ports (e.g., port 80 for HTTP) to pass through the firewall. Adding `--permanent` makes the rule permanent.
    
4. **Filtering Traffic by Source:**
    
    ```bash
    sudo firewall-cmd --add-source=10.11.12.0/24 --zone=trusted
    ```
    
    This command allows traffic from a specific IP address range (`10.11.12.0/24`) in the `trusted` zone. Use `--remove-source` to remove the filter.
    
5. **Viewing Active Zones:**
    
    ```bash
    firewall-cmd --get-active-zones
    ```
    
    This command displays the zones actively filtering traffic, along with associated interfaces and sources.
    

**Rule Permanence:** By default, rules added or modified with `firewall-cmd` are applied immediately but are not persistent across reboots. To make rules permanent, use the `--permanent` option with the appropriate command.

```bash
#Check default zone of firewalld
firewall-cmd --get-default-zone
public

#Change default zone
firewall-cmd --set-default-zone=external

#Current Firewall rules
sudo firewall-cmd --list-all
	public (active)
	target: default
	icmp-block-inversion: no
	interfaces: enp0s3
	sources:
	services: cockpit dhcpv6-client ssh #(incoming connection
#for these services are allowed)

#Finding port number of a service
sudo firewall-cmd --info-service=cockpit
Ports: 9090/tcp

#Allow traffic to certain service (for ex http)
#tomake permanent use --permanent
sudo firewall-cmd --add-service=http
success
#OR adding by port
#to make permanent use --permanent
sudo firewall-cmd --add-port=80/tcp

#To remove a service from list of accepted connections
sudo firewall-cmd --remove-service=http
success
#or
sudo firewall-cmd --remove-port=80/tcp

#Instead of filtering traffic based on incoming ports
#we can have rules based on where the traffic is coming from
sudo firewall-cmd --add-source=10.11.12.0/24 --zone=trusted
success

#to remove our filter,based on IP addresses
sudo firewall-cmd --remove-source=10.11.12.0/24 --zone=trusted
success

#Zones which are actively filtering traffic
firewall-cmd --get-active-zones
public
interfaces: enp0s3
trusted
sources: 10.11.12.0/24
```

## **Conclusion:**

Implementing packet filtering with `firewall-cmd` is essential for ensuring network security and controlling traffic flow in Linux systems. By leveraging zones, services, ports, and source-based filtering, administrators can enforce access controls, protect against unauthorized access, and maintain a secure network environment. With the flexibility and power of `firewall-cmd`, you can effectively manage packet filtering rules and safeguard your Linux infrastructure against potential threats.