# Thuto Skills Training College - Network Design and Implementation

## Project Overview

This repository documents the design, implementation and testing of a computer network for **Thuto Skills Training College (Klerksdorp)**.

- **Client ID**: CLI-023
- **Assigned addressing block**: 10.18.0.0/16
- **Assigned networking challenge**: VLAN Trunking (802.1Q across switches)
- **Client change request**: CR9 - secure off-site administrator access
- **Design constraint**: Boardroom requires dedicated wireless and wired presentation ports

## Repository Structure

    01-requirements.md        Client requirements, scope and constraints
    02-design/                Milestone 1 design documents
      ip-addressing-plan.md   VLAN and subnet addressing scheme
      physical-topology.png   Physical device and cabling diagram
      logical-topology.jpg    VLAN/IP logical view of the network
      assumptions.md          Design assumptions made where the brief is silent
    03-implementation/        Milestone 2 Packet Tracer configuration evidence and .pkt file
    04-testing/               Milestone 2 connectivity and feature verification evidence
    05-reflection/            Project reflection (to be added)

## Design Summary

The network uses a router connected to two switches joined by an 802.1Q trunk link, supporting four VLANs:

| VLAN | Purpose | Subnet | Gateway |
|------|---------|--------|---------|
| 10 | Admin | 10.18.10.0/24 | 10.18.10.1 |
| 20 | Labs | 10.18.20.0/24 | 10.18.20.1 |
| 30 | Boardroom | 10.18.30.0/24 | 10.18.30.1 |
| 99 | Management | 10.18.99.0/24 | 10.18.99.1 |

The boardroom has both wired and wireless presentation access (per the client's design constraint), and a dedicated Management VLAN supports CR9's requirement for secure remote SSH access to network devices.

## Milestone 2 - Implementation Summary

The working Packet Tracer file is `03-implementation/Thuto_College_Network_M2.pkt`.

**VLANs and trunking (assigned challenge)**
- VLANs 10, 20, 30 and 99 were created on both CORE-SW and ACCESS-SW.
- CORE-SW Fa0/2 and ACCESS-SW Fa0/2 form the 802.1Q trunk between the switches, allowing VLANs 10, 20, 30 and 99.
- CORE-SW Fa0/1 is also a trunk, carrying all VLANs to the router.
- Access ports: Admin PCs on CORE-SW Fa0/3-4 (VLAN 10); Lab PCs on ACCESS-SW Fa0/5-7 (VLAN 20); Board-AP on Fa0/8 and Board PC on Fa0/9 (VLAN 30).

**Inter-VLAN routing**
- Router-on-a-stick: HQ-ROUTER Gig0/0 is split into subinterfaces .10, .20, .30 and .99, each holding that VLAN's gateway address.
- The router's second link to ACCESS-SW (Gig0/1) is cabled but left administratively shut down as a spare, so it does not create a loop with the trunk.

**Boardroom (design constraint)**
- Wired presentation port: Board PC on ACCESS-SW Fa0/9.
- Wireless presentation port: Board-AP (SSID Thuto-Boardroom, WPA2-PSK with AES) with the Board Laptop connected wirelessly. Both are in VLAN 30.

**Secure remote management (CR9)**
- SSH version 2 is enabled on HQ-ROUTER, CORE-SW and ACCESS-SW, with local user authentication.
- The virtual terminal lines accept SSH only, so Telnet is blocked.
- Management addresses in VLAN 99: HQ-ROUTER 10.18.99.1, CORE-SW 10.18.99.2, ACCESS-SW 10.18.99.3.
- Admin PC1 acts as the administrator's workstation, standing in for the off-site administrator. A real off-site administrator would reach VLAN 99 through a VPN or secure gateway, which is outside the scope of this brief.

## Milestone 2 - Testing Summary

Evidence is in `04-testing/` (screenshots M2-13 to M2-22) and `03-implementation/` (M2-01 to M2-12c).

| Test | Result |
|------|--------|
| SSH from Admin PC1 to router, CORE-SW and ACCESS-SW | Successful login to all three |
| Telnet to the router | Closed without a login prompt (SSH only) |
| Admin PC1 to Admin PC2 (same VLAN) | 4/4 replies, TTL 128 |
| Admin PC1 to Lab PC1 (different VLANs) | Replies, TTL 127 (routed) |
| Lab PC1 to Board PC (different VLANs) | Replies, TTL 127 (routed) |
| Board Laptop (wireless) to Board PC | 4/4 replies, TTL 128 |
| Board Laptop (wireless) to Admin PC1 | 4/4 replies, TTL 127 (routed) |
| tracert Admin PC1 to Lab PC1 | Hop 1 is the router (10.18.10.1), then Lab PC1 |

On routed pings, the first packet can time out while devices learn each other's addresses; the remaining packets succeed.

## Troubleshooting Notes

- **Spanning-tree warning during trunk setup**: after the trunk was configured on CORE-SW first, ACCESS-SW reported an inconsistent port type on Fa0/2. Configuring the matching trunk on ACCESS-SW resolved it.
- **SSH command typo**: the first SSH attempt used `ssh -1` (number one) instead of `ssh -l` (letter L) and returned "Invalid Command".
- **Failed SSH logins**: several attempts returned "Login invalid" before a login succeeded, which came down to password entry at the prompt.
- **Management address entry**: a mistyped `ip address` command on CORE-SW's VLAN 99 interface was rejected; re-entering it by hand fixed it.

## Status

This repository now reflects **Milestone 2: Client Implementation Review** (working Packet Tracer file, assigned feature implemented, testing evidence). The reflection will be added before final submission.
