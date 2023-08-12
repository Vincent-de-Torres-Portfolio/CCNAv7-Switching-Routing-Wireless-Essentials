# HQ-2960-B3 Configuration

### Basic Settings

```cisco
enable
configure terminal

! Disable DNS lookup and set hostname
no ip domain-lookup
hostname HQ-2960-B3

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

### VTP and Trunk Configuration

```cisco
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
 exit
```

### VLAN Interfaces Configuration

```cisco
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
```

### Access Ports Configuration

```cisco
conf t
interface range fa 0/1, fa0/8-24
 switchport mode access
 shutdown
 exit

interface FastEthernet0/2
 switchport mode access
 switchport access vlan 20
 exit

interface FastEthernet0/3
 switchport mode access
 switchport access vlan 30
 exit

interface FastEthernet0/4
 switchport mode access
 switchport access vlan 40
 exit

interface FastEthernet0/5
 switchport mode access
 switchport access vlan 50
 exit

interface FastEthernet0/6
 switchport mode access
 switchport access vlan 60
 exit

interface FastEthernet0/7
 switchport mode access
 switchport access vlan 70
 exit

end

write memory
```
### Default Gateway
```
enable
configure terminal

ip default-gateway 10.3.20.1

end

copy running-config startup-config
```
