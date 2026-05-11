# V-ikeettev-tte-Linux-server

Eesmärk: luua praktiline ja töökindel väikeettevõtte serveri lahendus, mis sisaldab:
1.pakettide haldust ja automaatset uuendamist,
2. failiserverit (failide jagamine ja varundus), 
3. logisüsteemi, automaatseid hooldusülesandeid,
4. kaugühenduse haldust administreerimiseks.

Kujuta ette väikest ettevõtet (nt logistikafirma, disainiagentuur või IT-teenusepakkuja), ettevõtte nimeks juhendi huvites panen "Driim OÜ"
kellel on kontoris üks Linux-server. See server peab tegema kolm asja:
1. olema kättesaadav nii kontoris kui kodus
2. sisaldama töötajate failiserverit
3. hooldama ennast — uuendused, varundus, logide haldus.

1 Süsteemi ettevalmistus ja pakettide haldus
1. Veendu, et süsteem on uuendatud.
   
<img width="724" height="123" alt="siit edasi on opsi asjad" src="https://github.com/user-attachments/assets/3c9a8284-ff3d-4901-a648-ad2c5ae56fd8" />



3. Kuna Driim OÜ ei soovi iga päev manuaalselt uuendis teha, siis võiksime selle tegevuse automatiseerida ja ajastada.
Teeme scripti, mis kontrollub uuendusi ja paneb selle logifaili kirja.
Linuxi serverites tehakse tavaliselt logifailid kasuta /var/log/
Kuna tavaliselt /var/log/ kaust on root kasutaja oma ja me ei taha, et seal midagi katki läheks (by default sinu kasutajal ei olegi õigusi et sinna uusi faile jne teha), siis turvalisuse huvides teeme endale uue alamkausta driimou (ettevõtte nimi), paneme selle kausta omanikuks ennast ning teeme sinna kasuta sisse update_check.log faili.

<img width="724" height="283" alt="Screenshot 2026-05-11 at 06 52 07" src="https://github.com/user-attachments/assets/907ebce0-b06e-4762-b336-7d20851e0818" />


Nii, kui meil on olemas logide fail, siis me tahame teha scripti mis kontrollib uuendusi. Varasemalt olete teinud scriptifailid endale mugavas kohas, nüüd teeme need kausta /usr/local/bin <- see on default koht kuhu tavaliselt system-wide scriptid paigutatakse. 
Lähme sinna kausta ning teeme uue scripti faili.

<img width="724" height="297" alt="Screenshot 2026-05-11 at 06 52 24" src="https://github.com/user-attachments/assets/0cf9fb9d-9abc-4aef-815a-9c56a1e664e9" />


Nii, nüüd saame scripti kirjutama hakata - selleks, et kontrollida mitu upgradable packagit meil on, samme kasutada commandi:
"apt list --upgradable | wc -l"
See command vaatab mitu upgradable asja meil on, ja wc -l väljastab meil ridade arvu, kui kõik on uuendatud, peaks tulemus olema 1.

<img width="724" height="356" alt="Screenshot 2026-05-11 at 06 52 30" src="https://github.com/user-attachments/assets/5bf74035-0270-4b7a-8ed2-fc44253422f4" />


Nagu minu pildi pealt näha on, siis "sudo apt update" väljastas et kõik packagid on up to date, ja eelnev command väljastas vastuseks "1".
Tee script mis kontrollib mitu rida see eelnev command väljastab, ja kui see väljastab 1 - siis logifaili kirjutatakse et everything is up to date koos kuupäevaga, ja kui on suurem kui üks, siis kirjutatakse "$(updates) uuendust on saadaval $date"

Lisan kuvatõmmise näite koodist (ei ole 100% tehtud), pane tähele, et kui me updates arvu väljastame, siis me lahutame sellest 1, sest by default see updates annab meil vastuse 1, kui meil on 0 uuendust aadaval.

<img width="724" height="176" alt="Screenshot 2026-05-11 at 06 52 36" src="https://github.com/user-attachments/assets/17be85fc-409a-4b1c-a27f-20bea9c630b7" />



Ja kui su script on lõplikult valmis, testi seda, tulemus võiks olla sarnane nagu minul:

<img width="724" height="176" alt="Screenshot 2026-05-11 at 06 52 40" src="https://github.com/user-attachments/assets/5f65d06b-cea6-450a-8582-c930e31b4766" />


Nüüd pane see script crontabi jooksma iga 30 minuti tagant. 

Dokumentatsioonis kirjelda, kas võiks teha pigem scripti mis automaatselt uuendab kõiki pakette?




Osa 2 - Samba.
Eesmärk: luua ühiskaust kõikidele töötajatele et kõik saaksid failidele ligi otse internetist.

Paigalda endale teenus "samba"

Jagatud failikaustad me teeme tavaliselt kasuta /srv/
Ehk tee uus alamkaust /srv/driimou_share
Anna sellele kaustale 770 õigused, anna selle kausta omanik ja grupp "root" ile.
Tee uus kasutaja user1 ja samut lisa see kasutaja ka sambasse.

<img width="724" height="191" alt="Screenshot 2026-05-11 at 06 52 46" src="https://github.com/user-attachments/assets/fdf67f89-9f9f-48af-b2e9-e8cdee370953" />


Järgnevalt ava samba konfiguratsioon:
<img width="604" height="43" alt="Screenshot 2026-05-11 at 06 52 53" src="https://github.com/user-attachments/assets/faf848e6-15ec-45cd-9cd0-331a0d1db597" />


Lisa konfiguratsiooni järgnev:


<img width="712" height="413" alt="Screenshot 2026-05-11 at 06 53 03" src="https://github.com/user-attachments/assets/7f8cf38a-0e1d-4794-a51c-7d473f46426b" />


Taaskäivita samba
Lae alla teenus smbclient
Testime seda ning vastus võiks olla midagi sarnast:
<img width="712" height="226" alt="Screenshot 2026-05-11 at 06 53 09" src="https://github.com/user-attachments/assets/25a42953-3c4f-424a-910f-e687d0722b6f" />


Nüüd meil peaks olemas olema failiserver - aga kuna Driim OÜ soovib ka backuppe ja ka seda et kettaruum täis ei saaks.
Mis me nüüd teeme? <- Scripti mis varundab faile, ning ka pakib neid kokku.
Teeme uue varunduskausta samma folderisse kus meie logid on.

Lähme kausta kuhu me tavaliselt kõik scriptid teeme ja teeme sinna uue scripti backup.sh.

Minu skript on selline:
<img width="712" height="68" alt="Screenshot 2026-05-11 at 06 53 15" src="https://github.com/user-attachments/assets/b97764f4-0a1c-4c31-aa9b-9f1d19c687b4" />


See paneb backup folderisse pakitud (.tar.gz") faili, pakime sellepärast et ta võtaks vähe ruumi, ja sinna faili sisse paneme /srv/driimou_share kausta ja kõik logid.
Failide pakkimine käib üldiselt "tar" käsuga ja kõik mis see tar teeb olenevalt talle järgnevatest argumentidest. Siin all on 3 pilti mis selgitavad selle võimalusi.



xxxs





<img width="749" height="365" alt="image" src="https://github.com/user-attachments/assets/bcb2aff7-a0c2-43c6-b43e-378b60b42b08" />

<img width="809" height="349" alt="image" src="https://github.com/user-attachments/assets/760176bd-418a-48d9-8be8-d17f6248a82b" />

<img width="787" height="257" alt="image" src="https://github.com/user-attachments/assets/73a17771-90ef-485a-8138-7e4d20afdc8e" />

Nüüd kui oleme scripti käivitanud, veendunud et kõik failid läksid driimou_shared kaustast backup, siis võime selle scripti crontabi lisada.
Dokumentatsioonis peab olema välja toodud kuidas sa testisid kas see script töötab? Mina testisin nii, et panin 3 faili driimou_shared kausta, panin backup.sh scripti tööle, ning veendusin et pakitud fail on backup kaustas ning ma unpackisin selle siis seal olid kõik minu 3 faili olemas. (Scripti testides tuleb script käivitada sudo õigusega)

Osa 3: Lõpplahendus
Lõpplahendusena, me oleme teinud failiserver. Sarnast lahendust kasutatakse ka Teie koolis. 
Kui avate enda kooliarvutis File Exploreri ja vajutate "See arvuti" peale.
Siis näete, et teil on seal mitu ketast (I: M: O: R: jne jne)
Meie oleme teinud suhteliselt sama asja.

Näeme, et meie samba failiserver peaks asuma kasutas /srv/
<img width="1002" height="182" alt="image" src="https://github.com/user-attachments/assets/ca635c11-3547-480c-9194-1e264e82ad71" />
Nüüd me tahame ühineda sinna failiserverisse windowsi arvutist sestsee on kõige populaarsem kontoritöö operatsioonisüsteem.
Laadige alla windowsi VM.

Muutke nii Ubuntu VM-is ja Windowsi VM-is network setinguid - pange internal peale:
<img width="560" height="276" alt="image" src="https://github.com/user-attachments/assets/fe6129d2-b024-4f1a-b87b-92b88cd964e4" />

Tee järgnevad mudatused windosis>mine etherneti setingutesse:
<img width="764" height="301" alt="image" src="https://github.com/user-attachments/assets/9df0e27c-e02d-41b1-bdac-b1ee24b88571" />

Vajuta "Adapteri suvandite muutmine:
<img width="714" height="289" alt="image" src="https://github.com/user-attachments/assets/85af34a7-1790-4b86-b68b-018ee39c8155" />

Vajut Atribuudid:
<img width="430" height="378" alt="image" src="https://github.com/user-attachments/assets/efafe988-830b-4db9-ab17-39b540e481ad" />

Vali Internet-protocol version 4 ja siis vajuta uuesti atribuudid:
<img width="429" height="468" alt="image" src="https://github.com/user-attachments/assets/ed70cc9a-5063-4316-95fc-645b8f8e4be2" />

Seadista see järgnevalt ja salvesta:
<img width="449" height="505" alt="image" src="https://github.com/user-attachments/assets/cc3c517b-a214-4f6f-8a84-3903efac59cb" />

Pane enda ubuntu terminali command:
sudo ip addr add 10.0.0.20/24 dev enp0s3
Peale seda vaata enda ip-d (ip a) ja veendu et 10.0.0.20 on seal,

Seejärel pane enda windowsi käsureal command:
sudo ip addr add 10.0.0.10/24 dev enp0s3
Ja veendu et see muutus sinu IP-ks - (ipconfig)

Proovi windowsi arvutist pingida ubuntu masinat, kui vastus on positiivne, siis võiks töötada.

<img width="508" height="209" alt="image" src="https://github.com/user-attachments/assets/97d0972b-c503-40de-9fd7-c719b5025c77" />

Minge Windowsis file explorerisse, ning proovige ühendada ennast mingisse kausta millele te endale õiguse olete andnud:

<img width="375" height="196" alt="image" src="https://github.com/user-attachments/assets/266d1667-d3f2-4a9d-9118-9b7142fa37b0" />


<img width="1014" height="773" alt="image" src="https://github.com/user-attachments/assets/79c60dbf-731c-4d04-a6b8-e673c15d4156" />

<img width="442" height="219" alt="image" src="https://github.com/user-attachments/assets/5ac48cbb-bed8-4489-b86b-cb24618cdce9" />

<img width="784" height="217" alt="image" src="https://github.com/user-attachments/assets/4a544488-1c6f-4ee8-96a1-d8c2e7b7f59b" />

<img width="408" height="403" alt="image" src="https://github.com/user-attachments/assets/5275c167-95da-4174-9cd3-d3c8e4618dc8" />

<img width="394" height="395" alt="image" src="https://github.com/user-attachments/assets/536cd516-e88b-412c-b844-7e293acf9b1b" />

<img width="761" height="398" alt="image" src="https://github.com/user-attachments/assets/43b71836-f32c-4bc6-b852-cd7b1c184670" />

<img width="591" height="337" alt="image" src="https://github.com/user-attachments/assets/5d56ac21-2a14-4181-a520-8fc11bec68b5" />

# minu tehtud osa

<img width="601" height="371" alt="image" src="https://github.com/user-attachments/assets/ab1dc6a6-f92f-4838-b928-17cc2078aa8d" />

<img width="601" height="464" alt="image" src="https://github.com/user-attachments/assets/a676809a-afb6-450d-afca-55fe140fec1d" />

<img width="595" height="335" alt="image" src="https://github.com/user-attachments/assets/a0f9d83f-9b81-4cfb-8361-8173a12ecbc2" />

<img width="603" height="141" alt="image" src="https://github.com/user-attachments/assets/c223e0a8-bde8-4f80-877e-9b2b3b9d5fa5" />

<img width="602" height="130" alt="image" src="https://github.com/user-attachments/assets/e70cd1e8-15f7-456c-9a45-64d0033064db" />

<img width="597" height="134" alt="image" src="https://github.com/user-attachments/assets/f5b42506-f842-47b3-9826-61b682991170" />

<img width="608" height="203" alt="image" src="https://github.com/user-attachments/assets/b1c1849b-00ca-41cb-8029-1181f8f0ffee" />

Jah, kergem on teha skript, mis teeb kõik basic updatid ära, et oleks kergem edasi liiguda teiste asjadega. 

<img width="609" height="556" alt="image" src="https://github.com/user-attachments/assets/fd77ddf2-dd7e-41ce-b90a-ca6ec18d326d" />

<img width="810" height="746" alt="image" src="https://github.com/user-attachments/assets/017594d0-5b49-4f1a-b09e-bf34134e37a9" />

<img width="707" height="365" alt="image" src="https://github.com/user-attachments/assets/81b622c0-4528-403f-972d-4b47966e9181" />

<img width="442" height="186" alt="image" src="https://github.com/user-attachments/assets/11aa70a2-01bb-482b-ac0f-bd656e5fd6d1" />


<img width="250" height="65" alt="image" src="https://github.com/user-attachments/assets/b5ecfe27-06d0-4c04-84db-904d5b288e9c" />
