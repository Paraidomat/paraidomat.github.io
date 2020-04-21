# JunOS

## How check received routes from bgp neighbor.

In order to achieve the same output as `show ip bgp neighbor $neighbor-ip received routes` use the

```
show route receive-protocol bgp $neighbor-ip [community[-name] {$RT | $community-name}] all
```

The `all` at the end is important to get the results **before** the filtering by route policies that may be in place.

