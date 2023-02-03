
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


debug aaa authentication
debug aaa authorization
debug radius
```
