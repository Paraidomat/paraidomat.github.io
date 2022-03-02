# JunOS

## Router 
### How check received routes from bgp neighbor.

In order to achieve the same output as `show ip bgp neighbor $neighbor-ip received routes` use the

```
show route receive-protocol bgp $neighbor-ip [community[-name] {$RT | $community-name}] all
```

The `all` at the end is important to get the results **before** the filtering by route policies that may be in place.

## Firewall

### Get Hitcount from CLI:

https://www.juniper.net/documentation/us/en/software/junos/security-policies/topics/ref/command/show-security-policies-hit-count.html

```
show security policies hit-count 
<ascending>
<descending>
<dynamic-applications>
<from-zone zone-name>
<greater-than count>
<less-than count>
<logical-system (logical-system-name | all)>
<root-logical-system>
<tenant (tenant-name | all)>
<to-zone zone-name>
<url-categories>
```
