# SSH-henduse-tegemine-windowsi-ja-ubuntu-VM-iga
TTHK-st õpitud teemad

1. Mine enda Ubuntu VM-i. ja lae alla "ssh"
command: "sudo apt install openssh-server"

2. Enable ssh connection: 
"sudo systemctl enable ssh"
"sudo systemctl start ssh"

3. Lülita enda VM välja, ning mine VMi setingutesse. 
Network setingute all mine "port forwarding" ning lisa 
Name: "SSH"
protocol: "TCP"
Host: "127.0.0.0"
Host Port: "2222"
Guest port: "22"

<img width="597" height="206" alt="Screenshot 2026-05-11 at 06 37 58" src="https://github.com/user-attachments/assets/b1e895aa-6556-4d63-9404-9a2a466d6098" />

<img width="597" height="107" alt="Screenshot 2026-05-11 at 06 38 19" src="https://github.com/user-attachments/assets/c2b2c11b-67a8-4c75-9f7d-529a95d4d17e" />

<img width="597" height="186" alt="Screenshot 2026-05-11 at 06 38 27" src="https://github.com/user-attachments/assets/e2128ed6-82a4-4761-aba7-79ff848e814d" />

<img width="597" height="169" alt="Screenshot 2026-05-11 at 06 38 35" src="https://github.com/user-attachments/assets/651b196d-dae7-4452-bc98-0cec504f6954" />

<img width="597" height="438" alt="Screenshot 2026-05-11 at 06 38 45" src="https://github.com/user-attachments/assets/88cc19f5-9322-4f3d-9177-08a010fc5c48" />

<img width="597" height="118" alt="Screenshot 2026-05-11 at 06 38 52" src="https://github.com/user-attachments/assets/840cb3b1-ba87-473b-a673-5bc45074a1fe" />

<img width="597" height="412" alt="Screenshot 2026-05-11 at 06 39 00" src="https://github.com/user-attachments/assets/16c2d8d0-3b86-4aa9-955a-14aef1e89e4f" />


Kris-Saab kirjutada ja lugeda 
Op1- Saab lugeda 
op2-Ei saa failile ligipääsu 
