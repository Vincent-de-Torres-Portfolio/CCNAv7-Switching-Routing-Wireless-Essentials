# RIPv2: NY-2911

## RIPv2 Configuration: NY-2911 Device Setup Guide

In this guide, I'll walk you through the detailed yet straightforward steps to configure the NY-2911 Cisco router for optimal network functionality and security. The configuration process is divided into four main sections, each focusing on a crucial aspect of the router's setup.

## 1. Router Initialization and Security Enhancement

This section covers the essential steps to initialize the router's settings and enhance its security features.

### Explanation:
- We begin by enabling the router's privileged EXEC mode using the `enable` command.
- To set an identifiable hostname, we use the `hostname` command, naming it "NY-2911".
- User authentication is crucial. With the `username` command, we create an "admin" user with a secret password "cisco".
- The console line settings are configured to synchronize logs (`logging synchronous`) and prevent unauthorized access.
- Network security is elevated by setting up a domain name (`ip domain-name`) and generating RSA keys (`crypto key generate rsa`).
- A custom banner message (`banner motd`) welcomes users and emphasizes security.
- The enable password is fortified using `enable secret` with the password "cisco".
- The console line password "cisco" is set for user access (`line console 0`, `password cisco`).
- Secure remote access is enabled via SSH (`ip ssh version 2`, `transport input ssh`).

### Configuration:
```plaintext
enable
configure terminal

hostname NY-2911
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

## 2. Network Address Translation (NAT/PAT) Configuration

This section addresses the setup of Network Address Translation (NAT) to enable communication between private and public networks.

### Explanation:
- We configure NAT to allow devices from internal networks to communicate externally via a single public IP address.
- The router's interfaces are categorized as either "outside" (Serial) or "inside" (GigabitEthernet).
- We define a NAT pool, named "PAT-Pool," for address translation.
- Access lists (`access-list`) are set up to define which private IP addresses can undergo translation.
- Finally, we apply NAT overload (`ip nat inside source`) to enable multiple internal devices to share the single public IP address.

### Configuration:
```plaintext
conf t

interface Serial0/0/0
ip nat outside

interface range GigabitEthernet0/0 - 1
ip nat inside

ip nat pool PAT-Pool 33.33.33.2 33.33.33.2 netmask 255.255.255.252

access-list 1 permit 192.168.10.0 0.0.0.255
access-list 1 permit 192.168.20.0 0.0.0.255
... (add other access-list entries)

ip nat inside source list 1 pool PAT-Pool overload
exit
```

## 3. Dynamic Host Configuration Protocol (DHCP) Address Management

This section demonstrates how to set up DHCP to automate IP address assignments within the network.

### Explanation:
- We prevent the allocation of the first 50 addresses in each VLAN for exclusions (`ip dhcp excluded-address`).
- For specific VLANs, we exclude individual IP addresses from assignment.
- DHCP pools (`ip dhcp pool`) are configured for each VLAN, specifying network ranges, default gateways, and DNS servers.

### Configuration:
```plaintext
enable
configure terminal

ip dhcp excluded-address 192.168.20.1 192.168.20.50
... (add other excluded-address entries)

! DHCP Configuration
ip dhcp pool VLAN20
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 192.168.30.100
exit

... (repeat for other VLAN DHCP pools)
end
copy running-config startup-config
```

## 4. Routing Information Protocol (RIP) Optimization

This section optimizes routing using the Routing Information Protocol (RIP).

### Explanation:
- Interfaces are configured with appropriate IP addresses.
- The RIP routing protocol is activated (`router rip`) with version 2 and auto-summary disabled.
- The network addresses are specified (`network` command) to participate in the RIP routing process.
- A custom administrative distance is assigned to a specific route using the `distance` command.

### Configuration:
```plaintext
! Interface Configurations
interface GigabitEthernet0/0
ip address 192.168.100.1 255.255.255.252
no shutdown
exit

interface GigabitEthernet0/1
ip address 192.168.100.5 255.255.255.252
no shutdown
exit

interface Serial0/0/0
ip address 33.33.33.2 255.255.255.252
no shutdown
exit

router rip
version 2
no auto-summary
network 192.168.100.0
network 33.33.33.0
distance 110 192.168.100.0 0.0.0.3
exit
```

## NY-2911 Configuration Script
```

enable
configure terminal

! Set hostname and user credentials
hostname NY-2911
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

! Configure NAT settings
interface Serial0/0/0
ip nat outside
exit

interface range GigabitEthernet0/0 - 1
ip nat inside
exit

ip nat pool PAT-Pool 33.33.33.2 33.33.33.2 netmask 255.255.255.252

access-list 1 permit 192.168.10.0 0.0.0.255
access-list 1 permit 192.168.20.0 0.0.0.255
access-list 1 permit 192.168.30.0 0.0.1.255
access-list 1 permit 192.168.40.0 0.0.1.255
access-list 1 permit 192.168.50.0 0.0.3.255
access-list 1 permit 192.168.60.0 0.0.7.255
access-list 1 permit 192.168.70.0 0.0.3.255
access-list 1 permit 192.168.100.0 0.0.0.255

ip nat inside source list 1 pool PAT-Pool overload

! Configure DHCP settings
ip dhcp excluded-address 192.168.20.1 192.168.20.50
ip dhcp excluded-address 192.168.30.1 192.168.30.50
ip dhcp excluded-address 192.168.40.1 192.168.40.50
ip dhcp excluded-address 192.168.50.1 192.168.50.50
ip dhcp excluded-address 192.168.60.1 192.168.60.50
ip dhcp excluded-address 192.168.70.1 192.168.70.50
ip dhcp excluded-address 192.168.20.110
ip dhcp excluded-address 192.168.20.120
ip dhcp excluded-address 192.168.30.100
ip dhcp excluded-address 192.168.30.110

ip dhcp pool VLAN20
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 192.168.30.100
exit

ip dhcp pool VLAN30
network 192.168.30.0 255.255.254.0
default-router 192.168.30.1
dns-server 192.168.30.100
exit

ip dhcp pool VLAN40
network 192.168.40.0 255.255.254.0
default-router 192.168.40.1
dns-server 192.168.30.100
exit

ip dhcp pool VLAN50
network 192.168.50.0 255.255.252.0
default-router 192.168.50.1
dns-server 192.168.30.100
exit

ip dhcp pool VLAN60
network 192.168.60.0 255.255.248.0
default-router 192.168.60.1
dns-server 192.168.30.100
exit

ip dhcp pool VLAN70
network 192.168.70.0 255.255.252.0
default-router 192.168.70.1
dns-server 192.168.30.100
exit

! Configure interfaces
interface GigabitEthernet0/0
ip address 192.168.100.1 255.255.255.252
no shutdown
exit

interface GigabitEthernet0/1
ip address 192.168.100.5 255.255.255.252
no shutdown
exit

interface Serial0/0/0
ip address 33.33.33.2 255.255.255.252
no shutdown
exit

! Configure RIP
router rip
version 2
no auto-summary
network 192.168.100.0
network 33.33.33.0
distance 110 192.168.100.0 0.0.0.3
exit

! Save configuration
end
write memory
```