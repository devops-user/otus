# VxLAN. EVPN L2
### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_11/images/topology.JPG)

Настройка Overlay на основе VxLAN EVPN для L2 связанности между клиентами:

**DC1-LEAF-1A**
```
!
router bgp 65101
   router-id 10.0.0.1
   timers bgp 5 15
   maximum-paths 2
   neighbor 10.1.0.2 remote-as 65102
   neighbor 10.1.0.2 update-source Loopback1
   neighbor 10.1.0.2 ebgp-multihop 3
   neighbor 10.1.0.2 send-community extended
   neighbor 10.1.0.3 remote-as 65103
   neighbor 10.1.0.3 update-source Loopback1
   neighbor 10.1.0.3 ebgp-multihop 3
   neighbor 10.1.0.3 send-community extended
   redistribute connected
   !
   vlan 100
      rd 65101:10100
      route-target both 1:1
      redistribute learned
   !
   address-family evpn
      bgp next-hop-unchanged
      neighbor 10.1.0.2 activate
      neighbor 10.1.0.3 activate
   !
   address-family ipv4
      no neighbor 10.1.0.2 activate
      no neighbor 10.1.0.3 activate
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
   vxlan vlan 100 vni 10100
!
```
**DC1-LEAF-1B**
```
!
router bgp 65102
   router-id 10.0.0.2
   timers bgp 5 15
   maximum-paths 2
   neighbor 10.1.0.1 remote-as 65101
   neighbor 10.1.0.1 update-source Loopback1
   neighbor 10.1.0.1 ebgp-multihop 3
   neighbor 10.1.0.1 send-community extended
   neighbor 10.1.0.3 remote-as 65103
   neighbor 10.1.0.3 update-source Loopback1
   neighbor 10.1.0.3 ebgp-multihop 3
   neighbor 10.1.0.3 send-community extended
   redistribute connected
   !
   vlan 100
      rd 65102:10100
      route-target both 1:1
      redistribute learned
   !
   address-family evpn
      bgp next-hop-unchanged
      neighbor 10.1.0.1 activate
      neighbor 10.1.0.3 activate
   !
   address-family ipv4
      no neighbor 10.1.0.1 activate
      no neighbor 10.1.0.3 activate
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
   vxlan vlan 100 vni 10100
!
```

**DC1-LEAF-1C**
```
!
router bgp 65103
   router-id 10.0.0.3
   timers bgp 5 15
   maximum-paths 2
   neighbor 10.1.0.1 remote-as 65101
   neighbor 10.1.0.1 update-source Loopback1
   neighbor 10.1.0.1 ebgp-multihop 3
   neighbor 10.1.0.1 send-community extended
   neighbor 10.1.0.2 remote-as 65102
   neighbor 10.1.0.2 update-source Loopback1
   neighbor 10.1.0.2 ebgp-multihop 3
   neighbor 10.1.0.2 send-community extended
   redistribute connected
   !
   vlan 100
      rd 65103:10100
      route-target both 1:1
      redistribute learned
   !
   address-family evpn
      bgp next-hop-unchanged
      neighbor 10.1.0.1 activate
      neighbor 10.1.0.2 activate
   !
   address-family ipv4
      no neighbor 10.1.0.1 activate
      no neighbor 10.1.0.2 activate
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
   vxlan vlan 100 vni 10100
!
```

Проверка IP-связанности между хостами:
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_11/images/ping_vpc6.JPG)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_11/images/ping_vpc7.JPG)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_11/images/ping_vpc8.JPG)
