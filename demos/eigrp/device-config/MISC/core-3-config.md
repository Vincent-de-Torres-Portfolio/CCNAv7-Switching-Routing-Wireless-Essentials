# HQ-CORE-3 Configuration

## Basic Settings

```cisco
enable
configure terminal

! Disable DNS lookup and set hostname
no ip domain-lookup
hostname HQ-CORE-3

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

## Interface Configuration

```cisco
enable
configure terminal

interface GigabitEthernet1/0/1
 no switchport
 ip address 10.100.0.22 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/2
 no switchport
 ip address 10.100.0.34 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/0/22
 no switchport
 ip address 10.100.0.49 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/1/1
 no switchport
 ip address 10.100.0.37 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/1/2
 no switchport
 ip address 10.100.0.41 255.255.255.252
 no shutdown
 exit

end

write memory
```

## EIGRP Configuration

```cisco
enable
configure terminal

router eigrp 1
 eigrp router-id 34.34.34.34
 passive-interface default
 no passive-interface GigabitEthernet1/0/1
 no passive-interface GigabitEthernet1/0/2
 no passive-interface GigabitEthernet1/0/22
 no passive-interface GigabitEthernet1/1/1
 no passive-interface GigabitEthernet1/1/2
 no passive-interface GigabitEthernet1/1/3
 no passive-interface GigabitEthernet1/1/4

 network 10.100.0.20 0.0.0.3
 network 10.100.0.32 0.0.0.3
 network 10.100.0.49 0.0.0.3
 network 10.100.0.37 0.0.0.3
 network 10.100.0.41 0.0.0.3
 network 10.100.100.4 0.0.0.3
 network 10.100.100.16 0.0.0.3
 exit

end

write memory
```

## Additional Interface Configuration

```cisco
configure terminal
interface GigabitEthernet1/1/3
 no switchport
 ip address 10.100.100.6 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet1/1/4
 no switchport
 ip address 10.100.100.18 255.255.255.252
 no shutdown
 exit
```
