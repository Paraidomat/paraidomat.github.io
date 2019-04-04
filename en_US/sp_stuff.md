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
- Receivers express interest in multicast traffic by sending control messages to routers
  - Receiver is sending message to a device in the network.
  - In case of PIM Sparse Mode this device is called **Rendezvous Point**:
    > A Rendezvous Point (RP) is a router in a multicast network domain that acts as a shared root for a multicast shared tree. 
    > Any number of routers can be configured to work as RPs and they can be configured to cover different group ranges. 
    > For correct operation, every multicast router within a Protocol Independent Multicast (PIM) domain must be able to map a particular multicast group address to the same RP.
    [Source](https://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ip-multicast/whitepaper_c11-508498.html)
