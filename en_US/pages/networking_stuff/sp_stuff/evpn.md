# EVPN

## RFC

* [Requirements for Ethernet VPN (EVPN)](https://tools.ietf.org/html/rfc7209)
* [BGP MPLS-Based Ethernet VPN](https://tools.ietf.org/html/rfc7432)
* [A Modern, Open and Scalable Fabric with VXLAN and EVPN](https://www.cisco.com/c/dam/en/us/td/docs/switches/datacenter/nexus9000/sw/vxlan_evpn/VXLAN_EVPN.pdf)
* [EVPN Deep Dive and Troubleshooting with IOS-XR](https://www.ciscolive.com/c/dam/r/ciscolive/us/docs/2019/pdf/BRKSPG-3965.pdf)
  - Configuration examples for Service Provider Metro and Data Center
* [IOS-XR EVPN Hands On Lab](https://www.ciscolive.com/c/dam/r/ciscolive/emea/docs/2019/pdf/LTRSPG-2968.pdf)

## Troubleshooting

1. Don't fix what's not broken
2. Verify End-To-End LSP: 
   - `show route`, `show cef`, `show mpls forwarding`, etc.
3. BGP Control Plane
  - `show bgp l2vpn evpn summary`
4. EVPN Control Plane - L2
  - Ethernet Setment Verification
    - `show evpn ethernet-segment [detail]`
  - Received and Advertised MAC addresses
    - `show evpn evi mac`
    - `show l2route evpn mac all`
  - EVI Instance Summary
    - `show evpn evi vpn-id [evi id] detail`
5. EVPN Data Plane - L2
   - Nexthop Verification - Unicast
     - `show evpn evi mac`
     - `show l2route evpn mac all`
     - `show evpn internal-label`
   - Nexthop Verification - Broadcast/Multicast
     - `show evpn evi inclusive-multicast detail`
   - Bridge-Domain Status
     - `show l3vpn bridge-domain bd-name [bd name]
   - Bridge-Domain Local/Remote MAC addresses
     - `show l3vpn forwarding bridge-domain [bd group:bd name] mac-address detail location [LC location]`
6. EVPN Control Plane - L3
  - Received and Advertised Host Routes
    - `show evpn evi mac`
    - `show l2route evpn mac-ip all`
  - Routes in VRF
    - `show route vrf`
7. EVPN Data Plane - L3
   - Nexthop Verification
     - `show route vrf [vrf name]`
     - `show cef vrf [vrf name]`
