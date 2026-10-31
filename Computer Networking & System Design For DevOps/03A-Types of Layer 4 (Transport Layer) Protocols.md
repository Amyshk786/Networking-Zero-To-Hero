# Transport Layer (Layer 4) — TCP vs UDP

## 1. What is the Transport Layer?

The Transport Layer is Layer 4 of the OSI model.

It is responsible for communication between applications running on different devices.

For example:
- A browser communicates with a web server.
- An SSH client communicates with a remote Linux server.
- An email client communicates with a mail server.

The Transport Layer uses port numbers to identify applications and services.

Two important Transport Layer protocols are:

- **TCP (Transmission Control Protocol)**
- **UDP (User Datagram Protocol)**

---

## 2. TCP (Transmission Control Protocol)

TCP is a connection-oriented and reliable transport protocol.

Before sending application data, TCP establishes a connection between the client and the server.

TCP provides:
- Reliable data delivery
- Data ordering
- Acknowledgements (ACK)
- Retransmission of lost data
- Flow control

### TCP 3-Way Handshake

The TCP 3-way handshake is used to establish a connection between a client and a server.

Imagine the client wants to communicate with a server. Before starting the conversation, both sides confirm that they are ready.

The handshake consists of three steps:

### Step 1: SYN (Client → Server)

The client sends a SYN packet to the server.

It means:

"Hello server, I want to establish a TCP connection."

### Step 2: SYN-ACK (Server → Client)

The server responds with a SYN-ACK packet.

It means:

"I received your request, and I am ready to establish the connection."

### Step 3: ACK (Client → Server)

The client sends an ACK packet.

It means:

"I received your response. Let's start communicating."

After these three steps, the TCP connection is established, and application data can be exchanged.

### Simple TCP Handshake Diagram

```text
       CLIENT                         SERVER
         |                               |
         | -------- SYN ---------------> |
         |                               |
         | <----- SYN + ACK ------------ |
         |                               |
         | -------- ACK ---------------> |
         |                               |
         |      TCP CONNECTION           |
         |        ESTABLISHED           |
         |                               |
         | <---- Application Data -----> |
```

**Remember:** The handshake establishes the connection. Application data is sent after the connection is established.

---

## 3. How Does TCP Handle Data Loss?

TCP is designed to provide reliable and ordered data delivery.

When data is sent using TCP:

1. The data is divided into smaller segments.
2. TCP assigns sequence numbers to the data.
3. The receiver acknowledges received data using ACKs.
4. If data is lost, TCP can retransmit it.
5. The receiver receives the data in the correct order.

### Example

Imagine a client sends three TCP segments:

```text
Client sends:
Segment 1
Segment 2
Segment 3
```

Suppose Segment 2 gets lost during transmission.

```text
Client                 Server
  |                       |
  |------ Segment 1 -----> |
  |<--------- ACK --------- |
  |------ Segment 2 -X     |
  |------ Segment 3 -----> |
  |                       |
  |   Segment 2 missing   |
  |                       |
  |<-- ACK / recovery ---- |
  |                       |
  |---- Retransmit ------> |
  |                       |
  |   Data recovered      |
```

TCP can detect missing data and retransmit it.

### Does TCP mean there is no data loss?

Not exactly.

TCP provides mechanisms to recover from data loss, but it cannot guarantee that a connection will always succeed.

If the network or server becomes unavailable, TCP may eventually fail and report an error to the application.

**Key point:** TCP is reliable because it detects and recovers from many transmission problems, not because data can never be lost.

---

## 4. Applications and Protocols That Use TCP

| Application / Protocol | Transport Protocol |
|---|---|
| SSH | TCP |
| HTTP/1.1 | TCP |
| HTTP/2 | TCP |
| SMTP | TCP |
| HTTPS using HTTP/1.1 or HTTP/2 | TCP |

### Example: SSH

When connecting to a remote Linux server using SSH:

```bash
ssh user@server-ip
```

SSH normally uses TCP port 22.

TCP establishes the connection before SSH begins its secure communication.

### Example: HTTP and HTTPS

HTTP/1.1 and HTTP/2 commonly use TCP.

HTTPS adds TLS encryption to HTTP communication.

**Important:** HTTP/3 uses QUIC, which runs over UDP instead of TCP.

---

## 5. UDP (User Datagram Protocol)

UDP is a connectionless transport protocol.

Unlike TCP, UDP does not establish a connection using a three-way handshake.

The sender can send a UDP datagram without first establishing a TCP-style connection.

UDP provides:
- Low protocol overhead
- Fast transmission
- No built-in delivery guarantee
- No built-in retransmission
- No built-in ordering guarantee

### How UDP Communication Works

```text
       CLIENT                         SERVER
         |                               |
         | ---- UDP Datagram ----------> |
         |                               |
         |                         Receives data
         |                               |
         | <--- UDP Response ----------  |
         |                               |
```

The server may receive the datagram and process it.

However, UDP itself does not guarantee that the datagram will reach the server.

### Does UDP always lose data?

No.

UDP datagrams can arrive successfully.

The difference is that UDP itself does not provide mechanisms to guarantee delivery or recover lost datagrams.

If a datagram is lost:
- UDP does not automatically retransmit it.
- UDP does not automatically notify the sender.
- The application may implement its own recovery mechanisms.

---

## 6. Examples of Applications and Protocols That Use UDP

| Application / Protocol | Transport Protocol |
|---|---|
| DNS (commonly) | UDP |
| DHCP | UDP |
| Online gaming | UDP |
| Real-time voice and video | UDP |
| QUIC | UDP |

DNS commonly uses UDP port 53. Some DNS queries and responses use TCP when needed.

QUIC uses UDP as its transport and provides reliability and other features at a higher protocol layer. HTTP/3 uses QUIC.

---

## 7. TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| 3-way handshake | Yes | No |
| Delivery guarantee | Reliable delivery mechanisms | No built-in guarantee |
| Acknowledgements | Yes | No built-in acknowledgements |
| Retransmission | Yes | No built-in retransmission |
| Data ordering | Yes | No built-in ordering |
| Overhead | Higher | Lower |
| Examples | SSH, SMTP, HTTP/1.1, HTTP/2 | DNS, DHCP, gaming, QUIC |

---

## 8. Simple Real-Life Analogy

### TCP = Courier Service

Imagine sending an important parcel using a courier.

- The courier tracks the parcel.
- Delivery is confirmed.
- If delivery fails, the courier may try again.
- You expect the parcel to reach its destination.

This is similar to TCP's reliable delivery mechanisms.

### UDP = Sending a Quick Announcement

Imagine making an announcement over a loudspeaker.

- You send the message.
- People who hear it receive it.
- You do not individually confirm that everyone heard it.
- You do not automatically repeat it if someone missed it.

This is similar to UDP.

---

## 9. Useful Commands for Networking Practice

### Check SSH connection

```bash
ssh user@server-ip
```

### Check DNS using UDP

```bash
dig github.com
```

### Check HTTP response

```bash
curl -I https://github.com
```

### Check network route

```bash
traceroute github.com
```

These commands can help you understand how applications communicate over networks.

---

## 10. Key Takeaways

- The Transport Layer is Layer 4 of the OSI model.
- TCP establishes a connection using a 3-way handshake.
- TCP provides reliable and ordered data delivery using acknowledgements and retransmission.
- TCP cannot guarantee successful communication if the connection or network fails.
- UDP does not use a TCP-style 3-way handshake.
- UDP does not provide built-in delivery or ordering guarantees.
- SSH, SMTP, HTTP/1.1, and HTTP/2 use TCP.
- HTTP/3 uses QUIC over UDP.
- TCP and UDP both use port numbers to identify applications and services.

**One-line summary:**

TCP establishes a connection and provides reliable, ordered data delivery. UDP sends datagrams without a connection handshake and does not provide built-in delivery guarantees.
