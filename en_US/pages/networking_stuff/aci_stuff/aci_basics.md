
# ACI Basics

- [Introduction to ACI PDF](https://www.ciscolive.com/c/dam/r/ciscolive/apjc/docs/2019/pdf/BRKACI-1000.pdf)
- [Introduction to ACI VOD](https://www.ciscolive.com/global/on-demand-library.html?search=ACI&search.technicallevel=scpsSkillLevel_aintroductory#/video/15524933755090013XSm)
- [ACI Naming and Numbering](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/aci/apic/sw/kb/b-Cisco-ACI-Naming-and-Numbering.html)
- [ACI White Paper Listing](https://www.cisco.com/c/en/us/solutions/data-center-virtualization/application-centric-infrastructure/white-paper-listing.html)

## Mantras

> Each Leaf that has a routed communication (L3OUT) to the "outside world" 
> automatically becomes a border leaf.
> - Once a border leaf will be configured, the spines have to be configured
>   as Route Reflectors.

> Always check for scalability

# Where to find stuff on the GUI

## Check l3dom for l3outs - Version 4.2(4i)

- Fabric -> Access Policies -> Physical and External Domains -> L3 Domains

# Helpful Commands CLI

## Show active leafs

- Connect to APIC using ssh.
- run `show oob`

## Check routing for specific VRF on a leaf switch

- Connect to leaf using ssh.
- run `show vrf` to show all VRFs
- run `show ip route vrf ${vrf-name}` to show routes in VRF.

## Check if a specific leaf is dropping traffic for a specific network

- `show logging ip access-list internal packet-log deny | grep {{ $ipaddress }} | head -n 15`
  - The `head -n 15` is important because otherwise you will get a lot of old logging messages.
  
## Check for existing vlans in the whole fabric

- `show vlan-domain vlan {{vlanid}}`

## Backup Users

| Username | GUI/CLI? |
| -------- | -------- |
| `apic#fallback\\admin` | GUI |
| `apic:fallback\\admin` | CLI |

## `iping` Syntax

```
iping <destination_ip> -V <Tenant>:<VRF> -S <source_ip> -c <count> ( optional -i 0 for flooding ).
```

run on leaf.

