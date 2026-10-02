# Addressing Plan – 192.168.12.0/24 (VLSM)

| Segment | VLAN | Subnet | Mask | Gateway | Usable range | DHCP / static |
|---|---|---|---|---|---|---|
| Staff | 10 | 192.168.12.0/26 | 255.255.255.192 | 192.168.12.1 | .1 – .62 | DHCP .10 – .62 |
| Management | 20 | 192.168.12.64/27 | 255.255.255.224 | 192.168.12.65 | .65 – .94 | DHCP .70 – .94 |
| Servers | 30 | 192.168.12.96/28 | 255.255.255.240 | 192.168.12.97 | .97 – .110 | Static (web server .98) |
| **CR6 Branch (reserved)** | – | 192.168.12.128/26 | 255.255.255.192 | – | .129 – .190 | Design only, not built |
| Free / growth | – | 192.168.12.112 – .127, .192 – .255 | – | – | – | Unallocated |

## Point-to-point and external (outside the hospital block)
| Link | Subnet | R1 / ISP |
|---|---|---|
| R1 G0/1 <-> ISP G0/0 | 203.0.113.0/30 | R1 .2 / ISP .1 |
| ISP G0/1 (test "internet") | 198.51.100.0/24 | ISP .1, external server .10 |

## Device ports
| Device | Port | Connects to | Config |
|---|---|---|---|
| SW1 | Fa0/1 – 8 | Staff PCs | access VLAN 10 |
| SW1 | Fa0/9 – 12 | Management PCs | access VLAN 20 |
| SW1 | Fa0/13 | Web server | access VLAN 30 |
| SW1 | Gig0/1 | R1 G0/0 | trunk |
| R1 | G0/0.10 / .20 / .30 | SW1 | 802.1Q sub-interfaces (gateways) |
| R1 | G0/1 | ISP G0/0 | NAT outside |

## Security / design decisions
- Staff ACL permits only DHCP, ICMP/HTTP to the web server and ping to their own gateway; everything else is denied (logs hits).
- NAT is only applied to the Management subnet, so Staff cannot reach the internet even if the ACL is removed.
- Management has unrestricted internet access (requirement: management online even when staff network is restricted).
- CR6: the /26 above is held back so a branch can be added later without renumbering.

## Core services
| Service | Host | Notes |
|---|---|---|
| HTTP (assigned feature) | 192.168.12.98 : TCP 80 | Internal hospital portal |
| DNS | 192.168.12.98 : UDP 53 | `portal.lesedihospital.co.za`; issued to clients via DHCP |
| DHCP | R1-Lesedi | Pools STAFF and MANAGEMENT |
| Routing | R1-Lesedi | Inter-VLAN (router-on-a-stick) + default route to ISP |
| Internet | NAT overload on R1 G0/1 | Management only |
