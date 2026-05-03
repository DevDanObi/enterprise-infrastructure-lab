# Access Control Lists in an Enterprise Network

## Introduction

Once inter-VLAN routing is enabled, all segments in the network can communicate. While this confirms routing is functioning, it also creates a flat trust model where every network has unrestricted access to others.

In a structured environment, connectivity alone is not sufficient. Traffic must be controlled based on purpose and sensitivity. Access Control Lists are introduced to enforce that control.

## Enterprise Context

The head office network is divided into three VLANs:

- VLAN 10 represents the R&D segment using 192.168.10.0/24  
- VLAN 20 represents general users using 192.168.20.0/24  
- VLAN 30 represents operations using 192.168.30.0/24  

Routing between these VLANs is handled at the core router.

A co-location data centre is connected over a secured tunnel. It hosts application services within the 10.1.1.0/24 network.

A separate management network exists on 172.16.1.0/24. This network is used for monitoring, logging, and administrative control.

## Design Requirement

The network requires controlled access between segments.

- R&D should be able to communicate with the co-location data centre, but only over HTTPS  
- Other VLANs should not be able to access the data centre  
- Management traffic should be restricted in a simple and controlled way  

This introduces the need for selective filtering rather than full connectivity.

## ACL Approach

Two ACL types are used based on the level of control required.

Standard ACLs are used within the management network. The focus here is to control which systems are allowed to initiate management traffic. Since only the source needs to be validated, a simple source-based filter is sufficient.

Extended ACLs are used within the data plane. In this case, both the source and destination must be considered, along with the application being used. This allows precise control over how traffic flows between VLANs and toward the data centre.

## ACL Selection

The management network uses a standard ACL because control is based only on trusted source systems. There is no need to inspect destination or protocol.

The head office network uses an extended ACL because the requirement involves restricting specific VLANs from reaching a particular destination, while also limiting access to a specific application. This level of control cannot be achieved with a standard ACL.

## Traffic Policy

- R&D is allowed to access the data centre over HTTPS only  
- VLAN 20 and VLAN 30 are not allowed to access the data centre  
- All other internal communication remains permitted  

This ensures that sensitive resources are protected while maintaining normal network operation.

## Configuration

### Extended ACL

