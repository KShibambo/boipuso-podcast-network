OSPF Single-Area Configuration

Requirement

Configure, verify and demonstrate Basic OSPF (single-area dynamic routing).

Why OSPF instead of static routing

The network spans two physical sites with multiple VLANs at each. Static routing would require manual entries on every router for every subnet, and any topology change would need manual reconfiguration. OSPF gives us automatic route learning, automatic convergence if a link fails, and verifiable neighbour adjacencies.

Configuration on R1-HQ

router ospf 1
 router-id 1.1.1.1
 network 10.51.10.0 0.0.0.255 area 0
 network 10.51.30.0 0.0.0.255 area 0
 network 10.51.40.0 0.0.0.255 area 0
 network 10.51.50.0 0.0.0.255 area 0
 network 10.51.200.0 0.0.0.3 area 0
 passive-interface g0/0.10
 passive-interface g0/0.30
 passive-interface g0/0.40
 passive-interface g0/0.50

Configuration on R2-STUDIO

router ospf 1
 router-id 2.2.2.2
 network 10.51.20.0 0.0.0.255 area 0
 network 10.51.60.0 0.0.0.255 area 0
 network 10.51.200.0 0.0.0.3 area 0
 passive-interface g0/0.20
 passive-interface g0/0.60

Verification

Neighbour adjacency. show ip ospf neighbor on R1-HQ shows 2.2.2.2 in FULL state. Same command on R2-STUDIO shows 1.1.1.1 in FULL state. See screenshots 06 and 08.

Routing table. show ip route ospf on R1-HQ shows Studio subnets (10.51.20.0/24 and 10.51.60.0/24) via 10.51.200.2. On R2-STUDIO it shows all HQ subnets via 10.51.200.1. See screenshots 07 and 09.

End-to-end. Admin PC (VLAN 10) successfully pings Production PC (10.51.20.10) across the WAN link. TTL of 126 confirms two routers were crossed. See screenshot 11.
