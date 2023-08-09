enable

configure terminal

interface GigabitEthernet1/0/1
 no switchport
 ip address 10.100.0.22 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/2
 no switchport
 ip address 10.100.0.34 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/22
 no switchport
 ip address 10.100.0.49 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/1/1
 no switchport
 ip address 10.100.0.37 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/1/2
 no switchport
 ip address 10.100.0.41 255.255.255.252
 no shutdown
 exit

end

write memory

enable

configure terminal

router eigrp 1
 eigrp router-id 34.34.34.34
 passive-interface default
 no passive-interface GigabitEthernet1/0/1
 no passive-interface GigabitEthernet1/0/2
 no passive-interface GigabitEthernet1/0/22
 no passive-interface GigabitEthernet1/1/1
 no passive-interface GigabitEthernet1/1/2

  ! GigabitEthernet1/0/1
  network 10.100.0.20 0.0.0.3   
 ! GigabitEthernet1/0/2
 network 10.100.0.32 0.0.0.3   
 ! GigabitEthernet1/0/22
 network 10.100.0.49 0.0.0.3   
 ! GigabitEthernet1/1/1
 network 10.100.0.37 0.0.0.3   
 ! GigabitEthernet1/1/2
 network 10.100.0.41 0.0.0.3   
 exit

end

write memory
