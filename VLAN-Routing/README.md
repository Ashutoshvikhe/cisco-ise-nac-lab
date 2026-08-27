# VLAN and Routing

## Overview

This folder documents the VLAN segmentation and inter-VLAN routing implemented in the enterprise NAC lab.

The network uses VLAN segmentation on NAC-SW1 and Router-on-a-Stick on NAC-R1 to provide connectivity between VLANs.

## VLAN Design

| VLAN | Name | Network | Gateway |
|------|------|---------|---------|
| 10 | CORPORATE | 10.10.10.0/24 | 10.10.10.1 |
| 20 | GUEST | 10.10.20.0/24 | 10.10.20.1 |
| 30 | ADMIN | 10.10.30.0/24 | 10.10.30.1 |
| 99 | MANAGEMENT | 10.10.99.0/24 | 10.10.99.1 |

## Switch VLAN Assignment

| Interface | VLAN | Purpose |
|-----------|------|---------|
| Fa0/1 | 99 | Management |
| Fa0/2 | 10 | Corporate |
| Fa0/3 | 20 | Guest |
| Fa0/4 | 30 | Admin |
| Gi0/1 | Trunk | Router connection |

## Router-on-a-Stick

NAC-R1 uses the physical interface `GigabitEthernet0/0` with multiple 802.1Q subinterfaces.

| Subinterface | VLAN | IP Address |
|--------------|------|------------|
| G0/0.10 | 10 | 10.10.10.1 |
| G0/0.20 | 20 | 10.10.20.1 |
| G0/0.30 | 30 | 10.10.30.1 |
| G0/0.99 | 99 | 10.10.99.1 |

The switch-to-router connection uses an 802.1Q trunk.

## Guest Network Security

VLAN 20 is the Guest network.

The `GUEST_RESTRICT` extended ACL is applied inbound on `GigabitEthernet0/0.20`.

It blocks Guest VLAN traffic toward:

- Corporate VLAN 10
- Administrative VLAN 30
- Management VLAN 99

Other destinations are permitted by the final ACL rule.

## Verification

The routing configuration was verified using:

```text
show ip interface brief
show ip route
show ip interface GigabitEthernet0/0.20
show access-lists
