# Topology

## Logical topology

- Router connects to the internet (ISP link) and to a core switch.
- Core switch trunks to three VLANs/segments: Staff LAN, Clinical LAN, Server segment.
- Web server sits in the Server segment.
- Router ACL denies Staff LAN → Clinical LAN and Staff LAN → Server segment, while permitting all segments a default route to the internet.

## Physical topology (Packet Tracer)

- 1 router (sub-interfaces per VLAN, router-on-a-stick)
- 1–2 VLAN-capable switches
- 1 web server (HTTP service) in the server segment
- 2–3 PCs per VLAN for connectivity testing
- Cloud/internet connection

See [`../diagrams/topology.png`](../diagrams/topology.png) for the diagram.

## Testing plan

- Staff PC → internet: should succeed
- Staff PC → Clinical LAN / Server segment: should be blocked (ACL)
- Clinical/Server segment → internet and each other: should succeed
- HTTP request from a PC to the web server: should return the hosted page
