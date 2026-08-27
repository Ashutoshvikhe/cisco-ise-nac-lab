# 802.1X Network Access Control

## Overview

This folder documents the 802.1X Network Access Control (NAC) configuration implemented in the enterprise Cisco NAC lab.

The lab demonstrates IEEE 802.1X authentication with a Cisco Catalyst switch configured as the authenticator and a RADIUS server configured to provide centralized authentication.

## 802.1X Architecture

The lab uses the following components:

| Component | Device | Role |
|-----------|--------|------|
| Supplicant | CORP-PC1 | Endpoint requesting network access |
| Authenticator | NAC-SW1 | Controls access to the switch port |
| Authentication Server | ISE-RADIUS | Provides RADIUS authentication |
| Layer 3 Gateway | NAC-R1 | Provides inter-VLAN routing |

## Authentication Flow

The 802.1X authentication process is:

```text
CORP-PC1
   |
   | 802.1X / EAP
   v
NAC-SW1
   |
   | RADIUS
   v
ISE-RADIUS
   |
   | Authentication Response
   v
NAC-SW1
   |
   | Access Granted
   v
Corporate VLAN 10
```

## Global 802.1X Configuration

802.1X is enabled globally on NAC-SW1 using:

```text
dot1x system-auth-control
```

This enables the switch to perform 802.1X authentication on configured access ports.

## 802.1X Access Port

The corporate endpoint is connected to FastEthernet0/2.

The interface is configured as:

```text
interface FastEthernet0/2
 switchport access vlan 10
 switchport mode access
 authentication port-control auto
 dot1x pae authenticator
```

### Configuration Details

| Configuration | Purpose |
|---------------|---------|
| `switchport access vlan 10` | Places the port in the Corporate VLAN |
| `switchport mode access` | Configures the interface as an access port |
| `authentication port-control auto` | Enables authentication-controlled port access |
| `dot1x pae authenticator` | Configures the switch as the 802.1X authenticator |

## RADIUS Authentication

The switch uses RADIUS for 802.1X authentication.

The AAA configuration is:

```text
aaa new-model
aaa authentication dot1x default group radius
```

The configured RADIUS server is:

```text
10.10.99.10
```

The RADIUS server is located in:

```text
VLAN 99 - Management
Network: 10.10.99.0/24
```

## Corporate VLAN

The corporate endpoint access port is configured for:

```text
VLAN 10
Name: CORPORATE
Network: 10.10.10.0/24
Gateway: 10.10.10.1
```

## Authentication Process

The authentication process can be summarized as follows:

1. CORP-PC1 connects to NAC-SW1.
2. NAC-SW1 detects the endpoint on the 802.1X-enabled port.
3. NAC-SW1 initiates 802.1X authentication.
4. Authentication information is sent to the RADIUS server.
5. ISE-RADIUS processes the authentication request.
6. RADIUS returns an authentication response.
7. NAC-SW1 allows the authenticated endpoint to access the network.
8. Upon successful authentication, the endpoint is permitted through the 802.1X-controlled access port.

## Verification

The following commands can be used to verify the 802.1X configuration.

### Verify Global 802.1X

```text
show running-config | include dot1x
```

Expected configuration:

```text
dot1x system-auth-control
```

### Verify Interface Configuration

```text
show running-config interface FastEthernet0/2
```

Expected configuration:

```text
interface FastEthernet0/2
 switchport access vlan 10
 switchport mode access
 authentication port-control auto
 dot1x pae authenticator
```

### Verify AAA Configuration

```text
show running-config | section aaa
```

Expected output:

```text
aaa new-model
aaa authentication dot1x default group radius
aaa authentication login RADIUS_AUTH group radius local
```

### Verify RADIUS Configuration

```text
show running-config | include radius
```

The configured RADIUS server should be:

```text
10.10.99.10
```

## Security Controls

The 802.1X implementation provides:

- Port-based network access control
- Centralized authentication using RADIUS
- Controlled access to the Corporate VLAN
- Integration with Cisco AAA
- Authentication before network access is granted
- Separation between authenticated corporate access and other VLANs

## Lab Validation

The configuration was validated using Cisco Packet Tracer.

The switch configuration confirms that:

```text
dot1x system-auth-control
```

is enabled globally and that FastEthernet0/2 is configured as an 802.1X authenticator.

RADIUS authentication is configured through the centralized AAA method:

```text
aaa authentication dot1x default group radius
```

## Configuration File

The complete 802.1X-related switch configuration is also documented in:

```text
Device-Configurations/NAC-SW1-config.txt
```

## Limitations

This Packet Tracer lab demonstrates the 802.1X configuration and RADIUS integration conceptually.

Advanced Cisco ISE features such as:

- Endpoint profiling
- MAB
- Posture assessment
- BYOD
- Guest portal
- TrustSec
- Security Group Tags (SGTs)
- pxGrid

are outside the current implementation and may be added as future enhancements.

## Summary

This lab demonstrates a basic enterprise 802.1X NAC architecture using:

```text
CORP-PC1
     |
     | 802.1X
     v
NAC-SW1
     |
     | RADIUS
     v
ISE-RADIUS
     |
     v
Authenticated Network Access
     |
     v
Corporate VLAN 10
```

The implementation demonstrates how 802.1X, Cisco AAA, RADIUS, VLAN segmentation, and controlled switch-port access can be combined to provide network access control.
