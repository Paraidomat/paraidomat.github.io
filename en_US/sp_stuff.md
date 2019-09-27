# SP Stuff

## IP Multicast

![Multicast](https://upload.wikimedia.org/wikipedia/commons/thumb/3/30/Multicast.svg/800px-Multicast.svg.png)

### Introduction

- Multicast benefits:
  - Send the same data to multiple receivers in a 1:n fashion
  - Provides better bandwidth utilization, since packets don't have to be sent multiple times for each receiver
  - Less bandwidth utilization -> Less host and router processing
  - Simultaneously deliver data for a group of receivers (simulcast)
  
### Multicast Operations

- **Sender** (source) sends one copy of a single packet addressed to a group of **receivers** (= a multicast group)
- Group address 224.0.0.0/4
- Multicast routers replicate and forward the packet to all the branches where receivers may exist
- **Receivers** express interest in multicast traffic by sending control messages to routers
  - Receiver is sending message to a device in the network
  - In case of PIM Sparse Mode this device is called **Rendezvous Point** (RP):
    > A Rendezvous Point (RP) is a router in a multicast network domain that acts as a shared root for a multicast shared tree. 
    > Any number of routers can be configured to work as RPs and they can be configured to cover different group ranges. 
    > For correct operation, every multicast router within a Protocol Independent Multicast (PIM) domain must be able to map a particular multicast group address to the same RP.
    [Source](https://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ip-multicast/whitepaper_c11-508498.html)
    
  - What is the shared tree?
    - The receiver is going to send an [IGMP Report](https://tools.ietf.org/html/rfc2236) to his local gateway.
    - Gateway relays the report to the RP. RP builds a `(*,G)` entry in its [MFIB](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/ipmulti_mfib/configuration/xe-16/imc-mfib-xe-16-book/imc-mfib-overview.html#GUID-EFEE7C66-3BDB-468D-9CBC-F2B7F9C9A1F6) Table
    - `(*,G)` is also referred to as __Any Source Multicast__
    
### Multicast Advantages

- Increased efficiency (controlled network traffic / reduces server and CPU loads)
- Optimized performance since it eliminates traffic redundancy 
  - **PIM** is used to avoid loops using a so called **RPF Check**
- Makes multipoint applications possible
- Fewer sender resources required
- Almost simultaneous delivery is assured (one packet is simultaneously forwarded across the networks)
- Foundation for a range of new applications

### Multicast Disadvantages

- UDP based
- Best-effort delivery
- No congestion avoidance
- Duplicates
- Out-of-Sequence delivery
- Reliability is a special issue not adressed in original IP multicast research
- Security

### Multicast Application

- 1:many - A single host is sending to two or more (but not all) receivers
- many:many - Any number of hosts sending to the same multicast group
  - Hosts are also members of the group (senders are receivers) using BiDir-PIM
- Other (many:1) - Any number of receivers sending data back to a source (via unicast or multicast)

### Multicast Group Address

- Multicast group addresses are derived from "Class D" (RFC 3171)
  - 224.0.0.0 -> 239.255.255.255
- Source sends stream to the multicast group with destination address = Multicast Group Address
- Receivers join the multicast group to receive stream from source.
- Well-know link-local addresses assigned by IANA
  - Reserved use of 224.0.0.0 through 224.0.0.255
  - Transient addresses, assigned and reclaimed dynamically
    - Global range: 224.0.1.0-238.255.255.255
    - Limited (local) scope: 239.0.0.0/8
- Is part of a global scope recently used for new protocols and temporary usage

## MPLS

### Prerequisits

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

### Useful commands

#### IOS
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

### LSP Path Selection

- Path to network x:
  - Who are my LDP neighbors?
  - Do they advertise a label for network x?
  - Who owns next hop IPv4 for Network x?
  - Choose that neighbor for LSP to X

### RIB, FIB, LIB, LFIB

- Look at OSPF RIB: `show ip ospf 1 rib`
- Look at FIB: `show ip route`
- Look at LIB: `show mpls ldp bindings`
- Look at LFIB: `show mpls forwarding-table`

### Manipulating Transport & Router ID

- Neighbors will connect to the transport address

#### LDP Router-ID

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
#### Transport Address

1. Configured
2. Router ID

- Change transport address
  ```
  Router(config)# int g2/0
  Router(config-if)# mpls ldp discovery transport-address interface
  Router(config-if)# int g3/0
  Router(config-if)# mpls ldp discovery transport-address interface
  ```

## QoS 

### Mechanisms

- **Classification** of traffic
  - Supported by a class-oriented QoS mechanism using a `class-map`
  - Classification is the identifying and splitting of traffic into different classes.
  - Traffic can be classed by various means, including DSCP.
    - DSCP: 6 Bits in IP header aaadd0 
      - aaa = Assured Forwarding
      - dd = Drop Probability
  - Modular QoS CLI (MQC) allows classification to be implemented separately from policy.
    - Typically Class Based Weighted Fair Queuing (=Congestion management)
    - Use `class-map` to classify the traffic
    - Use `policy-map` to match the traffic and call the matching statement and do something with it.
      - Something = Queuing / Shaping / Policing / Prioritization technique.

- **Marking** of traffic
  - Used to mark packets based on classification
  - Marking (or coloring), marks each packet as member of a network class so
    that the packet cann be quickly recognized throughout the rest of the network.

- **Congestion management** 
  - Prioritize the transmission of packets with a queuing mechanism 
  - Uses the marking on each packet to determine in which queue to place packets.
  - Uses sophisticated queuing technologies, such as Weighted Fair Queuing (WFQ) and 
    Low Latency Queuing (LLQ), to ensure that time-sensitive packets (such as voice) are transmitted first.

- **Congestion avoidance**
  - Used to drop packets early to avoid congestion later in the network
  - Randomly drops packets from selected queues when previously defined limits are reached.
  - Prevents bottlenecks downstream in the network.
  - Technologies include Random Early Detection (RED) and Weighted Random Early Detection (WRED)
    - RED just drops random packets
    - WRED looks at the classification of the traffic and will drop ftp over voice.

- **Policing**
  - Used to enforce a __rate limit__ by **dropping or marking** down packets (= increase drop probability)
  - Typically done inbound on the provider side
  - Drops packets or marks packets when a predefined limit is reached

- **Shaping**
  - Enforce a rate limit by **delaying packets**, using buffers
  - Typically done outbound on the customer side 
  - Instead of dropping the packets, the packets will be slowed down
  - Queues packets when a predefined limit is reached

### Implementation

- Methods for implementing QoS
  - CLI can be used to individually configure QoS policy on each interface.
  - Cisco AutoQoS (VoIP or Enterprise) is a best-practice QoS configuration that 
    automatically generates QoS commands.
  - MQC allows the creation of modular QoS policies and attachment of these policies on the interface
  - CiscoWorks QPM (QoS Policy Manager) = tools that allows a network administrator to create, control,
    and monitor QoS policies.

- MQC to implement QoS
  - Predominant methodology on IOS, -XE, -XR
    - Great scalability
    - Uniform CLI structure
    - seperates classification engine from the policy
    - Steps to configure:
      - Define **traffic classes** using `class-map` command.
      - Define **policies** for traffic classes using `policy-map` command.
      - Apply **service policy** on interface (inbound or outbound) using `service-policy` command.
  - Applying one policy per direction can be used on an interface.
  - One policy can be applied on multiple interface:
    ```
    interface Gi0/0/1/9
     service-policy input POLICY1
    interface Gi0/0/1/7
     service-policy input POLICY2
    interface Gi0/0/1/8
     service-policy input POLICY1
    interface Gi0/0/1/6
     service-policy input POLICY3
    ```

- QoS requirements in the Service Provider
  - SPs must provide QoS provisioning within their MPLS networks.
  - Different actions are based on the type of device (PE- or P-Router)
  - Marking, policing, and shaping should be done at the edges of the 
    SP network.
  - Queuing and dropping are done in the core, based on packet marking.

- The Service Provider Trust Boundary
  - Seperates the enterprise and SP QoS domains
  - There are different QoS actions at ingress or egress trust boundaries

- The QoS Requirements on PE Routers
  - All classification, marking, shaping, and policing should be done at the PE router.
  - Input Policy (classification, marking, and policing) are typically done on the PE.
  - Output policy includes queuing, dropping, and shaping.

- The QoS Requirements on P Routers
  - P Routers are used to provide high-speed packet transport.
  - Queuing and dropping are done in the core, based on packet marking done at the edge.
  - There are two methods for QoS design:
    - Best effort with overprovisioning (expensive)
    - DiffServ backbone (commonly used)

- Hierarchical QoS policies
  - Specifies QoS behavior at different policy levels
  - Provides a high degree of granularity in traffic management
  - Uses the `service-policy` command to apply policy to another policy and 
    a policy to an interface.
  - Applies a child policy to a class of parent policy. 
