# Client Requirements \& Design Justification (Rubric 1)

**Client:** Lesedi Day Hospital, Vryburg (Healthcare) | **Block:** 192.168.12.0/24 | **Challenge:** HTTP/Web Server (Foundational)

## Requirements

|ID|Requirement|Source|
|-|-|-|
|R1|Host an internal web service (HTTP) for hospital users|Assigned challenge|
|R2|Management must have internet access even when the staff network is restricted|Client brief|
|R3|Staff network is restricted (no general internet) but can use the internal web service|Client brief / design|
|R4|Operate within 192.168.12.0/24|Client brief|
|R5|Accommodate a future branch office (CR6) in the design/addressing – no second site build|Change request CR6|
|R6|Protect hospital systems: separate staff, management and servers|Design decision (healthcare data sensitivity)|

## Design decisions and justification

|Decision|Why|
|-|-|
|3 VLANs (Staff 10, Management 20, Servers 30)|Limits broadcast domains and lets ACLs control who reaches what (R3, R6)|
|VLSM: /26, /27, /28|Right-sizes each segment, leaving space for growth and CR6 (R4, R5)|
|192.168.12.128/26 reserved for branch|Branch can be added without renumbering (R5)|
|Web server in its own VLAN with a static IP|Stable address for DNS/ACL rules; isolates the server from user PCs (R1, R6)|
|Router-on-a-stick for inter-VLAN routing|Foundational-level design, one trunk, easy to explain and verify|
|Extended ACL `STAFF-IN` on the Staff gateway|Staff can only reach DHCP, DNS and HTTP on the web server (R3)|
|NAT overload for Management only|Management keeps internet access (R2); Staff blocked even if the ACL were removed|
|DNS name for the portal|Users browse by name instead of an IP; part of core services|
|DHCP on R1|Central addressing for user VLANs, fewer manual errors|



