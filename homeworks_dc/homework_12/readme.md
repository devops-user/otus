# VxLAN. EVPN L3
### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_12/images/topology.JPG)

Настроим маршрутизацию в рамках Overlay между клиентами

**DC1-LEAF-1A**
```
!
vlan 100
vlan 200
!
vrf instance TEST
!
interface Vlan100
   vrf TEST
   ip address virtual 192.168.100.1/24
!
interface Vlan200
   vrf TEST
   ip address virtual 192.168.200.1/24
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
   vxlan vlan 100 vni 10100
   vxlan vlan 200 vni 10200
   vxlan vrf TEST vni 10999
!
ip routing vrf TEST
!
router bgp 65101
   router-id 10.0.0.1
   timers bgp 5 15
   maximum-paths 2 ecmp 2
   neighbor 10.0.1.0 remote-as 65000
   neighbor 10.0.1.0 update-source Loopback0
   neighbor 10.0.1.0 ebgp-multihop 3
   neighbor 10.0.1.0 send-community extended
   neighbor 10.0.2.0 remote-as 65000
   neighbor 10.0.2.0 update-source Loopback0
   neighbor 10.0.2.0 ebgp-multihop 3
   neighbor 10.0.2.0 send-community extended
   redistribute connected
   !
   vlan 100
      rd auto
      route-target both 100:100
      redistribute learned
   !
   vlan 200
      rd auto
      route-target both 200:200
      redistribute learned
   !
   address-family evpn
      bgp next-hop-unchanged
      neighbor 10.0.1.0 activate
      neighbor 10.0.2.0 activate
   !
   address-family ipv6
      neighbor 2002:5555:: activate
      neighbor 2002:5555:2222:: activate
   !
   vrf TEST
      rd 65101:10999
      route-target import evpn 65101:10999
      route-target export evpn 65101:10999
!
```

**DC1-LEAF-1B**
```
vlan 300
!
vrf instance TEST
!
interface Vlan300
   vrf TEST
   ip address virtual 192.168.30.1/24
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
   vxlan vlan 300 vni 10300
   vxlan vrf TEST vni 10999
!
ip routing vrf TEST
!
router bgp 65102
   router-id 10.0.0.2
   timers bgp 5 15
   maximum-paths 2
   neighbor 10.0.1.0 remote-as 65000
   neighbor 10.0.1.0 update-source Loopback0
   neighbor 10.0.1.0 ebgp-multihop 3
   neighbor 10.0.1.0 send-community extended
   neighbor 10.0.2.0 remote-as 65000
   neighbor 10.0.2.0 update-source Loopback0
   neighbor 10.0.2.0 ebgp-multihop 3
   neighbor 10.0.2.0 send-community extended
   redistribute connected
   !
   vlan 300
      rd auto
      route-target both 300:300
      redistribute learned
   !
   address-family evpn
      bgp next-hop-unchanged
      neighbor 10.0.1.0 activate
      neighbor 10.0.2.0 activate
   !
   vrf TEST
      rd 65102:10999
      route-target import evpn 65101:10999
      route-target export evpn 65101:10999
```

**DC1-LEAF-1C**
```
vlan 100
vlan 200
!
vrf instance TEST
!
interface Vlan100
   vrf TEST
   ip address virtual 192.168.100.1/24
!
interface Vlan200
   vrf TEST
   ip address virtual 192.168.200.1/24
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
   vxlan vlan 100 vni 10100
   vxlan vlan 200 vni 10200
   vxlan vrf TEST vni 10999
!
ip routing vrf TEST
!
router bgp 65103
   router-id 10.0.0.3
   timers bgp 5 15
   maximum-paths 2 ecmp 2
   neighbor 10.0.1.0 remote-as 65000
   neighbor 10.0.1.0 update-source Loopback0
   neighbor 10.0.1.0 ebgp-multihop 3
   neighbor 10.0.1.0 send-community extended
   neighbor 10.0.2.0 remote-as 65000
   neighbor 10.0.2.0 update-source Loopback0
   neighbor 10.0.2.0 ebgp-multihop 3
   neighbor 10.0.2.0 send-community extended
   redistribute connected
   !
   vlan 100
      rd auto
      route-target both 100:100
      redistribute learned
   !
   vlan 200
      rd auto
      route-target both 200:200
      redistribute learned
   !
   address-family evpn
      bgp next-hop-unchanged
      neighbor 10.0.1.0 activate
      neighbor 10.0.2.0 activate
   !
   vrf TEST
      rd 65103:10999
      route-target import evpn 65101:10999
      route-target export evpn 65101:10999
!
```

Проверим IP-связность между хостами:
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_12/images/vpc6.png)
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_12/images/vpc6.png)

Посмотрим какие типы маршрутов мы видим:
```
DC1-LEAF-1A#show bgp evpn
BGP routing table information for VRF default
Router identifier 10.0.0.1, local AS number 65101
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 10.0.0.1:100 mac-ip 0050.7966.6806
                                 -                     -       -       0       i
 * >      RD: 10.0.0.1:100 mac-ip 0050.7966.6806 192.168.100.6
                                 -                     -       -       0       i
 * >Ec    RD: 10.0.0.2:300 mac-ip 0050.7966.6807
                                 10.0.0.2              -       100     0       65000 65102 i
 *  ec    RD: 10.0.0.2:300 mac-ip 0050.7966.6807
                                 10.0.0.2              -       100     0       65000 65102 i
 * >Ec    RD: 10.0.0.2:300 mac-ip 0050.7966.6807 192.168.30.7
                                 10.0.0.2              -       100     0       65000 65102 i
 *  ec    RD: 10.0.0.2:300 mac-ip 0050.7966.6807 192.168.30.7
                                 10.0.0.2              -       100     0       65000 65102 i
 * >Ec    RD: 10.0.0.3:100 mac-ip 0050.7966.6808
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:100 mac-ip 0050.7966.6808
                                 10.0.0.3              -       100     0       65000 65103 i
 * >Ec    RD: 10.0.0.3:100 mac-ip 0050.7966.6808 192.168.100.8
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:100 mac-ip 0050.7966.6808 192.168.100.8
                                 10.0.0.3              -       100     0       65000 65103 i
 * >Ec    RD: 10.0.0.3:200 mac-ip 0050.7966.6809
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:200 mac-ip 0050.7966.6809
                                 10.0.0.3              -       100     0       65000 65103 i
 * >Ec    RD: 10.0.0.3:200 mac-ip 0050.7966.6809 192.168.200.9
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:200 mac-ip 0050.7966.6809 192.168.200.9
                                 10.0.0.3              -       100     0       65000 65103 i
 * >      RD: 10.0.0.1:200 mac-ip 0050.7966.680a
                                 -                     -       -       0       i
 * >      RD: 10.0.0.1:200 mac-ip 0050.7966.680a 192.168.200.13
                                 -                     -       -       0       i
 * >      RD: 10.0.0.1:100 imet 10.0.0.1
                                 -                     -       -       0       i
 * >      RD: 10.0.0.1:200 imet 10.0.0.1
                                 -                     -       -       0       i
 * >Ec    RD: 10.0.0.2:300 imet 10.0.0.2
                                 10.0.0.2              -       100     0       65000 65102 i
 *  ec    RD: 10.0.0.2:300 imet 10.0.0.2
                                 10.0.0.2              -       100     0       65000 65102 i
 * >Ec    RD: 10.0.0.3:100 imet 10.0.0.3
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:100 imet 10.0.0.3
                                 10.0.0.3              -       100     0       65000 65103 i
 * >Ec    RD: 10.0.0.3:200 imet 10.0.0.3
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:200 imet 10.0.0.3
                                 10.0.0.3              -       100     0       65000 65103 i
DC1-LEAF-1A#
```

```
DC1-LEAF-1B#show bgp evpn 
BGP routing table information for VRF default
Router identifier 10.0.0.2, local AS number 65102
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 10.0.0.1:100 mac-ip 0050.7966.6806
                                 10.0.0.1              -       100     0       65000 65101 i
 *  ec    RD: 10.0.0.1:100 mac-ip 0050.7966.6806
                                 10.0.0.1              -       100     0       65000 65101 i
 * >Ec    RD: 10.0.0.1:100 mac-ip 0050.7966.6806 192.168.100.6
                                 10.0.0.1              -       100     0       65000 65101 i
 *  ec    RD: 10.0.0.1:100 mac-ip 0050.7966.6806 192.168.100.6
                                 10.0.0.1              -       100     0       65000 65101 i
 * >      RD: 10.0.0.2:300 mac-ip 0050.7966.6807
                                 -                     -       -       0       i
 * >      RD: 10.0.0.2:300 mac-ip 0050.7966.6807 192.168.30.7
                                 -                     -       -       0       i
 * >Ec    RD: 10.0.0.3:100 mac-ip 0050.7966.6808
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:100 mac-ip 0050.7966.6808
                                 10.0.0.3              -       100     0       65000 65103 i
 * >Ec    RD: 10.0.0.3:100 mac-ip 0050.7966.6808 192.168.100.8
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:100 mac-ip 0050.7966.6808 192.168.100.8
                                 10.0.0.3              -       100     0       65000 65103 i
 * >Ec    RD: 10.0.0.3:200 mac-ip 0050.7966.6809
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:200 mac-ip 0050.7966.6809
                                 10.0.0.3              -       100     0       65000 65103 i
 * >Ec    RD: 10.0.0.3:200 mac-ip 0050.7966.6809 192.168.200.9
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:200 mac-ip 0050.7966.6809 192.168.200.9
                                 10.0.0.3              -       100     0       65000 65103 i
 * >Ec    RD: 10.0.0.1:200 mac-ip 0050.7966.680a
                                 10.0.0.1              -       100     0       65000 65101 i
 *  ec    RD: 10.0.0.1:200 mac-ip 0050.7966.680a
                                 10.0.0.1              -       100     0       65000 65101 i
 * >Ec    RD: 10.0.0.1:200 mac-ip 0050.7966.680a 192.168.200.13
                                 10.0.0.1              -       100     0       65000 65101 i
 *  ec    RD: 10.0.0.1:200 mac-ip 0050.7966.680a 192.168.200.13
                                 10.0.0.1              -       100     0       65000 65101 i
 * >Ec    RD: 10.0.0.1:100 imet 10.0.0.1
                                 10.0.0.1              -       100     0       65000 65101 i
 *  ec    RD: 10.0.0.1:100 imet 10.0.0.1
                                 10.0.0.1              -       100     0       65000 65101 i
 * >Ec    RD: 10.0.0.1:200 imet 10.0.0.1
                                 10.0.0.1              -       100     0       65000 65101 i
 *  ec    RD: 10.0.0.1:200 imet 10.0.0.1
                                 10.0.0.1              -       100     0       65000 65101 i
 * >      RD: 10.0.0.2:300 imet 10.0.0.2
                                 -                     -       -       0       i
 * >Ec    RD: 10.0.0.3:100 imet 10.0.0.3
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:100 imet 10.0.0.3
                                 10.0.0.3              -       100     0       65000 65103 i
 * >Ec    RD: 10.0.0.3:200 imet 10.0.0.3
                                 10.0.0.3              -       100     0       65000 65103 i
 *  ec    RD: 10.0.0.3:200 imet 10.0.0.3
                                 10.0.0.3              -       100     0       65000 65103 i
DC1-LEAF-1B#
```

```
DC1-LEAF-1C#show bgp evpn 
BGP routing table information for VRF default
Router identifier 10.0.0.3, local AS number 65103
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 10.0.0.1:100 mac-ip 0050.7966.6806
                                 10.0.0.1              -       100     0       65000 65101 i
 *  ec    RD: 10.0.0.1:100 mac-ip 0050.7966.6806
                                 10.0.0.1              -       100     0       65000 65101 i
 * >Ec    RD: 10.0.0.1:100 mac-ip 0050.7966.6806 192.168.100.6
                                 10.0.0.1              -       100     0       65000 65101 i
 *  ec    RD: 10.0.0.1:100 mac-ip 0050.7966.6806 192.168.100.6
                                 10.0.0.1              -       100     0       65000 65101 i
 * >Ec    RD: 10.0.0.2:300 mac-ip 0050.7966.6807
                                 10.0.0.2              -       100     0       65000 65102 i
 *  ec    RD: 10.0.0.2:300 mac-ip 0050.7966.6807
                                 10.0.0.2              -       100     0       65000 65102 i
 * >Ec    RD: 10.0.0.2:300 mac-ip 0050.7966.6807 192.168.30.7
                                 10.0.0.2              -       100     0       65000 65102 i
 *  ec    RD: 10.0.0.2:300 mac-ip 0050.7966.6807 192.168.30.7
                                 10.0.0.2              -       100     0       65000 65102 i
 * >      RD: 10.0.0.3:100 mac-ip 0050.7966.6808
                                 -                     -       -       0       i
 * >      RD: 10.0.0.3:100 mac-ip 0050.7966.6808 192.168.100.8
                                 -                     -       -       0       i
 * >      RD: 10.0.0.3:200 mac-ip 0050.7966.6809
                                 -                     -       -       0       i
 * >      RD: 10.0.0.3:200 mac-ip 0050.7966.6809 192.168.200.9
                                 -                     -       -       0       i
 * >Ec    RD: 10.0.0.1:200 mac-ip 0050.7966.680a
                                 10.0.0.1              -       100     0       65000 65101 i
 *  ec    RD: 10.0.0.1:200 mac-ip 0050.7966.680a
                                 10.0.0.1              -       100     0       65000 65101 i
 * >Ec    RD: 10.0.0.1:200 mac-ip 0050.7966.680a 192.168.200.13
                                 10.0.0.1              -       100     0       65000 65101 i
 *  ec    RD: 10.0.0.1:200 mac-ip 0050.7966.680a 192.168.200.13
                                 10.0.0.1              -       100     0       65000 65101 i
 * >Ec    RD: 10.0.0.1:100 imet 10.0.0.1
                                 10.0.0.1              -       100     0       65000 65101 i
 *  ec    RD: 10.0.0.1:100 imet 10.0.0.1
                                 10.0.0.1              -       100     0       65000 65101 i
 * >Ec    RD: 10.0.0.1:200 imet 10.0.0.1
                                 10.0.0.1              -       100     0       65000 65101 i
 *  ec    RD: 10.0.0.1:200 imet 10.0.0.1
                                 10.0.0.1              -       100     0       65000 65101 i
 * >Ec    RD: 10.0.0.2:300 imet 10.0.0.2
                                 10.0.0.2              -       100     0       65000 65102 i
 *  ec    RD: 10.0.0.2:300 imet 10.0.0.2
                                 10.0.0.2              -       100     0       65000 65102 i
 * >      RD: 10.0.0.3:100 imet 10.0.0.3
                                 -                     -       -       0       i
 * >      RD: 10.0.0.3:200 imet 10.0.0.3
                                 -                     -       -       0       i
DC1-LEAF-1C#
```
