# Enterprise Infrastructure Lab

## Overview

This repository documents the design and implementation of a multi-site enterprise network spanning a Head Office, a Co-location Data Centre, and a dedicated out-of-band management, logging, and monitoring environment. The design reflects real enterprise expectations around segmentation, security, availability, and operational visibility, while aligning with core CCNA domains.

![alt text](image.png)

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

## Table of Contents (CCNA Framework)

### 1. Network Fundamentals
- Network Components
- Topology Architectures
- Physical Interfaces and Cabling
- TCP vs UDP
- IPv4 Addressing and Subnetting
- IPv6 Addressing
- Wireless Fundamentals
- Virtualization Basics
- Switching Concepts

### 2. Network Access
- VLAN Configuration
- Inter-Switch Connectivity (Trunking, 802.1Q)
- Layer 2 Discovery Protocols (CDP, LLDP)
- EtherChannel
- Spanning Tree Protocol
- Wireless Architecture and Access

### 3. IP Connectivity
- Routing Table Components
- Routing Decision Process
- Static Routing
- OSPF (Single Area)
- First Hop Redundancy (HSRP)

### 4. IP Services
- NAT
- DHCP
- DNS
- NTP
- SNMP
- Syslog
- QoS Concepts
- SSH and Remote Access
- File Transfer (TFTP/FTP)

### 5. Security Fundamentals
- Security Concepts (Threats, Vulnerabilities)
- Device Access Control
- Password Policies and MFA
- VPN Technologies (IPsec)
- Access Control Lists (ACLs)
- Layer 2 Security Features
- AAA (Authentication, Authorization, Accounting)
- Wireless Security

### 6. Automation and Programmability
- Network Automation Concepts
- Controller-Based Networking
- Software-Defined Architecture
- APIs and REST Concepts
- Configuration Management Tools (Ansible, Puppet, Chef)
- JSON Data Structures

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