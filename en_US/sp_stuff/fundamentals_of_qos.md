[gimmick: math]()
# Fundamentals of QoS

## Disclaimer

Source: https://www.youtube.com/watch?v=S03QnO8uhC0

## Introduction

- QoS = Managed unfairness

### Three Categories of QoS [12:15]

- IntServ = Integrated Services
  - Uses RSVP (Resource Reservation Protocol)
  - Also called "Hard QoS" / is very strict)
  - RSVP reserves a specific amout of Bandwidth for the duration of an active 
    session.
  - Is rarely used today.
  
- DiffServ = Differentiated Services
  - Router will differentiate the traffic into **classes**
  - Cisco says that there shouldn't be more than eleven classes.
  - Not as strict as IntServ

- Best Effort
  - FIFO (First in first out) w/o prioritization
  - Not strict at all. 

## Common QoS Mechanics [19:10]

- Classification and Marking
  - The Packets should be classified and marked as early as possible (on the 
    first Switch or Router)

- Queueing
  - Packets will be directed into different queues.
  - Which queue is used is determined by the marking (DSCP-Field) in the IPv4 
    header.
  - There are different **Queueing Mechanisms** that can be used.
    - LLQ (Low Latency Queuing) 
    - WFQ (Weighted Fair Queuing)
    - CBWFQ (Class-Based Weighted Fair Queuing)
  - Using the mechanisms _Congestion Management_ is achieved.

- Congestion Avoidance
  - Is used the prevent the Queues from overflowing and therefor avoiding so 
    called [Tail Drops](https://en.wikipedia.org/wiki/Tail_drop)
    - Tail Drop description:
      > Tail drop treats all traffic equally and does not differentiate between
        classes of services. Queues fill during periods of congestion. When the
        ouput queue is full and tail drop is in effect, packets are dropped 
        until the congestion is eliminated and the queue is no longer full.
      
      [Source](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/qos_conavd/configuration/15-s/qos-conavd-15-s-book/qos-conavd-oview.pdf)
  - Tail drops cause [TCP slow starts](https://blog.stackpath.com/
    tcp-slow-start/), because the the traffic does
    not reach its destinations and therefor achkowlegements will not be 
    received back from the destionations, so the TCP Window size will be reset.
  - There are different **Congestion Avoidance Mechanisms** that can be used:
    - RED (Random Early Detection)
    - WRED (Weighted Random Early Detection)
    - See also: [Congestion Avoidance Overview](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/qos_conavd/configuration/15-s/qos-conavd-15-s-book/qos-conavd-oview.pdf) by Cisco.
  - (W)RED watches the depth of the queue, once the queue is filled over RED's 
    **minimum threshold** it starts to randomly drop some of the packets to 
    avoid tail drop.
    - RED is sacrificing a few flows for the good of the many flows.
    - [Analogy](https://www.youtube.com/watch?v=v1mE_lyVKRQ)


- Policing and Shaping [33:46]
  - Policing and Shaping are called _traffic conditioners_
  - They don't guarantee a minimum amount of bandwidth, they **specify a
    maximum amount of bandwidth**
  - Policing is going to **drop** all the packets that are over the specified
    bandwidth limit.
  - Shaping works in a similar fashion, but is parking the packets that are 
    over the specified bandwidth limit into a waiting queue **delaying** the 
    traffic until there is free bandwidth available.  
  
- Link Efficiency [35:50]
  - Link Fragmentation and Interleaving (LFI)
    - On lower bandwidth links it may be necessary to fragment a big cunck of
      data into smaller pieces of data so that there is a chance for smaller
      packets to "overtake" the bigger data.
    - This is only necessary on links with less than 768kbps.
  - Compression
    - Voice Traffic is using the Realtime Transport Protocol (RTP) on Layer 4.
    - In some cases for VoIP (dependent on the codec used) the header of a
      voice packet may be twice the size of the actual payload that is
      transmitted.
    - RTP Header Compression is used to salvage that.
      - It assignes a Session Context Idenfitier (CID) to the voice traffic so
        the routers are able to tell the voice calls apart from each other
        without having to send all the header information everytime.




## Understand QoS Markings [46:42]

### Layer 2 Class of Service (CoS) [46:45]

![CoS](https://bigevilsciscoworld.files.wordpress.com/2009/11/8021q_p_frame_cos.jpg)

- Classes 6 and 7 are reserved. Don't use them.
- Class 5 is used by Cisco Phones by default for voice traffic.
- Challenge: Class of Service value has to be rewritten on every Layer 3 
  device. If that doesn't happen the CoS value will be 0.
  
### Layer 3 Type of Service (ToS) Byte

> Alternatively: _Traffic Class_ Byte in IPv6. It works the same way as in 
> IPv4. The following example will be for IPv4.

![ToS / DSCP](https://forum.huawei.com/huaweiconnect/data/attachment/forum/201704/10/20170410095335096002.png)

- If only the three MSBs are used, it's called _IP Precedence_.
  - Just like with CoS: Don't use Classes 6 and 7, as they are reserved.
  - IP Precedence isn't really used anymore. It has been replaced with the 
    more scalable _DSCP_ Values.
  - A bigger IP Precedence value is "better" / the traffic has a higher
    priority.

- If the six MSBs are used, it's called _Differentiates Services Code Point_
  or **DSCP**-Marking.
  - The DSCP Markings could be used arbitrarily but it's recommended to use the
    so called _Per Hop Behaviors_ (PHBs) defined by the IETF. 
    - The IETF gave names to 21 classes of traffic.
    - When configuring Cisco hardware one can use either the name or the
      numbers.
      
#### DSCP Values [54:20]

| foo | Binary | Decimal |
| --- | ------ | ------- |
| Default | 000000 | 0 |
| Expedited Forwarding (EF) | 101110 | 46 |
| Class Selector (CS1) | `001`000 | 8 |
| Class Selector (CSx) | `xxx`000 | x |
| Class Selector (CS7) | `111`000 | 56 |

- If a router that only supports IP Precedence receives a packet with DSCP 
  markings it will just ignore the bits 4 to 6.
  - **The class selector markings are identical with the IP Precedence 
    values.**
    
##### Assured Forwarding (AF) DSCP Values

DP = Drop Probabilty

```
+-------+-----------+-----------+-----------+
| Class | Low DP    | Medium DP | High DP   |
+=======+===========+===========+===========+
| 1     | AF11 (10) | AF12 (12) | AF13 (14) |
|       | 001010    | 001100    | 001110    |
+-------+-----------+-----------+-----------+
| 2     | AF21 (18) | AF22 (20) | AF23 (22) |
|       | 010010    | 010100    | 010110    |
+-------+-----------+-----------+-----------+
| 3     | AF31 (26) | AF32 (28) | AF33 (30) |
|       | 011010    | 011100    | 011110    |
+-------+-----------+-----------+-----------+
| 4     | AF41 (34) | AF42 (36) | AF43 (38) |
|       | 100010    | 100100    | 100110    |
+-------+-----------+-----------+-----------+
```

- If an IP Precedence router receives an Packet with DSCP AF12, it will treat
  it like a packet with IP Precedence = `001` in binary or 1 in decimal.
- The second number in AF12 defines the **Drop Probability**
  - If we assign the Protocol FTP to AF11 and Telnet to AF22, in case of a 
    filling queue the traffic of the Telnet protocol will be dropped first.
  - > **Note:** The Priority or IP Precedence value has _nothing_ to do with
    > the drop priority!!!  

## Demystify Weighted RED [1:06:50]

### Random Early Detection [1:07:05]

![RED](http://network.jecool.net/wp-content/uploads/2015/03/Capture11.png)

- RED does not care about any markings, it just drops packets Randomly

### RED Drop Ranges [1:08:25]

![RED Drop Ranges](http://network.jecool.net/wp-content/uploads/2015/03/Capture12.png)

- In case of WRED it's possible to configure - for a particular marking - 
  the minimum- and maximum threshold.
- It's not possible to set the "probability of discard" itself.
- It's only possible to set the _MPD_ (Mark probability denominator)
  - Denominator = Bottom Part of a Fraction.
  - In this example it's set to 5, so the "maximum probability of discard" is
    1/5 = 20%.
- Real World Tip: Do not change default values of the thresholds or the MPD, 
  unless there is a very specific reason to do so.

### WRED Profiles

![WRED Profiles](http://network.jecool.net/wp-content/uploads/2015/03/Capture13.png)

- Cisco enhanced RED to be able to work together with markings and thus
  different traffic classes.
- This is where the Drop Probability from the DSCP Values comes into play 
  again.
- WRED also helps with the last few dropped packages that occur when the queue
  is completely full using the _Explicit Congestion Notification_ (ECN).
  - In the ToS Byte only 6 bits are used. The last to bits are:
    Bit #7: ECT-Bit (ECN-capable Transport)
    Bit #8: CE-Bit (Congestion Experienced)
  - Instead of dropping Packets out of flows randomly the router, the receiving
    router (the ones whos queue is staring to fill up) is using the ECN feature
    to ask the sending router to slow the traffic down a little. If the sending 
    router doesn't slow down voluntarily the receiving router drops the packets
    and forces a TCP slow start.

## Select Appropriate Queueing [1:14:45]

> CB-WRF vs. LLQ

- Cisco told us to only create 11 classes of traffic. Cisco also already
  created one class for us. The so called _class-default_ class. So in total
  there are 12 classes.
- `class-default` is using FIFO.
- One alternative to FIFO from the oldern days is the usage of Weighted Fair
  Queuing. 
  - With WFQ Flows that are only sending little traffic at a slow rate won't
    "starve" because of other (maybe more traffic intensive) flows.

### Prioritizing Traffic Types 

> Example

| Traffic classes | Additional Information     |
| --------------- | -------------------------- |
| **Priority Queue** | **less than** 3Mbps     |
| Call signaling  | provide **at least** 1Mbps |
| Network Control | provide **at least** 1Mbps |
| Critical Data   | provide **at least** 3Mbps |
| Bulk Data       | provide **at least** 1Mbps |
| class-default   | using WFQ                  |

- Without the Priority Queue this basically is Class-Based-Weighted Fair 
  Queuing.
- With the Priority Queue the Class-Based-Weighted Fair Queuing becomes
  Low-Latency Queuing.

## Explain the "Token Bucket" [1:22:10]

- With policing and shaping it's possible to set a maximum bandwidth on 
  traffic.
- The "maximum Bandwidth" is described by the _Commited Information Rate_ 
  (CIR). It is defined as:
  > The **average** speed over the period of a second. 
  > $$ CIR = \frac{B_c}{T_c}$$
  > $$ B_c $$ (Commited Burst) = Number of bits (for shaping) or bytes (for 
  > policing) that are
  > deposited in the token bucket during a timing interval.
  > $$ T_c $$ (Timing Interval) = The Interval at which tokens are deposited in 
  > the token bucket.
  
- So for policing and shaping to work we use the line speed 

## Configure QoS Using MQC [1:29:25]

- Configuring QoS using the MQC is a three step process:
  1. Configure your classes of service using the `class-map` command. 
     (Not more than 11).
  2. The class-maps will be used in a `policy-map`.
     - The `policy-map` is _where the magic happens_.
     - The policy-map defines what to do with the traffic that belongs to the
       classes that are specified. 
  3. The policy-map will be applied, using a `service-policy` that is usually 
     used on an interface in the incoming or outgoing direction on an 
     interface.

### MQC Demo [1:30:30]

#### 1. Define `class-map`s.

```
class-map match-any EMAIL  ! match any of the following protocols 
 match protocol pop3       ! using NBAR
 match protocol imap
 match protocol exchange
 match protocol smtp
class-map VOICE            ! there will only be one protocol
 match protocol rtp audio  ! only match audio rtp
class-map match-any WEB
 match protocol http
 match protocol secure-http
class-map SCAVENGER
 match protocol bittorrent
```

- Notice, that the `class-default` is implemented by default:

```
HQ-ROUTER#show class-map
 Class Map match-any class-default (id 0)
  Match any
  
<output omitted>
```

#### 2. Define `policy-map`.

```
policy-map CYBER-MONDAY
 class EMAIL                ! inside this is where the magic happens
  set dscp af13
  bandwidth 512             ! set minimal amout of bandwidth in kbps
  random-detect dscp-based  ! turns WRED into DSCP mode
  random-detect ecn         ! turns on the ECN feature
 class VOICE
  priority 256
 class WEB
  bandwidth 768
 class SCAVENGER
  police 128000             ! set maximum amout of bandwidth in bps(!)
```

#### 3. Assign to interface using `service-policy`

- Some mechanisms can only be applied outbound / inbound.
  - Inbound only: Marking
  - Outbound only: Shaping
  - Both: Policing

```
interface gig 0/1
 service-policy output CYBER-MONDAY
```

#### Show commands:

```
show policy-map
  Policy Map CYBER-MONDAY
    Class EMAIL
      set dscp af13
      bandwidth 512 (kbps)
       packet-based wred, exponential weight 9
      random-detect ecn
      
      dscp  min-threshold   max-threshold   mark-probability
      -------------------------------------------------------
      default (0) -                 -             1/10
<output omitted>


show policy-map interface Gig 0/1
GigabitEthernet0/1

 Service-Policy output: CYBER-MONDAY
 
   queue stats for all priority classes:
     Queuing
     queue limit 64 packets
     (queue depth/total drops/no-buffer drops) 0/0/0
     (pkts output/bytes output) 0/0
   
   Class-map: EMAIL (match-any)
    0 packets, 0 bytes
    5 minute offered rate 0000 bps, drop rate 0000 bps
    Match: protocol pop3
      0 packets, 0 bytes
      5 minute rate 0 bps
    <output omitted>
     Queuing
     queue limit 64 packets
     (queue depth/total drops/no-buffer drops) 0/0/0
     (pkts output/bytes output) 0/0
    QoS Set
      dscp af13
        Packets marked 0
    bandwidth 512 kbps
      Exp-weight-constand: 9 (1/512)
      Mean queue depth: 0 packets
   <output omitted>
```









