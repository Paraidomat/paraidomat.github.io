# Cisco IOS XR 

## Show MPLS Labels for local prefix

```
show mpls forwarding vrf { vrf } 
```

## Show BGP forwarding for labels

```
show bgp vpnv4 unicast vrf { vrf } labels
```

## BFD Configuration Guide

[Config Guide](https://www.cisco.com/c/en/us/td/docs/routers/asr9000/software/asr9k_r4-2/interfaces/configuration/guide/hc42bifw.html)

## Modify route-policy, prefix-set, as-path-set, community-set, extcommunity-set, policy-global, rd-set

```
edit { route-policy | prefix-set | as-path-set | community-set | extcommunity-set { rt | soo } | policy-global | rd-set } name [ nano | emacs | vim | inline { add | prepend | remove } set-element ]  
```

[Config Guide](https://www.cisco.com/c/en/us/td/docs/routers/asr9000/software/routing/configuration/guide/rcasr9krpl.html#wp1117659)

