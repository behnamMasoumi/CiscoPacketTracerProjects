# Cisco Packet Tracer Networking Labs

A collection of networking labs I built in Cisco Packet Tracer to practice switching, routing, network services, access controls, and troubleshooting.

The projects range from focused fundamentals labs to a three-site enterprise network. Each project folder includes its own documentation, Packet Tracer file, and screenshots.

## Featured Project: Multi-Site Enterprise Network

A simulated enterprise network connecting a headquarters campus with branches in **Abbotsford and Kelowna**.

![Multi-Site Enterprise Network Topology](Enterprise-Projects/MultiSite-Enterprise-Network)

**Key features:**

- Departmental VLANs, voice VLANs, and management networks
- Redundant HQ switching with HSRP, LACP EtherChannel, and Rapid PVST+
- OSPF routing between headquarters and both branches
- Centralized DHCP, DNS, HTTP, NTP, Syslog, and TFTP services
- PAT for simulated internet access
- Guest network isolation and restricted SSH management
- Centralized IP telephony for HQ and Kelowna
- Distribution-switch failover testing and configuration backups

The documentation includes the addressing plan, verification screenshots, troubleshooting lessons, and failover results.

**[Explore the project →](Enterprise-Projects/MultiSite-Enterprise-Network)**

## Other Network Projects

| Project | Documentation |
| --- | --- |
| Law Firm Office | [View project](Enterprise-Projects/LawFirmOffice) |
| Gaming Centre Network | [View project](Enterprise-Projects/Gaming%20Centre%20Network) |
| Coffee Shop Network | [View project](Enterprise-Projects/CoffeeShopNetwork) |

## Networking Fundamentals

Focused labs for practicing individual technologies and understanding how they work together.

| Lab | Focus |
| --- | --- |
| [Guest Network ACLs](Network-Fundamentals/ACL%20%28Guest%20Wifi%29) | VLANs, router-on-a-stick, DHCP, and guest access restrictions |
| [DHCP Server Configuration](Network-Fundamentals/DHCP%20Server%20Configuration) | DHCP configuration and client connectivity |
| [Dynamic Routing with OSPF](Network-Fundamentals/Dynamic%20Routing%20using%20OSPF) | OSPF, Layer 3 switching, SVIs, and DHCP relay |
| [DNS, DHCP, and Web Services](Network-Fundamentals/Server%20handling%20DNS%2C%20DHCP%2C%20Web%20Server) | Centralized services and name resolution |
| [IP Telephony](Network-Fundamentals/Telephony%20Service%20%28TFTP%29) | Voice VLANs, DHCP, TFTP, and phone registration |

## Opening the Labs

1. Open a project folder and read its README.
2. Download the `.pkt` file.
3. Open it in Cisco Packet Tracer.
4. Use the documented topology and verification steps to explore the network.

## Lab Environment

These projects use simulated devices and services in Cisco Packet Tracer. References to internet connectivity describe a simulated ISP and external network.
