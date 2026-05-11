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

<img width="584" height="256" alt="Screenshot 2026-05-11 at 09 23 04" src="https://github.com/user-attachments/assets/6afc6062-d515-47a4-a589-01c26f521e6c" />

<img width="522" height="256" alt="Screenshot 2026-05-11 at 09 23 10" src="https://github.com/user-attachments/assets/1a103df9-c298-4171-8702-b74f29f47417" />

<img width="522" height="256" alt="Screenshot 2026-05-11 at 09 23 15" src="https://github.com/user-attachments/assets/061f5406-a420-4b9a-b26c-ea8d132778d2" />


<img width="522" height="256" alt="Screenshot 2026-05-11 at 09 23 21" src="https://github.com/user-attachments/assets/ce1afb3f-9283-4024-b16d-6cb524782e28" />

<img width="485" height="224" alt="Screenshot 2026-05-11 at 09 23 31" src="https://github.com/user-attachments/assets/eb375b28-bdce-4957-9b5f-fc7f9acd2abb" />

<img width="606" height="450" alt="Screenshot 2026-05-11 at 09 23 41" src="https://github.com/user-attachments/assets/2b97ac8c-4d1c-4e75-8e08-a974698ba2b2" />

<img width="606" height="337" alt="Screenshot 2026-05-11 at 09 23 49" src="https://github.com/user-attachments/assets/610c2be8-1124-4e40-988b-8454d34acdb3" />

<img width="606" height="337" alt="Screenshot 2026-05-11 at 09 23 53" src="https://github.com/user-attachments/assets/48c42886-b0dc-4163-a1cc-e2ebdbfe8f27" />


<img width="606" height="318" alt="Screenshot 2026-05-11 at 09 23 59" src="https://github.com/user-attachments/assets/2126084e-ce8a-4169-96ed-9b5b39f601b8" />


