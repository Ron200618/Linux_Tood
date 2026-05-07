# failis-steemid

VIRTUAALMASINAT TEHES PEAB MASINA NIMES JA PEAKASUTAJAS OLEMA SINU NIMI - IGAL KUVATÕMMISEL PEAB SEE KA NÄHTAV OLEMA! VASTASEL JUHUL HINNE 2


ETTEVALMISTUS: Virtuaalkeskkonna seadistus
Enne töö alustamist tuleb VirtualBoxis virtuaalmasinale lisada füüsilised kettaseadmed.

Lülita virtuaalmasin välja (Power Off).
Ava Settings -> Storage -> Controller: SATA.
Lisa 3 uut virtuaalset kõvaketast (Create New Disk -> VDI -> Dynamically allocated).

Ketas 1: 5 GB (Tähis süsteemis tõenäoliselt /dev/sdb)
Ketas 2: 5 GB (Tähis süsteemis tõenäoliselt /dev/sdc)
Ketas 3: 10 GB (Tähis süsteemis tõenäoliselt /dev/sdd)
Käivita virtuaalmasin.
Kontrolli ketaste olemasolu käsuga lsblk. Veendu, et näed seadmeid sdb, sdc ja sdd.
ÜLESANNE 1: Failisüsteemide piirangud ja ühilduvus (FAT32 vs exFAT)
Tegevus: Tuvastada vanemate failisüsteemide piirangud suurte failide käitlemisel.

Partitsioneerimine: Kasuta tööriista fdisk või parted, et luua kettale /dev/sdb üks primaarne partitsioon, mis hõlmab kogu ketast.
Vormindamine (FAT32): Vorminda see partitsioon (/dev/sdb1) FAT32 failisüsteemiks. Leia ise sobiv käsk (vihje: mkfs).
Haakimine (Mount): Loo kaust /mnt/usb_test ja haagi (mount) partitsioon sinna.
Testimine: Ürita luua sinna kettale täpselt 4.5 GB suurune fail. Kasuta selleks käsku: sudo fallocate -l 4.5G /mnt/usb_test/bigfile.img (Kui fallocate ei toimi antud failisüsteemis, kasuta dd if=/dev/zero of=...).
Analüüs: Protsess ebaõnnestub veateatega. Uuri veateadet ja põhjenda raportis tehniliselt, miks 4.5 GB faili ei saa 5 GB tühjale kettale kirjutada. Mis on FAT32 arhitektuurne piirang?
Lahendus: Vorminda partitsioon ümber failisüsteemiks exFAT (või NTFS, kui exFAT pole saadaval). Korda faili loomise katset.
ÜLESANNE 2: Failiõigused ja POSIX ühilduvus (NTFS vs ext4)
Tegevus: Mõista, miks Linuxi õigused (rwx) ei toimi Windowsi failisüsteemides.

Ettevalmistus: Vorminda /dev/sdb1 ümber NTFS failisüsteemiks.
Skripti loomine:

Loo haakepunkti fail test_script.sh.
Sisu: echo "See on test"
Õiguste muutmine: Ürita anda failile käivitusõigus käsuga: sudo chmod +x /mnt/usb_test/test_script.sh
Kontroll: Käivita ls -l /mnt/usb_test/. Kas failil on x (execute) õigus? Proovi skripti käivitada (./test_script.sh).
Analüüs: Miks chmod käsk ei andnud veateadet, aga õigused ei muutunud? Uuri, kuidas NTFS lahendab failiõigusi Linuxis ja mis on "Mount options" roll siin.
Üleminek ext4-le: Vorminda sama partitsioon nüüd ext4 failisüsteemiks. Korda skripti loomist ja chmod käsku. Veendu, et failiõigused toimivad nüüd korrektselt.
ÜLESANNE 3: Andmete terviklikkus ja Journaling (ext2 vs ext4)
Tegevus: Simuleerida voolukatkestust ja võrrelda vana vs uue failisüsteemi taastumisvõimet.

Ettevalmistus:

Jaga ketas /dev/sdc kaheks võrdseks partitsiooniks (/dev/sdc1 ja /dev/sdc2).
Vorminda /dev/sdc1 -> ext2 (ilma journal'ita).
Vorminda /dev/sdc2 -> ext4 (koos journal'iga).
Haagi need vastavalt /mnt/lab_ext2 ja /mnt/lab_ext4.
Stressitest (Andmekirjutamine):

Ava kaks terminaliakent (või kasuta tmux/screen).
Terminal 1 (kirjutab ext2-le): sudo dd if=/dev/urandom of=/mnt/lab_ext2/crash_test bs=1M count=2000
Terminal 2 (kirjutab ext4-le): sudo dd if=/dev/urandom of=/mnt/lab_ext4/crash_test bs=1M count=2000
Simulatsioon:

Käivita mõlemad käsud.
KOHE (kui kirjutamine käib), sunni VM jõuga kinni (VirtualBox -> Close -> Power off the machine). Ära tee Shut Down.
Taastamine:

Käivita VM uuesti.
Enne haakimist käivita failisüsteemi kontroll: sudo fsck -fy /dev/sdc1 (ext2) sudo fsck -fy /dev/sdc2 (ext4)
Analüüs: Võrdle fsck väljundit. Kumb failisüsteem leidis rohkem vigu või nõudis rohkem parandusi? Selgita raportis, mis on "Journaling" roll ja miks ext2 on ebaturvaline.
ÜLESANNE 4: Inode'ide ammendamine (DDoS simulatsioon)
Tegevus: Tekitada olukord, kus ketas on "täis", kuigi vaba ruumi on veel megabaitides.

Ettevalmistus:

Loo kettale /dev/sdb (kui on vaba) uus väike partitsioon, suurusega ca 50 MB.
Vorminda see ext4-ks.
Haagi see /mnt/inode_test.
Info kogumine:

Vaata vaba ruumi: df -h /mnt/inode_test
Vaata vabu Inode: df -i /mnt/inode_test
Rünnak: Loo Bash skript, mis tekitab tuhandeid tühje faile:

Bash

#!/bin/bash
echo "Alustan failide loomist..."
for i in {1..100000}; do
    touch /mnt/inode_test/file_$i 2>/dev/null || break
done
echo "Lõpetatud. Failisüsteem keeldus uusi faile loomast."
Käivitus: Käivita skript ja oota, kuni see peatub.
Tõestus:

Proovi luua käsitsi üks fail: touch /mnt/inode_test/uus_fail. See peab ebaõnnestuma veaga "No space left on device".
Kontrolli uuesti df -h (kas ruumi on?) ja df -i (kas Inode on?).
Analüüs: Selgita, mis on Inode (Index Node) ja miks miljon 1KB faili on serverile koormavam kui üks 1GB fail.
ÜLESANNE 5: LVM (Logical Volume Manager) ja dünaamiline laiendamine
Tegevus: Ühendada mitu füüsilist ketast üheks loogiliseks grupiks ja laiendada failisüsteemi töö käigus (live-resize).

Füüsiline kiht (PV):

Kasuta ketast /dev/sdd (10GB) ja /dev/sdb (5GB).
Initsialiseeri mõlemad LVM-i jaoks: sudo pvcreate /dev/sdd /dev/sdb.
Grupi kiht (VG):

Loo uus Volume Group nimega server_storage, mis sisaldab esialgu ainult ketast /dev/sdd.
Käsk: sudo vgcreate server_storage /dev/sdd.
Loogiline kiht (LV):

Loo grupi sisse Logical Volume nimega andmed, suurusega 2GB.
Käsk: sudo lvcreate -n andmed -L 2G server_storage.
Failisüsteem ja kasutus:

Vorminda see köide (/dev/server_storage/andmed) failisüsteemiks XFS.
Haagi see /mnt/lvm_storage.
Loo sinna fail andmed.txt sisuga "Olen siin".
Laiendamine (Extend):

Oletame, et 2GB sai täis. Laienda köidet 5GB peale.
sudo lvextend -L 5G /dev/server_storage/andmed.
Laienda failisüsteemi (et OS näeks uut ruumi): sudo xfs_growfs /mnt/lvm_storage.
Kontrolli df -h väljundit. Kas fail andmed.txt jäi alles?
Grupi laiendamine:

Laienda Volume Groupi, lisades sinna teise ketta (/dev/sdb): sudo vgextend server_storage /dev/sdb.
Nüüd on sul grupis kokku ca 15GB ruumi.
ÜLESANNE 6: Püsiv seadistus (/etc/fstab)
Tegevus: Muuta LVM köite haakimine automaatseks, et see säiliks pärast restarti.

Identifitseerimine: Leia loodud LVM köite UUID.

Kasuta käsku: sudo blkid | grep andmed.
Konfigureerimine:

Ava fail /etc/fstab tekstiredaktoriga (nano või vim).
Lisa faili lõppu uus rida järgmises formaadis: UUID="SINU-UUID-KOOD" /mnt/lvm_storage xfs defaults 0 0
Testimine (Kriitiline!):

Enne restarti testi konfiguratsiooni käsuga sudo mount -a. Kui see annab veateate, paranda fail kohe! Vigane fstab võib takistada arvuti käivitumist.
Restart: Tee virtuaalmasinale restart ja veendu, et kaust /mnt/lvm_storage on automaatselt haagitud.
TULEMUSTE ESITAMINE
Koosta lühike tehniline raport (PDF/Word), mis sisaldab:

FAT32 vs exFAT: Veateate ekraanipilt ja selgitus failisuuruse piirangust.
Permissions: Selgitus, miks NTFS ei toeta Linuxi õigusi ja mis on selle turvariskid.
Journaling: fsck väljundite võrdlus (ext2 vs ext4) ja selgitus "Journali" tööpõhimõttest.
Inode: Ekraanipilt df -i väljundist ja selgitus, kuidas Inode'ide lõppemine mõjutab serverit.
LVM: Käsud, mida kasutasid köite (LV) ja failisüsteemi laiendamiseks.
Fstab: Koopia sinu /etc/fstab faili viimasest reast.

# lahendus 

<img width="491" height="307" alt="image" src="https://github.com/user-attachments/assets/d3d53260-6a3b-4387-b549-a77314b77d1e" />

# 1.2
<img width="577" height="98" alt="image" src="https://github.com/user-attachments/assets/386cd1ac-2127-4d5c-90c5-6d979f8112f3" />

# 1.3
<img width="915" height="256" alt="image" src="https://github.com/user-attachments/assets/55768838-123d-446f-add8-85dbe850fbb4" />

# 1.4
<img width="915" height="154" alt="image" src="https://github.com/user-attachments/assets/01ea481b-8659-4a3f-967f-d2c5276040b7" />

# 1.5
Mõned failid on liiga suured ja piirang on 4GB 
# 1.6
<img width="917" height="599" alt="image" src="https://github.com/user-attachments/assets/0ad95219-2e7d-418d-b960-138bec56974b" />

# 2.1
<img width="854" height="212" alt="image" src="https://github.com/user-attachments/assets/cd1feba1-f5d4-4ba9-b9e2-26a1774d2e83" />


# 2.2
<img width="919" height="306" alt="image" src="https://github.com/user-attachments/assets/12f5a7a9-a722-4263-8b32-eae60748cb79" />


# 2.4
Chmod ei anna veateadet, kuna NTFS failisüsteem ei saa Linux hallata failide õigusi. NTFS kasutab oma mudelit ACL, mida linux ei toeta 100%. Mount Options maarab ara kuidas failide õiguseid käsitletakse. 

# 2.5
<img width="947" height="591" alt="image" src="https://github.com/user-attachments/assets/ee6da725-22bd-4cdf-833d-61021ca8876e" />

<img width="927" height="119" alt="image" src="https://github.com/user-attachments/assets/a46a969c-4d89-459b-a817-ae8903a4e647" />

Ei luba luua uut partitsooni, mingil põhjusel, seega skippin selle osa ig. 

# 3.1
<img width="913" height="337" alt="image" src="https://github.com/user-attachments/assets/87dae862-431d-4bc9-ad1f-eee9a22416ee" />

# 3.2
<img width="928" height="466" alt="image" src="https://github.com/user-attachments/assets/80f242d0-c630-474a-8810-62fe30cd4c32" />


# 3.3
Töötas 

# 3.4
ext2
<img width="637" height="241" alt="image" src="https://github.com/user-attachments/assets/cffb3216-af97-4bd3-b899-b0b6f24fd476" />
ext1



# 3.5
ext2 nõuab rohkem parandusi ja on vigade suhtes tundlikum kui journaling-failisüsteemid. 

Journaling tähendab, et failisüsteem peab spetsiaalset logi (journal), kuhu salvestatakse muudatused enne, kui need kettale lõplikult kirjutatakse. 

ext2 ei kasuta journaling’ut, mis teeb selle ebaturvalisemaks. 

# 4.1
