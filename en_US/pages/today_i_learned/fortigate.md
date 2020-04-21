# Fortigate

## Get routing table

```
get router info routing-table details {$ip-address}
```

## Get bgp info

```
get router info bgp ?
```

## Get interface configuration

```
show system interface {$interface-name}
```

## Get MAC-Address of nexthop

```
get system arp
```

## Get policy counter field

```
diag firewall iprope show 100004 <policy_id>
```

[Source](https://kb.fortinet.com/kb/documentLink.do?externalID=FD33786)

## Get logging messages from firewall

```
execute log filter category 1
execute log display
```

