# Linuxi Käsurea Meistriklassi

ETTEVALMISTUS: Andmete genereerimine
Et meil oleks, mida järgmise 10 osa jooksul "remontida", peame looma ühe piisavalt koleda ja vigase logifaili.

Ülesanne õpilasele: Kopeeri allolev skript terminali ja käivita see. See loob faili nimega kaos.log.

Bash

# Loome päise
echo "TIMESTAMP;IP_ADDRESS;USER;ACTION;PAYLOAD;STATUS" > kaos.log

# Genereerime 50 rida segast andmestikku
for i in {1..50}; do
  # Juhuslikud andmed
  IP="192.168.1.$((RANDOM % 255))"
  USER="user_$((RANDOM % 100))"
  BYTES=$((RANDOM % 5000))
  
  # Iga 5. rida on veateade (müra)
  if (( i % 5 == 0 )); then
    echo "[CRITICAL] Kernel panic at memory address 0x00$i" >> kaos.log
  # Iga 7. rida on vale formaadiga (koma eraldajaga semikooloni asemel)
  elif (( i % 7 == 0 )); then
     echo "$(date -Iseconds),$IP,$USER,LOGIN,$BYTES,SUCCESS" >> kaos.log
  # Iga 12. rida on tühikutega risustatud
  elif (( i % 12 == 0 )); then
     echo "$(date +%Y/%m/%d);  $IP  ; $USER ;UPLOAD; $BYTES ; FAIL " >> kaos.log
  else
     # Korrektne rida
     echo "$(date +%Y-%m-%d\ %H:%M:%S);$IP;$USER;DOWNLOAD;$BYTES;SUCCESS" >> kaos.log
  fi
done

echo "Fail 'kaos.log' on edukalt loodud."
1. OSA: Luure ja Orienteerumine
Teema: Kuidas ohutult vaadata suuri faile ja noppida välja kindlaid ridu.

Kujuta ette, et see fail on 100GB suur. Kui sa kirjutad cat kaos.log, siis su terminal jookseb kokku. Pead oskama vaadata faili algust, lõppu ja keskpaika kirurgilise täpsusega.

Tööriistakast (Tools)
head – Faili alguse kuvamine.
tail – Faili lõpu kuvamine.
less – Interaktiivne faili lehitseja (väljumiseks vajuta q).
| (Toru/Pipe) – Ühendab ühe käsu väljundi teise käsu sisendiga.
Harjutused (Self-Study)
Ülesanne 1.1: Päise kontroll Me tahame näha ainult veergude nimesid ja esimest andmerida, et mõista struktuuri.

Uuri käsu head manuaali (man head).
Kirjuta käsk, mis kuvab failist täpselt esimesed 2 rida.
Ülesanne 1.2: Värskeimad sündmused Logifailides on kõige uuem info tavaliselt lõpus.

Kirjuta käsk, mis kuvab faili viimased 7 rida.
Ülesanne 1.3: "Võileiva" meetod (Kombineerimine) See on klassikaline intervjuuküsimus. Meil on vaja näha ridu, mis asuvad faili keskel, näiteks read 15 kuni 20.

head näitab algust. tail näitab lõppu.
Kuidas saaksid neid kombineerida, kasutades toru |?
Vihje:

Võta faili esimesed 20 rida (head abil).
Saada see tulemus (|) edasi käsule tail.
Mitu rida peab tail sellest 20-st võtma, et alles jääksid read 15, 16, 17, 18, 19, 20 (kokku 6 rida)?
Kirjuta see käsk.
Ülesanne 1.4: Turvaline lehitsemine

Käivita less kaos.log.
Proovi less sees otsingut: vajuta /, kirjuta sõna FAIL ja vajuta Enter.
Liigu järgmise vaste juurde klahviga n.
Välju programmist.
2. OSA: Mõõtmine ja Metaandmed
Teema: Enne kui sukeldud sisu analüüsimisse, pead teadma andmete mahtu.

Sysadmini kuldreegel: Ära kunagi käivita rasket töötluskäsku (nt sortimist), kui sa ei tea, kas failis on 100 rida või 100 miljonit rida. Sinu silmad võivad petta, numbrid mitte.

Tööriistakast (Tools)
wc (Word Count) – Loendab ridu, sõnu ja baite.
ls (List) – Failide nimekiri, aga meid huvitavad lipud (flags), mis näitavad faili suurust.
man – Sest sa pead teadma, mida lipud teevad.
Harjutused (Self-Study)
Ülesanne 2.1: Ridade loendamine Meid huvitab kõige rohkem ridade arv, sest see võrdub tavaliselt logikirjete arvuga.

Käivita wc kaos.log. Sa näed kolme numbrit.
Uuri man wc lehte.
Kirjuta käsk, mis väljastab ainult ridade arvu (mitte sõnu ega failinime).
Ülesanne 2.2: Baitide vs Tähemärkide täpsus

Uuri man wc lehte ja leia erinevus lippude -c (bytes) ja -m (chars) vahel.
Kirjuta käsk, mis loeb kokku, mitu tähemärki on failis.

Mõtlemisülesanne: Miks võivad baitide arv ja tähemärkide arv erineda (vihje: UTF-8 ja täpitähed)?
Ülesanne 2.3: Inimloetav suurus Kui fail on väga suur, on baitide arv (nt 54938201) raskesti loetav.

Käsk ls -l kaos.log näitab faili suurust baitides.
Uuri man ls lehte ja otsi lippu "human-readable".
Kirjuta käsk, mis kuvab faili suuruse formaadis (nt 4.0K, 12M, 1G).
Ülesanne 2.4: Torude test (Pipe check) Kasutame eelmise tunni teadmisi.

Me tahame teada, mitu rida on faili esimeses pooles (oletame suvaliselt, et võtame esimesed 15 rida).
Loomulikult on vastus 15, aga sinu ülesanne on tõestada, et torud töötavad.
Kirjuta käsk, mis:

Võtab head käsuga esimesed 15 rida.
Saadab need | kaudu wc käsule.
Väljastab numbri 15.
3. OSA: Nõel heinakuhjas (grep)
Teema: Kuidas leida tuhandete ridade seast need neli, mis on tegelikult olulised.

Kogu Linuxi filosoofia toetub suuresti tööriistale grep (Global Regular Expression Print). See on sinu peamine filter. Kui sa ei oska grep-i, oled sa pime.

Tööriistakast (Tools)
grep – Otsib failist ridu, mis vastavad mustrile.
man grep – Sinu spikker lippude leidmiseks.
Harjutused (Self-Study)
Ülesanne 3.1: Lihtne otsing Meid huvitavad ainult ebaõnnestunud tegevused.

Kirjuta käsk, mis kuvab failist kaos.log kõik read, mis sisaldavad sõna FAIL.
Ülesanne 3.2: Pöördotsing (Invert Match)

Mõnikord on lihtsam öelda, mida sa ei taha. Me tahame näha kõike, mis EI OLE "SUCCESS".

Uuri man grep lehte. Otsi lippu, mis teeb "invert matching" või "select non-matching lines".
Kirjuta käsk, mis kuvab kõik read, kus ei ole sõna SUCCESS.

Märkus: See toob nähtavale nii FAIL read kui ka [CRITICAL] veateated.
Ülesanne 3.3: Loendamine ilma torudeta Eelmises osas õppisid wc -l kasutamist. Sa võiksid teha grep "FAIL" kaos.log | wc -l, et saada vigade arv. Aga grep on võimekas ja oskab ise lugeda.

Leia man grep lehelt lipp (count), mis väljastab tekstiridade asemel ainult leitud vastete arvu.
Kirjuta käsk, mis väljastab ühe numbrina, mitu korda esineb sõna DOWNLOAD.
Ülesanne 3.4: Konteksti leidmine (Detektiivitöö) Failis on read, mis sisaldavad [CRITICAL]. See on tavaliselt tagajärg. Sysadminina tahad sa teada, mis juhtus enne seda.

Uuri man grep lehte ja otsi "Context control".
Meid huvitab lipp, mis näitab ridu enne vastet (lines Before match).
Kirjuta käsk, mis leiab [CRITICAL] vea ja kuvab ka 1 rea, mis oli failis vahetult selle ees.
4. OSA: Kirurgiline sekkumine (sed)
Teema: Andmete muutmine ja parandamine ilma faili avamata.

Meie fail kaos.log on nime vääriline – eraldajad on segamini (kord semikoolon, kord koma) ja tühikud on valedes kohtades. sed (Stream Editor) on käsurea kirurg, mis suudab tekstivoos teha lõikeid ja asendusi lennult.

Tööriistakast (Tools)
sed – Tekstivoo redaktor.
Põhisüntaks: 's/VANA/UUS/g'

s = substitute (asenda)
g = global (kõik esinemised real, mitte ainult esimene)
Harjutused (Self-Study)
Ülesanne 4.1: Eraldajate ühtlustamine Osa ridu kasutab eraldajana koma ,, teised semikoolonit ;. Analüüsiks peab kõik olema ühtne. Otsustame, et semikoolon (;) on standard.

Kirjuta sed käsk, mis võtab sisendiks kaos.log ja asendab kõik komad , semikoolonitega ;.
Väljund peaks jooksma ekraanile, ära faili veel salvesta.
Ülesanne 4.2: Tühikute kaotamine Skript tekitas ridu, kus on ; user_01 ;. Need tühikud segavad hiljem sortimist.

Kirjuta käsk, mis kustutab realt kõik tühikud.
Vihje: Tühiku kustutamine tähendab tühiku asendamist "mitte millegagi" (ehk tühjus //).
Hoiatus: Vaata, mis juhtub kuupäeva kellaajaga (nt 2024-10-01 12:00:00). Kas tühik kadus ka sealt? Selles harjutuses on see aktsepteeritav kahju.
Ülesanne 4.3: Ridade kustutamine sed-iga Eelmises osas kasutasime grep -v, et eemaldada veateateid. Ka sed oskab kustutada.

Süntaks kustutamiseks on '/muster/d' (delete).
Kirjuta käsk, mis kustutab väljundist kõik read, mis sisaldavad sõna CRITICAL.
Ülesanne 4.4: Ahelreaktsioon (Pipeline) Nüüd paneme kõik kokku üheks toruks. Me tahame puhast väljundit.

Ehita käsurida, mis teeb järgmist (järjekord on oluline):

Võtab faili kaos.log.
Kustutab CRITICAL read (kasuta sed).
Asendab komad semikoolonitega (kasuta sed).
Kustutab tühikud (kasuta sed).
Kontroll: Kasuta toru lõpus | head -n 5, et veenduda, kas tulemus on puhas ja ühtlane.
5. OSA: Andmete lahkamine (cut)
Teema: Me ei taha enam terveid ridu. Me tahame kindlaid veerge.

cut on "rumal" aga kiire tööriist. Ta ei oska otsida mustreid nagu grep, ta ei oska asendada teksti nagu sed. Ta oskab ainult ühte asja: lõigata teksti vertikaalselt, kasutades kindlat eraldajat.

Eeldus: cut töötab korrektselt ainult siis, kui eraldajad on terves failis ühesugused. Seega peame kasutama eelmise tunni sed käsku andmete ettevalmistamiseks.

Tööriistakast (Tools)
cut – Lõikab välja valitud väljad.
Põhisüntaks: ... | cut -d 'ERALDAJA' -f VEERUNUMBER

-d (delimiter) – Mis sümbol eraldab andmeid? (Meil on selleks nüüd ;).
-f (field) – Mitmendat tulpa sa tahad?
Harjutused (Self-Study)
Ülesanne 5.1: Eraldaja määramine Enne kui saame lõigata, peame uuesti jooksutama puhastuse (Part 4).

Võta aluseks käsk: sed 's/,/;/g' kaos.log (see teeb kõik eraldajad semikooloniteks).
Suuna see toruga | käsku cut.
Määra eraldajaks semikoolon (-d ';' või -d';').
Väljasta ainult IP-aadressid (2. veerg).
Ülesanne 5.2: Mitme veeru valimine Meid huvitab, kes tegi ja mida tegi.

Uuri man cut lehte. Kuidas valida mitut veergu korraga (list of fields)?
Modifitseeri eelmist käsku nii, et see väljastaks Kasutaja (3. veerg) ja Tegevuse (4. veerg).
Väljund peaks olema kujul: user_45;LOGIN.
Ülesanne 5.3: "Lõks" (Miks eeltöö on oluline) Proovi kasutada cut käsku otse toorel failil, ilma sed-ita.

Käivita: cut -d';' -f 2 kaos.log | head
Analüüsi: Miks osadel ridadel on IP-aadress ja teistel on mingi suvaline tekst või kuupäev?
Järeldus: cut ei ole tark. Kui real on komad, aga sina otsid semikoolonit, siis cut arvab, et terve rida ongi üks suur esimene veerg.
Ülesanne 5.4: Andmete anonüümimine (Reverse selection) Oletame, et pead saatma logi arendajale, aga IP-aadresse ei tohi näidata (GDPR).

cut oskab valida veerge, aga (sõltuvalt versioonist) on ka valiku "ümberpööramine" võimalik või tuleb valida lihtsalt "kõik muu".
Kõige lollikindlam viis: Vali kõik veerud, välja arvatud teine.
Kirjuta käsk, mis väljastab veerud 1, 3, 4, 5 ja 6 (jäta 2 vahele).
6. OSA: Statistika ja Järjestamine (sort, uniq)
Teema: Kellel on kõige rohkem logisissekandeid? Kes on "Top 1" kasutaja?

Andmed on logifailis tavaliselt kronoloogilises järjekorras (ajalises). Analüüsi jaoks on aga vaja grupeerida andmeid sisu järgi. Siin on Linuxi käsurea üks suurimaid lõkse: uniq käsk töötab korrektselt AINULT siis, kui sisend on eelnevalt sorteeritud.

Tööriistakast (Tools)
sort – Järjestab tekstiread (vaikimisi tähestikulises järjekorras).
uniq – Eemaldab või loendab külgnevaid korduvaid ridu.
Kuldne reegel: Kasuta alati kombinatsiooni sort | uniq.
Harjutused (Self-Study)
Ülesanne 6.1: Lihtne nimekiri Me tahame näha nimekirja kõikidest kasutajatest, kes on logis figureerinud, tähestikulises järjekorras.

Kasuta eelmistest osadest tuttavat puhastust (sed) ja lõikamist (cut), et eraldada ainult Kasutaja veerg (3. veerg).
Suuna tulemus torusse | sort.
Tulemus peaks olema pikk nimekiri nimedest, mis on järjestatud A-st Z-ni (kordustega).
Ülesanne 6.2: Unikaalsed kasutajad Nüüd eemaldame kordused.

Lisa eelmise käsu lõppu | uniq.
Nüüd peaksid nägema igat kasutajanime ainult üks kord.
Ülesanne 6.3: Sagedustabel (Toplist) See on sysadmini igapäevatöö: "Milline IP ründab kõige rohkem?" või "Kes teeb kõige rohkem päringuid?".

Uuri man uniq lehte. Otsi lippu, mis loendab kordusi (count). See paneb iga rea ette numbri.
Lisa see lipp oma käsule: ... | sort | uniq -c.
Probleem: Nüüd on sul nimekiri kujul "7 user_01", aga see on sorteeritud nime, mitte numbri järgi.
Ülesanne 6.4: Lõplik edetabel Et saada tõeline TOP, peame tulemuse uuesti sorteerima, seekord numbri järgi.

Võta Ülesande 6.3 tulemus.
Lisa lõppu veel üks | sort.
Uuri man sort.

Vaikimisi on sortimine tekstipõhine (10 on väiksem kui 2, sest '1' tuleb enne '2').
Leia lipp numeric sort (-n).
Leia lipp reverse (-r), et suurim number oleks üleval.
Tulemus peaks näitama kõige aktiivsemat kasutajat kõige esimesena.
Ülesanne 6.5: Numbriline sortimine (Ettevalmistus järgmiseks) Sorteeri esialgne kaos.log fail (peale puhastust) selle järgi, kui suur oli andmemaht (5. veerg, BYTES).

Siin pead kasutama sort käsku lipuga -k (key), et öelda talle: "Sorteeri 5. veeru järgi".
Proovi: sort -t';' -k5 -n

-t';' määrab eraldajaks semikooloni.
-k5 määrab veeru.
-n lülitab sisse numbrilise režiimi.
7. OSA: Tähemärkide Maagia (tr)
Teema: Arvuti on loll. Tema jaoks on "admin", "Admin" ja "ADMIN" kolm täiesti erinevat isikut.

Analüütikas on see katastroof. Kui sa teed kokkuvõtet, tahad sa, et need kõik loetaks üheks. tr (translate) on lihtne, kuid ülikiire tööriist tähemärkide asendamiseks, kustutamiseks või kokkusurumiseks.

Oluline erinevus: Erinevalt varasematest käskudest (grep, cut, sort), ei oska tr tavaliselt failinime argumendina võtta. Ta töötab ainult toru (|) kaudu vastu võetud andmetega.

Tööriistakast (Tools)
tr – Teisendab või kustutab tähemärke.
Põhisüntaks: ... | tr 'VANA_SET' 'UUS_SET'
Klassid: [:lower:] (väiketähed), [:upper:] (suurtähed), [:digit:] (numbrid).
Harjutused (Self-Study)
Ülesanne 7.1: Karjumise režiim (Case Conversion) Et vältida segadust suurte ja väikeste tähtedega, on kindlaim viis kõik suurtähtedeks teisendada.

Võta andmevoog (kasuta eelmist sed puhastust).
Uuri man tr.
Kirjuta käsk, mis muudab kõik tähed suurtähtedeks.

Variant A (Klassikaline): tr 'a-z' 'A-Z'
Variant B (Proff): tr '[:lower:]' '[:upper:]'
Tulemus peaks olema: 2024-10-01;192.168.1.55;USER_45;LOGIN...
Ülesanne 7.2: Tühikute "pressimine" (Squeeze) Meie logifailis on read, kus on palju tühikuid järjest: USER_01 ; UPLOAD. Eelmises osas kasutasime sed-i, et tühikud kustutada. Aga mis siis, kui me tahame, et sõnade vahele jääks üks tühik, aga mitu järjestikust kaoks?

Uuri man tr ja otsi lippu -s (squeeze-repeats).
Kirjuta käsk: echo "Tere maailm siin on tühikud" | tr -s ' '
Rakenda seda teadmist logifaili peal (kuigi meil on semikoolonid, on see kasulik oskus tekstifailide korrastamisel).
Ülesanne 7.3: Kustutamine (tr vs sed) tr on üksikute märkide kustutamisel kiirem kui sed. Me tahame eemaldada kõik semikoolonid ;, et näha, mis juhtub (tekib üks pikk joru).

Uuri man tr ja otsi lippu -d (delete).
Kirjuta käsk, mis kustutab voost kõik semikoolonid.
Ülesanne 7.4: Praktiline stsenaarium Oletame, et tahame teha statistikat tegevuste (4. veerg: LOGIN, UPLOAD jne) kohta. Kuna andmed on "mustad", võib seal olla Login, LOGIN või login.

Koosta toru (pipeline):

Puhasta andmed (sed).
Lõika välja tegevuse veerg (cut).
Muuda kõik suurtähtedeks (tr), et Login ja LOGIN muutuksid samaks.
Sorteeri ja loenda (sort | uniq -c).
Sorteeri tulemus sageduse järgi (sort -nr).
Siin on kursuse 8. Osa.

8. OSA: Programmeeritav Tank (awk) – I
Teema: Kui cut jääb nõrgaks ja grep liiga lihtsaks.

awk ei ole lihtsalt käsk. See on terve programmeerimiskeel, mis elab sinu terminalis. Kui cut oskab ainult lõigata ja grep ainult otsida, siis awk oskab:

Lugeda rida.
Saada aru numbritest (teha matemaatikat).
Tegutseda tingimuste alusel ("kui 5. veerg on suurem kui 100...").
Süntaks on hirmutav, aga võimas: awk -F'eraldaja' 'tingimus { tegevus }'

Tööriistakast (Tools)
awk – Mustrite skaneerimise ja töötlemise keel.
Muutujad:

$1, $2, $3... – Veergude numbrid.
$0 – Terve rida.
NR – Number of Records (mitmes rida parasjagu käsil on).
NF – Number of Fields (mitu veergu real kokku on).
Harjutused (Self-Study)
Ülesanne 8.1: cut asendamine awk suudab teha kõike, mida cut teeb, aga paremini.

Kasuta meie tavalist puhastustoru (sed-id).
Suuna see awk-i. Määra eraldajaks semikoolon: -F';'.
Käsk: {print $2, $3}.
Pane tähele: awk paneb nende vahele tühiku automaatselt.
Ülesanne 8.2: Ilus väljund (String concatenation) Me tahame raportit, mis näeb välja inimlik. Mitte 192.168.1.1 user_01, vaid Kasutaja user_01 tuli aadressilt 192.168.1.1.

awk sees jutumärkides olev tekst trükitakse otse.
Proovi: awk -F';' '{print "Kasutaja " $3 " tuli aadressilt " $2}'
Rakenda seda oma andmevoole.
Ülesanne 8.3: Tark filtreerimine (Boolean logic) grep suudab leida sõna "SUCCESS". Aga mis siis, kui mõne kasutaja nimi on "SUCCESS_MAN"? grep leiaks ka tema. awk on täpsem – ta vaatab ainult 6. veergu.

Süntaks tingimuse seadmiseks on tegevuse ees.
Käsk: awk -F';' '$6 == "SUCCESS" {print $0}'

$6 == "SUCCESS" on tingimus.
{print $0} (trüki terve rida) on tegevus.
Kirjuta käsk, mis leiab read, kus tegevus (4. veerg) on täpselt "LOGIN".
Ülesanne 8.4: Matemaatiline filter (Supervõime) See on koht, kus grep hätta jääb. Meid huvitavad ainult suured andmeedastused.

Veerg 5 on BYTES.
Kirjuta awk käsk, mis väljastab terve rea ainult siis, kui baitide hulk ($5) on suurem kui 2000.
Vihje: awk -F';' '$5 > 2000 {print $0}'
Ülesanne 8.5: Kombineeritud rünnak Paneme kõik kokku.

Leia logist read, kus:

Status ($6) on "SUCCESS".
JA (&&) andmemaht ($5) on suurem kui 1000.
Väljasta ainult lause: "Suur tehing: [BAIDID] baiti - [KASUTAJA]"
Struktuur: awk -F';' '$6 == "SUCCESS" && $5 > 1000 { ... }'
9. OSA: Programmeeritav Tank (awk) – II (Exceli Tapja)
Teema: Arvutamine, summeerimine ja koondtabelid.

Siiani oleme ridu töödelnud ükshaaval. Aga ülemus ei taha teada, mis juhtus igal sekundil. Ta tahab teada "Kui palju me kokku andmemahtu kasutasime?" või "Mis oli keskmine üleslaadimise suurus?".

Siin asendab awk Excelit. Et seda teha, peame mõistma awk kolme faasi:

BEGIN – Tee seda üks kord enne faili lugemist (nt "Määra summa nulliks").
MAIN (Keskel) – Tee seda iga reaga (nt "Liida rea number summale otsa").
END – Tee seda üks kord lõpus (nt "Trüki lõppsumma").
Tööriistakast (Tools)
awk struktuur: 'BEGIN {algus} {tsükkel} END {lõpp}'
Operaatorid: += (liida juurde), / (jagamine), * (korrutamine).
Harjutused (Self-Study)
Ülesanne 9.1: Lihtne summeerimine (SUM) Meil on vaja kokku liita 5. veerg (BYTES).

Mõttekäik: Iga rea puhul võtame $5 ja lisame selle muutujale nimega total. Kui fail on läbi, trükime total välja.
Süntaks: awk -F';' '{total += $5} END {print "Kogumaht:", total}'
Rakenda seda oma puhastatud andmevoole (sed-id ees).
Ülesanne 9.2: Tingimuslik summa (SUMIF) Me tahame teada ainult UPLOAD (üleslaadimine) mahtu.

Siin peame põhitsüklisse lisama if-lause.
Struktuur: { if ($4 == "UPLOAD") { upload_sum += $5 } }
Ülesanne: Koosta käsk, mis arvutab kokku ainult nende ridade baidid, kus tegevus on "UPLOAD", ja väljastab lõpus tulemuse.
Ülesanne 9.3: Ridade loendamine ja Keskmine (AVERAGE) Excelis on funktsioon =AVERAGE(). awk-is peame selle ise tegema: Summa / Kogus.

awk-is on sisseehitatud muutuja NR (Number of Records), mis loeb ridu, aga see loeb kõiki ridu. Kui me kasutame filtrit (nt ainult UPLOAD), peame tegema oma loenduri.
Loogika:

Awk

if ($4 == "UPLOAD") {
    sum += $5;   # Liida baidid
    count++;     # Suurenda loendurit 1 võrra
}
END plokis: Pead tegema jagamistehte: print sum / count.
Ülesanne: Arvuta keskmine baidi suurus kõikidele "UPLOAD" tegevustele.
Ülesanne 9.4: Ühikute teisendamine Baitide arv (nt 5493021) on inimestele raskesti loetav. Teeme sellest megabaidid (MB).

1 MB = 1024 * 1024 baiti (või lihtsustatult 1 000 000).
awk oskab teha tehteid trükkimise ajal.
Ülesanne: Võta Ülesande 9.1 lahendus ja muuda END plokki nii, et see jagaks summa 1048576-ga (1024*1024) ja trükiks vastuse "MB".
Näide: END { print total / 1048576 " MB" }
Ülesanne 9.5: Kasutajapõhine statistika (Massiivid) See on awk-i tõeline jõud. Me tahame teada mahtu iga kasutaja kohta eraldi, ilma et peaksime käsku 10 korda muutma. Kasutame massiivi (array), kus indeksiks on kasutajanimi.

Kood: { user_map[$3] += $5 }

See tähendab: "Võta sahtel, mille peale on kirjutatud kasutajanimi ($3), ja viska sinna baidid ($5) juurde."
END plokk: Nüüd peame kasutama tsüklit, et kõik sahtlid ette lugeda.

END { for (u in user_map) print u, user_map[u] }
10. OSA: Final lol – Automatiseerimine ja Arhiveerimine
Teema: Kuidas panna kõik õpitu ühte faili ja pakkida kohvrid.

Sa oskad nüüd käsureal teha maagiat. Sa suudad sekundi murdosaga analüüsida gigabaite andmeid. Aga on üks probleem: kui sa terminali kinni paned, kaob su geniaalne käsk ajalukku.

Tõeline süsteemiadministraator ei kirjuta iga päev samu käske uuesti. Ta kirjutab skripte. Selles viimases osas muudame sinu "pika vorsti" (pipeline) püsivaks programmiks ja pakime tulemused kokku.

Tööriistakast (Tools)
nano (Erandkorras lubatud!) – Skripti kirjutamiseks.
chmod (Change Mode) – Faili õiguste muutmine (et skript oleks käivitatav).
> ja >> – Väljundi suunamine faili.
tar (Tape Archive) – Failide pakkimine.
Harjutused (Self-Study)
Ülesanne 10.1: Sinu esimene Bash-skript Me tahame luua programmi raport_generaator.sh.

Ava tekstiredaktor: nano raport_generaator.sh.
Kirjuta faili esimene rida (Shebang): #!/bin/bash. See ütleb Linuxile, et tegu on skriptiga.
Lisa sinna alla oma 9.5 ülesande (või kõige keerulisem, mis sul on) lahendus.
Tähtis muudatus: Selle asemel, et tulemus jookseks ekraanile, lisa käsu lõppu > raport.txt. See salvestab tulemuse faili.
Salvesta ja välju (Ctrl+O, Enter, Ctrl+X).
Ülesanne 10.2: Käivitusõigus (chmod) Kui proovid praegu kirjutada ./raport_generaator.sh, ütleb Linux "Permission denied". Turvalisuse pärast ei ole tekstifailid vaikimisi programmid.

Uuri man chmod.
Meil on vaja lisada eXecute (käivitamise) õigus.
Kirjuta käsk, mis annab failile raport_generaator.sh käivitusõiguse. (Tavaliselt chmod +x ...).
Nüüd käivita skript: ./raport_generaator.sh. Kontrolli, kas tekkis fail raport.txt.
Ülesanne 10.3: Logide kokkupakkimine (tar) Töö on tehtud. Logifail kaos.log on vana ja võtab ruumi. Me ei taha seda kustutada, vaid arhiveerida.

Uuri man tar.
Me tahame luua arhiivi (-c), mis on pakitud gzip-iga (-z) ja kirjutatakse faili (-f).
Arhiivi nimi olgu logid_arhiiv.tar.gz.
Arhiivi peab minema fail kaos.log.
Kirjuta käsk ja käivita see.
Ülesanne 10.4: Koristustöö Nüüd on sul olemas logid_arhiiv.tar.gz. Algset kaos.log faili pole enam vaja.

Kustuta algne logifail käsklusega rm.
Lõpueksam: "Musta Kasti" Raport
Sinu ülemus astub uksest sisse. "Mul on vaja kohe teada saada meie TOP 3 kasutajat, kes tekitasid eelmisel kuul kõige rohkem liiklust. Ja saada see mulle pakitud kujul!"

Sinu ülesanne on esitada mulle (õpetajale) täpne käskude jada:

Kuidas sa käivitad oma skripti, et tekiks raport.txt?
Kuidas sa vaatad faili raport.txt sisu, et veenduda, et seal on andmed?
Kuidas sa pakid kokku failid raport.txt ja raport_generaator.sh üheks failiks nimega final_result.tar.gz?

# 1
<img width="601" height="393" alt="image" src="https://github.com/user-attachments/assets/7aa62afb-4903-4db0-9cb3-a3ec258eb0ce" />

# 1.1
<img width="593" height="176" alt="image" src="https://github.com/user-attachments/assets/2a4f739d-6944-46af-9a38-b49eb5afdcdb" />

# 1.2
<img width="581" height="194" alt="image" src="https://github.com/user-attachments/assets/0d633d78-b5af-49c6-9160-5b51cc58cf0b" />

# 1.3
<img width="582" height="186" alt="image" src="https://github.com/user-attachments/assets/4b60a6fa-f57c-4461-b02b-18efcea3bc46" />

# 1.4
<img width="599" height="422" alt="image" src="https://github.com/user-attachments/assets/47e5f0c8-115d-4881-96a1-c2fd96ba0796" />

# 2.1
<img width="431" height="49" alt="image" src="https://github.com/user-attachments/assets/4c1b5c52-6a79-4571-821e-85ae64d2f53d" />
# 2.2
<img width="441" height="55" alt="image" src="https://github.com/user-attachments/assets/7425ef36-6fa0-41d4-b380-208fe922b8be" />
# 2.3
<img width="553" height="63" alt="image" src="https://github.com/user-attachments/assets/f7bf4d17-61c9-4900-8fc1-be24f125fc52" />
# 2.4
<img width="508" height="53" alt="image" src="https://github.com/user-attachments/assets/b8ca5221-7b37-4527-84aa-2db6da94271f" />
# 3.1
<img width="573" height="100" alt="image" src="https://github.com/user-attachments/assets/f5725bc3-0a7b-44ed-9da5-334400687359" />
# 3.2
<img width="580" height="310" alt="image" src="https://github.com/user-attachments/assets/fce5a088-1ea6-47a6-9cb5-54a99a45f4e4" />
# 3.3
<img width="560" height="60" alt="image" src="https://github.com/user-attachments/assets/2f4fa229-31dd-43a7-8a99-45daeadcf525" />
# 3.4
<img width="579" height="359" alt="image" src="https://github.com/user-attachments/assets/242453d9-70b6-4446-99dc-e7d438200065" />
# 4.1
<img width="580" height="343" alt="image" src="https://github.com/user-attachments/assets/829697fd-6075-410f-83d3-01b35accca85" />
# 4.2
<img width="593" height="238" alt="image" src="https://github.com/user-attachments/assets/3646ae1b-bc81-4b87-84f2-8ab4b1592623" />
# 4.3
<img width="576" height="349" alt="image" src="https://github.com/user-attachments/assets/ce6b8645-39b4-47de-ac6d-6b612fbcad49" />
# 4.4
<img width="588" height="92" alt="image" src="https://github.com/user-attachments/assets/80ae4f96-9840-44ed-af2a-37bdf3c0e132" />
# 5.1
<img width="610" height="487" alt="image" src="https://github.com/user-attachments/assets/3e05c6da-2ca8-4fc0-b2d9-5b12291e153e" />
# 5.2
<img width="574" height="305" alt="image" src="https://github.com/user-attachments/assets/d962d70c-dacc-4bec-bc18-7c3de74da067" />
# 5.3
<img width="578" height="192" alt="image" src="https://github.com/user-attachments/assets/0a378dd1-61b8-42f8-96b2-57d5a0cbbd6a" />
# 5.4
<img width="601" height="271" alt="image" src="https://github.com/user-attachments/assets/73abc506-84ed-485e-837f-b34bc21be32a" />
# 6.1
<img width="567" height="429" alt="image" src="https://github.com/user-attachments/assets/ae5ddad2-f3d8-4436-aa22-1534d495a7d7" />
# 6.2
<img width="586" height="433" alt="image" src="https://github.com/user-attachments/assets/2c5d093c-27e3-4410-89f8-6ba0867e63cd" />
# 6.3
<img width="591" height="393" alt="image" src="https://github.com/user-attachments/assets/cc9e9ad7-89bb-4e13-9689-a689e958c99a" />
# 6.4
<img width="591" height="349" alt="image" src="https://github.com/user-attachments/assets/b173601e-9971-4957-9d19-22f2a3e37ea7" />
# 6.5
<img width="581" height="155" alt="image" src="https://github.com/user-attachments/assets/e48b9dc8-acb2-40a1-92ff-10b87fdbc15f" />
# 7.1
<img width="575" height="248" alt="image" src="https://github.com/user-attachments/assets/067692d4-d2a0-4150-af32-c366843ea04a" />
# 7.2
<img width="587" height="53" alt="image" src="https://github.com/user-attachments/assets/9afb96fe-e4e5-482b-b005-5e3b42314c12" />
# 7.3
<img width="571" height="319" alt="image" src="https://github.com/user-attachments/assets/0ea481b9-62b1-4d97-93b8-01caba1e286a" />
# 7.4
<img width="573" height="225" alt="image" src="https://github.com/user-attachments/assets/202ddc3a-8926-4e30-af5b-3d29838966ff" />
# 8.1
<img width="567" height="232" alt="image" src="https://github.com/user-attachments/assets/8dcbc9b3-5436-4ada-b213-bc8162cd9582" />
# 8.2
<img width="571" height="267" alt="image" src="https://github.com/user-attachments/assets/43f0b62e-5a07-4a1b-967f-70addd24274f" />
# 8.3
<img width="569" height="89" alt="image" src="https://github.com/user-attachments/assets/fe93f45f-a2d8-4789-a073-dd01347dcb37" />
# 8.4
<img width="568" height="227" alt="image" src="https://github.com/user-attachments/assets/9c35afea-820b-43fd-8552-a24a26e1be52" />
# 8.5
<img width="571" height="250" alt="image" src="https://github.com/user-attachments/assets/ed0f5ec8-d22f-42a8-877e-e6839745ee9f" />
# 9.1
<img width="575" height="41" alt="image" src="https://github.com/user-attachments/assets/fc4ad7db-c444-4630-b0e7-1a00fc5d05ed" />
# 9.2
<img width="568" height="43" alt="image" src="https://github.com/user-attachments/assets/1fae86d9-1dff-47eb-bad4-9fd426b7b173" />
# 9.3
<img width="569" height="143" alt="image" src="https://github.com/user-attachments/assets/2feb7426-afc2-4ef5-b019-4b377d63fbc9" />
# 9.4
<img width="565" height="36" alt="image" src="https://github.com/user-attachments/assets/3b27fb88-0b1e-4842-b87c-d3daa81c5f42" />
# 9.5 
<img width="815" height="313" alt="image" src="https://github.com/user-attachments/assets/7356c562-8611-42bb-8c5e-dfb70c02c57f" />
# 10.1
<img width="825" height="420" alt="image" src="https://github.com/user-attachments/assets/2bdf8b53-d098-492b-8d80-8b36e04fc334" />
# 10.2
<img width="814" height="418" alt="image" src="https://github.com/user-attachments/assets/7fa17a50-65ed-43f6-aee6-505f436d7817" />
# 10.3
<img width="734" height="57" alt="image" src="https://github.com/user-attachments/assets/7b461c10-25b8-4c4e-a4b0-304e6e53446f" />
<img width="826" height="225" alt="image" src="https://github.com/user-attachments/assets/5922c44d-fd1b-4dc6-b51f-bc11b3334a3e" />
# 10.4
<img width="824" height="155" alt="image" src="https://github.com/user-attachments/assets/77a51e65-17fa-4332-927e-6a137d552ae2" />
# lopp
<img width="856" height="168" alt="image" src="https://github.com/user-attachments/assets/64abb37a-a155-49fd-aab0-993bb983b5cc" />
