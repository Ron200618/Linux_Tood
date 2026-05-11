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

<img width="712" height="113" alt="see ja siit edasi" src="https://github.com/user-attachments/assets/efd04121-6c64-4081-a495-260b6ccfae8b" />

<img width="712" height="406" alt="Screenshot 2026-05-11 at 06 53 29" src="https://github.com/user-attachments/assets/c5a13232-0381-4864-a4e3-67e56bbe629c" />

<img width="712" height="357" alt="Screenshot 2026-05-11 at 06 53 39" src="https://github.com/user-attachments/assets/9034c724-56d6-4dfe-98de-03418978bc62" />

<img width="712" height="357" alt="Screenshot 2026-05-11 at 06 53 44" src="https://github.com/user-attachments/assets/4f2359a3-68d0-4580-8057-61a304384b26" />

<img width="712" height="357" alt="Screenshot 2026-05-11 at 06 53 49" src="https://github.com/user-attachments/assets/cd1fc243-5148-4442-a611-f3836458dad2" />

<img width="712" height="179" alt="Screenshot 2026-05-11 at 06 53 55" src="https://github.com/user-attachments/assets/a4b33ff6-4e64-4189-beb1-ab769d243dea" />




Nüüd kui oleme scripti käivitanud, veendunud et kõik failid läksid driimou_shared kaustast backup, siis võime selle scripti crontabi lisada.
Dokumentatsioonis peab olema välja toodud kuidas sa testisid kas see script töötab? Mina testisin nii, et panin 3 faili driimou_shared kausta, panin backup.sh scripti tööle, ning veendusin et pakitud fail on backup kaustas ning ma unpackisin selle siis seal olid kõik minu 3 faili olemas. (Scripti testides tuleb script käivitada sudo õigusega)

Osa 3: Lõpplahendus
Lõpplahendusena, me oleme teinud failiserver. Sarnast lahendust kasutatakse ka Teie koolis. 
Kui avate enda kooliarvutis File Exploreri ja vajutate "See arvuti" peale.
Siis näete, et teil on seal mitu ketast (I: M: O: R: jne jne)
Meie oleme teinud suhteliselt sama asja.

Näeme, et meie samba failiserver peaks asuma kasutas /srv/

<img width="712" height="122" alt="Screenshot 2026-05-11 at 06 54 26" src="https://github.com/user-attachments/assets/ac9ca173-d80c-4349-8e5f-6b12d1f282c5" />


Nüüd me tahame ühineda sinna failiserverisse windowsi arvutist sestsee on kõige populaarsem kontoritöö operatsioonisüsteem.
Laadige alla windowsi VM.

Muutke nii Ubuntu VM-is ja Windowsi VM-is network setinguid - pange internal peale:
<img width="558" height="249" alt="Screenshot 2026-05-11 at 06 54 37" src="https://github.com/user-attachments/assets/7a9fd941-ee9a-475e-b186-7d41b8a0eb27" />


Tee järgnevad mudatused windosis>mine etherneti setingutesse:
<img width="558" height="249" alt="Screenshot 2026-05-11 at 06 54 41" src="https://github.com/user-attachments/assets/23c4e048-dbdb-4515-b95f-13b557bb535f" />


Vajuta "Adapteri suvandite muutmine:
<img width="558" height="249" alt="Screenshot 2026-05-11 at 06 54 52" src="https://github.com/user-attachments/assets/3698d0d1-66a2-4519-88d1-11bb3101f0d7" />


Vajut Atribuudid:
<img width="441" height="314" alt="Screenshot 2026-05-11 at 06 55 01" src="https://github.com/user-attachments/assets/c28471d5-c55b-40a7-8afd-50606e15de9c" />


Vali Internet-protocol version 4 ja siis vajuta uuesti atribuudid:
<img width="353" height="457" alt="Screenshot 2026-05-11 at 06 55 11" src="https://github.com/user-attachments/assets/72882fb4-d02b-49e7-af67-a111cdb568fb" />


Seadista see järgnevalt ja salvesta:
<img width="353" height="457" alt="Screenshot 2026-05-11 at 06 55 15" src="https://github.com/user-attachments/assets/b3421b37-f456-42b1-9476-e8de2fc93bec" />


Pane enda ubuntu terminali command:
sudo ip addr add 10.0.0.20/24 dev enp0s3
Peale seda vaata enda ip-d (ip a) ja veendu et 10.0.0.20 on seal,

Seejärel pane enda windowsi käsureal command:
sudo ip addr add 10.0.0.10/24 dev enp0s3
Ja veendu et see muutus sinu IP-ks - (ipconfig)

Proovi windowsi arvutist pingida ubuntu masinat, kui vastus on positiivne, siis võiks töötada.

<img width="503" height="231" alt="Screenshot 2026-05-11 at 06 55 23" src="https://github.com/user-attachments/assets/05e65192-8bfd-4636-bcd2-b0133ca9c6ec" />


Minge Windowsis file explorerisse, ning proovige ühendada ennast mingisse kausta millele te endale õiguse olete andnud:

<img width="386" height="197" alt="Screenshot 2026-05-11 at 06 55 32" src="https://github.com/user-attachments/assets/289f0575-174c-4faa-a0f3-6759c19c5ece" />

<img width="713" height="526" alt="Screenshot 2026-05-11 at 06 55 41" src="https://github.com/user-attachments/assets/8431fc71-dbf2-4cf3-a22d-1562aaa2d28c" />

<img width="465" height="222" alt="Screenshot 2026-05-11 at 06 55 52" src="https://github.com/user-attachments/assets/5b1a6bc9-2250-4487-bfd8-52a806cd64ab" />

<img width="729" height="176" alt="Screenshot 2026-05-11 at 06 56 00" src="https://github.com/user-attachments/assets/b0675142-36ec-4bf5-bd0c-b89f8fabf2fa" />

<img width="411" height="368" alt="Screenshot 2026-05-11 at 06 56 10" src="https://github.com/user-attachments/assets/bc6240f9-1461-4538-937d-ef58ce02a333" />

<img width="411" height="368" alt="Screenshot 2026-05-11 at 06 56 13" src="https://github.com/user-attachments/assets/9d018e0e-3e8d-413d-af4f-902aea976cc6" />

<img width="411" height="368" alt="Screenshot 2026-05-11 at 06 56 16" src="https://github.com/user-attachments/assets/89b82287-a9a6-4f5e-b82b-49b73ed18815" />

<img width="603" height="218" alt="Screenshot 2026-05-11 at 06 56 26" src="https://github.com/user-attachments/assets/39448945-3905-492b-a1d9-6a5e43433286" />

<img width="717" height="104" alt="Screenshot 2026-05-11 at 06 56 43" src="https://github.com/user-attachments/assets/8b2e8e5b-f090-4f10-88f8-c8d2eac7dc40" />


# minu tehtud osa

<img width="597" height="365" alt="Screenshot 2026-05-11 at 06 45 40" src="https://github.com/user-attachments/assets/0e85e613-02ce-4c74-a647-661f4a0802f4" />

<img width="597" height="444" alt="Screenshot 2026-05-11 at 06 46 09" src="https://github.com/user-attachments/assets/d13e06f2-f2fd-4bc8-84fb-80efcee94cbe" />

<img width="597" height="327" alt="Screenshot 2026-05-11 at 06 46 16" src="https://github.com/user-attachments/assets/5d24f2fa-4c20-4cee-a17e-650c9d44146c" />

<img width="597" height="141" alt="Screenshot 2026-05-11 at 06 46 27" src="https://github.com/user-attachments/assets/b8074844-a3e1-4d1a-a057-979ba8aded22" />

<img width="597" height="141" alt="Screenshot 2026-05-11 at 06 46 34" src="https://github.com/user-attachments/assets/071c4134-7825-40e5-8946-394e297473ba" />

<img width="597" height="200" alt="Screenshot 2026-05-11 at 06 46 40" src="https://github.com/user-attachments/assets/8b19a592-d118-4e55-b2d4-db0871747507" />

<img width="597" height="200" alt="e" src="https://github.com/user-attachments/assets/0b41ee6f-26bd-450b-8744-3d57dadaa23c" />


Jah, kergem on teha skript, mis teeb kõik basic updatid ära, et oleks kergem edasi liiguda teiste asjadega. 

<img width="597" height="200" alt="Screenshot 2026-05-11 at 06 47 24" src="https://github.com/user-attachments/assets/b1b77c14-eb5a-49a8-82e3-7c4040c81fc0" />

<img width="473" height="94" alt="Screenshot 2026-05-11 at 06 47 35" src="https://github.com/user-attachments/assets/292d6d27-017c-41c7-942e-ce3421e373c9" />

<img width="607" height="379" alt="Screenshot 2026-05-11 at 06 47 46" src="https://github.com/user-attachments/assets/8d6a0731-fafb-4c1a-bd78-3ce8240dd184" />

<img width="607" height="166" alt="Screenshot 2026-05-11 at 06 48 01" src="https://github.com/user-attachments/assets/e8d51f47-0b32-46ed-a67e-a8c506d09245" />

<img width="543" height="283" alt="Screenshot 2026-05-11 at 06 48 10" src="https://github.com/user-attachments/assets/0a82da45-b6ae-4d57-b570-59b31a3654f9" />

<img width="352" height="164" alt="Screenshot 2026-05-11 at 06 48 20" src="https://github.com/user-attachments/assets/045899be-21f0-400e-ac72-468eb93208b7" />

<img width="604" height="451" alt="Screenshot 2026-05-11 at 06 48 34" src="https://github.com/user-attachments/assets/8837748a-bc28-49ef-bc9e-3bb0e03f6ae0" />

<img width="209" height="61" alt="Screenshot 2026-05-11 at 06 48 45" src="https://github.com/user-attachments/assets/e3d07a45-e84c-4493-a2bf-7f92fa05ca20" />


