---
title: "2.4 Subnetting and CIDR"
datePublished: Sat Feb 24 2024 04:30:40 GMT+0000 (Coordinated Universal Time)
cuid: clszl267f000b0alhf43scmh9
slug: 24-subnetting-and-cidr
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708410970990/da97541e-dfdd-4c70-b386-af6d69b43e26.png
tags: cloud, devops, networking, networking-for-beginners, 90daysofdevops, trainwithshubham, powertocloud

---

# Introduction:

In the vast realm of networking, the concepts of subnetting and Classless Inter-Domain Routing (CIDR) serve as indispensable tools for efficiently managing and organizing large networks. Let's embark on a journey to explore these fundamental principles and understand their significance in modern networking architectures.

## Subnetting: Bridging Networks with Precision

Subnetting, the practice of dividing a large network into smaller, individual subnetworks or subnets, plays a pivotal role in network management. By segmenting networks into manageable units, subnetting enhances scalability, efficiency, and security, catering to the diverse needs of modern organizations.

* **Cider Technique:** An advanced method offering more flexibility than standard subnetting.
    
* **Binary Math Techniques:** Important for understanding the workings of subnetting.
    
* **Incorrect Subnetting:** A common issue faced by IT support specialists, emphasizing the need for a robust understanding of subnetting.
    
* **Address Classes & Global IP Space:** Address classes help segment the total global IP space into distinct networks.
    
* **Gateway Routers:** These serve as the entry and exit points to a network and handle data routing to the correct system by looking at the host ID.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708405065715/e9ae2e73-2030-497c-a957-b6edfbb16a2f.png align="center")
    

* **Subnetting Application:** Subnetting allows for the division of large networks, each with its own gateway router, into manageable, smaller networks.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708405118942/55d617df-2fd1-4494-8ccf-8d20d9949569.png align="center")
    

### The CIDR Technique

An advanced method that offers greater flexibility than traditional subnetting, CIDR (Classless Inter-Domain Routing) enables precise allocation of IP addresses, optimizing address space utilization and simplifying network administration.

### Address Classes and Subnet Masks

Address classes delineate the global IP space into distinct networks, guiding the allocation of IP addresses. Subnet masks, represented as 32-bit numbers, define the size and structure of subnets, enabling computers to accurately identify network and host IDs for efficient data routing.

### **<mark>Subnet Masks</mark>**

* **Network IDs** and **Host IDs** are used to identify networks and individual hosts respectively.
    
* **Subnet ID** is a concept introduced for further division. In subnetting, some bits usually comprising the host ID are used for the subnet ID.
    
* A single 32-bit IP address can represent all three IDs, thus ensuring accurate delivery across different networks.
    
* Core routers at the Internet level only care about the network ID, while gateway routers use other information for delivery to the destination machine.
    
* **Subnet IDs** are calculated using a **subnet mask**, another 32-bit number usually written as four octets in decimal.
    
* A subnet mask has two sections: the beginning part with a string of ones is the mask itself, followed by zeros. The one-part tells us what to ignore when computing a host ID while the zeros part tells us what to keep.
    
* The size of a subnet is defined entirely by its subnet mask. For example, with a subnet mask of 255.255.255.0, only the last octet is available for host IDs.
    
* Generally, a subnet usually contains two less than the total number of host IDs available.
    
* The entire IP and subnet mask can be written in a shorthand notation such as 9.100.100.100/27, where /27 represents 27 ones followed by five zeros.
    

## CIDR (Classless Inter-Domain Routing)

* Address classes were the first attempt at organizing the global Internet IP space.
    
* Subnetting was introduced when address classes proved insufficient.
    
* The continuous growth of the Internet made traditional subnetting inadequate.
    

### Traditional Subnetting

* Network ID is always 8-bit for Class A, 16-bit for Class B, and 24-bit for Class C.
    
* Only 254 Class A networks exist, but there are over 2 million potential Class C networks, resulting in large routing tables.
    
* Network sizes often don't align with business needs.
    

### Problems

* Class C network with 254 hosts is too small for many uses.
    
* Class B network with 65,534 hosts is usually too large.
    
* Many companies had multiple adjoining Class C networks, leading to redundant entries in routing tables.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708408419716/e27d7885-4dd7-4755-b863-2b8c7a5cec77.png align="center")
    

## Introduction of CIDR

* CIDR offers a more flexible way of describing IP address blocks, expanding on subnetting by using subnet masks.
    
* The term 'demarcation point' refers to where one network or system ends, and another begins.
    
* CIDR combines network ID and subnet ID into one, simplifying how routers and network devices understand IP addresses.
    

### CIDR Notation

* CIDR introduces a shorthand slash notation, also known as CIDR notation.
    
* CIDR abandons address classes, defining an address by only two individual IDs.
    
* **Example:** 9.100.100.100 with a net mask of 255.255.255.0 can be written as <mark>9.100.100.100/24</mark>.
    

### Benefits of CIDR

* Allows for more arbitrary network sizes.
    
* Networks can differ in sizes, not just subnets.
    
* Allows combining address space into one contiguous chunk for more efficiency.
    
* Reduces the number of entries needed in a routing table for traffic delivery.
    
* Provides additional available host IDs.
    

## Conclusion:

In the ever-evolving landscape of networking, subnetting and CIDR stand as pillars of innovation, empowering organizations to build robust and efficient networks tailored to their unique requirements. By mastering these fundamental principles and embracing the flexibility and scalability they offer, network engineers can navigate the complexities of modern networking with confidence and precision, laying the foundation for a connected and resilient digital future.