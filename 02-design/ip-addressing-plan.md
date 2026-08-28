# IP Addressing Plan 

## Assigned Addressing Block
**10.18.0.0/16** 

This entire /16 block was subdivided into four /24 subnets, one per VLAN and 
keeping all subnets clearly within the assigned range.

## Addressing Table

| VLAN ID | VLAN Name  | Subnet          | Usable Range              | Gateway       |
|---------|------------|-----------------|----------------------------|---------------|
| 10      | Admin      | 10.18.10.0/24   | 10.18.10.1 – 10.18.10.254  | 10.18.10.1    |
| 20      | Labs       | 10.18.20.0/24   | 10.18.20.1 – 10.18.20.254  | 10.18.20.1    |
| 30      | Boardroom  | 10.18.30.0/24   | 10.18.30.1 – 10.18.30.254  | 10.18.30.1    |
| 99      | Management | 10.18.99.0/24   | 10.18.99.1 – 10.18.99.254  | 10.18.99.1    |

## Why /24 subnets were chosen

Each VLAN was given a /24 rather than a smaller 
subnet. This comfortably covers the assumed device counts for each area 
with significant room for growth while keeping 
the addressing scheme simple, uniform and easy to explain and verify.

Given the /16 block provides 256 possible /24 subnets, using only 4 of 
them is not wasteful for a network of this scale and avoids the added 
complexity of variable length subnet masking.

## Why each VLAN exists

- **VLAN 10 (Admin):** Isolates staff traffic from student and guest traffic.
- **VLAN 20 (Labs):** Isolates student lab PCs, limiting their access to 
  administrative systems.
- **VLAN 30 (Boardroom):** Supports the brief's design constraint
  requiring dedicated wired and wireless presentation ports.
- **VLAN 99 (Management):** Dedicated management network for switches 
  and the router, supporting CR9's requirement for secure offsite 
  administrative access via SSH and kept separate from user traffic.

## Gateway addressing

The router holds the gateway IP for every VLAN, enabling inter VLAN routing 
via router-on-a-stick or SVIs configured on the router's subinterfaces.
