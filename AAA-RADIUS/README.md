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
