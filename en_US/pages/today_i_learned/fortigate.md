# Fortigate

## get

| Usage | Command | 
| ----- | ------- | 
| Get routing table | `get router info routing-table details <ip-address>` |
| Get BGP info | `get router info bgp ?` |
| Get MAC address of next-hop | `get system arp` |



## Diagnose

| Usage | Command | 
| ----- | ------- | 
| Get interface counters | `diag netlink interface list [<interface>] <Enter>` |
| Get policy counters | `diag firewall iprope show 10004 <policy_id>` |

## Show

| Usage | Command | 
| ----- | ------- | 
| Show interface configuration | `show system interface | grep -f <interface-name>` |

## Execute

```
execute log filter category 1
execute log display
```

