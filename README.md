# 🌐 Networking for Cloud + DevOps — Beginner Guide

> A practical, example-driven guide to the networking concepts every Cloud/DevOps engineer needs — from OSI layers to AWS VPC architecture.

---

## 📖 Table of Contents

1. [OSI Model](#1-osi-model-)
2. [TCP/IP Model](#2-tcpip-model-)
3. [IP Address](#3-ip-address-)
4. [Public vs Private IP](#4-public-vs-private-ip-)
5. [IPv4](#5-ipv4-)
6. [CIDR](#6-cidr-)
7. [Subnetting](#7-subnetting-)
8. [MAC Address](#8-mac-address)
9. [Ports](#9-ports-)
10. [TCP vs UDP](#10-tcp-vs-udp-)
11. [HTTP](#11-http-)
12. [HTTPS](#12-https-)
13. [DNS](#13-dns-)
14. [DHCP](#14-dhcp-)
15. [NAT](#15-nat-)
16. [Routing](#16-routing-)
17. [Firewall](#17-firewall-)
18. [SSH](#18-ssh-)
19. [Load Balancing](#19-load-balancing-)
20. [Proxy vs Reverse Proxy](#20-proxy-vs-reverse-proxy-)
21. [TLS / SSL](#21-tls--ssl-)
22. [Common Ports](#22-common-ports-)
23. [Putting It All Together](#-putting-everything-together)
24. [Mental Model](#-the-most-important-mental-model)
25. [What to Master](#-what-you-should-master-for-cloud--devops)

---

## Getting Started

**Networking = How computers communicate with each other.**

When you open `https://google.com`, many concepts work together at once:

```
Your laptop → Wi-Fi/router → DNS → Internet → Google's servers → HTTPS → response back
```

---

## 1. OSI Model ⭐⭐⭐

The OSI model explains networking in **7 layers**. Think of sending a parcel.

| Layer | Name         | Simple Meaning                |
|:-----:|--------------|--------------------------------|
| 7     | Application  | What the user/application uses |
| 6     | Presentation | Data format/encryption         |
| 5     | Session      | Starts/manages communication   |
| 4     | Transport    | TCP/UDP, ports                 |
| 3     | Network      | IP addresses/routing           |
| 2     | Data Link    | MAC addresses                  |
| 1     | Physical     | Cables, signals, Wi-Fi         |

**Mnemonic:** *All People Seem To Need Data Processing* (A P S T N D P)

**Example** — opening `https://example.com`:

- Layer 7 → HTTP/HTTPS
- Layer 4 → TCP
- Layer 3 → IP
- Layer 2 → MAC
- Layer 1 → Wi-Fi/Ethernet signals

> 💡 **For Cloud/DevOps:** Don't memorize every theoretical detail. Focus on Layer 4 (TCP/UDP/Ports), Layer 3 (IP/Subnet/Routing), Layer 2 (MAC), and Layer 7 (HTTP/HTTPS/DNS).

---

## 2. TCP/IP Model ⭐⭐⭐

The practical model used on the real Internet — usually shown as **4 layers**:

| TCP/IP Layer  | Examples             |
|---------------|-----------------------|
| Application   | HTTP, HTTPS, DNS, SSH |
| Transport     | TCP, UDP              |
| Internet      | IP                    |
| Network Access| Ethernet, Wi-Fi, MAC  |

**Compare:** OSI = 7 layers | TCP/IP = 4 layers (more practical for real-world networking)

---

## 3. IP Address ⭐⭐⭐

An IP address identifies a device/interface on a network — like a house address.

**Example:** `192.168.1.10`

If Computer A (`192.168.1.10`) wants to talk to Computer B (`192.168.1.20`), it needs to know where B is — and networking rules must allow the communication.

---

## 4. Public vs Private IP ⭐⭐⭐

Very important in AWS.

### Private IP
Used inside a private network, **not directly reachable from the public Internet**.

**Example:** `10.0.1.10` or `192.168.1.20`

### Public IP
Reachable through the Internet, provided firewall/security rules allow it.

**Example:** `13.x.x.x`

```
Internet
   |
Public IP
   |
  EC2
   |
Private IP
   |
  VPC
```

An EC2 instance may have: Private IP `10.0.1.25` + Public IP `13.x.x.x`

---

## 5. IPv4 ⭐⭐⭐

The most common IP addressing system — uses **32 bits**.

**Example:** `192.168.1.10` — four numbers (0–255) separated by dots.

- Range: `0.0.0.0` → `255.255.255.255`
- About **4.3 billion** possible addresses
- Because IPv4 is limited, **IPv6** was created

---

## 6. CIDR ⭐⭐⭐

**CIDR = Classless Inter-Domain Routing.** Seen everywhere in AWS.

**Example:** `10.0.0.0/16` — the `/16` tells you how many bits represent the network portion.

- `10.0.0.0/16` → a relatively **large** network
- `10.0.1.0/24` → a **smaller** network

> 💡 Don't worry about calculating this mentally at first — just understand that CIDR notation defines the **size/range** of an IP network.

---

## 7. Subnetting ⭐⭐⭐

**Subnetting = dividing one large network into smaller networks.**

Imagine a large apartment building divided into floors. Similarly, `10.0.0.0/16` could become:

- `10.0.1.0/24`
- `10.0.2.0/24`
- `10.0.3.0/24`

In AWS, you'll commonly create:

```
VPC
 ├── Public Subnet
 ├── Private Subnet
 └── Database Subnet
```

**Why subnet?** Security, organization, routing, isolation, application architecture.

**Example layout:**

```
VPC:              10.0.0.0/16
Public Subnet:    10.0.1.0/24
Private Subnet:   10.0.2.0/24
Database Subnet:  10.0.3.0/24
```

---

## 8. MAC Address

**MAC = Media Access Control.** Identifies a network interface at the Data Link layer.

**Example:** `00:1A:2B:3C:4D:5E`

> **IP** = logical address | **MAC** = hardware/network-interface address

A laptop might have: IP `192.168.1.10` + MAC `AA:BB:CC:DD:EE:FF`. MAC addresses matter mainly within the **local network**.

---

## 9. Ports ⭐⭐⭐

Extremely important for DevOps.

> **IP** = building address | **Port** = apartment number

**Example:** `192.168.1.10:80` → go to that machine, service on port 80.

A server can run multiple services at once:

```
Server
 ├── 22   SSH
 ├── 80   HTTP
 ├── 443  HTTPS
 └── 3306 MySQL
```

There are **65,536** TCP/UDP port numbers (0–65535).

---

## 10. TCP vs UDP ⭐⭐⭐

Both operate at the Transport layer.

- **TCP** — connection-oriented and reliable; ensures data arrives correctly and in order. *Think: sending a registered parcel with confirmation.*
- **UDP** — connectionless and faster, but no delivery guarantee. *Think: throwing a ball without waiting for confirmation.*

| TCP                  | UDP                        |
|-----------------------|-----------------------------|
| Reliable              | Less reliable              |
| Connection-oriented   | Connectionless              |
| Ordered delivery      | No delivery guarantee       |
| More overhead         | Less overhead               |
| Used by SSH, HTTPS    | Used by DNS, streaming/gaming |

---

## 11. HTTP ⭐⭐⭐

**HTTP = HyperText Transfer Protocol** — communication between web clients and servers.

```
Browser --(HTTP Request)--> Web Server
Browser <--(HTTP Response)-- Web Server
```

**Example:** `GET /index.html` → server responds `200 OK`

Default port: **80**

---

## 12. HTTPS ⭐⭐⭐

**HTTPS = HTTP + TLS encryption.**

**Example:** `https://example.com` — default port **443**

Protects communication against attackers reading or modifying traffic in transit.

---

## 13. DNS ⭐⭐⭐

**DNS = Domain Name System** — converts a domain name into an IP address. *Think: the Internet's phonebook.*

```
www.example.com  →  DNS lookup  →  IP address  →  Browser connects
```

---

## 14. DHCP ⭐⭐

**DHCP = Dynamic Host Configuration Protocol** — automatically assigns network configuration to devices.

When your laptop joins Wi-Fi, DHCP can provide:

- IP address
- Subnet mask
- Default gateway
- DNS server

Without DHCP, you'd configure all of this manually.

---

## 15. NAT ⭐⭐⭐

**NAT = Network Address Translation** — translates between private and public IP addresses (needed because private IPs can't be directly routed over the public Internet).

```
Laptop (Private IP 192.168.1.10)
        ↓
   Router/NAT
        ↓
    Internet (Public IP)
```

**AWS example:**

```
Private EC2 → Private Subnet → NAT Gateway → Internet Gateway → Internet
```

This lets a private EC2 instance make outbound Internet requests **without** having its own public IP.

---

## 16. Routing ⭐⭐⭐

**Routing = deciding where network traffic should go.** Routers examine destination IPs and forward packets accordingly.

**AWS route table example:**

| Destination     | Target             |
|------------------|--------------------|
| `10.0.0.0/16`    | local              |
| `0.0.0.0/0`      | Internet Gateway   |

`0.0.0.0/0` means "any destination not otherwise matched" — the **default route**.

---

## 17. Firewall ⭐⭐⭐

A firewall controls network traffic — allow or deny.

**Example rule set:**

| Port | Rule  |
|------|-------|
| 22   | Allow |
| 80   | Allow |
| 3306 | Deny  |

**In AWS:** you'll work with **Security Groups** and **Network ACLs** — Security Groups are especially important for EC2.

---

## 18. SSH ⭐⭐⭐

**SSH = Secure Shell** — remote, secure access to a server.

```bash
ssh -i my-key.pem ec2-user@PUBLIC-IP
```

Default port: **22**. Essential for DevOps work with Linux servers.

---

## 19. Load Balancing ⭐⭐⭐

Distributes traffic across multiple servers for **high availability, scalability, fault tolerance**.

**Without a load balancer:** one server crash = app down.

**With a load balancer:**

```
                 ┌──> Server 1
Users → Load Balancer ──> Server 2
                 └──> Server 3
```

**AWS example:** Application Load Balancer (ALB) for HTTP/HTTPS traffic.

---

## 20. Proxy vs Reverse Proxy ⭐⭐⭐

### Forward Proxy
```
Client → Proxy → Internet
```
Acts on behalf of the **client**; the destination server sees the proxy, not the client directly.

### Reverse Proxy
```
Client → Reverse Proxy → Server 1 / Server 2
```
Acts on behalf of the **server**; the user never talks to the backend directly. Examples: NGINX, HAProxy, AWS ALB.

**Memory trick:** Forward proxy = represents the client | Reverse proxy = represents the server

---

## 21. TLS / SSL ⭐⭐⭐

**TLS** is the modern protocol; **SSL** is its older predecessor. TLS provides:

- **Encryption** — traffic can't be easily read by others
- **Authentication** — browser verifies server identity via certificates
- **Integrity** — data isn't silently modified in transit

**Formula:** `HTTP + TLS = HTTPS`

---

## 22. Common Ports ⭐⭐⭐

| Port  | Protocol/Service | Purpose                |
|-------|-------------------|--------------------------|
| 22    | SSH                | Remote Linux access      |
| 53    | DNS                | Domain name resolution   |
| 80    | HTTP               | Web traffic              |
| 443   | HTTPS              | Secure web traffic       |
| 3306  | MySQL              | MySQL database           |
| 5432  | PostgreSQL         | PostgreSQL database      |
| 8080  | HTTP alternate     | Common app/server port   |

**Example Security Group (inbound rules):**

| Port | Service | Source     |
|------|---------|------------|
| 22   | SSH     | Your IP    |
| 80   | HTTP    | 0.0.0.0/0  |
| 443  | HTTPS   | 0.0.0.0/0  |
| 8080 | Custom  | Restricted |

---

## 🔥 Putting Everything Together

Deploying a website on AWS:

```
                    INTERNET
                        │
                    Public IP
                        │
                 Load Balancer
                        │
                 Port 443 (HTTPS)
                        │
                   EC2 Servers
                /               \
        Private IP           Private IP
          EC2-1                 EC2-2
               \               /
                \             /
                   Database
```

**What happens when a user opens `https://mywebsite.com`:**

| Step | Action                                                        |
|------|----------------------------------------------------------------|
| 1    | **DNS** — browser resolves `mywebsite.com` to an IP address    |
| 2    | **HTTPS** — browser connects over port 443                     |
| 3    | **Load Balancer** — receives the request                       |
| 4    | **Routing** — route table determines the correct subnet        |
| 5    | **Firewall** — Security Group checks if traffic is allowed     |
| 6    | **EC2** — request reaches the application server                |
| 7    | **Database** — app communicates with DB (e.g., TCP port 3306)  |

---

## 🧠 The Most Important Mental Model

```
DOMAIN → DNS → IP ADDRESS → ROUTING → SUBNET → FIREWALL → PORT → PROTOCOL → APPLICATION
```

**Example:**

```
google.com → DNS → 142.x.x.x → Internet routing → Server network
           → Port 443 → TCP + TLS → HTTPS → Web application
```

---

## ⭐ What You Should Master for Cloud + DevOps

### 🔴 Must Master
1. IP addresses
2. Public vs private IP
3. IPv4
4. CIDR
5. Subnetting
6. Ports
7. TCP vs UDP
8. DNS
9. Routing
10. Firewalls
11. HTTP/HTTPS
12. SSH
13. NAT
14. Load balancing

### 🟡 Understand Well
15. OSI model
16. TCP/IP model
17. MAC address
18. DHCP
19. Proxy/reverse proxy
20. TLS/SSL

### 🔥 AWS Connection

Once these are solid, AWS networking becomes much easier to learn:

```
Networking → VPC → CIDR → Subnets → Route Tables → Internet Gateway
          → NAT Gateway → Security Groups → Network ACL → Load Balancer → EC2
```

> This networking foundation should be finished before going deep into **AWS VPC, Docker networking, Kubernetes networking, and CI/CD deployment architecture.**

---

*📌 Notes compiled for personal study — Cloud/DevOps networking fundamentals.*