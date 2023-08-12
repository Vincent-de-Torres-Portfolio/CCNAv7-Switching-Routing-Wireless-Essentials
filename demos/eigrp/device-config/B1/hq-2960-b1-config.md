
# HQ-2960-B1 Initial Configuration Script
```

! Common Settings
enable
configure terminal
no ip domain-lookup
hostname HQ-2960-B1
username admin secret cisco

line console 0
  logging synchronous
exit

conf t
  ip domain-name hqbranch.synapsetechnologies.com
  crypto key generate rsa general-keys modulus 1024

banner motd $Welcome to HQ Branch.$
enable secret cisco

line console 0
  password cisco
  login
exit

line vty 0 4
  login local
  transport input ssh
  ip ssh version 2
exit

ip default-gateway 10.1.20.1

# VLAN & VTP Configuration
enable
conf t
  vtp mode client
  vtp domain HQ-B1

  interface range GigabitEthernet 1/0/1 - 2
    switchport mode trunk
    switchport trunk native vlan 10
    switchport trunk allowed vlan 10,20,30,40,50,60,70
    channel-group 1 mode active

  interface Port-channel 1
    switchport mode trunk
    switchport trunk native vlan 10
    switchport trunk allowed vlan 10,20,30,40,50,60,70
exit
```
---
```CiscoIOS


! VLAN Interfaces Configuration
conf t
  interface Vlan10
    no shutdown
    exit

  interface Vlan20
    ip address 10.2.20.110 255.255.255.0
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
exit
```
---
```CiscoIOS
!Access Ports Configuration
conf t
  interface range FastEthernet 0/1, FastEthernet0/8-24
    switchport mode access
    shutdown

  interface FastEthernet0/2
    switchport mode access
    switchport access vlan 20

  interface FastEthernet0/3
    switchport mode access
    switchport access vlan 30

  interface FastEthernet0/4
    switchport mode access
    switchport access vlan 40

  interface FastEthernet0/5
    switchport mode access
    switchport access vlan 50

  interface FastEthernet0/6
    switchport mode access
    switchport access vlan 60

  interface FastEthernet0/7
    switchport mode access
    switchport access vlan 70
exit

```
