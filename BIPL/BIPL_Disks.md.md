# Configureren Disks,Volumes en VolumeGroups

Last edited time: March 25, 2022 4:15 PM
Reviewed: No

1. Voeg 2 sataschijven toe aan de VM
2. kijk met lsblk -fs wat de namen daarvan zijn
in dit scenario sdb en sdc

# Partities maken

1. maak met fdisk partities op de schijf /dev/sdb
```bash
sudo fdisk /dev/sdb
```
2. type `n` voor een nieuwe partitie
3. type `p` voor een primarie en `e` voor een extended partitie
4. geef een partitienummer op (default is meestal goed)
5. geef een eerste sector op (default is meestal goed)
6. geef een laatste sector op (je kan bijv +5g +10m +20k intypen)
7. type `w` om de aanpassingen ‘op te slaan’
8. nu heeft sdb een partitie genaamd sdb1 van 5gb

# Bestandssystemen op partities zetten

1. als we op sdb1 een ext3 bestandssysteem willen zetten voeren we het volgende uit:
```bash
sudo mkfs -t ext3 /dev/sdb1
```
2. als we dan `lsblk --fs` uitvoeren zie je dat sdb1 het bestandssysteem ext3 heeft

# VolumeGroup maken

1. maak een nieuwe partitie (zie partities maken)
2. maak van deze partitie een physical volume
`pvcreate /dev/sdc1`
3. maak daarna een volumegroep aan op dat fysieke volume met een naam (in dit geval VGDATA)
`sudo vgcreate VGDATA /dev/sdc1`
4. maak nu een logisch volume op de volumegroep
`lvcreate <size> <name> <volumegroepdestination>
lvcreate -L 1 GB -n lv_webcontent VGDATA`
5. maak op het logische volume een bestandssysteem xfs (kan net zo goed ext3 of ext4 zijn)
```bash
mkfs -t xfs -f /dev/VGDATA/lv_webcontent
```

![Untitled](Configureren%20Disks,Volumes%20en%20VolumeGroups%2071a7a350019d4ee9a04175cfd08344e0/Untitled.png)

# Partities mounten

1. maak bijvoorbeeld een /data map aan
2. mount daarna het logische volume aan de map data
```bash
sudo mount /dev/VGDATA/lv_webcontent /data
```
dit kan ook met andere partities en hoeft niet perse alleen met logische volumes/volumegroups

een andere manier om te mounten is het bewerken van fstab

1. zoek het UUID op van de partitie die je wil ‘mounten’, dit doe je door `lsblk` in te typen 
2. bewerk het bestand /etc/fstab en voeg een nieuwe regel toe
`<UUID> <mountpoint> <filesystem> <options> <dump> <fsck>`
`UUID = 2b6809fd-001a-4fc6-b5b4-49dcdab55302 /data xfs defaults 0 0`
3. Letop! wanneer je een typefout maakt in fstab en je herstart komt je vm in een recovery mode!