```
interface range g0/0-1
ip nat inside

interface se 0/0/0
ip nat outside

access-list 1 permit 172.20.0.0 0.0.255.255
access-list 1 permit 172.30.0.0 0.0.255.255
access-list 1 permit 10.10.10.0 0.0.0.255
access-list 1 permit 10.80.80.0 0.0.0.255

ip nat pool NAT_POOL 200.100.123.2 200.100.123.2 netmask 255.255.255.252

ip nat inside source list 1 pool NAT_POOL overload
```

interface GigabitEthernet0/0
ip address 10.10.10.10 255.255.255.252
no shutdown

interface GigabitEthernet0/1
ip address 10.80.80.1 255.255.255.0
no shutdown

interface Serial0/0/0
ip address 200.100.123.2 255.255.255.252
no shutdown


ip route 0.0.0.0 0.0.0.0 200.100.123.2 
ip route 172.20.0.0 255.255.0.0 10.10.10.9
ip route 172.30.0.0 255.255.0.0 10.10.10.9
ip route 10.10.10.0 255.255.255.0 10.10.10.9