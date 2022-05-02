# IDM server

Last edited time: May 2, 2022 11:35 AM
Reviewed: No

**Dit scenario bevat:
Domein/realm: school.test
Hostname: server1.school.test**

# Voorbereiding

*voor de voorbereiding dient de DNS van de server op 1.1.1.1 of 8.8.8.8 te staan aangezien internet toegang noodzakelijk is ivm installeren van packages.*

1. zorg dat de tijdzone goed staat voor de VM
2. zorg dat de VM geregistreerd staat in de subscription manager
3. zorg dat de VM up-to-date is door middel van 
`sudo dnf update`
4. zet de hostname goed 
`sudo hostnamectl set-hostname server1.school.local`
5. voeg de premium repositories toe
`subscription-manager repos --enable=rhel-8-for-x86_64-baseos-rpms`
`subscription-manager repos --enable=rhel-8-for-x86_64-appstream-rpms`

# Installatie

1. installeer de idM software
`sudo dnf module enable idm:DL1`
2. installeer de DNS server voor de IDM installatie
`sudo dnf install ipa-server ipa-server-dns -y`
3. **zet nu de dns server op het eigen ip address in plaats van de online DNS server
controleer dit ook in** `/etc/resolv.conf`
4. voeg de server zelf + eventuele clients toe aan het `/etc/hosts` bestand. doe een entry met, en een entry zonder domein naam
`xx.xx.xx.xx.xx server1.school.local  server1`
`xx.xx.xx.xx.xx client1.school.local  client1`
5. pas de firewall aan om alle services door te laten
`sudo firewall-cmd --add-service={http,https,dns,ntp,freeipa-ldap,freeipa-ldaps} --permanent
sudo firewall-cmd --reload`
6. maak een eventuele snapshot
7. installeer de ipa-server
`sudo ipa-server-install`
YES - <enter> - <wachtwoord> - <wachtwoord> - <negeer de forwardlookup zone error (NO)> - <reverse zones toevoegen (JA)> - NO - <bevestigen (YES) 
8. maak een eventuele snapshot
9. pas de kerberos ticket policy aan
edit bestand `/etc/krb5.conf`
voeg toe/pas aan de volgende regels:
`ticket_lifetime = 30d`
`renew_lifetime = 21d`
restart de service
`systemctl restart krb5kdc`

# Configuratie

**zoeken in IDM**
`ipa user-find x` voor het 1e resultaat
`ipa user-find x --all` voor alle matchende resultaten
`ipa user-find --disabled=False` voor alle actieve gebruikers
`ipa group-show x` hier zoek je naar de  details van groep x 

**toevoegen in IDM**
`ipa user-add` hierna vul je de voornaam + achternaam in
`ipa uer-add --password` hierna vul je voornaam + achternaam + wachtwoord in
`ipa group-add x` hierna vul je de naam van de groep in

**verwijderen in IDM**
`ipa user-del x` vul hier de ‘user login name’ in 
`ipa group-del x` vul de groep-naam in 

**aanpasen in IDM**
`ipa user-mod x --password` hier pas je het wachtwoord aan van gebruiker x
`ipa user-mod x -mobile=06-475638` hier pas je het mobiele nummer aan van gebruiker x

**voorbeelden**

`ipa user-add fmercury --first=Freddie --last=Mercury --manager=csantana --email=fmercury@school.local --homedir=/home/work/fmercury --password`

`ipa user-add rdaltrey --first=Roger --last=Daltery --manager=csantana --email=rdaltrey@school.local --homedir=/home/work/rdaltrey --password`

`ipa user-mod csantana --carlicense=AA-12-34 --city=IJmuiden --postalcode=1970AA --street=Dorpstraat16 --mobile=06-475638`