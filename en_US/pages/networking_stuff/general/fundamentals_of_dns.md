# Fundamentals of DNS (Domain Name System)

- [Source](https://www.youtube.com/playlist?list=PLCb8EhYsrW_tP7wRIStDiA8FtQn5Kf6ju)

## What is DNS for?

- Humans aren't able to memorize IP addresses very well.
  - Especially not IPv6 addresses
- IPs do not (really) encode geo information.
- DNS resolves (globally unique) hostnames into IP adresses.
- Name n <<-->> m IP

## DNS is a directory service

- structure of names (delimiter: `.`)
- assignments of unique names
- resolving names in both directions
- distributing data in layers

## Components / devices used

- DNS-Server
  - Name-Server.
  - All Servers that are responsible for DNS to work.
- Namespace
  - Is managed by the DNS-Servers
  - Is in a tree layout
- Resolver
  - Is usually part of the operating system
  - All softwares can ask the resolver to resolve a hostname to an IP address

## Simplyfied flow (client view)

![DNS Client Server](https://raw.githubusercontent.com/Paraidomat/paraidomat.github.io/master/en_US/resources/DNS_Client_Server.svg)

The way the DNS Server works is irrelevant for the client.

## In the dark ages

- Befor DNS was implemented so called `host-files` were used
  - Linux: `/etc/hosts`
  - Windows: `C:\Windows\hosts`

- File Format:
  ```
  192.168.1.2	foo	bar
  192.168.2.3	blupp	bla
  ```
  - `bar` is an alias for `foo`
  - `bla` is an alias for `blupp`

## Namespace Layout

- Distribution of hostnames into different domains helps with load distribution and enables decentralized management
- Tree layout that usually is searched from the root outwards to the leafs

![DNS Namespace](https://upload.wikimedia.org/wikipedia/commons/9/91/Dns-raum.svg)
Source: https://de.wikipedia.org/wiki/Domain_Name_System

- A name in DNS system is called a __Fully Qualified Domain Name__ (FQDN).
  - Example: `de.wikipedia.org.`
    - Notice the last `.` representing the root of the namespace

## How is a name resolved?

- Basis for DNS resolvement are the so called _root servers_
- Requests to the DNS system are handled in a distributed fashion.

![DNS Recursion](https://raw.githubusercontent.com/Paraidomat/paraidomat.github.io/master/en_US/resources/DNS_Recursion.svg)

- To follow the recursion of a hostname use the following command:
  `dig +trace +cmd §FQDN [@DNS-SERVER-IP]`

- Example Output:

```
paraidomat@zotac:~$ dig +trace +cmd www.heise.de @9.9.9.9

; <<>> DiG 9.11.5-P1-1ubuntu2.6-Ubuntu <<>> +trace +cmd www.heise.de @9.9.9.9
;; global options: +cmd
.                       36799   IN      NS      c.root-servers.net.  <-- Ask the root servers
.                       36799   IN      NS      g.root-servers.net.
.                       36799   IN      NS      e.root-servers.net.
.                       36799   IN      NS      f.root-servers.net.
.                       36799   IN      NS      j.root-servers.net.
.                       36799   IN      NS      l.root-servers.net.
.                       36799   IN      NS      k.root-servers.net.
.                       36799   IN      NS      i.root-servers.net.
.                       36799   IN      NS      d.root-servers.net.
.                       36799   IN      NS      a.root-servers.net.
.                       36799   IN      NS      h.root-servers.net.
.                       36799   IN      NS      b.root-servers.net.
.                       36799   IN      NS      m.root-servers.net.
[...]
;; Received 525 bytes from 9.9.9.9#53(9.9.9.9) in 9 ms

de.                     172800  IN      NS      z.nic.de.  <-- ask the authorative server for the TLD
de.                     172800  IN      NS      a.nic.de.
de.                     172800  IN      NS      n.de.net.
de.                     172800  IN      NS      f.nic.de.
de.                     172800  IN      NS      l.de.net.
de.                     172800  IN      NS      s.de.net.
de.                     86400   IN      DS      45580 8 2 918C32E2F12211766BE6226674F447458F2259B9A0D87B44D29D55AF ECA6B2E1
[...]
;; Received 778 bytes from 2001:7fe::53#53(i.root-servers.net) in 10 ms

heise.de.               86400   IN      NS      ns.s.plusline.de.  <-- Ask the authorative server for heise.de
heise.de.               86400   IN      NS      ns.heise.de.
heise.de.               86400   IN      NS      ns.plusline.de.
heise.de.               86400   IN      NS      ns.pop-hannover.de.
heise.de.               86400   IN      NS      ns2.pop-hannover.net.
[...]
;; Received 806 bytes from 194.146.107.6#53(n.de.net) in 13 ms

www.heise.de.           86400   IN      A       193.99.144.85  <-- Ask the authorative server for www.heise.de
heise.de.               86400   IN      NS      ns.s.plusline.de.
heise.de.               86400   IN      NS      ns2.pop-hannover.net.
heise.de.               86400   IN      NS      ns.heise.de.
heise.de.               86400   IN      NS      ns.pop-hannover.de.
heise.de.               86400   IN      NS      ns.plusline.de.
;; Received 259 bytes from 62.48.67.66#53(ns2.pop-hannover.net) in 31 ms
```



## What is a root server?

- The Root-Servers are the authorative name servers that serve the DNS root zone (`.`).
- They consist of a network of hundreds of servers in many countries around the world.
- Operators of DNS recursive resolvers need to configure a _root hints file_.
  - This file contains the names and IP addresses of the root servers, so the software can bootstrap the DNS resolution process.
  - For many pieces of software, this list comes built into the software.
- Further information on the DNS root servers, as well as a map of DNS root server location can be found on [https://root-servers.org/](root-servers.org)

### List of DNS Root-Servers

- Should you have a working DNS Server and need a new _root hints file_ you can use the following command:

```
paraidomat@server:~$ dig @A.ROOT-SERVERS.NET

; <<>> DiG 9.11.5-P1-1ubuntu2.6-Ubuntu <<>> @A.ROOT-SERVERS.NET
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 2587
;; flags: qr aa rd; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 27
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;.                              IN      NS

;; ANSWER SECTION:
.                       518400  IN      NS      e.root-servers.net.
.                       518400  IN      NS      h.root-servers.net.
.                       518400  IN      NS      l.root-servers.net.
.                       518400  IN      NS      i.root-servers.net.
.                       518400  IN      NS      a.root-servers.net.
.                       518400  IN      NS      d.root-servers.net.
.                       518400  IN      NS      c.root-servers.net.
.                       518400  IN      NS      b.root-servers.net.
.                       518400  IN      NS      j.root-servers.net.
.                       518400  IN      NS      k.root-servers.net.
.                       518400  IN      NS      g.root-servers.net.
.                       518400  IN      NS      m.root-servers.net.
.                       518400  IN      NS      f.root-servers.net.

;; ADDITIONAL SECTION:
e.root-servers.net.     518400  IN      A       192.203.230.10
e.root-servers.net.     518400  IN      AAAA    2001:500:a8::e
h.root-servers.net.     518400  IN      A       198.97.190.53
h.root-servers.net.     518400  IN      AAAA    2001:500:1::53
l.root-servers.net.     518400  IN      A       199.7.83.42
l.root-servers.net.     518400  IN      AAAA    2001:500:9f::42
i.root-servers.net.     518400  IN      A       192.36.148.17
i.root-servers.net.     518400  IN      AAAA    2001:7fe::53
a.root-servers.net.     518400  IN      A       198.41.0.4
a.root-servers.net.     518400  IN      AAAA    2001:503:ba3e::2:30
d.root-servers.net.     518400  IN      A       199.7.91.13
d.root-servers.net.     518400  IN      AAAA    2001:500:2d::d
c.root-servers.net.     518400  IN      A       192.33.4.12
c.root-servers.net.     518400  IN      AAAA    2001:500:2::c
b.root-servers.net.     518400  IN      A       199.9.14.201
b.root-servers.net.     518400  IN      AAAA    2001:500:200::b
j.root-servers.net.     518400  IN      A       192.58.128.30
j.root-servers.net.     518400  IN      AAAA    2001:503:c27::2:30
k.root-servers.net.     518400  IN      A       193.0.14.129
k.root-servers.net.     518400  IN      AAAA    2001:7fd::1
g.root-servers.net.     518400  IN      A       192.112.36.4
g.root-servers.net.     518400  IN      AAAA    2001:500:12::d0d
m.root-servers.net.     518400  IN      A       202.12.27.33
m.root-servers.net.     518400  IN      AAAA    2001:dc3::35
f.root-servers.net.     518400  IN      A       192.5.5.241
f.root-servers.net.     518400  IN      AAAA    2001:500:2f::f

```

- Explanation:
  - `518400` is the TTL in Seconds 
  - `NS` = Name Server
  - `A` = A-Record (For IPv4)
  - `AAAA` = AAAA-Record (For IPv6)
  - `IN` = Internet (`CH` (Chaosnet), and `HS` (Hesiod))
