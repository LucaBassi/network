# Cisco Commandes - CookBook

---
![picture 2](images/6d8a93d255369fc4f7e146203fa1ff215e598467d5d5774f4fc645f992f5156c.png)  

---
# Commandes de bases
```
!Définition d'un nom pour l'équipement réseau
hostname s2

!Désactive la résolution de nom
no ip domain-lookup

!Sécurise l'accès en mode priviliégié
enable secret cisco

## Commande Boot et dépannage

 Boot

S1(config)# **boot system flash:/c2960-lanbasek9-mz.150-2.SE/c2960-lanbasek9-mz.150-2.SE.bin**

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

Tous les mots de passe stockés encryptés

```
S1(config)#service password-encryption
```

Informations sur la version du logiciel Cisco IOS

```
S1#show version
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
### configuration SSH
```
login authentication console SSH

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

## Baniere et vlan poubelle
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
## Utilisateurs avancé

```
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
line con 0
```

Créer mot de passe

```
password motDePasseCONS
```

Associer mot de passe au login :

```
login
```

Accès console aux utilisateurs aaa authentication login console local

```
line con 0
```

