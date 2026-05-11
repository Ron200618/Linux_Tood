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
<img width="568" height="381" alt="Screenshot 2026-05-11 at 04 44 25" src="https://github.com/user-attachments/assets/d2df31cb-f6ef-478f-b0c9-117e3164d412" />


# 1.1
<img width="568" height="164" alt="Screenshot 2026-05-11 at 04 44 34" src="https://github.com/user-attachments/assets/a382be86-e2e2-4340-a17c-bb3075139023" />


# 1.2
<img width="568" height="179" alt="Screenshot 2026-05-11 at 04 44 40" src="https://github.com/user-attachments/assets/767e6a60-3c6a-4362-a366-6fb71d03d34b" />


# 1.3
<img width="568" height="179" alt="Screenshot 2026-05-11 at 04 44 45" src="https://github.com/user-attachments/assets/db7be45b-cf32-4795-80f6-4e124f07744b" />


# 1.4
<img width="568" height="411" alt="Screenshot 2026-05-11 at 04 44 57" src="https://github.com/user-attachments/assets/310ab215-8d36-4eab-91e0-5af303f898d1" />


# 2.1
<img width="426" height="53" alt="Screenshot 2026-05-11 at 04 45 11" src="https://github.com/user-attachments/assets/be9bad15-91ba-4616-8166-fc1d2f3cdcad" />


# 2.2
<img width="426" height="53" alt="Screenshot 2026-05-11 at 04 45 17" src="https://github.com/user-attachments/assets/27f1c16c-0466-445f-bd65-323b81353618" />


# 2.3
<img width="447" height="53" alt="Screenshot 2026-05-11 at 04 45 23" src="https://github.com/user-attachments/assets/bd3be286-d4a6-4531-ae96-acd718cca0f8" />


# 2.4
<img width="464" height="53" alt="Screenshot 2026-05-11 at 04 45 28" src="https://github.com/user-attachments/assets/8042e9a4-8719-428c-bd60-b84fe3f2543d" />


# 3.1
<img width="511" height="93" alt="Screenshot 2026-05-11 at 04 45 37" src="https://github.com/user-attachments/assets/7b509651-07ae-4b37-852e-547486aeb069" />


# 3.2
<img width="567" height="303" alt="Screenshot 2026-05-11 at 04 45 47" src="https://github.com/user-attachments/assets/4213a5d5-16e9-4719-a92d-4ad8c4c396d4" />


# 3.3
<img width="548" height="53" alt="Screenshot 2026-05-11 at 04 45 56" src="https://github.com/user-attachments/assets/3c8d91ce-3de8-446a-9879-62cb47b12188" />


# 3.4
<img width="548" height="344" alt="Screenshot 2026-05-11 at 04 46 08" src="https://github.com/user-attachments/assets/b47718c8-c017-4d02-aa02-64945bcbae20" />

# 4.1
<img width="548" height="337" alt="Screenshot 2026-05-11 at 04 46 17" src="https://github.com/user-attachments/assets/fc5885a3-72cf-45c9-b345-ab16a227e157" />


# 4.2
<img width="548" height="225" alt="Screenshot 2026-05-11 at 04 46 25" src="https://github.com/user-attachments/assets/0c702bfe-4832-4cc3-a1a2-6b8f799a1a28" />

# 4.3
<img width="562" height="339" alt="Screenshot 2026-05-11 at 04 46 35" src="https://github.com/user-attachments/assets/e429f9a9-e1b7-4b7c-904e-d5fb743e31bd" />


# 4.4
<img width="562" height="80" alt="Screenshot 2026-05-11 at 04 46 41" src="https://github.com/user-attachments/assets/f8ee241e-ccd4-44eb-a3bc-e3e7eb1f68b5" />


# 5.1
<img width="562" height="474" alt="Screenshot 2026-05-11 at 04 46 51" src="https://github.com/user-attachments/assets/97546d91-0b87-4f4c-b0aa-c8afde647e04" />


# 5.2
<img width="562" height="299" alt="Screenshot 2026-05-11 at 04 47 01" src="https://github.com/user-attachments/assets/3fe91e44-d0a4-461d-8232-977857c5a413" />

# 5.3
<img width="562" height="182" alt="Screenshot 2026-05-11 at 04 47 10" src="https://github.com/user-attachments/assets/b40f84ac-5bff-4f19-977d-559214382250" />


# 5.4
<img width="562" height="261" alt="Screenshot 2026-05-11 at 04 47 19" src="https://github.com/user-attachments/assets/cfad5058-2a4b-43d2-b54f-aca7de3e8688" />


# 6.1
<img width="562" height="404" alt="Screenshot 2026-05-11 at 04 47 26" src="https://github.com/user-attachments/assets/faa39772-4a55-4c05-a954-cf601490eb6a" />


# 6.2
<img width="562" height="422" alt="Screenshot 2026-05-11 at 04 47 32" src="https://github.com/user-attachments/assets/1f8dbc5c-0509-473a-82d8-3f93c25c35c2" />


# 6.3
<img width="562" height="380" alt="Screenshot 2026-05-11 at 04 47 39" src="https://github.com/user-attachments/assets/580cc9be-ca56-4fe8-98f0-8fd5d6713bc3" />


# 6.4
<img width="562" height="341" alt="Screenshot 2026-05-11 at 04 47 46" src="https://github.com/user-attachments/assets/3fe5f0eb-2bd4-48d4-95f4-565da04aaea3" />


# 6.5
<img width="562" height="150" alt="Screenshot 2026-05-11 at 04 47 53" src="https://github.com/user-attachments/assets/5343748d-aab7-4ff5-be88-47a4afaf9c0c" />


# 7.1
<img width="562" height="242" alt="Screenshot 2026-05-11 at 04 48 02" src="https://github.com/user-attachments/assets/49af4d54-8f5c-4065-a041-3765d0e59277" />


# 7.2
<img width="562" height="39" alt="Screenshot 2026-05-11 at 04 48 07" src="https://github.com/user-attachments/assets/8c723974-2411-47c8-9e8a-1f0f928badab" />


# 7.3
<img width="562" height="314" alt="Screenshot 2026-05-11 at 04 48 17" src="https://github.com/user-attachments/assets/54dcac98-5f9f-4040-bff4-46a5ace4041a" />


# 7.4
<img width="562" height="209" alt="Screenshot 2026-05-11 at 04 48 22" src="https://github.com/user-attachments/assets/5c8f0bbf-71f4-4db7-97bc-4a17cd91f7a9" />


# 8.1
<img width="562" height="209" alt="Screenshot 2026-05-11 at 04 48 27" src="https://github.com/user-attachments/assets/58c57de0-0e72-4812-b932-0b1f7c7b8f11" />


# 8.2
<img width="562" height="258" alt="Screenshot 2026-05-11 at 04 48 33" src="https://github.com/user-attachments/assets/6d1443f9-a8d5-4ef5-8df4-1545a8caa71b" />


# 8.3
<img width="562" height="90" alt="Screenshot 2026-05-11 at 04 48 41" src="https://github.com/user-attachments/assets/da33eb97-72b0-488c-8145-c37f1854d618" />


# 8.4
<img width="562" height="224" alt="Screenshot 2026-05-11 at 04 48 50" src="https://github.com/user-attachments/assets/4d608ad4-a1f1-4e36-aca7-a4904123f149" />


# 8.5
<img width="562" height="245" alt="Screenshot 2026-05-11 at 04 48 56" src="https://github.com/user-attachments/assets/149877c9-6e5b-49eb-ae8b-99d2bc80a69f" />


# 9.1
<img width="562" height="33" alt="Screenshot 2026-05-11 at 04 49 05" src="https://github.com/user-attachments/assets/1d92404d-057e-4b64-b747-25707288e548" />


# 9.2
<img width="562" height="37" alt="Screenshot 2026-05-11 at 04 49 12" src="https://github.com/user-attachments/assets/e7487ede-6d47-447d-9c88-ccc0fa81cd27" />


# 9.3
<img width="509" height="136" alt="Screenshot 2026-05-11 at 04 49 21" src="https://github.com/user-attachments/assets/f4f890b5-7cb8-4b8c-8dd1-f1de40a35c8e" />


# 9.4
<img width="518" height="30" alt="Screenshot 2026-05-11 at 04 49 29" src="https://github.com/user-attachments/assets/7d56cfce-0fac-47c5-bd81-01527aefa0c4" />


# 9.5 
<img width="559" height="214" alt="Screenshot 2026-05-11 at 04 49 42" src="https://github.com/user-attachments/assets/b571ed72-6445-4dfe-a4b8-ad3572ae78f3" />


# 10.1
<img width="559" height="291" alt="Screenshot 2026-05-11 at 04 49 51" src="https://github.com/user-attachments/assets/cb44692d-1b9c-452f-b6a1-e720b838be4f" />


# 10.2
<img width="559" height="285" alt="Screenshot 2026-05-11 at 04 49 59" src="https://github.com/user-attachments/assets/fe9af04c-efbc-4ba2-b370-cdc0e5e688ef" />


# 10.3
<img width="559" height="37" alt="Screenshot 2026-05-11 at 04 50 05" src="https://github.com/user-attachments/assets/bf8beeea-184f-4a49-9f4f-6ed240cc5f19" />

<img width="559" height="155" alt="10 3(2)" src="https://github.com/user-attachments/assets/0014d21a-c13d-40b0-812f-cd456a24eb6d" />



# 10.4
<img width="564" height="103" alt="Screenshot 2026-05-11 at 04 51 07" src="https://github.com/user-attachments/assets/ac27da2a-8a69-406a-b64b-5413a14a6fb7" />


# lopp
<img width="564" height="100" alt="Screenshot 2026-05-11 at 04 51 20" src="https://github.com/user-attachments/assets/c3d6b404-3cd5-4a42-a90e-228fe6628aab" />

