enable

configure terminal

! Configure interfaces with IP addresses
interface GigabitEthernet1/0/24
  no switchport
  ip address 10.100.0.9 255.255.255.252
  no shutdown
exit

interface GigabitEthernet1/0/23
  no switchport
  ip address 10.100.0.5 255.255.255.252
  no shutdown
exit

interface GigabitEthernet1/0/22
  no switchport
  ip address 10.100.0.18 255.255.255.252
  no shutdown
exit

interface GigabitEthernet1/0/2
  no switchport
  ip address 10.100.0.33 255.255.255.252
  no shutdown
exit

interface GigabitEthernet1/0/1
  no switchport
  ip address 10.100.0.29 255.255.255.252
  no shutdown
exit

! Configure EIGRP
router eigrp 1
  no auto-summary
  eigrp router-id 21.21.21.21
  passive-interface default
  no passive-interface GigabitEthernet1/1/1
  no passive-interface GigabitEthernet1/0/22
  no passive-interface GigabitEthernet1/0/1
  no passive-interface GigabitEthernet1/0/2
  no passive-interface GigabitEthernet1/0/23
  no passive-interface GigabitEthernet1/0/24
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
  ! Network for Gig1/0/23
  network 10.100.0.4 0.0.0.3   
  ! Network for Gig1/0/2
  network 10.100.0.32 0.0.0.3  
  ! Network for Gig1/0/1
  network 10.100.0.28 0.0.0.3  
  ! Network for Gig1/0/24
  network 10.100.0.8 0.0.0.3   
exit

end

copy run start



