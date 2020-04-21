# Nokia 7750 Router TiMOS-C-15.0.R5

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

