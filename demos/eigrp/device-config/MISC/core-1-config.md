# HQ-CORE-1 Configuration

## Basic Settings

```cisco
enable
configure terminal

! Disable DNS lookup and set hostname
no ip domain-lookup
hostname HQ-CORE-1

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

## Interface IP Addresses

```cisco
interface GigabitEthernet1/1/1
 no switchport
 ip address 10.100.0.1 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/1/2
 no switchport
 ip address 10.100.0.13 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/22
 no switchport
 ip address 10.100.0.17 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/1
 no switchport
 ip address 10.100.0.21 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/2
 no switchport
 ip address 10.100.0.25 255.255.255.252
 no shutdown
 exit
```

## EIGRP Configuration

```cisco
router eigrp 1
 eigrp router-id 12.12.12.12
 passive-interface default
 no auto-summary
 no passive-interface GigabitEthernet1/0/1
 no passive-interface GigabitEthernet1/0/2
 no passive-interface GigabitEthernet1/0/22
 no passive-interface GigabitEthernet1/1/1
 no passive-interface GigabitEthernet1/1/2
 network 10.100.0.0 0.0.0.255
 network 10.100.0.0 0.0.0.3
 network 10.100.0.12 0.0.0.3
 network 10.100.0.16 0.0.0.3
 network 10.100.0.20 0.0.0.3
 network 10.100.0.24 0.0.0.3
```
