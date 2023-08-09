conf t
!
interface GigabitEthernet0/0
 ip address 172.20.100.1 255.255.255.252
 no shutdown
!
interface GigabitEthernet0/1
 ip address 172.20.100.5 255.255.255.252
 no shutdown
!

router ospf 1
 router-id 1.1.1.1
 passive-interface default
 no passive-interface GigabitEthernet0/0
 no passive-interface GigabitEthernet0/1
 network 172.20.100.0 0.0.0.255 area 0

conf t
!
ip route 0.0.0.0 0.0.0.0 10.10.10.1
!
router ospf 1
default-information originate
!
end
