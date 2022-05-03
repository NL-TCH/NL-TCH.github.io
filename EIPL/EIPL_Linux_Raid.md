# Linux Raid

Last edited time: May 3, 2022 3:18 PM
Reviewed: No

**In dit scenario zijn:**

- **er 3 schijven toegevoegd aan de VM** sda, sdb en sdc (allemaal SATA schijven)
- **een volumegroep genaamd ‘VGDATA‘
- de mountpunten /data/webcontent en /data/db**

# Maken raid-array

1. maak partities op de schijven
`sudo fdisk /dev/sda`
maak een primaire partitie `p`
geef het partitienummer 1 `1`
laat de eerste en laatste sector als default `<Enter>`
wijzig het partitietype `t`
zet het partitietype naar ‘linux raid autodetect’ `fd`
sla de wijzigingen op `w`
**doe bovenstaande ook voor sdb en sdc**
2. monitor stap 3 door het volgende commando **in een nieuwe terminal uit te voeren**
`sudo watch cat /proc/mdstat`
3. maak een raidvolume door de 3 schijven te combineren
`sudo mdadm --create /dev/md0 --level=1 --raid-devices=3 /dev/sda1 /dev/sdb1 /dev/sdc1`
wanneer je maar 2 schijven hebt (sda en sdb) voer je onderstaande uit
`sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sda1 /dev/sdb1`

# Vastleggen van raid-array

1. maak de volgende map aan
`sudo mkdir /etc/mdadm`
2. maak het volgende bestand aan 
`sudo touch /etc/mdadm/mdadm.conf`
3. plaats de 3 schijven het bovenstaande bestand
`echo DEVICE /dev/sda1 /dev/sdb1 /dev/sdc1 > /etc/mdadm/mdadm.conf`
wanneer je maar 2 schijven hebt (sda en sdb) voer je onderstaande uit
`echo DEVICE /dev/sda1 /dev/sdb1 > /etc/mdadm/mdadm.conf`
4. laat de mdadm tool de partities scannen en vastleggen (vergelijkbaar met fstab)
`mdadm --detail --scan >> /etc/mdadm/mdadm.conf`
5. verifieer de gemaakte array
`sudo mdadm --detail /dev/md0`

# Logische volumes aanmaken op de array

1. maak een fysiek volume van de raid (md0) 
`sudo pvcreate /dev/md0`
2. Maak vervolgens een volume groep genaamd VGDATA aan
`sudo vgcreate VGDATA /dev/md0`
3. maak nu 2 logische volumes op de volumegroep
`sudo lvcreate -L 2GB -n lvwebcontent VGDATA`
`sudo lvcreate -L 3GB -n lvdatabase VGDATA`
4. verifieer de logische volumes
`sudo lvdisplay`
5. geef de logische volumes een bestandssysteem (in dit scenario ext4)
`sudo mkfs -t ext4 /dev/VGDATA/lvwebcontent
 sudo mkfs -t ext4 /dev/VGDATA/lvdatabase`

# De partities mounten

1. maak de mappen /data/webcontent en /data/db
`sudo mkdir /data /data/webcontent /data/db`
2. open het bestand /etc/fstab in een editor (nano, vi, vim, emacs  etc..)
`sudo nano /etc/fstab`
3. zet de volgende regels in het bestand
`/dev/VGDATA/lvwebcontent        /data/webcontent        ext4    defaults        0	0`
`/dev/VGDATA/lvdatabase          /data/db                ext4    defaults        0	0`
4. na een reboot zouden de volumes nog steeds gemount moeten zijn

# Dump & Restore

1. maak een bestand test2.txt in /data/db
2. maak een **level0 (full)** backup (dump) van alle bestanden in /data/db
`sudo dump -0u -f /backup.0 /data/db`
3. maak een **leve1 (incremental)** backup (dump) van alle betanden in /data/db
`sudo dump -1u -f /db-backup.1 /data/db`
4. verwijder het tets2.txt bestand
5. recover het test2.txt bestand doormiddel van het onderstaande commando
`sudo restore -if db-backup.1`
`add tets2.txt
 extract 
 1`
`y`
`quit`
6. nu zie je dat het test2.txt bestand niet op de oude locatie is teruggeplaatst, maar in de root (/) is gerecovered