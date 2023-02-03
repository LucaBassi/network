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


Spanning Tree 

```
Interface g 0/0
 switchport access vlan 10
 switchport mode access
 switchport nonegotiate
 spanning-tree portfast
 spanning-tree bpduguard enable

```


Enable rapid PVST

```
Conf t
Spanning-tree mode rapid-pvst
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


