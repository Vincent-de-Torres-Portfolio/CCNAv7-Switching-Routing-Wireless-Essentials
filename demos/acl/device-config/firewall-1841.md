# ACL

1. On `Firewall-1841` router, configure an extended named ACL called OUTBOUND and apply to the
outbound of the serial interface to ISP to allow ONLY the following traffic to the Internet: ICMP, DNS, HTTP, HTTPS, FTP (passive), SMTP and POP3.

```CiscoIOS

ip access-list extended OUTBOUND

permit icmp any any
permit tcp any any eq domain
permit tcp any any eq www
permit tcp any any eq 443
permit tcp any any eq ftp-data
permit tcp any any eq ftp
permit tcp any any eq smtp
permit tcp any any eq pop3

deny ip any any

interface Serial0/0/0
ip access-group OUTBOUND out

```

---

2. On Firewall_1841 router, configure an extended named ACL called INBOUND and apply to the inbound of the serial interface to ISP to allow ONLY the following returning traffic from the Internet: ICMP, DNS, HTTP, HTTPS, FTP (passive), SMTP and POP3. Block the any traffic initiated from the Internet.

```
ip access-list extended INBOUND

permit icmp any any echo-reply
permit tcp any any eq domain
permit tcp any any eq www
permit tcp any any eq 443
permit tcp any any eq ftp-data
permit tcp any any eq ftp
permit tcp any any eq smtp
permit tcp any any eq pop3


deny ip any any

exit

interface Serial0/0/0
ip access-group INBOUND in

```

---

3. 