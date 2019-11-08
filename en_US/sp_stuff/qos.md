# QoS 

## Describe the DiffServ and IntServ QoS models

- Three models for implementation of QoS.
  - Best-effort
    - No QoS is applied on any packets.
    - All packets are treated the same way.
  - IntServ
    - Based on signaling events from endpoints requesting special treatment for packets that are delivered to a network.
    - Eplicitly managing network resources.
    - Uses RSVP-signaling (like MPLS-TE) for resource reservation and connection to admission mechanisms to establish and maintain QoS.
    - Can severly limit network scalability
  - DiffServ
    - provides greatest scalability and flexibility
    - Network devices recognize traffic classes and provide different levels of QoS to different traffic **classes**
    - Mechanisms are used without prior signaling
    - QoS Characeristics are are managed on a hop-by-hop basis using policies that are established independently on each intermediate device.
    - Is not considered an end-to-end QoS strategy
    - More Scalable, because thousands of applications can be mapped into a small set of classes.

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

- Traffic classification characteristics:
  - Classification is the identifying and splitting of traffic into different classes.
  - Traffic can be classed by various means, including DSCP.
  - MQC allows classification to be implemented spearately from policy.
  
Example:

```
access-list 100 permit ip any any precedence 5
access-list 100 permit ip any any dscp ef

class-map Class1
 match access-group 100
 
access-list 101 permit tcp any host 10.1.10.20 range 2000 2002
access-list 101 permit tcp any host 10.1.10.20 range 11000 11999

class-map Class2
 match access-group 101
 
policy-map Policy1
 class Class1
  priority 100
 class Class2
  bandwidth 8
 class class-default
  fair-queue
 
 interface GigabitEthernet0/1/0/9
  service-policy output Policy1
```

## Implement QoS pre-classify on tunnel interface on Cisco IOS-XR and IOS-XE

## Implement CB-WFQ on Cisco IOS-XR and IOS-XE

Class-Based Weighted Fair Queuing is an extension of WFQ that allows queuing policies that are specific to network requirements.

Characeristics:
- CBWFQ is a mechanism that is used to guarantee BW to classes
- CBWFS extends the standard WFQ functionality to provide support for user-defined traffic classes:
  - Classes are based on user-defined match criteria.
  - Packets satisfying the match criteria for a class constitute the traffic for that class. 
- A queue is reserved for each class, and traffic belonging to a class is dierected into that class queue.

- After a class has been defined, one can assign characteristics to it. 
  - To characterize a class, you can assign the guaranteed BW to it. 
  - The BW assigned to a class is the minimum bandwidth allocated to the class during congestion.
  
- Each Queue has a queue size:
  - Maximum number of packets that it can hold.
  - Manimum queue size is platform dependent.
  - Cisco IOS XR platforms use dynamic thresholds.
  
- Classification:
  - Uses `class-map`s.
  - After classification, packet enqueued
  - If the queue limit has been reached, **tail drop** within each class.

### Configuration of CBWFQ

```
class-map match-any Mission-critical
 match dscp af21 af22 af23 cs2
class-map match-any Bulk
 match dscp af11 af12 af13 cs1
 
policy-map POP-CBWFQ-policy
 class Mission-critical
  bandwidth percent 30
 class Bulk
  bandwidth percent 40
 class class-default
  bandwidth percent 20
  
interface GigabitEthernet0/0/0/1
 service-policy input POP-CBWFQ-policy
 
interface GigabitEthernet0/0/0/2
 service-policy output POP-CBWFQ-policy
```

### Configuration of Hierarchical CBWFQ

```
class-map match-any External
 match access-group ipv4 External-nets
!
class-map match-any Internal
 match access-group ipv4 Internal-nets
!
policy-map cbwfq-child
 class Internal
  bandwidth remaining percent 80
 !
 class External
  bandwidth remaining percent 20
!
policy-map cbwfq-parent
 class Mission-critical
  service-policy cbwfq-child
  bandwidth percent 30
 !
 class Bulk
  service-policy cbwfq-child
  bandwidth percent 40
!
interface GigabitEthernet0/0/0/1
 service-policy output cbwfq-parent
```

### Monitoring CBWFFQ

- `show policy-map interface $interface` displays the configuration of all classes configured for all service policies on the specified interface.

## Implement LLQ on Cisco IOS-XR and IOS-XE

- Low Latency Queuing (LLQ) brings strict priority to CBWFQ. 
  - Allows delay-sensitive data (such as voice) to be dequeued and sent first.
  - Gives delay sensitive data preferential treatment.
  
- Characteristics
  - Priority queue added to CBWFQ for real-time traffic
  - High-priority classes are guaranteed:
    - Low-latency propagation of packets
    - Bandwidth
  - Consistent configuration and operation across all media types
  - Entrance criteria to a class can be defined by any classifier:
    - Not limited to UDP as with IP RTP (Real-Time Transport Protocol) priority
    - Defines trust boundary to ensure simple classification and entry to a queue.
    - 
  - High-priority classes are also policed when congestion occurs; then they cannot exceed their guaranteed bandwidth.
  - Lower-priority classes use CBWFQ
  
> LLQ is only policed when there is congestion on the link and the software queueing is engaged.

### Configuration Example:

```
policy-map LLQ-POLICY
 class VOICE-INTERNAL
  priority level 1            ! Priority queue has precedence but requires a throttle (policing)
  police rate percent 5
  queue-limit 20 ms           ! Default maximum threshold for priority queue is 10ms
 !
 class BULK                   ! CBWFQ queue with minimum bandwidth guarantee
  bandwidth 60
  queue-limit 50 ms           ! Default maximum threshold for regular queueus is 100 ms
 !
 class VOICE-EXTERNAL
  priority level 1
  police rate percent 10      ! All traffic with same priority level directed to the same queue.
 !
 class VIDEO
  priority level 2
  police rate percent 20      ! Level 2 has lower priority than level 1
 !
inteface GigabitEthernet0/0/0/1
 service-policy input LLQ-POLICY
interface GigabitEthernet0/0/0/2
 service-policy output LLQ-POLICY
```

### Monitoring

- `show policy-map interface $interface` displays the configuration of all classes configured for all service policies on the specified interface.

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
