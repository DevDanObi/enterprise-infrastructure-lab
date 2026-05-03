# Enterprise Infrastructure Lab

## Overview

This repository documents the design and implementation of a multi-site enterprise network consisting of a Head Office, a Co-location Data Centre, and a dedicated Out-of-Band management and monitoring network.
![alt text](<Screenshot 2026-05-03 024804-1.png>)

The environment is built to reflect real-world enterprise requirements:
- Segmentation and controlled access
- Secure site-to-site connectivity
- Centralized monitoring and logging
- High availability at the distribution layer
- Controlled service exposure across sites

All implementations align with core CCNA domains including Network Fundamentals, Network Access, IP Connectivity, IP Services, Security, and Automation concepts.

---

## Architecture Summary

The Head Office follows a three-tier hierarchical design:
- Access Layer handles endpoint connectivity and VLAN segmentation
- Distribution Layer enforces routing and policy control
- Core Layer provides high-speed forwarding toward the edge

The Co-location site uses a collapsed core model optimized for service hosting.

Connectivity between sites is achieved using GRE over IPsec across an ISP network.

---

## Addressing Overview

| Segment       | Network            |
|--------------|------------------|
| VLAN 10 (R&D) | 192.168.10.0/24 |
| VLAN 20       | 192.168.20.0/24 |
| VLAN 30       | 192.168.30.0/24 |
| Management    | 10.10.10.8/30   |
| WAN Links     | /30 point-to-point |
| Data Centre   | 10.1.1.0/24     |

---

## Key Design Principles

- VLAN-based segmentation for isolation
- Extended ACL enforcing business policy (R&D-only access to Co-lo over HTTPS)
- Standard ACL protecting management plane
- GRE over IPsec for secure site-to-site communication
- Centralized monitoring using SNMP and Syslog
- Redundant Layer 2 and Layer 3 paths for availability

---

## Repository Structure

### 00-introduction
- Overview
- Design Objectives
- Topology and Architecture

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
- Security Concepts
- Standard ACL (Management Plane)
- Extended ACL (Data Plane)
- Layer 2 Security (Port Security, DHCP Snooping, DAI)
- VPN (IPsec Concepts)
- Wireless Security

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

## Security Implementation

Security is enforced through layered controls:

- Standard ACL restricts access to management systems only
- Extended ACL ensures only VLAN 10 (R&D) can access Co-location services over HTTPS
- Layer 2 protections include port security and DHCP snooping
- IPsec ensures encrypted communication across WAN

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