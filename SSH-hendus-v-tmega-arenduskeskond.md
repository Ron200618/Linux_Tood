# SSH-hendus-v-tmega-arenduskeskond
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

<img width="580" height="318" alt="Screenshot 2026-05-11 at 06 33 31" src="https://github.com/user-attachments/assets/2f05fc5b-aa52-4edd-9542-d5472bd5ea57" />

<img width="580" height="81" alt="Screenshot 2026-05-11 at 06 33 52" src="https://github.com/user-attachments/assets/5624cbfe-3d9e-4a3e-aff6-8fc40de76bca" />

<img width="580" height="333" alt="Screenshot 2026-05-11 at 06 33 59" src="https://github.com/user-attachments/assets/88b8c101-07ec-4e14-9b12-47fc7b2077a1" />

<img width="598" height="293" alt="Screenshot 2026-05-11 at 06 34 08" src="https://github.com/user-attachments/assets/f50dd55d-ae5d-40c9-b85c-b97a39a10dd1" />

<img width="598" height="185" alt="Screenshot 2026-05-11 at 06 34 17" src="https://github.com/user-attachments/assets/7bc40005-f239-49d7-a9ab-f87025da3b9c" />


<img width="598" height="338" alt="Screenshot 2026-05-11 at 06 34 21" src="https://github.com/user-attachments/assets/b6f2a926-2b79-480c-80a2-239b125b461c" />

<img width="598" height="265" alt="Screenshot 2026-05-11 at 06 34 27" src="https://github.com/user-attachments/assets/e5841906-f796-4f0f-9cfb-5b2aeaa98f08" />

<img width="598" height="312" alt="Screenshot 2026-05-11 at 06 34 32" src="https://github.com/user-attachments/assets/06dbf6b2-8301-4879-954d-82c039aacadf" />

<img width="597" height="49" alt="Screenshot 2026-05-11 at 06 34 43" src="https://github.com/user-attachments/assets/54e1e8b1-800c-49e9-997c-f8b543412da1" />

<img width="597" height="89" alt="Screenshot 2026-05-11 at 06 34 48" src="https://github.com/user-attachments/assets/c9d77895-24ca-4bed-a183-459e86fb6fba" />
