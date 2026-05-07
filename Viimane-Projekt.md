# Viimane-Projekt-
Pole ules laetud
Viimaseks projektiks võtame suure osa mis me õppinud oleme, ning lisame sinna ka juurde asju mida saate iseseisvalt õppida.
Enda dokumentatsiooni või lahenduste jagamine teistele töö tegijatele, on keelatud!

Ettevalmistus punktid:
1. Arvestada sellega, et ma tahan 2 dokumentatsiooni, 1 dokumentatsioon on "ametlik" dokumentatsioon mis kirjeldab tervet sinu süsteemi ning kuidas seda hallata, sarnane asi mis sa õppisid siin: https://moodle.edu.ee/mod/assign/view.php?id=2871194
1.2. Teine dokumentatsioon on dokumentatsioon mida oled harjutanud mulle tegema, mis "tõestab" et sa oled projekti ära teinud. Arvesta, et ma tahan et virtuaalmasinad oleksid sinu nimelised. Dokumentatsioonis ma tahan näha samm sammulist töötavat lahendust, kuidas sa selle lahendasid ei ole nii tähtis. Näide: Ma tahan näha et kui sa näiteks pagari kasutajaga tahad minna autotöökoja kaustadesse, siis sa ei saa.
1.3. Dokumentatsioonis ma ei taha näha erroreid ega mingeid lambiseid screenshote, ainult vajalikud ja töötavad asjad.
2. Kui tuled mu juurde jutuga et "mul jäi *see* tegemata sest mu VM läks katki" - siis ma vastan sulle "õige sysadmin teeb backupe ja snapshotte enda tööst, unlucky".
3. Ma ei taha näha commande ja koodi mis on overengineered by chatGPT, kui sulle on õpetatud commandi kasutajate tegemiseks "sudo useradd robin" siis ma ei taha näha sellist commandi: sudo bash -c 'u=robin; id "$u"||{ useradd -m -s /bin/bash "$u"; chmod 750 "/home/$u"; }'
4. For your own sake tehke asju võimalikult lihtsalt, suure tõenäosusega ma küsin sinu töö kohta küsimusi ja kui sa kohapealt mulle vastata ei oska - siis sa ju aru ei saa? - samuti scriptidega, kõik scriptid mis sa kirjutama pead on mul varasemates ülesannetes suures pildis ette tehtud, kui sa annad mulle 100 realise scripti mille eesmärk on liigutada fail ühest kohast teise, siis pigem ole valmis selgitama igat rida mis seal scriptis on.
5. Kuna suure tõenäosusega sa ei jõua seda tööd 1 õppepäevaga valmis, siis kasuta vabalt valitud lahendust et olla kindel et sinu töö salvestub ja saad seda kodus/järgmie koolipäev edasi teha.
6. Tööjuhendist on nimelt välja jäetud paar punkti - need on need asjad mida süsteemiadministraator teab, et peab tegema. Toon ühe näite - kui ülesandes on kirjas, et tee script mis teeb /home/robin/Desktop kasutast tar.gz backupi, siis sina teed selle scripti, ja ISESEISVALT tead, et iga script peab/võiks logisid teha. 
7. Arvesta, et dokumentatsioon on nimelt jäetud keerukamaks kui see olla võiks. See on see hetk kui sa pead enda AJU kasutama, ja mõtlema loogiliselt milline lahenduskäik on kõige mõistlikum

Minu isiklik soovitus: Kuna projekt on natukene aeganõudvam ja tõenäoliselt ei saa copy paste commandidega seda tehtud chatgpt abil 1 tunniga, siis ma soovitan teha oma peaga need ülesanded - põhjenduseks toon selle, et siis te saate ise ka aru mis toimub. Sügavalt rõhutan, et väga tähtis on, et te ise saate kõigest aru ja ei ole nii, et "aga see ju töötab" - kui ma küsin teilt, et selgita kuidas mingi asi töötab ja sa ei oska selgitada, siis see läheb kirja kui tegemata.

Iseseisvalt ülesande täitmise täiendamine asjadele mis on vajalik kuid juhendis pole kirjas - 1 hindepunkt
Süsteemist ja failipuust arusaamine - 1 hindepunkt
Dokumentatsioonide välimus ja kasutuslikkus - 1 hindepunkt
Nõuete täitmine - 2 hindepunkti kuid bare minimum.



 

LÕPUPROJEKT
Eesmärk: Ehitada nullist tootmiskõlbulik (production-ready) server, mis majutab kolme klienti, on turvatud, automatiseeritud ja monitooritud.


1. Stsenaarium ja Arhitektuur
Sina oled süsteemiadministraator ettevõttes. Sinu serverisse kolivad sisse kolm nõudlikku klienti:

"Pagariäri OÜ"
"Autotöökoda AS"
"Advokaadibüroo OÜ"
Sinu ülesanne on seadistada ÜKS Ubuntu server, mis teenindab neid kõiki korraga.

Põhireeglid (Kuldne Standard):

Isolatsioon: Ükski klient ei tohi näha teise kliendi faile ega protsesse.
Hierarhia: Igal ettevõttel on 2 töötajat (Juhataja ja Assistent/Töötaja), kellel on oma ettevõtte piires erinevad õigused.
Turvalisus: Server peab olema rünnakukindel (Firewall, Fail2Ban, SSH Key-only).
Automatiseerimine: Käsitöö on keelatud – backupid, monitooring ja logid peavad toimima iseseisvalt.
2. Tehnilised Nõuded
Kettahaldus (LVM)
Serverile on lisatud 3 tühja kõvaketast. Sinu ülesanne on need liita ja jagada loogiliselt.

Loo Volume Group (VG) nimega vg_hosting.
Loo järgmised Logical Volumes (LV):
/srv/clients – Siin asuvad klientide andmed.
/srv/backups – Siia lähevad varukoopiad.
/var/log/hosting – Siia suunatakse sinu skriptide ja ettevõtete admin-logid.
Mõtle ise, kas on vaja veel LV-sid teha?
Veendu, et need haagitakse (mount) automaatselt serveri käivitamisel (/etc/fstab).
Kasutajad, Grupid ja Õigusted
See on projekti kõige keerulisem osa. Vale õigus = turvaintsident.

Struktuur:
Grupid: grp_pagar, grp_auto, grp_advokaat.
Kasutajad (kokku 6 + sina):
pagar_juhataja, pagar_töötaja (Grupp: grp_pagar)
auto_juhataja, auto_töötaja (Grupp: grp_auto)
advokaat_juhataja, advokaat_töötaja (Grupp: grp_advokaat)
Failipuu (Iga kliendi kohta identne):
Asukoht: /srv/clients/[kliendi_nimi]/
public_html/ – Veebilehe failid. Mõlemad töötajad saavad lugeda/kirjutada.
private_docs/ – Ainult Juhataja saab lugeda/kirjutada. Tavaline töötaja ei tohi sissegi saada.
admin_logs/ – Selles kaustas on fail system_audit.log. Seda faili tohib kirjutada/lugeda AINULT serveri admin (sina/root) ja mitte keegi teine (isegi mitte juhataja).
Ehk iga kliendi kohta on logi kaustad, dokumentide kaustad, htmli kaust ja muud vajalikud. Ehk kõik on eraldatud.
SSH Ligipääs:
Keela parooliga sisselogimine terves serveris.
Kõik 6 kasutajat peavad sisse saama ainult SSH võtmega.
Kasutajad peavad olema Chrooted (vangistatud) oma kodukausta /srv/clients/[nimi]. Nad ei tohi näha /etc ega teiste klientide kaustu.
Teenused
Apache2 Veebiserver:
Seadista 3 Virtual Hosti:
www.pagar.local serveerib /srv/clients/pagar/public_html
www.auto.local serveerib /srv/clients/auto/public_html
www.advokaat.local serveerib /srv/clients/advokaat/public_html
Iga lehe jaoks peab olema eraldi AccessLog ja ErrorLog kaustas /var/log/hosting/apache_[nimi]/.
Samba (SMB) Failiserver:
Jaga välja /srv/clients all olevad kolm peakausta (pagar, auto, advokaat).
Reegel: pagar_töötaja saab ühendada Windowsist oma firma kausta, aga ta ei tohi saada siseneda private_docs, logs ja public_html kausta.
Kõrgema õiguse töötajana saan ühendada ainult enda ettevõtte kausta, ning teha igalpool vajalikke muudatusi va. admin_logsis. Mina näen kõiki olemasolevat informatsiooni mis mulle kuulub.
Madalama astme töötajana saan näha ainult public_docs kausta ja seal sees olevaid faile ning teha seal muudatusi, muud õigused mul puuduvad.
Tee kindlaks, et kaustades on päriselt mingid failid ole kui demonstreerid enda õigusi.
Automatiseerimine ja Skriptimine (Bash + Cron + Git)
GitHub Integratsioon:
Veebilehtede sisu (index.html, style.css).
Loo skript /usr/local/bin/deploy_websites.sh.
Skript peab tõmbama (git clone/pull) sisu sinu avalikust GitHubi repositooriumist ja jaotama need õigesse kausta (public_html) kõigile kolmele kliendile.
"Suur Vend" (Audit Skript):
Skript audit_permissions.sh, mis kontrollib iga tund (Cron):
Kas keegi on andnud failidele 777 õigused? Kui jah, muuda need automaatselt 750 peale.
Kirjuta intsidendi kohta märge vastava firma admin_logs/system_audit.log faili.
Backup Skript:
Skript daily_backup.sh.
Pakib kokku (tar.gz) iga kliendi kausta eraldi.
Failinimi peab sisaldama kuupäeva ja kliendi nime: backup_pagar_2023-10-25.tar.gz.
Salvestab failid backups/ kausta.
Kustutab automaatselt failid, mis on vanemad kui 7 päeva.
Turvalisus ja Monitooring
Firewall (UFW):
Kõik pordid vaikimisi kinni (deny incoming).
Luba ainult: SSH (22), HTTP (80), Samba (139/445) ja monitooringu port.
Fail2Ban:
Seadista Fail2Ban kaitsma SSH-d (sshd jail).
Test: Proovi vale võtmega/parooliga 3 korda siseneda ja veendu, et sinu IP saab bänni (näita logi).
Monitooring (3. osapool):
Paigalda kerge monitooringu tööriist (nt Glances, Netdata, Cockpit või kirjuta ise HTML-i genereeriv bash skript).
See peab olema kättesaadav veebis (nt http://server-ip:19999 või http://server-ip/monitor.html).
Peab näitama: CPU, RAM ja vg_hosting kettakasutust.
Dokumentatsioon
Lihtsalt "asi töötab" ei anna punkte. Vaja on Administraatori Käsiraamatut (PDF).

Nõutud peatükid (MINE TAGASI JA VAATA DOKUMENTEERIMISE ÜL, SEAL ON SAMUTI NÕUDEID KIRJAS MIS PEAVAD OLEMA ADMINI KÄSIRAAMATUS!:

Õiguste tabel:
Kuvatõmmis ls -laR /srv/clients koos selgitusega, miks pagar_töötaja ei saa private_docs kausta.
Teenuste tõestus (Kuvatõmmised):
Samba: Pilt Windowsi "Map Network Drive" vaatest ja veateatest, kui töötaja üritab avada private_docs kausta.
Veeb: Pilt brauserist, kus on avatud pagar.local, auto.local ja advokaat.local (kõik peavad olema erineva sisuga).
Turvalisus: Pilt sudo ufw status verbose ja Fail2Ban logist, kus on näha IP blokeerimine.
Skriptid:
Lisa dokumentatsiooni kõigi 3 skripti (deploy, audit, backup) täielik kood.
Pilt Crontab-ist (crontab -l).
Taasteplaan:
Lühike juhend (step-by-step): "Kui server hävineb, kuidas ma taastan Advokaadibüroo andmed kaustast /srv/backups?"



Arhitektuuri joonis:
Joonis, mis näitab ketaste jaotust, LVM-i ja failipuu õiguste loogikat.
Näide (sa ei pea tegema nii ilusat joonist, loeb mõte et saaksid enda filipuust ja õigustest aru):

# minu osa

Kettahaldus (LVM)
<img width="571" height="769" alt="image" src="https://github.com/user-attachments/assets/ac043ae2-0359-4275-894b-fa39c5331c13" />

Et nad käivitaksid automaatselt peame lisama LV kettad /etc/fstab
<img width="576" height="274" alt="image" src="https://github.com/user-attachments/assets/e9973950-3aaf-47a9-829c-e0eddcf324c6" />
# Kasutajad, Grupid ja Õigusted


<img width="567" height="211" alt="image" src="https://github.com/user-attachments/assets/56835b28-3eac-48cc-acf3-349fe127a785" />
Lisame pagariäri
<img width="744" height="79" alt="image" src="https://github.com/user-attachments/assets/9b82f22f-181c-4acd-bf61-018ebf245c3b" />


Lisame autoäri
<img width="727" height="72" alt="image" src="https://github.com/user-attachments/assets/0f029576-21cc-4a5a-94fb-05c202e85b85" />


Lisame advokaatiäri
<img width="747" height="356" alt="image" src="https://github.com/user-attachments/assets/19c2cb6c-da38-4456-908c-359532a6f6e9" />

TEST
<img width="750" height="674" alt="image" src="https://github.com/user-attachments/assets/19f96175-1b35-4d7c-a3ec-91e27bc12c83" />
Lubame teha SSH võtme
<img width="287" height="40" alt="image" src="https://github.com/user-attachments/assets/baf2d39e-c8e5-4482-ab07-fc9f4711d2b5" />
Enne oli yes, muidu siis küsiks parooli aga enam ei küsi
<img width="310" height="33" alt="image" src="https://github.com/user-attachments/assets/3efcfa26-d44c-4702-b369-b7be7024d43e" />

Paneme selle rea lõppu, et keelada gruppil ssh ühenduse tegemise

<img width="632" height="439" alt="image" src="https://github.com/user-attachments/assets/bd7c5ba3-ef98-4b29-99d1-bd7155e956da" />

Teeme root kaustad, et vangistada kasutajad oma kausta
<img width="744" height="279" alt="image" src="https://github.com/user-attachments/assets/3d1444e2-f2fe-49b5-85bb-16d37939ef9e" />


Testime
<img width="742" height="400" alt="image" src="https://github.com/user-attachments/assets/65580fdc-3bff-4090-a6f0-f99c77d9fd54" />


Skript mis annab kõigile juhatajatele samad omadused
USERS=("pagar_tootaja" "auto_juhataja" "auto_tootaja" "advokaat_juhataja"
"advokaat_tootaja")
for user in "${USERS[@]}"; do
sudo mkdir -p /home/$user/.ssh
sudo cp /home/pagar_juhataja/.ssh/authorized_keys /home/$user/.ssh/
GROUP_NAME="grp_${user%_*}"
sudo chown -R $user:$GROUP_NAME /home/$user/.ssh
sudo chmod 700 /home/$user/.ssh
sudo chmod 600 /home/$user/.ssh/authorized_keys
sudo usermod -p "*" $user
echo "Kasutaja $user on valmis."

# teenused

Teeme kaustad ja anname teile õigused
<img width="738" height="91" alt="image" src="https://github.com/user-attachments/assets/a48294c5-730c-4f0d-8356-86c0e269566b" />

Muutame conf faili igal gruppi, et et neil oleks oma logid ja õiged teed. Ehk
veebilehed oleksid olemas.


<img width="742" height="702" alt="image" src="https://github.com/user-attachments/assets/01564889-718f-4e39-8515-adfb154ec9c1" />

<img width="741" height="288" alt="image" src="https://github.com/user-attachments/assets/25ee9f6d-d155-4096-834c-15b38cd19768" />

Veebisisu asukohad

<img width="744" height="427" alt="image" src="https://github.com/user-attachments/assets/701afd8c-37de-423d-9736-73a1f225f247" />

Keelame töötajatel, mitte ligi saada teatud kaustadese

<img width="745" height="606" alt="image" src="https://github.com/user-attachments/assets/f69105a7-59a9-4d20-9e6e-0ea534a8b5e8" />

Pagari juhataja ei saa ligi tähtsamatele failidele/folderitele
<img width="750" height="235" alt="image" src="https://github.com/user-attachments/assets/dfbef89b-5114-4d11-ae60-470f9f88d7fa" />

Tootajal pole ligipaasu tähtsamatele failidele.
<img width="748" height="287" alt="image" src="https://github.com/user-attachments/assets/84083bee-ccec-4b5f-ae36-b68962eb8cf4" />

Tegin näide, et on mingu sugune ligpääs ikkagi
<img width="740" height="41" alt="image" src="https://github.com/user-attachments/assets/1baad410-18f8-4a38-a14c-aa2479d64972" />


# Automatiseerimine ja Skriptimine (Bash + Cron + Git)Tegin näide, et on mingu sugune ligpääs ikkagi
<img width="752" height="304" alt="image" src="https://github.com/user-attachments/assets/c40bbf34-4132-4559-981c-80d4f1947f3b" />

see skript tõmbab sisu GitHubist ja jaotab selle kolme kliendi vahel.
Paneme õigused ja kopeerimine sisu
Pakime kausta kokku ja teeme backupi kausta
<img width="740" height="415" alt="image" src="https://github.com/user-attachments/assets/1fa32c34-c821-481d-94e4-77631d445e81" />

Paneme õigused ja kopeerimine sisu
<img width="743" height="70" alt="image" src="https://github.com/user-attachments/assets/8695ca6d-b6e3-4443-a8c4-57793bde680d" />

Pakime kausta kokku ja teeme backupi kausta
<img width="742" height="326" alt="image" src="https://github.com/user-attachments/assets/522b82cb-e237-44e7-92ac-deb4bbb0e049" />

<img width="744" height="541" alt="image" src="https://github.com/user-attachments/assets/5598c713-e603-4de8-a288-4373c020fe4b" />

<img width="749" height="446" alt="image" src="https://github.com/user-attachments/assets/ed95ebe8-c615-474c-ab8b-9af64084cab1" />

Ja töötab ka
<img width="742" height="64" alt="image" src="https://github.com/user-attachments/assets/6d62fb49-8241-4aca-9faa-62a6315d6cdc" />

# Turvalisus ja Monitooring
<img width="550" height="27" alt="image" src="https://github.com/user-attachments/assets/9918ea18-feac-4724-9e50-4d16985da96b" />
Sise lubab aga mitte valja

<img width="645" height="139" alt="image" src="https://github.com/user-attachments/assets/94e21d83-23be-4a3a-ac4a-33d036b74a92" />

Avame pordid
<img width="691" height="381" alt="image" src="https://github.com/user-attachments/assets/8e08e617-5e32-43d8-b82c-34f98fbc3110" />

Lülitame firewalli sisse
<img width="739" height="74" alt="image" src="https://github.com/user-attachments/assets/d5a98f4b-8a27-437a-89d6-197e66bac511" />

Tõmbame alla fail2ban
<img width="621" height="44" alt="image" src="https://github.com/user-attachments/assets/d995ac91-c789-4577-abdb-8c12d7ba7e06" />

Loome seadistus faili
<img width="542" height="127" alt="image" src="https://github.com/user-attachments/assets/1a4789ca-81c7-4a9b-884d-761426f92e09" />

Proovin sisse logita mingi x kontoga
<img width="652" height="252" alt="image" src="https://github.com/user-attachments/assets/a111e1cd-f572-46b1-aa29-cb994a7b37dc" />

Serveri vaade
<img width="682" height="229" alt="image" src="https://github.com/user-attachments/assets/ad61a506-92c3-4c4b-9968-253b9b55fde3" />

Paigaldame netdata
<img width="943" height="42" alt="image" src="https://github.com/user-attachments/assets/dcef1f80-b4c2-40b3-8116-87e0c67a7962" />

<img width="846" height="643" alt="image" src="https://github.com/user-attachments/assets/1bf0c1de-01f7-4f6d-97a7-2fda6c88cef1" />

<img width="890" height="596" alt="image" src="https://github.com/user-attachments/assets/e7462d69-ee3e-43c9-a519-0237dbc15cb5" />

# Tähtsamad kontrollid

<img width="680" height="274" alt="image" src="https://github.com/user-attachments/assets/1e0e945e-baaf-4478-8751-d2bb841c0196" />

<img width="722" height="612" alt="image" src="https://github.com/user-attachments/assets/5d187a8a-e5dc-4ab8-bbb2-84a4d3fbd5cd" />

Suur Vend/Audit
Öine backup

GitHubi uuendus
<img width="728" height="225" alt="image" src="https://github.com/user-attachments/assets/5ac682b2-7e26-43ce-a20f-6ab0833bdba2" />

# Dokumentatsioon
GitHubi uuendus
ron@ron-VirtualBox:~$ ls -laR /srv/clients
/srv/clients:
total 36
drwxr-xr-x 6 root root 4096 Jan 19 19:43 .
drwxr-xr-x 4 root root 4096 Jan 19 19:04 ..
drwxr-xr-x 3 root root 4096 Jan 19 19:43 advokaat_root
drwxr-xr-x 3 root root 4096 Jan 19 19:42 auto_root
drwxr-xr-x 2 root root 16384 Jan 19 19:03 lost+found
drwxr-xr-x 3 root root 4096 Jan 19 19:42 pagar_root
/srv/clients/advokaat_root:
total 12
drwxr-xr-x 3 root root 4096 Jan 19 19:43 .
drwxr-xr-x 6 root root 4096 Jan 19 19:43 ..
drwxr-xr-x 6 root grp_advokaat 4096 Jan 19 20:18 advokaat
/srv/clients/advokaat_root/advokaat:
total 24
drwxr-xr-x 6 root grp_advokaat 4096 Jan 19 20:18 .
drwxr-xr-x 3 root root 4096 Jan 19 19:43 ..
drwxr-xr-x 2 root grp_advokaat 4096 Jan 19 19:32 admin_logs
drwxr-xr-x 2 root grp_advokaat 4096 Jan 19 22:03 backups
drwxr-xr-x 2 root grp_advokaat 4096 Jan 19 19:32 private_docs
drwxr-x--- 2 advokaat_juhataja grp_advokaat 4096 Jan 19 20:36 public_html
/srv/clients/advokaat_root/advokaat/admin_logs:
total 8
drwxr-xr-x 2 root grp_advokaat 4096 Jan 19 19:32 .
drwxr-xr-x 6 root grp_advokaat 4096 Jan 19 20:18 ..
-rwxr-xr-x 1 root grp_advokaat 0 Jan 19 19:32 system_audit.log

/srv/clients/advokaat_root/advokaat/backups:
total 12
drwxr-xr-x 2 root grp_advokaat 4096 Jan 19 22:03 .
drwxr-xr-x 6 root grp_advokaat 4096 Jan 19 20:18 ..
-rw-r--r-- 1 root root 328 Jan 19 22:03 backup_advokaat_2026-01-19.tar.gz
/srv/clients/advokaat_root/advokaat/private_docs:
total 8
drwxr-xr-x 2 root grp_advokaat 4096 Jan 19 19:32 .
drwxr-xr-x 6 root grp_advokaat 4096 Jan 19 20:18 ..
ls: cannot open directory '/srv/clients/advokaat_root/advokaat/public_html': Permission
denied
/srv/clients/auto_root:
total 12
drwxr-xr-x 3 root root 4096 Jan 19 19:42 .
drwxr-xr-x 6 root root 4096 Jan 19 19:43 ..
drwxr-xr-x 6 root grp_auto 4096 Jan 19 20:18 auto
/srv/clients/auto_root/auto:
total 24
drwxr-xr-x 6 root grp_auto 4096 Jan 19 20:18 .
drwxr-xr-x 3 root root 4096 Jan 19 19:42 ..
drwxr-xr-x 2 root grp_auto 4096 Jan 19 19:32 admin_logs
drwxr-xr-x 2 root grp_auto 4096 Jan 19 22:03 backups
drwxr-xr-x 2 root grp_auto 4096 Jan 19 19:32 private_docs
drwxr-x--- 3 auto_juhataja grp_auto 4096 Jan 19 21:45 public_html
/srv/clients/auto_root/auto/admin_logs:
total 8
drwxr-xr-x 2 root grp_auto 4096 Jan 19 19:32 .
drwxr-xr-x 6 root grp_auto 4096 Jan 19 20:18 ..
-rwxr-xr-x 1 root grp_auto 0 Jan 19 19:32 system_audit.log
/srv/clients/auto_root/auto/backups:
total 12
drwxr-xr-x 2 root grp_auto 4096 Jan 19 22:03 .
drwxr-xr-x 6 root grp_auto 4096 Jan 19 20:18 ..
-rw-r--r-- 1 root root 371 Jan 19 22:03 backup_auto_2026-01-19.tar.gz
/srv/clients/auto_root/auto/private_docs:
total 8
drwxr-xr-x 2 root grp_auto 4096 Jan 19 19:32 .
drwxr-xr-x 6 root grp_auto 4096 Jan 19 20:18 ..
ls: cannot open directory '/srv/clients/auto_root/auto/public_html': Permission denied
/srv/clients/lost+found:
total 20

drwxr-xr-x 2 root root 16384 Jan 19 19:03 .
drwxr-xr-x 6 root root 4096 Jan 19 19:43 ..
/srv/clients/pagar_root:
total 12
drwxr-xr-x 3 root root 4096 Jan 19 19:42 .
drwxr-xr-x 6 root root 4096 Jan 19 19:43 ..
drwxr-xr-x 7 pagar_juhataja grp_pagar 4096 Jan 19 20:49 pagar
/srv/clients/pagar_root/pagar:
total 28
drwxr-xr-x 7 pagar_juhataja grp_pagar 4096 Jan 19 20:49 .
drwxr-xr-x 3 root root 4096 Jan 19 19:42 ..
dr-x------ 2 pagar_juhataja grp_pagar 4096 Jan 19 19:24 admin_logs
drwxr-xr-x 2 pagar_juhataja grp_pagar 4096 Jan 19 22:03 backups
drwx------ 2 pagar_juhataja grp_pagar 4096 Jan 19 20:49 private_docs
drwxrwx--- 2 pagar_juhataja grp_pagar 4096 Jan 19 21:01 public_docs
drwxr-x--- 2 pagar_juhataja grp_pagar 4096 Jan 19 20:27 public_html
ls: cannot open directory '/srv/clients/pagar_root/pagar/admin_logs': Permission denied
/srv/clients/pagar_root/pagar/backups:
total 16
drwxr-xr-x 2 pagar_juhataja grp_pagar 4096 Jan 19 22:03 .
drwxr-xr-x 7 pagar_juhataja grp_pagar 4096 Jan 19 20:49 ..
-rw-r--r-- 1 root root 617 Jan 19 21:31 backup_pagar_2024-05-20.tar.gz
-rw-r--r-- 1 root root 1471 Jan 19 22:03 backup_pagar_2026-01-19.tar.gz
ls: cannot open directory '/srv/clients/pagar_root/pagar/private_docs': Permission denied
ls: cannot open directory '/srv/clients/pagar_root/pagar/public_docs': Permission denied
ls: cannot open directory '/srv/clients/pagar_root/pagar/public_html': Permission denied

miks pagar_töötaja ei saa private_docs kausta
Kuna docsil on 700 õigused ehk aint omanik saab ligi ja gruppil on sättitud, et sinna ligi
ei saaks.

<img width="618" height="535" alt="image" src="https://github.com/user-attachments/assets/8726c235-b6d3-4a19-8749-abf28f4f9da8" />

# Veebilehed
<img width="443" height="200" alt="image" src="https://github.com/user-attachments/assets/e3e1bd2f-2e1f-4722-9921-7b6d803fefdd" />
<img width="727" height="479" alt="image" src="https://github.com/user-attachments/assets/c6c77b5e-a171-492c-b75e-ac2f967d1072" />

Pilt sudo ufw status verbosest
<img width="726" height="401" alt="image" src="https://github.com/user-attachments/assets/5b68210f-92ee-46a3-b18d-8cef5ba1f2bc" />

Fail2ban logi
<img width="730" height="543" alt="image" src="https://github.com/user-attachments/assets/75167cb1-aeeb-410d-849e-1d9b7a0d8817" />

Veebilehtede uuendamise skript (deploy_websites.sh)

#!/bin/bash
 Automaatne veebilehtede uuendamine GitHubist
REPO_URL="SINU_GITHUBI_URL_SIIA"
TEMP_DIR="/tmp/veebiaudit_repo"
 Tõmbame värske sisu
rm -rf $TEMP_DIR
git clone $REPO_URL $TEMP_DIR
 Jaotame sisu klientide kaustadesse
clients=("pagar" "auto" "advokaat")
for client in "${clients[@]}"; do
TARGET="/srv/clients/${client}_root/${client}/public_html"
if [ -d "$TEMP_DIR/$client" ]; then
cp -r $TEMP_DIR/$client/* $TARGET
chown -R ${client}_juhat
chmod -R 750 $TARGET
echo "$(date): $client veebileht uuendatud."
fi
done
rm -rf $TEMP_DIR

"Suur Vend" ehk õiguste kontroll (audit_permissions.sh)
#!/bin/bash
 Automaatne õiguste audit ja parandus
LOG_FILE="/srv/clients/pagar_root/pagar/admin_logs/system_audit.log"
 Otsime faile, millel on liiga vabad õigused (nt 777)
FAILED_FILES=$(find /srv/clients -type f -perm 777)
if [ ! -z "$FAILED_FILES" ]; then
echo "$(date) - AUDIT: Leiti ohtlikud õigused! Failid: $FAILED_FILES" >> $LOG_FILE
 Parandame õigused automaatselt 750 peale
find /srv/clients -type f -perm 777 -exec chmod 750 {} +
echo "$(date) - AUDIT: Õigused parandatud 750-le." >> $LOG_FILE
finish

Varundamise skript (daily_backup.sh)
#!/bin/bash
# Igapäevane varundamine
DATE=$(date +%Y-%m-%d)
CLIENTS=("pagar" "auto" "advokaat")
for client in "${CLIENTS[@]}"; do
SRC="/srv/clients/${client}_root/${client}"
DEST="$SRC/backups/backup_${client}_$DATE.tar.gz"
# Pakime sisu (v.a. juba olemasolevad backupid)
tar --exclude="$SRC/backups" -czf $DEST $SRC
# Kustutame vanemad kui 7 päeva backupid
find "$SRC/backups" -name "*.tar.gz" -mtime +7 -delete
done

<img width="520" height="85" alt="image" src="https://github.com/user-attachments/assets/1ea74b41-44e3-492a-9c55-c5f03e0b706d" />

Lühike juhend (step-by-step): "Kui server hävineb, kuidas ma taastan
Advokaadibüroo andmed kaustast /srv/bac

Juhul kui see laguneb, siis on olemas backupfail
<img width="639" height="75" alt="image" src="https://github.com/user-attachments/assets/d19bc881-3b7a-446b-902c-fee769ac2c4d" />
Selles on vaja sinna kausta minna ja liigutada backup kausta
cd /srv/backups
Kasuta tar käsku, et taastada failid otse kliendi kataloogi.
sudo tar -xzvf backup_advokaat_2026-01-19.tar.gz -C /srv/clients/advokaat_root/
Ja siis määrame õigused
sudo chown -R advokaat_juhataja:grp_advokaat /srv/clients/advokaat_root/advokaat
sudo chmod -R 750 /srv/clients/advokaat_root/advokaat
