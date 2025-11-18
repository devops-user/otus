# VxLAN. Аналоги VPC
### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_14/images/topology.PNG)

Настроим порты Eth2 на DC1-LEAF-1B и DC1-LEAF-1C, чтобы использовать их в качествет одного Port-Channel и настроим сам Port-Channel:

**DC1-LEAF-1B**
```
interface Ethernet2
   description Port-channel 99
   switchport mode trunk
   channel-group 99 mode active

interface Port-Channel99
   switchport trunk allowed vlan 100
   switchport mode trunk
   !
   evpn ethernet-segment
      identifier 0000:0000:0000:001b:0100
      designated-forwarder election algorithm preference 80
      route-target import 00:00:00:00:1b:10
   lacp system-id 0000.0000.1b10
```

**DC1-LEAF-1C**
```
interface Ethernet2
   description Port-channel 99
   switchport mode trunk
   channel-group 99 mode active

interface Port-Channel99
   switchport trunk allowed vlan 100
   switchport mode trunk
   !
   evpn ethernet-segment
      identifier 0000:0000:0000:001b:0100
      designated-forwarder election algorithm preference 80
      route-target import 00:00:00:00:1b:10
   lacp system-id 0000.0000.1b10
```

На нашей виртуальной машине так-же соберем Port-Channel, для этого настроим порты Eth1 и Eth2:
**VM-1**
```
interface Ethernet1
   switchport mode trunk
   channel-group 99 mode active
#
interface Ethernet2
   switchport mode trunk
   channel-group 99 mode active
#
interface Port-Channel99
   switchport trunk allowed vlan 100
   switchport mode trunk
```

Нас виртуальной машине создим vlan 100 и настроим interface Vlan100:
```
interface Vlan100
   ip address 192.168.100.100/24
```

Посмотрим статусы Port-Channel на DC1-LEAF-1B и DC1-LEAF-1C:
**DC1-LEAF-1B**
```
DC1-LEAF-1B#show interfaces port-Channel 99 phy
Port-Channel99: displaying PHY details not supported
DC1-LEAF-1B#show interfaces port-Channel 99
Port-Channel99 is up, line protocol is up (connected)
  Hardware is Port-Channel, address is 5000.0004.0002
  Ethernet MTU 9214 bytes, BW 1000000 kbit
  Full-duplex, 1Gb/s
  Active members in this channel: 1
  ... Ethernet2 , Full-duplex, 1Gb/s
  Fallback mode is: off
  Up 18 minutes, 40 seconds
  5 link status changes since last clear
  Last clearing of "show interface" counters never
  5 minutes input rate 0 bps (0.0% with framing overhead), 0 packets/sec
  5 minutes output rate 0 bps (0.0% with framing overhead), 0 packets/sec
     215 packets input, 26922 bytes
     Received 0 broadcasts, 103 multicast
     0 input errors, 0 input discards
     824 packets output, 104163 bytes
     Sent 0 broadcasts, 788 multicast
     0 output errors, 0 output discards

DC1-LEAF-1B#show interfaces port-Channel 99 status
Port       Name   Status       Vlan     Duplex Speed  Type         Flags Encapsulation
Po99              connected    trunk    full   1G     N/A
```

**DC1-LEAF-1C**
```
DC1-LEAF-1C#show interfaces port-Channel 99
Port-Channel99 is up, line protocol is up (connected)
  Hardware is Port-Channel, address is 5000.0005.0002
  Ethernet MTU 9214 bytes, BW 1000000 kbit
  Full-duplex, 1Gb/s
  Active members in this channel: 1
  ... Ethernet2 , Full-duplex, 1Gb/s
  Fallback mode is: off
  Up 15 minutes, 32 seconds
  8 link status changes since last clear
  Last clearing of "show interface" counters never
  5 minutes input rate 0 bps (0.0% with framing overhead), 0 packets/sec
  5 minutes output rate 0 bps (0.0% with framing overhead), 0 packets/sec
     305 packets input, 39007 bytes
     Received 3 broadcasts, 250 multicast
     0 input errors, 0 input discards
     942 packets output, 117122 bytes
     Sent 1 broadcasts, 809 multicast
     0 output errors, 0 output discards

DC1-LEAF-1C#show interfaces port-Channel 99 status
Port       Name   Status       Vlan     Duplex Speed  Type         Flags Encapsulation
Po99              connected    trunk    full   1G     N/A
```

Посмотрим статус Port-Channel на виртуальной машине:
```
VM-1#show interfaces port-Channel 99
Port-Channel99 is up, line protocol is up (connected)
  Hardware is Port-Channel, address is 5000.0002.0001
  Ethernet MTU 9214 bytes, BW 2000000 kbit
  Full-duplex, 2Gb/s
  Active members in this channel: 2
  ... Ethernet1 , Full-duplex, 1Gb/s
  ... Ethernet2 , Full-duplex, 1Gb/s
  Fallback mode is: off
  Up 25 minutes, 51 seconds
  2 link status changes since last clear
  Last clearing of "show interface" counters never
  5 minutes input rate 1.10 kbps (0.0% with framing overhead), 1 packets/sec
  5 minutes output rate 157 bps (0.0% with framing overhead), 0 packets/sec
     1865 packets input, 233880 bytes
     Received 1 broadcasts, 1695 multicast
     0 input errors, 0 input discards
     531 packets output, 67383 bytes
     Sent 3 broadcasts, 363 multicast
     0 output errors, 0 output discards
```

Попробуем пингануть с виртуальнйо машины наши VPC-6  и VPC-8, и посторим какие маршруты мы увидим на DC1-LEAF-1A, DC1-LEAF-1B и DC1-LEAF-1C:

**DC1-LEAF-1A**
```
DC1-LEAF-1A#show bgp evpn route-type auto-discovery
BGP routing table information for VRF default
Router identifier 10.0.0.1, local AS number 65101
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 10.0.0.2:100 auto-discovery 0 0000:0000:0000:001b:0100
                                 10.0.0.2              -       100     0       65000 65102 i
 *  ec    RD: 10.0.0.2:100 auto-discovery 0 0000:0000:0000:001b:0100
                                 10.0.0.2              -       100     0       65000 65102 i
 * >Ec    RD: 10.0.0.3:100 auto-discovery 0 0000:0000:0000:001b:0100
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:100 auto-discovery 0 0000:0000:0000:001b:0100
                                 10.0.0.3              -       100     0       65000 65103 i
 * >Ec    RD: 10.0.0.2:1 auto-discovery 0000:0000:0000:001b:0100
                                 10.0.0.2              -       100     0       65000 65102 i
 *  ec    RD: 10.0.0.2:1 auto-discovery 0000:0000:0000:001b:0100
                                 10.0.0.2              -       100     0       65000 65102 i
 * >Ec    RD: 10.0.0.3:1 auto-discovery 0000:0000:0000:001b:0100
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:1 auto-discovery 0000:0000:0000:001b:0100
                                 10.0.0.3              -       100     0       65000 65103 i
DC1-LEAF-1A#show bgp evpn route-type ethernet-segment
BGP routing table information for VRF default
Router identifier 10.0.0.1, local AS number 65101
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 10.0.0.2:1 ethernet-segment 0000:0000:0000:001b:0100 10.0.0.2
                                 10.0.0.2              -       100     0       65000 65102 i
 *  ec    RD: 10.0.0.2:1 ethernet-segment 0000:0000:0000:001b:0100 10.0.0.2
                                 10.0.0.2              -       100     0       65000 65102 i
 * >Ec    RD: 10.0.0.3:1 ethernet-segment 0000:0000:0000:001b:0100 10.0.0.3
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:1 ethernet-segment 0000:0000:0000:001b:0100 10.0.0.3
                                 10.0.0.3              -       100     0       65000 65103 i
DC1-LEAF-1A#
```

**DC1-LEAF-1B**
```
DC1-LEAF-1B#show bgp evpn route-type ethernet-segment
BGP routing table information for VRF default
Router identifier 10.0.0.2, local AS number 65102
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 10.0.0.2:1 ethernet-segment 0000:0000:0000:001b:0100 10.0.0.2
                                 -                     -       -       0       i
 * >Ec    RD: 10.0.0.3:1 ethernet-segment 0000:0000:0000:001b:0100 10.0.0.3
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:1 ethernet-segment 0000:0000:0000:001b:0100 10.0.0.3
                                 10.0.0.3              -       100     0       65000 65103 i
DC1-LEAF-1B#
DC1-LEAF-1B#show bgp evpn route-type auto-discovery
BGP routing table information for VRF default
Router identifier 10.0.0.2, local AS number 65102
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 10.0.0.2:100 auto-discovery 0 0000:0000:0000:001b:0100
                                 -                     -       -       0       i
 * >Ec    RD: 10.0.0.3:100 auto-discovery 0 0000:0000:0000:001b:0100
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:100 auto-discovery 0 0000:0000:0000:001b:0100
                                 10.0.0.3              -       100     0       65000 65103 i
 * >      RD: 10.0.0.2:1 auto-discovery 0000:0000:0000:001b:0100
                                 -                     -       -       0       i
 * >Ec    RD: 10.0.0.3:1 auto-discovery 0000:0000:0000:001b:0100
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:1 auto-discovery 0000:0000:0000:001b:0100
                                 10.0.0.3              -       100     0       65000 65103 i
DC1-LEAF-1B#
```

**DC1-LEAF-1C**
```
DC1-LEAF-1C#show bgp evpn route-type ethernet-segment
BGP routing table information for VRF default
Router identifier 10.0.0.3, local AS number 65103
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 10.0.0.2:1 ethernet-segment 0000:0000:0000:001b:0100 10.0.0.2
                                 10.0.0.2              -       100     0       65000 65102 i
 *  ec    RD: 10.0.0.2:1 ethernet-segment 0000:0000:0000:001b:0100 10.0.0.2
                                 10.0.0.2              -       100     0       65000 65102 i
 * >      RD: 10.0.0.3:1 ethernet-segment 0000:0000:0000:001b:0100 10.0.0.3
                                 -                     -       -       0       i
DC1-LEAF-1C#show bgp evpn route-type auto-discovery
BGP routing table information for VRF default
Router identifier 10.0.0.3, local AS number 65103
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 10.0.0.2:100 auto-discovery 0 0000:0000:0000:001b:0100
                                 10.0.0.2              -       100     0       65000 65102 i
 *  ec    RD: 10.0.0.2:100 auto-discovery 0 0000:0000:0000:001b:0100
                                 10.0.0.2              -       100     0       65000 65102 i
 * >      RD: 10.0.0.3:100 auto-discovery 0 0000:0000:0000:001b:0100
                                 -                     -       -       0       i
 * >Ec    RD: 10.0.0.2:1 auto-discovery 0000:0000:0000:001b:0100
                                 10.0.0.2              -       100     0       65000 65102 i
 *  ec    RD: 10.0.0.2:1 auto-discovery 0000:0000:0000:001b:0100
                                 10.0.0.2              -       100     0       65000 65102 i
 * >      RD: 10.0.0.3:1 auto-discovery 0000:0000:0000:001b:0100
                                 -                     -       -       0       i
DC1-LEAF-1C#
```

Попингуем с наших VPC-6 и VPC-8 виртуальную машину:
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_14/images/vpc_ping.PNG)
