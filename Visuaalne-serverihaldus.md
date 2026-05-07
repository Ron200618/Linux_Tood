# Visuaalne-serverihaldus
Pole uleslaetud
See ülesanne tuleb teha ubuntu serveris mitte desktopis.
Ettevalmistus: Lisa endale 3 uut ketast VMi.

Lae alla teenus: cockpit ja cockpit-storaged
Offical url: https://cockpit-project.org/
Kirjelda dokumentatsioonis oma sõnadega mis see on ja too 2 näided kuna see päris elus kasuks tuleb.


Eesmärk: Sinu ülesanne on seadistada Linuxi server, hallata kettaid (LVM), võrku ja teenuseid, kasutades selleks spetsiaalset veebipõhist haldusliidest Cockpit. Ajaline limiit: 3 tundi.

Eeldused:

Sinu käsutuses on värske Ubuntu Server virtuaalmasin.
Virtuaalmasinale on lisatud kaks (2) täiendavat tühja virtuaalset kõvaketast (kumbki vähemalt 5GB).
Stsenaarium
NB! Sinu peamine tööriist on Cockpit. Terminali (SSH) võid kasutada ainult siis, kui Cockpiti graafiline liides vastavat funktsionaalsust ei paku (nt tarkvara esmane paigaldus). Isegi siis peaksid eelistama Cockpiti sisse-ehitatud "Terminal" vahelehte.

1. Etapp: Keskkonna ettevalmistus ja ligipääs
Uuri iseseisvalt, kuidas paigaldada Ubuntu serverile Cockpit haldusliides.
Kuna meil on vaja hallata kettamassiive, pead paigaldama ka Cockpiti lisamooduli, mis tegeleb salvestusruumiga (vihje: paketi nimi sisaldab sõna storaged).
Aktiveeri teenus ja veendu, et see käivitub automaatselt koos arvutiga.
Ava oma host-masina (Windows) veebibrauser ja logi serverisse sisse.
Kontrollpunkt: Pead nägema "Overview" lehel serveri koormust reaalajas.

2. Etapp: Võrguseadistus (Networking)
Server ei tohi kasutada dünaamilist (DHCP) aadressi.

Liigu Cockpitis Networking sektsiooni.
Määra serverile staatiline IP-aadress (vali ise sobiv aadress oma alamvõrgust, nt .20 lõpuga).
Lisa kindlasti ka DNS server (nt 1.1.1.1), et server ei kaotaks internetiühendust.
Salvesta muudatused.
Hoiatus: Pärast IP muutmist kaotab brauser ühenduse. Pead ise uuele IP-le navigeerima.

3. Etapp: Storage Management (LVM) – Kõige tähtsam osa
Sinu serveris on kaks tühja füüsilist ketast. Ettevõte nõuab, et need oleksid liidetud üheks suureks loogiliseks tervikuks (LVM), et tulevikus oleks ruumi lihtne juurde lisada.

Tegevused (ainult Cockpiti "Storage" vaates):

Tuvasta oma kaks lisatud tühja ketast.
Loo nende baasil uus Volume Group (VG) nimega company_storage.
Loo selle grupi sisse uus Logical Volume (LV) nimega fileshare.

Suurus: Kasuta ära kogu vaba maht.
Failisüsteem: XFS.
Haakepunkt (Mount point): Failisüsteem peab olema haagitud kausta /srv/share. (Kui kausta ei eksisteeri, pead leidma viisi see luua).
Krüpteerimine (LUKS):

Turvanõuete tõttu peab see partitsioon olema krüpteeritud.
Seadista see nii, et serveri taaskäivitamisel on ketas lukus ja administraator peab selle Cockpiti kaudu lahti lukustama (või seadista automaatne avamine, kui oskad).
4. Etapp: Kasutajad ja Failijagamine
Kasutajad:

Loo läbi Cockpiti ("Accounts" sektsioon) kaks uut kasutajat:

juhataja (Anna talle "Server Administrator" õigused).
praktikant (Tavakasutaja).
Määra kasutajale praktikant nõue, et ta peab järgmisel sisselogimisel parooli vahetama.
Samba teenus:

Paigalda serverisse Samba tarkvara (siin võid kasutada Cockpiti terminali).
Seadista (konfiguratsioonifaili kaudu), et kaust /srv/share oleks võrgus kättesaadav nimega DriimFailid.
Tagada, et kasutaja juhataja saab sinna kirjutada.
Teenuse juhtimine:

Ära kasuta terminali teenuse restartimiseks!
Mine Cockpiti Services lehele, otsi üles Samba teenus (smbd) ja tee sellele restart ning veendu, et see on "Enabled".
5. Etapp: Automaatika ja Taimerid (Systemd Timers)
Driim OÜ ei kasuta iganenud Cron-i. Me kasutame Systemd Timers lahendust, mida Cockpit suurepäraselt kuvab.

Ülesanne: Luua automaatne logija, mis salvestab iga 5 minuti tagant faili, kui palju vaba kettaruumi serveris on.

Kirjuta lihtne bash script (nt /opt/disk_check.sh), mis kirjutab hetke kellaaja ja kettakasutuse faili /var/log/disk_usage.log.
Loo Systemd Service fail, mis käivitab selle skripti.
Loo Systemd Timer, mis käivitab selle service'i iga 5 minuti järel.
Aktiveeri taimer.
Tõestus: Mine Cockpitis Services -> Timers sektsiooni. Sinu taimer peab seal nimekirjas olema, staatuses "Active" ja näitama, millal on järgmine käivitus ("Next Run").
6. Etapp: Dokumentatsioon ja Tõrkeotsing
Sinu töö vastuvõtmiseks on vaja esitada raport (PDF või Google Doc link). Raport peab sisaldama kuvatõmmiseid (screenshot) järgmistest vaadetest Cockpitis (mitte terminalist!):

Storage vaade: Pilt peab tõestama, et Volume Group company_storage koosneb kahest füüsilisest kettast ja seal asub krüpteeritud XFS failisüsteem.
Logs vaade: Tekita süsteemis mingi viga (näiteks proovi vale parooliga sisse logida või tee vigane konfiguratsioon) ja filtreeri Cockpiti logidest välja see konkreetne veateade.
Timers vaade: Pilt, kus on näha sinu loodud taimer ja selle "Next Run" aeg.
Terminali vaade (brauseris): Tõestus, et failis /var/log/disk_usage.log on andmed olemas.
