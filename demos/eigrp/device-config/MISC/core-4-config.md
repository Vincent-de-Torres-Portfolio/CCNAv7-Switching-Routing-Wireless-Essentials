interface GigabitEthernet1/0/1
 no switchport
 ip address 10.100.0.30 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/2
 no switchport
 ip address 10.100.0.26 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/22
 no switchport
 ip address 10.100.0.50 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/23
 no switchport
 ip address 10.100.0.53 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/24
 no switchport
 ip address 10.100.0.45 255.255.255.252
 no shutdown
 exit
router eigrp 1
eigrp router-id 43.43.43.43
 passive-interface default
 no passive-interface GigabitEthernet1/0/1
 no passive-interface GigabitEthernet1/0/2
 no passive-interface GigabitEthernet1/0/22
 no passive-interface GigabitEthernet1/0/23
 no passive-interface GigabitEthernet1/0/24
 no passive-interface GigabitEthernet1/1/3
no passive-interface GigabitEthernet1/1/4

 network 10.100.0.28 0.0.0.3  
 network 10.100.0.24 0.0.0.3
 network 10.100.0.48 0.0.0.3
 network 10.100.0.52 0.0.0.3
 network 10.100.0.44 0.0.0.3
 network 10.100.100.8 0.0.0.3
network 10.100.100.20 0.0.0.3

 exit

 configure terminal
interface GigabitEthernet1/1/3
no switchport
ip address 10.100.100.10 255.255.255.252
no shutdown
exit
interface GigabitEthernet1/1/4
no switchport
ip address 10.100.100.22 255.255.255.252
no shutdown
exit
