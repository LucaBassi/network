# Cisco_commandes_lbi



# Cisco Commandes - CookBook

---
![picture 2](images/6d8a93d255369fc4f7e146203fa1ff215e598467d5d5774f4fc645f992f5156c.png)  

---

## Commande Boot et dépannage

 Boot

S1(config)# **boot system flash:/c2960-lanbasek9-mz.150-2.SE/c2960-lanbasek9-mz.150-2.SE.bin**

## Commandes de bases

 Aide

```
S1>?
S1#?
```

Passer en mode privilégié

```
S1>enable
S1#|
```

Desactiver message d erreur console

```
S1># logging syncronious
```

Visualiser la configuration active (dans la RAM)

```
S1#show running-configuration ou show run
```

Visualiser la configuration de démarrage (dans la NVRAM)

```
S1#show startup-configuration ou show start
```

Parcourir les pages de configuration Touche « enter » : ligne par ligne
Touche « tabulation » : page par page

Revenir en arrière dans les niveaux configuration (utiliser de manière répétitive permet de quitter le CLI)

```
S1(config-line)#exit
S1(config)#|
```

Revenir à la racine des niveaux de configuration sans quitter

```
S1(config-line)#end
S1#|
```

##Configuration de base

Configurer le terminal

```
S1#configure terminal
S1(config)#|
```

Ajouter un nom d’hôte au switch

```
Switch(config)#hostname S1
S1(config)#|
```

Désactiver la recherche DNS

```
S1(config)#no ip domain-lookup
S1(config)#|
```

Adresse IP de gestion sur le VLAN par défaut (1)

```
S1(config)#interface vlan 1
S1(config-if)#ip address 192.168.1.1 255.255.255.0
S1(config-if)#no shutdown
S1(config-if)#end
```

Sécurisation de l’accès à distance Mot de passe de la console (en clair !)

```
S1(config)#line console 0
S1(config-line)#password ccisco
S1(config-line)#Login (pour l’activer !)
```

Mot de passe du mode privilégié (en clair !)

```
S1(config)#enable password ecisco
```

Mot de passe du mode privilégié (encrypté!)

```
S1(config)#enable secret ecisco
```

Configuration et mot de passe de telnet (en clair !)

```
S1(config)#line vty 0 15
S1(config-line)# password vcisco
```

Tous les mots de passe stockés encryptés

```
S1(config)#service password-encryption
```

Enregistrer la configuration Running config vers startup config

```
S1# copy running-config startup-config
```

ou

```
S1# write
```

Informations sur la version du logiciel Cisco IOS

```
S1#show version
```

---

##Cisco Switch - Sécurité

Sécurisé le mode privilège (#enable) pour tout le monde 

```markdown
Router(config)#enable algorithm-type sha256 secret motDePasseEN 
Router(config)#username root secret motDePasseROOT
Router(config)#username root privilege 15
```

Ajouter utilisateurs

```
Router(config)#username root secret motDePasseROOT
Router(config)#username root privilege 15
```

ou

```
Router(config)#username admin1 privilege 0 secret Study-CCNA1
Router(config)#username admin2 privilege 15 secret Study-CCNA2
Router(config)#username admin3 secret Study-CCNA3
```

- Level 0 – Zero-level access only allows five commands- logout, enable, disable, help and exit.
- Level 1 – User-level access allows you to enter in User Exec mode that provides very limited read-only access to the router.
- Level 15 – Privilege level access allows you to enter in Privileged Exec mode and provides complete control over the router.

Config custom :

```
Router(config)#username admin4 privilege 5 secret Study-CCNA4
Router(config)#privilege exec level 5 show running-config
```

Password enable mot de passe perso :

```
Router(config)#enable secret level 5 Study-CCNA5
```

Sécuriser Port console Se rendre sur le port console :

```
#line con 0
```

Créer mot de passe

```
#password motDePasseCONS
```

Associer mot de passe au login :

```
#login
```

Accès console aux utilisateurs aaa authentication login console local

```
line con 0
```

## Configuration avancée

login authentication console SSH

```

!Créer un utilisateur local pour l'accès SSH
user admin secret Pa$$w0rd

!Définir un nom de domane
ip domain-name cpnv.ch
! Créer un paire de clé de chiffrement
crypto key generate rsa 
4096

!Forcer les accès distant via SSH (le telnet sera désactivé)
line vty 0 4
transport input ssh
login local
exit

!Activer la version 2 du protocole SSH
ip ssh version 2

line console 0
logging synchronous
login local

exit 
enable secret Pa$$w0rd

#Connexion SSH
ssh -caes128-cbc -o KexAlgorithms=+diffie-hellman-group1-sha1 admin@172.16.10.1
```

```
switchport trunk encapsulation dot1q
switchport mode trunk
switchport nonegotiate
```

Put interfaces in the correct vlans

```
Interface g 0/0
switchport access vlan 10
switchport mode access
switchport nonegotiate
spanning-tree portfast
no shutdown
exit

```

Spanning Tree 

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

Router On a Stick

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

Trunk allowed VLAN

```
interface g0/1
NO switchport access vlan 10
no shutdown
SWITCHPORT MODE TRUNK
SWITCHPORT TRUNK ALLOWED VLAN 10,20,30,90,95
switchport trunk native vlan 95
```

Vlan Voix

```markdown
interface FastEthernet0/4
switchport access vlan 10
switchport mode access
switchport voice vlan 40
mls qos trust cos
```

Vlan Natif

```markdown
interface GigabitEthernet0/1
switchport trunk native vlan 100
switchport trunk allowed vlan 10,20,30
switchport mode trunk
switchport nonegotiate
```

Enable rapid PVST

```
Conf t
Spanning-tree mode rapid-pvst
```

Port Security

```
!Activez la sécurité des ports sur les ports Fast Ethernet 0/1 et 0/2.

interface range f0/1 – 2
switchport port-security

!Maximum pour qu'un seul périphérique puisse accéder aux port
switchport port-security maximum 1

!MAC address apprise dynamiquement et ajoutée
switchport port-security mac-address sticky

!Mode de violation -> log de sécurité est générée et paquets provenant de la source inconnue sont supprimés.
switchport port-security violation restrict

!Eteint le port lors d une violation
switchport port-security violation shutdown



!Surveillance des trames BPDU
spanning-tree bpduguard enable

```

HSRP groups are assigned the same number as the VLAN counterpart

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

Configure DHCP

```
ip dhcp excluded-address 10.0.10.1 10.0.10.3
ip dhcp excluded-address 10.0.20.1 10.0.20.3
ip dhcp excluded-address 10.0.30.1 10.0.30.3
ip dhcp excluded-address 10.0.40.1 10.0.40.3
ip dhcp excluded-address 10.0.40.254
ip dhcp pool VLAN10
network 10.0.10.0 255.255.255.0
default-router 10.0.10.1

```

Configure DHCPv6

```
SLAAC :

ipv6 unicast-routing

POOL

ipv6 dhcp pool NAME
 address prefix 2001:1111:1111:1111::/64
 dns-server 2001:4860:4860::8888
 domain-name cpnv.local

Stateless :

interface GigabitEthernet1/0
 ipv6 nd other-config-flag
 ipv6 dhcp server NAME

Statefull :

interface GigabitEthernet1/0
 ipv6 nd managed-config-flag
 ipv6 nd prefix default no-autoconfig
 ipv6 dhcp server NAME

Relay ipv6

R1(config)# interface gigabitethernet 0/0/1
R1(config-if)# ipv6 dhcp relay destination 2001:db8:acad:1::2 G0/0/0
R1(config-if)# exit
R1(config)#
```

SNMP

```
snmp-server community SNMPstring RO
snmp-server host 172.16.10.6 version 2c SNMPstring
snmp-server enable traps
```

## Spanning-Tree

```markdown

```

OSPF et EIGRP

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

NAT

```
ZT-SP-RTR01(config)#ip access-list standard nat-clients
ZT-SP-RTR01(config-std-nacl)#permit 192.168.10.0 0.0.0.255
ZT-SP-RTR01(config)#ip nat inside source list nat-clients interface GigabitEthernet0/0 overload

ZT-SP-RTR01(config)#int g0/0
ZT-SP-RTR01(config-if)#ip nat outside
ZT-SP-RTR01(config)#int g0/1
ZT-SP-RTR01(config-if)#ip nat inside
```

PAT

```
ZT-SP-RTR01(config)#ip nat inside source static tcp ip_serveur_web 80 ip_interface_wan 80
```

Route

```
ZT-SP-RTR01(config)#Ip route 0.0.0.0 0.0.0.0 gi0/0
```

RADIUS

```
hostname lbi-SW-2

aaa new-model

aaa group server radius RAD_SERVERS
 server-private 192.168.10.20 auth-port 1812 acct-port 1813 key cisco
ex

aaa authentication login default group RAD_SERVERS local
aaa authorization console
aaa authorization exec default group RAD_SERVERS local if-authenticated

username Admin privilege 15 secret cisco

ip radius source-interface loopback 0

interface Loopback0
 ip address 10.0.0.6 255.255.255.255
ex

interface GigabitEthernet0/1
no sw
 ip address 10.0.0.2 255.255.255.252
ex

router ospf 1
 router-id 10.0.0.6
 network 10.0.0.6 0.0.0.0 area 0
 network 10.0.0.0 0.0.0.3 area 0
ex
end

debug aaa authentication
debug aaa authorization
debug radius
```

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

### Negotiated Interface Modes

switchport mode access: 

```markdown
Puts the interface (access port) into permanent nontrunking mode and negotiates to convert the link into a nontrunk link. The interface becomes a nontrunk interface, regardless of whether the neighboring interface is a trunk interface.
```

switchport mode dynamic auto: 

```markdown
Makes the interface able to convert the link to a trunk link. The interface becomes a trunk interface if the neighboring interface is set to trunk or desirable mode. The default switchport mode for newer Cisco switch Ethernet interfaces is dynamic auto. Note that if two Cisco switches are left to the common default setting of auto, a trunk will never form.

```

switchport mode dynamic desirable:

```markdown
 Makes the interface actively attempt to convert the link to a trunk link. The interface becomes a trunk interface if the neighboring interface is set to trunk, desirable, or auto mode. This is the default switchport mode on older switches, such as the Catalyst 2950 and 3550 Series switches.
switchport mode trunk: Puts the interface into permanent trunking mode and negotiates to convert the neighboring link into a trunk link. The interface becomes a trunk interface even if the neighboring interface is not a trunk interface.
```

switchport nonegotiate: 

```markdown
Prevents the interface from generating DTP frames. You can use this command only when the interface switchport mode is access or trunk. You must manually configure the neighboring interface as a trunk interface to establish a trunk link.
```