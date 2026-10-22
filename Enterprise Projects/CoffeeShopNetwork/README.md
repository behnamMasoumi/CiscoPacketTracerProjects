# ☕ Coffee Shop Network — Cisco Packet Tracer

A small-business network designed and implemented in Cisco Packet Tracer for a coffee shop environment. The network provides separate and secure network segments for office operations, point-of-sale (POS) systems, guest Wi-Fi, and network management.

The project demonstrates practical implementation of **VLAN segmentation, router-on-a-stick inter-VLAN routing, DHCP, ACLs, SSH management, trunking, port security, and Layer 2 security hardening.**

The topology represents a small coffee shop with an office, front payment desk, guest Wi-Fi, and an Internet connection through an ISP.

---

## 🏪 Network Scenario

The coffee shop has three primary areas:

- **Office:** Contains an office PC and network printer.
- **Front Desk / POS:** Contains a POS workstation and receipt printer used for customer payments.
- **Guest Area:** Provides Wi-Fi access for customers.

All devices connect to a **Layer 3 switch**, which provides the Layer 2 infrastructure and connects to an edge router. The router provides **inter-VLAN routing, DHCP services, security filtering, and the connection to the ISP/Internet.**

A dedicated **Management VLAN** is also configured to allow secure remote administration using SSH.

---

## 🗂️ VLAN & IP Addressing

| VLAN | Purpose | Network | Gateway |
| --- | --- | --- | --- |
| **10** | Office | `192.168.10.0/24` | `192.168.10.1` |
| **20** | POS | `192.168.20.0/24` | `192.168.20.1` |
| **30** | Guest Wi-Fi | `192.168.30.0/24` | `192.168.30.1` |
| **99** | Management | `192.168.99.0/24` | `192.168.99.1` |
| **999** | Native VLAN | — | — |

**VLAN 999** is used as the native VLAN rather than the default VLAN 1.

---

## 🔀 VLAN & Trunking Configuration

Access ports on the Layer 3 switch are assigned to the appropriate VLANs:

- **VLAN 10 →** Office devices
- **VLAN 20 →** POS devices
- **VLAN 30 →** Guest devices

The switch-to-router uplink is configured as an **802.1Q trunk** carrying:

- VLAN 10
- VLAN 20
- VLAN 30
- VLAN 99
- **Native VLAN 999**

The network uses **router-on-a-stick**, with separate router subinterfaces providing Layer 3 gateways for each VLAN.

### Router Subinterfaces

```
Router
├── G0/0.10 → VLAN 10 → 192.168.10.1
├── G0/0.20 → VLAN 20 → 192.168.20.1
├── G0/0.30 → VLAN 30 → 192.168.30.1
└── G0/0.99 → VLAN 99 → 192.168.99.1
```

---

## 🌐 DHCP

The edge router provides **DHCP services** for the Office, POS, and Guest VLANs.

This allows devices in VLANs 10, 20, and 30 to automatically receive:

- IP address
- Subnet mask
- Default gateway
- DNS information

For the printers, I configured **static IP addresses** to avoid connectivity issues during normal business operations.

---

## 🔐 Guest Network Security

An **extended ACL** was configured on the router to restrict traffic originating from the Guest VLAN.

The goal is to allow guest users to access the **Internet** while preventing them from accessing internal network segments such as:

- Office VLAN
- POS VLAN
- Management VLAN

This provides basic network segmentation and protects internal business resources from guest devices.

The ACL was tested after implementation to verify that guest devices were prevented from reaching internal networks while maintaining Internet connectivity.

---

## 🛡️ Layer 2 Security Hardening

Several Layer 2 security measures were implemented.

### **Native VLAN 999**

The default native VLAN was changed from VLAN 1 to VLAN 999.

This reduces exposure to certain VLAN-hopping scenarios, including double-tagging attacks, by ensuring that the native VLAN is not used for normal user traffic.

### **Port Security**

Port security was enabled on applicable access interfaces to restrict the number of MAC addresses permitted on each port.

The configuration was designed to allow **one MAC address per applicable access interface**, helping prevent unauthorized devices from being connected to protected network ports.

### **Unused Ports**

Unused switch ports were disabled to reduce the available attack surface and prevent unauthorized devices from being connected to unused interfaces.

---

## 🔑 SSH Remote Management

SSH was configured to allow **secure remote management** of the network devices.

The management network uses:

| Configuration | Value |
| --- | --- |
| **Management VLAN** | VLAN 99 |
| **Switch Management IP** | `192.168.99.2` |
| **Router VLAN 99 Gateway** | `192.168.99.1` |

This provides a dedicated management segment rather than relying on an end-user VLAN for device administration.

---

## 🧪 Troubleshooting & Lessons Learned

### **Port Security Accidentally Applied to the Trunk**

I initially applied port security across a range of switch interfaces that included the **switch-to-router trunk**.

Because the trunk carries traffic from multiple VLANs, the port security configuration resulted in a security violation and the interface entered an **err-disabled** state.

I identified the affected interface, removed port security from the trunk, and brought the interface back online.

**Lesson:** Port security should generally be applied to appropriate **edge/access ports**, not infrastructure trunk links.

---

### **ISL vs. 802.1Q Trunking**

When initially configuring trunking, the switch was using **ISL encapsulation** rather than 802.1Q.

I identified the encapsulation issue and changed the trunk to **802.1Q**, allowing the VLANs to be carried correctly between the switch and router.

**Lesson:** Understanding trunk encapsulation is important when configuring Cisco devices, particularly when working with older equipment or Packet Tracer devices that support multiple encapsulation types.

---

### **Management VLAN Connectivity Issue**

While configuring SSH management, I initially could not reach the switch's management IP:

`192.168.99.2`

After troubleshooting the configuration, I discovered that I had accidentally enabled:

```
ip routing
```

on the Layer 3 switch.

Because this project uses **router-on-a-stick**, inter-VLAN routing was intended to be handled by the router rather than the switch. The switch only needed its **VLAN 99 SVI for management.**

I removed the unnecessary routing configuration and configured the appropriate management gateway:

`192.168.99.1`

After correcting the configuration, connectivity to the management interface was restored and **SSH was successfully tested.**

**Lesson:** Understanding which device is responsible for Layer 3 routing is critical. A Layer 3 switch is capable of routing, but enabling that capability when the router is intentionally performing the routing can create unexpected behavior.

---

## 🧪 Testing & Verification

After completing the configuration, the network was tested to verify functionality.

Testing included:

- **VLAN connectivity**
- **DHCP address assignment**
- **Inter-VLAN routing**
- **Router-on-a-stick functionality**
- **VLAN 99 management connectivity**
- **SSH remote management**
- **Guest VLAN isolation**
- **ACL functionality**
- **Internet connectivity**
- **Trunk operation**
- **Native VLAN configuration**
- **Port security behavior**
- **Disabled unused interfaces**

All major network functions were successfully tested after troubleshooting and correcting the configuration issues described above.