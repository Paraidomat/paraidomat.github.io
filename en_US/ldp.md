# LDP Introduction

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
