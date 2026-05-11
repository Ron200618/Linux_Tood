# Initial-minecraft-server-setup

Esimese asjana, me tahame teha endale normaalse ubuntu desktop masina, mille peale m ehitame minecraft serveri.



Dockeri paigaldamine:


sudo apt-get remove docker docker-engine docker.io containerd runc

sudo apt update

sudo apt install -y ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo $ID)/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/$(. /etc/os-release; echo $ID) $(. /etc/os-release; echo $VERSION_CODENAME) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin


sudo usermod -aG docker $USER

newgrp docker

Tee uus kaust endale failipuus kuhu hakkad dockeri minecrafti serverit installima.


tee sinna sisse fai: docker-compose.yml

Pane docker-compose.yml faili see:

version: "3.9"



services:

  paper:

    image: itzg/minecraft-server:latest

    container_name: mc-paper

    restart: unless-stopped

    ports:

      - "25565:25565"

    environment:

      EULA: "TRUE"

      TYPE: "PAPER"

      VERSION: "1.21.8"

      MEMORY: "2G"

    volumes:

      - ./data:/data

Command et startida docker:

docker compose up -d


<img width="496" height="312" alt="Screenshot 2026-05-11 at 04 12 08" src="https://github.com/user-attachments/assets/d6db278f-e9c1-4706-9d8b-aa12b166f817" />
