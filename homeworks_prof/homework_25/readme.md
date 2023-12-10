# Настройка eBGP

**R14**  
Настроим секцию для BGP:
```
!
router bgp 1001
 bgp router-id 1.1.14.14
 bgp log-neighbor-changes
 neighbor 2002:5555::12 remote-as 101
 neighbor 2002:5555::12 description Kitorn
 neighbor 85.75.123.37 remote-as 101
 neighbor 85.75.123.37 description Kitorn
 !
 address-family ipv4
  redistribute connected
  no neighbor 2002:5555::12 activate
  neighbor 85.75.123.37 activate
 exit-address-family
 !
 address-family ipv6
  redistribute connected
  neighbor 2002:5555::12 activate
 exit-address-family
!
```

**R15**  
Настроим секцию для BGP:
```
!
router bgp 1001
 bgp router-id 1.1.15.15
 bgp log-neighbor-changes
 neighbor 2002:5555::14 remote-as 301
 neighbor 2002:5555::14 description Lamas
 neighbor 85.75.123.33 remote-as 301
 neighbor 85.75.123.33 description Lamas
 !
 address-family ipv4
  redistribute connected
  no neighbor 2002:5555::14 activate
  neighbor 85.75.123.33 activate
 exit-address-family
 !
 address-family ipv6
  redistribute connected
  neighbor 2002:5555::14 activate
 exit-address-family
!
```

**R18**  
Настроим секцию для BGP:
```
!
router bgp 2042
 bgp router-id 1.1.18.18
 bgp log-neighbor-changes
 neighbor 2002:5555::14 remote-as 520
 neighbor 2002:5555::14 description Triada
 neighbor 85.75.123.25 remote-as 520
 neighbor 85.75.123.25 description Triada
 !
 address-family ipv4
  redistribute connected
  no neighbor 2002:5555::14 activate
  neighbor 85.75.123.25 activate
 exit-address-family
 !
 address-family ipv6
  redistribute connected
  neighbor 2002:5555::14 activate
 exit-address-family
!
```

**R21**  
Настроим секцию для BGP:
```
!
router bgp 301
 bgp router-id 1.1.21.21
 bgp log-neighbor-changes
 neighbor 2002:5555::6 remote-as 520
 neighbor 2002:5555::6 description Triada
 neighbor 2002:5555::8 remote-as 101
 neighbor 2002:5555::8 description Kitorn
 neighbor 2002:5555::15 remote-as 1001
 neighbor 2002:5555::15 description Moscow
 neighbor 85.75.123.5 remote-as 520
 neighbor 85.75.123.5 description Triada
 neighbor 85.75.123.29 remote-as 101
 neighbor 85.75.123.29 description Kitorn
 neighbor 85.75.123.34 remote-as 1001
 neighbor 85.75.123.34 description Moscow
 !
 address-family ipv4
  redistribute connected
  no neighbor 2002:5555::6 activate
  no neighbor 2002:5555::8 activate
  no neighbor 2002:5555::15 activate
  neighbor 85.75.123.5 activate
  neighbor 85.75.123.29 activate
  neighbor 85.75.123.34 activate
 exit-address-family
 !
 address-family ipv6
  redistribute connected
  neighbor 2002:5555::6 activate
  neighbor 2002:5555::8 activate
  neighbor 2002:5555::15 activate
 exit-address-family
!
```

**R22**  
Настроим секцию для BGP:
```
!
router bgp 101
 bgp router-id 1.1.22.22
 bgp log-neighbor-changes
 neighbor 2002:5555::9 remote-as 301
 neighbor 2002:5555::9 description Lamas
 neighbor 2002:5555::11 remote-as 520
 neighbor 2002:5555::11 description Triada
 neighbor 2002:5555::13 remote-as 1001
 neighbor 2002:5555::13 description Moscow
 neighbor 85.75.123.1 remote-as 520
 neighbor 85.75.123.1 description Triada
 neighbor 85.75.123.30 remote-as 301
 neighbor 85.75.123.30 description Lamas
 neighbor 85.75.123.38 remote-as 1001
 neighbor 85.75.123.38 description Moscow
 !
 address-family ipv4
  redistribute connected
  no neighbor 2002:5555::9 activate
  no neighbor 2002:5555::11 activate
  no neighbor 2002:5555::13 activate
  neighbor 85.75.123.1 activate
  neighbor 85.75.123.30 activate
  neighbor 85.75.123.38 activate
 exit-address-family
 !
 address-family ipv6
  redistribute connected
  neighbor 2002:5555::9 activate
  neighbor 2002:5555::11 activate
  neighbor 2002:5555::13 activate
 exit-address-family
!
```

**R23**  
Настроим секцию для BGP:
```
!
router bgp 520
 bgp router-id 1.1.23.23
 bgp log-neighbor-changes
 neighbor 2002:5555::10 remote-as 101
 neighbor 2002:5555::10 description Kitorn
 neighbor 85.75.123.2 remote-as 101
 neighbor 85.75.123.2 description Kitorn
 !
 address-family ipv4
  redistribute connected
  no neighbor 2002:5555::10 activate
  neighbor 85.75.123.2 activate
 exit-address-family
 !
 address-family ipv6
  redistribute connected
  neighbor 2002:5555::10 activate
 exit-address-family
!
```

**R24**  
Настроим секцию для BGP:
```
!
router bgp 520
 bgp router-id 1.1.24.24
 bgp log-neighbor-changes
 neighbor 2002:5555::7 remote-as 301
 neighbor 2002:5555::7 description Lamas
 neighbor 2002:5555::15 remote-as 2042
 neighbor 2002:5555::15 description SPB
 neighbor 85.75.123.6 remote-as 301
 neighbor 85.75.123.6 description Lamas
 neighbor 85.75.123.26 remote-as 2042
 neighbor 85.75.123.26 description SPB
 !
 address-family ipv4
  redistribute connected
  no neighbor 2002:5555::7 activate
  no neighbor 2002:5555::15 activate
  neighbor 85.75.123.6 activate
  neighbor 85.75.123.26 activate
 exit-address-family
 !
 address-family ipv6
  redistribute connected
  neighbor 2002:5555::7 activate
  neighbor 2002:5555::15 activate
 exit-address-family
!
```
