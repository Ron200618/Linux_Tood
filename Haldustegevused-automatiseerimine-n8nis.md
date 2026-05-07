# Haldustegevused-automatiseerimine-n8nis

eema: n8n paigaldus "Bare Metal" ja serveri monitooring

Mis on n8n?
n8n (Nodemation) on avatud lähtekoodiga, ise-majutatav (self-hosted) töövoogude automatiseerimise platvorm.

Arhitektuur: Sõlmedepõhine (node-based). Iga samm (nt "Loe fail", "Saada email", "Käivita käsk") on visuaalne kast, mis on ühendatud joontega.
Tööpõhimõte: See on "liim" erinevate teenuste ja süsteemide vahel. See võtab sisendi (trigger), töötleb andmeid ja saadab need edasi, kasutades API-sid või kohalikke süsteemikäske.
Erinevus teistest: Erinevalt Zapierist või Make-ist on n8n mõeldud jooksutamiseks oma serveris (Docker/Node.js), andes täieliku kontrolli andmete ja infrastruktuuri üle.
Kuidas n8n täiendab masinate haldust
1. Asendab ja täiendab Cron-i

Tavaline skript: Pead seadistama crontab -e, logisid on raske jälgida, ajastuse muutmine on tülikas.
n8n: Pakub visuaalset ajastajat (Schedule Trigger). Näed kohe, millal töövoog viimati jooksis, kas see õnnestus või ebaõnnestus.
2. Lihtsustab API-dega suhtlemist

Tavaline skript: curl päringute kirjutamine, päiste (headers) ja autentimise (OAuth2) käsitisi haldamine Bashis on vaevarikas ja veaohtlik.
n8n: Posedib valmis mooduleid (nodes) sadadele teenustele (Slack, Google Sheets, Jira, SQL). Autentimine on GUI-põhine ja turvaline.
3. Visualiseerib loogikat ja veahaldust

Tavaline skript: Keerukad if/else blokid ja try/catch loogika koodis.
n8n: Visuaalsed harud (Switch/If nodes). Vea korral saab töövoo suunata automaatselt "Error Handler" harusse, mis saadab adminile teavituse.
4. Andmete töötlemine (JSON parsing)

Tavaline skript: JSON-i parsimine jq või grep/awk abil on algajale keeruline.
n8n: Kõik andmed liiguvad sõlmede vahel JSON objektidena. Andmete filtreerimine ja muutmine on visuaalne või lihtsa JavaScriptiga tehtav.
5. Hübriidlahendused

Sina kirjutad Bashi skripti, mis teeb midagi süsteemispetsiifilist (nt loob uue Linuxi kasutaja ja kodukausta).
n8n võtab veebivormist sisendi -> käivitab sinu skripti (Execute Command) -> analüüsib väljundit -> saadab uuele kasutajale parooli e-mailiga.

1. Ettevalmistus ja paigaldus (Ubuntu CLI)
n8n vajab töötamiseks Node.js keskkonda. Kasutame versiooni 20 (LTS). Lisaks kasutame PM2 (Process Manager 2), et hoida n8n taustal töös ja käivitada see automaatselt pärast serveri restarti.

Samm 1: Node.js repositooriumi lisamine ja install Käivita terminalis:

Bash

# Uuenda pakettide nimekirja ja installi curl
sudo apt update && sudo apt install -y curl build-essential

curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

# Installi Node.js
sudo apt install -y nodejs

# Kontrolli versioone
# Lisa NodeSource rep (Node 20.x)
node -v
npm -v
Samm 2: n8n paigaldamine Installime n8n globaalselt npm kaudu:

Bash

sudo npm install n8n -g
Samm 3: PM2 paigaldamine ja n8n käivitamine teenusena Me ei taha n8n-i käivitada lihtsalt käsuga n8n start, sest see sulgub, kui SSH ühendus katkeb.

Bash

# Installi PM2 globaalselt
sudo npm install pm2@latest -g

# Käivita n8n PM2 all
pm2 start n8n

# Salvesta praegune protsesside nimekiri, et see taastuks restardil
pm2 save

# Seadista startup skript (jälgi ekraanile tulevat käsku, see tuleb kopeerida ja käivitada)
pm2 startup
Samm 4: Ligipääs Avage brauseris: http://<VM_IP_ADDRESS>:5678 Looge admin kasutaja.

2. Ülesanne: "Linux Teenuste Automaatne Kontroll ja Raporteerimine"
Selles ülesandes kasutab n8n host-masina ressursse otse. Eesmärk on kontrollida kriitilise teenuse (nt SSH või Apache/Nginx) staatust ja kettamahtu.

Eesmärk: Luua automaatika, mis kontrollib, kas SSH teenus töötab ja kas kettal on ruumi. Vigade korral saadetakse teavitus ja kirjutatakse logi.

Töövoo (Workflow) loomise juhend
Samm 1: Trigger (Ajastamine)

Otsi node: Schedule Trigger.
Seadista: Trigger Interval -> Minutes -> 1 (Testimiseks).
Samm 2: Andmete kogumine (Bash skriptimine) Kuna n8n jookseb otse Ubuntus, on tal ligipääs süsteemi käskudele (kasutaja õigustes, kes n8n käivitas).

Lisa node: Execute Command.
Nimeta ümber: Check System Status.
Command kasti kirjuta liitkäsk (semikooloniga eraldatud), mis väljastab JSON laadse struktuuri või lihtsa teksti.

Me kontrollime kettakasutust ja SSH teenuse staatust.
Bash

DISK=$(df -h / | tail -1 | awk '{print $5}' | sed 's/%//');
SERVICE=$(systemctl is-active ssh);
echo "$DISK,$SERVICE"
Selgitus: Skript tagastab stringi formaadis 45,active (protsent, staatus).
Samm 3: Andmete töötlemine (Code/Set Node) Et andmeid oleks lihtsam if-lausetes kasutada, tükeldame väljundi.

Lisa node: Code (või vanemates versioonides "Function").
Keel: JavaScript.
Kood:

JavaScript

const output = items[0].json.stdout.trim().split(',');
return [
  {
    json: {
      disk_usage: parseInt(output[0]),
      ssh_status: output[1]
    }
  }
];
Samm 4: Loogika (Switch/If) Meil on kaks tingimust, mida kontrollida.

Lisa node: If.
Tingimus 1 (Ketas): disk_usage > 90.
Tingimus 2 (Teenus): ssh_status not equal "active".
Ühenda: Combine -> ANY (kui kasvõi üks on valesti, mine True harru).
Samm 5: Tegevused (True - Viga leitud)

Logimine faili:

Lisa node: Execute Command.
Command:
Bash

echo "$(date) - ALERT: Disk: {{ $json.disk_usage }}%, SSH: {{ $json.ssh_status }}" >> ~/n8n_system_monitor.log
B. Discordi teavituse saatmine:

Eeldus: Loo oma Discordi serveris Webhook (Server Settings -> Integrations -> Webhooks -> New Webhook) ja kopeeri Webhook URL.
Lisa node: Discord.
Seadistus:

Resource: Webhook
Webhook URL: (Kleebi siia oma Discordi URL)
Content:

Plaintext

⚠️ **[SERVER ALERT] Probleem VM-is!**
Kettakasutus: {{ $json.disk_usage }}%
SSH Staatus: {{ $json.ssh_status }}
Samm 6: Error Handling (Veahaldus) Kui skript (Samm 2) ebaõnnestub (nt systemctl käsk ei toimi õiguste tõttu), peab n8n sellest teada andma.

Ava Check System Status node seaded.
Vali: On Error -> Continue.
Lisa kohe pärast seda node: If.
Tingimus: kontrolli, kas exitCode on olemas ja ei võrdu 0-ga.
Ühenda see "Send Discord message" node-ga, mille sisu on "Monitooringu skript kukkus kokku (Exit Code != 0)".
3. Logide ja protsessi haldus (PM2 käsud õpilastele)
Kuna me ei kasuta Dockerit, peavad õpilased oskama n8n protsessi hallata käsurealt. See on osa õppetööst.

Vaata logisid (Real-time):

Bash

pm2 logs n8n
Restardi n8n (kui midagi kiilub kinni):

Bash

pm2 restart n8n
Vaata protsessi staatust (CPU/RAM kasutus):

Bash

pm2 monit

# samm 1


<img width="625" height="686" alt="image" src="https://github.com/user-attachments/assets/82ba9a12-8ee6-4ef8-9ffd-68f909235ec0" />

<img width="600" height="656" alt="image" src="https://github.com/user-attachments/assets/616bc76b-5e30-455a-8c8f-46463d88c84a" />

<img width="659" height="757" alt="image" src="https://github.com/user-attachments/assets/c76d3c57-9e64-4bac-bc84-b3d6619d3654" />

# samm 2

<img width="644" height="725" alt="image" src="https://github.com/user-attachments/assets/a0c08ad6-3934-4a96-9f79-447c9c851900" />

<img width="612" height="252" alt="image" src="https://github.com/user-attachments/assets/2e7d42b7-0242-4780-8640-299fe119270e" />

# samm 3

<img width="608" height="409" alt="image" src="https://github.com/user-attachments/assets/20eea22f-c1e1-4d21-81f5-d1c7e4c5113f" />

<img width="602" height="406" alt="image" src="https://github.com/user-attachments/assets/d2acd00a-d5de-48fa-a93d-6540936aaeca" />
