# Visuaalne-serverihaldus

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

## minu osa

Cockpit on interaktiivne ja veebipõhine graafiline kasutajaliides Linuxi serverite
haldamiseks. See on loodud selleks, et muuta süsteemi administreerimine lihtsamaks ja
visuaalsemaks, ilma et peaksid kõiki käsurida käske peast teadma
# 1.
<img width="563" height="542" alt="Screenshot 2026-05-11 at 08 27 33" src="https://github.com/user-attachments/assets/41a2cbe2-6224-4391-8b5d-7b6acdeed0ad" />

# 2.
<img width="563" height="542" alt="Screenshot 2026-05-11 at 08 27 44" src="https://github.com/user-attachments/assets/bc51284e-c419-4c8a-9e1c-269b96918d08" />

# 3.
<img width="563" height="179" alt="Screenshot 2026-05-11 at 08 27 54" src="https://github.com/user-attachments/assets/e74d75eb-5439-4613-9f1b-9aabc8270068" />


Tegin mõlemal encrypt
<img width="563" height="211" alt="Screenshot 2026-05-11 at 08 28 01" src="https://github.com/user-attachments/assets/adf20491-b6ae-4d23-a378-8af7540a0707" />



# 4. kasutajad
<img width="563" height="211" alt="Screenshot 2026-05-11 at 08 28 07" src="https://github.com/user-attachments/assets/1807eaf8-9462-4446-81c0-b0d1b380ab82" />

<img width="563" height="211" alt="Screenshot 2026-05-11 at 08 28 07" src="https://github.com/user-attachments/assets/4c4c23f7-6e63-4c3c-81d3-ee01a67eb578" />

<img width="563" height="438" alt="Screenshot 2026-05-11 at 08 28 16" src="https://github.com/user-attachments/assets/b60bef0c-3cc4-4dea-9dd2-c6de54c0c25e" />

<img width="535" height="127" alt="Screenshot 2026-05-11 at 08 28 27" src="https://github.com/user-attachments/assets/b69fa662-48bc-49ce-96db-2de255b14aef" />

<img width="326" height="243" alt="Screenshot 2026-05-11 at 08 28 35" src="https://github.com/user-attachments/assets/a932f997-5e16-4cd8-b7a6-75cb966fdcc6" />

<img width="379" height="157" alt="Screenshot 2026-05-11 at 08 29 02" src="https://github.com/user-attachments/assets/eed97573-b3d3-498e-a7f8-b450c4ad2df1" />

<img width="404" height="223" alt="Screenshot 2026-05-11 at 08 29 08" src="https://github.com/user-attachments/assets/6ed19b2d-b187-46ff-a6f4-d8a677a90912" />


# 5.

<img width="541" height="223" alt="Screenshot 2026-05-11 at 08 29 13" src="https://github.com/user-attachments/assets/507cfaa3-465a-47fd-b921-a5387f30c24d" />

<img width="541" height="293" alt="Screenshot 2026-05-11 at 08 29 19" src="https://github.com/user-attachments/assets/3ca0b44f-ebc8-486b-b0ac-507ed6345018" />

<img width="532" height="293" alt="Screenshot 2026-05-11 at 08 30 14" src="https://github.com/user-attachments/assets/02a62e89-0e7c-41d6-a0ff-49dea7d4040e" />

<img width="532" height="191" alt="Screenshot 2026-05-11 at 08 30 30" src="https://github.com/user-attachments/assets/0d4ff786-a6cc-42a8-90e0-15a7379a21ce" />


# 6.
6.2
<img width="567" height="156" alt="Screenshot 2026-05-11 at 08 30 49" src="https://github.com/user-attachments/assets/17d2ce18-b169-4b95-8aaf-0a6b6babc8c7" />


6.3
<img width="567" height="78" alt="Screenshot 2026-05-11 at 08 31 26" src="https://github.com/user-attachments/assets/a2a6f476-e687-4518-aea3-f41bc3162f9c" />

6.4 
<img width="308" height="179" alt="Screenshot 2026-05-11 at 08 31 37" src="https://github.com/user-attachments/assets/73cf2a80-b60c-492b-a3db-78af1c184094" />

<img width="573" height="84" alt="Screenshot 2026-05-11 at 08 31 46" src="https://github.com/user-attachments/assets/fd478646-3660-46ec-9044-cebb9c92c515" />


