# AWS Internet Gateway (IGW) and NAT Gateway — Complete Networking Notes

## 1. Introduction

When we create an AWS Virtual Private Cloud (VPC), we get a logically isolated network where we can launch AWS resources such as:

- EC2 instances
- Application Load Balancers
- Databases
- Kubernetes clusters (EKS)
- Application servers

But how do these resources communicate with the internet?

AWS provides networking components such as:

1. Internet Gateway (IGW)
2. NAT Gateway

Both help connect a VPC to external networks, but they serve different purposes.

### Main Difference

| Internet Gateway | NAT Gateway |
|---|---|
| Connects a VPC to the internet | Allows private IPv4 resources to initiate internet connections |
| Used for public subnet internet access | Used for private subnet outbound internet access |
| Supports inbound and outbound communication when configured | Does not allow unsolicited inbound internet connections |
| Attached to a VPC | Created inside a subnet |
| Supports IPv4 and IPv6 | Public NAT Gateway supports IPv4 internet access |

---

# 2. What Is an Internet Gateway (IGW)?

An Internet Gateway is an AWS VPC component that allows communication between resources in a VPC and the internet.

It acts as a connection point between your VPC and the public internet.

### Real-Life Example

Imagine you have an EC2 instance hosting a website.

Users around the world want to access your website.

The Internet Gateway helps route traffic between your VPC and the internet.

### Internet Gateway Architecture

```text
                  INTERNET
                      |
                      |
                      v
             INTERNET GATEWAY
                      |
                      |
                      v
               AWS VPC
                      |
                      v
                PUBLIC SUBNET
                      |
                      v
                  EC2 SERVER
```

### Important Points

- An Internet Gateway is attached to a VPC.
- It supports IPv4 and IPv6.
- It is horizontally scaled, redundant, and highly available.
- It does not automatically make every subnet public.
- Route tables must be configured correctly.
- For IPv4 internet communication, the EC2 instance needs a public IPv4 address or Elastic IP.
- Security Groups and Network ACLs must also allow the required traffic.

---

# 3. What Is a Public Subnet?

A public subnet is a subnet whose associated route table contains a route to an Internet Gateway.

Example:

```text
VPC CIDR: 10.0.0.0/16

Public Subnet:
10.0.1.0/24
```

The public subnet route table contains:

| Destination | Target |
|---|---|
| 10.0.0.0/16 | local |
| 0.0.0.0/0 | Internet Gateway |

### What Does 0.0.0.0/0 Mean?

`0.0.0.0/0` represents all IPv4 destinations.

When this route is configured with an Internet Gateway as the target, traffic that does not match a more specific route can be sent toward the internet.

Example:

```text
Destination       Target

10.0.0.0/16       local
0.0.0.0/0         igw-xxxxxxxx
```

This means:

- Traffic within the VPC uses the local route.
- Other IPv4 traffic can go through the Internet Gateway.

**Important:** A route to an Internet Gateway alone does not give an EC2 instance internet connectivity. It also needs suitable public addressing and security configuration.

---

# 4. How Does Internet Gateway Communication Work?

Imagine an EC2 instance with:

```text
Private IP: 10.0.1.10
Public IP:  54.100.20.10
```

The instance wants to communicate with an external website.

### Outbound Traffic Flow

```text
EC2 INSTANCE
Private IP: 10.0.1.10
Public IP: 54.100.20.10
       |
       |
       v
ROUTE TABLE
0.0.0.0/0 -> IGW
       |
       |
       v
INTERNET GATEWAY
       |
       |
       v
INTERNET
       |
       |
       v
EXTERNAL WEBSITE
```

The Internet Gateway performs the necessary public/private IPv4 address translation for the instance.

### Inbound Traffic Flow

Imagine a user wants to access your EC2-hosted website.

```text
USER / INTERNET
       |
       |
       v
INTERNET GATEWAY
       |
       |
       v
PUBLIC IP TRANSLATION
       |
       |
       v
EC2 INSTANCE
Private IP: 10.0.1.10
```

The Internet Gateway translates the public IPv4 destination to the instance's private IPv4 address.

The Security Group and Network ACL must allow the traffic.

---

# 5. What Is a NAT Gateway?

NAT stands for Network Address Translation.

A NAT Gateway allows resources in a private subnet to initiate connections to external networks, including the internet through a public NAT Gateway.

It does this without allowing unsolicited inbound internet connections directly to those private resources.

### Real-Life Example

Imagine you have an EC2 instance in a private subnet.

The instance needs to:

- Download Linux packages.
- Install Docker.
- Download application dependencies.
- Connect to external APIs.
- Download container images.

However, you do not want this instance to be directly accessible from the internet.

This is where a NAT Gateway is useful.

### NAT Gateway Architecture

```text
                 INTERNET
                     |
                     |
                     v
             INTERNET GATEWAY
                     |
                     |
                     v
               PUBLIC SUBNET
                     |
                     v
                NAT GATEWAY
                     |
                     |
                     v
               PRIVATE SUBNET
                     |
                     v
                  EC2 SERVER
```

The NAT Gateway is created in a public subnet.

The private subnet routes internet-bound IPv4 traffic to the NAT Gateway.

---

# 6. How Does NAT Gateway Work?

Imagine an EC2 instance in a private subnet:

```text
Private EC2 IP: 10.0.2.10
```

It wants to download software from the internet.

The instance does not need its own public IPv4 address.

### Step-by-Step Traffic Flow

**Step 1: Private EC2 sends a request**

The private EC2 instance wants to connect to an external website.

```text
Source:      10.0.2.10
Destination: External Website
```

**Step 2: Private Route Table**

The private subnet route table contains:

```text
Destination       Target

10.0.0.0/16       local
0.0.0.0/0         NAT Gateway
```

The request is forwarded to the NAT Gateway.

**Step 3: NAT Gateway translates the source address**

The NAT Gateway replaces the private source address with its public Elastic IP for internet communication.

Example:

```text
Original Source IP:
10.0.2.10

NAT Gateway Public IP:
54.200.10.20
```

**Step 4: Request reaches the Internet Gateway**

The NAT Gateway forwards the request through the Internet Gateway.

**Step 5: External server responds**

The external website sends a response to the NAT Gateway's public IP.

**Step 6: NAT Gateway translates the response**

The NAT Gateway maps the response back to the original private EC2 connection.

**Step 7: Private EC2 receives the response**

The private EC2 instance receives the response.

### Complete NAT Gateway Traffic Flow

```text
PRIVATE EC2
10.0.2.10
     |
     | Request
     v
PRIVATE ROUTE TABLE
0.0.0.0/0 -> NAT Gateway
     |
     v
NAT GATEWAY
     |
     | Source IP translated
     | Private IP -> Public EIP
     v
INTERNET GATEWAY
     |
     v
INTERNET
     |
     v
EXTERNAL WEBSITE
     |
     | Response
     v
INTERNET GATEWAY
     |
     v
NAT GATEWAY
     |
     | Reverse translation
     v
PRIVATE EC2
10.0.2.10
```

**Important:** The NAT Gateway allows return traffic for connections initiated from the private subnet. It does not allow the internet to initiate arbitrary connections directly to the private EC2 instance.

---

# 7. Public Subnet vs Private Subnet

| Public Subnet | Private Subnet |
|---|---|
| Route table has a route to IGW | No direct route to IGW |
| Can provide direct internet connectivity | Can use NAT Gateway for outbound IPv4 internet access |
| Public IP required for direct IPv4 internet access | Public IP is not required for NAT-based outbound IPv4 access |
| Commonly hosts Load Balancers and NAT Gateways | Commonly hosts application servers and databases |

### Example AWS Architecture

```text
                         INTERNET
                             |
                             |
                             v
                    INTERNET GATEWAY
                             |
               +-------------+-------------+
               |                           |
               v                           v
         PUBLIC SUBNET A              PUBLIC SUBNET B
               |                           |
         LOAD BALANCER               NAT GATEWAY
               |                           |
               |                           |
               v                           v
       PRIVATE SUBNET A             PRIVATE SUBNET B
               |                           |
          APP SERVER A                 APP SERVER B
               |                           |
               +-------------+-------------+
                             |
                             v
                       DATABASE
                      PRIVATE SUBNET
```

This is a simplified architecture. In production, application and database tiers are commonly distributed across multiple Availability Zones.

---

# 8. Public NAT Gateway vs Private NAT Gateway

AWS supports two NAT Gateway connectivity types.

### Public NAT Gateway

A public NAT Gateway:

- Is created in a public subnet.
- Requires an Elastic IP address.
- Uses an Internet Gateway for internet connectivity.
- Allows private IPv4 resources to initiate internet connections.
- Does not allow unsolicited inbound internet connections to private resources.

### Private NAT Gateway

A private NAT Gateway:

- Is created for private network connectivity.
- Does not use an Elastic IP for internet access.
- Can translate traffic going to other VPCs or on-premises networks.
- Cannot provide direct public internet connectivity through an Internet Gateway.

### Comparison

| Feature | Public NAT | Private NAT |
|---|---|---|
| Internet access | Yes | No direct internet access |
| Elastic IP | Required | Not required |
| Main use | Private subnet internet access | Private network communication |
| Internet Gateway | Used for internet routing | Not used for direct internet routing |

---

# 9. Internet Gateway vs NAT Gateway

| Feature | Internet Gateway | NAT Gateway |
|---|---|---|
| Purpose | Connect VPC to internet | Enable outbound connectivity for private resources |
| Attached to | VPC | Created in a subnet |
| Public subnet | Used | Public NAT Gateway is hosted here |
| Private subnet | No direct IGW route for private IPv4 egress | Can route to NAT Gateway |
| Public IP | Resource needs public IPv4 for direct IPv4 internet access | Public NAT uses an Elastic IP |
| Unsolicited inbound internet connections | Possible when configured and permitted | Not allowed to private resources |
| Management | AWS managed | AWS managed |
| Availability | Highly available and redundant | Availability-zone scoped |
| Pricing | No hourly IGW charge; data transfer charges can apply | Hourly and data processing charges apply |

---

# 10. Route Table Configuration

Route tables decide where network traffic should go.

### Public Subnet Route Table

Example:

```text
Destination       Target

10.0.0.0/16       local
0.0.0.0/0         igw-xxxxxxxx
```

This sends internet-bound IPv4 traffic through the Internet Gateway.

### Private Subnet Route Table

Example:

```text
Destination       Target

10.0.0.0/16       local
0.0.0.0/0         nat-xxxxxxxx
```

This sends internet-bound IPv4 traffic through the NAT Gateway.

**Remember:**

```text
PUBLIC SUBNET:
0.0.0.0/0 -> Internet Gateway

PRIVATE SUBNET:
0.0.0.0/0 -> NAT Gateway
```

---

# 11. What Is an Elastic IP?

An Elastic IP is a static public IPv4 address provided by AWS.

A public NAT Gateway requires an Elastic IP address.

Example:

```text
NAT Gateway
     |
     v
Elastic IP: 54.200.10.20
     |
     v
Internet Gateway
     |
     v
Internet
```

The Elastic IP acts as the public source address used by the NAT Gateway for internet communication.

---

# 12. NAT Gateway High Availability

NAT Gateways are created in a specific Availability Zone.

For production environments, AWS recommends deploying NAT Gateways across Availability Zones where needed.

Example:

```text
                 VPC
                  |
       +----------+----------+
       |                     |
       v                     v
Availability Zone A   Availability Zone B
       |                     |
  Public Subnet A       Public Subnet B
       |                     |
  NAT Gateway A         NAT Gateway B
       |                     |
       v                     v
  Private Subnet A      Private Subnet B
       |                     |
    EC2 App A             EC2 App B
```

Benefits:

- Better availability.
- Reduced dependency on another Availability Zone.
- Reduced cross-AZ data transfer charges when workloads use their local NAT Gateway.

---

# 13. IPv6 and Egress-Only Internet Gateway

For IPv6 communication, AWS provides an Egress-Only Internet Gateway.

It allows resources in a VPC to initiate outbound IPv6 communication with the internet.

It prevents the internet from initiating connections to those resources.

Example private subnet IPv6 route:

```text
Destination       Target

::/0              Egress-Only Internet Gateway
```

Important differences:

- NAT Gateway is commonly used for private IPv4 outbound internet access.
- Egress-Only Internet Gateway is used for outbound-only IPv6 internet access.
- An Internet Gateway can support public IPv6 communication when routing and security are configured.

---

# 14. DevOps Real-World Example

Imagine you are deploying an application on AWS using Terraform, Docker, and Kubernetes.

Your architecture:

- Public subnet: Application Load Balancer.
- Public subnet: NAT Gateway.
- Private subnet: EC2 or EKS worker nodes.
- Private subnet: Database.

The application servers need to download Docker images and operating system updates.

But they should not be directly exposed to the internet.

Solution:

1. Attach an Internet Gateway to the VPC.
2. Configure a public route table.
3. Create a public NAT Gateway and associate an Elastic IP.
4. Configure the private subnet route table to use the NAT Gateway.
5. Allow only required traffic through Security Groups and Network ACLs.

Traffic flow:

```text
Private EC2 / EKS Node
          |
          v
     NAT Gateway
          |
          v
    Internet Gateway
          |
          v
       Internet
```

This architecture allows outbound internet connectivity without giving private instances direct public IPv4 internet access.

---

# 15. Common Interview Questions

### Q1. What is an Internet Gateway?

An Internet Gateway is an AWS VPC component that allows communication between a VPC and the internet when routing, public addressing, and security rules are configured.

### Q2. What is a NAT Gateway?

A NAT Gateway allows resources in private subnets to initiate connections to external networks without allowing unsolicited inbound connections directly to those resources.

### Q3. What is the difference between an Internet Gateway and a NAT Gateway?

An Internet Gateway provides a VPC-to-internet connection. A NAT Gateway allows private IPv4 resources to initiate outbound connections through a public NAT Gateway.

### Q4. Can a private subnet access the internet?

Yes. A private subnet can access the IPv4 internet through a NAT Gateway when its route table is configured correctly.

### Q5. Does a NAT Gateway allow inbound connections?

It allows return traffic for connections initiated from inside. It does not allow unsolicited inbound internet connections to private instances.

### Q6. Where do we create a NAT Gateway?

A public NAT Gateway is created in a public subnet.

### Q7. Why does a NAT Gateway need an Elastic IP?

A public NAT Gateway uses an Elastic IP as its public IPv4 address when communicating with the internet.

### Q8. Does an Internet Gateway automatically make a subnet public?

No. The subnet's associated route table must contain an appropriate route to the Internet Gateway. Resources also need suitable public addressing and security rules.

### Q9. Can we use one NAT Gateway for multiple private subnets?

Yes. Multiple private subnet route tables can route traffic through the same NAT Gateway, although this creates an Availability Zone dependency and may introduce cross-AZ charges.

### Q10. What is an Egress-Only Internet Gateway?

It enables outbound-only IPv6 internet connectivity for resources in a VPC.

---

# 16. Key Takeaways

- Internet Gateway connects a VPC to the internet.
- NAT stands for Network Address Translation.
- NAT Gateway allows private IPv4 resources to initiate outbound connections.
- A public NAT Gateway is created in a public subnet.
- A public NAT Gateway requires an Elastic IP.
- Public subnet route: `0.0.0.0/0 -> IGW`.
- Private subnet route: `0.0.0.0/0 -> NAT Gateway`.
- An Internet Gateway does not automatically make every subnet public.
- NAT Gateway does not allow unsolicited inbound internet connections to private resources.
- Egress-Only Internet Gateway is used for outbound-only IPv6 communication.
- Use NAT Gateways in multiple Availability Zones when appropriate for production resilience.

**One-line summary:**

Internet Gateway provides a connection between a VPC and the internet, while NAT Gateway allows private IPv4 resources to initiate outbound internet connections without being directly exposed to unsolicited inbound internet traffic.
