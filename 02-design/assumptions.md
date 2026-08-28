# Design Assumptions

The project brief does not specify exact departments, device counts or room layout for Thuto Skills Training College.The following reasonable assumptions were made for a small training college.

## Assumed departments

- **Administration Office** – staff handling registration, finance and admin tasks. Requires a stable wired network with access to student records and justifies its own VLAN for isolation.
- **Student Computer Lab** – shared PCs used by students. Kept separate from Admin so student traffic can't affect staff systems.
- **Boardroom** – used for meetings and presentations. Explicitly required by the brief to have both wired and wireless presentation ports , so it gets its own VLAN and an access point.
- **IT/Server Room** – houses the core switch, router and any local server. Natural location for the network's core equipment.

## Assumed device counts (per VLAN for addressing purposes)

- Admin: ~20 staff PCs
- Labs: ~40 student PCs 
- Boardroom: ~5 wired/wireless devices
- Management: switches, router and AP

A /24 subnet per VLAN comfortably covers these counts with room to grow and keeps the addressing plan simple to explain and verify.

## Assumed remote administration method (CR9)

CR9 requires "secure remote management access" but doesn't specify a protocol. SSH was chosen over Telnet because Telnet transmits credentials in plain text, which does not satisfy "secure." A dedicated Management VLAN (VLAN 99) was created so device management interfaces are logically separated from user traffic, following standard network security practice.

## Justification for VLAN trunking design

Two switches were used specifically to give a genuine demonstrable use case for 802.1Q trunking between switches as required by the assigned networking challenge.
