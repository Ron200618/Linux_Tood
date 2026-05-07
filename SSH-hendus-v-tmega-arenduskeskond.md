# Linux_-pingud
TTHK-st õpitud Linux teadmised

1. Lae alla "apache2" ja "openssh-server".
2. Seadista VM vastavalt, et saaks luua SSH ühendus.
3. Loo enda windowsi arvutis SSH võti, et saaks luua ühenduse ilma paroolita.
ssh-keygen -t ed25519 -C "kasutajanimi@hostname"
See command teeb sinu arvutis ssh võtme.
Vaata, kuhu see võti salvestus, ning pane see enda VMi
Näide: "scp -P 2222 ~/.ssh/id_ed25519.pub robin@127.0.0.1:/home/robin/"
Jälgi, et ei oleks erroreid neid commande kirjutades.
Mine enda virtuaalmasinasse, ning tee järgnevad muudatused
3.1 Liiguta id_ed25519.pub fail .ssh kausta.
Command: 
mkdir -p ~/.ssh
cat ~/id_ed25519.pub >> ~/.ssh/authorized_keys

Võid etteruttavalt teha juba arendajad1 ja arendaja2 ssh võtmed.
4. Muuda enda ssh configi, nii et parooliga ei saa ühendada.
Ava fail:  /etc/ssh/sshd_config
Tee muudatused:
PubKeyAuthentication yes
PasswordAuthentication no

Taaskäivita ssh
6. Proovi ühendada ssh kaudu enda VMi, kas peab parooli kasutama? Kui ei, siis töötab hästi! Selgita miks selline lahendus on laialdaselt kasutatud.

7. Tee grupp "webdevelopers"
8. Tee 2 kasutajate "arendaja1" ja "arendaja2" ning pane nad gruppi webdevelopers.
9. Tee desktopile kaust /website
10. Anna webdevelopersitele ligipääs sellele kasutale ja et nad saaksid sinna sisse kirjutada.
11. tee script mis kopeerib iga 5 minuti tagant /website kaustast kõik uued muudatused kausta /var/www/html
12. Tee SSH võtmed ka kasutajatele arendaja1  ja arendaja
Uute võtmete tegemine ilma vana overwritemisetta:  ssh-keygen -t ed25519 -C "robin_vmuser1" -f C:/Users/robin.rattasep/.ssh/id_ed25519_vmuser1
13. Ühenda windowsi arvutist enda VMi, tee muudatusi kaustas /website ja vaata kas see muudatused on ka sinu http://localhost lehel, kui ei ole, siis miks?

<img width="579" height="310" alt="image" src="https://github.com/user-attachments/assets/6f8d6df7-8712-4a14-8b7c-0d6f6df6e4b0" />

<img width="597" height="65" alt="image" src="https://github.com/user-attachments/assets/4bc2c818-e911-4276-ab79-59e8e6fd2ab6" />


<img width="601" height="335" alt="image" src="https://github.com/user-attachments/assets/0d449105-baad-4a55-b1ca-8e228796a451" />

<img width="608" height="296" alt="image" src="https://github.com/user-attachments/assets/c154d6b7-aa0a-4ca7-9407-0d5672a8c1c2" />

<img width="589" height="182" alt="image" src="https://github.com/user-attachments/assets/a1f4210f-4e30-4cde-b2cb-09a020619bf0" />

<img width="614" height="347" alt="image" src="https://github.com/user-attachments/assets/63e70e84-d0e0-45f0-ad89-16b459d9b972" />

<img width="604" height="272" alt="image" src="https://github.com/user-attachments/assets/0ee08e6c-3393-437e-bf49-08304f7c29bd" />

<img width="597" height="314" alt="image" src="https://github.com/user-attachments/assets/bd100a6e-c504-4acc-8683-551217615b5f" />

<img width="587" height="46" alt="image" src="https://github.com/user-attachments/assets/a21ea663-72c4-406f-894a-ac0fee0604a7" />

<img width="522" height="84" alt="image" src="https://github.com/user-attachments/assets/2927761f-59bd-483e-b1c8-50f185deed96" />

<img width="604" height="252" alt="image" src="https://github.com/user-attachments/assets/be024a70-e1a2-4163-9604-5156b2015c1b" />
