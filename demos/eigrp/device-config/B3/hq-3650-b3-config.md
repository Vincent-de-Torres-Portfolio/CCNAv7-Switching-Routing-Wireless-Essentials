enable
configure terminal

interface Vlan10
 ip address 10.3.10.1 255.255.255.0
 exit

interface Vlan20
 ip address 10.3.20.1 255.255.255.0
 exit

interface Vlan30
 ip address 10.3.30.1 255.255.254.0
 exit

interface Vlan40
 ip address 10.3.40.1 255.255.254.0
 exit

interface Vlan50
 ip address 10.3.50.1 255.255.252.0
 exit

interface Vlan60
 ip address 10.3.60.1 255.255.248.0
 exit

interface Vlan70
 ip address 10.3.70.1 255.255.252.0
interface GigabitEthernet1/0/24
 no switchport
 ip address 10.100.0.54 255.255.255.252
 no shutdown
 exit
interface GigabitEthernet1/1/1

 no switchport
 ip address 10.100.0.38 255.255.255.252
 no shutdown
 exit

end

write memory

end

write memory

vtp mode server
vtp domain HQ-B3

vlan 10
 name NATIVE
vlan 20
 name MGT
vlan 30
 name SRV
vlan 40
 name IT
vlan 50
 name INT
vlan 60
 name WL-STF
vlan 70
 name WL-GST


 enable
configure terminal

interface range GigabitEthernet1/0/1 - 2
  switchport mode trunk
  switchport trunk native vlan 10
  switchport trunk allowed vlan 10,20,30,40,50,60,70
    channel-group 1 mode active


interface Port-channel1
  switchport mode trunk
  switchport trunk native vlan 10
  switchport trunk allowed vlan 10,20,30,40,50,60,70
  exit

<!-- interface Vlan20
 ip address 10.3.20.110 255.255.255.0
 exit -->

end

copy running start



enable
configure terminal

ip dhcp pool VLAN20_POOL
 network 10.3.20.0 255.255.255.0
 default-router 10.3.20.1
 dns-server 10.3.30.100
 exit
ip dhcp excluded-address 10.3.20.1 10.3.20.100

ip dhcp pool VLAN30_POOL
 network 10.3.30.0 255.255.254.0
 default-router 10.3.30.1
 dns-server 10.3.30.100
 exit
ip dhcp excluded-address 10.3.30.1 10.3.30.100

ip dhcp pool VLAN40_POOL
 network 10.3.40.0 255.255.254.0
 default-router 10.3.40.1
 dns-server 10.3.30.100
 exit
ip dhcp excluded-address 10.3.40.1 10.3.40.100

ip dhcp pool VLAN50_POOL
 network 10.3.50.0 255.255.252.0
 default-router 10.3.50.1
 dns-server 10.3.30.100
 exit
ip dhcp excluded-address 10.3.50.1 10.3.50.100

ip dhcp pool VLAN60_POOL
 network 10.3.60.0 255.255.248.0
 default-router 10.3.60.1
 dns-server 10.3.30.100
 exit
ip dhcp excluded-address 10.3.60.1 10.3.60.100

ip dhcp pool VLAN70_POOL
 network 10.3.70.0 255.255.252.0
 default-router 10.3.70.1
 dns-server 10.3.30.100
 exit
ip dhcp excluded-address 10.3.70.1 10.3.70.100

end

write memory

enable
configure terminal
ip routing 
router eigrp 1
 router-id 3.3.3.3
 passive-interface default
 no auto-summary
 no passive-interface GigabitEthernet1/0/24
 no passive-interface GigabitEthernet1/1/1
 network 10.100.0.38 0.0.0.3
 network 10.100.0.54 0.0.0.3
 network 10.3.0.0 0.0.255.255
 exit

end

write memory

