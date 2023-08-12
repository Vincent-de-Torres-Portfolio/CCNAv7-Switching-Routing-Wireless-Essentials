# HQ-FW-2 Configuration

### Device Configuration

```cisco
enable
configure terminal

! Disable DNS lookup and set hostname
no ip domain-lookup
hostname HQ-FW-2

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
!
```

### Interface Configuration

```cisco
interface GigabitEthernet0/0
ip address 10.80.80.2 255.255.255.0
ip nat inside
duplex auto
speed auto
standby 2 ip 10.80.80.1
standby 2 preempt
priority 100
!

interface GigabitEthernet0/1
ip address 212.20.20.132 255.255.255.248
ip nat outside
duplex auto
speed auto
standby 1 ip 212.20.20.130
standby 1 preempt
standby preempt
priority 100
!

interface GigabitEthernet0/0/0
ip address 10.100.100.17 255.255.255.252
ip nat inside
!

interface GigabitEthernet0/1/0
ip address 10.100.100.21 255.255.255.252
ip nat inside
!

interface GigabitEthernet0/2/0
ip address 10.100.100.14 255.255.255.252
ip nat inside
!
```

### NAT Configuration

```cisco
ip nat pool PAT-Pool 212.20.20.130 212.20.20.130 netmask 255.255.255.248

ip nat inside source static 10.80.80.80 212.20.20.133
ip nat inside source list 1 pool PAT-Pool overload

access-list 1 permit 10.1.0.0 0.0.255.255
access-list 1 permit 10.2.0.0 0.0.255.255
access-list 1 permit 10.3.0.0 0.0.255.255
access-list 1 permit 10.4.0.0 0.0.255.255
access-list 1 permit 10.100.0.0 0.0.255.255
access-list 1 permit 10.100.100.0 0.0.0.255
access-list 1 permit 10.80.80.0 0.0.0.255
!
```


### EIGRP Configuration

```cisco
router eigrp 1
eigrp router-id 200.200.200.200
redistribute static 
passive-interface default
no passive-interface GigabitEthernet0/1
no passive-interface GigabitEthernet0/0/0
no passive-interface GigabitEthernet0/1/0
no passive-interface GigabitEthernet0/2/0
network 10.100.100.0 0.0.0.3
network 10.100.100.16 0.0.0.3
network 10.100.100.20 0.0.0.3
network 10.100.100.12 0.0.0.3
network 212.20.20.128 0.0.0.7
```

### Floating Static Route

```cisco
ip route 0.0.0.0 0.0.0.0 212.20.20.129 5
ip route 0.0.0.0 0.0.0.0 10.100.100.13 5
```
