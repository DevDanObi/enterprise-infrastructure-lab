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

## ACL Design and Implementation

### Scenario Overview

You are tasked with implementing access control policies in a multi-site enterprise network consisting of a Head Office, a Co-location Data Centre, and a Network Management and Logging block.

Connectivity between the Head Office and the Co-location Data Centre is established through a GRE over IPsec tunnel across an ISP network. Routing between all segments is already functional.

The Head Office contains multiple VLANs, including a dedicated Research and Development segment (VLAN 10). The Co-location Data Centre hosts critical services in VLAN 100, including an HTTPS server.

The Network Management and Logging block provides centralized services such as monitoring, logging, directory services, and automation.

Your task is to design and implement ACL policies that enforce strict access control while maintaining operational flexibility.

---

### Task 1 – Head Office to Co-Lo Data Centre Access Control

The Research and Development segment (VLAN 10 – 192.168.10.0/24) in the Head Office requires access to an HTTPS service hosted in the Co-Lo Data Centre (10.1.1.30). No other VLANs within the Head Office are permitted to access services in the Co-Lo Data Centre. This means that only VLAN 10 is allowed to initiate HTTPS connections to the Co-Lo HTTPS server. All other traffic from any Head Office VLAN to the Co-Lo network must be denied. Traffic should be filtered before traversing the WAN tunnel  

#### Design Considerations

The policy requires matching source, destination, and protocol. Traffic should be controlled as close to its origin as possible. Unnecessary traffic should not be allowed into the GRE/IPsec tunnel. 

#### Implementation Questions

1. What type of ACL should be used to enforce this requirement  
2. On which device in the Head Office should this ACL be configured  
3. On which interface and in which direction should the ACL be applied  

---

### Task 2 – Network Management and Logging Block to Co-Lo Data Centre Access Control

The Co-Lo Data Centre does not have internet access. It communicates only with:

- The Research and Development segment (VLAN 10) for application traffic  
- The Network Management and Logging block (172.16.1.0/24) for administrative and operational functions  

The Network Management and Logging block supports multiple services including SNMP, SSH, Telnet, Syslog, NTP, HTTP, and HTTPS. Additional services may be introduced in the future.

#### Requirements

- Only the Network Management and Logging subnet is permitted to access Co-Lo infrastructure devices  
- All required management and logging protocols must be allowed  
- The solution must accommodate future protocol additions without frequent modification  
- Unauthorized sources must be prevented from accessing Co-Lo devices  

#### Design Considerations

- The control requirement is based on trusted source networks rather than specific applications  
- Introducing protocol-specific filtering may increase operational complexity  
- The enforcement point should protect Co-Lo devices directly  

#### Implementation Questions

1. What type of ACL should be used for this requirement  
2. On which device should this ACL be configured  
3. On which interface or access point should this ACL be applied  

---

### Evaluation Criteria

Your solution will be evaluated based on:

- Correct selection of ACL type for each scenario  
- Proper placement of ACLs within the topology  
- Alignment with enterprise design practices  
- Efficiency and scalability of the solution  
- Ability to enforce policy without impacting legitimate communication  

---

### Expected Outcome

A correct implementation will result in:

- Successful HTTPS communication from VLAN 10 to the Co-Lo HTTPS server  
- Denial of access from other Head Office VLANs to Co-Lo services  
- Controlled access from the Network Management and Logging block to Co-Lo devices  
- Continued operation of all required management and logging services  

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