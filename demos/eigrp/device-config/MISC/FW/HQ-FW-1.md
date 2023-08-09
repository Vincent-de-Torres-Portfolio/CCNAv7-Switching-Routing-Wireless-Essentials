interface GigabitEthernet0/1
no shutdown
ip address 10.100.100.1 255.255.255.252
standby 1 ip 10.100.100.1
standby 1 priority 110
standby 1 preempt

interface GigabitEthernet0/1
ip address 10.100.100.2 255.255.255.252
standby 1 ip 10.100.100.1
standby 1 priority 100
standby 1 preempt

configure terminal
interface GigabitEthernet0/0/0
ip address 10.100.100.5 255.255.255.252
no shutdown
exit
interface GigabitEthernet0/1/0
ip address 10.100.100.9 255.255.255.252
no shutdown
exit
interface GigabitEthernet0/2/0
ip address 10.100.100.13 255.255.255.252
no shutdown
exit
end
copy running-config startup-config


router eigrp 1
 eigrp router-id 100.100.100.100
 passive-interface default
 no passive-interface GigabitEthernet0/1
 no passive-interface GigabitEthernet0/0/0
 no passive-interface GigabitEthernet0/1/0
 no passive-interface GigabitEthernet0/2/0
 network 10.100.100.0 0.0.0.3
 network 10.100.100.4 0.0.0.3
 network 10.100.100.8 0.0.0.3
 network 10.100.100.12 0.0.0.3


router eigrp 1
 eigrp router-id 200.200.200.200
 passive-interface default
 no passive-interface GigabitEthernet0/1
 no passive-interface GigabitEthernet0/0/0
 no passive-interface GigabitEthernet0/1/0
 no passive-interface GigabitEthernet0/2/0
 network 10.100.100.0 0.0.0.3
 network 10.100.100.16 0.0.0.3
 network 10.100.100.20 0.0.0.3
 network 10.100.100.12 0.0.0.3
 exit


---


configure terminal
interface GigabitEthernet0/0/0
ip address 10.100.100.17 255.255.255.252
no shutdown
exit
interface GigabitEthernet0/1/0
ip address 10.100.100.21 255.255.255.252
no shutdown
exit
interface GigabitEthernet0/2/0
ip address 10.100.100.14 255.255.255.252
no shutdown
exit
end
copy running-config startup-config

---
interface GigabitEthernet0/0
no shut
 ip address 10.80.80.1 255.255.255.0
 standby 2 ip 10.80.80.1
 standby 2 priority 110
 standby 2 preempt
 exit

interface GigabitEthernet0/0
 ip address 10.80.80.2 255.255.255.0
 standby 2 ip 10.80.80.1
 standby 2 priority 110
 standby 2 preempt
 exit
