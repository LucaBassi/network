# Vlans - Access - TRUNK
## Trunk allowed VLAN

Put interfaces in the correct vlans

```
Interface g 0/0
switchport access vlan 10
switchport mode access
switchport nonegotiate
spanning-tree portfast
spanning-tree bpduguard enable
no shutdown
exit

```


```
interface g0/1
NO switchport access vlan 10
no shutdown
SWITCHPORT MODE TRUNK
SWITCHPORT TRUNK ALLOWED VLAN 10,20,30,90,95
switchport trunk native vlan 95
```

## Vlan Voix

```
interface FastEthernet0/4
switchport access vlan 10
switchport mode access
switchport voice vlan 40
mls qos trust cos
```

## Vlan Natif

```
interface GigabitEthernet0/1
 switchport trunk native vlan 100
 switchport trunk allowed vlan 10,20,30
 switchport mode trunk
 switchport nonegotiate
```

## Trunk


```
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport nonegotiate
```


Enable rapid PVST

```
Conf t
Spanning-tree mode rapid-pvst
```

##Spanning Tree 

```
(config)#spanning-tree vlan vlan-id root [primary|secondary]

(config)#spanning-tree vlan vlan-id priority priority
```



## Negotiated Interface Modes

switchport mode access: 

```
Puts the interface (access port) into permanent nontrunking mode and negotiates to convert the link into a nontrunk link. The interface becomes a nontrunk interface, regardless of whether the neighboring interface is a trunk interface.
```

switchport mode dynamic auto: 

```
Makes the interface able to convert the link to a trunk link. The interface becomes a trunk interface if the neighboring interface is set to trunk or desirable mode. The default switchport mode for newer Cisco switch Ethernet interfaces is dynamic auto. Note that if two Cisco switches are left to the common default setting of auto, a trunk will never form.

```

switchport mode dynamic desirable:

```
 Makes the interface actively attempt to convert the link to a trunk link. The interface becomes a trunk interface if the neighboring interface is set to trunk, desirable, or auto mode. This is the default switchport mode on older switches, such as the Catalyst 2950 and 3550 Series switches.
switchport mode trunk: Puts the interface into permanent trunking mode and negotiates to convert the neighboring link into a trunk link. The interface becomes a trunk interface even if the neighboring interface is not a trunk interface.
```

switchport nonegotiate: 

```
Prevents the interface from generating DTP frames. You can use this command only when the interface switchport mode is access or trunk. You must manually configure the neighboring interface as a trunk interface to establish a trunk link.
```
