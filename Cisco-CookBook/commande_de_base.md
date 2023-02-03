# Commandes de bases
```
!Définition d'un nom pour l'équipement réseau
hostname s2

!Désactive la résolution de nom
no ip domain-lookup

!Sécurise l'accès en mode priviliégié
enable secret cisco

```
## Gestion a distance 
```

! Configure l'interface SVI pour la gestion à distance sur le port vlan 99
interface vlan 99
 ip address 192.168.1.2 255.255.255.0
 no shutdown
exit

! Configure la passerelle par défaut
ip default-gateway 192.168.1.254

! Sécurise par un mot de passe l'accès via le port console
line console 0
logging synchronous
password cisco
login

! Sécurise par un mot de passe l'accès distant via telnet
line vty 0 4
logging synchronous
password cisco
login
exit

! Chiffre les mots de passe dans le fichier de configuration
service password-encryption

```
## Baniere 
```

! Création d'un message de bannière
banner motd "Acces restreint au personel IT"

! Création d'un VLAN poubelle
vlan 100
name blackhole
exit

! Désactive et attribue toutes les interfaces au vlan poubelle
 interface range fa0/1 - 48
 switchport access vlan 100
 shutdown

interface range gi1/1/1 - 4
 switchport access vlan 100
shutdown

interface gi1/0/6
 no shutdown
 switchport access vlan 99
```