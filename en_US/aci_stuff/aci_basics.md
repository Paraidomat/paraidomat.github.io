# ACI Basics

- [Introduction to ACI](https://www.ciscolive.com/c/dam/r/ciscolive/apjc/docs/2019/pdf/BRKACI-1000.pdf)
- [ACI Naming and Numbering](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/aci/apic/sw/kb/b-Cisco-ACI-Naming-and-Numbering.html)

# Helpful Commands

## Show active leafs

- Connect to APIC using ssh.
- run `show oob`

## Check routing for specific VRF on a leaf switch

- Connect to leaf using ssh.
- run `show vrf` to show all VRFs
- run `show ip route vrf ${vrf-name}` to show routes in VRF.
