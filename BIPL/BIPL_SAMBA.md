# Installeren SAMBA

Last edited time: March 25, 2022 11:43 AM
Reviewed: No

1. installeer samaba
`sudo dnf install samba`
2. maak voor de zekerheid een backup van /etc/samba/smb.conf
`cp /etc/samba/smb.conf /etc/samba/smb.conf.old`
3. bewerk daarna het smb.conf bestand en wijzig de global config naar:

```bash
[global]
	workgroup = bmc
	server string = Samba Server %v
	netbios name = rhel1
	security = user
	map to guest = bad user
	dns proxy = no
```

1. en voeg onderaan het bestand de shareinfo toe

```bash
[secure]
	path = /home/secure
	valid users = @smbgrp
	guest ok = no
	writable = yes
	browsable = yes
```

1. start de services smb en nmb
`systemctl start smb nmb`
2. configureer de groep en de gebruiker
    1. voeg een gebruiker toe (bijv hu)
    `sudo useradd hu`
    2. maak de smbgrp aan (zie stap 4 voor de naam)
    `sudo groupadd smbgrp`
    3. voeg de hu gebruiker toe aan de smbgrp
    `sudo usermod -aG smbgrp hu`
    4. geef een samba-wachtwoord aan de hu gebruiker
    `sudo smbpasswd -a hu`
3. maak de sharemap lokaal aan en geef de rechten aan de sambasysteem gebruiker
    1. maak de sharemap aan
    `mkdir /home/secure`
    2. maak de smbgrp de owner van deze map
    `sudo chown -R hu:smbgrp /home/secure`
    3. wijzig de rechten zodat alleen de owner (hu) en de groep (smbgrp) mogen w,r,x
    `sudo chmod -R 0770 /home/secure/`
    4. ivm SELinux voer je het volgende commando uit
    `sudo chcon -t samba_share_t /home/secure/`
4. voeg de service samba toe aan de firewall
```bash
sudo firewall-cmd --add-service=samba
sudo firewall-cmd --runtime-to-permanent
sudo firewall-cmd --reload
```
5. ga op een windowsclient in de verkenner naar \\ip\secure en login met de hu gebruiker