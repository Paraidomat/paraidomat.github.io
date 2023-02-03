# Fortigate

Note: One interface can only be attached to one zone, not to several zones.

## get

| Usage | Command | 
| ----- | ------- | 
| Get routing table | `get router info routing-table details <ip-address>` |
| Get BGP info | `get router info bgp ?` |
| Get MAC address of next-hop | `get system arp` |
| Get Interface Speed | `get system interface physical` |
| Get Transceiver Part No. | `get system interface transceiver` |

Pro Tip: If you move in config mode to a specific object and use `get` there, you get a different view about the dynamic objects.
```
config system interface
 edit port1
  get
  {{output ommited}}
```


## Diagnose

| Usage | Command | 
| ----- | ------- | 
| Get interface counters | `diag netlink interface list [<interface>] <Enter>` |
| Get policy counters | `diag firewall iprope show 10004 <policy_id>` |
| Run [Sniffer](https://help.fortinet.com/fa/cli-olh/5-6-1/Document/1600_diagnose/sniffer.htm) | `diag sniffer packet {{ interface }} '{{ filter }}' {{ verbosity }} {{ count }}` |

## Show

| Usage | Command | 
| ----- | ------- | 
| Show interface configuration | `show system interface \| grep -f <interface-name>` (or move to the interface in config mode and drop `get` there) |

## Execute

### Read Firewall Log Messages

```
execute log filter category 1
execute log display
```

