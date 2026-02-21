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

#### 4. Application Layer
- HTTP/HTTPS, DNS, SSH, FTP, SMTP
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


ping _dns_
nslookup
ifconfig
curl ifconfig.me
dig
