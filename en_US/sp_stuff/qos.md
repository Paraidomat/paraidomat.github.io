# QoS 

## Describe the DiffServ and IntServ QoS models

## Describe the QoS mechanisms

### Classification
  - Identifying and categorizing traffic into different classes
  - Without classification, all packets are treated the same
  - Should be performed close to the network edge
  - Supported by a class-oriented QoS mechanism using a `class-map`
  - Traffic can be classed by various means, including DSCP.
    - DSCP: 6 Bits in IP header aaadd0 
      - aaa = Assured Forwarding
      - dd = Drop Probability
  - Modular QoS CLI (MQC) allows classification to be implemented separately from policy.
    - Typically Class Based Weighted Fair Queuing (=Congestion management)
    - Use `class-map` to classify the traffic
    - Use `policy-map` to match the traffic and call the matching statement and do something with it.
      - Something = Queuing / Shaping / Policing / Prioritization technique.

### Marking
  - Used to mark packets based on classification
  - Marking (or coloring), marks each packet as member of a network class so
    that the packet cann be quickly recognized throughout the rest of the network.
  - Easily distinguish the marked packet belonging to a specific class
  - Commonly used markers: **CoS** (Class of Service), **DSCP** (Differentiated Services CodePoint), **MPLS EXP** (Experimental) Bits. 
  - Ethernet 802.1Q __CoS__ define three bits of priority, allowing for eight different leves of priority (0-7)
    - Class 0 and 1 are reserved for Scavenger / Best Effort
    - Class 2 and 3 **usually** is Premium (SQL / Web / anything that has higher priority over scavenger traffic)
    - Class 4 and 5 **usually** is Priority (VoIP / Video)
    - Class 6 and 7 are reserved for Control Plane
    
#### Classification / Marking in Data Link Layer
- Ethernet 802.1Q CoS
- Cisco ISL CoS
- Frame Relay discard eligible bit (DE)
- ATM cell loss priority (CLP)
- MPLS EXP
  
#### Classification / Marking in Network Layer
- IP precedence: three most significant bits of the ToS byte
  - ToS (L3) and Class of Service (L2) map one to one.
- DSCP: six most significant bits of ToS byte
- DSCP is backward-compatible with IP precedence.
 
#### QoS Traffic Models
- Logical grouping of packets that are to receive the same level of applied quality.
- QoS service class can be:
  - Single user (MAC address, IP address)
  - Specific customer or set of customers
  - Specific application or set of applications.
    
##### RFC 4595

```
    -----------------------------------------------------------------
   | Application |    Service    | Signaled |  Flow     |   G.1010   |
   |  Categories |     Class     |          | Behavior  |   Rating   |
   |-------------+---------------+----------+-----------+------------|
   | Application |   Signaling   |   Not    | Inelastic | Responsive |
   |   Control   |               |applicable|           |            |
   |-------------+---------------+----------+-----------+------------|
   |             |   Telephony   |   Yes    | Inelastic | Interactive|
   |             |---------------+----------+-----------+------------|
   |             |   Real-Time   |   Yes    | Inelastic | Interactive|
   |             |  Interactive  |          |           |            |
   |             |---------------+----------+-----------+------------|
   |    Media-   |   Multimedia  |   Yes    |    Rate   | Interactive|
   |   Oriented  |  Conferencing |          |  Adaptive |            |
   |             |---------------+----------+-----------+------------|
   |             |Broadcast Video|   Yes    | Inelastic | Responsive |
   |             |---------------+----------+-----------+------------|
   |             |  Multimedia   |   Yes    |  Elastic  |   Timely   |
   |             |   Streaming   |          |           |            |
   |-------------+---------------+----------+-----------+------------|
   |             |  Low-Latency  |    No    |  Elastic  | Responsive |
   |             |     Data      |          |           |            |
   |             |---------------+----------+-----------+------------|
   |   Data      |High-Throughput|    No    |  Elastic  |   Timely   |
   |             |    Data       |          |           |            |
   |             |---------------+----------+-----------+------------|
   |             | Low-Priority  |    No    |  Elastic  |Non-critical|
   |             |    Data       |          |           |            |
   |-------------+---------------+----------+-----------+------------|
   | Best Effort |   Standard    |    Not Specified     |Non-critical|
    -----------------------------------------------------------------

           Figure 1. User/Subscriber Service Classes Grouping
```

```
    -------------------------------------------------------------------
   |Service Class  |                              |    Tolerance to    |
   |    Name       |  Traffic Characteristics     | Loss |Delay |Jitter|
   |===============+==============================+======+======+======|
   |   Network     |Variable size packets, mostly |      |      |      |
   |   Control     |inelastic short messages, but |  Low |  Low | Yes  |
   |               | traffic can also burst (BGP) |      |      |      |
   |---------------+------------------------------+------+------+------|
   |               | Fixed-size small packets,    | Very | Very | Very |
   |  Telephony    | constant emission rate,      |  Low |  Low |  Low |
   |               | inelastic and low-rate flows |      |      |      |
   |---------------+------------------------------+------+------+------|
   |   Signaling   | Variable size packets, some  | Low  | Low  |  Yes |
   |               | what bursty short-lived flows|      |      |      |
   |---------------+------------------------------+------+------+------|
   |  Multimedia   | Variable size packets,       | Low  | Very |      |
   | Conferencing  | constant transmit interval,  |  -   | Low  | Low  |
   |               |rate adaptive, reacts to loss |Medium|      |      |
   |---------------+------------------------------+------+------+------|
   |   Real-Time   | RTP/UDP streams, inelastic,  | Low  | Very | Low  |
   |  Interactive  | mostly variable rate         |      | Low  |      |
   |---------------+------------------------------+------+------+------|
   |  Multimedia   |  Variable size packets,      |Low - |Medium|  Yes |
   |   Streaming   | elastic with variable rate   |Medium|      |      |
   |---------------+------------------------------+------+------+------|
   |   Broadcast   | Constant and variable rate,  | Very |Medium|  Low |
   |     Video     | inelastic, non-bursty flows  |  Low |      |      |
   |---------------+------------------------------+------+------+------|
   |  Low-Latency  | Variable rate, bursty short- | Low  |Low - |  Yes |
   |      Data     |  lived elastic flows         |      |Medium|      |
   |---------------+------------------------------+------+------+------|
   |      OAM      |  Variable size packets,      | Low  |Medium|  Yes |
   |               |  elastic & inelastic flows   |      |      |      |
   |---------------+------------------------------+------+------+------|
   |High-Throughput| Variable rate, bursty long-  | Low  |Medium|  Yes |
   |      Data     |   lived elastic flows        |      |- High|      |
   |---------------+------------------------------+------+------+------|
   |   Standard    | A bit of everything          |  Not Specified     |
   |---------------+------------------------------+------+------+------|
   | Low-Priority  | Non-real-time and elastic    | High | High | Yes  |
   |      Data     |                              |      |      |      |
    -------------------------------------------------------------------

               Figure 2. Service Class Characteristics
```

```
    ------------------------------------------------------------------
   |   Service     |  DSCP   |    DSCP     |       Application        |
   |  Class Name   |  Name   |    Value    |        Examples          |
   |===============+=========+=============+==========================|
   |Network Control|  CS6    |   110000    | Network routing          |
   |---------------+---------+-------------+--------------------------|
   | Telephony     |   EF    |   101110    | IP Telephony bearer      |
   |---------------+---------+-------------+--------------------------|
   |  Signaling    |  CS5    |   101000    | IP Telephony signaling   |
   |---------------+---------+-------------+--------------------------|
   | Multimedia    |AF41,AF42|100010,100100|   H.323/V2 video         |
   | Conferencing  |  AF43   |   100110    |  conferencing (adaptive) |
   |---------------+---------+-------------+--------------------------|
   |  Real-Time    |  CS4    |   100000    | Video conferencing and   |
   |  Interactive  |         |             | Interactive gaming       |
   |---------------+---------+-------------+--------------------------|
   | Multimedia    |AF31,AF32|011010,011100| Streaming video and      |
   | Streaming     |  AF33   |   011110    |   audio on demand        |
   |---------------+---------+-------------+--------------------------|
   |Broadcast Video|  CS3    |   011000    |Broadcast TV & live events|
   |---------------+---------+-------------+--------------------------|
   | Low-Latency   |AF21,AF22|010010,010100|Client/server transactions|
   |   Data        |  AF23   |   010110    | Web-based ordering       |
   |---------------+---------+-------------+--------------------------|
   |     OAM       |  CS2    |   010000    |         OAM&P            |
   |---------------+---------+-------------+--------------------------|
   |High-Throughput|AF11,AF12|001010,001100|  Store and forward       |
   |    Data       |  AF13   |   001110    |     applications         |
   |---------------+---------+-------------+--------------------------|
   |    Standard   | DF (CS0)|   000000    | Undifferentiated         |
   |               |         |             | applications             |
   |---------------+---------+-------------+--------------------------|
   | Low-Priority  |  CS1    |   001000    | Any flow that has no BW  |
   |     Data      |         |             | assurance                |
    ------------------------------------------------------------------

                Figure 3. DSCP to Service Class Mapping
```

### Congestion management
  - Prioritize the transmission of packets with a **queuing mechanism**
  - Uses the marking on each packet to determine in which queue to place packets.
  - Uses sophisticated queuing technologies, such as Weighted Fair Queuing (WFQ) and 
    Low Latency Queuing (LLQ), to ensure that time-sensitive packets (such as voice) are transmitted first.

### Congestion avoidance
  - Used to drop packets early to avoid congestion later in the network
  - **Randomly drops packets from selected queues** when previously defined limits are reached.
  - Prevents bottlenecks downstream in the network.
  - Technologies include Random Early Detection (RED) and Weighted Random Early Detection (WRED)
    - RED just drops random packets
    - WRED looks at the classification of the traffic and will drop ftp over voice.

### Policing
  - Used to enforce a __rate limit__ by **dropping or marking** down packets (= increase drop probability)
  - Typically done inbound on the provider side
  - Drops packets or marks packets when a predefined limit is reached

### Shaping
  - Enforce a rate limit by **delaying packets**, using buffers
  - Typically done outbound on the customer side 
  - Instead of dropping the packets, the packets will be slowed down
  - Queues packets when a predefined limit is reached
  
## Describe IPv6 Flow Label

- Similar to the type of service (ToS) field in the IPv4 header, the traffic class field (8 bits) is available for use by originating nodes and/or forwarding routers to identify and distinguish between different classes or priorities of IPv6 packets.
- The traffic class field may be used to set specific precedence or differentiated services code point(DSCP) values.
  - Thesevalues are used in the exact same way as in IPv4.

> The 20-bit Flow Label field in the IPv6 header [IPv6] is used by a
> source to label packets of a flow.  A Flow Label of zero is used to
> indicate packets not part of any flow.  Packet classifiers use the
> triplet of Flow Label, Source Address, and Destination Address fields
> to identify which flow a particular packet belongs to.  Packets are
> processed in a flow-specific manner by the nodes that have been set
> up with flow-specific state.

## Describe Trust Boundaries in enterprise and SP Environments

- The Service Provider Trust Boundary
  - Seperates the enterprise and SP QoS domains
  - There are different QoS actions at ingress or egress trust boundaries

- Network edge at which packets are trusted or not
  - Packets are treated differently depending on whether they are confined within boundary.
  - Where should classification and marking take place?
  - Were to enforce the trust boundary?
    - Should be set as close as possible to the source.
  - Trust Boundary exists from perspective of:
    - Enterprise
    - Service Provider
    
## Describe Cisco MQC for QoS Configurations

> MQC = Modular QoS Command-Line 

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
  - One policy can be applied on multiple interfaces:
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
### Describe hierarchical QoS configurations

- Hierarchical QoS policies
  - Specifies QoS behavior at different policy levels
  - Provides a high degree of granularity in traffic management
  - Uses the `service-policy` command to apply policy to another policy and 
    a policy to an interface.
  - **Applies a child policy to a class of parent policy.**
  
### Describe the Cisco NBAR feature for discovering network protocols and for packets classifications

> NBAR = Network Based Application Recognition

- NBAR is a classification engine that recognizes and classifies protocols and applications
- When NBAR recognizes and classifies a protocol / application, the network can be configured to apply the appropriate QoS for that protocol / application.
- QoS is applied using MQC.
- NBAR introduces several classification features that identify applications and protocols from L4 to L7, like:
  - Statically assigned TCP and UDP port numbers.
  - Non-TCP and non-UDP IP protocols.
  - Dynamically assikgned TCP and UDP port numbers. (Requires stateful inspection)
  - Subport classification or classification based on DPI.
- NBAR includes a protocol discovery feature that provides an easy way to discover application protocols that are operating on an interface.

### Describe the typical Edge PE routers and Core P routers QoS requirements

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
  - **All classification, marking, shaping, and policing should be done at the PE router.**
  - Input Policy (classification, marking, and policing) are typically done on the PE.
  - Output policy includes queuing, dropping, and shaping.

- The QoS Requirements on P Routers
  - P Routers are used to provide high-speed packet transport.
  - **Queuing and dropping are done in the core, based on packet marking done at the edge.**
  - There are two methods for QoS design:
    - Best effort with overprovisioning (expensive)
    - DiffServ backbone (commonly used)
    
## Implement classification and marking in an inter-domain network using QPPB on Cisco IOS-XR and IOS-XE

## Implement class-based markings on Cisco IOS-XR and IOS-XE

## Implement QoS pre-classify on tunnel interface on Cisco IOS-XR and IOS-XE

## Implement CB-WFQ on Cisco IOS-XR and IOS-XE

## Implement LLQ on Cisco IOS-XR and IOS-XE

## Implement WRED on Cisco IOS-XR and IOS-XE

## Implement traffic policing on Cisco IOS-XR and IOS-XE

## Implement traffic shaping on Cisco IOS-XR and IOS-XE

## Describe LPTS and hardware rate limiters on Cisco IOS-XR routers

## Describe MPLS EXP bits

## Describe MPLS QoS implementation concepts and models

## Implement MPLS DiffServ tunneling on Cisco IOS-XR and IOS-XE

## Troubleshoot QoS IOS-XR and IOS-XE configuration errors

<!-- Old stuff

## Implementation

- Methods for implementing QoS
  - CLI can be used to individually configure QoS policy on each interface.
  - Cisco AutoQoS (VoIP or Enterprise) is a best-practice QoS configuration that 
    automatically generates QoS commands.
  - MQC allows the creation of modular QoS policies and attachment of these policies on the interface
  - CiscoWorks QPM (QoS Policy Manager) = tools that allows a network administrator to create, control,
    and monitor QoS policies.




- Hierarchical QoS policies
  - Specifies QoS behavior at different policy levels
  - Provides a high degree of granularity in traffic management
  - Uses the `service-policy` command to apply policy to another policy and 
    a policy to an interface.
  - Applies a child policy to a class of parent policy. -->
