# Internet Addressing - Internet addressing is the system used to identify devices, networks, and services so that data can travel from a source to a destination across the Internet.

For example, when you open GitHub on your Mac mini M4, several addressing systems work together to connect your browser to GitHub's servers.



IPv4 has about 4.3 billion addresses · 32 bits

IPv6 has 340 undecillion
About (3.4 x 10³⁸) addresses · 128 bits



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

<img width="474" height="238" alt="image" src="https://github.com/user-attachments/assets/d0165b80-7aac-40c9-96d9-6cab9e1fb5a1" />
