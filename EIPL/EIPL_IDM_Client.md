# IDM client

Last edited time: May 2, 2022 11:37 AM
Reviewed: No

**Dit scenario bevat:
Domein/realm: school.test
Hostname: client1.school.test**

# Voorbereiding

*voor de voorbereiding dient de DNS van de server op 1.1.1.1 of 8.8.8.8 te staan aangezien internet toegang noodzakelijk is ivm installeren van packages.*

1. zorg dat de tijdzone goed staat voor de VM
2. zorg dat de VM geregistreerd staat in de subscription manager
3. zorg dat de VM up-to-date is door middel van 
`sudo dnf update`
4. zet de hostname goed 
`sudo hostnamectl set-hostname client1.school.local`
5. voeg de premium repositories toe
`subscription-manager repos --enable=rhel-8-for-x86_64-baseos-rpms`
`subscription-manager repos --enable=rhel-8-for-x86_64-appstream-rpms`

# Installatie

1. installeer de idM client software
`sudo dnf -y install @idm:client`
2. **zet nu de dns server op het adress van de server1 in plaats van de online DNS server
controleer dit ook in** `/etc/resolv.conf`
3. voeg de server zelf + eventuele clients toe aan het `/etc/hosts` bestand. doe een entry met, en een entry zonder domein naam
`xx.xx.xx.xx.xx server1.school.local  server1`
`xx.xx.xx.xx.xx client1.school.local  client1`
4. pas de firewall aan om de DNS en NTP services door te laten
`sudo firewall-cmd --add-service={dns,ntp} --permanent`
`sudo firewall-cmd --reload`
5. ‘join’ nu het domein
`sudo ipa-client-install --hostname=client1.school.local --mkhomedir --server=server1.school.local --domain school.local --realm SCHOOL.LOCAL`
YES-NO-YES
6. maak een snapshot