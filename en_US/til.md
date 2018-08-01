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

