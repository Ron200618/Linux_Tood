# NextCloudi-lesanded-p2
TTHK-st opitud teadmised

sudo snap install nextcloud

mine localhosti ja tee GUI-ga endale admin kasutaja

Kõik tegevused tee command lainil:
1. Tee command lainilt nexcloudile 3 kasutajat.

sudo nextcloud.occ user:add alice

sudo nextcloud.occ user:list


2. Oih, sinu 3 tehtud kasutajat unustasid enda parooli ja emaili, reseti need nendele.
3. Scripti rutniine maintenece script.
Võid küsida siin kohas abi chatGPT-lt.
Script peab exportima nextcloudi failid, scannima kõik ja logima failid.
Pane see script iga 1 tunni tagant käima.

4. Luba firewallis liiklus portidel 80 ja 8080.
5. Sulge enda arvuti ja lisa port forwardingusse us rida, 
NIMI: nextcloud
PROTOCO: TCP
Host IP: tühi
Host port: 8080
Guest port: 80

6. Pane VM käima ning veendu, et nextcloud töötab.
7. Mine enda windowsi arvutist veebilehele "localhost:8080" - kas saad ligi?
8. Logi mitte admin kontoga sisse, ning salvesta mõned enda failid sinna, kas töötab?
9. Tee google docsi fail kus on screenshot sinu nextcloudi lehest windowsi arvutis ning vähemalt 50 sõnaline kirjeldus miks ja kes kasutavad nextcloudi. Screenshotil PEAB OLEMA näha et tegu on windowsiga, ja sinu täisnimi.

<img width="605" height="407" alt="Screenshot 2026-05-11 at 05 19 00" src="https://github.com/user-attachments/assets/ff151852-a775-4e31-9fce-2f7240aadbea" />

<img width="605" height="366" alt="Screenshot 2026-05-11 at 05 19 05" src="https://github.com/user-attachments/assets/e1ca2984-b4fb-44e2-81a8-7daa2c06cf9a" />

<img width="605" height="308" alt="Screenshot 2026-05-11 at 05 19 14" src="https://github.com/user-attachments/assets/a7b0ea9c-e4b8-4627-a344-c76da1205461" />

<img width="605" height="192" alt="Screenshot 2026-05-11 at 05 19 20" src="https://github.com/user-attachments/assets/f2777dc8-dcee-4f7f-9046-95653e4d4367" />

<img width="605" height="260" alt="Screenshot 2026-05-11 at 05 19 29" src="https://github.com/user-attachments/assets/1b7ff923-ee1f-466d-958f-813f71a13a04" />




Nextcloud on hea ise majutatav pilveplatvorm, mida kasutavad nii eraisikud kui ka organisatsioonid, kes peavad oluliseks andmete kontrolli ja privaatsust. Kuna majutate andmeid ise, säilitate täieliku turvalisust, erinevalt kommertspilveteenustest. 
See on kõik-ühes keskus failide sünkroonimiseks, jagamiseks, koostööks (nagu dokumentide redigeerimine ja videokõned) ning kalendri tegekelemiseks. See on parim valik reguleeritud valdkondade ettevõtetele ja kõigile, kes soovivad võimsat, kohandatavat, avatud lähtekoodiga alternatiivi Google Driveile või Dropboxile. 
