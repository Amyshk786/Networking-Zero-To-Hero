# How Security Works on the Internet

## 1. Introduction to Internet Security

Internet security protects communication between devices, applications, and servers.

When we access a website, connect to a remote server, or send data over a network, that data travels through networks that we may not control.

Security helps protect our systems and data from:

- Unauthorized access
- Data theft
- Data modification
- Malicious network traffic
- Unauthorized connections

A **firewall** is one of the important components used to protect networks and systems.

Other security technologies include TLS, encryption, authentication, VPNs, and access controls.

---

## 2. What Is a Firewall?

A firewall is a security system that controls network traffic based on configured rules.

A firewall can allow or block traffic depending on:

- Source IP address
- Destination IP address
- Protocol (TCP, UDP, ICMP)
- Port number
- Direction of traffic

### Example

Imagine a Linux server running a web application.

The server has:

- SSH service on port 22
- HTTP service on port 80
- HTTPS service on port 443

A firewall can be configured to:

- Allow SSH only from an administrator's IP address.
- Allow HTTP and HTTPS from the internet.
- Block unnecessary incoming connections.

This helps reduce unauthorized access to the server.

---

## 3. Ingress and Egress

Firewall rules commonly use two traffic directions:

### Ingress

Ingress means **incoming traffic**.

It is traffic entering a resource or network.

Examples:

- A user connecting to a website hosted on your server.
- An administrator connecting to a Linux server using SSH.
- A client sending a request to an application.

### Egress

Egress means **outgoing traffic**.

It is traffic leaving a resource or network.

Examples:

- A server downloading software updates.
- An application connecting to a database.
- A server sending a request to an external API.

### Simple Diagram

```text
                  INTERNET
                     |
                     |
            Incoming Traffic
                     |
                     v
              +-------------+
              |  FIREWALL  |
              +-------------+
                     |
                     v
              +-------------+
              |   SERVER    |
              +-------------+
                     |
                     |
            Outgoing Traffic
                     |
                     v
                  INTERNET
```

---

## 4. Inbound and Outbound Security Rules

Firewall rules define which network traffic is allowed or blocked.

### Inbound Rules

Inbound rules control traffic entering a resource.

Example:

| Protocol | Port | Source | Action |
|---|---:|---|---|
| TCP | 22 | Admin IP | Allow |
| TCP | 80 | 0.0.0.0/0 | Allow |
| TCP | 443 | 0.0.0.0/0 | Allow |
| TCP | 3306 | 0.0.0.0/0 | Block |

Explanation:

- Port 22 is used for SSH.
- Port 80 is used for HTTP.
- Port 443 is used for HTTPS.
- Port 3306 is commonly used by MySQL.

The MySQL port should not normally be open to the entire internet.

### Outbound Rules

Outbound rules control traffic leaving a resource.

Example:

| Protocol | Port | Destination | Action |
|---|---:|---|---|
| TCP | 443 | Internet | Allow |
| UDP | 53 | DNS resolver | Allow |
| TCP | 3306 | Database server | Allow, if required |

Outbound rules help control which external systems a server or application can communicate with.

**Note:** Firewall behavior depends on the firewall platform. Stateful firewalls can automatically allow return traffic for established connections. Other firewalls may require separate rules.

---

## 5. What Is a Port Number?

A port number identifies a service endpoint on a device.

An IP address identifies a device or network interface, while a port number helps identify the application or service communicating on that device.

### Simple Example

Imagine an apartment building:

- IP address = Building address
- Port number = Apartment number
- Application = Person living in the apartment

Example:

```text
Server IP: 192.168.1.10

SSH service:   192.168.1.10:22
HTTP service:  192.168.1.10:80
HTTPS service: 192.168.1.10:443
```

The IP address identifies the server, while the port number identifies the service endpoint.

Port numbers are used with transport protocols such as TCP and UDP.

---

## 6. Protocol : IP Address : Port Number

To communicate with a service, a client commonly needs:

- Protocol
- Destination IP address
- Destination port number

For example:

```text
TCP : 192.168.1.10 : 22
```

This means:

- Protocol: TCP
- Destination IP: 192.168.1.10
- Destination port: 22 (SSH)

Another example:

```text
TCP : 192.168.1.20 : 443
```

This means a TCP connection to the server at `192.168.1.20` on port `443`.

### Example: Connecting to a Web Server

```text
Client
  |
  | TCP connection
  | Destination IP: 192.168.1.20
  | Destination Port: 443
  |
  v
Web Server
```

The client uses the destination IP address and port number to reach the service.

**Important:** A port number by itself does not make a service secure. Security also depends on firewall rules, authentication, encryption, and service configuration.

---

## 7. Important Port Number Cheat Sheet for DevOps

| Port | Protocol | Service | Common Purpose |
|---:|---|---|---|
| 20/21 | TCP | FTP | File transfer |
| 22 | TCP | SSH | Remote server access |
| 25 | TCP | SMTP | Email transfer |
| 53 | TCP/UDP | DNS | Domain name resolution |
| 67/68 | UDP | DHCP | IP address assignment |
| 80 | TCP | HTTP | Web traffic |
| 110 | TCP | POP3 | Email retrieval |
| 123 | UDP | NTP | Time synchronization |
| 143 | TCP | IMAP | Email access |
| 161/162 | UDP | SNMP | Network monitoring |
| 389 | TCP/UDP | LDAP | Directory services |
| 443 | TCP/UDP | HTTPS | Secure web traffic; HTTP/3 uses UDP |
| 445 | TCP | SMB | File sharing |
| 465 | TCP | SMTP over TLS | Secure email submission |
| 514 | UDP | Syslog | Log forwarding |
| 587 | TCP | SMTP Submission | Email submission |
| 636 | TCP | LDAPS | LDAP over TLS |
| 993 | TCP | IMAPS | Secure email access |
| 995 | TCP | POP3S | Secure email retrieval |
| 1433 | TCP | MS SQL Server | Database |
| 1521 | TCP | Oracle DB | Database |
| 2049 | TCP/UDP | NFS | Network file system |
| 2375 | TCP | Docker API | Docker remote API, unencrypted |
| 2376 | TCP | Docker API over TLS | Secure Docker remote API |
| 3000 | TCP | Common web app port | Development applications |
| 3306 | TCP | MySQL | Database |
| 3389 | TCP | RDP | Remote Windows desktop |
| 5432 | TCP | PostgreSQL | Database |
| 5672 | TCP | AMQP | RabbitMQ messaging |
| 6379 | TCP | Redis | In-memory data store |
| 6443 | TCP | Kubernetes API | Kubernetes control plane |
| 8080 | TCP | HTTP alternate | Web apps and proxies |
| 8443 | TCP | HTTPS alternate | Secure web apps |
| 9090 | TCP | Common service port | Prometheus commonly uses this |
| 9092 | TCP | Kafka | Kafka broker |
| 9200 | TCP | Elasticsearch | Elasticsearch API |
| 9418 | TCP | Git | Git protocol |
| 10250 | TCP | Kubelet | Kubernetes node agent |
| 30000–32767 | TCP/UDP | Kubernetes NodePort range | Services exposed through NodePort |

**Note:** Ports are conventions, not guarantees. Services can be configured to listen on other ports. Verify actual configuration before creating firewall rules.

---

## 8. Common Ports to Remember for DevOps Interviews

| Port | Remember It For |
|---:|---|
| 22 | SSH |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 3306 | MySQL |
| 5432 | PostgreSQL |
| 6379 | Redis |
| 6443 | Kubernetes API |
| 8080 | Common application port |
| 9090 | Prometheus |
| 9092 | Kafka |
| 9200 | Elasticsearch |

---

## 9. What Is SSL?

SSL stands for **Secure Sockets Layer**.

SSL was an older protocol designed to secure communication over networks.

SSL is now obsolete and should not be used for modern secure communication.

The modern replacement is TLS.

People often say "SSL certificate" even when the certificate is used with TLS.

---

## 10. What Is TLS?

TLS stands for **Transport Layer Security**.

TLS protects communication between a client and a server.

It provides:

- Encryption
- Integrity protection
- Server authentication

Example:

```text
Browser
   |
   | HTTPS request
   | Encrypted using TLS
   |
   v
Web Server
```

When a browser connects to an HTTPS website:

1. The browser connects to the server.
2. The server presents its TLS certificate.
3. The browser verifies the certificate.
4. The browser and server establish cryptographic session keys.
5. Encrypted communication begins.

### SSL vs TLS

| SSL | TLS |
|---|---|
| Older security protocol | Modern security protocol |
| Obsolete | Used today |
| Should not be enabled | Recommended secure protocol |
| Often used casually to refer to certificates | Technically correct name for modern security |

**Key point:** When someone says "SSL certificate," they usually mean a certificate used for TLS.

---

## 11. DevOps Example: Securing a Web Application

Imagine a three-tier application:

```text
                  INTERNET
                      |
                      v
               Load Balancer
                HTTPS :443
                      |
                      v
             Application Server
                  TCP :8080
                      |
                      v
                 Database
                  TCP :5432
```

Example firewall rules:

| Resource | Inbound Rule | Source |
|---|---|---|
| Load Balancer | TCP 443 | Internet |
| Application Server | TCP 8080 | Load Balancer |
| Database | TCP 5432 | Application Server |

The database should not be publicly accessible.

The application server should accept traffic only from the load balancer or other trusted application components.

This follows the principle of **least privilege**: allow only the traffic that is required.

---

## 12. Useful Linux Commands

### Check listening ports

```bash
sudo ss -tulnp
```

### Check a specific port

```bash
sudo lsof -i :8080
```

### Check whether a port is reachable

```bash
nc -vz example.com 443
```

### Check HTTP headers

```bash
curl -I https://example.com
```

### Check TLS certificate information

```bash
openssl s_client -connect example.com:443 -servername example.com
```

### Check firewall status on Ubuntu

```bash
sudo ufw status
```

---

## 13. Key Takeaways

- A firewall controls network traffic using rules.
- Ingress means incoming traffic.
- Egress means outgoing traffic.
- Inbound rules control traffic entering a resource.
- Outbound rules control traffic leaving a resource.
- An IP address identifies a device or network interface.
- A port number identifies a service endpoint.
- Protocol : IP : Port helps identify how to reach a service.
- TLS is the modern protocol used to secure communication.
- SSL is obsolete.
- Firewall rules should follow the principle of least privilege.
- Database ports should not normally be exposed to the public internet.

**One-line summary:**

Internet security uses firewalls, TLS, authentication, and access controls to protect systems and communication. Firewall rules determine which network traffic is allowed to enter or leave a resource.
