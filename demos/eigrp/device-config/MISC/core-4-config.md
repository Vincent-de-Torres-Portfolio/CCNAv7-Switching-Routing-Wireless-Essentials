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
 router-id 43.43.43.43
 passive-interface default
 no passive-interface GigabitEthernet1/0/1
 no passive-interface GigabitEthernet1/0/2
 no passive-interface GigabitEthernet1/0/22
 no passive-interface GigabitEthernet1/0/23
 no passive-interface GigabitEthernet1/0/24
 network 10.100.0.28 0.0.0.3  
 network 10.100.0.24 0.0.0.3
 network 10.100.0.48 0.0.0.3
 network 10.100.0.52 0.0.0.3
 network 10.100.0.44 0.0.0.3
 exit
