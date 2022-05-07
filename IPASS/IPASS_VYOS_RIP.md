# vyos rip

Last edited time: May 7, 2022 4:55 PM
Reviewed: No

# Installeren

1. installeer de image
`install image`
volg de stappen en reboot

# configureren

### Interfaces

1. zet de omschrijving op WAN, of LAN
`set interfaces ethernet eth0 description WAN
 set interfaces ethernet eth1 description LAN`
2. zet de ip adressen goed op de interfaces
`set interfaces ethernet eth0 address 172.16.1.1/24`
`set interfaces ethernet eth1 address 192.168.1.1/24`
3. valideer de configuratie
`show interfaces`
4. commit en save

### RIP

1. specificeer welk netwerk je via RIP wil adverteren
`set protocols rip network 192.168.1.0/24` 
alternatief kan je ook de interface opgeven waar het netwerk aan gekoppeld is
`set protocols rip interface eth1`
2. zeg dat RIP direct verbonden netwerken moet adverteren
`set protocols rip redistribute connected`
3. specificeer een andere RIP router waarnaar + waarvan rip gerout moet worden
`set protocols rip neighbor 172.16.1.2`
4. valideer de configuratie
`show ip rip`