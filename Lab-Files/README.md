# Cisco Packet Tracer Lab Files

## Overview

This folder contains the Cisco Packet Tracer `.pkt` file for the **Enterprise Cisco ISE Network Access Control (NAC) Lab**.

The lab demonstrates an enterprise-style NAC environment using Cisco Packet Tracer, including:

- 802.1X authentication
- RADIUS authentication
- AAA
- VLAN segmentation
- Router-on-a-Stick
- Inter-VLAN routing
- SSH version 2
- VTY access control
- Guest network restrictions
- Extended ACL security
- Network verification
- Troubleshooting

---

## Lab File

### Enterprise Cisco NAC Lab

**Packet Tracer File:**

[Download Enterprise Cisco NAC Lab](./enterprise-cisco-nac-lab.pkt)

The `.pkt` file contains the network topology and device configurations used for this project.

> **Note:** The file name in the download link must exactly match the `.pkt` file uploaded to this folder.

---

## Network Components

The Packet Tracer topology includes:

| Device | Role |
|--------|------|
| NAC-R1 | Cisco Router / Inter-VLAN Routing |
| NAC-SW1 | Cisco Catalyst Switch |
| ISE-RADIUS | RADIUS / AAA Server |
| CORP-PC1 | Corporate Endpoint |
| GUEST-PC1 | Guest Endpoint |
| ADMIN-PC1 | Administrative Endpoint |

---

## VLANs

| VLAN | Name | Network | Gateway |
|------|------|---------|---------|
| 10 | CORPORATE | 10.10.10.0/24 | 10.10.10.1 |
| 20 | GUEST | 10.10.20.0/24 | 10.10.20.1 |
| 30 | ADMIN | 10.10.30.0/24 | 10.10.30.1 |
| 99 | MANAGEMENT | 10.10.99.0/24 | 10.10.99.1 |

---

## Security Implementation

The lab demonstrates:

```text
802.1X
   |
   v
NAC-SW1
   |
   | RADIUS
   v
ISE-RADIUS
   |
   v
Authenticated Network Access
```

Guest network traffic is restricted using the `GUEST_RESTRICT` extended ACL.

Administrative access to the switch is secured using:

- AAA
- RADIUS
- Local authentication fallback
- SSH version 2
- VTY access control

---

## How to Open the Lab

1. Install Cisco Packet Tracer.
2. Download the `.pkt` file from this folder.
3. Open the `.pkt` file using Cisco Packet Tracer.
4. Review the network topology and device configurations.
5. Use Cisco IOS verification commands to validate the implementation.

---

## Verification Commands

Useful commands included in the lab:

```text
show running-config
show ip interface brief
show ip route
show vlan brief
show interfaces trunk
show running-config | section aaa
show running-config | include radius
show ip ssh
show access-lists
show ip interface GigabitEthernet0/0.20
show running-config | include dot1x
```

---

## Documentation

Detailed configuration and verification documentation is available in the main repository:

- [Topology](../Topology/)
- [AAA and RADIUS](../AAA-RADIUS/)
- [ACL Security](../ACL-Security/)
- [Device Configurations](../Device-Configurations/)
- [802.1X NAC](../NAC-802.1X/)
- [VLAN and Routing](../VLAN-Routing/)
- [Verification](../Verification/)
- [Troubleshooting](../Troubleshooting/)
- [Screenshots](../Screenshots/)

---

## Security Note

This is an educational and portfolio lab created using Cisco Packet Tracer.

Do not use production credentials or sensitive information in the lab file.

RADIUS shared secrets, passwords, and other sensitive credentials should not be published in the repository.

---

## Purpose

This lab file provides the working Packet Tracer implementation behind the documentation in the **Enterprise Cisco ISE Network Access Control (NAC) Lab** project.

It is intended for:

- Networking practice
- Cisco NAC learning
- 802.1X and RADIUS practice
- Troubleshooting practice
- Technical portfolio demonstration
- Network Security interview preparation
