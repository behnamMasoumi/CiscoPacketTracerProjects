# Law Firm Office Network — Cisco Packet Tracer

## Overview

This project simulates a **segmented law firm office network** designed to separate business departments, voice traffic, guests, servers, printers, and network management.

The network demonstrates practical enterprise networking concepts including **VLAN segmentation, Layer 3 inter-VLAN routing, trunking, DHCP/DNS services, ACL-based access control, VoIP, and static/default routing**.

The project was built and tested in **Cisco Packet Tracer**, with configuration and validation documented through the 23 screenshots included in the `images` folder.

## Network Segmentation

| VLAN | Name       | Network         | Purpose                     |
| ---- | ---------- | --------------- | --------------------------- |
| 10   | ATTORNEYS  | 192.168.10.0/24 | Attorney workstations       |
| 20   | PARALEGAL  | 192.168.20.0/24 | Paralegal workstations      |
| 30   | RECEPTION  | 192.168.30.0/24 | Reception workstations      |
| 40   | GUESTS     | 192.168.40.0/24 | Guest wireless access       |
| 50   | PRINTER    | 192.168.50.0/24 | Network printers            |
| 60   | SERVER     | 192.168.60.0/24 | DHCP and DNS services       |
| 70   | VOICE      | 192.168.70.0/24 | IP telephony                |
| 99   | MANAGEMENT | 192.168.99.0/24 | Network management          |
| 999  | NATIVE     | —               | Native VLAN for trunk links |


![Final Topology](images/23%20-%20Final%20Topology.png)

## Network Architecture

The office uses an **access-layer / Layer 3 switching architecture**.

* End devices connect to an access switch.
* The access switch uplinks to a Layer 3 switch using an **802.1Q trunk**.
* The trunk carries VLANs **10, 20, 30, 40, 50, 60, 70, and 99**.
* **VLAN 999** is configured as the native VLAN.
* SVIs are configured on the Layer 3 switch to provide default gateways for the VLANs.
* IP routing is enabled on the Layer 3 switch to provide inter-VLAN connectivity.
* The Layer 3 switch connects upstream to the router through a point-to-point `/30` network:

  * Layer 3 switch: `10.0.0.2`
  * Router: `10.0.0.1`
  * Network: `10.0.0.0/30`
* Last-resort/default routes are configured on both the Layer 3 switch and router to provide upstream connectivity.

## Voice Network

IP phones are logically separated from user data traffic using **Voice VLAN 70**.

The deployment includes:

* 3 attorney PCs + 3 IP phones
* 1 paralegal PC + 1 IP phone
* 1 reception PC + 1 IP phone

The phones receive their network connectivity through the access switches while remaining segmented from the corresponding user VLANs.

**Cisco CME/telephony service** is configured on the router to provide phone extensions and support the simulated office VoIP environment.

![Telephony Service configured on the router](images/18-%20Configuring%20telephony-service%20on%20the%20router%20for%20the%20VoIP%20phones.png)

## Guest Network Security

Guest wireless clients are isolated from the firm's internal networks.

* VLAN 40 is dedicated to guest access.
* A wireless access point currently provides connectivity to 3 guest clients.
* An **ACL** is configured to prevent guest devices from accessing internal VLANs.
* Guest users are permitted to use the network for external/internet access without gaining access to internal office resources.

This demonstrates basic **network segmentation and least-privilege access control** for an untrusted network.

![ACL configured against the GUEST Vlan](images/20%20-%20Configuring%20ACL%20on%20the%20layer%203%20switch%20to%20block%20access%20to%20the%20internal%20network%20from%20the%20guest%20VLAN.png)

## Network Services

A dedicated server network (VLAN 60) provides core infrastructure services:

* **DHCP** — automatic IP address assignment for network clients.
* **DNS** — name resolution for the internal network.

The network was tested to verify that clients could successfully obtain addressing information through DHCP and communicate with the required network resources.

![server to provide DHCP and DNS](images/10%20-%20setting%20up%20a%20dhcp%20server%20providing%20ip%20addresses%20for%20the%20devices.png)

## Configuration Highlights

Key technologies and configurations demonstrated in this project:

* VLAN creation and segmentation
* Access and trunk port configuration
* 802.1Q trunking
* Native VLAN configuration
* Layer 3 SVIs
* Inter-VLAN routing
* IP routing on a multilayer switch
* DHCP
* DNS
* Extended ACL configuration
* Guest network isolation
* Wireless networking
* Voice VLANs
* Cisco telephony/CME
* IP phone extensions
* Default/last-resort routing
* Point-to-point `/30` uplink
* Network connectivity validation

## Testing & Validation

After configuration, the network was tested to verify:

* DHCP address assignment
* Inter-VLAN connectivity
* Overall end-to-end network connectivity
* Guest VLAN access restrictions
* Guest access to external networks
* IP phone connectivity and extensions
* Routing between the Layer 3 switch and router
* Availability of DHCP and DNS services

![Testing connectivity and other things](images/22%20-%20Testing%20connectivity%20in%20our%20network.png)

The included screenshots document the topology, configuration, and testing performed throughout the project.

