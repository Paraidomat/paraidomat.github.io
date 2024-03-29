# Cisco IOS XR 

## Show Hostname of originating host for IS-IS

```
RP/0/RSP0/CPU0:routerA#show isis ipv4 route detail a.b.c.d/32
Fri Sep 17 09:43:28.165 CEST

L2 a.b.c.d/32 [20490/115] critical priority
     via x.y.z.b/32, Bundle-Ether1337, routerB tag 17, Weight: 0
     src routerC.00-00, d.e.f.g, tag 17, prefix-SID index 123, R:0 N:1
         P:0 E:0 V:0 L:0
         ^--- You are looking for routerC
```

## Show MPLS Labels for local prefix

```
show mpls forwarding [vrf { vrf }] [labels { incoming_label }]
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

## Show Transceiver / SFP / QSFP Information

```
RP/0/RP0/CPU0:xr1#show controllers Hu0/2/0/12     
Operational data for interface HundredGigE0/2/0/12:

State:
    Administrative state: disabled
    Operational state: Down (Reason: Link is shutdown)
    LED state: Not Applicable

Phy:
    Media type: R fiber over 4 Lane optics (long reach)
    Optics:
        Vendor: CISCO-FINISAR                           <-----
        Part number: FTLC1154RDPL-C1                    <-----
        Serial number: {{redacted}}
        Wavelength: 1302 nm
    Digital Optical Monitoring:
        Transceiver Temp: 33.246 C
        Transceiver Voltage: 3.256 V
```
