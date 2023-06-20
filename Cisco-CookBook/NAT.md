# NAT Dynamique 

## Créer access list pour le réseau 172.16.0.0/16
```
access-list 1 permit 172.16.0.0 0.0.255.255
```
## Créer le pool NAT qui utilise 2 IP dans l'espace de 209.165.200.232
```
ip nat pool natpool 209.165.200.232 209.165.200.233 netmask 255.255.255.252
```
## Que se passera-t-il si plus de deux appareils tentent d'accéder à l'internet ?
Il ne pourra pas sortir, ou forcer une des 2 machines a perdre son ip outside

"clear ip nat translation"  

## Créer la règle NAT qui va lier l'ACL et le pool
```
ip nat inside source list 1 pool natpool
```

## Définir les interfaces inside et outside
```
int Se0/0/0
ip nat outside
```
```
int Se0/0/1
ip nat inside
```
## Afficher la config NAT avec show ip nat translations
```
Access Server1 depuis L1 - 209.165.201.5
```
R2#show ip nat translations 
Pro  Inside global     Inside local       Outside local      Outside global
tcp 209.165.200.233:1025172.16.11.1:1025   209.165.201.5:80   209.165.201.5:80
```
# NAT Statique 
## Créer access list pour le réseau 172.16.0.0/16
```
access-list 1 permit 172.16.0.0 0.0.255.255
```
## Créer le pool NAT qui utilise 2 IP dans l'espace de 209.165.200.232
```
ip nat pool natpool 209.165.200.232 209.165.200.233 netmask 255.255.255.252
```
## Que se passera-t-il si plus de deux appareils tentent d'accéder à l'internet ?
Il ne pourra pas sortir, ou forcer une des 2 machines a perdre son ip outside
"clear ip nat translation"

## Créer la règle NAT qui va lier l'ACL et le pool
```
ip nat inside source list 1 pool natpool
```
## Définir les interfaces inside et outside
```
int Se0/0/0
ip nat outside
```
```
int Se0/0/1
ip nat inside
```
## Afficher la config NAT avec show ip nat translations
```
Access Server1 depuis L1 - 209.165.201.5
```
R2##show ip nat translations 
Pro  Inside global     Inside local       Outside local      Outside global
tcp 209.165.200.233:1025172.16.11.1:1025   209.165.201.5:80   209.165.201.5:80
