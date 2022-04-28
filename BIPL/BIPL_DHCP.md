# Installeren DHCP

Last edited time: March 25, 2022 11:43 AM
Reviewed: No

1. installeer de dhcp service
`sudo dnf install dhcp-server`
2. kopieer het voorbeeldbestand naar de configuratiemap
`sudo cp /usr/share/doc/dhcp-server/dhcpd.conf.example /etc/dhcp/dhcpd.conf`
3. comment in de dhcpd.conf alle configuraties uit **Na de regel log-facility** op degene na met `A slightly different configuration for an internal subnet`

```bash
subnet 192.168.6.0 netmask 255.255.255.0 {
  range 192.168.6.30 192.168.6.60;
  option domain-name-servers 1.1.1.1;
  option domain-name "bmc.test";
  option routers 192.168.6.1;
  option broadcast-address 192.168.6.255;
  default-lease-time 600;
  max-lease-time 7200;
}
```

1. Start the dhcpd service
`sudo systemctl start dhcpd`
2. zet op een windows client de netwerkkaart op ‘obtain automatically’
3. voer op de windowsclient het volgende commando uit om een ip address op te vragen
`ipconfig /renew`