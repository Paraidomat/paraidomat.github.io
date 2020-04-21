# Multicast

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

