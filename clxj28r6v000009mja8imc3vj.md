---
title: "2.6 Gateway Chronicles: Navigating the Network Realm"
seoDescription: "Non-Routable Address Space: A Brief History
Autonomous Systems
Gateway Protocols
RFC"
datePublished: Mon Jun 17 2024 14:18:08 GMT+0000 (Coordinated Universal Time)
cuid: clxj28r6v000009mja8imc3vj
slug: 26-gateway-chronicles-navigating-the-network-realm
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1718633690421/47d4106c-0051-49e3-8119-0673ebe34ad1.png
tags: networking, 2articles1week, gateway-protocol, interior-gateway-protocol, external-gateway-protocol

---

## Interior Gateway Protocols

* **Routing Basics:** Routing tables are continuously updated with information about the fastest path to destination networks.
    
* **Routing Protocols:** Routers use routing protocols to communicate with each other and share information, enabling them to learn the best path to a network anywhere in the world.
    
* **Categories:** Routing protocols fall into two main categories: Interior Gateway Protocols (IGP) and Exterior Gateway Protocols (EGP).
    
* **Interior Gateway Protocols:** Interior Gateway Protocols (IGPs) are routing protocols used to exchange routing information within a single autonomous system (AS). They’re essential for managing internal traffic and ensuring data is efficiently routed. Common IGPs include RIP, OSPF, and EIGRP.
    
    These are further divided into link state routing protocols and distance vector protocols.
    
* **Usage:** *IGPs* are used by routers within a single autonomous system, which is a collection of networks under the control of a single network operator. Examples include a large corporation routing data between its offices, or the many routers used by an Internet Service Provider (ISP).
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1710711181644/5f3f0700-2cd1-40f3-8d2e-1a17ffc42874.png align="center")
    
    **Contrast with EGPs:** EGPs are used for information exchange between independent autonomous systems.
    
* **Distance Vector Protocols:** An older standard, a router using distance vector protocol sends a list (known as a vector in computer science) of every network it knows and how far these networks are in terms of hops to every neighboring router.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1710711267102/6ca94959-60fa-4d27-9157-5d62267ca971.png align="center")
    
* **Link State Protocols:** More sophisticated, each router advertises the state of its interfaces' links. The information about each router is propagated across the autonomous system, allowing every router to know every detail about every other router. This requires more memory and processing power but has become more prevalent as computer hardware has become more powerful and cheaper.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1710711304945/baf42db9-5d8b-4f37-98c5-b5e94ca7ffda.png align="center")
    

## Exterior Gateways, Autonomous Systems, and the IANA

* **Exterior Gateway Protocols**:
    
    * These protocols are used for data communication between routers at the edges of an autonomous system.
        
    * They play a crucial role in the operation of the Internet by enabling data sharing across various organizations.
        
* **Autonomous Systems**:
    
    * The Internet is essentially a vast mesh of autonomous systems.
        
    * Core Internet routers need to identify and understand these systems to correctly forward traffic.
        
    * The primary goal is to direct data to the edge router of an autonomous system.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1710711382562/ca07168d-31e4-4c8a-b937-8a679b272906.png align="center")
        
* **Internet Assigned Numbers Authority (IANA)**:
    
    * The IANA is a nonprofit organization responsible for managing IP address allocation.
        
    * Without the IANA's management, the Internet could not function effectively.
        
    * The IANA is also responsible for the allocation of Autonomous System Numbers (ASNs).
        
* **Autonomous System Numbers (ASNs)**:
    
    * ASNs are numbers assigned to individual autonomous systems.
        
    * They are represented as 32-bit numbers, typically referred to as a single decimal number.
        
    * ASNs symbolize entire autonomous systems. For instance, IBM is represented by AS19604.
        
* **Understanding Exterior Gateway Protocols**:
    
    * The in-depth understanding of how exterior gateway protocols work may not be necessary for most in the IT field.
        
    * However, it's essential to grasp the basics of autonomous systems, ASNs, and the role of core Internet routers in routing traffic between these systems.
        

## Non-Routable Address Space: A Brief History

* The **Internet growth** was obvious as early as 1996, outpacing IP address availability.
    
* The **IPv4 standard** defines an IP address as a 32-bit number, allowing for approximately 4.3 billion unique addresses. However, this is insufficient in present scenario, let alone data centers hosting thousands of computers.
    
* To address this, **RFC 1918** was published in 1996. RFC (Request for Comments) is a method for setting Internet standards. RFC 1918 defines certain networks as non-routable address space.
    

### Understanding Non-Routable Address Space

* Non-routable address spaces are IP ranges set aside for anyone to use but cannot be routed to.
    
* They enable nodes within such a network to communicate with each other, but no gateway router will forward traffic to this type of network.
    
* Despite seeming limiting, **NAT (Network Address Translation)** technology can allow computers on non-routable address space to communicate with other devices on the Internet.
    

### RFC 1918 Defined Address Ranges

* RFC 1918 defined **three IP address ranges that will never be routed anywhere by core routers: 10.0.0.0/8, 172.16.0.0/12, and 192.168.0.0/16**.
    
* These ranges can be used freely for **internal networks**.
    
* While interior gateway protocols will route these address spaces, exterior gateway protocols will not.
    

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Congratulations🎉 you have completed The Network Layer module. ✌🏻</div>
</div>