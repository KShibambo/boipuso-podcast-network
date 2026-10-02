Testing Evidence

Connectivity matrix

Source            Destination        Expected    Actual
Admin PC          10.51.10.1         Pass        Pass
Admin PC          10.51.20.10        Pass        Pass (OSPF)
Admin PC          10.51.30.10        Pass        Pass
Admin PC          10.51.40.10        Pass        Pass
Admin PC          10.51.100.2        Pass        Pass
Editing PC        10.51.40.10        Fail        Fail (CR4)
Production PC     10.51.40.10        Fail        Fail (CR4)
Legacy PC         10.51.40.10        Fail        Fail (Legacy ACL)
Legacy PC         10.51.10.10        Fail        Fail (Legacy ACL)
Legacy PC         10.51.20.10        Pass        Pass
Guest Laptop      192.168.50.1       Pass        Pass
Guest Laptop      10.51.50.1         Pass        Pass
Guest Laptop      10.51.100.2        Pass        Pass
Guest Laptop      10.51.40.10        Fail        Fail (NAT + CR4)

Troubleshooting log

Issue 1 - SW-ADMIN uplink flapping
Admin PC could not reach its gateway. SW-ADMIN F0/1 flapped up and down. Cause: wrong cable type between SW-ADMIN F0/1 and SW-CORE-HQ. Fix: replaced the cable.

Issue 2 - SW-EDIT and SW-PROD cables swapped
End devices could not reach their gateways. Uplink and PC cables were physically reversed. Fix: swapped the cables so trunk is on F0/1 and PC on F0/2.

Issue 3 - SW-STUDIO not configured
R2-STUDIO G0/0 was not passing traffic to Studio switches. SW-STUDIO had never been configured. Fix: configured VLANs 20 and 60, and trunks on F0/1, F0/2, and G0/1.

Issue 4 - WRT300N cable in wrong port
Guest Laptop could reach WRT LAN but not R1 VLAN 50 gateway. Cause: cable was in a LAN port instead of the Internet port. Fix: moved cable to Internet port and to SW-CORE-HQ F0/3 (access VLAN 50).

Issue 5 - ACLs missing
Editing and Production PCs could reach server. Fix: applied CR4-SERVER-ACCESS and LEGACY-BLOCK on R1-HQ.

Issue 6 - Guest Laptop stale DHCP lease
Laptop had 192.168.0.100 after LAN subnet change. Fix: ran ipconfig /release and /renew. Laptop received 192.168.50.100.
