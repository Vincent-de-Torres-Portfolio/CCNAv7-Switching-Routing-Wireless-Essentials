 enable

configure terminal

interface Vlan10
 ip address 10.1.10.1 255.255.255.0
 exit

interface Vlan20
 ip address 10.1.20.1 255.255.255.0
 exit

interface Vlan30
 ip address 10.1.30.1 255.255.254.0
 exit

interface Vlan40
 ip address 10.1.40.1 255.255.254.0
 exit

interface Vlan50
 ip address 10.1.50.1 255.255.252.0
 exit

interface Vlan60
 ip address 10.1.60.1 255.255.248.0
 exit

interface Vlan70
 ip address 10.1.70.1 255.255.252.0
 exit

end

write memory
 
 vtp mode server
vtp domain HQ-B1

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

interface range GigabitEthernet0/1 - 2
  channel-group 1 mode active
  switchport trunk encapsulation dot1q
  switchport mode trunk
  switchport trunk native vlan 10
  switchport trunk allowed vlan 10,20,30,40,50,60,70


int po1
switchport mode trunk
  switchport trunk native vlan 10
  switchport trunk allowed vlan 10,20,30,40,50,60,70

int vlan 20 
ip address 10.1.20.110 255.255.255.0

end


 enable
configure terminal

ip dhcp pool VLAN20_POOL
 network 10.1.20.0 255.255.255.0
 default-router 10.1.20.1
 dns-server 10.1.30.100
 exit
ip dhcp excluded-address 10.1.20.1 10.1.20.100

ip dhcp pool VLAN30_POOL
 network 10.1.30.0 255.255.254.0
 default-router 10.1.30.1
 dns-server 10.1.30.100
 exit
ip dhcp excluded-address 10.1.30.1 10.1.30.100

ip dhcp pool VLAN40_POOL
 network 10.1.40.0 255.255.254.0
 default-router 10.1.40.1
 dns-server 10.1.30.100
 exit
ip dhcp excluded-address 10.1.40.1 10.1.40.100

ip dhcp pool VLAN50_POOL
 network 10.1.50.0 255.255.252.0
 default-router 10.1.50.1
 dns-server 10.1.30.100
 exit
ip dhcp excluded-address 10.1.50.1 10.1.50.100

ip dhcp pool VLAN60_POOL
 network 10.1.60.0 255.255.248.0
 default-router 10.1.60.1
 dns-server 10.1.30.100
 exit
ip dhcp excluded-address 10.1.60.1 10.1.60.100

ip dhcp pool VLAN70_POOL
 network 10.1.70.0 255.255.252.0
 default-router 10.1.70.1
 dns-server 10.1.30.100
 exit
ip dhcp excluded-address 10.1.70.1 10.1.70.100

end

write memory


enable
configure terminal

interface range GigabitEthernet 1/0/1 - 2
  channel-group 1 mode active
  switchport trunk encapsulation dot1q
  switchport mode trunk
  switchport trunk native vlan 10
  switchport trunk allowed vlan 10,20,30,40,50,60,70


int po1
switchport mode trunk
  switchport trunk native vlan 10
  switchport trunk allowed vlan 10,20,30,40,50,60,70

end
 