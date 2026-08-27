# Network Topology

## Overview

This lab demonstrates an enterprise Cisco Network Access Control (NAC) environment using Cisco ISE/RADIUS with a Cisco switch and router.

## Network Devices

- NAC-R1 — Cisco Router
- NAC-SW1 — Cisco Catalyst Switch
- ISE-RADIUS — RADIUS/AAA Server
- CORP-PC1 — Corporate Endpoint
- GUEST-PC1 — Guest Endpoint
- ADMIN-PC1 — Administrative Endpoint

## VLAN Design

| VLAN | Name | Network | Gateway |
|------|------|---------|---------|
| 10 | CORPORATE | 10.10.10.0/24 | 10.10.10.1 |
| 20 | GUEST | 10.10.20.0/24 | 10.10.20.1 |
| 30 | ADMIN | 10.10.30.0/24 | 10.10.30.1 |
| 99 | MANAGEMENT | 10.10.99.0/24 | 10.10.99.1 |

## Management

- NAC-SW1: 10.10.99.2
- ISE-RADIUS: 10.10.99.10
- NAC-R1: 10.10.99.1

## Security Architecture

The lab implements:

- 802.1X authentication
- RADIUS authentication
- AAA
- NAC
- VLAN segmentation
- SSH-based device management
- VTY access control
- Guest network restrictions
- ACL-based security

## Topology Diagram

The topology diagram is included in this folder.

## Traffic Flow

```text
Endpoint
   |
   | 802.1X / Authentication
   v
NAC-SW1
   |
   | RADIUS
   v
ISE-RADIUS
   |
   | Authorization
   v
VLAN Assignment
   |
   v
NAC-R1
   |
   v
Inter-VLAN Communication
