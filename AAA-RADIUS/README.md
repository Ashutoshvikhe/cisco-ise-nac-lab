# AAA and RADIUS

## Overview

This folder documents the AAA and RADIUS authentication configuration used in the enterprise Cisco NAC lab.

The lab uses a RADIUS server to provide centralized authentication for network access and device management.

## RADIUS Server

| Parameter | Value |
|-----------|-------|
| Server | ISE-RADIUS |
| IP Address | 10.10.99.10 |
| Authentication Port | 1645 |
| Protocol | RADIUS |
| Shared Secret | Redacted |

## AAA Configuration

NAC-SW1 uses Cisco AAA with RADIUS:

```text
aaa new-model
aaa authentication dot1x default group radius
aaa authentication login RADIUS_AUTH group radius local
```

## Authentication Methods

### 802.1X Authentication

The switch uses the RADIUS server for 802.1X authentication.

```text
aaa authentication dot1x default group radius
```

### SSH Device Authentication

SSH access uses RADIUS as the primary authentication method, with local authentication available as a fallback.

```text
aaa authentication login RADIUS_AUTH group radius local
```

## RADIUS Integration

The switch communicates with the RADIUS server at:

```text
10.10.99.10
```

The RADIUS server is located in the Management VLAN:

```text
VLAN 99
Network: 10.10.99.0/24
```

The switch management address is:

```text
NAC-SW1
10.10.99.2/24
```

The default gateway is:

```text
10.10.99.1
```

## 802.1X Configuration

NAC-SW1 is configured as an 802.1X authenticator.

### Global 802.1X Configuration

```text
dot1x system-auth-control
```

The corporate endpoint access port uses:

```text
interface FastEthernet0/2
 switchport access vlan 10
 switchport mode access
 authentication port-control auto
 dot1x pae authenticator
```

### Authentication Flow

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
   | Authentication Response
   v
NAC-SW1
   |
   | Access Granted
   v
Corporate VLAN 10
```

## SSH Authentication

SSH access to NAC-SW1 uses the AAA authentication method:

```text
line vty 0 4
 access-class 10 in
 login authentication RADIUS_AUTH
 transport input ssh
```

This provides centralized authentication for administrative access.

The switch is configured for SSH version 2:

```text
ip ssh version 2
```

The local domain is:

```text
enterprise.local
```

## Local Authentication Fallback

The AAA method is configured with:

```text
aaa authentication login RADIUS_AUTH group radius local
```

This means RADIUS is attempted first. Local authentication can be used as a fallback if the RADIUS server is unavailable.

## Management Network

| Device | IP Address | VLAN |
|--------|------------|------|
| NAC-SW1 | 10.10.99.2 | 99 |
| ISE-RADIUS | 10.10.99.10 | 99 |
| NAC-R1 | 10.10.99.1 | 99 |

## RADIUS Parameters

| Parameter | Configuration |
|-----------|---------------|
| RADIUS Server | 10.10.99.10 |
| Authentication Port | 1645 |
| Protocol | RADIUS |
| Shared Secret | Redacted |

The shared secret is intentionally not published in this repository.

## Verification

The following commands can be used to verify the AAA and RADIUS configuration:

```text
show running-config | section aaa
show running-config | include radius
show ip ssh
show running-config | section line vty
show running-config | include dot1x
```

### Expected Verification

AAA configuration should show:

```text
aaa new-model
aaa authentication dot1x default group radius
aaa authentication login RADIUS_AUTH group radius local
```

SSH should show:

```text
SSH Enabled - version 2.0
```

The RADIUS server should be configured as:

```text
10.10.99.10
```

## Security Considerations

The RADIUS shared secret, passwords, and password hashes are not published in this repository.

Sensitive credentials are represented as:

```text
<REDACTED>
```

