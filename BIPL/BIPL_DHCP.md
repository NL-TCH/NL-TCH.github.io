# Installeren DHCP

Last edited time: March 25, 2022 11:43 AM
Reviewed: No

**Dit scenario bevat:
Netwerkaddres: 192.16.6.0
subnet 255.255.255.0 (/24)
uitgeefbare range: 192.168.6.30 tot 192.168.6.60
dns server: 1.1.1.1
default gateway: 192.16.6.1
broadcast address: 192.16.6.255**

1. installeer de dhcp service
```bash
sudo dnf install dhcp-server
```
2. kopieer het voorbeeldbestand naar de configuratiemap
```bash
sudo cp /usr/share/doc/dhcp-server/dhcpd.conf.example /etc/dhcp/dhcpd.conf
```
3. comment in de dhcpd.conf alle configuraties uit **Na de regel log-facility** op degene na met `A slightly different configuration for an internal subnet`

```bash
subnet 192.168.6.0 netmask 255.255.255.0 { #het netwerkaddress + subnetmask
  range 192.168.6.30 192.168.6.60;  #eerste en laatste uitgeefbare adres
  option domain-name-servers 1.1.1.1; #een dns server (kan lokale server zijn)
  option domain-name "bmc.test"; #een naam die rechtsonderin komt te staan
  option routers 192.168.6.1; #een default gateway (voor de client)
  option broadcast-address 192.168.6.255; #een broadcastadres voor het netwerk
  default-lease-time 600; #dit is een standaard instelling (houden staan)
  max-lease-time 7200; #dit is een standaard instelling (houden staan)
}
```

1. Start the dhcpd service
```bash
sudo systemctl start dhcpd
```
2. zet op een windows client de netwerkkaart op ‘obtain automatically’
3. voer op de windowsclient het volgende commando uit om een ip address op te vragen
```powershell
ipconfig /renew
```