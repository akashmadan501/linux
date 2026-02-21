## COMPUTER NETWORK 
A computer network is a group of computers and devices connected so they can share data, resources, and services. This could be two laptops sharing files or millions of servers powering the internet.

```
Ping = latency (ms)
Gaming, SSH, APIs → care about low ping

Speed test (Mbps) = bandwidth
Downloads, uploads, backups → care about high bandwidth
```
## OSI Model(Theory)
The OSI (Open Systems Interconnection) model is a conceptual framework that standardizes how computer systems communicate over a network.  Developed by the International Organization for Standardization (ISO), it divides network communication into seven distinct layers, each with specific functions and responsibilities

- Application
- Presentation
- Session
- Transport
- Network
- Data link
- Physical

#### Layer 7: Application
- HTTP, HTTPS, REST, gRPC, DNS, SMTP
- Tools: curl, Postman, browser, APIs

#### Layer 6: Presentation
Format and Security
- JSON, XML, compression
- SSL/TLS encryption


#### Layer 5: Session 
Connection Management
- session creation, keep-alive, auth session
- Timeouts, reconnects


#### Layer 4: Transport
Ports and Reliability
- TCP & UDP 
- Port(80, 443, 22)


#### Layer 3: Network 
IP & Routing
- IP addresses, routing, subnets
- ICMP ping
- Firewall, subnets issues

#### Layer 2: Data LInk
MAC & switching
- MAC addresses
- ARP 
- Switches, VLANs

#### Layer 1: Physical
Hardware reality
- Cables, NICs, fiber, Wifi
- Link up/down

**Ping works ≠ app works**     

**App works locally ≠ network allows it**

**Most outages are Transport or Application layer**

**Debug always bottom → top**

---

## TCP/IP Model (production)
TCP - Transmission Control Protocol
IP - Internet protocol

#### 4. Application Layer

- HTTP/HTTPS, DNS, SSH, FTP, SMTP
- Encryptions, sessions
- APIs, web apps, microservices
- If curl, browser, or API fails but network is fine, problem lives here.

#### 3. Transport Layer
- Ports & reliability
- TCP (reliable, ordered)
- UDP (fast, no guarantee)
- Port numbers (22, 80, 443)
- Service running but not reachable = port / protocol issue.

#### 2. Internet Layer
- IP & routing
- IP addressing
- Routing
- ICMP (ping)
- Ping fails = routing, firewall, subnet, or security group issue.

####  1. Network Access Layer
- Hardware + local network
- Ethernet, Wi-Fi
- MAC address
- ARP
- Interface down or cable issue = stop debugging software.


**OSI vs TCP/IP (quick mapping)**

**OSI 7 layers → TCP/IP 4 layers**

**OSI (L7–L5) → TCP/IP Application**

**OSI L4 → TCP/IP Transport**

**OSI L3 → TCP/IP Internet**

**OSI (L2–L1) → TCP/IP Network Access**


---


## OSI vs TCP/IP Model
| OSI Layer | TCP/IP Layer        | Layer Name        | What It Handles                         | DevOps Focus / Examples                     | Common Issues                              |
|----------:|---------------------|-------------------|------------------------------------------|---------------------------------------------|--------------------------------------------|
| 7         | Application         | Application       | HTTP, HTTPS, DNS, APIs                   | curl, APIs, Nginx, App configs               | 4xx/5xx errors, wrong endpoints             |
| 6         | Application         | Presentation      | TLS/SSL, encryption, data format         | Certificates, JSON/XML, compression          | Cert expired, SSL handshake failure         |
| 5         | Application         | Session           | Sessions, auth, keep-alive               | Login sessions, timeouts                     | Random disconnects, session drops           |
| 4         | Transport           | Transport         | TCP/UDP, ports                           | Ports 22/80/443, retries                     | Port closed, connection refused             |
| 3         | Internet            | Network           | IP, routing, ICMP                        | ping, traceroute, routing tables             | Ping fails, subnet or SG issue               |
| 2         | Network Access      | Data Link         | MAC, ARP, switching                     | VLANs, ARP cache                             | IP works, no connectivity                   |
| 1         | Network Access      | Physical          | Cable, NIC, Wi-Fi                        | Interface up/down                            | Link down, hardware failure                 |



---


## DNS (Domain Name System)
- DNS is contact list of the internet.

- DNS converts a domain name into an IP address so your system knows where to connect.

**What actually happens when you open a website**
```
You type:
google.com
```



1. Browser asks OS: “Do I already know this IP?”

2. OS checks cache (fast, local)

3. If not found → DNS resolver (ISP / router / public DNS)

4. Resolver asks:

- Root server → “Where is .com?”

- TLD server → “Where is google.com?”

- Authoritative server → “Here is the IP”

5. IP comes back:
```
google.com → 142.x.x.x
```          
6. Browser connects to that IP using TCP/HTTPS


---

## Commands

ping _dns_
nslookup
ifconfig
curl ifconfig.me
dig
hostname -i   private ip

---

## IP, Ports, Subnets, VPCs(AWS)


## 1. What is an IP Address?

An **IP address** uniquely identifies a device in a network.

- IP = Machine identity
- Port = Application identity on that machine

---

## 2. Public IP vs Private IP

### Public IP
- Globally unique
- Reachable from the internet
- Assigned by ISP or cloud provider
- Limited and costly

**Usage**
- Load Balancers
- Bastion Hosts
- NAT Gateways
- Internet-facing servers

---

### Private IP
- Used inside private networks
- Not reachable directly from the internet
- Reusable across different networks

**Private IP ranges**

    10.0.0.0/8
    172.16.0.0/12
    192.168.0.0/16


**Usage**
- EC2 internal communication
- App → DB traffic
- Kubernetes pods
- Corporate networks

---

## 3. Why Private IPs Can Be Same

Private IPs only need to be **unique within their own network**.

Example:

VPC-1: 192.168.1.10
VPC-2: 192.168.1.10


No conflict because networks are isolated.

Public IPs **must always be unique**.

---

## 4. What is a Subnet?

A **subnet** is a smaller network carved out of a larger network using CIDR.

Subnetting helps with:
- Security isolation
- Traffic control
- Scalability
- High availability

---

## 5. Subnets in AWS VPC

- VPC is created with a CIDR block
- Subnets are created inside the VPC
- Each subnet belongs to **one Availability Zone**

Example:

    VPC: 10.0.0.0/16

Subnets:
    10.0.1.0/24 → Public Subnet (AZ-A)
    10.0.2.0/24 → Private Subnet (AZ-A)
    10.0.3.0/24 → Public Subnet (AZ-B)
    10.0.4.0/24 → Private Subnet (AZ-B)


---

## 6. Public vs Private Subnet (AWS)

A subnet is **not public or private by default**.

### Public Subnet
- Route table has a route to Internet Gateway (IGW)
- Used for:
  - Load Balancer
  - Bastion Host
  - NAT Gateway

### Private Subnet
- No direct route to IGW
- Used for:
  - Application servers
  - Databases
  - Internal services

  **Route table decides subnet behavior**

---

## 7. Reserved IPs in AWS Subnet (Important)

AWS reserves **5 IPs per subnet**:

| IP | Purpose |
|----|--------|
| .0 | Network address |
| .1 | VPC router |
| .2 | DNS |
| .3 | Reserved |
| Last IP | Reserved |

Example:

/24 = 256 total IPs
Usable = 256 - 5 = 251


---

## 8. What is a Port?

A **port** identifies a service running on a machine.

| Port | Service |
|----|-------|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |
| 3306 | MySQL |

- IP tells **where**
- Port tells **what**

---

## 9. Why Ports Are Needed

One IP can run many applications.

Without ports:
- One IP = One service

With ports:
- One IP = 65,535 services/connections

---

## 10. NAT (Network Address Translation)

NAT allows:
- Many private IPs
- To access the internet
- Using one public IP

---

### NAT Flow (AWS Example)


EC2 (10.0.1.10:54001)
↓
NAT Gateway (54.12.34.56:60001)
↓
Internet (8.8.8.8:53)


NAT keeps a mapping table:

10.0.1.10:54001 → 54.12.34.56:60001


Ports make this possible.

---

## 11. Why NAT Is Used in Private Subnets

- Private instances stay hidden
- Outbound internet access allowed
- No inbound access from internet

Common use:
- OS updates
- Package downloads
- API calls

---

## One-Liners

- **Subnet**: Logical network division using CIDR
- **Private IP**: Internal identity inside a network
- **Public IP**: Internet-facing identity
- **Port**: Application-level identifier
- **NAT**: Allows private networks to access internet securely
- **Two EC2s can share same Public IP using Load Balancer or NAT(outbound only)**
- **SSH use port 22, becoz convention + Standaradization, not a techical limit.**
---


#### Pending Task
**Update notes after learning AWS VPC**