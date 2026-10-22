# Multi-Site Enterprise Network — Cisco Packet Tracer

A three-site enterprise networking lab connecting a headquarters campus, an Abbotsford branch, and a Kelowna branch. I built this project to practice network segmentation, redundant campus switching, dynamic routing, centralized services, access controls, and troubleshooting.

The lab includes a simulated ISP and external web server. Internet access in this project refers to that simulated external network, not the real internet.

## Final topology

![Final picture-project finished](screenshots/28-MultiSite-Enterprise-Final.png)

| Site | Design |
| --- | --- |
| HQ | Two distribution switches, four access switches, separate WAN and internet-edge routers, a centralized server, departmental endpoints, IP phones, and guest Wi-Fi |
| Abbotsford | ABB-EDGE and ABB-RTR in series, a wired access switch for four PCs and two printers, and a separate guest access switch and wireless access point |
| Kelowna | KEL-EDGE and KEL-RTR in series, one access switch, two staff PCs, and a supervisor PC connected through an IP phone |

HQ-WAN connects the branches to the campus and hosts Cisco Unified Communications Manager Express (CME). HQ-EDGE connects both distribution switches to the ISP and performs PAT. The branch EDGE/RTR pairs are successive routing hops, not redundant router pairs.

## Technologies implemented

- VLAN segmentation, 802.1Q trunks, and router-on-a-stick at the branches
- Layer 3 SVIs and HSRP at HQ
- LACP EtherChannel and Rapid PVST+
- Single-area OSPF with passive LAN interfaces and default-route advertisement
- Centralized DHCP with relay, DNS, HTTP, NTP, Syslog, and TFTP backups
- PAT for HQ and branch user networks
- SSH management restricted to the HQ IT subnet
- Extended ACLs for HQ and Abbotsford guest isolation
- Voice VLANs and centralized CME for HQ and Kelowna phones

## VLAN and addressing plan

### Headquarters

All HQ VLANs use /24 masks. The HSRP virtual gateway is `.1`; DSW1 uses `.2` and DSW2 uses `.3` in each subnet.

| VLAN | Purpose | Subnet | Virtual gateway |
| --- | --- | --- | --- |
| 10 | IT | 10.10.10.0/24 | 10.10.10.1 |
| 20 | HR | 10.10.20.0/24 | 10.10.20.1 |
| 30 | Finance | 10.10.30.0/24 | 10.10.30.1 |
| 40 | Staff | 10.10.40.0/24 | 10.10.40.1 |
| 50 | Voice | 10.10.50.0/24 | 10.10.50.1 |
| 60 | Guests | 10.10.60.0/24 | 10.10.60.1 |
| 70 | Servers | 10.10.70.0/24 | 10.10.70.1 |
| 99 | Management | 10.10.99.0/24 | 10.10.99.1 |

### Branches

| Site | VLAN | Purpose | Subnet | Gateway |
| --- | --- | --- | --- | --- |
| Abbotsford | 10 | Staff | 10.20.10.0/24 | 10.20.10.1 |
| Abbotsford | 60 | Guests | 10.20.60.0/24 | 10.20.60.1 |
| Abbotsford | 99 | Management | 10.20.99.0/24 | 10.20.99.1 |
| Kelowna | 10 | Staff | 10.30.10.0/24 | 10.30.10.1 |
| Kelowna | 20 | Supervisors | 10.30.20.0/24 | 10.30.20.1 |
| Kelowna | 50 | Voice | 10.30.50.0/24 | 10.30.50.1 |
| Kelowna | 99 | Management | 10.30.99.0/24 | 10.30.99.1 |

Abbotsford has no voice VLAN. Kelowna's phone-facing switch port carries data VLAN 20 for the supervisor PC and voice VLAN 50 for the phone. VLAN separation creates distinct broadcast domains; it does not by itself prevent routed communication between departments.

### Routed links and services

| Link | Endpoint addresses |
| --- | --- |
| DSW1 — HQ-WAN | 10.255.0.1/30 — 10.255.0.2/30 |
| DSW2 — HQ-WAN | 10.255.0.5/30 — 10.255.0.6/30 |
| DSW1 — HQ-EDGE | 10.255.0.9/30 — 10.255.0.10/30 |
| DSW2 — HQ-EDGE | 10.255.0.13/30 — 10.255.0.14/30 |
| HQ-WAN — ABB-EDGE | 10.255.1.1/30 — 10.255.1.2/30 |
| ABB-EDGE — ABB-RTR | 10.255.1.5/30 — 10.255.1.6/30 |
| HQ-WAN — KEL-EDGE | 10.255.2.1/30 — 10.255.2.2/30 |
| KEL-EDGE — KEL-RTR | 10.255.2.5/30 — 10.255.2.6/30 |
| HQ-EDGE — ISP | 203.0.113.2/30 — 203.0.113.1/30 |
| ISP — Internet-Web | 198.51.100.1/24 — 198.51.100.10/24 |

HQ-SERVER uses **10.10.70.10**. The CME loopback on HQ-WAN uses **10.255.255.1/32**.

## Campus switching and gateway redundancy

The HQ distribution switches are connected by LACP Port-channel 1, using Gi1/0/10 and Gi1/0/11 on both sides. Each HQ access switch has uplinks to both distribution switches.

Rapid PVST+ provides loop prevention. DSW1 is the primary STP root and preferred HSRP Active gateway; DSW2 provides the secondary path. HSRP priorities are 110 and 100, with preemption enabled. PortFast and BPDU Guard are configured on appropriate endpoint-facing ports.


![HQ Etherchannel summary verification](screenshots/02-etherchannel-verification.png)

![HQ DSW1 STP Root Verification](screenshots/05-spanning-tree-root.png)

![HQ HSRP verification](screenshots/07-HSRP-Verification.png)
## Routing and simulated internet access

OSPF area 0 carries internal routes between HQ and both branches. LAN interfaces are passive: their subnets are advertised without forming adjacencies with end devices.

HQ-EDGE has a static default route to the ISP and advertises it internally with `default-information originate`. The ISP does not participate in the company's OSPF domain. HQ-EDGE performs PAT using its outside address, **203.0.113.2**, for permitted HQ and branch user subnets.


![HQ-EDGE OSPF and IP Route OSPF](screenshots/18-HQ-EDGE-OSPF.png)

![HQ-EDGE Pat verification](screenshots/19-Internet-Access-and-PAT-Verification.png)

## Centralized services

One HQ server provides DHCP, DNS, HTTP, NTP, Syslog, and TFTP. DHCP relay on client-facing gateways forwards requests across routed networks. The internal DNS record **portal.bencompany.test** resolves to the server's HTTP service.


![HQ PC DHCP address, gateway, DNS verification](screenshots/08-dhcp-client-verification.png)

![Domainname resolution testing and verification](screenshots/09-internal-portal.png)

NTP synchronizes configured HQ network devices to the server. Syslog collects device events centrally. TFTP stores configuration backups after major milestones.


![NTP status](screenshots/12-ntp-verification.png)

![Syslog verification](screenshots/16-centralized-syslog.png)

## Management and guest access controls

HQ switches use management VLAN 99. SSH uses local authentication, and a VTY access-class permits management connections from **10.10.10.0/24**. This management restriction is separate from interface ACLs that filter routed user traffic.


![SSH Access Control](screenshots/15-SSH-access-control.png)

Inbound guest ACLs on both HQ distribution switches and ABB-RTR allow DHCP and DNS to HQ-SERVER, deny guest traffic to private internal networks, and allow remaining external destinations. The HQ ACL includes an HSRP v1 exception. These ACLs do not provide wireless client isolation within the same VLAN.


![Visitors Access Control](screenshots/20-Guest-Istolation.png)

![Abbotsford Guest Access Control](screenshots/23-ABB-Guest-isolation.png)

## Branch connectivity

Abbotsford's wired staff and management VLANs use router-on-a-stick. Its guest switch uses a separate physical interface on ABB-RTR. Kelowna uses a trunk carrying staff, supervisors, voice, and management VLANs to KEL-RTR.

Both sites receive client addresses from HQ and route external traffic through HQ-EDGE's PAT service.


![Abbotsford Staff DHCP and connectivity to HQ](screenshots/22-ABB-Staff-Connectivity.png)

![Kelowna Connectivity](screenshots/25-KEL-Connectivity.png)


## Centralized IP telephony

The HQ 2811 router hosts CME. Voice DHCP pools provide **10.255.255.1** as the TFTP server address, and OSPF advertises this loopback so the remote phone can reach it. HQ extensions are **1001–1006**; Kelowna uses **2001**.


![HQ Phones working](screenshots/11-HQ-voip-call.png)

![Kelowna to HQ Call](screenshots/26-Kelowna-HQ-Call.png)

## Failover validation

I powered off HQ-DSW1 and verified that DSW2 assumed the Active gateway role and that server and external connectivity worked through the surviving path. After powering DSW1 back on, I checked gateway roles and connectivity again.

This validates recovery from a distribution-switch outage. It does not demonstrate zero packet loss or uninterrupted voice sessions during convergence.


![HSRP Failover Test](screenshots/27-HQ-Failover-Test.png)

## Troubleshooting lessons

| Issue | Action and lesson |
| --- | --- |
| IP addressing rejected on a 2811 module port | Distinguished Layer 2 switch-module ports from routed interfaces; used routed ports or transit VLAN interfaces as appropriate. |
| HSRP role changes and failed LAN connectivity | Inspected switching state and gateway roles. Earlier link bouncing and later restarting Packet Tracer restored operation; the underlying cause was not conclusively established. |
| Telephony configuration-file timestamp warning | Set the router clock, regenerated the files, and later configured NTP. |
| NTP initially unsynchronized | Checked server reachability and associations; subsequent output confirmed synchronization. |
| Syslog severity keyword rejected | Encountered a simulator command limitation; forwarding was configured using supported logging commands. |

## Configuration backups

Running configurations were saved to startup configuration and backed up to the HQ TFTP server. The final backup set covers 14 company devices: eight at HQ, three in Abbotsford, and three in Kelowna.

ABB-ACCESS2 is excluded from TFTP backups because it has no configured management IP. The ISP has no route to the internal TFTP server. The final Packet Tracer project preserves their configurations as part of the lab.


![TFTP Final backup](screenshots/29-TFTP-Final-backup.png)



