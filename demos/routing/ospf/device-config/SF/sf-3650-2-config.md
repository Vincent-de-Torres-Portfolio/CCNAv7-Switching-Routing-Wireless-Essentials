enable
configure terminal
no ip domain-lookup
hostname SF-3650-2
username admin secret cisco

line console 0
logging synchronous
exit
conf t
ip domain-name SF.ccna.com
crypto key generate rsa modulus 1024

banner motd $Welcome to SF branch.$
enable secret cisco

line console 0
password cisco
login
exit

line vty 0 4
login local
transport input ssh
ip ssh version 2


interface range g1/0/1-2
channel-group 1 mode passive

int po1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70
 
interface range g1/0/3-4
channel-group 2 mode active

int po2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70

interface range g1/0/5-6
channel-group 3 mode active

int po3
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70
 
interface Vlan10
 ip address 172.20.10.2 255.255.255.0
 standby 1 ip 172.20.10.1
 standby 1 preempt
 standby 1 priority 100

interface Vlan20
 ip address 172.20.20.2 255.255.255.0
 standby 1 ip 172.20.20.1
 standby 1 preempt
 standby 1 priority 100

interface Vlan30
 ip address 172.20.30.2 255.255.254.0
 standby 1 ip 172.20.30.1
 standby 1 preempt
 standby 1 priority 100

interface Vlan40
 ip address 172.20.40.2 255.255.254.0
 standby 1 ip 172.20.40.1
 standby 1 preempt
 standby 1 priority 100

interface Vlan50
 ip address 172.20.50.2 255.255.252.0
 standby 1 ip 172.20.50.1
 standby 1 preempt
 standby 1 priority 100

interface Vlan60
 ip address 172.20.60.2 255.255.248.0
 standby 1 ip 172.20.60.1
 standby 1 preempt
 standby 1 priority 100

interface Vlan70
 ip address 172.20.70.2 255.255.252.0
 standby 1 ip 172.20.70.1
 standby 1 preempt
 standby 1 priority 100


spanning-tree vlan 10 root secondary
spanning-tree vlan 20 root secondary
spanning-tree vlan 30 root secondary
spanning-tree vlan 40 root secondary
spanning-tree vlan 50 root secondary
spanning-tree vlan 60 root secondary
spanning-tree vlan 70 root secondary
 
ip routing 

router ospf 1
router-id 20.20.20.20  
passive-interface default
no passive-interface GigabitEthernet1/0/23
no passive-interface GigabitEthernet1/1/1
no passive-interface GigabitEthernet1/1/2
no passive-interface Vlan10
no passive-interface Vlan20
no passive-interface Vlan30
no passive-interface Vlan40
no passive-interface Vlan50
no passive-interface Vlan60
no passive-interface Vlan70

network 172.20.100.0 0.0.0.255 area 0
