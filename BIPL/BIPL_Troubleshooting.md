# Troubleshooting Linux

Last edited time: April 2, 2022 7:30 PM
Reviewed: No

# Rechten controleren

[meer informatie en de bron van onderstaande informatie](https://linuxize.com/post/chmod-command-in-linux/)

als services niet werken kan het eraan liggen dat de service (bijv apache) geen rechten heeft op het .html bestand. controleer de rechten met `ls -al`

```bash
teunis@pop-os ~/test> ls -al
total 8
drwxrwxr-x  2 teunis teunis 4096 Apr  2 19:07 ./
drwxrwxrwx 42 teunis teunis 4096 Apr  2 19:07 ../
-rw-rw-r--  1 teunis teunis    0 Apr  2 19:07 test.html
```

Verander de owner van het bestand met chown. De syntax van chown is:
`chown USER:GROUP FILE`

om de rechten (read,write of execute) aan te passen op een bestand gebruik je chmod. 

de rechtenvolgorde is:
Owner-Group-Others
je kan rechten toevoegen met getallen.
Execute = 1
Write = 2
Read = 4

zet de permissies voor alleen de Owner Read,Write en Execute toe met bijvoorbeeld:

`chmod 700 test.html`

ook kan je met letters werken

`chmod u +wrx test.html` (u=user +rwx is toevoegen van read,write,execute)

om rechten te verwijderen voor de others groep gebruik je de volgende syntax

`chmod o -rwx test.html` (o=others -rwx is het verwijderen van read,write,execute)

# Troubleshooten DNS en DHCP

Om de status te zien van de bind en dhcpd services gebruik je `systemctl status bind` of `systemctl status dhcpd`. Om de errorcodes te zien gebruik je het volgende commando `journalctl -xe`

# Firewall troubleshooting

Gebruik het commando `firewall-cmd --list-all` om te zien wat er allemaal wordt toegelaten door je firewall.

# Services troubleshooting

Een dhcp service kan geinstalleerd zijn en draaien, maar na een reboot blijft deze soms uit staan. Om een service te laten starten bij het opstarten van de machine moet je deze ‘enabelen’. Dit doe je met het volgende commando: `systemctl enable $service` $service kan zijn: bind,dhcpd,smb,etc...