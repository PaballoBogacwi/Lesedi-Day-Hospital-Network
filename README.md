# Lesedi Day Hospital — Network Design (CMPG 325)

Individual semester project for CMPG 325 — Computer Networks
North-West University (NWU), Mahikeng Campus
Department of Computer Science and Information Systems

**Project ID:** CMPG325-2026-004
**Client ID:** CLI-004
**Client:** Lesedi Day Hospital (Vryburg) — Healthcare
**Student:** Paballo Bogacwi (52442179)
**Assigned Challenge:** HTTP/Web Server Hosting
**IP Address Block:** `192.168.12.0/24`

## Overview

This repository documents the design, implementation, and testing of a Cisco Packet Tracer network developed for Lesedi Day Hospital. The project addresses the hospital's networking requirements and the assigned challenge of internal HTTP/Web Server hosting within the allocated address block `192.168.12.0/24`.

The network incorporates VLAN segmentation, inter-VLAN routing, DHCP, DNS, NAT, access control lists (ACLs), and an internal web server.

## Project Status

| Milestone                                  | Date            | Status                  |
| ------------------------------------------ | --------------- | ----------------------- |
| Milestone 1 — Client Design Review         | 28 August 2026  | Completed and Submitted |
| Milestone 2 — Client Implementation Review | 3 October 2026  | Completed and Submitted |
| Final Submission                           | 16 October 2026 | Pending                 |

### Milestone Checklist

* [x] Milestone 1 — Client Design Review
* [x] Milestone 2 — Client Implementation Review
* [ ] Final Submission — Portfolio, report, Packet Tracer file and demonstration video

## Network Implementation

The network design and implementation include:

* **VLANs:** Staff (VLAN 10), Management (VLAN 20), and Servers (VLAN 30).
* **Inter-VLAN Routing:** Router-on-a-stick configuration.
* **HTTP/Web Server:** Internal web service hosted at `192.168.12.98` with a custom portal page.
* **DHCP:** Dynamic IP address allocation for Staff and Management devices.
* **DNS:** Name resolution for `portal.lesedihospital.co.za`.
* **NAT Overload:** Provides internet access to the Management network.
* **Access Control:** ACL `STAFF-IN` restricts Staff access to the internet and Management network while permitting access to authorised internal services.
* **Future Expansion:** The CR6 branch network, `192.168.12.128/26`, is reserved for future implementation.

## Project Files

- [Client Requirements](Client-Requirements.md)
- [IP Addressing Plan](IP-Addressing-Plan.md)
- [Network Topology](Topology.md)
- [HTTP Web Server](feature-http-web-server.md)
- [Reflections](reflections.md)
- [Troubleshooting Log](troubleshooting-log.md)
- [Technical Report Outline](technical-report-outline.md)

## Packet Tracer

- [Packet Tracer Project – Lesedi CMPG325](Lesedi_CMPG325-2026-004_M2.pkt)

## Testing Evidence

- [Testing Evidence (Screenshots & Proof)](testing-evidence.zip)
  
## Testing

The network testing process covers the following:

* DHCP address allocation for Staff and Management PCs.
* VLAN configuration and trunk connectivity.
* Inter-VLAN routing.
* DNS resolution for the internal web portal.
* HTTP access to the internal web server.
* Management internet access through NAT.
* Staff access restrictions through ACLs.
* Connectivity between network devices and services.


## How to Run

1. Open the Cisco Packet Tracer `.pkt` file in the `packet-tracer/` directory.
2. Allow the network topology and devices to load.
3. Ensure that PCs are configured to obtain IP addresses through DHCP and that servers use the static IP addresses documented in `configs/`.
4. Verify the router and switch configurations using the files in `configs/`.
5. From a Staff PC, browse to `http://192.168.12.98` to test access to the internal web server.
6. From a Management PC, browse to `http://198.51.100.10` to test external server access, where configured.
7. Follow the test plan to reproduce and document the network test results.

## Troubleshooting

The `troubleshooting-log.md` file documents configuration problems encountered during implementation, their symptoms, diagnostic steps, and resolutions.

It also provides guidance for diagnosing common networking issues involving DHCP, VLANs, ACLs, NAT, DNS, and HTTP services.

## Reflections

The `reflections.md` file contains reflections on:

* **Milestone 1:** Network design decisions, requirements, topology and IP addressing.
* **Milestone 2:** Implementation challenges, troubleshooting and configuration.



**Final Submission Date:** 16 October 2026

---

**CMPG 325 — Computer Networks**
**North-West University**
**Student: Paballo Bogacwi (52442179)**

