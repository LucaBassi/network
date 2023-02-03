VPN Accès distant

```
aaa new-model

aaa authentication login vpn-authen local
aaa authorization network vpn-autho local

username johndoe password 0 paSSword

crypto isakmp policy 1
encryption aes
hash sha
authentication pre-share
exit
crypto isakmp client configuration group RA-VPN
key cisco
pool vpnpool
exit

crypto ipsec transform-set vpn-ad esp-3des esp-md5-hmac
crypto dynamic-map dmap 1
set transform-set vpn-ad
reverse-route
exit

crypto map VPN-MAP client authentication list vpn-authen
crypto map VPN-MAP isakmp authorization list vpn-autho
crypto map VPN-MAP client configuration address respond
crypto map VPN-MAP 1 ipsec-isakmp dynamic dmap

ip local pool vpnpool 192.168.0.200 192.168.0.250

----------------------------------------------------------

aaa authorization login VPN local
aaa authorization network VPN local

username johndoe password 0 paSSword

crypto isakmp policy 1
 encr 3des
 authentication pre-share
 group 2
ex

crypto isakmp client configuration group VPNGROUP
 key secret
 dns 8.8.8.8
 pool VPNRAPOOL
 acl VPN_SPLIT
ex

ip local pool VPNRAPOOL 10.100.3.1 10.100.3.254

ip access-list extended VPN_SPLIT
  permit ip 10.100.0.0 0.0.255.255 10.100.3.0 0.0.0.255
ex

crypto ipsec transform-set T1 esp-3des esp-sha-hmac

crypto dynamic-map DYNMAP 10
 set transform-set T1
 reverse-route
ex

crypto map VPN client authentication list VPN
crypto map VPN isakmp authorization list VPN
crypto map VPN client configuration address respond
crypto map VPN 10 ipsec-isakmp dynamic DYNMAP

interface GigabitEthernet0/0
 description Internet
 ip address dhcp
 speed auto
 crypto map VPN
```
