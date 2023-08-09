
enable
configure terminal

vtp mode client
vtp domain HQ-B3

interface range GigabitEthernet 0/1 - 2
  switchport mode trunk
  switchport trunk native vlan 10
  switchport trunk allowed vlan 10,20,30,40,50,60,70
channel-group 1 mode passive



int po1
switchport mode trunk
  switchport trunk native vlan 10
  switchport trunk allowed vlan 10,20,30,40,50,60,70

end

enable
configure terminal

interface Vlan10
 no shutdown
 exit

interface Vlan20
 ip address 10.3.20.110 255.255.255.0
 exit

interface Vlan30
 no shutdown
 exit

interface Vlan40
 no shutdown
 exit

interface Vlan50
 no shutdown
 exit

interface Vlan60
 no shutdown
 exit

interface Vlan70
 no shutdown
 exit

end

write memory


conf t
!
!
interface range fa 0/1, fa0/8-24
switchport mode access
shutdown
!
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 20
!
interface FastEthernet0/3
 switchport mode access
 switchport access vlan 30
!
interface FastEthernet0/4
 switchport mode access
 switchport access vlan 40
!
interface FastEthernet0/5
 switchport mode access
 switchport access vlan 50
!
interface FastEthernet0/6
 switchport mode access
 switchport access vlan 60
!
interface FastEthernet0/7
 switchport mode access
 switchport access vlan 70
!
end
!
copy run start 
 