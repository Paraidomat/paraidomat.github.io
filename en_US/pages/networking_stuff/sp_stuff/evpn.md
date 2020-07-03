# EVPN

## RFC

* [Requirements for Ethernet VPN (EVPN)](https://tools.ietf.org/html/rfc7209)
* [BGP MPLS-Based Ethernet VPN](https://tools.ietf.org/html/rfc7432)
* [A Modern, Open and Scalable Fabric with VXLAN and EVPN](https://www.cisco.com/c/dam/en/us/td/docs/switches/datacenter/nexus9000/sw/vxlan_evpn/VXLAN_EVPN.pdf)

## Advantages in Summary

- Standards-based Overlay (VXLAN) with standards-based control plane (BGP)
- Layer 2 MAC and Layer 3 IP information distribution by control plane (BGP)
- Forwarding decision based on scalable control plane (minimizes flooding)
- Integrated Routing/Bridging (IRB) for optimized forwarding in the Overlay
- Leverages Layer 3 ECMP - all links forwarding - in the underlay
- Significantly larger namespace in the overlay (16M segments)
- Integration of physical and virtual networks with hybrid overlays
- Facilitation of SDN
