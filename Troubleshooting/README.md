# Troubleshooting

## Overview

This folder documents troubleshooting scenarios encountered during the implementation and verification of the enterprise Cisco NAC lab.

The troubleshooting process involved identifying configuration errors, verifying device status, checking authentication and security configurations, and validating network behavior.

## Troubleshooting Methodology

The following approach was used:

1. Identify the problem.
2. Check the device and interface status.
3. Verify the relevant configuration.
4. Use Cisco `show` commands to isolate the issue.
5. Correct the configuration where required.
6. Re-test the affected functionality.
7. Save the corrected configuration.

---

## Issue 1 — Incorrect CLI Command

### Problem

The following command was entered incorrectly:

```text
NAC-SW1>enbale
```

The switch attempted to interpret the incorrect command and returned an error.

### Correct Command

```text
enable
```

### Resolution

The correct command was entered:

```text
NAC-SW1>enable
```

The switch then entered privileged EXEC mode:

```text
NAC-SW1#
```

### Lesson Learned

Cisco IOS commands must be entered using the correct syntax.

---

## Issue 2 — Incorrect Configuration Command

### Problem

The following command was entered:

```text
comfigure terminal
```

Cisco IOS returned:

```text
% Invalid input detected at '^' marker.
```

### Correct Command

```text
configure terminal
```

### Resolution

The correct command was entered:

```text
NAC-SW1#configure terminal
```

The switch entered global configuration mode:

```text
NAC-SW1(config)#
```

---

## Issue 3 — AAA and `login local`

### Problem

While configuring VTY access, the following command was attempted:

```text
login local
```

The switch returned:

```text
AAA is enabled. Command not supported.
Use an aaa authentication methodlist
```

### Cause

AAA was already enabled on NAC-SW1 using:

```text
aaa new-model
```

The VTY lines were therefore configured to use an AAA authentication method list.

### Resolution

The configured AAA method list was used:

```text
aaa authentication login RADIUS_AUTH group radius local
```

The VTY lines use:

```text
line vty 0 4
 login authentication RADIUS_AUTH
 transport input ssh
```

### Result

RADIUS is used as the primary authentication method, with local authentication available as a fallback.

---

## Issue 4 — OSPF Command on NAC-SW1

### Problem

The following command was entered on NAC-SW1:

```text
show ip ospf neighbor
```

The switch returned:

```text
% Invalid input detected at '^' marker.
```

### Investigation

The command was intended to verify OSPF neighbors.

However, the current lab does not implement OSPF on NAC-SW1.

The router was checked separately using:

```text
show ip ospf neighbor
```

and:

```text
show running-config | section router ospf
```

No OSPF configuration was present.

### Resolution

OSPF was not added because the current topology uses a single router with directly connected VLAN networks.

### Result

The absence of OSPF neighbors is expected in the current topology.

---

## Issue 5 — Guest Network Access Restrictions

### Problem

Guest traffic was being tested against internal networks.

The security requirement was to prevent Guest VLAN 20 from accessing:

- Corporate VLAN 10
- Admin VLAN 30
- Management VLAN 99

### Investigation

The ACL was checked using:

```text
show access-lists
```

The configured ACL was:

```text
Extended IP access list GUEST_RESTRICT

10 deny ip 10.10.20.0 0.0.0.255 10.10.10.0 0.0.0.255
20 deny ip 10.10.20.0 0.0.0.255 10.10.30.0 0.0.0.255
30 deny ip 10.10.20.0 0.0.0.255 10.10.99.0 0.0.0.255
40 permit ip 10.10.20.0 0.0.0.255 any
```

### ACL Application

The ACL was verified on:

```text
GigabitEthernet0/0.20
```

using:

```text
show ip interface GigabitEthernet0/0.20
```

The result showed:

```text
Inbound access list is GUEST_RESTRICT
```

### Result

ACL match counters confirmed that traffic was being processed by the configured security rules.

---

## Issue 6 — Verifying the Correct Device

### Problem

Some commands were initially entered on the wrong device.

For example:

```text
show ip ospf neighbor
```

was entered on NAC-SW1 instead of NAC-R1.

### Resolution

Commands were matched to the device responsible for the relevant function.

| Function | Device |
|----------|--------|
| VLANs | NAC-SW1 |
| 802.1X | NAC-SW1 |
| RADIUS / AAA | NAC-SW1 |
| SSH | NAC-SW1 |
| VTY access control | NAC-SW1 |
| Inter-VLAN routing | NAC-R1 |
| Guest ACL | NAC-R1 |
| OSPF | NAC-R1 if implemented |

### Lesson Learned

Always verify the current device prompt before executing a troubleshooting command.

---

## Issue 7 — Configuration Verification

### Problem

After configuration changes, it was necessary to confirm that the settings were actually applied.

### Verification Commands

The following commands were used:

```text
show running-config
show ip interface brief
show vlan brief
show interfaces trunk
show running-config | section aaa
show running-config | include radius
show ip ssh
show access-lists
show ip interface GigabitEthernet0/0.20
```

### Result

The commands confirmed the configured:

- VLANs
- Trunk
- Management interface
- Router subinterfaces
- AAA
- RADIUS
- SSH
- 802.1X
- ACL

---

## Issue 8 — Saving Configuration

### Problem

Configuration changes must survive a device reload.

### Resolution

The running configuration was saved using:

```text
copy running-config startup-config
```

The device returned:

```text
Building configuration...

[OK]
```

### Result

The configuration was successfully saved to startup-config.

---

## Troubleshooting Commands Reference

### Interface Status

```text
show ip interface brief
```

### VLANs

```text
show vlan brief
```

### Trunk

```text
show interfaces trunk
```

### Routing

```text
show ip route
```

### AAA

```text
show running-config | section aaa
```

### RADIUS

```text
show running-config | include radius
```

### SSH

```text
show ip ssh
```

### 802.1X

```text
show running-config | include dot1x
```

### ACL

```text
show access-lists
```

### ACL Interface Application

```text
show ip interface GigabitEthernet0/0.20
```

### Configuration

```text
show running-config
```

## Troubleshooting Summary

The lab provided practical troubleshooting experience involving:

- Cisco IOS command syntax
- Privileged EXEC and configuration modes
- AAA authentication
- RADIUS integration
- SSH access
- 802.1X
- VLAN segmentation
- Router-on-a-Stick
- ACL security
- Device-specific verification commands
- Configuration persistence

These troubleshooting scenarios demonstrate the ability to identify configuration issues, verify network behavior, and use Cisco IOS diagnostic commands to resolve problems.
