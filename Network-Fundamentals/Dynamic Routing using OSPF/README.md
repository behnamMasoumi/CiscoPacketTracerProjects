# OSPF Dynamic Routing — Cisco Packet Tracer

## Overview

A small multi-department network built to demonstrate **OSPF dynamic routing**, VLAN segmentation, Layer 3 switching, and centralized DHCP.

The network contains two departments, each with two VLANs. Access switches connect end devices to Layer 3 switches, which provide inter-VLAN routing. A central router connects both departments and participates in the OSPF routing domain.

## Network Design

| VLAN | Name | Network         |
| ---- | ---- | --------------- |
| 10   | A    | 192.168.10.0/24 |
| 20   | B    | 192.168.20.0/24 |
| 30   | C    | 192.168.30.0/24 |
| 40   | D    | 192.168.40.0/24 |

Each VLAN contains two PCs.

**Routed links:**

* Department A L3 Switch ↔ Router: `10.0.0.2 ↔ 10.0.0.1`
* Department B L3 Switch ↔ Router: `10.0.0.6 ↔ 10.0.0.5`

## OSPF

OSPF is configured on both Layer 3 switches and the central router to dynamically exchange routes between departments.

`passive-interface` is configured on the VLAN interfaces. This allows the VLAN networks to be advertised through OSPF without attempting to form OSPF neighbor relationships with end-user devices. OSPF adjacencies are established only across the routed links between the Layer 3 switches and router.

## DHCP

DHCP is centralized on the router, with **DHCP relay** configured on both Layer 3 switches so clients across all VLANs can obtain addresses from the centralized DHCP service.

## Validation

The network was tested for:

* DHCP address assignment
* OSPF neighbor formation
* OSPF-learned routes
* Inter-VLAN connectivity
* End-to-end connectivity between Department A and Department B

## Documentation

The `images` folder contains **12 screenshots** documenting the network topology, configuration, OSPF operation, and connectivity testing.
