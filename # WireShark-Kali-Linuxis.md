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

<img width="635" height="713" alt="image" src="https://github.com/user-attachments/assets/42ca8e74-3f44-44b0-94eb-5f3c9310c4f8" />

<img width="593" height="306" alt="image" src="https://github.com/user-attachments/assets/7ca3953c-619c-44c9-8bea-f0b17145cda3" />

<img width="641" height="522" alt="image" src="https://github.com/user-attachments/assets/04fd24ef-4203-4650-a096-02f2f961331d" />

# OSA 2 

<img width="620" height="441" alt="image" src="https://github.com/user-attachments/assets/bc3d5178-fe16-485e-9eea-49356289b4b9" />

<img width="612" height="434" alt="image" src="https://github.com/user-attachments/assets/c14a6122-44b5-4f82-a9fd-fe1314be918e" />

<img width="591" height="461" alt="image" src="https://github.com/user-attachments/assets/21046b4a-11b0-447d-b2d4-49dc20dd4b55" />

<img width="497" height="399" alt="image" src="https://github.com/user-attachments/assets/895de028-a9e7-4c60-bb56-2a476edcaa42" />
