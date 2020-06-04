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

# How is a name resolved?

- Basis for DNS resolvement are the so called _root servers_
- Requests to the DNS system are handled in a distributed fashion.

![DNS Recursion](https://raw.githubusercontent.com/Paraidomat/paraidomat.github.io/master/en_US/resources/DNS_Recursion.svg)


