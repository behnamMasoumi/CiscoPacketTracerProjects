# Server-Based DHCP, DNS & Web Services

## Overview

A small VLAN-based network demonstrating a centralized server providing **DHCP, DNS, and HTTP services**, with a Layer 3 switch handling inter-VLAN routing and DHCP relay.

### Topology

- 1 Layer 3 Switch
- 1 Layer 2 Access Switch
- 1 Multi-Service Server
- Client PCs across multiple VLANs

### VLANs

| VLAN | Name | Network |
| --- | --- | --- |
| 10 | HR | 192.168.10.0/24 |
| 20 | SALES | 192.168.20.0/24 |
| 30 | IT | 192.168.30.0/24 |
| 40 | SERVER | 192.168.40.0/24 |
| 99 | MANAGEMENT | 192.168.99.0/24 |
| 999 | NATIVE | — |

### Key Configuration

- Static IP assigned to the server
- DHCP scopes configured on the server
- DNS service configured on the server
- Custom HTTP page configured and tested
- SVIs configured for inter-VLAN routing
- DHCP relay configured on the Layer 3 switch
- IP routing enabled on the Layer 3 switch
- 802.1Q trunking between switches

### Verification

Clients successfully received IP addresses from the server, communicated across VLANs, resolved DNS names, and accessed the hosted web page.

Additional configuration screenshots are available in the `images` folder.

**Focus:** DHCP • DNS • HTTP • DHCP Relay • Inter-VLAN Routing • Layer 3 Switching