# Installeren HTTPD

Last edited time: March 25, 2022 1:14 PM
Reviewed: No

1.installeer eerst de httpd service
```bash
sudo dnf install httpd
```

1. start en enable de service
`sudo systemctl start httpd`
`sudo systemctl enable httpd`
2. pas de firewall aan
`firewall-cmd --add-service=http --permanent
firewall-cmd –reload`
3. maak de directory aan waar de website komt
`mkdir /var/www/bmc.test`
4. ga naar de directory en maak het bestand index.html aan
`cd /var/www/html/bmc.test`
`sudo nano index.html`
5. plaats wat domme text in de html zoals:

```html
<h1>hoi</h1>
<h3>doei</h3>
```

1. geef de apache servicegebruiker de owner rechten
`chown root.apache index.html`
2. bewerk daarna de webserverconfiguratie
`sudo nano /etc/httpd/conf.d/www.bmc.conf`
3. en plaats de volgende text in dat bestand

```
<VirtualHost *:80>
	ServerName www.bmc.test
	ServerAlias bmc.test
	DocumentRoot /var/www/bmc.test
</VirtualHost>
```

1. check of de configuratie klopt (geen bericht is goed bericht)
`apachectl configtest`
2. bereik daarna de webserver vanaf een browser en daar zal je de webpagina zien staan.