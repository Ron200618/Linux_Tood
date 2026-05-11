# WireShark-Kali-Linuxis

Wireshark on maailma populaarseim võrguliikluse analüüsiprogramm, mis toimib nagu digitaalne mikroskoop, võimaldades näha detailselt kõike, mis arvutivõrgus toimub. See tarkvara püüab kinni reaalajas võrgukaarti läbivad andmepaketid ja muudab binaarse arvutikeele inimestele loetavaks tekstiks. IT-spetsialistid kasutavad seda peamiselt võrguprobleemide diagnoosimiseks, et mõista, miks ühendused on aeglased või miks teatud teenused ei tööta. Küberturvalisuses on see hädavajalik tööriist, millega kontrollitakse, kas rakendused saadavad andmeid turvaliselt või lekivad paroolid ja sõnumid krüpteerimata kujul. Kokkuvõttes võimaldab Wireshark sügavuti analüüsida ja filtreerida kogu võrguliiklust, olles asendamatu abimees nii võrguadministraatoritele kui ka turvatestijatele.

Praktiline Lab: "Paroolide jahtimine Wiresharkiga"
Eesmärk: Mõista, miks HTTP protokoll on ebaturvaline ja kuidas näeb välja sisselogimisinfo võrgupaketi sees. Tööriistad: Kali Linux (või muu OS), Wireshark, Veebibrauser.

Samm 1: Nuhkvara käivitamine
Ava Wireshark.
Vali avalehel õige võrguliides (Network Interface).

Kui oled kaabliga: tavaliselt eth0.
Kui oled Wi-Fi-s: tavaliselt wlan0.
Vihje: Vali see, mille taga liigub väike graafik.
Vajuta sinist uime ikooni (Start capturing packets) vasakul üleval nurgas.

Nüüd näed ekraanil jooksmas sadu ridu – see on kogu sinu arvuti suhtlus internetiga.
Samm 2: "Ohvri" mängimine
Jätame Wiresharki taustale tööle ja tekitame liiklust, mida kinni püüda.

Ava veebibrauser (nt Firefox).
Mine aadressile: http://testphp.vulnweb.com/login.php

NB! Jälgi, et aadress algaks http://, mitte https://.
Sisesta sisselogimisvormi järgmised andmed:

Username: häkker
Password: MinuSalajaneParool123 (või midagi muud meeldejäävat).
Vajuta Login.

(Pole oluline, kui leht ütleb "error" – andmed on juba saadetud).
Samm 3: Andmete väljasõelumine
Nüüd on "võrk kinni püütud". Leiame üles selle ühe paketi, kus on parool.

Mine tagasi Wiresharki ja vajuta punast ruutu (Stop capturing), et lõpetada salvestamine.
Kirjuta üles filtriribale (Apply a display filter) järgmine käsk:

http.request.method == POST
Vajuta Enter.

Selgitus: POST meetodit kasutatakse veebis andmete saatmiseks serverile (nt sisselogimisel).
Sulle peaks jääma nähtavale ainult 1–2 rida.
Tee paremklikk sellel real (tavaliselt Protocol: HTTP).
Vali menüüst: Follow -> TCP Stream.
Samm 4: Tulemuse analüüs
Sulle avaneb uus aken, mis näitab kogu vestlust sinu ja serveri vahel.

Vaata akna alumisse ossa (tavaliselt punane tekst).
Otsi rida, mis näeb välja selline: uname=häkker&pass=MinuSalajaneParool123
Palju õnne! Oled edukalt pealt kuulanud sisselogimist.
OSA 2:
Suurepärane! Kui esimene osa oli "häkkimine" (parooli leidmine), siis teine osa on "kriminalistika" (forensics).

See harjutus on õpilastele sageli veelgi suurem üllatus, sest nad ei taju, et võrgukaablist ei liigu läbi mitte ainult tekst, vaid terved failid (pildid, dokumendid, viirused), mida saab hiljem "õhust" kokku panna.

Siin on tööleht teiseks harjutuseks.

Praktiline Lab 2: Digitaalne kriminalistika – piltide taastamine
Eesmärk: Õppida kasutama Wiresharki keerukamaid funktsioone failide (pildid, dokumendid) eraldamiseks võrguliiklusest. Stsenaarium: "Kahtlusalune on külastanud veebilehte, kuid kustutas oma brauseri ajaloo. Meil on aga salvestus tema võrguliiklusest. Sinu ülesanne on taastada pildid, mida ta vaatas, et tõestada tema tegevust."

Töövahendid: Kali Linux (või muu OS), Wireshark, veebibrauser.

1. Samm: Liikluse salvestamine
Käivita Wireshark.
Vali õige võrguliides ja vajuta sinist uime-ikooni (Start capturing).
Jäta Wireshark taustale tööle.
2. Samm: "Kahtlase materjali" vaatamine
Genereerime liiklust, mis sisaldab pildifaile.

Ava veebibrauser.
Mine samale testlehele: http://testphp.vulnweb.com/

(Võid valida ka menüüst "Browse categories" ja klõpsata erinevatel plakatitel/piltidel).
Veendu, et leht laeks pildid ilusti ära.

Märkus: Oluline on taas, et leht oleks HTTP, mitte HTTPS. HTTPS-i puhul oleksid pildid krüpteeritud.
3. Samm: Maagia ehk failide eraldamine
Nüüd tuleb kõige põnevam osa – me ei otsi enam ridu tekstist, vaid laseme Wiresharkil pusle kokku panna.

Mine Wiresharki ja peata salvestamine (punane ruut).
Vali ülemisest menüüst: File -> Export Objects -> HTTP...
Avaneb aken "Wireshark: Export HTTP object list".

See nimekiri sisaldab kõiki faile, mis sinu sessiooni jooksul võrgust läbi käisid (HTML failid, CSS stiilid, JPG/PNG pildid).
4. Samm: Asitõendite salvestamine
Otsi nimekirjast faile, mille Content Type on image/jpeg või image/png.

Tavaliselt on need failinimedega nagu logo.png, poster.jpg vms.
Klõpsa ühel pildifailil ja vajuta nuppu Save (või Save All, kui tahad kõiki korraga).
Salvesta fail oma kausta (nt Desktopile).
5. Samm: Tõestus
Mine oma arvuti kausta, kuhu faili salvestasid.
Ava pildifail.
Tulemus: Sa näed täpselt sama pilti, mida kasutaja veebilehel vaatas.

<img width="601" height="452" alt="Screenshot 2026-05-11 at 08 48 59" src="https://github.com/user-attachments/assets/0ac6f5ac-f82b-42b9-b180-50ef96d1c53a" />

<img width="601" height="288" alt="Screenshot 2026-05-11 at 08 49 08" src="https://github.com/user-attachments/assets/9cfb1a47-5d25-4d25-a149-506ef57fa839" />

<img width="549" height="315" alt="Screenshot 2026-05-11 at 08 49 17" src="https://github.com/user-attachments/assets/82a23bff-9701-424a-bb5a-2eb95ab5aa5d" />

<img width="549" height="442" alt="Screenshot 2026-05-11 at 08 49 27" src="https://github.com/user-attachments/assets/f510c821-92bf-4b92-b814-a72195b3e263" />

<img width="549" height="107" alt="Screenshot 2026-05-11 at 08 49 38" src="https://github.com/user-attachments/assets/5e2ce862-405a-4bcb-b976-635970643684" />



# OSA 2 

<img width="601" height="446" alt="Screenshot 2026-05-11 at 08 51 49" src="https://github.com/user-attachments/assets/a24ab7a9-bd70-4f33-a5da-e3c8eca0484f" />

<img width="601" height="446" alt="Screenshot 2026-05-11 at 08 51 55" src="https://github.com/user-attachments/assets/4db86956-67b4-4311-9302-87fbd7611e88" />

<img width="601" height="446" alt="Screenshot 2026-05-11 at 08 52 00" src="https://github.com/user-attachments/assets/8a4ef7b1-07aa-466e-9d52-241909e0f031" />

<img width="462" height="418" alt="Screenshot 2026-05-11 at 08 52 13" src="https://github.com/user-attachments/assets/80b5e084-e79d-48a5-9173-e92aa8f2d2b5" />

