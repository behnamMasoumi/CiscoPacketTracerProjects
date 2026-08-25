# 🏢 Enterprise Multi-Building Network — Cisco Packet Tracer

## 📌 Project Overview

Designed and implemented a **redundant enterprise network** spanning two geographically separated buildings, connected through a routed WAN.

The network uses **VLAN segmentation, Layer 3 switching, HSRP gateway redundancy, OSPF dynamic routing, centralized DHCP/DNS services, 802.1Q trunking, and DHCP relay.**

The project was also used as a practical troubleshooting exercise, including identifying **routing, SVI, OSPF, DHCP, and redundancy issues** and validating the resulting fixes.

---

## 🏗️ Network Architecture


### **Building A**

- **VLAN 10 — HR**
- **VLAN 20 — Sales**
- **VLAN 30 — Finance**
- **3 Access Switches**
- **2 Layer 3 Distribution Switches**
- **1 WAN Router**

### **Building B**

- **VLAN 40 — HR**
- **VLAN 50 — Sales**
- **VLAN 60 — Finance**
- **3 Access Switches**
- **2 Layer 3 Distribution Switches**
- **1 WAN Router**

Access switches have **redundant connections to the distribution layer**. The distribution switches provide Layer 3 routing and gateway redundancy for the VLANs.

A centralized server network is also deployed using **VLAN 99 (Server)** for network services.

---

## ⚙️ Technologies Implemented

### **VLAN Segmentation**

Created separate VLANs for each department to provide logical segmentation and isolate broadcast domains.

| Building | VLAN | Department | Network |
| --- | --- | --- | --- |
| **A** | 10 | HR | `192.168.10.0/24` |
| **A** | 20 | Sales | `192.168.20.0/24` |
| **A** | 30 | Finance | `192.168.30.0/24` |
| **B** | 40 | HR | `192.168.40.0/24` |
| **B** | 50 | Sales | `192.168.50.0/24` |
| **B** | 60 | Finance | `192.168.60.0/24` |
| **Server** | 99 | Server | `192.168.100.0/24` |

---

### **802.1Q Trunking**

Configured **802.1Q trunk links** between access and distribution switches, as well as between distribution switches, allowing multiple VLANs to traverse shared links.

---

### **Layer 3 Inter-VLAN Routing**

Configured **SVIs** on the distribution switches and enabled Layer 3 routing using:

```
ip routing
```

This allows communication between VLANs directly through the distribution layer without relying on router-on-a-stick.

---

### **HSRP Gateway Redundancy**

Implemented **HSRP** between the two distribution switches in each building.

Each VLAN uses an **HSRP virtual IP address** as its default gateway.

Failover was tested by intentionally taking a distribution switch offline and verifying that hosts retained connectivity through the surviving distribution switch.

---

### **OSPF Dynamic Routing**

Implemented **OSPF** across the routed infrastructure to dynamically exchange routes between distribution switches and WAN routers.

VLAN-facing interfaces were configured as **passive interfaces**, allowing their networks to be advertised without attempting to form OSPF adjacencies with end devices.

---

### **Centralized DHCP, DNS & Web Services**

A standalone server in **VLAN 99 (Server)** was configured to provide centralized **DHCP, DNS, and web services**.

The server uses:

```
192.168.100.10
```

DHCP relay was implemented using:

```
ip helper-address 192.168.100.10
```

on the appropriate VLAN interfaces.

This allows clients in different subnets and buildings to obtain DHCP addresses from the centralized server.

---

## 🛠️ Troubleshooting Experience

A major objective of the project was to **troubleshoot configuration issues rather than simply build a functioning topology.**

### **SVI Routing Failure**

**Issue:** Inter-VLAN routing was not functioning despite correctly configured SVIs.

**Cause:** Layer 3 routing was not enabled on the distribution switches.

**Resolution:**

```
ip routing
```

**Lesson:** An SVI provides a Layer 3 interface, but the switch must have **IP routing enabled** to forward traffic between networks.

---

### **OSPF Configuration Issue**

**Issue:** OSPF was initially configured on VLAN interfaces without passive-interface settings.

**Resolution:** Configured VLAN-facing interfaces as **passive**, while keeping routed infrastructure links active for OSPF neighbor formation.

**Lesson:** Passive interfaces allow networks to be advertised without unnecessarily attempting OSPF adjacency formation toward end-user segments.

---

### **Centralized DHCP Across Layer 3 Boundaries**

**Issue:** Clients in remote VLANs could not initially obtain DHCP addresses from the centralized server.

**Cause:** DHCP discovery uses broadcasts, which do not normally cross Layer 3 boundaries.

**Resolution:** Configured DHCP relay with:

```
ip helper-address 192.168.100.10
```

on the appropriate SVIs.

**Lesson:** DHCP relay allows centralized DHCP services to operate across routed networks.

---

### **HSRP Failover**

**Test:** Intentionally disabled a distribution switch.

**Result:** The remaining distribution switch assumed the active gateway role and hosts retained network connectivity.

**Lesson:** Redundancy was validated through an actual failure scenario rather than simply verifying that HSRP was configured.

---

## 🔍 Validation & Testing

The completed topology was validated using:

- **End-to-end connectivity tests**
- **Inter-VLAN ping tests**
- **WAN connectivity tests**
- **DHCP address assignment**
- **DHCP relay verification**
- **DNS resolution testing**
- **Web server connectivity testing**
- **OSPF neighbor verification**
- **Routing table inspection**
- **HSRP failover testing**
- **VLAN and trunk verification**

The testing confirmed that the network could provide **inter-VLAN and inter-building connectivity** while maintaining segmentation, dynamic routing, centralized network services, and gateway redundancy.

---

## 🎯 Skills Demonstrated

### **Networking**

- VLANs
- 802.1Q trunking
- Layer 3 switching
- SVI configuration
- Inter-VLAN routing
- HSRP
- OSPF
- DHCP
- DHCP relay
- DNS
- Web services
- WAN routing
- Network troubleshooting

### **Cisco IOS**

- Interface configuration
- VLAN configuration
- SVI configuration
- Routing configuration
- OSPF configuration
- HSRP configuration
- DHCP relay configuration
- Verification and troubleshooting using `show` commands