# Linux_Tööd – Linuxi ja süsteemihalduse portfoolio

![OS](https://img.shields.io/badge/OS-Linux-blue)
![Fookus](https://img.shields.io/badge/Fookus-Serverihaldus%20%7C%20Automatiseerimine-green)
![Staatus](https://img.shields.io/badge/Staatus-Aktiivne-success)

See repositoorium koondab minu praktilised Linuxi, serverihalduse, võrgu, automatiseerimise ja pilvelahenduste ülesanded. Eesmärk on näidata tööandjale reaalseid oskusi: teenuste paigaldus, turvaline haldus, skriptimine, dokumenteerimine ja probleemilahendus.

---

## Sisukord
- [Minust](#minust)
- [Oskused ja tööriistad](#oskused-ja-tööriistad)
- [Esiletõstetud projektid](#esiletõstetud-projektid)
- [Projektid](#projektid)
- [Harjutused ja laborid](#harjutused-ja-laborid)
- [Repositooriumi struktuur](#repositooriumi-struktuur)
- [Kuvatõmmised](#kuvatõmmised)
- [Käivitamine / kasutamine](#käivitamine--kasutamine)
- [Kontakt](#kontakt)

## Minust
Olen IT-õppija, kes keskendub Linuxi süsteemihaldusele ja DevOps-laadsele töövoole. Selles portfoolios on samm-sammult dokumenteeritud ülesanded, kus lahendan päris serveri- ja võrguolukordi ning automatiseerin korduvaid tegevusi.

## Oskused ja tööriistad
- **Operatsioonisüsteemid:** Ubuntu, Kali Linux, TrueNAS
- **Serverid ja teenused:** Apache2, Samba, SSH, Nextcloud, Minecraft server
- **Automatiseerimine:** Bash, n8n, cron, logimine ja varundus
- **Võrk ja turvalisus:** port forwarding, tulemüür, Wireshark, kasutajaõigused
- **Virtualiseerimine:** Oracle VirtualBox, VM-i seadistamine ja taastamine
- **Versioonihaldus:** Git ja GitHub

## Esiletõstetud projektid
- [Väikeettevõtte Linux server](./V-ikeettev-tte-Linux-server.md)  
  Täislahenduse kirjeldus väikeettevõtte vajadustele: teenused, kasutajad, ligipääsud ja haldusprotsessid. Projekt näitab süsteemi planeerimise ja töökindla juurutuse oskust.

- [Linux webserver automation](./linux-webserver-automation.md)  
  Automatiseeritud veebiserveri seadistuse ülesanne, kus käsitsi tehtavad sammud viiakse skriptidesse. Tulemus vähendab vigade arvu ja kiirendab korduvat juurutust.

- [Haldustegevuste automatiseerimine n8n-is](./Haldustegevused-automatiseerimine-n8nis.md)  
  n8n töövood serveri monitooringuks ja haldustegevuste automatiseerimiseks. Fookus on sündmuspõhisel haldusel ning admini ajakulu vähendamisel.

- [Rakenduse live-viimine kahe serveriga (load balancing)](./rakenduse-live-viimine-2-serveriga-load-balancing.md)  
  Praktika kahe serveri vahel koormuse jaotamiseks ja rakenduse töökindluse tõstmiseks. Näitab teadmisi skaleerimise ja kõrge käideldavuse alustes.

## Projektid
### Pilv, salvestus ja teenused
- [TrueNAS operatsioonisüsteem](./Truenas-Operatsioonis-susteem)  
  Juhend TrueNAS-i paigaldamiseks VM-is, salvestuse seadistamiseks ja Nextcloudi teenuse käivitamiseks. Tulemusena valmib isemajutatav failiplatvorm koos haldusliidesega.

- [NextCloud ülesanded](./NextCloudi-ulesanded.md)  
  Nextcloudi juurutus, kasutajate loomine ja põhiline failihalduse töövoog. Harjutus kinnistab privaatpilve praktilise kasutuse aluseid.

- [NextCloud ülesanded (osa 2)](./NextCloudi-ulesanded-p2)  
  Edasijõudnud Nextcloudi haldus: CLI-käsud, kasutajahaldus, hooldusskript ja ligipääsu testimine. Projekt näitab, kuidas teenust hallata nii GUI kui käsurea kaudu.

### Veebi- ja failiteenuste automatiseerimine
- [Bash + PHP (osa 1)](./bash-php.md)  
  Apache2 veebilehe ja Bash-skriptide sidumine, et veebitoimingud käivitaksid taustal käske. Aitab mõista lihtsa web-to-shell automatiseerimise loogikat.

- [Bash + PHP (osa 2)](./bash-php-%20part%202.md)  
  Esimese osa edasiarendus, kus veebist saadud andmeid töödeldakse ja salvestatakse süsteemselt. Harjutus parandab andmevoo ja skriptimise oskust.

- [Bash + Apache2 + Samba](./bash-apache2-samba.md)  
  Failiüleslaadimise veebilahendus, kus failid suunatakse Samba failiserverisse. Tulemus ühendab veebiteenuse ja sisevõrgu failijagamise üheks töövooguks.

- [Parooliga faili üleslaadimine](./parooliga-file-upload.md)  
  Turvalisema failiüleslaadimise ülesanne, kus ligipääs on kaitstud. Fookus on lihtsal autentimisel ja teenuse turvalisemal kasutusel.

### Minecraft serveri lahendused
- [Initial Minecraft server setup](./Initial-minecraft-server-setup.md)  
  Minecraft serveri baasseadistus Ubuntu keskkonnas. Annab vundamendi teenuse käivitamiseks ja edasiseks automatiseerimiseks.

- [Minecraft server – arhitektuur ja backupi plaan](./Minecraft-server-1)  
  Serveri failipuu, varunduse, logimise ja RAID-põhise salvestuse planeerimine. Projekt näitab infrastruktuuri mõtlemist ning töökindluse disaini.

- [Minecraft serveri töö esitamine](./minecrafti-serveri-t-esitamine.md)  
  Lõppdokumentatsiooni koostamine koos tõendusmaterjaliga tehtud seadistustest. Arendab oskust tehnilist lahendust selgelt esitleda.

- [Viimane projekt](./Viimane-Projekt.md)  
  Projekti koondfail, kuhu on märgitud lõpptöö staatus. Sobib projekti edenemise lühikokkuvõtteks.

## Harjutused ja laborid
### Linux, süsteemihaldus ja kasutajaõigused
- [Failisüsteemid](./failis-susteemid.md)  
  Praktika Linuxi failisüsteemi, ketaste ja õiguste teemadel. Aitab kinnistada süsteemiadministraatori baasoskusi.

- [Failide varundamine ja logid](./Failide-varundamine-ja-logid.md)  
  Varunduse ja logihalduse ülesanded, kus rõhk on taastatavusel ja jälgitavusel. Tulemus aitab ennetada andmekadu ning kiirendada veaotsingut.

- [Kordav ülesanne: kasutajad ja õigused](./KORDAV-LESANNE-KASUTAJAD-JA-IGUSED.md)  
  Kasutajate/gruppide õiguste haldus praktilises stsenaariumis. Näitab, kuidas ligipääse turvaliselt piirata ja avada.

- [Linux tools – iseseisev õppimine](./Linux-tools-iseseisev-ppimine.md)  
  Iseseisev probleemilahendus Linuxi tööriistadega. Harjutus arendab dokumentatsiooni lugemise ja kiire katsetamise oskust.

- [Linuxi käsurea meistriklass](./Linuxi%20K%C3%A4surea%20Meistriklassi.md)  
  Käsurea ülesannete kogum andmete töötlemiseks ja halduseks. Aitab kasvatada töökiirust terminalipõhistes töövoogudes.

- [Visuaalne serverihaldus](./Visuaalne-serverihaldus.md)  
  Serveri haldus veebiliidese abil (sh LVM, võrk ja teenused). Sobib hästi CLI-teadmiste täiendamiseks visuaalse halduse poolel.

- [Broken VM](./Broken-VM.md)  
  Katkise virtuaalmasina taastamise ülesanne. Näitab veaotsingu, parandamise ja süsteemi töökorras kinnitamise oskust.

### SSH, Git ja kaugühendus
- [SSH ühenduse tegemine Windowsi ja Ubuntu VM-iga](./SSH-uhenduse-tegemine-windowsi-ja-ubuntu-VM-iga.md)  
  Kaugühenduse loomine eri platvormide vahel. Eesmärk on turvaline terminaliühendus ja põhiline kaugadministreerimine.

- [SSH ühendus võtmega arenduskeskkonda](./SSH-hendus-v-tmega-arenduskeskond.md)  
  Võtmepõhine autentimine ja turvalisem SSH kasutus. Harjutus vähendab paroolipõhise ligipääsu riske.

- [Online ülesanne SSH-ga](./Online-lesanne-SSH-ga.md)  
  Bandit-tüüpi SSH väljakutse, mis treenib käsurida ja turvalisuse mõtlemist. Tugevdab probleemilahendust järjestikuste ülesannete kaudu.

- [GitHub + Apache2 versioonihaldus](./GitHub-Apache2-versioonihaldus.md)  
  Praktika, kus veebiserveri tööd seotakse GitHubi versioonihaldusega. Näitab töövoogu muudatuste jälgimiseks ja avaldamiseks.

### Võrk ja turvalisus
- [Kali Linux](./Kali-linux.md)  
  Kali Linuxi ja Ubuntu VM-i keskkonna ettevalmistus turvalisuse laboriteks. Loob baasi võrgu- ja pentest tööriistade kasutamiseks.

- [Wireshark Kali Linuxis](./WireShark-Kali-Linuxis.md)  
  Võrguliikluse analüüsi harjutus Wiresharkiga. Aitab mõista pakettide liikumist ning leida võrguühenduse probleeme.

- [n8n osa 2 – kaugjuhitav automaatne haldus](./n8n-Osa-2---Kaugjuhitav-Automaatne-Haldus.md)  
  n8n töövoogude edasiarendus turvaliseks kaugjuhitavaks halduseks. Fookus on skaleeritaval ja taaskasutataval administreerimisel.

## Repositooriumi struktuur
```text
Linux_Tood/
├── README.md
├── *.md                         # Ülesanded, projektid ja dokumentatsioon
├── Minecraft-server-1           # Minecrafti arhitektuuri/backupi dokument
├── NextCloudi-ulesanded-p2      # Nextcloudi osa 2 dokument
└── Truenas-Operatsioonis-susteem# TrueNAS + Nextcloudi praktiline töö
```

## Kuvatõmmised
Repositooriumis ei ole eraldi pildifaile (`.png/.jpg`) kaustadena versioonihalduses; kuvatõmmised on lisatud ülesannete sisse. Soovituslikud näited:
- [TrueNAS + Nextcloud seadistus (palju kuvatõmmiseid)](./Truenas-Operatsioonis-susteem)
- [Nextcloud osa 2 kuvatõmmised](./NextCloudi-ulesanded-p2)
- [n8n ja serverihalduse vood](./n8n-Osa-2---Kaugjuhitav-Automaatne-Haldus.md)

## Käivitamine / kasutamine
Kuna tegu on dokumentatsioonipõhise portfoolioga, saab materjale kasutada nii:
1. Ava huvipakkuv fail otse GitHubis (lingid üleval).
2. Järgi juhiseid oma VM-is või Linuxi serveris samm-sammult.
3. Vajadusel kohanda käske oma keskkonna IP-de, kasutajate ja kaustateedega.

## Kontakt
- **GitHub:** [@Ron200618](https://github.com/Ron200618)
- **Repositoorium:** [Ron200618/Linux_Tood](https://github.com/Ron200618/Linux_Tood)

---
Kui soovid, võin järgmise sammuna teha ka ingliskeelse CV-versiooni samast README-st (EST + ENG), et seda oleks mugavam tööavaldustes kasutada.
