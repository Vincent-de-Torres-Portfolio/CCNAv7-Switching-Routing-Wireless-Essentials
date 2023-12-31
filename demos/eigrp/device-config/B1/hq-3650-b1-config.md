
# HQ-3650-B1

## Enable and Configure Basic Settings

```cisco
enable
configure terminal

! Disable DNS lookup and set hostname
no ip domain-lookup
hostname HQ-3650-B1

! Create admin user
username admin secret cisco

! Configure console settings
line console 0
 logging synchronous
 exit

! Configure domain name and generate RSA key
conf t
 ip domain-name hqbranch.synapsetechnologies.com
 crypto key generate rsa modulus 1024

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

## Configure VLAN Interfaces

```cisco
configure terminal

interface Vlan10
 ip address 10.1.10.1 255.255.255.0
 exit

(interface configurations for other VLANs)

end

write memory
```

## Configure VTP Settings and VLANs

```cisco
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
```

## Configure EtherChannel for Trunk Links

```cisco
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
```

## Configure DHCP Pools for Each VLAN

```cisco
configure terminal

(ip dhcp pool configurations for each VLAN pool)

end

write memory
```

## Configure EtherChannel for Trunk Links on Another Set of Interfaces

```cisco
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
```

## Shutdown Unused Ports

To improve security and optimize network performance, it's recommended to shut down unused ports. Here's how you can do it:

```cisco
enable
configure terminal

! Shutdown unused GigabitEthernet ports
interface range GigabitEthernet1/0/3 - 21,GigabitEthernet1/0/23-24, GigabitEthernet1/1/2 - 4
shutdown
```

