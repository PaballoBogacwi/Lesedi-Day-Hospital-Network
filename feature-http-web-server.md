# Assigned Feature: HTTP / Web Server (Rubric 3)

## Goal
Host an internal web portal that hospital staff and management can reach, while keeping it protected from the rest of the network.

## How it works
1. Client PC gets IP, gateway and DNS (192.168.12.98) from R1 via DHCP.
2. User browses to `portal.lesedihospital.co.za` -> DNS query (UDP 53) to the server -> returns 192.168.12.98.
3. Browser opens HTTP (TCP 80) to 192.168.12.98. Traffic crosses VLANs via R1's sub-interfaces.
4. Staff traffic is checked by `STAFF-IN` on G0/0.10: only DHCP, DNS, HTTP to the server and ping to the server/gateway are allowed; everything else is denied and logged.
5. Management traffic is not filtered, so it also reaches the internet through NAT.

## Implementation
- Server: 192.168.12.98/28 static, gateway 192.168.12.97, Services > HTTP On with custom `index.html`, Services > DNS On with A records.
- Segmentation: Servers VLAN 30 on SW1 Fa0/13.
- Access control: `STAFF-IN` (see `configs/R1-Lesedi.txt`).

## Verification (see `testing-evidence/test-plan.md`)
| Check | Tests |
|---|---|
| Server reachable by Staff and Management | T4, T5, T17 |
| DNS works | T16, T17, T18 |
| Restriction works (Staff blocked from internet and from Management) | T7, T8, T9 |
| Management internet unaffected | T6, T13, T18 |
| Config evidence | T10 – T15 |

## Why this is optimised
- Narrow ACL (only needed ports) instead of "permit all to server".
- Server isolated in its own VLAN with a static IP so ACL/DNS never break from DHCP changes.
- NAT limited to the Management subnet, so the restriction does not rely on one control only.
