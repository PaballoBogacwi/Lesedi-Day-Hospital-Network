# Troubleshooting Log – Lesedi Day Hospital Network

This log has two parts: a **diagnostic playbook** written for this specific design (what breaks in a VLAN + ACL + NAT + DNS build, and how to prove it), and a **build log** of the problems I actually hit.

## Part 1 – Diagnostic playbook (design-specific)

|Symptom|Most likely cause in this design|How I check|Fix|
|-|-|-|-|
|PC gets `169.254.x.x`|Trunk down, G0/0 shut, or the VLAN missing on SW1|`show interfaces trunk` on SW1; `show ip interface brief` on R1|`no shutdown` on G0/0; recreate VLAN; set Gig0/1 to trunk|
|Staff PC gets no DHCP but Management does|`STAFF-IN` blocking DHCP discover (0.0.0.0 -> 255.255.255.255)|`show access-lists` – check the `bootpc/bootps` line exists first|Add `permit udp any eq bootpc any eq bootps` above the deny|
|Staff can browse by IP but not by name|DNS (UDP 53) not permitted to the server|`nslookup` fails; ACL has no line with `eq 53`|Add `permit udp ... host 192.168.12.98 eq 53`|
|Staff can reach the internet (should be blocked)|ACL applied to the wrong interface/direction, or missing `in`|`show ip interface g0/0.10` -> "Inbound access list"|`ip access-group STAFF-IN in` on G0/0.10|
|Management has no internet|NAT inside/outside swapped, or ACL 1 doesn't match the Mgmt subnet|`show ip nat translations`, `show running-config \| include nat`|`ip nat inside` on G0/0.20, `ip nat outside` on G0/1, `access-list 1 permit 192.168.12.64 0.0.0.31`|
|Mgmt can ping the ISP but not the external server|ISP G0/1 not up or the server's gateway is wrong|`show ip interface brief` on ISP|Set server gateway to 198.51.100.1|
|Web page won't load but ping works|HTTP service is Off, or the wrong server mask/gateway|Server > Services > HTTP; check `/28` mask `255.255.255.240`|Turn HTTP on; fix mask/gateway|
|Sub-interface up/up but no inter-VLAN traffic|Wrong `encapsulation dot1Q` ID vs VLAN|`show vlan brief`; compare with `show run`|Match IDs 10/20/30|



## Part 2 – Build log (what actually happened)

|#|Date/time|What I was doing|What went wrong (exact symptom/message)|How I diagnosed it|Fix|Screenshot|
|-|-|-|-|-|-|-|
|1|2 Oct 2026|Pasting the switch configuration into the CLI|The first line, `enable`, was dropped, so the prompt remained `Switch>`. The following commands returned `% Invalid input`.|I checked the prompt and noticed that the switch was still in user EXEC mode instead of privileged EXEC mode.|I typed `enable` manually, confirmed the prompt changed to `Switch#`, and pasted the remaining configuration again.|Not captured|
|2|2 Oct 2026|Configuring Staff1 to obtain an IP address|Staff1 displayed an invalid IP address and did not receive a usable address.|I checked the PC's IP Configuration settings and saw that it was not set to obtain its address automatically.|I changed the setting to DHCP. Staff1 then obtained an IP address from the network.|Not captured|
|3|2 Oct 2026|Checking the router access list with `show access-lists`|The command returned `% Invalid input` at the `>` prompt. When I tried `enable`, the password was rejected.|I checked the router configuration and saw that the console password (`cisco`) and the privileged-mode enable secret (`Lesedi@2026`) were different.|I used the configured enable secret, `Lesedi@2026`, to enter privileged EXEC mode. Once the prompt changed to `Router#`, I could run `show access-lists`.|Not captured|



I learned to check the device prompt and its IP settings before assuming that the whole configuration was wrong. Small issues such as a missed CLI command or an incorrect DHCP setting can stop a device from working, so I should verify each step and keep evidence of the fix.



