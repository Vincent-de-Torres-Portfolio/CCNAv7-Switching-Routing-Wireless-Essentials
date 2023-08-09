interface GigabitEthernet0/0
 ip address 212.20.20.129 255.255.255.248
 no shutdown
 exit

en
conf t

 interface GigabitEthernet0/0/0
 no shutdown
 ip address 22.22.22.2 255.255.255.252
 no shutdown
exit


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

interface GigabitEthernet0/1
 ip nat outside
 exit

interface GigabitEthernet0/0
 ip nat inside
 exit

 interface GigabitEthernet0/1/0
 ip nat inside
 exit

  interface GigabitEthernet0/2/0
 ip nat inside
 exit
  interface GigabitEthernet0/3/0
 ip nat inside
 exit

end

write memory
