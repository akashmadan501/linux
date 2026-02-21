## COMPUTER NETWORK 
A computer network is a group of computers and devices connected so they can share data, resources, and services. This could be two laptops sharing files or millions of servers powering the internet.

```
Ping = latency (ms)
Gaming, SSH, APIs → care about low ping

Speed test (Mbps) = bandwidth
Downloads, uploads, backups → care about high bandwidth
```
## OSI Model
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


