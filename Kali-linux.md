# Kali-linux

ee endale VM kali linux ja ubuntu.

Seadista ubuntus SSH ja tee 3 uut kasutajat.

Sea 3le kasutajale paroolid nii, et 1 on kerge, 2 on keskmise tugevusega ja kolmas on turvaline parool.

<img width="319" height="422" alt="Screenshot 2026-05-11 at 05 10 37" src="https://github.com/user-attachments/assets/a711f772-d8ae-4252-855d-1fbf83d2a368" />

Seadista host only ühendus mõlemale enda masinale.

Kalis tee endale samasugune failipuu:
Miks? Me proovime SSH-ga sisse "häkkida" enda ubuntu masinasse.

 Selleks kasutame tööriista nimega hydra.

<img width="725" height="409" alt="Screenshot 2026-05-11 at 05 10 45" src="https://github.com/user-attachments/assets/c36d5cc1-fabc-45a7-b9b4-1c25fd54e6d5" />

 
hydra on tööriist mis proovim meie antud paroolide ja kasutajanimedega luua SSH ühendust meie ubunut masinaga. Kui me teame näiteks, et ubuntu kasutajanimi on "robin", siis me võime teha paroolifaili (või alla laadida kusagilt netist) mis on sadu tuhandeid ridu pikk, ning see proovib kõik need paroolid läbi.

Käsk (Kali VM-is):

Bash

hydra -L kasutajad.txt -P paroolid.txt ssh://<UBUNTU_MASINA_IP> -t 4 -V
Seletus:

-L kasutajad.txt: Kasutajanimede nimekiri.
-P paroolid.txt: Paroolide nimekiri.
ssh://<UBUNTU_MASINA_IP>: Määrab protokolli (SSH) ja sihtmärgi.
-t 4: Määrab samaaegsete ühenduste arvu. (See on oluline, et vältida sihtsüsteemi ummistamist ja potentsiaalset Fail2Ban'i käivitamist).
-V: Näitab kogu protsessi (Verbose).

Tee kindlasti kuvatõmmis edukast hydrast.

Nüüd lähme ubunut masinasse ning vaata enda ssh ühenduste logifaili, ja kas sa näed sisselogimiskatseid läbi ssh? Tee kindlasti kuvatõmmis koos selgitusega mis sa näed.

Mina näen enda failis et sinna on palju failed requeste tehtud erinevatele kasutajatele ja teie kui süsadminid peaksite aru saama, et see on turvarisk.
Vaja on teha lahendus, mis piiraks seda.

Nii, nüüd enableme firewalli ubuntus ja paneme sinna vajalikud reeglid et saaksime ssh ühendusi luua.
testime uuesti kui firewall on enabled.

Kui firewall on enabled ja saad ikka ssh ühenduse luua võid edasi minna.

Laeme alla tööriista fail2ban.
Tee endale selgeks ja uuri internetist mis asi see fail2ban täpsemalt on.
Minu selgitus:
See on tööriist mis jälgib auth.log kasuta ning vastavalt sinu kofiguratsioonile bannib kasutajad ssh ühenduse loomisest kindlaks ajaks.
selgita, mis tingimustel me võiksimesellist lahendust kasutada?

Proovi uuesti hydra kasutada, kui kõik on õige siis peaksid banni saama.

<img width="725" height="72" alt="Screenshot 2026-05-11 at 05 10 55" src="https://github.com/user-attachments/assets/cfc8f0fd-5588-4af2-9d21-c3c47d58251e" />


Muuda nüüd enda kofiguratsiooni nii, et sa saaksid banni siis, kui oled 3 korda valesti parooli pannud 1 minuti jooksul ning siis saad banni ainult 3 minutiks.



# minu osa

<img width="417" height="104" alt="Screenshot 2026-05-11 at 04 16 01" src="https://github.com/user-attachments/assets/0a17fb5e-2a3f-48c2-95c5-f8b19c828b2d" />

<img width="552" height="112" alt="Screenshot 2026-05-11 at 04 16 10" src="https://github.com/user-attachments/assets/18992d4a-1f92-4ccc-85f7-aa7796c1d171" />

<img width="552" height="304" alt="Screenshot 2026-05-11 at 04 16 25" src="https://github.com/user-attachments/assets/a9a137a6-1915-4c6e-84e2-f0d7ba881822" />

<img width="480" height="435" alt="Screenshot 2026-05-11 at 04 16 36" src="https://github.com/user-attachments/assets/bb9c8e0d-d618-42b9-baee-151669b89202" />

<img width="561" height="400" alt="Screenshot 2026-05-11 at 04 16 46" src="https://github.com/user-attachments/assets/e0fda817-8fc7-4c9a-920b-7d5d329a3db5" />

<img width="561" height="318" alt="Screenshot 2026-05-11 at 04 16 54" src="https://github.com/user-attachments/assets/3e51855c-9ec3-47ef-b56b-8f1fc2b9ad35" />

<img width="561" height="198" alt="Screenshot 2026-05-11 at 04 17 06" src="https://github.com/user-attachments/assets/4400f8d4-eec0-471e-92c1-d3a899ec982f" />

<img width="350" height="47" alt="Screenshot 2026-05-11 at 04 17 17" src="https://github.com/user-attachments/assets/e591c851-953b-42bd-89ce-33a4e095949d" />

<img width="566" height="274" alt="Screenshot 2026-05-11 at 04 17 28" src="https://github.com/user-attachments/assets/7fdd1ff6-5b70-4f65-ae19-890162c47287" />

<img width="566" height="327" alt="Screenshot 2026-05-11 at 04 17 33" src="https://github.com/user-attachments/assets/0994fe68-cebc-48ff-9a00-d473a4f8e439" />


<img width="566" height="195" alt="Screenshot 2026-05-11 at 04 17 41" src="https://github.com/user-attachments/assets/4aa487cf-096b-43d7-90f9-d9c3004b2da1" />











