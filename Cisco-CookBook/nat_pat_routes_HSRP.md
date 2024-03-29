## NAT

```
ZT-SP-RTR01(config)#ip access-list standard nat-clients
ZT-SP-RTR01(config-std-nacl)#permit 192.168.10.0 0.0.0.255
ZT-SP-RTR01(config)#ip nat inside source list nat-clients interface GigabitEthernet0/0 overload

ZT-SP-RTR01(config)#int g0/0
ZT-SP-RTR01(config-if)#ip nat outside
ZT-SP-RTR01(config)#int g0/1
ZT-SP-RTR01(config-if)#ip nat inside
```

## PAT

```
ZT-SP-RTR01(config)#ip nat inside source static tcp ip_serveur_web 80 ip_interface_wan 80
```

## Route

```
ZT-SP-RTR01(config)#Ip route 0.0.0.0 0.0.0.0 gi0/0
```


## Router On a Stick

```
interface g0/1
no shutdown
interface g0/1.10
encapsulation dot1q 10
ip address 172.20.12.1 255.255.255.128
interface g0/1.20
encapsulation dot1q 20
ip address 172.20.12.193 255.255.255.224
interface g0/1.30
encapsulation dot1q 30
ip address 172.20.12.129 255.255.255.192
interface g0/1.90
encapsulation dot1q 90
ip address 172.20.12.225 255.255.255.240
interface g0/1.95
encapsulation dot1q 95 native
```

## OSPF et EIGRP

```
Int g0/0
 Ip address 10.0.0.29 255.255.255.252
 No shutdown

Int g1/0
 no switchport
 Ip address 10.0.0.33 255.255.255.252

router eigrp 1
 network 10.0.0.4 0.0.0.3
 network 10.0.0.12 0.0.0.3
 network 10.0.0.16 0.0.0.3
 network 10.0.0.28 0.0.0.3
 network 10.0.0.32 0.0.0.3
 no auto-summary
 exit
or
router ospf 1
 network 10.0.0.4 0.0.0.3 area 0
 network 10.0.0.12 0.0.0.3 area 0
 network 10.0.0.16 0.0.0.3 area 0
 network 10.0.0.28 0.0.0.3 area 0
 network 10.0.0.32 0.0.0.3 area 0
exit

```

## OSPF DR/BDR Election
```
R1(config)#interface fastEthernet 0/0
R1(config-if)#ip ospf priority 200
```

## OSPF Hello and Dead Interval
```
(config)#interface FastEthernet 0/0
(config-if)#ip ospf hello-interval 1 
(config-if)#ip ospf dead-interval 3
```

## Passive interface
```
R1(config)#router ospf 1
R1(config-router)#passive-interface FastEthernet 0/1
```

## Adjust cost for 10Go/s
```
auto-cost reference-bandwidth 10000
```


## Adjust cost per interface
```
R1(config)# interface g0/0/1
R1(config-if)# ip ospf cost 30
R1(config-if)# interface lo0
R1(config-if)# ip ospf cost 10
R1(config-if)# end
R1#
```

## Clear OSPF
```
R1# clear ip ospf process
Reset ALL OSPF processes? [no]: y
```
```
clear ip ospf [pid] {process | redistribution | counters [neighbor [intf] [nbr-id]]}
Syntax Description pid (Optional) Process ID.

process
	
Reset OSPF process.
redistribution

Clear OSPF route redistribution.
counters

OSPF counters.
neighbor

(Optional) Neighbor statistics per interface.
intf

(Optional) Neighbor interface.
nbr-id
	
(Optional) Neighbor ID.

```

## HSRP 
### groups are assigned the same number as the VLAN counterpart

```
interface Vlan 10
 ip address 10.0.10.2 255.255.255.0
 standby 10 ip 10.0.10.1
 standby 10 priority 120
 standby 10 preempt
no shutdown

interface Vlan 10
 ip address 10.0.10.3 255.255.255.0
 standby 10 ip 10.0.10.1
no shutdown
```



## SNMP

```
snmp-server community SNMPstring RO
snmp-server host 172.16.10.6 version 2c SNMPstring
snmp-server enable traps
```


