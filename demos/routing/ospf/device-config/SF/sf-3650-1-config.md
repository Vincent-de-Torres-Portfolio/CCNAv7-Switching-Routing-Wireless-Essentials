

## VLAN:

```CiscoIOS
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
conf t
!
interface GigabitEthernet1/1/1
 no shutdown
 no switchport
 ip address 172.20.100.14 255.255.255.252
!
interface GigabitEthernet1/0/23
 no shutdown
 no switchport
 ip address 172.20.100.26 255.255.255.252
!
interface GigabitEthernet1/1/2
 no shutdown
 no switchport
 ip address 172.20.100.29 255.255.255.252
!
end

## STP & HSRP:

```CiscoIOS
interface Vlan10
ip address 172.20.10.1 255.255.255.0
standby 1 ip 172.20.10.1
standby 1 preempt
standby 1 priority 120

interface Vlan20
ip address 172.20.20.1 255.255.255.0
standby 1 ip 172.20.20.1
standby 1 preempt
standby 1 priority 120

interface Vlan30
ip address 172.20.30.1 255.255.254.0
standby 1 ip 172.20.30.1
standby 1 preempt
standby 1 priority 120

interface Vlan40
ip address 172.20.40.1 255.255.254.0
standby 1 ip 172.20.40.1
standby 1 preempt
standby 1 priority 120

interface Vlan50
ip address 172.20.50.1 255.255.252.0
standby 1 ip 172.20.50.1
standby 1 preempt
standby 1 priority 120

interface Vlan60
ip address 172.20.60.1 255.255.248.0
standby 1 ip 172.20.60.1
standby 1 preempt
standby 1 priority 120

interface Vlan70
ip address 172.20.70.1 255.255.252.0
standby 1 ip 172.20.70.1
standby 1 preempt
standby 1 priority 120

spanning-tree vlan 10 root primary
spanning-tree vlan 20 root primary
spanning-tree vlan 30 root primary
spanning-tree vlan 40 root primary
spanning-tree vlan 50 root primary
spanning-tree vlan 60 root primary
spanning-tree vlan 70 root primary
```

## DHCP:

```CiscoIOS

ip dhcp pool AP
network 172.20.10.0 255.255.255.0
default-router 172.20.10.1
dns-server 172.20.30.100

ip dhcp pool MGT
network 172.20.20.0 255.255.255.0
default-router 172.20.20.1
dns-server 172.20.30.100

ip dhcp pool SRV
network 172.20.30.0 255.255.254.0
default-router 172.20.30.1
dns-server 172.20.30.100

ip dhcp pool IT
network 172.20.40.0 255.255.254.0
default-router 172.20.40.1
dns-server 172.20.30.100

ip dhcp pool INT
network 172.20.50.0 255.255.252.0
default-router 172.20.50.1
dns-server 172.20.30.100

ip dhcp pool WL-STF
network 172.20.60.0 255.255.248.0
default-router 172.20.60.1
dns-server 172.20.30.100

ip dhcp pool WL-GST
network 172.20.70.0 255.255.252.0
default-router 172.20.70.1
dns-server 172.20.30.100
```

## Routing & OSPF:

```CiscoIOS
router ospf 1
router-id 10.10.10.10
no auto-summary

passive-interface default
network 172.20.0.0 0.0.255.255 area 0

no passive-interface GigabitEthernet1/0/23
no passive-interface GigabitEthernet1/1/1
no passive-interface GigabitEthernet1/1/2
no passive-interface Vlan10
no passive-interface Vlan20
no passive-interface Vlan30
no passive-interface Vlan40
no passive-interface Vlan50
no passive-interface Vlan60
no passive-interface Vlan70
```

