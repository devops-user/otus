# VxLAN. EVPN L3
### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_12/images/topology.JPG)

Настроим маршрутизацию в рамках Overlay между клиентами

**DC1-LEAF-1A**
```
!
vlan 100,200
!
vrf instance OTUS
!
interface Vlan100
   vrf OTUS
   ip address 192.168.100.1/24
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
   vxlan vrf OTUS vni 100200
!
ip routing vrf OTUS
!
router bgp 65101
   vrf OTUS
      rd 65101:100200
      route-target import evpn 100:200
      route-target export evpn 100:200
      router-id 10.0.0.1
      redistribute connected
!
```

**DC1-LEAF-1C**
```
vlan 200
!
vrf instance OTUS
!
interface Vlan200
   vrf OTUS
   ip address 192.168.200.1/24
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
   vxlan vrf OTUS vni 100200
!
ip routing vrf OTUS
!
router bgp 65103
   vrf OTUS
      rd 65103:100200
      route-target import evpn 100:200
      route-target export evpn 100:200
      router-id 10.0.0.3
      redistribute connected
!
```
