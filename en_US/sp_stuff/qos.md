# QoS 

## Mechanisms

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

## Implementation

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

## Classification and Marking

### Classification and Marking Concepts

- Classification:
  - Identifying and categorizing traffic into different classes
  - Without classification, all packets are treated the same
  - Should be performed close to the network edge
  
- Marking:
  - "Coloring" packet using traffic descriptors.
  - Easily distinguish the marked packet belonging to a specific class
  - Commonly used markers: CoS (Class of Service), DSCP (Differentiated Services CodePoint), MPLS EXP (Experimental) Bits. 
  - Ethernet 802.1Q CoS define three bits of priority, allowing for eight different leves of priority (0-7)
    - Class 6 and 7 are reserved for Control Plane
    - Class 0 and 1 are reserved for Scavenger / Best Effort
    - Class 2 and 3 **usually** is Premium (SQL / Web / anything that has higher priority over scavenger traffic)
    - Class 4 and 5 **usually** is Priority (VoIP / Video)
    
- Classification / Marking in Data Link Layer
  - Ethernet 802.1Q CoS
  - Cisco ISL CoS
  - Frame Relay discard eligible bit (DE)
  - ATM cell loss priority (CLP)
  - MPLS EXP
  
- Classification / Marking in Network Layer
  - IP precedence: three most significant bits of the ToS byte
    - ToS (L3) and Class of Service (L2) map one to one.
  - DSCP: six most significant bits of ToS byte
  - DSCP is backward-compatible with IP precedence.
  
- QoS Traffic Models
  - Logical grouping of packets that are to receive the same level of applied quality.
  - QoS service class can be:
    - Single user (MAC address, IP address)
    - Specific customer or set of customers
    - Specific application or set of applications.
    
### RFC 4595

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

### Trust boundaries in Enterprise and Service Provider

- Network edge at which packets are trusted or not
  - Packets are treated differently depending on whether they are confined within boundary.
  - Where should classification and marking take place?
  - Were to enforce the trust boundary?
    - Should be set as close as possible to the source.
  - Trust Boundary exists from perspective of:
    - Enterprise
    - Service Provider
