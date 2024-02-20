---
title: "1.2 Core Component of Networking: Cables, Devices, and Protocols"
datePublished: Tue Feb 13 2024 03:35:24 GMT+0000 (Coordinated Universal Time)
cuid: clsjt8q90000109jmcj9kctdt
slug: 12-core-component-of-networking-cables-devices-and-protocols
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1707620334436/c9a3cf5b-b926-49b2-bc52-499e2b88870b.png
tags: linux, windows, networking, trainwithshubham, azurenetworking

---

# Introduction:

In today's digital age, networking has become an integral part of our daily lives. From scrolling through social media feeds, sending emails, to streaming our favorite shows – all these activities require a network. But how does this all work? To understand networking, it is essential to get acquainted with the fundamental components involved, which include <mark>cables, devices, and protocols</mark>.

## The Backbone: Cables

In the world of networking, cables are the unsung heroes. They are the basic components of a wired network, connecting different devices to each other and enabling the transmission of data. There are several types of cables, each with their unique properties and uses.

### Copper Cables: The Networking Workhorse

The most common form of networking cable is the copper cable. This type of cable is made up of multiple pairs of copper wires encased inside a plastic insulator. They are used for sending binary data across by changing voltage between two ranges.

There are several kinds of copper cables, including Cat5, Cat5e, and Cat6 cables. Each type comes with different physical characteristics, usable lengths, and data transfer rates. The arrangement of twisted pairs inside these cables can significantly affect data transmission speed and resistance to outside interference.

For instance, <mark>Cat5e and Cat6</mark> cables are designed with reduced Crosstalk (accidental detection of an electrical pulse on another wire), which results in fewer network errors and a more efficient data transfer.

### Fiber Cables: The Speedy Alternative

Fiber cables, also known as Fiber Optic Cables, are another type of networking cable. Unlike copper cables, fiber cables contain tiny glass tubes capable of transporting light beams. Consequently, fiber cables use pulses of light to represent binary data, offering a unique approach to data transmission.

Fiber cables are ideal for environments with a lot of electromagnetic interference as they are immune to such disturbances. They are generally faster than copper but are also more expensive and fragile. One of the primary advantages of fiber cables is their ability to transport data over longer distances without potential data loss.

**<mark>A Note on Fiber Cables</mark>**

Even though fiber cables have several advantages, they are more commonly found in data centers than in offices or homes. This prevalence is primarily due to their cost and fragility. Nonetheless, as technology continues to advance, we may see more widespread use of fiber cables in various settings.

In conclusion, understanding the different types of networking cables and their properties can help you make an informed decision when setting up a network. Whether you choose copper or fiber will depend on your specific needs, budget, and environment.

## The Connectors: Devices

In the realm of Information Technology, specialists often interact with various network devices. Understanding these devices is critical as they enable communication between the billions of computers, we have in the world today. This blog post will demystify three essential network devices: Hubs, Switches, and Routers, and their roles in networking.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">A five-layermodel is used to explain how network devices communicate.</div>
</div>

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707610572012/5e63bcd8-cfa0-4915-8a0d-be776e7d4a30.png align="center")

### Hubs

Starting with the most basic, a hub is a physical layer device that allows multiple computers to connect simultaneously. However, <mark>all devices connected to a hub communicate with each other at the same time, creating a lot of noise on the network and forming a collision domain.</mark> The hub's design leads to inefficiencies and potential data delays, making them historical artifacts in today's networking world. They're rarely used in modern setups.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707610301167/0a5c49f1-d312-486e-836e-92332ec95c52.png align="center")

### Switches

A step up from hubs, switches are more sophisticated devices. A switch is a layer two or data link device that allows the connection of many devices for communication. The critical difference between a hub and a switch is in data handling. <mark>A switch can inspect the contents of the Ethernet protocol data being sent across the network and determine the system for which the data is intended, sending the data only to that specific system</mark>. This process reduces or even entirely eliminates the size of collision domains on the network, making data transfer more efficient.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707610710362/9d91f4ff-44c5-49d7-a9b8-88b9dfc3fc27.png align="center")

### Routers

While hubs and switches are primary devices used to connect computers on a single network, such as a <mark>LAN (Local Area Network)</mark>, routers come into play when we need to send or receive data to computers on other networks. <mark>Operating at layer three</mark>, the network layer, a router can inspect IP data to determine where to send it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707610935746/16c6c991-7f13-4251-a62d-40085aa03136.png align="center")

Routers store internal tables containing information about how to route traffic between numerous different networks all over the world. In a home network or small office, you will typically find a common type of router whose main purpose is to take traffic originating from inside the home or office LAN and forward it to the ISP (Internet Service Provider).

Once the traffic reaches the ISP, a more sophisticated type of router takes over. These core routers form the backbone of the internet and are directly responsible for how we send and receive data all over the internet every single day. They handle a significantly larger amount of traffic compared to home or small office routers and deal with much more complexity in making decisions about where to send traffic. The core routers use a protocol called <mark>BGP (Border Gateway Protocol)</mark> to share data and learn about the most optimal paths to forward traffic.

In conclusion, the internet is incredibly large and complicated, and these network devices act as guides for getting traffic to the right places. Understanding these devices and their functions is essential for anyone looking to delve into the IT field or even troubleshoot their home network.

## The Rules: Protocols

Protocols are the set of rules or standards that dictate how data is transmitted over a network. They determine how data should be packaged, sent, received, and unpacked. Some common protocols you might have heard of include TCP/IP (Transmission Control Protocol/Internet Protocol), HTTP (Hypertext Transfer Protocol), and FTP (File Transfer Protocol).

These protocols ensure that data is sent and received in a form that the sender and receiver understand. For instance, the TCP/IP protocol is foundational to the internet. It breaks down data into packets, transmits them, and then reassembles them on the receiving end.

**<mark>Transmission Control Protocol/Internet Protocol (TCP/IP)</mark>**

The TCP/IP is a suite of communication protocols used to interconnect network devices on the internet. It has two primary components. The Internet Protocol (IP) is responsible for addressing and routing packets of data so they can travel across networks and arrive at the correct destination. On the other hand, the Transmission Control Protocol (TCP) ensures reliable data transmission. It checks that data sent from one device arrives intact and in the correct order at the destination device.

**<mark>User Datagram Protocol (UDP)</mark>**

Unlike TCP, the User Datagram Protocol (UDP) does not provide guaranteed delivery of packets. It's a simpler, faster protocol that sends packets without establishing a connection or checking that the data is received. This lack of 'handshaking' and guaranteed delivery makes UDP suitable for real-time applications like live video or game streaming, where speed is more important than accuracy.

**<mark>Hypertext Transfer Protocol (HTTP)</mark>**

The Hypertext Transfer Protocol (HTTP) is the foundation for data communication on the World Wide Web. It's a protocol used for transmitting hypermedia documents like HTML, where a web browser sends an HTTP request to a web server, which then sends an HTTP response back. HTTP is not secure, so it is often used in conjunction with Secure Sockets Layer (SSL) or Transport Layer Security (TLS) to provide secure communication, known as HTTPS.

Protocols play a vital role in ensuring effective communication between computers in a network. They set the rules and standards, determining how data is transmitted, received, and interpreted. Understanding these protocols is crucial to understanding how our interconnected digital world operates.

## Conclusion

In conclusion, understanding the basics of networking – cables, devices, and protocols – is crucial to understanding how our interconnected world functions. It allows us to appreciate the complexity and ingenuity behind the scenes every time we browse a webpage, send an email, or stream a video.