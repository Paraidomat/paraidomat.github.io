
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

## Select Appropriate Queueing

## Explain the "Token Bucket"

## Configure QoS Using MQC
