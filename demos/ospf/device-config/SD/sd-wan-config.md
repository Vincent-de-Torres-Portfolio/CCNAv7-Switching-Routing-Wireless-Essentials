conf t
!
interface GigabitEthernet0/0
 ip address 172.30.100.1 255.255.255.252
 no shutdown
!
interface GigabitEthernet0/1
 ip address 172.30.100.5 255.255.255.252
 no shutdown
!
end

router ospf 1
 router-id 1.1.1.1
 passive-interface default
 no passive-interface GigabitEthernet0/0
 no passive-interface GigabitEthernet0/1
network 172.30.100.0 0.0.0.255 area 0
 