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

<img width="408" height="288" alt="Screenshot 2026-05-11 at 09 01 22" src="https://github.com/user-attachments/assets/d66afd05-73fc-401c-9306-8016dc4fe16f" />


# 1.2
<img width="563" height="94" alt="Screenshot 2026-05-11 at 09 01 36" src="https://github.com/user-attachments/assets/d182092d-5eea-4f6e-bd1f-988d6f7ba31a" />


# 1.3
<img width="563" height="153" alt="Screenshot 2026-05-11 at 09 01 43" src="https://github.com/user-attachments/assets/d35f9a1a-2cb5-4feb-86ba-ec66e90e36d5" />


# 1.4
<img width="563" height="92" alt="Screenshot 2026-05-11 at 09 01 49" src="https://github.com/user-attachments/assets/9bf8142d-93b6-443e-a4cc-63e86770b9db" />


# 1.5
Mõned failid on liiga suured ja piirang on 4GB 

# 1.6
<img width="563" height="369" alt="Screenshot 2026-05-11 at 09 02 12" src="https://github.com/user-attachments/assets/6974ac3f-fcd9-4587-a35c-7aab2dea664f" />


# 2.1
<img width="526" height="133" alt="Screenshot 2026-05-11 at 09 03 45" src="https://github.com/user-attachments/assets/7b59af46-f2e6-4b95-9736-5cc7d9c57c09" />



# 2.2
<img width="551" height="187" alt="Screenshot 2026-05-11 at 09 03 54" src="https://github.com/user-attachments/assets/6434d386-96ef-40f5-a03e-c8afa435ad87" />



# 2.4
Chmod ei anna veateadet, kuna NTFS failisüsteem ei saa Linux hallata failide õigusi. NTFS kasutab oma mudelit ACL, mida linux ei toeta 100%. Mount Options maarab ara kuidas failide õiguseid käsitletakse. 

# 2.5
<img width="551" height="356" alt="2 5" src="https://github.com/user-attachments/assets/2cf565fb-3e86-4ac4-99f5-a6c9eb0c9ae0" />

<img width="551" height="66" alt="2 5-2" src="https://github.com/user-attachments/assets/2511f45f-f22b-4d53-9493-4cf2ca339cb9" />


Ei luba luua uut partitsooni, mingil põhjusel, seega skippin selle osa ig. 

# 3.1
<img width="555" height="204" alt="Screenshot 2026-05-11 at 09 04 36" src="https://github.com/user-attachments/assets/5d2e94b2-5eae-48ef-86eb-245e6d7ed010" />


# 3.2
<img width="563" height="280" alt="Screenshot 2026-05-11 at 09 04 57" src="https://github.com/user-attachments/assets/c6b16b3b-bba6-410f-ba5c-fa84d924ae2d" />


# 3.3
Töötas 

# 3.4
ext2
<img width="388" height="143" alt="3 4" src="https://github.com/user-attachments/assets/5b965dc2-cbc1-41af-8e72-a8dfc97a1bb1" />

ext1
<img width="569" height="87" alt="3 4-2" src="https://github.com/user-attachments/assets/bb1f765f-d235-400d-85e7-c2b6cd586491" />




# 3.5
ext2 nõuab rohkem parandusi ja on vigade suhtes tundlikum kui journaling-failisüsteemid. 

Journaling tähendab, et failisüsteem peab spetsiaalset logi (journal), kuhu salvestatakse muudatused enne, kui need kettale lõplikult kirjutatakse. 

ext2 ei kasuta journaling’ut, mis teeb selle ebaturvalisemaks. 

# 4.1
<img width="569" height="352" alt="Screenshot 2026-05-11 at 09 05 50" src="https://github.com/user-attachments/assets/62136fda-48a1-40e3-9515-5eb2fd213243" />

# 4.2
<img width="569" height="118" alt="Screenshot 2026-05-11 at 09 05 58" src="https://github.com/user-attachments/assets/ac93cea8-143c-4b87-846c-21afa96114f8" />


# 4.3
<img width="569" height="107" alt="Screenshot 2026-05-11 at 09 06 04" src="https://github.com/user-attachments/assets/18d93676-6bd3-4fab-86cd-40f8d846eb84" />


# 4.4
<img width="436" height="49" alt="Screenshot 2026-05-11 at 09 06 11" src="https://github.com/user-attachments/assets/6dfdc333-ba16-47f4-9485-81b3a35211c0" />


# 5.1
<img width="436" height="59" alt="Screenshot 2026-05-11 at 09 06 18" src="https://github.com/user-attachments/assets/2dfed630-57fd-4371-8bcf-96dbe1290a68" />

# 5.2
<img width="523" height="377" alt="Screenshot 2026-05-11 at 09 06 30" src="https://github.com/user-attachments/assets/058e9810-f05d-4aba-aba5-805b956b12f6" />


# 5.3
<img width="523" height="30" alt="Screenshot 2026-05-11 at 09 06 37" src="https://github.com/user-attachments/assets/3dea7b54-b53d-415f-9a03-370871013604" />


# 5.4
<img width="564" height="278" alt="Screenshot 2026-05-11 at 09 06 46" src="https://github.com/user-attachments/assets/21d7cd6a-6e5c-4538-9362-99708f4a9cc2" />

# 5.5
<img width="564" height="306" alt="Screenshot 2026-05-11 at 09 06 56" src="https://github.com/user-attachments/assets/8df34c89-210e-4f26-afb8-2680e98245a9" />


# 5.6
<img width="564" height="60" alt="Screenshot 2026-05-11 at 09 07 05" src="https://github.com/user-attachments/assets/e9ad4abc-6ef9-4845-90ed-9c5c0a12476c" />


# 6.1
<img width="564" height="60" alt="Screenshot 2026-05-11 at 09 07 09" src="https://github.com/user-attachments/assets/b31fa7f9-3745-48ff-938d-70691d45ca76" />

# 6.2
<img width="564" height="248" alt="Screenshot 2026-05-11 at 09 07 15" src="https://github.com/user-attachments/assets/5fe0a5d5-8144-4ec9-933e-90119632c513" />


# 6.3
<img width="508" height="36" alt="Screenshot 2026-05-11 at 09 07 24" src="https://github.com/user-attachments/assets/764d3352-d1a4-4c6d-8bbe-d2756e27dbad" />

# 7.0
FAT32 vs exFAT

FAT32 max failisuurus 4 GB → viga: “File is too large for the destination file system”
exFAT toetab suuri faile → piirang puudub

Permissions (NTFS)
NTFS kasutab Windowsi ACL-e, Linux kasutab POSIX õigusi
Linuxis õigused emuleeritakse → turvarisk, õigusi ei saa usaldusväärselt hallata

Journaling (ext2 vs ext4)

ext2: puudub journal → fsck aeglane, palju vigu
ext4: journaling → kiire taastumine, vähem vigu
Journal salvestab muudatused enne kettale kirjutamist

Inode
df -i näitab inode’e
inode’id otsas → uusi faile ei saa luua, teenused võivad seiskuda

LVM
lvextend -L +5G /dev/vg/lv
resize2fs /dev/vg/lv

fstab
/dev/vg/lv /mnt/data ext4 defaults 0 2
