# akenduse-live-viimine-2-serveriga-load-balancing

Ülesanne: “Rakendus live’i kahe serveriga”
Eesmärk
Panna tööle veebirakendus nii, et:

Rakendus on kahes serveris (app1, app2).
Klient kasutab ühte URL-i / IP-d, mis suunab liikluse kahe serveri vahel (load balancing) ja töötab edasi ka siis, kui üks server maas (failover).
Deploy on automatiseeritud skriptiga (minimaalselt 1 käsk = “uusi muudatusi live’i”).
Süsteem on kontrollitav (healthcheck + logid).
Sul peaks olema kokku 3 masinat.

Host-Only võrk (lihtsaim koolis), näiteks 192.168.56.0/24.

lb (load balancer / reverse proxy)

IP: 192.168.56.10
app1 (rakenduse server 1)

IP: 192.168.56.11
app2 (rakenduse server 2)

IP: 192.168.56.12

 Nginx serveerib HTML/CSS + üks endpoint /health (kõige lihtsam).

Funktsionaalsed nõuded (mida peab näitama)
1) Rakendus peab kuvama “serveri identiteeti”
Näiteks esilehel peab olema rida:

Served by: app1 või Served by: app2
Lisada ka timestamp ja hostname (hostname), et oleks selge, mis server vastas.
2) Load balancer peab jaotama liiklust
Kui teed 10 päringut (curl / refresh), näed vaheldumisi app1/app2 vastuseid.
3) Failover
Kui app1 teenus peatada või server välja lülitada, peab URL endiselt töötama (teenindab app2).
4) Automatiseeritud deploy
Üks skript (näiteks deploy.sh) serveris lb või eraldi “admin” masinas:

Tõmbab uue versiooni Git repo’st (või kopeerib release’i).
“Pusib” uue versiooni app1 ja app2 masinasse (scp/rsync).
Restartib teenuse (systemctl restart …).
Kontrollib healthchecki (curl …) ja raporteerib tulemuse.
Kui healthcheck ebaõnnestub, teeb rollback eelmisele versioonile (miinimum: hoiab eelmist buildi alles ja taastab).
5) Logimine
Rakenduse logid peavad minema kas:

journalctl -u <service> (systemd) või
/var/log/<app>/app.log
 Nginx access/error logid peavad olema olemas ja vaadeldavad.
