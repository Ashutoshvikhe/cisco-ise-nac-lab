
# Device Configurations

This folder contains the sanitized configurations used in the enterprise Cisco NAC lab.

## Devices

### NAC-R1

NAC-R1 provides:

- Inter-VLAN routing
- Router-on-a-Stick
- VLAN gateway services
- Guest network security
- Extended ACL enforcement

Configuration:

[`NAC-R1-config.txt`](./NAC-R1-config.txt)

### NAC-SW1

NAC-SW1 provides:

- VLAN segmentation
- 802.1X authentication
- RADIUS integration
- AAA
- SSH version 2
- Management VLAN
- VTY access control
- 802.1Q trunking

Configuration:

[`NAC-SW1-config.txt`](./NAC-SW1-config.txt)

## VLAN Gateway Configuration

| VLAN | Name | Gateway |
|------|------|---------|
| 10 | CORPORATE | 10.10.10.1 |
| 20 | GUEST | 10.10.20.1 |
| 30 | ADMIN | 10.10.30.1 |
| 99 | MANAGEMENT | 10.10.99.1 |

## Management

| Device | Management IP |
|--------|---------------|
| NAC-R1 | 10.10.99.1 |
| NAC-SW1 | 10.10.99.2 |
| ISE-RADIUS | 10.10.99.10 |

## Security

The configurations implement:

- RADIUS-based authentication
- AAA
- 802.1X
- SSH v2
- VTY access restrictions
- VLAN segmentation
- Guest network isolation
- ACL-based access control

## Security Note

Credentials, password hashes, and RADIUS shared secrets have been removed or redacted from the configurations before publishing them to this public repository.
