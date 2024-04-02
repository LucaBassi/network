## Configure DHCP

```
ip dhcp excluded-address 192.168.10.1 192.168.10.3
ip dhcp excluded-address 10.0.40.254

ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1

```

## Configure DHCPv6

```
! SLAAC :

ipv6 unicast-routing

! POOL

ipv6 dhcp pool NAME
 address prefix 2001:1111:1111:1111::/64
 dns-server 2001:4860:4860::8888
 domain-name cpnv.local

! Stateless :

interface GigabitEthernet1/0
 ipv6 nd other-config-flag
 ipv6 dhcp server NAME

! Statefull :

interface GigabitEthernet1/0
 ipv6 nd managed-config-flag
 ipv6 nd prefix default no-autoconfig
 ipv6 dhcp server NAME

! Relay ipv6

interface gigabitethernet 0/0/1
 ipv6 dhcp relay destination 2001:db8:acad:1::2 G0/0/0

```
