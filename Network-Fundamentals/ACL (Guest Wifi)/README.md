# Guest Network ACL Configuration

## Overview

A small business LAN demonstrating **ACL-based guest network isolation** using VLANs, Router-on-a-Stick, and DHCP.

### Topology

- 1 Router
- 1 Layer 2 Switch
- 4 Office/Front Desk PCs
- 1 Wireless Access Point
- 3 Guest Laptops
- ISP/Internet connection

### VLANs

| VLAN | Name | Purpose |
| --- | --- | --- |
| 10 | OFFICE | Office PCs |
| 20 | FRONTDESK | Front desk PCs |
| 30 | GUEST | Guest Wi-Fi |
| 99 | MANAGEMENT | Network management |
| 999 | NATIVE | Native VLAN |

### Key Configuration

- Access ports and 802.1Q trunking
- Router-on-a-Stick / Inter-VLAN routing
- DHCP configured on the router
- Extended ACL for guest network restrictions
- Guest traffic restricted from internal VLANs
- Internet access permitted for guests

### Verification

DHCP successfully assigned addresses to all devices. Internal devices maintained connectivity, while guest devices could communicate with each other and access the Internet but were **blocked from the Office, Front Desk, and Management networks**.

**Focus:** ACLs • VLANs • Guest Network Isolation • DHCP • Router-on-a-Stick