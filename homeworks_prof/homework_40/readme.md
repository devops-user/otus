# IPSec over DmVPN

#### 1. Настроим GRE поверх IPSec между офисами Москва и С.-Петербург.
  * Настроим в качестве центра сертфикации маршрутизатор - R14 в офисе Москва:

**R14**
```
ip domain-name homelab.com
ip http server
crypto key generate rsa general-keys label CA exportable modulus 2048
crypto pki server CA 
  no shutdown
```

  * Запросим сертификат сами себе на R14:
```
crypto key generate rsa label VPN modulus 2048
crypto pki trustpoint VPN
  enrollment url http://1.1.14.14
  subject-name CN=R14,OU=VPN,O=Otus,C=RU 
  rsakeypair VPN
  revocation-check none

crypto pki authenticate VPN
crypto pki enroll VPN
```

  * Посмотрим статус:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_40/images/R14_cert.png)

  * Запросим сертификат на R18:

**R18**
```
crypto pki trustpoint VPN
 enrollment url http://1.1.14.14:80
 subject-name CN=R18,OU=VPN,O=Otus,C=RU
 source interface Loopback0
 rsakeypair VPN
 revocation-check none

crypto pki authenticate VPN
crypto pki enroll VPN
```

  * Посмотрим статус:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_40/images/R18_cert.png)

  * Настроим IPSEC и применим его на интерфейс Tunnel:

**R14**
```
crypto ikev2 proposal GRE_PHASE1 
 encryption aes-cbc-128
 integrity md5
 group 2
!
crypto ikev2 policy IKEV2 
 proposal GRE_PHASE1
!
crypto ikev2 profile GRE_PROFILE1
 match address local interface Loopback0
 match identity remote address 0.0.0.0 
 authentication remote rsa-sig
 authentication local rsa-sig
 pki trustpoint VPN
!
crypto ipsec transform-set IPSEC_TS esp-aes esp-md5-hmac 
 mode tunnel
!
crypto ipsec profile IPSEC_PROFILE
 set transform-set IPSEC_TS 
 set ikev2-profile GRE_PROFILE1
!
interface Tunnel0
 description GRE_to_SPB_R18
 ip address 172.16.0.14 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source 1.1.14.14
 tunnel destination 1.1.18.18
 tunnel protection ipsec profile IPSEC_PROFILE
end
```

**R18**
```
crypto ikev2 proposal GRE_PHASE1 
 encryption aes-cbc-128
 integrity md5
 group 2
!
crypto ikev2 policy IKEV2 
 proposal GRE_PHASE1
!
crypto ikev2 profile GRE_PROFILE1
 match address local interface Loopback0
 match identity remote address 0.0.0.0 
 authentication remote rsa-sig
 authentication local rsa-sig
 pki trustpoint VPN
!
crypto ipsec transform-set IPSEC_TS esp-aes esp-md5-hmac 
 mode tunnel
!
crypto ipsec profile IPSEC_PROFILE
 set transform-set IPSEC_TS 
 set ikev2-profile GRE_PROFILE1
!
interface Tunnel0
 description GRE_to_MSK_R14
 ip address 172.16.0.18 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source 1.1.18.18
 tunnel destination 1.1.14.14
 tunnel protection ipsec profile IPSEC_PROFILE
end
```

  * Запустим пинг и посмотрим на статус IPSEC:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_40/images/R14_gre_ipsec.png)
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_40/images/R18_gre_ipsec.png)
