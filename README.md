# Enterprise Cisco ISE Network Access Control (NAC) Lab

A practical enterprise Network Access Control (NAC) lab demonstrating Cisco ISE/RADIUS concepts, 802.1X configuration, AAA, VLAN segmentation, inter-VLAN routing, SSH-based device management, ACL-based security, network verification, and troubleshooting using Cisco Packet Tracer.

---

## Project Overview

This project simulates an enterprise network access control environment where endpoints connect to a Cisco access switch and are authenticated through a centralized RADIUS/AAA server.

The lab demonstrates how network access can be controlled using:

- 802.1X authentication
- RADIUS
- AAA
- VLAN segmentation
- Router-on-a-Stick
- SSH
- VTY access control
- Guest network restrictions
- Extended ACLs
- Network verification
- Troubleshooting

The project was designed and implemented using Cisco Packet Tracer.

---

## Network Architecture

```text
                         +----------------+
                         |     NAC-R1     |
                         | Cisco Router   |
                         |                |
                         | Router-on-a-   |
                         |     Stick      |
                         +-------+--------+
                                 |
                                 | 802.1Q Trunk
                                 |
                         +-------+--------+
                         |    NAC-SW1     |
                         | Cisco Catalyst |
                         |     Switch     |
                         +---+---+---+----+
                             |   |   |
               +-------------+   |   +-------------+
               |                 |                 |
        +------+-----+     +-----+------+    +-----+------+
        |  CORP-PC1  |     |  GUEST-PC1 |    |  ADMIN-PC1 |
        |  VLAN 10   |     |  VLAN 20   |    |  VLAN 30   |
        +------------+     +------------+    +------------+

                         |
                         | RADIUS
                         |
                  +------+-------+
                  | ISE-RADIUS  |
                  | 10.10.99.10 |
                  | Management   |
                  |    VLAN 99   |
                  +--------------+
```

---

## Network Devices

| Device | Role |
|--------|------|
| NAC-R1 | Cisco Router / Inter-VLAN Routing |
| NAC-SW1 | Cisco Catalyst Access Switch |
| ISE-RADIUS | RADIUS / AAA Server |
| CORP-PC1 | Corporate Endpoint |
| GUEST-PC1 | Guest Endpoint |
| ADMIN-PC1 | Administrative Endpoint |

---

## VLAN and IP Addressing

| VLAN | Name | Network | Gateway |
|------|------|---------|---------|
| 10 | CORPORATE | 10.10.10.0/24 | 10.10.10.1 |
| 20 | GUEST | 10.10.20.0/24 | 10.10.20.1 |
| 30 | ADMIN | 10.10.30.0/24 | 10.10.30.1 |
| 99 | MANAGEMENT | 10.10.99.0/24 | 10.10.99.1 |

### Management Addresses

| Device | IP Address |
|--------|------------|
| NAC-R1 | 10.10.99.1 |
| NAC-SW1 | 10.10.99.2 |
| ISE-RADIUS | 10.10.99.10 |

---

## Security Architecture

The lab implements multiple network security controls:

### 802.1X

Corporate endpoint access is protected using 802.1X authentication.

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
   v
Corporate VLAN 10
```

### RADIUS

The switch communicates with the centralized RADIUS server:

```text
RADIUS Server: 10.10.99.10
Authentication Port: 1645
Protocol: RADIUS
```

### AAA

AAA is used for centralized authentication:

```text
aaa new-model
aaa authentication dot1x default group radius
aaa authentication login RADIUS_AUTH group radius local
```

### SSH

Administrative access uses SSH version 2.

```text
ip ssh version 2
```

VTY access is controlled using an AAA authentication method:

```text
line vty 0 4
 access-class 10 in
 login authentication RADIUS_AUTH
 transport input ssh
```

### VLAN Segmentation

The network is divided into separate security zones:

- Corporate
- Guest
- Administrative
- Management

### Guest Network Restrictions

Guest VLAN 20 is restricted from accessing:

- Corporate VLAN 10
- Administrative VLAN 30
- Management VLAN 99

The `GUEST_RESTRICT` ACL is applied inbound to the Guest VLAN interface.

---

## Routing

The lab uses Router-on-a-Stick for inter-VLAN routing.

Router subinterfaces:

```text
GigabitEthernet0/0.10
10.10.10.1/24

GigabitEthernet0/0.20
10.10.20.1/24

GigabitEthernet0/0.30
10.10.30.1/24

GigabitEthernet0/0.99
10.10.99.1/24
```

The switch-to-router connection operates as an 802.1Q trunk.

---

## ACL Security

The Guest VLAN uses an extended ACL:

```text
Extended IP access list GUEST_RESTRICT

10 deny ip 10.10.20.0 0.0.0.255 10.10.10.0 0.0.0.255
20 deny ip 10.10.20.0 0.0.0.255 10.10.30.0 0.0.0.255
30 deny ip 10.10.20.0 0.0.0.255 10.10.99.0 0.0.0.255
40 permit ip 10.10.20.0 0.0.0.255 any
```

This provides controlled Guest network access while allowing permitted traffic.

---

## Verification

The implementation was verified using Cisco IOS commands including:

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

Connectivity was also tested between the relevant devices and VLAN gateways.

---

## Troubleshooting

The project includes practical troubleshooting scenarios involving:

- Cisco IOS command syntax errors
- AAA authentication behavior
- SSH authentication
- RADIUS configuration
- 802.1X configuration
- VLAN verification
- Trunk verification
- Inter-VLAN routing
- Guest ACL restrictions
- Device-specific troubleshooting commands
- Configuration persistence

Configuration changes were saved using:

```text
copy running-config startup-config
```

---

## Project Structure

```text
cisco-ise-nac-lab/
│
├── AAA-RADIUS/
│   ├── README.md
│   └── radius-aaa-configuration.txt
│
├── ACL-Security/
│   └── README.md
│
├── Device-Configurations/
│   └── README.md
│
├── NAC-802.1X/
│   └── README.md
│
├── Screenshots/
│   ├── 01-topology.png
│   ├── 02-vlan-verification.png
│   ├── 03-trunk-verification.png
│   ├── 04-router-intervlan.png
│   ├── 05-aaa-radius.png
│   ├── 06-ssh-verification.png
│   ├── 07-8021x-verification.png
│   ├── 08-acl-verification.png
│   ├── 09-connectivity-tests.png
│   ├── 10-troubleshooting.png
│   └── README.md
│
├── Topology/
│   ├── README.md
│   └── enterprise-nac-topology.png
│
├── Troubleshooting/
│   └── README.md
│
├── VLAN-Routing/
│   ├── README.md
│   └── router-on-a-stick.txt
│
├── Verification/
│   └── README.md
│
└── README.md
```

---

## Skills Demonstrated

### Networking

- Cisco Switching
- Cisco Routing
- VLANs
- 802.1Q Trunking
- Inter-VLAN Routing
- Router-on-a-Stick
- IP Addressing
- Network Connectivity Testing

### Network Security

- Cisco ISE concepts
- Network Access Control
- 802.1X
- RADIUS
- AAA
- SSH Security
- VTY Access Control
- ACLs
- VLAN Segmentation
- Guest Network Security

### Troubleshooting

- Cisco IOS verification
- Connectivity troubleshooting
- Authentication troubleshooting
- ACL troubleshooting
- VLAN troubleshooting
- Routing troubleshooting
- Configuration verification

---

## Tools Used

- Cisco Packet Tracer
- Cisco IOS
- RADIUS
- Git
- GitHub

---

## Project Outcome

The completed lab demonstrates an enterprise-style NAC architecture where endpoint authentication, network segmentation, administrative access, and Guest network restrictions are implemented using 802.1X, RADIUS/AAA, VLANs, SSH, and ACL-based security controls.

The project also documents configuration, verification, troubleshooting, and visual evidence of the implemented network.

---

## Repository Documentation

Detailed documentation is available in the following sections:

- [Topology](./Topology/)
- [AAA and RADIUS](./AAA-RADIUS/)
- [ACL Security](./ACL-Security/)
- [Device Configurations](./Device-Configurations/)
- [802.1X NAC](./NAC-802.1X/)
- [VLAN and Routing](./VLAN-Routing/)
- [Verification](./Verification/)
- [Troubleshooting](./Troubleshooting/)
- [Screenshots](./Screenshots/)

---

## Security Note

No production credentials or sensitive secrets should be stored in this repository.

RADIUS shared secrets, passwords, and password hashes are represented as:

```text
<REDACTED>
```

This project is intended for educational and portfolio purposes.
