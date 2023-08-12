# HQ-CORE-4 Configuration


## Basic Settings

```cisco
enable
configure terminal

! Disable DNS lookup and set hostname
no ip domain-lookup
hostname HQ-CORE-4

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


## Interfaces Configuration

```cisco
interface GigabitEthernet1/0/1
 no switchport
 ip address 10.100.0.30 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/2
 no switchport
 ip address 10.100.0.26 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/22
 no switchport
 ip address 10.100.0.50 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/23
 no switchport
 ip address 10.100.0.53 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/24
 no switchport
 ip address 10.100.0.45 255.255.255.252
 no shutdown
 exit
```

## EIGRP Configuration

```cisco
router eigrp 1
 eigrp router-id 43.43.43.43
 passive-interface default
 no passive-interface GigabitEthernet1/0/1
 no passive-interface GigabitEthernet1/0/2
 no passive-interface GigabitEthernet1/0/22
 no passive-interface GigabitEthernet1/0/23
 no passive-interface GigabitEthernet1/0/24
 no passive-interface GigabitEthernet1/1/3
 no passive-interface GigabitEthernet1/1/4
 network 10.100.0.28 0.0.0.3
 network 10.100.0.24 0.0.0.3
 network 10.100.0.48 0.0.0.3
 network 10.100.0.52 0.0.0.3
 network 10.100.0.44 0.0.0.3
 network 10.100.100.8 0.0.0.3
 network 10.100.100.20 0.0.0.3
```

## Interface Configuration for GigabitEthernet1/1/3

```cisco
configure terminal
interface GigabitEthernet1/1/3
 no switchport
 ip address 10.100.100.10 255.255.255.252
 no shutdown
 exit
```

## Interface Configuration for GigabitEthernet1/1/4

```cisco
configure terminal
interface GigabitEthernet1/1/4
 no switchport
 ip address 10.100.100.22 255.255.255.252
 no shutdown
 exit
```
