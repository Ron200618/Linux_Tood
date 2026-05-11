# n8n-Osa-2---Kaugjuhitav-Automaatne-Haldus

See ülesanne viib 1. osas õpitud oskused uuele tasemele. Nüüd õpime, kuidas n8n-i kasutada tsentraalse halduskonsoolina, mis suudab turvaliselt käivitada skripte ja haldusoperatsioone teistes serverites, kasutades Secure Shell (SSH) protokolli.

Eesmärk: Luua n8n-i töövoog, mis kontrollib ja arhiveerib kriitilisi logifaile kaugserveris.

🎯 1. Ettevalmistus: VirtualBoxi Võrgukonfiguratsioon
Reaalses elus asuvad serverid tavaliselt andmekeskuse kohtvõrgus. VirtualBoxi keskkonnas peame selle simulatsiooni looma.

Me vajame kahte Linuxi virtuaalmasinat (VM):

VM 1 (n8n Host): Jookseb n8n. See on meie Haldusserver.
VM 2 (Sihtserver): Tavaline Linuxi paigaldus (nt Ubuntu 22.04). See on meie Haldatav Server.
1.1. VM 1 (n8n Host) Võrguseaded
n8n host peab olema kättesaadav sinu füüsilisest arvutist (host-masinast) ja suutma rääkida sihtserveriga.

Adapter	Tüüp	Eesmärk
Adapter 1	NAT (Network Address Translation) koos Pordi Edastusega (Port Forwarding)	Välispääs: Annab n8n-ile Interneti-ühenduse (apt update/install) ja teeb n8n-i veebiliidese (port 5678) kättesaadavaks sinu host-brauserist.
Adapter 2	Host-Only Adapter	Sisemine privaatvõrk: Loob privaatse võrgu, milles n8n host ja sihtserver saavad rääkida stabiilsete privaat-IP-aadresside kaudu (nt 192.168.56.x).
Pordi Edastuse Seadistamine (NAT):

Mine VM 1 seadetesse $\rightarrow$ Network $\rightarrow$ Adapter 1 $\rightarrow$ Advanced $\rightarrow$ Port Forwarding.
Lisa reegel:

Nimi: n8n_WEB
Protocol: TCP
Host Port: 5678 (Sinu Host-arvuti port)
Guest Port: 5678 (VM 1 port, kus n8n jookseb)
Nüüd on n8n kättesaadav brauseris aadressil: http://localhost:5678 (või http://127.0.0.1:5678).
1.2. VM 2 (Sihtserver) Võrguseaded
Sihtserver peab saama haldusülesandeid n8n hostilt.

Adapter	Tüüp	Eesmärk
Adapter 1	NAT	Annab sihtserverile Interneti-ühenduse (nt värskenduste või pakettide installimiseks).
Adapter 2	Host-Only Adapter	Sisemine privaatvõrk: Suhtlemiseks n8n hostiga privaatse, stabiilse IP kaudu (nt 192.168.56.y). See on kriitiline SSH ühenduse jaoks.
🔒 2. Turvalisus: SSH Võtmete Seadistamine
n8n suhtleb teise serveriga turvaliselt, kasutades privaat-avaliku SSH võtmepaari. See on tööstusstandard, mis on turvalisem kui parooli edastamine.

2.1. Loo Võtmed (n8n Hostis)
Logi sisse VM 1 (n8n Host).
Loo spetsiaalne RSA võtmepaar (ära kasuta parooli, et n8n saaks automaatselt ühenduda):

Bash

ssh-keygen -t rsa -b 4096 -f ~/.ssh/n8n_auth_key -N ""
Tekib kaks faili: ~/.ssh/n8n_auth_key (Privaatvõti) ja ~/.ssh/n8n_auth_key.pub (Avalik võti).
2.2. Kopeeri Avalik Võti (Sihtserverisse)
Logi sisse VM 2 (Sihtserver).
Veendu, et SSH on lubatud (sudo systemctl status ssh).
Kopeeri avalik võti n8n hostist sihtserveri kasutaja faili ~/.ssh/authorized_keys:

Bash

# Käivita n8n hostis (VM 1)
# Asenda 'user@192.168.56.y' oma sihtserveri kasutaja ja Host-Only IP-ga
ssh-copy-id -i ~/.ssh/n8n_auth_key.pub user@192.168.56.y
Kontroll: Proovi nüüd parooli küsimata SSH ühendust: ssh -i ~/.ssh/n8n_auth_key user@192.168.56.y
2.3. Loo SSH Credentials n8n-is
Ava n8n veebiliides (http://localhost:5678).
Mine Settings $\rightarrow$ Credentials $\rightarrow$ New Credential.
Otsi SSH Key Credential.
Nimi: Sihtserver_SSH_Key
Auth Method: Key File (Absolute Path)
Key File Path: Sisesta absoluutne tee oma privaatvõtmele n8n hostis (nt /home/kasutajanimi/.ssh/n8n_auth_key).
Salvesta.
🛠️ 3. Töövoog: Logide Kontroll ja Arhiveerimine
Töövoo nimi: Remote_Log_Archiver
Samm 1: Trigger (Ajastamine)
Node: Schedule Trigger
Seadistus: Every Hour või Every Day.
Samm 2: Kaugkäsu Käivitamine ja Andmete Väljastamine
See node teostab SSH kaudu kaugserveris mitu haldusoperatsiooni korraga.

Node: SSH
Resource: Execute Command
Credentials: Vali Sihtserver_SSH_Key.
Host: Sisesta VM 2 Host-Only IP-aadress (nt 192.168.56.101).
User: Sisesta sihtserveri kasutajanimi (nt user).
Command: See skript otsib logist vigu, arhiveerib need ja väljastab arhiivi tee.

Bash

# 1. Määra unikaalne arhiivi nimi
ARCHIVE_NAME="critical_$(date +%Y%m%d_%H%M%S).txt.gz"

# 2. Leia kriitilised logid ja salvesta need ajutisse faili
# Otsime '/var/log/syslog' failist 'error', 'fail' või 'denied'
grep -E 'error|fail|denied' /var/log/syslog > /tmp/${ARCHIVE_NAME%.gz}

# 3. Zipi fail (gzip eemaldab originaali)
gzip /tmp/${ARCHIVE_NAME%.gz}

# 4. Väljasta tulemus n8n-ile, et teada saada arhiivi täielik tee
echo "/tmp/$ARCHIVE_NAME"
Samm 3: Faili Allalaadimine (Auditeerimine)
Allalaadimiseks kasutame taas SSH node'i, aga teise operatsiooniga.

Node: SSH
Operation: Download File
Host/User/Credentials: Samad, mis eelnevalt.
File Path (Remote): See peab olema dünaamiline, kasutades eelmise node'i väljundit.

Kliki väljal $\rightarrow$ Add Expression
Expression: {{ $node["SSH"].json["stdout"] }}

Selgitus: See võtab eelmise SSH node'i poolt echo abil väljastatud stringi (/tmp/critical_...txt.gz) ja kasutab seda failiteena.
Destination Path (Local): Tee n8n hostis, kuhu arhiiv salvestatakse (nt /home/user/n8n_audits/).
Samm 4: Puhastamine (Koristustöö)
Halduses on oluline ajutised failid kustutada.

Node: SSH
Operation: Execute Command
Command: Kasutame sama dünaamilist väljundit rm käsus.

Bash

rm {{ $node["SSH"].json["stdout"] }}
Samm 5: Teavitus
Node: Discord või Email
Content: "Kaugserveri logide arhiveerimine õnnestus. Fail salvestati: /home/user/n8n_audits/"
🌐 4. Päriselu Kasu ja Haldusnode'ide Ülevaade
Mida see meile annab? (Real-World Use)
Tsentraliseeritud Haldus: Väikestes ja keskmistes ettevõtetes on servereid 5–50. n8n võimaldab ühest kohast hallata kõiki kriitilisi hooldus- ja auditeerimisoperatsioone (logirotatsioon, ajutiste failide puhastamine, turvaaukude kontroll).
Audit Jälje loomine: Iga n8n-i töövoo käivitamine jätab täpse logi (Execution Log), kes, mida ja millal tegi, mis on kriitiline nõue IKT-s (nt ISO 27001).
Vead Ennetatud: Automatiseerides rutiinsed kontrollid (kettaruumi, teenuste staatus), saad teavituse enne, kui probleem kasutajateni jõuab.
Lisaks SSH-le (Muud Haldusnode'id)
Peale SSH on n8n-is veel palju halduseks kasulikke node'e, mille pead tegema:


Eelmises osas loodi töövoog kriitiliste logide arhiveerimiseks. Nüüd lisame:

Tõrke teavitused: Kuidas teavitada probleemi ilmnemisel (nt SSH käsu ebaõnnestumine)?
Täiendav audit: Kettaruumi kasutuse kontrollimine.
⚠️ 5. Tõrgete Käsitlemine (Error Handling)
Igas professionaalses töövoos on hädavajalik määratleda, mis juhtub, kui teatud samm ebaõnnestub (nt sihtserver on maas või SSH käsk annab vea). n8n lahendab selle Error Edge'ide abil.

5.1. Lisame Tõrketeavituse (Discord)
Lisa uus node: Lisa töövoo lõppu (või eraldi haruna) Discord node ja seadista see (vajad uut Webhook URL credentiali).
Ühenda Error Edge:

Klõpsa hiire parema nupuga Samm 2: Kaugkäsu Käivitamine (või mõnel muul kriitilisel SSH node'il).
Vali Connect Error.
Lohistades ühenda see Discord node'iga.
Nüüd, kui Samm 2 ebaõnnestub, läheb töövoog otse Discordi harule, mitte ei proovi edutult Samm 3 ja 4.

5.2. Tõrketeate Sisu
Seadista Discord node'i sisu, et see annaks spetsiifilist infot. Kasuta n8n-i väljendeid (Expressions) sisseehitatud tõrkeandmete saamiseks:

Väli	Väljend	Eesmärk
Content	**VIGA!** Kaugserveri töövoog ebaõnnestus!	Pealkiri.
Additional Field $\rightarrow$ Text	Töövoog: {{ $workflow.name }}	Annab teada, milline töövoog ebaõnnestus.
Additional Field $\rightarrow$ Text	Vea sõnum: {{ $error.message }}	Annab teate SSH node'ist (nt connect timeout või command failed).
Additional Field $\rightarrow$ Text	Ebaõnnestunud Node: {{ $error.node.name }}	Anname teada täpse node nime.
💾 6. Täiendav Haldus: Kettaruumi Kontroll
Eelmine töövoog oli auditeerimiseks (logide arhiveerimine). See on ennetav hooldus.

6.1. Lisa Uus Haru Töövoogu
Kasutame Branching (Split In Batches) node'i, et saata eraldi teavitused, kui kettaruum on kriitiline.

Samm 2.1: Kettaruumi Kontroll

Node: SSH
Operation: Execute Command
Command: df -h / | tail -n 1 | awk '{print $5}'

Selgitus: See skript tagastab ainult juurpartitsiooni kettaruumi kasutuse protsendi (nt 55%).
Samm 2.2: Andmete Puhastamine (Puhastus-Node)

Node: Set (See node aitab meil hilisemat loogikat lihtsustada).
Operation: Set
Value: {{ $node["SSH"].json["stdout"].replace("%", "").trim() | int }}

Selgitus: See teisendab eelmise node'i väljundi (55%) puhtaks täisarvuks (55), mida me saame võrrelda.
Samm 2.3: Kriteeriumi Kontroll

Node: IF (See node on loogika süda)
Condition:

Value 1: {{ $node["Set"].json["value"] }} (Meie puhastatud kettaruumi % number)
Operation: Is greater or equal
Value 2: 85 (Meie kriitiline piir, nt 85%)
6.2. Teavituse Haru
Jah (True) Haru:

Ühenda IF node'i True väljund Discord node'iga (või tee uus).
Sisu: **TÄHELEPANU!** Sihtserveri kettaruum on kriitiline: {{ $node["Set"].json["value"] }}% täis! (Siia võiks lisada isegi käskluse, et automaatselt ajutised failid kustutada).
Ei (False) Haru:

Ühenda IF node'i False väljund NoOp node'iga (teeb mitte midagi) või jätka töövoogu muude rutiinsete kontrollidega (nt teenuste staatus).
💡 Kokkuvõte ja Järgmised Sammud
Part 1 andis sulle võime tsentraliseeritult käivitada mis tahes skripti (logi arhiveerimine).

Part 2 õpetas sulle professionaalse käsitluse (veahaldus) ja andis tööriistad ennetavaks halduseks (kettaruumi kontroll).

Kokkuvõte Haldus-Node'idest:

Node	Põhikasutus IKT Halduses
SSH	Serverite konfiguratsioon, auditeerimine, patchimine, failiülekanded.
HTTP Request	Suhtlemine API-dega (nt pilveteenused, monitoring lahendused).

# lahendus

<img width="556" height="171" alt="Screenshot 2026-05-11 at 09 39 48" src="https://github.com/user-attachments/assets/4fa55024-d711-458f-b369-145a91d47b50" />


Ping	Lihtne teenuse kättesaadavuse kontrollimine.
IF / Switch	Loogiline otsustusprotsess (nt Kui ketas on > 80%, siis teavita).
Schedule Trigger	Rutiinsete haldusülesannete automatiseerimine.
