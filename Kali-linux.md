# Kali-linux

ee endale VM kali linux ja ubuntu.

Seadista ubuntus SSH ja tee 3 uut kasutajat.

Sea 3le kasutajale paroolid nii, et 1 on kerge, 2 on keskmise tugevusega ja kolmas on turvaline parool.

<img width="323" height="439" alt="image" src="https://github.com/user-attachments/assets/effd794b-289e-4692-9761-d8960615bd18" />
Seadista host only ühendus mõlemale enda masinale.

Kalis tee endale samasugune failipuu:
Miks? Me proovime SSH-ga sisse "häkkida" enda ubuntu masinasse.

 Selleks kasutame tööriista nimega hydra.

 <img width="850" height="477" alt="image" src="https://github.com/user-attachments/assets/06cd6832-a86f-4cc4-a34e-38fc636c9fa1" />
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

<img width="1044" height="127" alt="image" src="https://github.com/user-attachments/assets/53348fd6-a859-4b7b-9d01-ab4887809392" />

Muuda nüüd enda kofiguratsiooni nii, et sa saaksid banni siis, kui oled 3 korda valesti parooli pannud 1 minuti jooksul ning siis saad banni ainult 3 minutiks.



# minu osa

<img width="619" height="565" alt="image" src="https://github.com/user-attachments/assets/66fc5577-67db-4435-ae84-419ff6101441" />

<img width="587" height="459" alt="image" src="https://github.com/user-attachments/assets/8a537ad4-8e38-495c-bcad-c851125ee4a2" />
