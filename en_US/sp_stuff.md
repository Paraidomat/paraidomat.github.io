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
  - 


