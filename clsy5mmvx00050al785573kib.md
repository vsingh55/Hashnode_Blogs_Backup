---
title: "2.3 Mysteries of IPv4: From Addresses to Encapsulation"
datePublished: Fri Feb 23 2024 04:30:55 GMT+0000 (Coordinated Universal Time)
cuid: clsy5mmvx00050al785573kib
slug: 23-mysteries-of-ipv4-from-addresses-to-encapsulation
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708410888457/df02c149-98da-4cc0-a79c-272b107362d5.png
tags: cloud, devops, networking, networking-for-beginners, 90daysofdevops, trainwithshubham

---

# Introduction:

In the vast landscape of networking, IPv4 reigns supreme as the backbone of communication, facilitating the seamless exchange of data across networks. Let's embark on a journey through the intricate realm of IPv4, exploring its addressing system, datagram structure, and the indispensable Address Resolution Protocol (ARP).

## Understanding IPv4 Addresses

At the heart of IPv4 lies its distinctive addressing scheme, characterized by 32-bit numbers arranged in four octets, each ranging from 0 to 255. This format, known as dotted decimal notation, provides a human-readable representation of IP addresses, such as the example 12.34.56.78.

### Allocation and Ownership

Unlike hardware vendors assigning addresses, IPv4 addresses are distributed to organizations in large blocks. Ownership of specific address ranges, such as those beginning with '9' belonging to IBM, underscores the hierarchical nature of IP address allocation.

### Device vs. Network

Contrary to popular belief, IP addresses are assigned to networks, not individual devices. Consequently, an IP address may change based on the network to which a device connects, highlighting the dynamic nature of network addressing.

### Static vs. Dynamic IP Assignment

IP addresses can be assigned either statically or dynamically. Static IPs are manually configured, typically reserved for servers and network devices, while dynamic IPs are automatically assigned, primarily used for client devices.

## Deconstructing the IPv4 Datagram

At the core of IPv4 communication lies the IP datagram, a packet at the network layer comprising a header and payload. Let's dissect the components of an IPv4 header to unravel its inner workings.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708403532689/8da58d90-b740-4711-9175-09c8bf5f9aec.png align="center")

### Header Structure

The IPv4 header encompasses essential information necessary for routing and delivering data across networks. Key fields include the version, header length, service type, total length, identification, flags, and fragmentation offset.

### IP Address Classes

IPv4 employs a hierarchical address class system, dividing the global IP address space into distinct classes, namely Class A, B, and C. Each class imposes specific constraints on network and host identification, shaping the allocation of IP addresses.

**IP Address Structure**:

Divided into two sections: Network ID and Host ID.

Example: 9.100.100.100 (owned by IBM).

The first octet is the Network ID, the following octets are the Host ID.

**Address Class System**:

Defines how global IP address space is divided.

Three primary types: Class A, B, and C.

**Address Classes**:

Class A: First octet for Network ID, last three for Host ID. Allows 16,777,216 addresses.

Class B: First two octets for Network ID, last two for Host ID.

Class C: First three octets for Network ID, final octet for Host ID. Allows 256 addresses.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708403445877/b29407e8-5264-4e4a-89ba-fbde3febb216.png align="center")

**Identifying Address Classes**:

By looking at the first bit(s) of an IP address: 0 for Class A, 10 for Class B, and 110 for Class C.

This correlates to dotted decimal notation: 0-127 for Class A, 128-191 for Class B, and 192-223 for Class C.

**Other Address Classes**:

Class D: Used for multicasting, starts with bits 1110 and decimal values between 224 and 239.

Class E: Unassigned and used for testing.

**Modern System**:

Classless Inter-Domain Routing (CIDR) mostly replaced the class system, but understanding the latter is still crucial for a well-rounded networking education.

### Address Resolution Protocol (ARP)

Address Resolution Protocol (ARP) serves as the vital link between MAC addresses at the data link layer and IP addresses at the network layer. By mapping IP addresses to corresponding MAC addresses, ARP facilitates the seamless transmission of data within local networks.

* ARP discovers the hardware address (MAC) of a node with a specific IP address.
    
* An IP datagram, once fully formed, is encapsulated in an Ethernet frame. The transmitting device uses the destination MAC address to complete the Ethernet frame header.
    
* Nearly all network-connected devices have a local ARP table, a list of IP addresses and their associated MAC addresses.
    
* If the destination IP address (e.g., 10.20.30.40) doesn't have an entry in the ARP table, the node wanting to send data broadcasts an ARP message to the MAC Broadcast address (all Fs).
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708403784287/d84b3505-3e36-4fe2-a741-d40b6a8f7cdf.png align="center")
    

* Broadcast ARP messages are delivered to all computers on the local network. The network interface assigned the IP 10.20.30.40 will receive this ARP broadcast and send back an ARP response containing its MAC address.
    
* The transmitting computer now knows the MAC address to put in the destination hardware address field and can send the Ethernet frame.
    
* The transmitting computer will likely store this IP address in its local ARP table to avoid future ARP broadcasts when communicating with this IP.
    
* ARP table entries typically expire after a short period to account for network changes.
    

## IP Address Lookup: Dispelling Myths

Despite misconceptions surrounding IP addresses and privacy, IP lookup tools offer limited insights into users' personal information. Instead, they provide valuable data for various applications, including law enforcement investigations, fraud prevention, and location verification in retail transactions.

## Conclusion

IPv4 stands as a cornerstone of modern networking, orchestrating the flow of data across vast digital landscapes. By unraveling its complexities, we gain a deeper understanding of the mechanisms driving communication in the digital age, paving the way for further innovation and connectivity.