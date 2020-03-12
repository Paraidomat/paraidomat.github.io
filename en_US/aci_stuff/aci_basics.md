
# ACI Basics

- [Introduction to ACI PDF](https://www.ciscolive.com/c/dam/r/ciscolive/apjc/docs/2019/pdf/BRKACI-1000.pdf)
- [Introduction to ACI VOD](https://www.ciscolive.com/global/on-demand-library.html?search=ACI&search.technicallevel=scpsSkillLevel_aintroductory#/video/15524933755090013XSm)
- [ACI Naming and Numbering](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/aci/apic/sw/kb/b-Cisco-ACI-Naming-and-Numbering.html)

## Mantras

> Each Leaf that has a routed communication (L3OUT) to the "outside world" 
> automatically becomes a border leaf.
> - Once a border leaf will be configured, the spines have to be configured
>   as Route Reflectors.

> Always check for scalability

# Helpful Commands

## Show active leafs

- Connect to APIC using ssh.
- run `show oob`

## Check routing for specific VRF on a leaf switch

- Connect to leaf using ssh.
- run `show vrf` to show all VRFs
- run `show ip route vrf ${vrf-name}` to show routes in VRF.

