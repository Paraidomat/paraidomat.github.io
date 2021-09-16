# EVPN Course

## EVPN Fundamentals

### Examining Next-Generation Solutions for L2VPNs

#### L2VPN Technologies Envolution

##### LAN

1. Ethernet IEEE 802.3
2. VLANS IEEE802.1Q
3. QinQ IEEE802.1ad
4. MACinMAC 802.1ah
5. SPB 802.1aq
6. ECMP 302.1Qbp

##### WAN

1. VPLS
2. H-VPLS (PW Scale)
3. PBB-VPLS (MAC Scale)
4. EVPN (RFC 7432)
   - PBB-EVPN
   - EVPN-VxLAX
   - EVPN-IRB
   - EVPN-VPWS

###### VPLS Challenges

- no all-active per-flow redundancy
- looping traffic flooded from PE
- Duplicate frames from floods from the core
- MAC flip-flopping over PW (caused e.g. by Port-Channels)

### Introducing EVPN Basics

- BGP Control Plane
- Same principles as L3VPNs 
- No use of PWs

#### Benefits

- Integrated L2 and L3 services
- Efficiency (no flood and learn)
- flexible Multihoming (n-way redundancy)
- Control Plane and Data Plane Separation
  - Control Plane: EVPN (MP-BGP)
  - Data Plane:
    - MPLS (RFC7432)
    - PBB-MPLS (RFC7623)
    - Network Virtualization Overlay (NVO)
      (VXLAN, NVGRE, MPLSoGRE)
      draft-ietf-bess-evpn-overlay

### Examining EVPN Terminology and Concepts

#### EVPN Concepts

- EVI (EVPN Instance) equivalent to VPNs / VRFs.
- Ethernet Segments 
  - Single Homed Devices
  - Multi Homed Devices
    - Redundancy / Load balancing
- BGP Routes
  | Number | Route Type |
  | ------ | ---------- |
  |      1 | Ethernet Auto-Discovery (AD) Route) |
  |      2 | MAC Advertisement Route |
  |      3 | Inclusive Multicas Route |
  |      4 | Ethernet Segment Route |
- BGP Route Attributes
  | Extended Communities |
  | -------------------- |
  | ESI MPLS Label       |
  | ES-Import (like RT)  |
  | MAC-Mobility         |
  | Default Gateway      |

##### EVPN Instance

> EVPN Instance (EVI) Identifies a VPN in the MPLS/IP Network

- VLAN-Based Service Interface
  - Single VLAN corresponds to a single bridge table (MAC forwarding table)
- Port-Based Service Interface
  - All VLANs on the port are part of the same bridge table
- VLAN Bundling Service Interface
  - Multiple VLANs share the same bridge table.
- VLAN-Aware Bundling Service Interface
  - EVI consists of multiple broadcast domains (VLANs), with each VLAN having its own bridge table.

##### Ethernet Tag-ID

- 32-bit field containing either a 12-bit (VLAN-ID) or 24-bit ID that identifies a broadcast domain **in** an EVI.
- EVI consists of one or more broadcast domains (VLANs)
- A given VLAN can be represented by multiple VLAN-IDs
  - PEs are responsible for transforming translation

##### Ethernet Segment

- Is a "site" connected to one or more PEs
- could be a single device (CE) or an entire network
- Uniquely identified by a 10 byte global ESI


  
### EVPN Multihoming and Suppression Mechanisms

### EVPN Startup Sequence

### Examining EVPN Traffic Forwarding

### Handling Multidestination Traffic

### MAC Mobility Procedure
