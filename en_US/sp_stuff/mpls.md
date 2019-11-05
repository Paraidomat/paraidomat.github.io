# MPLS

## Prerequisits

- IPv4 and IGP working
- MPLS / LDP enabled
  - IOS
    - Configuration
      ```
      Device# configure terminal
      Device(config)# ip cef distributed
      Device(config)# mpls ip
      Device(config)# interface gigabitethernet 4/0/0
      Device(config-if)# mpls ip
      Device(config-if)# end
      ```
    - Verification
      
      Verify, that interfaces are configured using **show mpls interfaces detail**
      ```
      Device# show mpls interfaces detail

      Interface GigabitEthernet1/0/0:
               IP labeling enabled (ldp)
               LSP Tunnel labeling not enabled
               MPLS operational
               MTU = 1500
      Interface POS2/0/0:
               IP labeling enabled (ldp)
               LSP Tunnel labeling not enabled
               MPLS not operational
               MTU = 4470
      ```
      Verify, that the interface is up and sending LDP Discovery Hellp messages using **show mpls ldp discovery**
      ```
      Router# show mpls ldp discovery
      Local LDP Identifier:
          172.16.12.1:0
          Discovery Sources:
          Interfaces:
              Ethernet3/0 (ldp): xmit
      ```
      Verify, that the LDP session has between two routers was successfully established
      ```
      Router# show mpls ldp neighbor
      Peer LDP Ident: 10.1.1.2:0; Local LDP Ident 10.1.1.1:0
         TCP connection: 10.1.1.2.18 - 10.1.1.1.66
         State: Oper; Msgs sent/rcvd: 12/11; Downstream
         Up time: 00:00:10
         LDP discovery sources:
         FastEthernet1/0, Src IP addr: 10.20.10.2
         Addresses bound to peer LDP Ident:
         10.1.1.2    10.20.20.1    10.20.10.2
      ```
  - IOS XR
    - Configuration
      ```
      RP/0/RSP0/CPU0:R2(config)#mpls ldp
      RP/0/RSP0/CPU0:R2(config-ldp)#int g0/1/0/1
      RP/0/RSP0/CPU0:R2(config-ldp-if)#comm
      ```
    - Verify
      ```
      RP/0/RSP0/CPU0:R2#sh mpls ldp neighbor
      <omitted>
      Peer LDP Identifier: 150.1.1.1:0
      TCP connection: 150.1.1.1:646 – 150.2.2.2:43857
      Graceful Restart: No
      Session Holdtime: 180 sec
      State: Oper; Msgs sent/rcvd: 12/10; Downstream-Unsolicited
      Up time: 00:00:26
      LDP Discovery Sources:
      GigabitEthernet0/1/0/11
      Addresses bound to this peer:
      150.1.1.1        150.1.12.1     
      ```
- Router creates local labels
- LDP Hellos / Sessions
- Router shares its labels

## Useful commands

### IOS
- **show mpls ldp binding**
  ```
  R1#sh mpls ldp binding
    lib entry: 1.1.1.1/32, rev 4
          local binding:  tag: imp-null
          remote binding: tsr: 2.2.2.2:0, tag: 19
          remote binding: tsr: 3.3.3.3:0, tag: 20
    lib entry: 2.2.2.2/32, rev 8
          local binding:  tag: 19
          remote binding: tsr: 2.2.2.2:0, tag: imp-null
          remote binding: tsr: 3.3.3.3:0, tag: 21
    lib entry: 3.3.3.3/32, rev 10
          local binding:  tag: 20
          remote binding: tsr: 2.2.2.2:0, tag: 21
          remote binding: tsr: 3.3.3.3:0, tag: imp-null
    lib entry: 192.168.12.0/24, rev 2
          local binding:  tag: imp-null
          remote binding: tsr: 2.2.2.2:0, tag: imp-null
          remote binding: tsr: 3.3.3.3:0, tag: 19
    lib entry: 192.168.13.0/24, rev 6
          local binding:  tag: imp-null
          remote binding: tsr: 2.2.2.2:0, tag: 20
          remote binding: tsr: 3.3.3.3:0, tag: imp-null
    lib entry: 192.168.23.0/24, rev 12
          local binding:  tag: 21
          remote binding: tsr: 2.2.2.2:0, tag: imp-null
          remote binding: tsr: 3.3.3.3:0, tag: imp-null
  ```
## IOS-XR

- **show mpls ldp binding**
  ```
  RP/0/0/CPU0:router# show mpls ldp bindings
    5.41.0.0/16 , rev 4
            local binding: label:IMP-NULL
            No remote bindings
    5.43.9.98/32 , rev 6
            local binding: label:IMP-NULL
            No remote bindings
    10.10.2.0/24 , rev 12
            local binding: label:IMP-NULL
            remote bindings :
                lsr:10.255.255.255:0, label:16
                lsr:10.256.256.256:0, label:IMP-NULL
    10.10.3.0/24 , rev 10
            local binding: label:IMP-NULL
            remote bindings :
                lsr:10.255.255.255:0, label:IMP-NULL
                lsr:10.256.256.256:0, label:22
    22.22.22.22/32 , rev 14
            local binding: label:16
            remote bindings :
                lsr:10.255.255.255:0, label:17
                lsr:10.256.256.256:0, label:IMP-NULL
    33.33.33.33/32 , rev 2
            local binding: label:IMP-NULL
            remote bindings :
                lsr:10.255.255.255:0, label:18
                lsr:10.256.256.256:0, label:23
  ```

## LSP Path Selection

- Path to network x:
  - Who are my LDP neighbors?
  - Do they advertise a label for network x?
  - Who owns next hop IPv4 for Network x?
  - Choose that neighbor for LSP to X

## RIB, FIB, LIB, LFIB

- Look at OSPF RIB: `show ip ospf 1 rib`
- Look at FIB: `show ip route`
- Look at LIB: `show mpls ldp bindings`
- Look at LFIB: `show mpls forwarding-table`

## Manipulating Transport & Router ID

- Neighbors will connect to the transport address

### LDP Router-ID

1. Configured
2. Highest IP on Loopback
3. Highest IP on Interface

- Get current Router-ID
  ```
  Router# show mpls ldp discovery detail
    Local LDP Identifier:
      3.3.3.3:0   ! <--- This is the LDP Router ID
                  !      Zero represents, that "system wide label space" is used.
      Discovery Sources:
      Interfaces:
        <omitted>
  ```
- Change LDP Router ID to different interface
  ```
  Router(config)# mpls ldp router-id gig 2/0 force
  ```
### Transport Address

1. Configured
2. Router ID

- Change transport address
  ```
  Router(config)# int g2/0
  Router(config-if)# mpls ldp discovery transport-address interface
  Router(config-if)# int g3/0
  Router(config-if)# mpls ldp discovery transport-address interface
  ```
