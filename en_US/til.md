# Today I learned

## Does `show bgp neighbor x.x.x.x received-routes` show routes received from the same AS?

No, it doesn't, even if `soft reconfiguration inbound` is configured.

## The difference between `allowas-in`, `as-override` and `local-as` 

- [`allowas-in`](https://www.cisco.com/c/en/us/support/docs/ip/border-gateway-protocol-bgp/112236-allowas-in-bgp-config-example.html)
  - > This document describes a scenario where two branch routers are connected via an ISP and running Border Gateway Protocol (BGP) between them. The two branch routers (R1 and R2), though at different locations, share the same AS number. Once the routes arrive from a branch (R1 in this case) to the Service Provider (SP) network, they will be tagged with the customer AS. Once the SP passes it to the other branch router (R2), by default, the routes will be dropped if the other branch was also running BGP with the SP using the same AS number. In this scenario, the neighbor allowas-in command is issued in order to allow BGP at the other side to inject updates. This document provides a sample configuration that helps you to understand the Allowas-in feature in BGP.
- [`as-override`](https://www.cisco.com/c/en/us/td/docs/ios/iproute_bgp/command/reference/irg_book/irg_bgp1.html#wp1116741)
  - > When the neighbor as-override command is specified to configure that a PE router overrides the autonomous system number (ASN) of a site with the ASN of a provider, it is standard practice to also configure Site of Origin (SoO). SoO prevents the route originated by a CE towards a PE from being sent back to the same CE by the PE.
- [`local-as`](https://www.cisco.com/c/en/us/support/docs/ip/border-gateway-protocol-bgp/13761-39.html)
  - > The local-AS feature allows a router to appear to be a member of a second autonomous system (AS), in addition to its real AS. This feature can only be used for true eBGP peers. You cannot use this feature for two peers that are members of different confederation sub-ASs.

## `tcpdump` on F5

- https://support.f5.com/csp/article/K13637

## BGP States

1. Idle State
   - Initializes TCP connection
   - Listens for a TCP initialization from the neighbor
   - Changes to `Connect` state
1. Connect State
   - BGP process is waiting for TCP connection to be completed.
   - If TCP connection successful:
     - Completes initialization
     - Sends `Open` message and transitions to `OpenSent` State
   - Tansitions to `Active` state, if TCP connection is unsuccessful 
1. Active State
   - Trying to initiate TCP connection
   - If TCP connection initialized send `Open` message and transition to `OpenSent` state
   - If ConnectRetry timer expires go back to `Connect` state
   - If neighbor tries to establish TCP session with unexpected IP, ConnectRetry timer is 
     reset, connection is refused and local process stays in `Active` state
1. OpenSent State
   - `Open` message has been sent, BGP is waiting to hear an `Open` message from its neighbor
   - When `Open` message has been received all fields are checked
     - [Fields](https://tools.ietf.org/html/rfc4271#page-13)
     - Should an error occur a `Notification` message is sent and state transitions to `Idle`
     - If nor errors occur a `Keepalive` message is sent
     - Hold time will be negotiated to the smaller value
     - Connection is identified as external or internal BGP by AS number
     - State changes to `OpenConfirm`
1. OpenConfirm State
   - process waits for a `Keepalive` or `Notification` message.
     - If `Keepalive`: transition to `Established`
     - If `Notification` or TCP disconnect: transition to `Idle`
1. Established State
   - BGP connection fully established
   - peers can exchange `Update`, `Keepalive`, and `Notification` messages.
     - `Update` and `Keepalive` reset Hold Timer
     - `Notification` state transitions to `Idle`
     
## Why are PIM sparse mode and dense mode called sparse mode and dense mode?

### Sparse Mode

- PIM-SM is based on a __join mechanism__, where traffic is not forwarded on a segment
  unless an explicit request originates (typically through IGMP) from the network segment

### Dense Mode

- PIM-DM is based on the __flood and prune__ mechanism, where veryone receives traffic
  until they explicitly inform (through prune mechanism) that they do not want that
  particular stream.
  
> **PIM-DM** is typically deployed in topologies where listeners are densley populated.
> And **PIM-SM** is typically deployed where the receivers are sparsely populated over
> the network, so that most of the network segment' bandwidth is conserved.

[Source](https://gtacknowledge.extremenetworks.com/articles/Q_A/What-are-the-differences-between-sparse-mode-and-dense-mode-routing)
