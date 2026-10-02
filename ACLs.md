ACL Configuration

CR4 - Server access restriction

Requirement

A new application/file server must be reachable by authorised departments only.

Interpretation

Only Admin (VLAN 10) and IT (VLAN 40) may reach the server at 10.51.40.10.

Configuration on R1-HQ

ip access-list extended CR4-SERVER-ACCESS
 permit ip 10.51.10.0 0.0.0.255 10.51.40.0 0.0.0.255
 permit ip 10.51.40.0 0.0.0.255 10.51.40.0 0.0.0.255
 deny   ip any 10.51.40.0 0.0.0.255
 permit ip any any

interface g0/0.40
 ip access-group CR4-SERVER-ACCESS in

Verification

Admin PC to 10.51.40.10 - Reply (allowed)
Editing PC to 10.51.40.10 - Request timed out (CR4 denies VLAN 30)
Production PC to 10.51.40.10 - Request timed out (CR4 denies VLAN 20)
Guest Laptop to 10.51.40.10 - Request timed out (NAT plus CR4)
See screenshots 13 and 17.

Legacy isolation ACL

Requirement

Legacy devices without modern security features must still be accommodated safely.

Configuration on R1-HQ

ip access-list extended LEGACY-BLOCK
 deny   ip 10.51.60.0 0.0.0.255 10.51.40.0 0.0.0.255
 deny   ip 10.51.60.0 0.0.0.255 10.51.10.0 0.0.0.255
 permit ip any any

interface g0/0.10
 ip access-group LEGACY-BLOCK in

Verification

Legacy PC to 10.51.40.10 (server) - Request timed out. See screenshot 14.
Legacy PC to 10.51.10.10 (admin) - Request timed out. See screenshot 15.
Legacy PC to 10.51.20.10 (production) - Reply (allowed, only server and admin are blocked).

Layer 2 isolation

Port security on SW-LEGACY F0/2 restricts the port to one MAC address, sticky learned, shutdown on violation. See screenshot 05.

show access-lists on R1-HQ confirms both ACLs are installed. See screenshot 18.
