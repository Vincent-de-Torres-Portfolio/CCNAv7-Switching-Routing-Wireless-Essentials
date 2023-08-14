# CCNAv7 II: ACL Skills Integration Demonstration 
## Network Security Policy

### 1. **Firewall_1841 Router - OUTBOUND ACL:**
- Configure an extended named ACL called OUTBOUND.
- Apply the OUTBOUND ACL outbound on the serial interface to the ISP.
- Allow ONLY the following traffic to the Internet: ICMP, DNS, HTTP, HTTPS, FTP (passive), SMTP, and POP3.

---

```CiscoIOS
ip access-list extended OUTBOUND
permit icmp any any
permit udp any any eq 53
permit tcp any any eq 80
permit tcp any any eq 443
permit tcp any any eq 21
permit tcp any any eq 25
permit tcp any any eq 110
deny ip any any
```
Apply to the outbound interface:
```shell
interface Serial0/0/0
ip access-group OUTBOUND out
```
### 2. **Firewall_1841 Router - INBOUND ACL:**
- Configure an extended named ACL called INBOUND.
- Apply the INBOUND ACL inbound on the serial interface from the ISP.
- Allow ONLY the following returning traffic from the Internet: ICMP, DNS, HTTP, HTTPS, FTP (passive), SMTP, and POP3.
- Block any traffic initiated from the Internet.

---

```shell
ip access-list extended INBOUND
permit icmp any any
permit udp any eq 53 any
permit tcp any eq 80 any
permit tcp any eq 443 any
permit tcp any eq 21 any
permit tcp any eq 25 any
deny ip any any
```
Apply to the inbound interface:
```shell
interface Serial0/0/0
ip access-group INBOUND in
```



### 3. **Access Control for Management and Telnet:**
- On LA_3560_1, configure an ACL named TELNET_IN to allow Telnet access from specific IP range.
- Apply the TELNET_IN ACL to line vty 0 to 4 on the device.

---
```shell
ip access-list extended TELNET_IN
permit tcp 10.1.3.0 0.0.0.255 any eq 23
```
Apply to VTY lines:
```shell
line vty 0 4
access-class TELNET_IN in
```
### 4. **Access Control for Server VLAN (VLAN 2):**
- Configure an ACL named VLAN2_SRV_IN for Server VLAN.
- Allow specific services (DNS, HTTP, HTTPS, FTP passive) and IP communication.
- Deny certain ICMP traffic and permit echo-reply messages.

---
```shell
ip access-list extended VLAN2_SRV_IN
permit udp 10.0.2.0 0.255.0.255 any eq 53
permit tcp 10.0.2.0 0.255.0.255 any eq 80
permit tcp 10.0.2.0 0.255.0.255 any eq 443
permit tcp 10.0.2.0 0.255.0.255 any eq 21
permit tcp 10.0.2.0 0.255.0.255 any gt 1023
permit ip 10.0.2.0 0.255.0.255 10.0.2.0 0.255.0.255
permit ip 10.0.2.0 0.255.0.255 10.0.3.0 0.255.0.255
permit ip 10.0.2.0 0.255.0.255 10.0.4.0 0.255.0.255
deny icmp 10.0.2.0 0.255.0.255 10.0.6.0 0.255.0.255 echo
permit icmp 10.0.2.0 0.255.0.255 any echo
permit icmp 10.0.2.0 0.255.0.255 10.0.0.0 0.255.255.255 echo-reply
```
Apply to VLAN 2 interface:
```shell
interface Vlan2
ip access-group VLAN2_SRV_IN in
```

### 5. **Access Control for IT VLAN (VLAN 3):**
- Configure an ACL named VLAN3_IT_IN for IT VLAN.
- Allow specific services (DNS, HTTP, HTTPS, FTP passive, SMTP, POP3) and IP communication.
- Permit ICMP and IP communication with certain networks.

---

```shell
ip access-list extended VLAN3_IT_IN
permit udp 10.1.3.0 0.0.0.255 any eq 53
permit tcp 10.1.3.0 0.0.0.255 any eq 80
permit tcp 10.1.3.0 0.0.0.255 any eq 443
permit tcp 10.1.3.0 0.0.0.255 any eq 21
permit tcp 10.1.3.0 0.0.0.255 any eq 25
permit tcp 10.1.3.0 0.0.0.255 any eq 110
permit tcp 10.1.3.0 0.0.0.255 any eq 25
permit icmp 10.1.3.0 0.0.0.255 any echo
permit ip 10.1.3.0 0.0.0.255 10.0.0.0 0.255.255.255
```
Apply to VLAN 3 interface:
```shell
interface Vlan3
ip access-group VLAN3_IT_IN in
```


6. **Access Control for Admin VLANs (VLANs 4, 5):**
- Configure an ACL named VLAN4_ADMIN_IN for Admin VLAN.
- Allow specific services (DNS, HTTP, HTTPS) and IP communication.
- Deny ICMP to certain networks and permit echo-reply.
- Allow IP communication to Server VLAN.
---
```shell
ip access-list extended VLAN4_ADMIN_IN
permit udp 10.0.4.0 0.255.0.255 any eq 53
permit tcp 10.0.4.0 0.255.0.255 any eq 80
permit tcp 10.0.4.0 0.255.0.255 any eq 443
deny icmp 10.0.4.0 0.255.0.255 10.0.1.0 0.255.0.255 echo
deny icmp 10.0.4.0 0.255.0.255 10.1.3.0 0.0.0.255 echo
deny icmp 10.0.4.0 0.255.0.255 10.0.4.0 0.255.0.255 echo
deny icmp 10.0.4.0 0.255.0.255 10.0.5.0 0.255.0.255 echo
deny icmp 10.0.4.0 0.255.0.255 10.0.6.0 0.255.0.255 echo
permit icmp 10.0.4.0 0.255.0.255 any echo
permit ip 10.0.4.0 0.255.0.255 10.0.2.0 0.255.0.255
permit ip 10.0.4.0 0.255.0.255 10.1.3.0 0.0.0.255
```
Apply to VLAN 4 interface on LA_3560_1 and VLAN 4 interface on SD_1841:
```shell
interface Vlan4
ip access-group VLAN4_ADMIN_IN in
```


7. **Access Control for Users VLANs (VLANs 5, 6):**
- Configure an ACL named VLAN5_USERS_IN for Users VLAN.
- Allow specific services (DNS, HTTP, HTTPS) and IP communication.
- Deny ICMP to certain networks and permit echo-reply.
- Allow IP communication to Server VLAN.
---
```shell
ip access-list extended VLAN5_USERS_IN
permit udp 10.0.5.0 0.255.0.255 any eq 53
permit tcp 10.0.5.0 0.255.0.255 any eq 80
permit tcp 10.0.5.0 0.255.0.255 any eq 443
deny icmp 10.0.5.0 0.255.0.255 10.0.1.0 0.255.0.255 echo
deny icmp 10.0.5.0 0.255.0.255 10.1.3.0 0.0.0.255 echo
deny icmp 10.0.5.0 0.255.0.255 10.0.4.0 0.255.0.255 echo
deny icmp 10.0.5.0 0.255.0.255 10.0.5.0 0.255.0.255 echo
deny icmp 10.0.5.0 0.255.0.255 10.0.6.0 0.255.0.255 echo
permit icmp 10.0.5.0 0.255.0.255 any echo
permit ip 10.0.5.0 0.255.0.255 10.0.2.0 0.255.0.255
permit ip 10.0.5.0 0.255.0.255 10.1.3.0 0.0.0.255
```
Apply to VLAN 5 interface on LA_3560_1 and VLAN 5 interface on SD_1841:
```shell
interface Vlan5
ip access-group VLAN5_USERS_IN in
```

### 8. **Access Control for Wireless VLANs (VLANs 6, 7):**
- Configure an ACL named VLAN6_WIFI_IN for Wireless VLAN.
- Allow specific services (DNS, HTTP, HTTPS) on the Internet.
- Deny ICMP to internal devices and allow echo-reply.
- Allow echo-reply messages from IT VLAN.
---
```shell
ip access-list extended VLAN6_WIFI_IN
permit udp 10.0.6.0 0.255.0.255 any eq 53
permit tcp 10.0.6.0 0.255.0.255 any eq 80
permit tcp 10.0.6.0 0.255.0.255 any eq 443
deny icmp 10.0.6.0 0.255.0.255 10.0.0.0 0.255.255.255 echo
permit icmp 10.0.6.0 0.255.0.255 any echo
permit icmp 10.0.6.0 0.255.0.255 10.1.3.0 0.0.0.255 echo-reply
```
Apply to VLAN 6 interface on LA_3560_1 and VLAN 6 interface on SD_1841:
```shell
interface Vlan6
ip access-group VLAN6_WIFI_IN in
```

> **Note:** The ACLs are configured to meet the specified access requirements for each VLAN and location while maintaining consistency across different VLANs. Adjustments may be necessary based on specific network configurations and requirements.
