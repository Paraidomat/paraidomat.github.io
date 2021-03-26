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

## BGP MPLS-Based EVPN Overview

- An EVPN instance comprises CEs that are connected to PEs
- The PEs form the edge of the MPLS infrastructure.
- A CE may be a host, a router, or a switch.
- PEs provide virtual Layer 2 bridged connectivity between CEs.
- There may be multiple EVPN instances in the provider's network.

- The PEs may be connected by an MPLS LSP infrastructure.
  - provides benefits of MPLS technology like FRR, resiliency, etc.
- The PEs may also be connected by an IP infrastructure, in which case IP/GRE
  tunneling can be used between the PEs.

- In an EVPN, MAC learning between PEs occurs not in the data plane (as with
  VPLS) but in the control plane.
- Control plane learning offers greater control over the MAC learning process
  - Such as restricting who learns what and the ability to apply policies.
- Control plane chosen for advertising MAC reachability information is MP-BGP
- This provides flexibility and the ability to preserve the "virtualization" 
  or isolation of groups of interacting agents (host, servers, VMs) from each 
  other.
- In EVPN, PEs advertise the MAC addresses learned from the CEs that are 
  connected to them, along with an MPLS label via MP-BGP.
- Control Plane learning enables load balancing of traffic to and from CEs that
  are multihomed to multiple PEs.
- This is in addition to load balancing across the MPLS core via multiple LSPs
  between the same pair of PEs
- It allows CEs to connect to multiple active points of attachment.
  - It also improves convergence times in the event of certain network failures.
  
- Learning between PEs and CEs is done by the moethod best suited to the CE
  - data-plane learning, IEEE 802.1x, LLDP, IEEE 802.1aq ARP, management plane,
    and other.

- Wether the Layer 2 forwarding table on a PE is populated with all the MAC
  destination addresses known to the control plane, or wether the PE implements 
  a cache-based scheme, is a local decision.
- For instance: The MAC forwarding table may be populated only with the
  MAC destinations of the active flows transiting a specific PE.
- The policy attributes of EVPN are very similar to those of IP-VPN
- An EVPN instance requires a RD that is unique per MAC-VRF and one or more 
  globally unique RTs.
- A CE attaches to a MAC-VRF on a PE, on an Ethernet interface that may be 
  configured for one or more Ethernet tags, e.g., VLAN IDs.
- Some deployment scenarios guarantee uniqueness of VLAN IDs accross EVPN instance
  - All points of attachment for a given EVPN instance use the same VLAN ID, 
    and no other EVPN instance uses this VLAN ID.
    
## Slide Deck

https://www.cisco.com/c/dam/m/en_us/network-intelligence/service-provider/digital-transformation/knowledge-network-webinars/pdfs/0420-epn-ckn.pdf

## Why EVPN

- VPLS and PBB (Provider Backbone Briding) rely on flooding and learning to build L2 forwarding database
- Network operators needs:
  - Data Center Interconnect (DCI)
  - Cloud and Services virtualization
  - Reduce protocol stack 
  - Simplify network
  - Integrated L2 and L3 Services over the same VPN

### VPLS challenges for per-flow redundancy

- Existing VPLS solutions do not offer an All-Active per-flow redundancy
- Looping of Traffic Flooded from PE
- Duplicate Frames from Floods from the Core
- MAC Flip-Flopping over Pseudowire
  - e.g. Port-Channel Load-Balancing does not produce a consistent hash-value for a frame with the same source MAC (non MAC based hash-schemes)

### Data Center Interconnect requirements

> not fully addressed by current L2VPN technologies

- Per-flow redundancy and load balancing
- simplified provisioning and operation
- optimal forwarding
- fast convergance
- MAC Address Scalability

**EVPN** with a choice of data plane encapsulation (MPLS, VxLAN, PBB) is the designed to address these requirements.

## EVPN

### EVPN Unifying control plane

- Control Plane: 
  - EVPN MP-BGP
- Data Plane:
  - MPLS (RFC-7432)
    - EVPN over MPLS (E-LAN service)
    - All-active Multi-homing
    - SR/SR-TE as underlay
    - EVPN-VPWS (E-Line service)
  - NVP evpn-overlay
    - //omitted
  - PBB (RFC-7623)
    - PBB Encapsulation for CMAC scalability
    - MPLS as underlay
    - All-active Multi-homing
    - BMACs advertisements  
  - VPWS (evpn-vpws)
    - P2P services
    - MPLS as underlay
    - All-active Multi-homing
    - Flexible xconnect services

... continue on slide 9
