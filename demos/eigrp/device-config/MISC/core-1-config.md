enable

configure terminal

interface GigabitEthernet1/1/1
 no switchport
 ip address 10.100.0.1 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/1/2
 no switchport
 ip address 10.100.0.13 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/22
 no switchport
 ip address 10.100.0.17 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/1
 no switchport
 ip address 10.100.0.21 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/2
 no switchport
 ip address 10.100.0.25 255.255.255.252
 no shutdown
 exit

end

write memory
enable

configure terminal

router eigrp 1
 no auto-summary
 router-id 12.12.12.12
 passive-interface default
 no passive-interface GigabitEthernet1/1/1
 no passive-interface GigabitEthernet1/0/22
 no passive-interface GigabitEthernet1/0/1
 no passive-interface GigabitEthernet1/0/2
 ! Network for Gig1/1/1
 network 10.100.0.0 0.0.0.3  
 ! Network for Gig1/1/2
 network 10.100.0.12 0.0.0.3  
 ! Network for Gig1/0/22
 network 10.100.0.16 0.0.0.3  
 ! Network for Gig1/0/1
 network 10.100.0.20 0.0.0.3  
 ! Network for Gig1/0/2
 network 10.100.0.24 0.0.0.3  
 exit

end

write memory
