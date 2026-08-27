# ACL Security

## Overview

This folder documents the Access Control List (ACL) configuration implemented in the enterprise Cisco NAC lab.

The ACL is used to restrict traffic originating from the Guest VLAN while allowing permitted traffic to other networks.

The security objective is to prevent guest endpoints from accessing internal corporate, administrative, and management networks.

## Security Policy

The Guest VLAN is isolated from the following internal networks:

- Corporate VLAN 10
- Admin VLAN 30
- Management VLAN 99

Guest traffic to other destinations is permitted.

## Network Information

| VLAN | Network | Purpose |
|------|---------|---------|
| 10 | 10.10.10.0/24 | Corporate |
| 20 | 10.10.20.0/24 | Guest |
| 30 | 10.10.30.0/24 | Admin |
| 99 | 10.10.99.0/24 | Management |

## Guest Restriction ACL

The router uses an extended ACL named:

```text
GUEST_RESTRICT
```

The ACL configuration is:

```text
ip access-list extended GUEST_RESTRICT
 deny ip 10.10.20.0 0.0.0.255 10.10.10.0 0.0.0.255
 deny ip 10.10.20.0 0.0.0.255 10.10.30.0 0.0.0.255
 deny ip 10.10.20.0 0.0.0.255 10.10.99.0 0.0.0.255
 permit ip 10.10.20.0 0.0.0.255 any
```

### ACL Logic

| Sequence | Source | Destination | Action |
|----------|--------|-------------|--------|
| 10 | Guest VLAN 20 | Corporate VLAN 10 | Deny |
| 20 | Guest VLAN 20 | Admin VLAN 30 | Deny |
| 30 | Guest VLAN 20 | Management VLAN 99 | Deny |
| 40 | Guest VLAN 20 | Any | Permit |

## ACL Application

The ACL is applied inbound on the Guest VLAN router subinterface:

```text
interface GigabitEthernet0/0.20
 ip address 10.10.20.1 255.255.255.0
 ip access-group GUEST_RESTRICT in
```

This allows the router to inspect traffic entering the routing device from the Guest VLAN before it reaches other networks.

## Traffic Policy

### Blocked Traffic

Guest endpoints cannot access:

```text
10.10.10.0/24    Corporate Network
10.10.30.0/24    Admin Network
10.10.99.0/24    Management Network
```

### Permitted Traffic

Guest endpoints are permitted to communicate with destinations outside the explicitly blocked internal networks.

```text
10.10.20.0/24 → Any
```

The final permit statement allows other traffic from the Guest network.

## Verification

Use the following commands on `NAC-R1` to verify the ACL:

```text
show access-lists
show ip interface GigabitEthernet0/0.20
```

### Expected ACL Verification

```text
Extended IP access list GUEST_RESTRICT
    10 deny ip 10.10.20.0 0.0.0.255 10.10.10.0 0.0.0.255
    20 deny ip 10.10.20.0 0.0.0.255 10.10.30.0 0.0.0.255
    30 deny ip 10.10.20.0 0.0.0.255 10.10.99.0 0.0.0.255
    40 permit ip 10.10.20.0 0.0.0.255 any
```

The ACL should be shown as an inbound ACL on the Guest VLAN subinterface:

```text
GigabitEthernet0/0.20 is up, line protocol is up
Internet address is 10.10.20.1/24
Inbound access list is GUEST_RESTRICT
```

## Testing

Traffic from `GUEST-PC1` can be tested against the following destinations:

| Test | Expected Result |
|------|-----------------|
| Guest → 10.10.20.1 | Permit |
| Guest → Corporate network | Block |
| Guest → Admin network | Block |
| Guest → Management network | Block |

Example tests:

```text
ping 10.10.20.1
ping 10.10.10.10
ping 10.10.30.20
ping 10.10.99.10
```

The blocked destinations should not be reachable from the Guest VLAN when the ACL is operating correctly.

## Security Benefits

The ACL provides basic network segmentation by:

- Isolating Guest users from internal corporate resources
- Preventing Guest access to administrative systems
- Protecting the management network
- Reducing unauthorized lateral movement
- Enforcing network-level access restrictions
- Providing a simple security control for the NAC lab

## Summary

The `GUEST_RESTRICT` ACL provides an additional security layer for the enterprise NAC environment.

Guest traffic is restricted from accessing Corporate, Admin, and Management VLANs while permitted traffic to other destinations is allowed.

This demonstrates how ACLs can be combined with VLAN segmentation, router-on-a-stick inter-VLAN routing, and NAC controls to implement layered network security.
