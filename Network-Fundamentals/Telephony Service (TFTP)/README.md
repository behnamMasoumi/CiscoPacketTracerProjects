# Telephony service Configuration

## Overview

Built a Cisco Packet Tracer network demonstrating **VLAN segmentation, Router-on-a-Stick, DHCP, and Cisco VoIP/CME configuration**.

The network separates user departments into dedicated VLANs while placing all VoIP phones in a separate **Voice VLAN**.

## Network Design

| VLAN | Department | Gateway |
| --- | --- | --- |
| **VLAN 10** | HR | `192.168.10.1` |
| **VLAN 20** | Sales | `192.168.20.1` |
| **VLAN 30** | Finance | `192.168.30.1` |
| **VLAN 40** | Voice | `192.168.40.1` |
- **ACCESS-SW** — Access Layer Switch
- **EDGE** — Edge Router
- 1 PC and 1 VoIP phone per department
- All VoIP phones are isolated in **VLAN 40**

## Configuration

- Configured **VLAN 10, 20, 30, and 40** on the access switch.
- Configured the PC ports with the appropriate **data VLAN** and the phone connections with **Voice VLAN 40**.
- Configured the **ACCESS-SW → EDGE** uplink as an **802.1Q trunk** carrying VLANs 10, 20, 30, and 40.
- Implemented **Router-on-a-Stick** on EDGE using subinterfaces for inter-VLAN routing.
- Configured **DHCP pools** on EDGE to dynamically assign IP addresses to each VLAN.
- Configured **DHCP Option 150** for the Voice VLAN, pointing to `192.168.40.1` as the **TFTP/CME server address** used by the IP phones.
- Enabled **Cisco CallManager Express (CME)** using `telephony-service` on EDGE to provide VoIP phone registration and extensions.

## Testing

Verified that:

- PCs received IP addresses from the correct DHCP scopes.
- Devices were placed into their appropriate VLANs.
- VoIP phones received addresses from the Voice VLAN.
- Phones successfully obtained their required TFTP/CME information through Option 150.
- Inter-VLAN routing operated through the EDGE router.
- VoIP phones successfully registered with the CME service.

📁 **Screenshots:** See the `images` folder for the topology, configurations, IP addressing, and verification results.