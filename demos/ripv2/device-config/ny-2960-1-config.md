# RIPv2: NY-2960-1

## RIPv2 Configuration: NY-2960-1 Device Setup Guide

In this comprehensive guide, we'll walk you through each step to configure your Cisco Catalyst 2960 switch, NY-2960-1, ensuring optimal performance and security. The following sections detail the configurations required for each aspect of the device's setup.

## 1. Spanning Tree and Trunk Interfaces

This section configures the Rapid Per-VLAN Spanning Tree (RPVST) mode and trunk interfaces for inter-VLAN communication.

### Explanation:
- RPVST mode is enabled using the `spanning-tree mode rapid-pvst` command.
- Interfaces G0/1 and G0/2 are configured as trunk interfaces to allow multiple VLAN traffic.
- Native VLAN for trunk interfaces is set to VLAN 10.
- VLANs allowed on trunk interfaces are specified using `switchport trunk allowed vlan`.

### Configuration:
```plaintext
spanning-tree mode rapid-pvst

int range g0/1-2
switchport mode trunk
switchport trunk native vlan 10
switchport trunk allowed vlan 10,20,30,40,50,60,70
```

## 2. Router Initialization and Security Enhancement

This section focuses on initializing the switch and enhancing its security settings.

### Explanation:
- Start by enabling privileged EXEC mode using the `enable` command.
- Set the hostname to "NY-2960-1" with the `hostname` command.
- Create an "admin" user with the password "cisco" using the `username` command.
- Configure console line settings for synchronous logging.
- Configure the router's domain name and generate RSA keys for enhanced security.
- Display a custom banner message to welcome users and highlight security.
- Set the enable secret password as "cisco" using `enable secret`.

### Configuration:
```plaintext
enable
configure terminal

hostname NY-2960-1
username admin secret cisco

line console 0
logging synchronous
exit

conf t
ip domain-name ny.synapsetechnologies.com
crypto key generate rsa general-keys modulus 1024
banner motd $Welcome to NY Branch.$
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
```

## 3. VTP Configuration and VLAN Interfaces

This section covers Virtual Trunking Protocol (VTP) configuration and VLAN interface settings.

### Explanation:
- Set the VTP mode to "client" and the domain to "NY" using the `vtp mode` and `vtp domain` commands.
- Configure VLAN interfaces and set IP addresses for VLANs 10, 20, 30, 40, 50, 60, and 70.

### Configuration:
```plaintext
vtp mode client
vtp domain NY

interface vlan 10
interface vlan 20
ip address 192.168.20.110 255.255.255.0

interface vlan 30
interface vlan 40
interface vlan 50
interface vlan 60
interface vlan 70
exit
```

## 4. Default Gateway and Access Ports

This section sets the default gateway and configures access ports for specific VLANs.

### Explanation:
- Set the default gateway IP address using the `ip default-gateway` command.
- Configure access ports for VLANs 20, 30, 40, 50, 60, and 70.
- Enable PortFast and BPDU Guard on each access port to enhance security.

### Configuration:
```plaintext
ip default-gateway 192.168.20.1

interface FastEthernet0/2
switchport mode access
switchport access vlan 20
spanning-tree portfast
spanning-tree bpduguard enable
exit

interface FastEthernet0/3
switchport mode access
switchport access vlan 30
spanning-tree portfast
spanning-tree bpduguard enable
exit

interface FastEthernet0/4
switchport mode access
switchport access vlan 40
spanning-tree portfast
spanning-tree bpduguard enable
exit

interface FastEthernet0/5
switchport mode access
switchport access vlan 50
spanning-tree portfast
spanning-tree bpduguard enable
exit

interface FastEthernet0/6
switchport mode access
switchport access vlan 60
spanning-tree portfast
spanning-tree bpduguard enable
exit

interface FastEthernet0/7
switchport mode access
switchport access vlan 70
spanning-tree portfast
spanning-tree bpduguard enable
exit
```

## 5. Shutdown and Miscellaneous Configurations

This section shuts down specified ports and finalizes any remaining configurations.

### Explanation:
- Shut down port FastEthernet0/1 and ports from FastEthernet0/8 to 24.
- Set access mode and disable Dynamic Trunking Protocol (DTP).

### Configuration:
```plaintext
interface FastEthernet0/1
shutdown
switchport mode access
switchport nonegotiate
exit

interface range FastEthernet0/8 - 24
switchport mode access
switchport nonegotiate
shutdown
exit
```


## NY-2960-1 Configuration Script

``` CiscoIOS

enable
configure terminal

! Spanning Tree and Trunk Interfaces
spanning-tree mode rapid-pvst
int range g0/1-2
switchport mode trunk
switchport trunk native vlan 10
switchport trunk allowed vlan 10,20,30,40,50,60,70
exit

! Router Initialization and Security Enhancement
enable
configure terminal
hostname NY-2960-1
username admin secret cisco
line console 0
logging synchronous
exit
conf t
ip domain-name ny.synapsetechnologies.com
crypto key generate rsa general-keys modulus 1024
banner motd $Welcome to NY Branch.$
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

! VTP Configuration and VLAN Interfaces
vtp mode client
vtp domain NY
interface vlan 10
interface vlan 20
ip address 192.168.20.110 255.255.255.0
interface vlan 30
interface vlan 40
interface vlan 50
interface vlan 60
interface vlan 70
exit

! Default Gateway and Access Ports
ip default-gateway 192.168.20.1
interface FastEthernet0/2
switchport mode access
switchport access vlan 20
spanning-tree portfast
spanning-tree bpduguard enable
exit
interface FastEthernet0/3
switchport mode access
switchport access vlan 30
spanning-tree portfast
spanning-tree bpduguard enable
exit
interface FastEthernet0/4
switchport mode access
switchport access vlan 40
spanning-tree portfast
spanning-tree bpduguard enable
exit
interface FastEthernet0/5
switchport mode access
switchport access vlan 50
spanning-tree portfast
spanning-tree bpduguard enable
exit
interface FastEthernet0/6
switchport mode access
switchport access vlan 60
spanning-tree portfast
spanning-tree bpduguard enable
exit
interface FastEthernet0/7
switchport mode access
switchport access vlan 70
spanning-tree portfast
spanning-tree bpduguard enable
exit

! Shutdown and Miscellaneous Configurations
interface FastEthernet0/1
shutdown
switchport mode access
switchport nonegotiate
exit
interface range FastEthernet0/8 - 24
switchport mode access
switchport nonegotiate
shutdown
exit

end
write memory
```