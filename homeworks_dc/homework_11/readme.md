# Построение Underlay сети(eBGP)
### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_03/images/topology.JPG)

Настройка Overlay на основе VxLAN EVPN для L2 связанности между клиентами:

**DC1-LEAF-1A**
```
!
router bgp 65101
   router-id 10.0.0.1
   timers bgp 5 15
   maximum-paths 2
   neighbor 10.1.0.3 remote-as 65103
   neighbor 10.1.0.3 update-source Loopback1
   neighbor 10.1.0.3 ebgp-multihop 3
   neighbor 10.1.0.3 send-community extended
   neighbor 10.2.1.0 remote-as 65000
   neighbor 10.2.1.0 bfd
   neighbor 10.2.2.0 remote-as 65000
   neighbor 10.2.2.0 bfd
   redistribute connected route-map rm-LOOPBACK0
   !
   vlan-aware-bundle PC
      rd 1.1.1.1:10100
      route-target both 1:1
      redistribute learned
      vlan 100
   !
   address-family evpn
      bgp next-hop-unchanged
      neighbor 10.1.0.3 activate
   !
   address-family ipv4
      no neighbor 10.1.0.3 activate
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
   vxlan vlan 100 vni 10100
!
```
