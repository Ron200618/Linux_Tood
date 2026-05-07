# linux-webserver-automation
TTHK-st õpitud teemad
# tööjuhised

Seadista VM järgnevalt.
Lae alla inotify-tools, apache2.
Enable vajalikud moodulid:
sudo a2enmod auth_basic authn_file authz_user

Ülesanne:
1. Tee desktopile kaust /ülesanne_1 ning sinna tee terve enda ülesanne.
2. Seadista apache2 veebiserver nii, et see oleks kättesaadav sinunimi.local domeenil.
3. Lisa apache2 veebilehele 404 errori leht, kus kuvad kasutajae sõnumi "kahjuks see leht ei ole kättesaadav"
4. Tee 3 uut gruppi: "optajaid, opilased ja veebiadmin"
5. Lisa script, millega saad automaatselt lisada kasutajaid ja neid kasutajaid gruppidesse.
6. Tee selle scriptiga automaatselt 10 opilase kasutajat ja lisad nad õigesse gruppi ning teeb kausta /var/www/html faili opilasenimi.html.
7. Tee uus script millega teed automaatselt 3 opetaja kasutajat ja lisad nad õigesse gruppi.
8. Lisa õigused, et veebilehel saavad enda lehte (ehk opilasenimi.html) vaadata ainult seda faili omav õpilane.
9. Tee script mis varundab kõik failid mis on /var/www/html-is kausta /var/www/backup iga 5 minuti tagant.

<img width="590" height="253" alt="image" src="https://github.com/user-attachments/assets/ffa5b0de-2fa9-402a-b528-182b60674309" />

<img width="527" height="308" alt="image" src="https://github.com/user-attachments/assets/c8c8c0bd-dbbd-4f8b-a344-bc5beaca9626" />

<img width="603" height="521" alt="image" src="https://github.com/user-attachments/assets/fa26b888-0b67-4696-bf94-c64a08acbdb8" />

<img width="598" height="345" alt="image" src="https://github.com/user-attachments/assets/5d5157fc-8f84-4196-9083-d4705a680727" />

<img width="492" height="221" alt="image" src="https://github.com/user-attachments/assets/f56079d6-9745-482a-93ba-5dfa50eb248b" />

<img width="598" height="453" alt="image" src="https://github.com/user-attachments/assets/25a08ac5-bf7f-4268-ab30-29b3e0257e58" />

<img width="603" height="351" alt="image" src="https://github.com/user-attachments/assets/a1cd374e-9ee7-4adb-b236-23868962d1f0" />

<img width="602" height="333" alt="image" src="https://github.com/user-attachments/assets/5aebb2f8-7d30-4fb6-965d-df9fdae533cb" />

<img width="605" height="317" alt="image" src="https://github.com/user-attachments/assets/91e68a3e-7ed6-4821-818d-1a5aaf050ea0" />
