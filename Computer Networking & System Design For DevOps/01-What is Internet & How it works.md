TWS Networking Notes Link - https://d502jbuhuh9wk.cloudfront.net/digital_products/62cd128e0cf2e6a1694683bb/64909ff3aeda092a26b20536/asset/1687199731158/file.pdf


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








========================================================================================================


# OSI Model Explained

## 1. What Is the OSI Model?

OSI stands for **Open Systems Interconnection**.

The OSI model explains how data travels from one computer to another over a network. It divides network communication into **7 layers**, with each layer having a specific responsibility.

Think of it like ordering a product online: you place an order, the company processes it, the package is transported, and it gets delivered. Network communication also involves different stages.

## 2. The 7 Layers of the OSI Model

| Layer | Name | Simple Explanation | Examples |
|---|---|---|---|
| 7 | Application | Network services used by applications | HTTP, DNS, SMTP |
| 6 | Presentation | Data formatting, encryption, compression | Data formats, encryption functions |
| 5 | Session | Manages communication sessions | Session management |
| 4 | Transport | Delivers data between application endpoints | TCP, UDP, port numbers |
| 3 | Network | Routes packets between networks | IP addresses, routers |
| 2 | Data Link | Transfers frames across a local network link | Ethernet, MAC addresses, switches |
| 1 | Physical | Transmits bits as physical signals | Cables, fiber, radio waves |

## 3. Layer 7 — Application Layer

The Application layer provides network services to applications.

Examples:
- Web browsing
- Email
- Domain name resolution

Common protocols:
- HTTP/HTTPS
- DNS
- SMTP

**Simple example:** You type `github.com` into your browser.

## 4. Layer 6 — Presentation Layer

The Presentation layer represents how data is formatted, encoded, compressed, or encrypted.

Examples:
- Data encoding
- Data compression
- Encryption and decryption

**Simple example:** Data must be represented in a format the receiving application can understand.

Note: In real internet communication, TLS encryption does not map exclusively to this OSI layer.

## 5. Layer 5 — Session Layer

The Session layer represents establishing, managing, and ending communication sessions between applications.

**Simple example:** Managing a conversation between two communicating applications.

Real-world internet protocols may combine these responsibilities rather than using a separate session-layer protocol.

## 6. Layer 4 — Transport Layer

The Transport layer provides communication between application endpoints.

Important protocols:

### TCP (Transmission Control Protocol)

- Provides reliable, ordered byte-stream delivery.
- Retransmits lost data when necessary.
- Commonly used by HTTP/1.1 and HTTP/2.

### UDP (User Datagram Protocol)

- Sends datagrams without TCP's built-in reliability and ordering.
- Has lower protocol overhead.
- Used by protocols such as DNS and QUIC.

### Port Numbers

Port numbers help identify application services on a host.

Examples:
- HTTP: port 80
- HTTPS: port 443
- DNS: commonly port 53

**Simple example:** Your browser connects to a web server using a transport protocol and destination port.

## 7. Layer 3 — Network Layer

The Network layer handles logical addressing and routing between networks.

Important concepts:
- IPv4 and IPv6
- IP packets
- Routers
- Routing tables

**Simple example:** Your Mac sends traffic toward a website outside your home network. Routers forward packets toward the destination.

Useful macOS command:

```bash
route -n get default
```

This displays information about the default route.

## 8. Layer 2 — Data Link Layer

The Data Link layer handles frames and delivery over a local network link.

Important concepts:
- MAC addresses
- Ethernet
- Wi-Fi link communication
- Network switches

**Simple example:** Your Mac sends a Wi-Fi frame toward your wireless access point.

Useful macOS command:

```bash
ifconfig
```

This displays network interface information, including interface addresses.

## 9. Layer 1 — Physical Layer

The Physical layer transmits raw bits using physical signals.

Examples:
- Ethernet cables
- Fiber-optic cables
- Electrical signals
- Radio waves used by Wi-Fi

**Simple example:** Radio signals carry data between your Mac and the Wi-Fi access point.

## 10. How the OSI Model Works When Opening GitHub

Imagine opening `https://github.com` on your Mac.

1. **Application:** The browser prepares an HTTP request.
2. **Presentation:** Data representation and encryption concepts apply; TLS commonly protects HTTPS communication.
3. **Session:** Communication session management is represented conceptually.
4. **Transport:** TCP or UDP carries data between application endpoints.
5. **Network:** IP addresses help route packets to the destination.
6. **Data Link:** Frames carry data over each local network link.
7. **Physical:** Signals transmit bits through Wi-Fi, cables, or fiber.

At the destination, the receiving system processes the data through the relevant protocol layers.

**Important:** The OSI model is a conceptual framework. Real internet protocols do not always map neatly to one individual layer.

## 11. Encapsulation and Decapsulation

### Encapsulation

When data travels from an application toward the network, each relevant layer adds information needed for communication.

Simplified sequence:

```text
Application Data
       |
       v
Transport Segment / Datagram
       |
       v
IP Packet
       |
       v
Data-Link Frame
       |
       v
Physical Signals
```

### Decapsulation

At the receiving device, the relevant protocol layers process the incoming information and remove their respective headers as needed.

This allows the receiving application to access the original data.

## 12. OSI Model vs TCP/IP Model

The TCP/IP model is commonly represented using four layers.

| OSI Model | TCP/IP Model |
|---|---|
| Application (7) | Application |
| Presentation (6) | Application |
| Session (5) | Application |
| Transport (4) | Transport |
| Network (3) | Internet |
| Data Link (2) | Link / Network Access |
| Physical (1) | Link / Network Access |

The TCP/IP model is more closely associated with the protocols used for real internet communication.

## 13. Troubleshooting Using the OSI Model

| Layer | Example Problem |
|---|---|
| Layer 7 — Application | Website or application error |
| Layer 6 — Presentation | Data format or encryption issue |
| Layer 5 — Session | Session management issue |
| Layer 4 — Transport | Port blocked or connection refused |
| Layer 3 — Network | Incorrect IP configuration or routing issue |
| Layer 2 — Data Link | Wi-Fi association or local-link problem |
| Layer 1 — Physical | Cable disconnected or weak physical signal |

These are examples, not definitive diagnoses. A single symptom can involve multiple layers.

## 14. Useful Networking Commands on macOS

### Check IP and Network Interfaces

```bash
ifconfig
```

### Check Default Route

```bash
route -n get default
```

### Test Basic Reachability

```bash
ping -c 4 1.1.1.1
```

### Check DNS Resolution

```bash
dig github.com
```

### Trace the Route to a Website

```bash
traceroute github.com
```

### Check an HTTPS Response

```bash
curl -I https://github.com
```

## 15. Easy Way to Remember the OSI Layers

From Layer 7 down to Layer 1:

**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing.

- Application
- Presentation
- Session
- Transport
- Network
- Data Link
- Physical

## 16. Quick Revision

- **Layer 7:** Applications and network services
- **Layer 6:** Data representation and encryption concepts
- **Layer 5:** Communication sessions
- **Layer 4:** TCP, UDP, and ports
- **Layer 3:** IP addressing and routing
- **Layer 2:** Frames and local network delivery
- **Layer 1:** Physical signals and transmission

**DevOps interview tip:** Pay particular attention to Layers 4 and 7 when troubleshooting application connectivity, Layer 3 for IP routing, and Layer 2 for local network problems.

---

**Networking Notes — OSI Model from Scratch**


# OSI Model Explained

![OSI Model infographic](images/osi-model-infographic.png)






















