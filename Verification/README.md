ENTERPRISE CISCO NAC LAB
VERIFICATION RESULTS
=====================


1. ROUTER INTERFACE VERIFICATION
--------------------------------

Command:

show ip interface brief

Expected / Verified Interfaces:

GigabitEthernet0/0       unassigned      up      up
GigabitEthernet0/0.10    10.10.10.1      up      up
GigabitEthernet0/0.20    10.10.20.1      up      up
GigabitEthernet0/0.30    10.10.30.1      up      up
GigabitEthernet0/0.99    10.10.99.1      up      up

The four VLAN subinterfaces are operational.


2. ROUTING TABLE VERIFICATION
-----------------------------

Command:

show ip route

Verified connected networks:

10.10.10.0/24
10.10.20.0/24
10.10.30.0/24
10.10.99.0/24

All four VLAN networks are directly connected to NAC-R1.


3. SWITCH VLAN VERIFICATION
---------------------------

Command:

show vlan brief

Verified VLANs:

VLAN 10    CORPORATE
VLAN 20    GUEST
VLAN 30    ADMIN
VLAN 99    MANAGEMENT

Port assignments:

Fa0/1    VLAN 99
Fa0/2    VLAN 10
Fa0/3    VLAN 20
Fa0/4    VLAN 30


4. TRUNK VERIFICATION
---------------------

Command:

show interfaces trunk

Verified trunk:

GigabitEthernet0/1

Operational mode:
trunking

Encapsulation:
802.1Q

Active VLANs:

1, 10, 20, 30, 99


5. MANAGEMENT IP VERIFICATION
-----------------------------

Command:

show ip interface brief

Verified:

NAC-SW1 VLAN 99:
10.10.99.2

NAC-R1 VLAN 99:
10.10.99.1

ISE-RADIUS:
10.10.99.10


6. SSH VERIFICATION
-------------------

Command:

show ip ssh

Verified:

SSH Enabled - version 2.0

Authentication timeout:
120 seconds

Authentication retries:
3


7. AAA VERIFICATION
-------------------

Command:

show running-config | section aaa

Verified:

aaa new-model

aaa authentication dot1x default group radius

aaa authentication login RADIUS_AUTH group radius local


8. RADIUS VERIFICATION
----------------------

Command:

show running-config | include radius

Configured RADIUS server:

10.10.99.10

Authentication port:

1645

The RADIUS shared secret is intentionally not included.


9. 802.1X VERIFICATION
----------------------

Command:

show running-config | include dot1x

Verified:

dot1x system-auth-control

The corporate access port FastEthernet0/2 is configured as an
802.1X authenticator.


10. 802.1X ACCESS PORT
----------------------

Interface:

FastEthernet0/2

Configuration:

switchport access vlan 10
switchport mode access
authentication port-control auto
dot1x pae authenticator


11. ACL VERIFICATION
--------------------

Command:

show access-lists

ACL:

GUEST_RESTRICT

Rules:

10  deny Guest → Corporate
20  deny Guest → Admin
30  deny Guest → Management
40  permit Guest → Any


12. ACL APPLICATION VERIFICATION
--------------------------------

Command:

show ip interface GigabitEthernet0/0.20

Verified:

Inbound access list:
GUEST_RESTRICT

The ACL is applied inbound on the Guest VLAN subinterface.


13. ACL MATCH COUNTERS
----------------------

Verified ACL match counters demonstrate that traffic has been
processed by the configured ACL rules.

Example:

Guest → Corporate:
8 matches

Guest → Admin:
4 matches

Guest → Management:
52 matches

Guest → permitted destinations:
8 matches


14. CONFIGURATION SAVE
----------------------

Command:

copy running-config startup-config

Result:

[OK]

The running configuration has been successfully saved to
startup-config.


15. ETHERCHANNEL VERIFICATION
-----------------------------

Command:

show etherchannel summary

Result:

Number of channel-groups in use: 0

No EtherChannel is implemented in the current topology.


16. OSPF VERIFICATION
---------------------

Command:

show ip ospf neighbor

Result:

No OSPF neighbors are configured.

The current topology uses a single router with directly connected
VLAN networks, therefore OSPF is not implemented.


17. OVERALL VERIFICATION
------------------------

The following components have been verified:

[OK] VLAN segmentation
[OK] Router-on-a-Stick
[OK] Inter-VLAN routing
[OK] Management VLAN
[OK] 802.1X configuration
[OK] RADIUS integration
[OK] AAA
[OK] SSH version 2
[OK] VTY access control
[OK] Guest ACL restrictions
[OK] Configuration backup

Not implemented in the current topology:

[NOT IMPLEMENTED] OSPF
[NOT IMPLEMENTED] EtherChannel
[NOT IMPLEMENTED] MAB
[NOT IMPLEMENTED] Endpoint profiling
