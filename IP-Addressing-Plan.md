# IP Addressing Plan

Base Block: `192.168.12.0/24`

| Segment | Network | Usable range | Broadcast | Hosts |
|---|---|---|---|---|
| Staff LAN | 192.168.12.0/27 | .1–.30 | .31 | 30 |
| Clinical LAN | 192.168.12.32/27 | .33–.62 | .63 | 30 |
| Server segment (web server) | 192.168.12.64/28 | .65–.78 | .79 | 14 |
| Management VLAN (switches/router) | 192.168.12.80/28 | .81–.94 | .95 | 14 |
| Router-to-ISP link | 192.168.12.96/30 | .97–.98 | .99 | 2 |
| Reserved for CR6 branch office | 192.168.12.128/25 | — | — | 126 (unused, reserved) |

## Notes

- Web server address will be assigned from the server segment (e.g. `192.168.12.65`).
- The reserved `/25` block satisfies CR6 without requiring a second-site build — it documents where branch office addressing would live if the site is built in future.
- Router ACL enforces the design constraint: staff LAN is denied access to clinical/server segments but retains a default route to the internet.
