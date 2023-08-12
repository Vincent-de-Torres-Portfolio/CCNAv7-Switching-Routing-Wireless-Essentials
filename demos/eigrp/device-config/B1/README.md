# HQ-3650-B1 Initial Configuration

This document outlines the essential initial configuration steps to set up the HQ-3650-B1 network device for secure and efficient management.

## Common Settings

```cisco
enable
configure terminal
no ip domain-lookup
hostname HQ-3650-B1
username admin secret cisco

line console 0
logging synchronous
exit

conf t
ip domain-name hqbranch.synapsetechnologies.com
crypto key generate rsa modulus 1024

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
```

## VLAN and VTP Configuration

```cisco
enable
configure terminal

vtp mode server
vtp domain HQ-B1

vlan 10
 name NATIVE
vlan 20
 name MGT
...
vlan 70
 name WL-GST

interface range GigabitEthernet 0/1 - 2
  channel-group 1 mode active
  switchport trunk encapsulation dot1q
  switchport mode trunk
  switchport trunk native vlan 10
  switchport trunk allowed vlan 10,20,30,40,50,60,70

interface Port-channel 1
  switchport mode trunk
  switchport trunk native vlan 10
  switchport trunk allowed vlan 10,20,30,40,50,60,70

end
```

## VLAN Interfaces Configuration

```cisco
enable
configure terminal

interface Vlan10
 ip address 10.1.10.1 255.255.255.0
 exit

interface Vlan20
 ip address 10.1.20.1 255.255.255.0
 exit

...
(interface configurations for other VLANs)

end
```

## DHCP Pool Configuration

```cisco
enable
configure terminal

(ip dhcp pool configurations for each VLAN pool)

end
```

## Port-channel Configuration

```cisco
enable
configure terminal

(interface range GigabitEthernet 1/0/1 - 2 configurations)
(interface Port-channel 1 configurations)
(interface VLAN 20 configuration)

end
```

## EIGRP Configuration

Enable EIGRP routing protocol for dynamic routing:

```cisco
enable
configure terminal

ip routing
router eigrp 1
 router-id 1.1.1.1
 passive-interface default
 no auto-summary
 no passive-interface GigabitEthernet1/0/22
 no passive-interface GigabitEthernet1/1/1
 network 10.100.0.6 0.0.0.3
 network 10.100.0.2 0.0.0.3
 network 10.1.0.0 0.0.255.255
 exit

end
```
