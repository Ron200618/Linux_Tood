Gecos on mõeldud selleks, et muuta kasutaja loomine täiesti automaatseks, jättes vahele küsimused kasutaja täisnime ja muude andmete kohta. 

ENV failid on lihtsad tekstifailid, mida kasutatakse rakenduse konfigureerimiseks ja turvamiseks. Selle asemel, et kirjutada seaded otse koodi sisse 

lb

<img width="572" height="378" alt="Screenshot 2026-05-11 at 09 47 08" src="https://github.com/user-attachments/assets/438b737b-e60e-4ef9-90a5-41d4fc8886b6" />


<img width="572" height="154" alt="Screenshot 2026-05-11 at 09 47 28" src="https://github.com/user-attachments/assets/1d126c82-e13f-4fcd-a919-7e3fb7973084" />


4.
#!/bin/bash


# --- KONFIGURATSIOON ---
APP
_
SERVERS=("app1
_
ip" "app2
_
ip") # Lisa siia oma serverite IP-aadressid
REMOTE
_
USER="deploy"
APP
_
PATH="/opt/demoapp"
TIMESTAMP=$(date +%Y%m%d%H%M%S)
NEW
RELEASE="$APP
PATH/releases/$TIMESTAMP"
_
_
GIT
_
REPO="https://github.com/kasutaja/repo.git" # Sinu repo aadress
echo "🚀 Alustan deploy protsessi: $TIMESTAMP"


# 1. TÕMBA UUS VERSIOON (Lokaalselt admin masinas)
echo "📥 Tõmban koodi...
"
rm -rf /tmp/build && mkdir -p /tmp/build
git clone --depth 1 $GIT
_
REPO /tmp/build



# 2. PUSI UUS VERSIOON SERVERITESSE
for SERVER in "${APP
_
SERVERS[@]}"; do
echo "📤 Kopeerin koodi serverisse $SERVER...
"
# Loo uus release kaust
ssh $REMOTE
_
USER@$SERVER "mkdir -p $NEW
_
RELEASE"
# Kopeeri failid (rsync on kiirem ja targem kui scp)
rsync -avz /tmp/build/ $REMOTE
_
USER@$SERVER:$NEW
_
RELEASE/
# Salvestame vana sümbollingi sihi, et saaksime rollbacki teha
PREVIOUS
_
RELEASE=$(ssh $REMOTE
_
USER@$SERVER "readlink -f
$APP
_
PATH/current")


# 3. UUENTA LINK JA RESTARTI TEENUS
echo "🔄 Uuendan sümbollinki ja restardin teenuse...
"
ssh $REMOTE
_
USER@$SERVER "ln -sfn $NEW
RELEASE $APP
_
_
sudo systemctl restart demoapp"
PATH/current &&




# 4. HEALTHCHECK
echo "🩺 Kontrollin tervist (Healthcheck)...
"
sleep 3
RESPONSE=$(curl -s -o /dev/null -w "%{http_
code}" http://$SERVER/)
if [ "$RESPONSE"
-eq 200 ]; then
echo "✅ Server $SERVER on üleval!"
else
echo "❌ VIGA: Server $SERVER vastas koodiga $RESPONSE. TEEN ROLLBACKI!"
$APP
# 5. ROLLBACK
ssh $REMOTE
_
USER@$SERVER "ln -sfn $PREVIOUS
_
_
PATH/current && sudo systemctl restart demoapp"
echo "🔙 Rollback teostatud serveris $SERVER.
"
exit 1
RELEASE
fi
done
echo "🎉 Kõik serverid on edukalt uuendatud!"

<img width="835" height="380" alt="Screenshot 2026-05-11 at 09 49 20" src="https://github.com/user-attachments/assets/c595aac7-c7b9-4e39-a2a6-c9bf10e5d3b7" />

5.
<img width="835" height="183" alt="Screenshot 2026-05-11 at 09 49 36" src="https://github.com/user-attachments/assets/9ac350e1-791e-40ad-9ba8-c8de839003b9" />

Tulemused:Parandatud

<img width="835" height="356" alt="Screenshot 2026-05-11 at 09 50 20" src="https://github.com/user-attachments/assets/afef7aea-c671-4734-ae47-0913b36b5c75" />


<img width="835" height="234" alt="Screenshot 2026-05-11 at 09 50 27" src="https://github.com/user-attachments/assets/763ff46c-47d3-4dae-9315-7b3775bec879" />


<img width="835" height="265" alt="Screenshot 2026-05-11 at 09 50 33" src="https://github.com/user-attachments/assets/3445594e-37ad-49c1-95c5-9b1b5c2332fa" />
