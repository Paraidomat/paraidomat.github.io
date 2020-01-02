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




