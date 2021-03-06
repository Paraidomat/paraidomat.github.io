# MPLS

## Prerequisits

- IPv4 and IGP working
- MPLS / LDP enabled
  - IOS
    - Configuration
      ```
      Device# configure terminal
      Device(config)# ip cef distributed
      Device(config)# mpls ip
      Device(config)# interface gigabitethernet 4/0/0
      Device(config-if)# mpls ip
      Device(config-if)# end
      ```
    - Verification
      
      Verify, that interfaces are configured using **show mpls interfaces detail**
      ```
      Device# show mpls interfaces detail

      Interface GigabitEthernet1/0/0:
               IP labeling enabled (ldp)
               LSP Tunnel labeling not enabled
               MPLS operational
               MTU = 1500
      Interface POS2/0/0:
               IP labeling enabled (ldp)
               LSP Tunnel labeling not enabled
               MPLS not operational
               MTU = 4470
      ```
      Verify, that the interface is up and sending LDP Discovery Hellp messages using **show mpls ldp discovery**
      ```
      Router# show mpls ldp discovery
      Local LDP Identifier:
          172.16.12.1:0
          Discovery Sources:
          Interfaces:
              Ethernet3/0 (ldp): xmit
      ```
      Verify, that the LDP session has between two routers was successfully established
      ```
      Router# show mpls ldp neighbor
      Peer LDP Ident: 10.1.1.2:0; Local LDP Ident 10.1.1.1:0
         TCP connection: 10.1.1.2.18 - 10.1.1.1.66
         State: Oper; Msgs sent/rcvd: 12/11; Downstream
         Up time: 00:00:10
         LDP discovery sources:
         FastEthernet1/0, Src IP addr: 10.20.10.2
         Addresses bound to peer LDP Ident:
         10.1.1.2    10.20.20.1    10.20.10.2
      ```
  - IOS XR
    - Configuration
      ```
      RP/0/RSP0/CPU0:R2(config)#mpls ldp
      RP/0/RSP0/CPU0:R2(config-ldp)#int g0/1/0/1
      RP/0/RSP0/CPU0:R2(config-ldp-if)#comm
      ```
    - Verify
      ```
      RP/0/RSP0/CPU0:R2#sh mpls ldp neighbor
      <omitted>
      Peer LDP Identifier: 150.1.1.1:0
      TCP connection: 150.1.1.1:646 – 150.2.2.2:43857
      Graceful Restart: No
      Session Holdtime: 180 sec
      State: Oper; Msgs sent/rcvd: 12/10; Downstream-Unsolicited
      Up time: 00:00:26
      LDP Discovery Sources:
      GigabitEthernet0/1/0/11
      Addresses bound to this peer:
      150.1.1.1        150.1.12.1     
      ```
- Router creates local labels
- LDP Hellos / Sessions
- Router shares its labels

## Useful commands

### IOS
- **show mpls ldp binding**
  ```
  R1#sh mpls ldp binding
    lib entry: 1.1.1.1/32, rev 4
          local binding:  tag: imp-null
          remote binding: tsr: 2.2.2.2:0, tag: 19
          remote binding: tsr: 3.3.3.3:0, tag: 20
    lib entry: 2.2.2.2/32, rev 8
          local binding:  tag: 19
          remote binding: tsr: 2.2.2.2:0, tag: imp-null
          remote binding: tsr: 3.3.3.3:0, tag: 21
    lib entry: 3.3.3.3/32, rev 10
          local binding:  tag: 20
          remote binding: tsr: 2.2.2.2:0, tag: 21
          remote binding: tsr: 3.3.3.3:0, tag: imp-null
    lib entry: 192.168.12.0/24, rev 2
          local binding:  tag: imp-null
          remote binding: tsr: 2.2.2.2:0, tag: imp-null
          remote binding: tsr: 3.3.3.3:0, tag: 19
    lib entry: 192.168.13.0/24, rev 6
          local binding:  tag: imp-null
          remote binding: tsr: 2.2.2.2:0, tag: 20
          remote binding: tsr: 3.3.3.3:0, tag: imp-null
    lib entry: 192.168.23.0/24, rev 12
          local binding:  tag: 21
          remote binding: tsr: 2.2.2.2:0, tag: imp-null
          remote binding: tsr: 3.3.3.3:0, tag: imp-null
  ```
### IOS-XR

- **show mpls ldp binding**
  ```
  RP/0/0/CPU0:router# show mpls ldp bindings
    5.41.0.0/16 , rev 4
            local binding: label:IMP-NULL
            No remote bindings
    5.43.9.98/32 , rev 6
            local binding: label:IMP-NULL
            No remote bindings
    10.10.2.0/24 , rev 12
            local binding: label:IMP-NULL
            remote bindings :
                lsr:10.255.255.255:0, label:16
                lsr:10.256.256.256:0, label:IMP-NULL
    10.10.3.0/24 , rev 10
            local binding: label:IMP-NULL
            remote bindings :
                lsr:10.255.255.255:0, label:IMP-NULL
                lsr:10.256.256.256:0, label:22
    22.22.22.22/32 , rev 14
            local binding: label:16
            remote bindings :
                lsr:10.255.255.255:0, label:17
                lsr:10.256.256.256:0, label:IMP-NULL
    33.33.33.33/32 , rev 2
            local binding: label:IMP-NULL
            remote bindings :
                lsr:10.255.255.255:0, label:18
                lsr:10.256.256.256:0, label:23
  ```

## LSP Path Selection

- Path to network x:
  - Who are my LDP neighbors?
  - Do they advertise a label for network x?
  - Who owns next hop IPv4 for Network x?
  - Choose that neighbor for LSP to X

## RIB, FIB, LIB, LFIB

- Look at OSPF RIB: `show ip ospf 1 rib`
- Look at FIB: `show ip route`
- Look at LIB: `show mpls ldp bindings`
- Look at LFIB: `show mpls forwarding-table`

## Manipulating Transport & Router ID

- Neighbors will connect to the transport address

## LDP

> LDP performs label distribution in MPLS environments. LDP uses hop-by-hop or dynamic path
> setup, but does not provide end-to-end switching services. Labels are assigned to routes
> that are chosen by the underlying IGP routing protocols. The Label Switched Paths (LSPs)
> that result from the routes, forward labeled traffic across the MPLS backbone to adjacent
> nodes.
[Source](https://www.cisco.com/c/en/us/td/docs/routers/asr9000/software/asr9k-r6-2/mpls/configuration/guide/b-mpls-cg-asr9000-62x/b-mpls-cg60x-asr9k_chapter_01010.html#con_1027186)

### LDP Port Numbers

> The UDP port the LDP Hello messages is 646.
> The TCP port for establishing LDP session connections is 646.

[Source](https://tools.ietf.org/html/rfc5036#section-3.10.1)

### Establishing and Maintaining LDP Sessions

- The exchange of LDP Discovery Hellos between to LSRs triggers LDP session 
  establishment.
- Session establishment is a two step process:
  - Transport connection establishment
  - Session initialization

[Further Reading](https://tools.ietf.org/html/rfc5036#section-2.5)
[Further Reading 2](https://networklessons.com/mpls/mpls-ldp-label-distribution-protocol)


### LDP Router-ID

1. Configured
2. Highest IP on Loopback
3. Highest IP on Interface

- Get current Router-ID
  ```
  Router# show mpls ldp discovery detail
    Local LDP Identifier:
      3.3.3.3:0   ! <--- This is the LDP Router ID
                  !      Zero represents, that "system wide label space" is used.
      Discovery Sources:
      Interfaces:
        <omitted>
  ```
- Change LDP Router ID to different interface
  ```
  Router(config)# mpls ldp router-id gig 2/0 force
  ```
### Transport Address

```
1. Configured
2. Router ID
```

- Change transport address
  ```
  Router(config)# int g2/0
  Router(config-if)# mpls ldp discovery transport-address interface
  Router(config-if)# int g3/0
  Router(config-if)# mpls ldp discovery transport-address interface
  ```
### LDP Graceful Restart

- GR provides a control plane mechanism to ensure high availability and allows
  detection and recovery from failure conditions while preserving 
  Nonstop forwarding (NSF) services.
- GR is a way to recover from signaling and control plane failures without
  impacting forwarding.
- Without GR, when an established session failes, the corresponding forwarding
  states are cleaned immediately from the restarting and peer nodes.
  - In this case LDP forwarding restarts from the beginning, causing a
    potential loss of data and connectivity.
- GR is negotiated between two peers during session initialization time in
  the [`FT SESSION` TLV](https://tools.ietf.org/html/rfc3479#page-27).
  (FT = Fault Tolerance)
  - In the TLV each peer advertises the following information:
    - **Reconnect Time:** The maximum time that the other peer will wait for
      this LSR to reconnect after control channel failure.
    - **Recovery Time:** The maximum time that the other peer has on its side
      to reinstate or refresh its states with this LSR. This time is used
      only during session reestablishment after earlier session failure.
    - **FT Flag:** Specifies whether a restart could restore the preserved
      (local) node state for this flag.
- Once GR restart session parameteres are conveyed and the session is up and
  running, graceful restart procedures are activated.

- When configuring GR in a network with multiple links and/or targeted LDP
  hello neighbor adjacencies with the same neighbor, or both:
  - Make sure that GR is activated on the session before any hello adjacency
    times out in case of neighbor control plane failures.
  - One way of achieving this is by configuring a lower session hold time
    between neighbors such that session timeout occurs before hello adjacency
    timeout. It is recommended to set the LDP session hold time using the
    formula:
    ```
    Session Holdtime <= (Hello holdtime - Hello interval) * 3
    ```
  - This means that for default values of 15 seconds and 5 seconds for link 
    Hello holdtime and interval respectively, session hold time should be set
    to 30 seconds at most.

#### Control Plane Failure

- When a control plane failure occurs, connectivity can be affected. 
- The forwardings states installed by the router control planes are lost,
  and the in-transit packets could be dropped, thus breaking NSF.

![Control Plane Failure](https://www.cisco.com/c/dam/en/us/td/i/000001-100000/95001-100000/95001-96000/95127.ps/_jcr_content/renditions/95127.jpg)
**Control Plane Failure**: This figure illustrates a control plane failure. 
For more information see [this link](https://www.cisco.com/c/en/us/td/docs/routers/asr9000/software/asr9k-r6-2/mpls/configuration/guide/b-mpls-cg-asr9000-62x/b-mpls-cg60x-asr9k_chapter_01010.html#con_1061504)

#### Phases in Graceful Restart

- The GR mechanism is divided into different phases:

1. **Control communication failure detection**
   Control communication failure is deteced when the system detects either:
   - Missed LDP hello discovery messages
   - Missed LDP keepalive protocol messages
   - Detection of TCP disconnection with a peer
2. **Forwarding state maintenance duing failure**
   - Persistent forwarding states at each LDP are achieved though persistent
     storage (checkpoint) by the LDP control plane.
   - While the control plane is in the process of recovering, the forwarding 
     plane keeps the forwarding states, but marks them as stale.
   - Similarly, the peer control plane also keeps (and marks as stale) the 
     installed forwarding rewrites associated with the node that is restarting.
   - The combination of local node forwarding and remote node forwarding plane
     states ensures NSF and no disruption of the traffic.
3. **Control state recovery**
   - Recovery occurs when the session is reestablished and label bindings are
     exchanged again. This process allows the peer nodes to synchronize and
     refresh stale forwarding states.

For further Information check [this link](https://www.cisco.com/c/en/us/td/docs/routers/asr9000/software/asr9k-r6-2/mpls/configuration/guide/b-mpls-cg-asr9000-62x/b-mpls-cg60x-asr9k_chapter_01010.html#con_1137244)

### Label Advertisement Control (Outbound Filtering)

- By default, LDP advertises labels for all the prefixses to all its neighbors.
- When this is not desirable (scalability/security), you can configure LDP to 
  perform outbound filtering for local label advertisement for one or more
  prefixes to one or more peers.

#### IOS-XR:

```
mpls ldp 
     address-family ipv4
       label local advertise
          disable
          for pfx_acl_1 to peer_acl_1 
          for pfx_acl_2 to peer_acl_2
          for pfx_acl_3
          interface POS 0/1/0/0
          interface POS 0/2/0/0
      !
    !
 
   	ipv4 access-list pfx_acl_1
     10 permit ipv4 host 1.0.0.0 any
 !
 ipv4 access-list pfx_acl_2
     10 permit ipv4 host 2.0.0.0 any
 !
 ipv4 access-list peer_acl_1
     10 permit ipv4 host 1.1.1.1 any
     20 permit ipv4 host 1.1.1.2 any
 !
 ipv4 access-list peer_acl_2
     10 permit ipv4 host 2.2.2.2 any
 !
!   
```


### Label Acceptance Control (Inbound Filtering)

- By default, LDP accepts labels (as remote bindings) for all prefixes from 
  all peers.
- LDP operates in **liberal label retention mode**, which instructs LDP to
  keep remote bindings from all peers for a given prefix.
- For security reasons, or to conserve memory, you can override this behavior
  by configuring label binding acceptance for set of prefixes from a given
  peer. 

> Note: Inbound filtering can also be implemented using an outbound filtering policy; however, you may not be able to implement this system if an LDP peer resides under a different administration domain. When both inbound and outbound filtering options are available, we recommend that you use outbound label filtering.

#### IOS-XR:

```
mpls ldp
 label 
accept
 for pfx_acl_2 from 192.168.2.2
!
 !
!

mpls ldp
 address-family ipv4
  label remote accept from 192.168.1.1:0 for pfx_acl_2
  !
 !
!
```

### Local Label Allocation Control

- By default, LDP allocates labels for all prefixes that are not BGP prefixes.
- This is acceptable when LDP is used for applications other than L3VPN core
  transport.
- When LDP is used to set up L3VPN traffic in the core, it is not efficient
  or even necessary to allocate and advertise local labels for, potentially, 
  thousands of IGP prefixes. 
  - In such a case, LDP is typically required to allocate and advertise local 
    labels for /32 addresses for the PE routers.
  - This is accomplished using LDP local label allocation control, where an
    ACL can be used to limit allocation of local labels to a set of prefixes.
- Limiting local label allocation provides several benefits, including reduced
  memory usage requirements, fewer local forwarding updates, and fewer network 
  peer updates.

> Note: You can configure label allocation using an IP access list to specify
> a set of prefixes that local labels can allocate and advertise.

#### IOS-XR:

```
mpls ldp
 address-family ipv4
  label local allocate for pfx_acl_1
  ! 
 !
```

### Session Protection

- When a link comes up, IP converges earlier and much faster than MPLS LDP and
  may result in MPLS traffic loss until MPLS convergence.
- If a link flaps, the LDP session will also flap due to loss of link
  discovery.
- LDP session Protection minimizes traffic loss, provides faster convergence,
  and protects exsisting LDP (link) sessions by means of "parallel" source of 
  targeted discovery hello.
- An LDP session is kept alive and nighbor label bindings are maintained when
  links are down.
- Upon reestablishment of primarly link adjacencies, MPLS convergence is 
  expedited as LDP need not relearn the neighbor label bindings.

- LDP Session Protection lets you configure LDP to automatically protect 
  sessions with all or a given set of peers (as specified by peer-acl).
- When configured, LDP initiates backup targeted hellos automatically for 
  neighbors for which primary link adjacencies already exist.
- These backup targeted hellos maintain LDP sessions when primary link 
  adjacencies go down.


![Session Protection](https://www.cisco.com/c/dam/en/us/td/i/100001-200000/150001-160000/158001-159000/158015.ps/_jcr_content/renditions/158015.jpg)
- The session protection figure illustrates LDP session protection between 
  the neighbors R1 and R3. 
- The primary link adjacency between R1 and R3 is directly connected and the
  backup targeted adjacency is maintained between R1 and R3. 
- If the direct link fails, LDP link adjacency is destroyed, but the session
  is kept up and running using targeted hello adjacency (trough R2). 
- When the direct link comes back up, there is no change in the LDP session 
  state and LDP can converge quickly and begin forwarding MPLS traffic.

> Note: When LDP session protection is activated (upon link failure), 
> protection is maintained for an unlimited period of time if not specified
> otherwise.

#### IOS-XR

```
mpls ldp
  session protection duration 60 for peer_acl_1
!
```

### IGP Synchonization

- Lack of synchronization between LDP and IGP can cause MPLS traffic loss.
- Upon link up, for example, IGP can advertise and use a link before LDP 
  convergence has occurred; or, a link may continue to be used in IGP after an
  LDP session goes down.

- LDP IGP synchronization synchronizes LDP and IGP so that IGP advertises links
  with regular metrics only when MPLS LDP is converged on that link.
- LDP considers a link converged when at least one LDP session is up and 
  running on the link for which LDP has sent its applicable label bindings and
  received at least one label binding from the peer.
- LDP communicates this information to IGP upon link up or session down events
  and IGP acts accordingly, depending on sync state.

- In the event of an LDP graceful restart session disconnect, a session is
  treated as converged as long as the graceful restart neighbor is timed out.
- Additionaly, upon local LDP restart, a checkpointed recovered LDP graceful
  restart session is used and treated as converged and is given an opportunity
  to connect and resynchronize.

- Under certain circumstances, it might be required to delay declaration of 
  resyncronization to a configurable interval.
  - LDP provides a configuration option to delay declaring synchronization up 
    for up to 60 seconds.
  - LDP communicates this information to IGP upon linkup or session down 
    events.

> Note: The configuration for LDP IGP synchronization resides in respective IGPs 
> (OSPF and IS-S) and there is no LDP-specific configuration for enabling this
> feature. However, there is a specific LDP configuration for IGP sync delay 
> timer.

#### IOS-XR

##### OSPF

```
router ospf 100
  mpls ldp sync
  !
  mpls ldp
   igp sync delay 30
  !
```

##### IS-IS

```
router isis 100
 interface POS 0/2/0/0
  address-family ipv4 unicast
  mpls ldp sync
  !
 !
!
mpls ldp
 igp sync delay 30
```

## Traffic Engineering

### Technology Overview

[Source](https://www.ciscolive.com/c/dam/r/ciscolive/us/docs/2015/pdf/BRKMPL-2100.pdf)

#### Overview

- Traffic Engineering ...
  - ... introduces explicit routing
  - ... supports constraint-based routing
  - ... support admission control
  - ... provides protection capabilities
  - ... uses RSVP-TE to establish LSPs
  - ... uses IS-IS / OSPF extensions to advertise link attributes

#### Operation steps:

- Link information distribution using IS-IS-TE / OSPF-TE
- Path calcucations (CSPF)
- Path setup (RSVP-TE)
- Forwarding Traffic down the Tunnel
  - Autoroute
  - Static route
  - Policy based routing
  - Policy/Class-based tunnel selection
  - Forwarding Adjacency
  - Pseudowire Tunnel selection

#### Link Information Distribution

- Additional link characteristics
  - Interface address
  - Neighbor address
  - Physical bandwidth
  - Maximum reservable bandwidth
  - Unreserved bandwidth (at eight priorities)
  - TE metric
  - Administrative group (attribute flags)
- ISIS or OSPF flood link information
- All TE nodes build a TE topology database
- Not required if using off-line path computation

#### Path Calculation

- TE nodes can perform constraint-based routing
- Tunnel head end generally responsible for path calculation
- Constraints and topology database used as input to path computation
- Shortest-path-first algorithm ignores links not meeting constraints
- Tunnel can be signaled once a path is found

#### TE LSP Signaling

- Tunnel signaled with TE extensions to RSVP
- Soft state maintained with **downstream PATH messages**
- Soft state maintained with **upstream RESV messages**
- LFIB populated using RSVP labels allocated by RESV messages


### Tunneling Modes for MPLS DiffServ

- Tunneling is the ability of QoS to be transparent from one edge of a network
  to the other edge of the network.
- A tunnel starts where there is MPLS label imposition.
- A tunnel ends where there is label disposition.

- There are three ways to forward packets through a network:
  1. Pipe mode with an explicit NULL LSP
  2. Short Pipe mode
  3. Uniform mode

- Pipe mode and Short Pipe mode QoS trasparency. 
  - With QoS transparency the customer's IP marking in the IP packet is 
    preserved.

> Note: The only difference between Pipe mode and Short Pipe mode is which PHB
> is used on the service provider's egress edge router.
> - In Pipe mode with an explicit NULL LSP, QoS is done on the PE-to-CE link
>   based on the service provider's PHB markings.
> - The egress LSR still uses the marking that was used by intermediate LSRs

#### Pipe Mode with an Explicit Null LSP 

- The QoS tunnel goes from the ingress CE router through the PE router to the
  egress CE router.
- There is an explicit NULL LSP from the CE router to the PE router. 
  - The label entry contains an MPLS EXP flield, but does not carry a label 
    value for forwarding purposes. 
  - It contains a zero (a null label value) for all packets going to the 
    ingress PE router.
- The egress PE router removes the label entry and forwards packets as IP, but 
  QoS is done on the output interface based on the MPLS EXP field received by
  the egress PE router.
- The service provider does not overwrite the IP Precedence value in the 
  service provider's network.

[Further Reading](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/mp_te_diffserv/configuration/15-mt/mp-te-diffserv-15-mt-book/mp-diffserv-tun-mode.html#GUID-26B0B6AD-D814-4E4B-94F0-C4CC0FC8E1BB)


#### Short Pipe Mode

- The QoS tunnel goes from the ingress PE router to the egress PE router.
- The egress PE router transmits packets as IP and QoS is done on the output
  interface based on the IP DSCP or IP Precedence value. 
- The service provider does not overwrite the DSCP or IP Precedence value
  in the service provider's network.

[Further Reading](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/mp_te_diffserv/configuration/15-mt/mp-te-diffserv-15-mt-book/mp-diffserv-tun-mode.html#GUID-78E7DB58-B014-4105-80BF-591E1375E469)

#### Uniform Mode 

- In a label, the MPLS EXP field is not the same as the label value.

- The topmost label contains the following:
  - Label value, which contains labels and other information, to forward the
    packet.
  - MPLS EXP field, which only pertains to the QoS of the packet, not the 
    route.
    - The EXP Field value is not advertised. Its value comes from the way that
      the packet is received.

- In Uniform mode, packets are treated uniformly in the IP and MPLs networks.
  - IP Precendes and MPLS EXP Values are **always** identical.
  - Whenever a router changes (or recolors) the PHB of a packet, that change
    must be propagated to all encapsulation markings.
  - The propagation is performed by a router only when a PHB is added or
    exposed due to label imposition or disposition on any router in the 
    packet's path. 
  - The color must be reflected everywhere, at all levels.
  - For example, if a packet's QoS marking is changed in the MPLS network, the 
    IP QoS marking reflects that change.

- Uniform mode functions as follows:
  - In both the MPLS-to-MPLS path and the MPLS-to-IP path, the PHBs of the 
    topmost popped label are copied into the new top label or in the IP DSCP
    if no label remains.
  - There can be a maximum of eight PHBs.
  - If the PHBs are enclosed using more than the three Precedence bits, you 
    must map DSCP to MPLS at the entry to the MPLS cloud.
  - When packets leave the MPLS cloud, you must remap from the MPLS EXP value
    to the DSCP field in the IP header.


[Further Reading](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/mp_te_diffserv/configuration/15-mt/mp-te-diffserv-15-mt-book/mp-diffserv-tun-mode.html#GUID-2758FEF1-9B35-4748-90F6-93ABE8457EC3)

### Fast Reroute

[Source](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/mp_te_path_protect/configuration/xe-3s/mp-te-path-protect-xe-3s-book/mp-te-frr-node-prot.html#GUID-DBA75617-F75C-47BF-88B2-DE11DD11D3CB)

#### Fast Reroute Overview

- FRR is a mechanism for protecting MPLS TE LSPs from link and node failures
  by locally repairing the LSPs at the point of failure, allowing data to 
  continue to flow on them while their headend routers attempt to establish
  new end-to-end LSPs to replace them.
- FRR locally repairs the protected LSPs by rerouting them over backup 
  tunnels that bypass failed links or node.

#### Link Protection

- Backup tunnels that bypass only a single link of the LSP's path provide link
  protection.
- They protect LSPs if a link along their path fails by rerouting the LSPs to 
  the next hop (bypassing the failed link).
- These are referred to as next-hop (NHOP) backup tunnels because they 
  terminate at the LSP's next hop beyond the point of failure.

![NHOP Backup Tunnel](https://www.cisco.com/c/dam/en/us/td/i/000001-100000/55001-60000/59001-60000/59556.ps/_jcr_content/renditions/59556.jpg)

#### Node Protection

- FRR provides node protection for LSPs. Backup tunnels that bypass next-hop
  nodes along LSP paths are called next-next-hop (NNHOP) backup tunnels because
  they terminate at the following the next-hop node of the LSP paths, thereby
  bypassing the next-hop node.
- They protect LSPs if a node along their path fails by enabling the nodes 
  upstream of the failure to reroute the LSPs and their traffic around the 
  failed node to the next-next hop.
- FRR supports the use of RSVP Hellos to accelerate the detection of node 
  failures.
- NNHOP backup tunnels also proide protection from link failures, because they
  bypass the failed link and the node.

![NNHOP Backup Tunnel](https://www.cisco.com/c/dam/en/us/td/i/000001-100000/55001-60000/59001-60000/59557.ps/_jcr_content/renditions/59557.jpg)

- If an LSP is using a backup tunnel and something changes so that the LSP is
  no longer appropriate for the backup tunnel, the LSP is torn down. Such
  changes are the following:
  - Backup bandwidth of the backup tunnel is reduced.
  - Backup bandwidth type of backup tunnel is changed to a type that is 
    incompatible with the primary LSP.
  - Primary LSP is modified so that FRR is disabled. (The `no mpls traffic-eng
    fast-reroute` command is entered.)




