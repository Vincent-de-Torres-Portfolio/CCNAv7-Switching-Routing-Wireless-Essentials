# RIPv2: NY-3650-1

## RIPv2 Configuration: NY-3650-1 Device Setup Guide

In this comprehensive guide, I'll guide you through each step to configure your Cisco Catalyst 3650 router for optimal performance and security. The following sections detail the configurations needed for each aspect of the device's setup.

## 1. Router Initialization and Security Enhancement

This section pertains to the initialization and reinforcement of security settings on the NY-3650-1 router.

### Explanation:
- The process starts by enabling the privileged EXEC mode with the `enable` command.
- The router's hostname is set as "NY-3650-1" using the `hostname` command.
- User authentication is established through the creation of an "admin" user with a secret password "cisco" using the `username` command.
- Console line settings are configured for synchronous logging (`line console 0`, `logging synchronous`).
- The router's domain name is configured with `ip domain-name`, and RSA keys are generated with `crypto key generate rsa`.
- A custom banner message is crafted using `banner motd` to provide a welcome and highlight network security.
- The enable secret password is set as "cisco" with `enable secret`.

### Configuration:
```plaintext
enable
configure terminal

hostname NY-3650-1
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

## 2. VLAN and Interface Configurations

This section covers the setup of VLANs and interfaces, enabling communication between devices and networks.

### Explanation:
- Interfaces G1/0/1 to G1/0/4 are configured as trunk interfaces, allowing multiple VLAN traffic.
- The native VLAN for trunk interfaces is set to VLAN 10.
- VLANs are allowed on the trunk interfaces using `switchport trunk allowed vlan`.
- EtherChannel is created for interfaces G1/0/3 and G1/0/4 with channel group mode "active" using `channel-group`.

### Configuration:
```plaintext
int range g1/0/1-4
switchport mode trunk
switchport trunk native vlan 10
switchport trunk allowed vlan 10,20,30,40,50,60,70

int range g1/0/3-4
channel-group 1 mode active

int po 1
switchport mode trunk
switchport trunk native vlan 10
switchport trunk allowed vlan 10,20,30,40,50,60,70

int g1/0/24
no switchport
ip address 192.168.100.2 255.255.255.252
no shutdown

int g1/1/4
no switchport
no shutdown
ip address 192.168.100.9 255.255.255.252
```

## 3. VLAN Interface Configuration

This section configures VLAN interfaces, enabling routing between VLANs and subnets.

### Explanation:
- VLANs are created with appropriate names and numbers using the `vlan` command.
- VLAN interfaces are configured with IP addresses, standby IP addresses, priorities, and preemption settings.

### Configuration:
```plaintext
vtp mode server
vtp domain NY

vlan 10
 name NATIVE
vlan 20
 name MGT
... (add other VLAN configurations)

interface Vlan10
 ip address 192.168.10.1 255.255.255.0
 standby 10 ip 192.168.10.1
 standby 10 priority 110
 standby 10 preempt
 exit

interface Vlan20
 ip helper-address 192.168.100.1
 ip address 192.168.20.1 255.255.255.0
 standby 20 ip 192.168.20.1
 standby 20 priority 110
 standby 20 preempt
 exit

... (repeat for other VLAN interfaces)
```

## 4. Spanning Tree and Routing Configuration

This section configures spanning tree and implements the Routing Information Protocol (RIP) for routing between networks.

### Explanation:
- Rapid Per-VLAN Spanning Tree (RPVST) mode is enabled globally using `spanning-tree mode rapid-pvst`.
- The bridge priority is set for each VLAN using `spanning-tree vlan` (optional).
- IP routing is activated using `ip routing`.
- RIP is configured with version 2 and network addresses to advertise.

### Configuration:
```plaintext
spanning-tree mode rapid-pvst
spanning-tree vlan 10 root primary
... (add other spanning-tree commands)

ip routing
router rip
version 2
network 192.168.100.0
network 192.168.10.0
network 192.168.20.0
... (add other RIP network commands)
passive-interface default
no passive-interface G1/0/24
no passive-interface G1/1/4
no auto-summary
```

---

This guide meticulously outlines the steps to configure the Cisco Catalyst 3650 router (NY-3650-1). Each

 section provides a clear explanation of the configurations and their purpose, along with corresponding code blocks. Ensure that the configurations match your network requirements before implementation.
## NY-3650-1 Configuration Script

```
enable
configure terminal

! Set hostname and user credentials
hostname NY-3650-1
username admin secret cisco

! Configure console line
line console 0
logging synchronous
exit

! Configure domain, crypto keys, banner, and enable secret
conf t
ip domain-name ny.synapsetechnologies.com
crypto key generate rsa general-keys modulus 1024
banner motd $Welcome to NY Branch.$
enable secret cisco

! Configure console line password and SSH settings
line console 0
password cisco
login
exit

line vty 0 4
login local
transport input ssh
ip ssh version 2
exit

! Configure trunk interfaces
int range g1/0/1-4
switchport mode trunk
switchport trunk native vlan 10
switchport trunk allowed vlan 10,20,30,40,50,60,70
exit

! Configure EtherChannel
int range g1/0/3-4
channel-group 1 mode active
exit

int po 1
switchport mode trunk
switchport trunk native vlan 10
switchport trunk allowed vlan 10,20,30,40,50,60,70
exit

! Configure routed interfaces
int g1/0/24
no switchport
ip address 192.168.100.2 255.255.255.252
no shutdown
exit

int g1/1/4
no switchport
no shutdown
ip address 192.168.100.9 255.255.255.252
exit

! Configure VLANs and VLAN interfaces
vtp mode server
vtp domain NY

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

interface Vlan10
ip address 192.168.10.1 255.255.255.0
standby 10 ip 192.168.10.1
standby 10 priority 110
standby 10 preempt
exit

interface Vlan20
ip helper-address 192.168.100.1
ip address 192.168.20.1 255.255.255.0
standby 20 ip 192.168.20.1
standby 20 priority 110
standby 20 preempt
exit

interface Vlan30
ip helper-address 192.168.100.1
ip address 192.168.30.1 255.255.254.0
standby 30 ip 192.168.30.1
standby 30 priority 110
standby 30 preempt
exit

interface Vlan40
ip helper-address 192.168.100.1
ip address 192.168.40.1 255.255.254.0
standby 40 ip 192.168.40.1
standby 40 priority 110
standby 40 preempt
exit

interface Vlan50
ip helper-address 192.168.100.1
ip address 192.168.50.1 255.255.252.0
standby 50 ip 192.168.50.1
standby 50 priority 110
standby 50 preempt
exit

interface Vlan60
ip helper-address 192.168.100.1
ip address 192.168.60.1 255.255.248.0
standby 60 ip 192.168.60.1
standby 60 priority 110
standby 60 preempt
exit

interface Vlan70
ip helper-address 192.168.100.1
ip address 192.168.70.1 255.255.252.0
standby 70 ip 192.168.70.1
standby 70 priority 110
standby 70 preempt
exit

! Enable Rapid PVST globally
spanning-tree mode rapid-pvst

! Configure the bridge priority for each VLAN (optional)
spanning-tree vlan 10 root primary
spanning-tree vlan 20 root primary
spanning-tree vlan 30 root primary
spanning-tree vlan 40 root primary
spanning-tree vlan 50 root primary
spanning-tree vlan 60 root primary
spanning-tree vlan 70 root primary

! Enable IP routing and configure RIP
ip routing
router rip
version 2
network 192.168.100.0
network 192.168.10.0
network 192.168.20.0
network 192.168.30.0
network 192.168.40.0
network 192.168.50.0
network 192.168.60.0
network 192.168.70.0
passive-interface default
no passive-interface G1/0/24
no passive-interface G1/1/4
no auto-summary
exit

! Save configuration
write memory
```