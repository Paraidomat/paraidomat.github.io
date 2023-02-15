# Nokia 7750 Router TiMOS-C-15.0.R5

## Look at Global Routing Table

```
B:HOSTNAME# show router route-table x.y.z.a  

===============================================================================
Route Table (Router: Base)
===============================================================================
Dest Prefix[Flags]                            Type    Proto     Age        Pref
      Next Hop[Interface Name]                                    Metric   
-------------------------------------------------------------------------------
x.y.z.a/31                              Local   Local     0167d12h   0
       a.b.c.d INTERFACE_NAME                                               0
-------------------------------------------------------------------------------
No. of Routes: 1
Flags: n = Number of times nexthop is repeated
       B = BGP backup route available
       L = LFA nexthop available
       S = Sticky ECMP requested
===============================================================================
```

## Look at MPLS Forwarding Table

```
A:HOSTNAME# show router ldp bindings active | match 260336 pre-lines 5 post-lines 5
x.y.z.a                                lag-2

x.y.z.a/32                             Push              --      16311
x.y.z.a                                lag-2

x.y.z.a/32                             Swap            260336    16311
x.y.z.a                                lag-2

x.y.z.a/32                             Push              --      16159
x.y.z.a                                lag-2
```

