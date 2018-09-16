---
layout: post
title: "Google + Coursera"
author: "Amy"
---

# Google IT Support Professional Certificate
- [The launchpad to a career in IT. This program is designed to take beginner learners to job readiness in eight to 12 months.](https://www.coursera.org/specializations/google-it-support)

### Technical Support Fundamentals
- Introduction to IT, Hardware, Operating System, Networking, Software

### The Bits and Bytes of Computer Networking
- Introduction to Networking, The Network Layer, The Transport and Application Layers, Networking Services, Connecting to the Internet, Future of Networking


<br>
<br>
<hr>


# Operating System
### Hardware ⇌ Kernel ⇌ User Space
- The whole package that manages our computers resources and lets us interact with it.
- Windows, Mac, Linux (Open Source)
- cf. [Ubuntu - Linux Distribution](https://www.ubuntu.com), [Chrome OS](https://en.wikipedia.org/wiki/Chrome_OS)


# Kernel space
- ✨ **Process manager**
- ✨ **Memory manager**
- ✨ **File manager**
- ✨ **I/O manager** / Input, Output

# User space
- Everything outside the kernel

## File Management
> We write data to our hard drive in the form of **data blocks**. **Block storage ** improves faster handling of data because the data isn't stored on one long piece and it can be accessed quicker.

## Process Management
- A process is a program that's executing, like our internet browser or text editor.
- A program is an application that we can run, like Chrome.

> The kernel creates processes, efficiently schedules them, and manages how processes are terminated.

## Memory Management
- **Virtual memory** is **a combination of hard drive space and RAM** that acts like memory that our processes can use.

> When we store our virtual memory on our hard drive, we call the allocated space, swap space. When we get into practical applications of disk partitioning, we'll allocate space for swap. The kernel takes care of all of this for us, of course. It handles the process of taking pages of data and swapping them between RAM and virtual memory.

## I/O Management
- kernel handles all the intercommunication between devices.

> If you don't have enough RAM, you can't load up as many processes. If you don't have enough CPU, you can't execute programs fast enough.

## Shell / GUI
- A shell is basically **a program that interprets text commands and sends them to the OS to execute.**
- CLI Shell (Command Line Interface), Especially in Linux

# CPU
> We have different CPU architecture's, 32-bit and 64-bit. Our operating systems will also be optimized for this architecture, so make sure that the CPU and OS are compatible. If you have a 64-bit CPU, you should also install the 64-bit version of the operating system you choose.

# Virtual Machine
- Virtual machines use physical resources like memory, CPU and storage, but they offer the added benefit of running multiple operating systems at once. 

# Network Model

- ✨ **physical layer** / cable, connetors, sending signal
    - modulation, line coding
    - Twisted pair, electromagnetic interference
    - A standard cat six cable has eight wires consisting of four twisted pairs inside a single jacket.
    - Duplex communication (쌍방향)
    - Patch Panel
- ✨ **datalink layer** / Ethernet, wi-fi  ===> interpreting signals
- ✨ **network layer** / Internet Protocol (IP), allows different networks to communicate with each other through devices known as routers. A collection of networks connected together through routers is an internetwork, the most famous of these being the Internet.
- ✨ **transport layer** / the transport layer, is known as TCP or Transmission Control Protocol.
- ✨ **application layer** / application specific

> in our case IP, is responsible for getting data from one node to another. Also, remember that the transport layer, mostly TCP and UDP, is responsible for ensuring that data gets to the right applications running on those nodes. 


# Networking Devices
### ❏ Hubs 
- 1 Network ⇄ Multiple Device
- Physical Layer

> A collision domain is a network segment where only one device can communicate at a time. If multiple systems try sending data at the same time, the electrical pulses sent across the cable can interfere with each other.

### ❏ Switch 
- 1 Network ⇄ Multiple Device
- Data Link Layer

> This means that a switch can actually inspect the contents of the Ethernet protocol data being sent around the network, determine which system the data is intended for and then only send that data to that one system. 

> A switch remembers which devices are connected on each interface, while a hub does not.

### ❏ Summary
> Hubs and switches are the primary devices used to connect computers on a single network, usually referred to as a LAN, or local area network. But we often want to send or receive data to computers on other networks. This is where routers come into play. 

### ❏ Router
- Network Layer
- Multiple Networks  ⇄ Multiple Device
- A device that knows how to forward data between independent networks

> A router is a device that knows how to forward data between independent networks. While a hub is a layer one device and a switch is a layer two device.

### ❏ Home Router
- LAN (Local Area Network) ⇄ ISP (Internet Service Provider)

> The most common type of router you'll see is one for a home network, or a small office. These devices generally don't have very detailed routing tables. The purpose of these routers is mainly just to take traffic originating from inside the home, or office LAN, and to forward it along to the ISP, or Internet Service Provider. 

### ❏ ISP Routers
- Core ISP routers

> Core ISP routers don't just handle a lot more traffic than a home or a small office router. They also have to deal with much more complexity in making decision about where to send traffic. A core router usually has many different connections to many other routers. Routers share data with each other via a protocol known as BGP, or Border Gateway Protocol. That lets them learn about the most optimal paths to forward traffic. When you open a web browser and load a webpage, the traffic between computers and the web servers could have traveled over dozens of different routers. The Internet is incredibly large and complicated. And routers are global guides for getting traffic to the right places. 


# Physical Layer

### ❏ Moving Bits Across the Wire
- 1s & 0s are moving through tiny wires with incredible speed.
- A bit is the smallest representation of data that a computer can understand. It's a one or a zero.
- modern networks are capable of moving 10 billion ones and zeros across a single network cable every second. 

### ❏ Twisted Pair Cabling and Duplexing
- Duplex communication is the concept that information can flow in **both directions** across the cable. On the flip side, a process called simplex communication is unidirectional.

### ❏ Ethernet
- Ethernet over twisted pair technologies are the communications protocols that determine how much data can be sent over a twisted pair cable, how quickly that data can be sent, and how long a network cable can be before the data quality begins to degrade.
- [Ethernet ](https://en.wikipedia.org/wiki/Ethernet_over_twisted_pair)

### ❏ Network ports 
- Network ports are generally directly attached to the devices that make up a computer network. Switches would have many network ports because their purpose is to connect many devices. But servers and desktops usually only have one or two. Your laptop, tablet or phone probably don't have any. 


# DataLink Layer
	
### ❏ Ethernet protocol
- The protocol most widely used to send data across individual links is known as Ethernet. Ethernet and the data link layer provide a means for software at higher levels of the stack to send and receive data.

### ❏ MAC address (Hardware Address)
- A MAC address is a globally unique identifier attached to an individual network interface. It's a 48-bit number normally represented by six groupings of two hexadecimal numbers.

> Ethernet uses MAC addresses to ensure that the data it sends has both an address for the machine that sent the transmission, as well as the one that the transmission was intended for. In this way, even on a network segment, acting as a single collision domain, each node on that network knows when traffic is intended for it. 


# Network Layer

### ❏ ARP (Address Resolution Protocol)

### ❏ IP datagram
- Under the IP protocol, a **packet** is usually referred to as an IP datagram.
- **Header** / version, header length, Service Type, Total Length field
- **Playload** / Identification, Flag, Fragmentation
- **The flag field** is used to indicate if a datagram is allowed to be fragmented, or to indicate that the datagram has already been fragmented. 
- **Fragmentation** is the process of taking a single IP datagram and splitting it up into several smaller datagrams.
- **TTL** / Time to live (TTL) or hop limit is a mechanism that limits the lifespan or lifetime of data in a computer or network. The main purpose of this field is to make sure that when there's a misconfiguration in routing that causes an endless loop, datagrams don't spend all eternity trying to reach their destination. 
- **Protocol Field** / TCP, UDP
- **Header Checksum**
- **IP Address** / 32bit
- **Source IP Address**, **Destination IP Address**

### ❏ Subnetting
- The process of taking a large network and **splitting it up into many individual smaller subnetworks** or subnets.
- With subnets, _you can split your large network up into many smaller ones_. These individual subnets will all have their own gateway routers serving as the ingress and egress point for each subnet. 
- **Subnet IDs** are calculated via what's known as a **subnet mask**. Just like an IP address, subnet masks are **32-bit numbers** that are normally written now as four octets in decimal. 

### ❏ IANA (Internet Assigned Numbers Authority)
- Along with managing IP address allocation, the IANA is also responsible for ASN, or Autonomous System Number allocation.


<br>
<br>

# Standby
> Goal to be done by 31, March

### Transport Layer
### Application Layer
### DNS
### Dynamic Host Configuration Protocol
### NAT
### VPN
### Proxies
