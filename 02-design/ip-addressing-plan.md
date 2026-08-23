# IP Addressing Plan — CMPG325-2026-023

## Assigned Addressing Block
**10.18.0.0/16** (as specified in the client brief, Section 6)

This entire /16 block was subdivided into four /24 subnets, one per VLAN, 
keeping all subnets clearly within the assigned range.

## Addressing Table

| VLAN ID | VLAN Name  | Subnet          | Usable Range              | Gateway       |
|---------|------------|-----------------|----------------------------|---------------|
| 10      | Admin      | 10.18.10.0/24   | 10.18.10.1 – 10.18.10.254  | 10.18.10.1    |
| 20      | Labs       | 10.18.20.0/24   | 10.18.20.1 – 10.18.20.254  | 10.18.20.1    |
| 30      | Boardroom  | 10.18.30.0/24   | 10.18.30.1 – 10.18.30.254  | 10.18.30.1    |
| 99      | Management | 10.18.99.0/24   | 10.18.99.1 – 10.18.99.254  | 10.18.99.1    |

## Why /24 subnets were chosen

Each VLAN was given a /24 (254 usable addresses) rather than a smaller 
subnet. This comfortably covers the assumed device counts for each area 
(see `assumptions.md`) with significant room for growth, while keeping 
the addressing scheme simple, uniform, and easy to explain and verify — 
important given the assigned VLAN trunking challenge already adds 
complexity to the network.

Given the /16 block provides 256 possible /24 subnets, using only 4 of 
them is not wasteful for a network of this scale, and avoids the added 
complexity of variable-length subnet masking (VLSM), which is not 
required by the brief.

## Why each VLAN exists

- **VLAN 10 (Admin):** Isolates staff traffic (registration, finance, 
  student records) from student and guest traffic.
- **VLAN 20 (Labs):** Isolates student lab PCs, limiting their access to 
  administrative systems.
- **VLAN 30 (Boardroom):** Supports the brief's design constraint 
  (Section 8) requiring dedicated wired and wireless presentation ports.
- **VLAN 99 (Management):** Dedicated management network for switches 
  and the router, supporting CR9's requirement for secure off-site 
  administrative access via SSH, kept separate from user traffic.

## Gateway addressing

The router (HQ-Router) holds the gateway IP for every VLAN (the first 
usable address in each subnet, x.x.x.1), enabling inter-VLAN routing 
via router-on-a-stick or SVIs configured on the router's subinterfaces.
