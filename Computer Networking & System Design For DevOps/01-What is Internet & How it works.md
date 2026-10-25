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
































