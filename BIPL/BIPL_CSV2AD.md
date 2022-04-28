# Importeren CSV

Last edited time: April 2, 2022 7:04 PM
Reviewed: No

# CSVDE

csvde kan geen wachtwoorden importeren en kan geen objecten verwijderen

1. open cmd als de Administrator gebruiker
2. gebruik het commando `csvde -i -v -k -F users.csv`
    1. -i voor import
    2. -v voor ‘verbose’ zodat je de output ziet
    3. -k voor skip de foutmeldingen en ga door met de volgende
    4. -F voor ‘file’ om een csv bestand aan te wijzen

het .csv bestand moet er als volgend uit zien:

![Untitled](Importeren%20CSV%20fc5b322e1c5e46858b10c2e73b47626a/Untitled.png)

# LDIFDE

ldifde kan objecten wijzigen en verwijderen, kan ook geen wachtwoorden importeren/veranderen

1. open cmd als de Administrator gebruiker
2. gebruik het commando `ldifde -i user.ldf -s Server2`
    1. -i voor import
    2. -s voor server
    

het .ldf bestand ziet er als volgend uit:

![Untitled](Importeren%20CSV%20fc5b322e1c5e46858b10c2e73b47626a/Untitled%201.png)

# DSADD

DSADD kan alleen objecten toevoegen en niet wijzigen. DSADD ondersteund geen scripten en wordt in regelvorm uitgevoerd. Het kan echter wel wachtwoorden invoeren.

dsadd kan je uitvoeren met de volgende syntax:

- UserDN
- UPN
- Password
- Description
- Memberof
- Homdrv /dir
- Profile

Een DSADD regel ziet het er als volgend uit:

![Untitled](Importeren%20CSV%20fc5b322e1c5e46858b10c2e73b47626a/Untitled%202.png)

# DSMOD

DSMOD kan alleen objecten wijzigen en niet toevoegen. Een voorbeeld van DSMOD is:

![Untitled](Importeren%20CSV%20fc5b322e1c5e46858b10c2e73b47626a/Untitled%203.png)

# DSQuery

DSQuery kan op basis van zoek criteria AD objecten vinden. Een voorbeeld van DSQuery is:

![Untitled](Importeren%20CSV%20fc5b322e1c5e46858b10c2e73b47626a/Untitled%204.png)

# DSMove

DSMove is om AD objecten te verplaatsen van OU naar OU. Hieronder wat voorbeelden van DSMove.

![Untitled](Importeren%20CSV%20fc5b322e1c5e46858b10c2e73b47626a/Untitled%205.png)

# Powershell

Met Powershell kunnen ook AD objecten worden aangemaakt. Bijvoorbeeld een OU:

`New-ADOrganizationalUnit studenten`

Ook kan er een Import worden gedaan van een .csv om gebruikers te importeren:

`import-csv c:\powershell\source\users.csv |New-ADUser`