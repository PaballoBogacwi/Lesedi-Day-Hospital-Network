# Reflections – CMPG 325 Individual Project

## Milestone 1 – Design (what drove my decisions)

A hospital network is different from an office network because a mistake can expose patient information or take a service offline when staff need it. That is why I did not use one flat subnet. Splitting the 192.168.12.0/24 block into Staff, Management and Servers VLANs let me control exactly who can talk to the web server, and sizing the subnets with VLSM (/26, /27, /28) left room to grow. I deliberately held back 192.168.12.128/26 for the CR6 branch office so the addressing would not need redoing later.

The client requirement that interested me most was that management must keep internet access even when the staff network is restricted. It forced me to treat "restricted" as two things: who may leave the network (NAT) and who may enter the server segment (ACL). I used both controls so the restriction does not depend on one rule.


## Milestone 2 – Implementation

Building the web server feature showed me that the service itself is the easy part; reaching it correctly is the hard part. Clients needed DHCP, a gateway, DNS and an ACL that let exactly the right traffic through. Because the staff ACL is applied inbound on the sub-interface, even DHCP and DNS had to be explicitly permitted.



## What I would improve

* Use HTTPS instead of HTTP for the portal, since hospital data should be encrypted in transit.
* Separate DNS from the web server and add a second switch/router link for redundancy.
* Replace the single router-on-a-stick trunk with a Layer 3 switch for better performance as the branch is added.
* Add logging/monitoring for the ACL deny hits so unusual staff activity is noticed.

