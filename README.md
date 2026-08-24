# Thuto Skills Training College — Network Design (CMPG325-2026-023)

## Project Overview

This repository documents the design, implementation, and testing of a computer network for **Thuto Skills Training College (Klerksdorp)**, completed as an individual project for CMPG325 (Computer Networks).

- **Client ID:** CLI-023
- **Assigned addressing block:** 10.18.0.0/16
- **Assigned networking challenge:** VLAN Trunking (802.1Q across switches)
- **Client change request:** CR9 — secure off-site administrator access

## Repository Structure

```
01-requirements.md        Client requirements, scope, and constraints
02-design/
  ip-addressing-plan.md   VLAN and subnet addressing scheme
  physical-topology.png   Physical device and cabling diagram
  logical-topology.jpg    VLAN/IP logical view of the network
  assumptions.md          Design assumptions made where the brief is silent
03-implementation/        Packet Tracer configuration evidence (Phase 2)
04-testing/               Connectivity and feature verification evidence
05-reflection/            Project reflection
```

## Design Summary

The network uses a router connected to two switches joined by an 802.1Q trunk link, supporting four VLANs:

| VLAN | Purpose    | Subnet         |
|------|------------|----------------|
| 10   | Admin      | 10.18.10.0/24  |
| 20   | Labs       | 10.18.20.0/24  |
| 30   | Boardroom  | 10.18.30.0/24  |
| 99   | Management | 10.18.99.0/24  |

The boardroom is equipped with both wired and wireless presentation access (per the client's design constraint), and a dedicated Management VLAN supports CR9's requirement for secure off-site SSH access to network devices.

## Status

This repository currently reflects **Milestone 1: Client Design Review** (client requirements, physical topology, logical topology, IP addressing plan). Implementation, testing, and reflection evidence will be added as the project progresses toward final submission.
