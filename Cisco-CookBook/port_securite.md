# Port Security

```
!Activez la sécurité des ports sur les ports Fast Ethernet 0/1 et 0/2.

interface range f0/1 – 2
switchport port-security

!Maximum pour qu'un seul périphérique puisse accéder aux port
switchport port-security maximum 1

!MAC address apprise dynamiquement et ajoutée
switchport port-security mac-address sticky

!Les adresses MAC autorisées peuvent être fixées :
switchport port-security mac-address 0000.0000.0003

!Mode de violation -> log de sécurité est générée et paquets provenant de la source inconnue sont supprimés.
switchport port-security violation restrict

!Eteint le port lors d une violation
switchport port-security violation shutdown



!Surveillance des trames BPDU
spanning-tree bpduguard enable

```
