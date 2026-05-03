# Enterprise Infrastructure Lab

## Overview

This repository documents the design and implementation of a multi-site enterprise network spanning a Head Office, a Co-location Data Centre, and a dedicated out-of-band management, logging, and monitoring environment. The design reflects real enterprise expectations around segmentation, security, availability, and operational visibility, while aligning with core CCNA domains.
![alt text](<Screenshot 2026-05-03 024804-2.png>)

---

## Architecture Summary

The Head Office follows a three-tier hierarchical model:

- **Access Layer** provides endpoint connectivity and VLAN-based segmentation  
- **Distribution Layer** performs inter-VLAN routing and enforces policy control  
- **Core Layer** delivers high-speed, resilient forwarding toward external networks  

The Co-location Data Centre uses a **collapsed core design**, optimized for service hosting and reduced operational overhead.

Inter-site connectivity is implemented using **GRE over IPsec across an ISP**, providing secure transport between sites.

---

## Addressing Overview

| Segment            | Network              |
|-------------------|---------------------|
| VLAN 10 (R&D)     | 192.168.10.0/24     |
| VLAN 20           | 192.168.20.0/24     |
| VLAN 30           | 192.168.30.0/24     |
| Management        | 172.16.1.0/24       |
| Data Centre       | 10.1.1.0/24         |
| WAN Links         | Public /30 (P2P)    |

---

## Design Principles

- VLAN-based segmentation is used to isolate traffic across business units  
- Access control is policy-driven  
  - Extended ACLs regulate data plane traffic, for example restricting R&D to HTTPS-only access to Data Centre services  
  - Standard ACLs are used to protect the management plane  
- GRE over IPsec is used to secure inter-site communication  
- Monitoring and logging are centralized using SNMP and Syslog  
- High availability is achieved through redundant Layer 2 and Layer 3 design at the distribution layer  
- Service exposure between sites is tightly controlled to reflect enterprise security requirements  


---

## 


### 01-network-fundamentals
- Network Components
- Topology Design (3-tier, collapsed core)
- IP Addressing (IPv4, Subnetting)
- Switching Fundamentals
- Wireless and Virtualization Concepts

### 02-network-access
- VLAN Design and Segmentation
- Trunking and Inter-switch Connectivity
- Spanning Tree (Loop Prevention)
- EtherChannel
- Layer 2 Discovery (CDP/LLDP)

### 03-ip-connectivity
- Routing Concepts
- Static Routing
- OSPF (Single Area)
- Routing Decision Process
- First Hop Redundancy (HSRP Concepts)

### 04-ip-services
- NAT
- DHCP and DNS
- NTP
- SNMP
- Syslog
- SSH and Remote Access
- QoS Fundamentals
- File Transfer (TFTP/FTP)

### 05-security
## Security Implementation

The security design follows a layered approach where controls are applied based on function, traffic type, and enforcement point within the network. The objective is to separate management access from user data traffic while enforcing strict policy between the Head Office and the Co-location Data Centre.

### Security Concepts

The implementation covers core areas of network security:

- Management plane protection using access control  
- Data plane restriction using policy-based filtering  
- Layer 2 attack prevention mechanisms  
- Encrypted communication across untrusted networks  
- Secure access considerations for wireless environments  

Each control is applied with a clear purpose and placement within the topology.

---

### Standard ACL (Management Plane)

Standard ACLs are used to regulate access between the Network Management segment and the Co-location environment. The requirement is straightforward. Only known and trusted management systems such as monitoring servers, automation hosts, and administrative endpoints should be allowed to interact with network devices.

The management network supports multiple services across different protocols and ports, including NTP, SSH, Telnet, SNMP, and Syslog. The goal is not to filter based on application type, but to strictly control which sources are permitted to initiate any form of management communication.

Introducing an Extended ACL in this context would add unnecessary complexity. Each service would require explicit protocol and port definitions, increasing the likelihood of configuration errors, service interruptions, or unintended blocking of legitimate management traffic.

A Standard ACL addresses this cleanly by filtering only on the source IP address. Once a trusted host is permitted, all required management services can operate without additional policy overhead.

This control is applied close to the destination, typically on VTY lines or management interfaces. This ensures that only authorized systems can reach the management plane, while avoiding unnecessary filtering within the internal network.

**Design reasoning:**
- Control is based on trusted source systems rather than individual services  
- Reduces configuration complexity and operational risk  
- Maintains availability of all management protocols without granular filtering  
- Establishes a clear and consistent enforcement point for administrative access

### Extended ACL (Data Plane)

Extended ACLs are used to enforce business policy between VLANs and across sites. In this design, only the R and D network in VLAN 10 is permitted to access services in the Co-location Data Centre, and only over HTTPS.

An Extended ACL is required because the policy depends on:
- Source network  
- Destination network  
- Protocol and port  

The requirement is precise. R and D must reach the Co-lo services securely over HTTPS, while all other traffic must be denied. This cannot be achieved with a Standard ACL.

The ACL is applied close to the **source**, at the Head Office edge interface facing the WAN. This ensures unwanted traffic is dropped before entering the WAN, reducing unnecessary load and exposure.

**Design reasoning:**
- Business-driven segmentation  
- Protocol-specific control using TCP port 443  
- Early enforcement to protect WAN and remote site  

---

### Layer 2 Security

Layer 2 protections are implemented at the access layer to prevent common local network attacks.

Controls include:
- Port security to limit MAC address learning  
- DHCP snooping to prevent rogue DHCP servers  
- Dynamic ARP inspection to mitigate ARP spoofing  

These controls ensure that endpoints cannot manipulate Layer 2 behavior to gain unauthorized access or disrupt the network.

---

### VPN (IPsec Concepts)

Traffic between the Head Office and Co-location Data Centre traverses an untrusted ISP network. To secure this communication, IPsec is used in combination with GRE.

IPsec provides:
- Encryption to protect data confidentiality  
- Integrity to prevent tampering  
- Authentication to verify peer identity  

GRE allows routing flexibility, while IPsec secures the transport.

---

### Wireless Security

No Wireless implemented here.

---

Each control is placed deliberately based on what needs to be protected and where enforcement is most effective.

### 06-wan-and-edge
- WAN Design
- ISP Connectivity
- GRE Tunnel
- IPsec Overlay
- Edge Routing

### 07-automation-and-programmability
- Automation Concepts
- Infrastructure as Code
- Ansible Integration
- REST APIs and JSON

### 08-observability-and-monitoring
- Monitoring Architecture (Zabbix)
- Syslog and Logging Strategy
- SNMP Monitoring
- Telemetry and Visibility
- Troubleshooting Approach

### assets
- Topology Diagrams
- Screenshots
- Packet Captures

---




## Services and Operations

- NAT enables external connectivity
- DHCP and DNS support endpoint operations
- NTP ensures time consistency across devices
- SNMP and Syslog provide monitoring and logging visibility
- SSH is used for secure administrative access

---

## Observability

The Out-of-Band network hosts:
- Zabbix monitoring platform
- Automation node
- Domain controller

All network devices export:
- Syslog for centralized logging
- SNMP metrics for monitoring

This provides full operational visibility across the environment.

---

## Design Outcome

This implementation demonstrates:
- Structured enterprise network design
- Policy-driven segmentation
- Secure WAN connectivity
- Integrated monitoring and logging
- Alignment with real-world operational practices

---

## Closing Statement

This project represents a practical application of CCNA concepts within an enterprise context, focusing on design decisions, operational visibility, and security enforcement rather than isolated configuration tasks.