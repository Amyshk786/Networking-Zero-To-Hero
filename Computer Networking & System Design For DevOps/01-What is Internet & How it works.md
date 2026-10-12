Suppose you are in Pune, your friend is in Kashmir, and your other friend is in New York

-> When your computer is not connected to any other computer, then it is called a Standalone

-> If all your computers are connected, then it is called Computer Networking [A, B & C are connected to each other]

-> When all these Devices are Inter-Connected then it is called the Internet.

-> Internet flows through Fiber Optic Cables




# How the Internet Works

-> World - Across the world, we have Fiber Optic Cables, which are in the sea.

-> Country - In the Country, various Companies have purchased these Fiber Optic Cables Wide Area Network (WAN)

-> City - The Data is then transmitted from these Fiber Optic Cables to a City Metropolitan Area Network (MAN),  an Internet Exchange Point (IXP), which is a physical data center. [IXP gives Internet to ISP]

-> Home - The ISP takes Internet from IXP and provides us Internet in the form of WiFi, which is a Local Area Network (LAN) [U-Broadband, Airtel, Jio, Hathway]




# This Data is Transferred in Packets from One Computer to another 

[A packet is a formatted unit of data that is transmitted across a computer network from a source to a destination. The data is encoded]

[Latency is the time it takes for data to travel from a source to a destination across a network.]

[Bandwidth is the maximum amount of data that can be transmitted over a network connection in a given amount of time.]

[Jitter is the variation in the delay (latency) of data packets as they travel across a network.]

[Packet loss is when some pieces of data sent over a network fail to reach their destination.] Imagine you're sending 100 messages over the internet, but only 95 arrive. The other 5 are lost along the way. That's 5% packet loss.





# The Physical Internet

1) Router - It routes the Packet between Network


2) Switch - A Switch is a Physical Device in which we can Connect Multiple Devices, Provide Internet in the Switch and this Internet will keep the Multiple Devices connected. This is used in the Data Center.


3) Hub - It was previously used as a switch when the switch was not invented.




# Traceroute is a network diagnostic tool that shows the path your data takes from your computer to a destination server, such as GitHub or Google. It helps identify where delays or network problems might occur.


# Content Delivery Network (CDN) - CDN stands for Content Delivery Network. It is a network of servers distributed across different geographical locations that deliver website content to users from a server close to them.  For example, when you open YouTube, Netflix, GitHub, or an online shopping website, a CDN can help load images, videos, scripts, and other content faster. [If the Data is in Image form or if the  Data is static data, then CDN will be used]


# An Edge Location is a server location close to end users that delivers content and services with lower latency. It is commonly used by CDNs (Content Delivery Networks) such as Cloudflare and Amazon CloudFront. Think of it as a nearby distribution center: instead of shipping everything from a distant warehouse, a company keeps frequently requested items closer to its customers.
[For example, if a website's origin server is in the United States and you're browsing from India, a CDN may serve images or JavaScript files from an edge location in India. This can reduce the distance the content travels.]




# Difference Between Region, Availability Zone & Local Zone


1) Region - A Region is a geographical area where a cloud provider operates infrastructure. Each AWS Region is designed to be isolated from other Regions.
Examples:
- Mumbai — ap-south-1
- Singapore — ap-southeast-1
- N. Virginia — us-east-1
- Tokyo — ap-northeast-1

For example, if most of your customers are in India, you might deploy your application in the Mumbai Region to help reduce latency.




2) Availability Zone (AZ) - An Availability Zone is an isolated location within an AWS Region, made up of one or more data centers with independent infrastructure.

A Region has multiple AZs, designed to support fault isolation.

Example: Suppose your application runs in two AZs in Mumbai.
- AZ A hosts your application servers.
- AZ B hosts another set of application servers.
- If AZ A experiences an outage, your application can continue serving users from AZ B, provided you've configured redundancy and failover correctly.

Key point: Deploying across multiple AZs helps protect against a single AZ failure. It does not automatically provide high availability; your application and data must also be configured appropriately.




3) Local Zone - An AWS Local Zone extends selected AWS services closer to users in a particular metropolitan area, while remaining associated with a parent Region.

It is useful when applications need very low latency. Local Zones are not available in every city, and the services offered vary by location.

Examples of workloads include:
- Real-time gaming
- Media production and processing
- Interactive applications
- Virtual desktops
- Applications with latency-sensitive workloads
Imagine your application's main infrastructure is in Mumbai, but you need some computing resources closer to users in another city. A supported Local Zone may help reduce network latency for those users.





==============================================================================================================================================================================================================

# Internet Addressing - Internet addressing is the system used to identify devices, networks, and services so that data can travel from a source to a destination across the Internet.

For example, when you open GitHub on your Mac mini M4, several addressing systems work together to connect your browser to GitHub's servers.



# Subnetting and CIDR — From Scratch


## Table of Contents

1. [What Is an IP Address?](#1-what-is-an-ip-address)
2. [What Is a Subnet?](#2-what-is-a-subnet)
3. [What Is a Subnet Mask?](#3-what-is-a-subnet-mask)
4. [What Is CIDR?](#4-what-is-cidr)
5. [Network Bits and Host Bits](#5-network-bits-and-host-bits)
6. [Calculate Total and Usable Addresses](#6-calculate-total-and-usable-addresses)
7. [Common CIDR Prefixes](#7-common-cidr-prefixes)
8. [Subnetting Example: /24 to /26](#8-subnetting-example-24-to-26)
9. [Network, Broadcast, and Host Range](#9-network-broadcast-and-host-range)
10. [Practical Commands on macOS](#10-practical-commands-on-macos)
11. [Practice Questions](#11-practice-questions)

## 1. What Is an IP Address?

An IP address identifies a network interface for communication using the Internet Protocol.

Example:

`192.168.1.10`

An IPv4 address contains 32 bits divided into four 8-bit sections called octets.

Example:

`192.168.1.10`

Each octet represents a number from 0 to 255.

## 2. What Is a Subnet?

A subnet is a logical division of an IP network.

For example, these devices can belong to the same subnet:

- Mac mini: `192.168.1.10`
- Laptop: `192.168.1.11`
- Printer: `192.168.1.12`

If their subnet is `192.168.1.0/24`, they share the same IPv4 network prefix.

### Why Do We Use Subnets?

- Organize devices into networks.
- Reduce broadcast traffic within each subnet.
- Separate departments and workloads.
- Apply network security policies.
- Plan IP address allocation efficiently.

## 3. What Is a Subnet Mask?

A subnet mask separates the network portion from the host portion of an IPv4 address.

Example:

- IP address: `192.168.1.10`
- Subnet mask: `255.255.255.0`
- CIDR prefix: `/24`
- Network address: `192.168.1.0`

The first 24 bits identify the network. The remaining 8 bits identify the host portion.

## 4. What Is CIDR?

CIDR stands for Classless Inter-Domain Routing.

It represents an IP network using an address followed by a slash and a prefix length.

Example:

`192.168.1.0/24`

The `/24` means 24 bits belong to the network prefix.

IPv4 has 32 bits, so:

`Host bits = 32 - Prefix length`

For `/24`:

`Host bits = 32 - 24 = 8`

CIDR allows networks to have different sizes instead of relying on the old fixed Class A, B, and C network boundaries.

## 5. Network Bits and Host Bits

An IPv4 address has 32 bits.

- Network bits identify the subnet.
- Host bits provide address combinations within that subnet.

Example: `/26`

`Network bits = 26`

`Host bits = 32 - 26 = 6`

The subnet contains:

`2^6 = 64 total addresses`

For a conventional IPv4 subnet, 2 addresses are reserved for the network and broadcast addresses.

Therefore:

`Usable hosts = 64 - 2 = 62`

## 6. Calculate Total and Usable Addresses

### Formula

For an ordinary IPv4 subnet with prefix lengths from `/0` through `/30`:

`Host bits = 32 - prefix length`

`Total addresses = 2^(host bits)`

`Usable hosts = Total addresses - 2`

The conventional usable-host formula excludes the network and broadcast addresses.

Special cases:

- `/31`: commonly used for point-to-point links, where both addresses can be used as endpoints.
- `/32`: represents one IPv4 address.

### Example: /27

`Host bits = 32 - 27 = 5`

`Total addresses = 2^5 = 32`

`Usable hosts = 32 - 2 = 30`

A `/27` subnet has 32 total addresses and 30 conventional usable host addresses.

## 7. Common CIDR Prefixes

| CIDR | Subnet Mask | Total Addresses | Usable Hosts |
|---|---|---:|---:|
| `/24` | `255.255.255.0` | 256 | 254 |
| `/25` | `255.255.255.128` | 128 | 126 |
| `/26` | `255.255.255.192` | 64 | 62 |
| `/27` | `255.255.255.224` | 32 | 30 |
| `/28` | `255.255.255.240` | 16 | 14 |
| `/29` | `255.255.255.248` | 8 | 6 |
| `/30` | `255.255.255.252` | 4 | 2 |
| `/31` | `255.255.255.254` | 2 | Special point-to-point use |
| `/32` | `255.255.255.255` | 1 | One address |

Remember:

- Larger prefix = smaller subnet.
- Smaller prefix = larger subnet.

## 8. Subnetting Example: /24 to /26

Suppose we have:

`192.168.1.0/24`

We want to divide it into four equal subnets.

A `/26` provides 64 addresses per subnet.

The four subnets are:

| Subnet | Network Address | Usable Host Range | Broadcast |
|---|---|---|---|
| 1 | `192.168.1.0/26` | `192.168.1.1–62` | `192.168.1.63` |
| 2 | `192.168.1.64/26` | `192.168.1.65–126` | `192.168.1.127` |
| 3 | `192.168.1.128/26` | `192.168.1.129–190` | `192.168.1.191` |
| 4 | `192.168.1.192/26` | `192.168.1.193–254` | `192.168.1.255` |

Each subnet contains:

- 64 total addresses.
- 1 network address.
- 1 broadcast address.
- 62 conventional usable host addresses.

Four `/26` subnets cover the complete address range of the original `/24` network.

## 9. Network, Broadcast, and Host Range

Consider:

`192.168.10.77/27`

A `/27` has 32 addresses per subnet.

The subnet boundaries in the final octet increase by 32:

`0, 32, 64, 96, 128, 160, 192, 224`

The address `77` falls within the range from 64 through 95.

Therefore:

- Network address: `192.168.10.64`
- First usable host: `192.168.10.65`
- Last usable host: `192.168.10.94`
- Broadcast address: `192.168.10.95`
- Total addresses: 32
- Usable hosts: 30

The network address identifies the subnet. The broadcast address is used to reach all IPv4 hosts on that subnet.

## 10. Practical Commands on macOS

### Check Your Local IPv4 Address

```bash
ipconfig getifaddr en0
```

This commonly shows the Wi-Fi IPv4 address when Wi-Fi uses `en0`.

### Inspect Network Interfaces

```bash
ifconfig
```

Look for the active interface's `inet` and `netmask` fields.

### Check Your Default Gateway

```bash
route -n get default
```

Look for the `gateway` field.

### Convert a Hexadecimal Netmask

If `ifconfig` shows:

`netmask 0xffffff00`

It corresponds to:

`255.255.255.0`

This is the `/24` prefix.

## 11. Practice Questions

### Question 1

How many usable hosts are available in `192.168.1.0/26`?

Answer: 62 usable hosts.

Calculation:

`32 - 26 = 6 host bits`

`2^6 - 2 = 62`

### Question 2

What is the subnet mask for `/27`?

Answer: `255.255.255.224`

### Question 3

What is the network address of `192.168.10.77/27`?

Answer: `192.168.10.64`

### Question 4

How many total addresses are in a `/28` subnet?

Answer: 16 total addresses, with 14 conventional usable host addresses.

## Summary

- IP addresses identify network interfaces.
- Subnets divide address space into logical networks.
- Subnet masks identify network and host bits.
- CIDR represents a network prefix, such as `/24` or `/26`.
- Host bits determine the number of address combinations.
- Network and broadcast addresses are normally reserved in conventional IPv4 subnets.
- Smaller sub




================

Domain Name System (DNS) 

https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Ftse2.mm.bing.net%2Fth%2Fid%2FOIP.F219uDcRLIxZF6LG09hZUAHaDu%3Fr%3D0%26pid%3DApi&f=1&ipt=d732bc41db8f6d6685c2f1dc635f47776a5439fe8f81e3d621b51138b8095834&ipo=images






















