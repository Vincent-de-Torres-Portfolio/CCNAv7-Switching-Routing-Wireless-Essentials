
# HQ-3650-B2

## Enable and Configure Basic Settings

```cisco

enable
configure terminal

! Disable DNS lookup and set hostname
no ip domain-lookup
hostname HQ-3650-B2

! Create admin user
username admin secret cisco

! Configure console settings
line console 0
 logging synchronous
 exit

! Configure domain name and generate RSA key
conf t
 ip domain-name hqbranch.synapsetechnologies.com
 crypto key generate rsa general-keys modulus 1024

! Set banner message and enable secret
banner motd $Welcome to HQ Branch.$
 enable secret cisco

! Configure console password
line console 0
 password cisco
 login
 exit

! Configure SSH for remote management
line vty 0 4
 login local
 transport input ssh
 ip ssh version 2
```

## VLAN Interfaces Configuration

```cisco
enable
configure terminal

interface Vlan10
 ip address 10.2.10.1 255.255.255.0
 exit

interface Vlan20
 ip address 10.2.20.1 255.255.255.0
 exit

interface Vlan30
 ip address 10.2.30.1 255.255.254.0
 exit

interface Vlan40
 ip address 10.2.40.1 255.255.254.0
 exit

interface Vlan50
 ip address 10.2.50.1 255.255.252.0
 exit

interface Vlan60
 ip address 10.2.60.1 255.255.248.0
 exit

interface Vlan70
 ip address 10.2.70.1 255.255.252.0
 exit

interface GigabitEthernet1/0/24
 no switchport
 ip address 10.100.0.10 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/1/1
 no switchport
 ip address 10.100.0.14 255.255.255.252
 no shutdown
 exit

end

write memory
```

## VLAN & VTP Configuration

```cisco
enable
configure terminal

vtp mode server
vtp domain HQ-B2

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

end

copy running start
```

## DHCP Configuration

```
ip dhcp pool VLAN20_POOL
 network 10.2.20.0 255.255.255.0
 default-router 10.2.20.1
 dns-server 10.2.30.100
 exit
ip dhcp excluded-address 10.2.20.1 10.2.20.100

ip dhcp pool VLAN30_POOL
 network 10.2.30.0 255.255.254.0
 default-router 10.2.30.1
 dns-server 10.2.30.100
 exit
ip dhcp excluded-address 10.2.30.1 10.2.30.100

ip dhcp pool VLAN40_POOL
 network 10.2.40.0 255.255.254.0
 default-router 10.2.40.1
 dns-server 10.2.30.100
 exit
ip dhcp excluded-address 10.2.40.1 10.2.40.100

ip dhcp pool VLAN50_POOL
 network 10.2.50.0 255.255.252.0
 default-router 10.2.50.1
 dns-server 10.2.30.100
 exit
ip dhcp excluded-address 10.2.50.1 10.2.50.100

ip dhcp pool VLAN60_POOL
 network 10.2.60.0 255.255.248.0
 default-router 10.2.60.1
 dns-server 10.2.30.100
 exit
ip dhcp excluded-address 10.2.60.1 10.2.60.100

ip dhcp pool VLAN70_POOL
 network 10.2.70.0 255.255.252.0
 default-router 10.2.70.1
 dns-server 10.2.30.100
 exit
ip dhcp excluded-address 10.2.70.1 10.2.70.100


```
## Routing Configuration

```cisco
enable
configure terminal

ip routing 
router eigrp 1
 router-id 2.2.2.2
 passive-interface default
 no auto-summary
 no passive-interface GigabitEthernet1/0/24
 no passive-interface GigabitEthernet1/1/1
 network 10.100.0.14 0.0.0.3
 network 10.100.0.10 0.0.0.3
 network 10.2.0.0 0.0.255.255
 exit

end

```
