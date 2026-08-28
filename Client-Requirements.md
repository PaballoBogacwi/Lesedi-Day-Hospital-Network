# Client Requirements



## Client Background

**Organisation:** Lesedi Day Hospital (Vryburg)
**Industry:** Healthcare
**Client ID:** CLI-004



## Requirements

* Assigned addressing block: `192.168.12.0/24`
* Provide appropriate connectivity and network services for the assigned scenario
* Accommodate the stated design constraint and change request
* Produce a working, testable Packet Tracer implementation



## Assigned Networking Challenge

**HTTP/Web Server (internal web service hosting)** — Foundational difficulty

Configure, verify, and demonstrate an HTTP/web server within the hospital's network, hosting an internal web service.



## Design Constraint

Management requires internet access even when the staff network is restricted. This is addressed by applying an access control list (ACL) at the router that restricts the staff LAN from reaching internal segments (clinical LAN, server segment), while still permitting a default route out to the internet for all segments.





## Change Request — CR6

A branch office is planned. The requirement is design/addressing accommodation only — no second-site build is required. This is addressed by reserving `192.168.12.128/25` for future branch office use within the addressing plan (see [IP Addressing Plan](IP-Addressing-Plan.md) ).

