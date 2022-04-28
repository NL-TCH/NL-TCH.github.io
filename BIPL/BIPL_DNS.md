# Installeren DNS

Last edited time: March 25, 2022 4:12 PM
Reviewed: No

**Dit scenario bevat:
Hostname: dnsrehl
Domein: bmc.test
ip address: 192.168.5.2**

1. Installeer named 
`sudo dnf install bind

2. zet de hostname van de vm correct
`hostname dnsrehl.bmc.test`
3. edit `/etc/named.conf` en voeg de zone toe

```bash
zone "bmc.test." IN {
        type master;
        file "bmc.test";
};
```

1. pas ook de `listen-on port 53` aan van [localhost](http://localhost)/127.0.0.1 aan naar any
2. pas daarna de `allow-query` aan van [localhost](http://localhost)/127.0.0.1 aan naar any

1. ga nu naar /var/named/ en kopieer named.empty naar bmc.test
`cp /var/named/named.empty /var/named/bmc.test`
2. geef de named gebruiker de rechten op het bestand
`chown root.named bmc.test`
3. wijzig het bestand zodat het er zo uitziet:

```bash
$ORIGIN bmc.test.
$TTL 3H
@	IN SOA  @ bmc.test. (
                                        2022001 ; serial
                                        1D	; refresh
                                        1H	; retry
                                        1W	; expire
                                        3H )    ; minimum
        NS	dnsrehl.bmc.test.
dnsrehl IN	A       192.168.5.2
srv4    IN	A	192.168.5.2
ftp     IN	CNAME   srv4
www     IN	CNAME   dnsrehl
```

1. start de named service
`systemctl start named`
2. laat de firewall poort 53 voor DNS verkeer toestaan voor TCP en UDP
`sudo firewall-cmd --permanent --add-port=53/tcp --zone=public
sudo firewall-cmd --permanent --add-port=53/udp --zone=public
sudo firewall-cmd --reload`
3. zet je lokale server in je resolv.conf bestand (te vinden in /etc/resolv.conf)
4. test de dns door de volgende commando’s uit te voeren
`nslookup www.bmc.test`
`dig www.bmc.test`
`host dnsrehl.bmc.test`