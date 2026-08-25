# DHCP Configuration — Router-on-a-Stick

## Overview

A small VLAN-based LAN demonstrating **DHCP configuration on a Cisco router** using **Router-on-a-Stick** for inter-VLAN routing.

### Topology

- 1 Router
- 1 Layer 2 Switch
- 3 PCs
- 1 Server
- 1 Access Point + Laptop
- 5 VLANs

### VLANs

| VLAN | Name | Network |
| --- | --- | --- |
| 10 | Ten | 192.168.10.0/24 |
| 20 | Twenty | 192.168.20.0/24 |
| 30 | Thirty | 192.168.30.0/24 |
| 40 | Vlan Forty | 192.168.40.0/24 |
| 50 | Fifty | 192.168.50.0/24 |

The router provides DHCP services for all five VLANs. Addresses `.1–.20` are excluded from each subnet to reserve them for static assignments.

### Key Configuration

- Router-on-a-Stick / 802.1Q
- DHCP pools for each VLAN
- DHCP excluded addresses
- Inter-VLAN routing
- DHCP client verification

### Verification

Clients successfully received IP addresses from the correct DHCP pool and were able to communicate across VLANs through the router.

Additional configuration screenshots are available in the `images` folder.

**Focus:** DHCP • VLANs • Router-on-a-Stick • Inter-VLAN Routing
