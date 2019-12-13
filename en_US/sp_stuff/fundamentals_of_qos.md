
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

## Learn QoS Mechanisms [19:10]

### Common QoS Mechanics [19:10]

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
  - Using the mechanisms __Congestion Management__ is achieved.

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
  - Policing and Shaping are called __traffic conditioners__
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



## Demystify Weighted RED

## Select Appropriate Queueing

## Explain the "Token Bucket"

## Configure QoS Using MQC
