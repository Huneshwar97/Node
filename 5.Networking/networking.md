# FAANG-Level Backend Networking Notes (From Note 1 & Note 2)

## Overview

These notes are structured sequentially from the transcripts in Note 1 and Note 2 and rewritten into:

- FAANG interview quality explanations
- Production-level backend engineering understanding
- Strong conceptual hierarchy
- Practical Node.js networking context

Core focus:

- Computer networking fundamentals
- Internet architecture
- OSI/TCP-IP style layering
- TCP vs UDP
- IP addresses, MAC addresses, routers, switches
- Ports and sockets
- Node.js networking mindset
- Real-world backend system understanding

---

# 1. Why Networking Matters for Backend Engineers

Modern backend systems are fundamentally:

- Distributed systems
- Networked applications
- Remote communication systems

Every backend service:

- Receives requests over a network
- Talks to databases over a network
- Communicates with caches/services over a network
- Streams data over a network
- Uploads/downloads files over a network

Examples:

- WhatsApp messages
- Netflix streaming
- AWS services communication
- Microservice RPC calls
- HTTP APIs
- SSH connections
- File uploads

Node.js was specifically designed for scalable network applications.

---

# 2. Applications Built in the Transcript

The transcript demonstrates two important low-level networking applications:

## 2.1 Chat Application

### Features

- Multiple clients connect to a TCP server
- Clients exchange messages in real time
- Communication happens over TCP
- Users can be anywhere in the world

### Concepts Learned

- TCP sockets
- Client-server architecture
- Ports
- Bidirectional communication
- Persistent connections
- Real-time communication

### Architecture

```text
Client 1 ─┐
          │
Client 2 ─┼── TCP Server
          │
Client 3 ─┘
```

---

## 2.2 File Upload Application

### Features

- Upload large files over TCP
- Stream file data
- Save uploaded file to storage
- Show upload progress

### Concepts Learned

- Streams
- Binary transfer
- Network throughput
- Chunked transfer
- Storage systems
- File integrity

### Architecture

```text
Client File System
       │
       ▼
TCP Upload Stream
       │
       ▼
Server Storage
```

---

# 3. Evolution of Computer Communication

## 3.1 Before Networking

Before networking existed:

- Computers were isolated
- No direct communication
- Data transfer required physical media

Examples:

- Floppy disks
- CDs/DVDs
- USB drives

### Workflow

```text
Computer A
   ↓
Write to disk
   ↓
Physically carry disk
   ↓
Computer B
```

Problems:

- Extremely slow
- Not scalable
- Not practical
- Impossible for real-time communication

---

# 4. Basic Networking Infrastructure

Networking evolved by introducing:

## Components

### 1. Ethernet Cables

Physical transmission medium.

### 2. Network Interface Cards (NIC)

Hardware that enables devices to connect to networks.

### 3. Switches

Connect multiple devices inside the same network.

### 4. Routers

Connect multiple networks together.

---

# 5. Network Interface Card (NIC)

Every network-enabled device contains a NIC.

Examples:

- Laptop
- Phone
- Server
- Router

Responsibilities:

- Send/receive bits
- Connect device to network
- Own a unique MAC address

---

# 6. MAC Address

## Definition

A MAC (Media Access Control) address is:

- A hardware identifier
- Unique per NIC
- Used in local network communication

Example:

```text
00:1A:2B:3C:4D:5E
```

Characteristics:

- 48 bits
- Represented in hexadecimal
- Burned into hardware

---

# 7. What a Switch Does

A switch connects devices inside a local network.

## Switch Responsibilities

It learns:

```text
MAC Address → Physical Port
```

Example:

```text
Port 1 → Laptop
Port 2 → iPad
Port 3 → Printer
```

When a frame arrives:

1. Switch reads destination MAC address
2. Finds corresponding port
3. Forwards frame only there

This is efficient local delivery.

---

# 8. Frames and Packets

Important interview distinction.

## Frame

Used at:

- Data Link Layer

Contains:

- Source MAC
- Destination MAC
- Data

## Packet

Used at:

- Network Layer

Contains:

- Source IP
- Destination IP
- Payload

---

# 9. Why Switches Alone Are Not Enough

Switches work only inside local networks.

Problems:

- Cannot scale globally
- Massive cable complexity
- Impossible to manage internet-scale routing

Solution:

## Routers

Routers connect:

```text
Network ↔ Network
```

---

# 10. Routers

Routers operate using:

- IP addresses
- Routing tables
- Network forwarding logic

Responsibilities:

- Connect different networks
- Assign IP addresses
- Forward packets globally
- Determine optimal path

---

# 11. IP Address

## Definition

Logical network identifier for devices.

Example:

```text
192.168.1.5
```

Structure:

```text
Network Portion + Host Portion
```

Example:

```text
192.168.1.x
```

Devices sharing first portion belong to same subnet/network.

---

# 12. The Internet

The internet is:

```text
A gigantic network of interconnected networks.
```

Connected using:

- Fiber optic cables
- Routers
- Switches
- Submarine cables
- ISPs

Data physically travels across:

- Countries
- Oceans
- Continents

Important realization:

Internet communication is physical.

Your messages are converted into bits and transported through actual infrastructure.

---

# 13. ISP (Internet Service Provider)

Examples:

- Jio
- Airtel
- Verizon
- Comcast

Responsibilities:

- Connect users to internet backbone
- Provide routing access
- Assign public IPs

Without ISPs:

- Your machine cannot access internet

---

# 14. Networking Layers

Extremely important FAANG topic.

The transcript explains layered networking architecture.

---

# 15. Physical Layer

## Responsibility

Move raw bits.

## Works With

- Cables
- Electrical signals
- Light pulses
- Radio waves

## Data Unit

```text
Bits (0s and 1s)
```

The physical layer knows NOTHING about:

- IP addresses
- MAC addresses
- Applications
- Packets

It only transports signals.

---

# 16. Data Link Layer

## Responsibility

Local network communication.

## Uses

- Switches
- MAC addresses
- Frames

## Responsibilities

- Device-to-device communication
- Error detection
- Frame delivery inside LAN

---

# 17. Network Layer

## Responsibility

Global routing.

## Uses

- IP addresses
- Routers
- Packets

## Responsibilities

- Route packets between networks
- Determine destination path
- Enable internet communication

Key protocol:

```text
IP (Internet Protocol)
```

---

# 18. Transport Layer

One of the MOST important backend concepts.

## Responsibility

Reliable process-to-process communication.

The transport layer:

- Ensures delivery
- Detects missing packets
- Detects corruption
- Handles retransmission
- Uses ports

Main protocols:

- TCP
- UDP

---

# 19. Why Transport Layer Exists

Network layer alone is insufficient.

Problems without transport layer:

1. Packet Loss
2. Duplicate Packets
3. Corruption
4. Ordering Problems
5. Connection Failure

Transport layer solves these.

---

# 20. TCP (Transmission Control Protocol)

## Goal

Reliable communication.

TCP guarantees:

- Ordered delivery
- Reliable delivery
- Retransmission
- Error detection
- Connection management

Used by:

- HTTP
- HTTPS
- SSH
- Databases
- File uploads

---

# 21. TCP Characteristics

TCP is:

- Connection-oriented
- Reliable
- Stateful
- Ordered

Tradeoff:

```text
Reliability > Speed
```

---

# 22. TCP Three-Way Handshake

Connection establishment process.

1. SYN
2. SYN-ACK
3. ACK

After this:

```text
TCP connection established
```

---

# 23. TCP Sequence Numbers

Every TCP packet is numbered.

Example:

```text
Packet 1
Packet 2
Packet 3
```

If packet 2 is missing:

- Receiver detects gap
- Sender retransmits packet 2

This enables reliability.

---

# 24. TCP Acknowledgements

Receiver continuously responds:

```text
ACK 1
ACK 2
ACK 3
```

Meaning:

```text
I received packet successfully
```

Missing ACKs trigger retransmission.

---

# 25. TCP Window Size

TCP dynamically adjusts throughput.

Initially:

- Small packets

Gradually:

- Larger packets
- Higher throughput

Concept:

```text
Flow Control
```

---

# 26. UDP (User Datagram Protocol)

UDP prioritizes speed over reliability.

Characteristics:

- Connectionless
- No retransmission
- No ordering guarantee
- Minimal overhead

Tradeoff:

```text
Speed > Reliability
```

---

# 27. UDP Use Cases

Perfect for:

- Video streaming
- Gaming
- Voice calls
- Live streaming
- Real-time telemetry

Reason:

Losing a few frames is acceptable.

Retransmission would create lag.

---

# 28. TCP vs UDP

| Feature        | TCP                 | UDP               |
| -------------- | ------------------- | ----------------- |
| Reliable       | Yes                 | No                |
| Ordered        | Yes                 | No                |
| Retransmission | Yes                 | No                |
| Speed          | Slower              | Faster            |
| Connection     | Connection-oriented | Connectionless    |
| Use Cases      | APIs, DBs, uploads  | Streaming, gaming |

---

# 29. TCP Headers

Important TCP fields:

- Source Port
- Destination Port
- Sequence Number
- Acknowledgement Number
- Window Size
- Flags (SYN, ACK, FIN)
- Checksum

---

# 30. UDP Headers

UDP is much simpler.

Contains:

- Source port
- Destination port
- Length
- Checksum

Fixed size:

```text
8 bytes
```

---

# 31. Ports

One machine can run many applications.

| Application | Port |
| ----------- | ---- |
| HTTP        | 80   |
| HTTPS       | 443  |
| SSH         | 22   |
| Node App    | 3000 |

Ports identify:

```text
Which application should receive data
```

---

# 32. Socket

A socket uniquely identifies a connection.

Typically:

```text
IP Address + Port
```

Example:

```text
192.168.1.5:3000
```

---

# 33. Loopback Address

Special IP:

```text
127.0.0.1
```

Meaning:

```text
This machine itself
```

Aliases:

- localhost
- loopback interface

---

# 34. localhost

```text
localhost = 127.0.0.1
```

If server binds to localhost:

- Only same machine can access it

External devices cannot.

---

# 35. Private Network Example

```text
Laptop ↔ Router ↔ iPad
```

Router assigns private IPs:

```text
192.168.x.x
172.x.x.x
10.x.x.x
```

Devices communicate directly inside same LAN.

---

# 36. Node.js HTTP Server Example

```js
const http = require("http");

const port = 4080;
const hostname = "127.0.0.1";

const server = http.createServer((req, res) => {
  const data = { message: "Hi there!" };

  res.setHeader("Content-Type", "application/json");
  res.statusCode = 200;
  res.end(JSON.stringify(data));
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}`);
});
```

---

# 37. Important Production Insight

Binding to:

```js
127.0.0.1
```

Means:

- Local-only access

Binding to:

```js
0.0.0.0
```

Means:

- Accessible from network
- External devices can connect

Critical in:

- Docker
- Kubernetes
- AWS deployments
- EC2 servers

---

# 38. Why Servers Are Called Servers

A server is simply:

```text
A machine listening on a port
and responding to requests.
```

Your laptop can be a server.

---

# 39. Real Backend Engineering Understanding

Modern backend systems involve:

- Layer 7 → Application logic
- Layer 4 → TCP/UDP
- Layer 3 → IP routing
- Layer 2 → Frames/MAC
- Layer 1 → Physical transmission

Senior engineers understand:

- Failures at every layer
- Performance bottlenecks
- Latency
- Reliability tradeoffs
- Packet loss
- Throughput
- Connection handling

---

# 40. FAANG-Level Interview Insights

## Explain TCP vs UDP

Must discuss:

- Reliability
- Ordering
- Retransmission
- Speed tradeoff

## Explain How HTTP Works

```text
HTTP
  runs on
TCP
  runs on
IP
  runs on
Ethernet/WiFi
```

## Explain What Happens When Visiting a Website

1. DNS lookup
2. Get server IP
3. TCP handshake
4. TLS handshake
5. HTTP request
6. Server response
7. Browser rendering

---

# 41. Core Backend Mental Model

Backend engineering is fundamentally:

```text
Moving data reliably between machines.
```

Everything eventually becomes:

```text
Bits transmitted across networks.
```

---

# 42. Key Takeaways

## Networking Stack

```text
Application Layer
Transport Layer (TCP/UDP)
Network Layer (IP)
Data Link Layer (MAC/Switch)
Physical Layer (Bits/Cables)
```

## TCP

Use when reliability matters.

Examples:

- APIs
- Authentication
- File uploads
- Banking

## UDP

Use when speed matters.

Examples:

- Video streaming
- Gaming
- Voice calls

---

# 43. Recommended Advanced Topics After This

## Networking

- DNS
- HTTP/HTTPS
- TLS/SSL
- NAT
- Subnetting
- CIDR
- Load balancing
- Reverse proxies
- WebSockets
- QUIC
- HTTP/2
- HTTP/3

## Systems

- Linux networking
- epoll/kqueue
- TCP tuning
- Connection pooling
- CDN architecture
- Distributed systems

## Node.js Internals

- libuv
- Event loop
- Streams
- Backpressure
- Cluster mode
- Socket programming

---

# 44. Final Mental Model

```text
A request is not magic.

It is:
1. Broken into packets
2. Routed across the world
3. Reassembled
4. Processed by an application
5. Returned the same way
```

Understanding this deeply separates:

```text
Framework developers
from
real systems engineers.
```
