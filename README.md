# Eesti.ee tegevuslubade ja majandustegevusteadete ärisündmusteenusega liidestumine.
Selle dokumentatsiooniga käib kokku Tegevuslubade ja majandustegevusteadete sündmusteenuse REST teenuse OpenAPI spetsifikatsioon YAML failina. 
YAML faili saab kasutada sisendina X-Tee REST teenuse loomisel.
[YAML file](tegevusload-yaml-schema-v5.yaml)

## Sisukord
[Versioonid](#versioonid) <br>
[1. Sissejuhatus](#1-sissejuhatus)<br>
[2. Sõnastik](#2-sõnastik)<br>
[3. Äriloogika](#3-äriloogika)<br>
[4. Liidestumine](#4-liidestumine)<br>
    &nbsp; [4.1 Nõuded tehnoloogiatele](#41-nõuded-tehnoloogiatele)<br>
    &nbsp; [4.2 HTTP koodid](#42-http-koodid)<br>
    &nbsp; [4.3 API visualiseerimiseks](#43-api-visualiseerimiseks)<br>
    &nbsp; [4.4 Päringu URL-i kuju](#44-päringu-url-i-kuju)<br>
    &nbsp; [4.5 Päised](#45-päised)<br>
    &nbsp; [4.6 Päringu vastuse kuju](#46-päringu-vastuse-kuju)<br>
[5. Andmemudel](#5-andmemudel)<br>
    &nbsp; [5.1 Kohustuslikkus](#51-kohustuslikkus)<br>
    &nbsp; [5.2 Andmekoosseis](#52-andmekoosseis)<br>
    &nbsp; [5.3 Süvalink](#53-süvalink)<br>
[6. Andmemudeli näited JSON kujul](#6-andmemudeli-näited-json-kujul)<br>
    &nbsp; [6.1 Näide 1: TTJA MTR-i taksoveo sõidukikaardi tegevusluba](#61-näide-1-ttja-mtr-i-taksoveo-sõidukikaardi-tegevusluba)<br>
    &nbsp; [6.2 Näide 2: TTJA MTR-i toitlustuse majandustegevusteade](#62näide-2-ttja-mtr-i-toitlustuse-majandustegevusteade)<br>
    &nbsp; [6.3 Näide 3: Terviseameti haigla tegevusluba](#63-näide-3-terviseameti-haigla-tegevusluba)<br>



## Versioonid

Küsimuste korral, täiendava teabe või abi saamiseks palume pöörduda kasutajatoe poole [help@ria.ee](mailto:help@ria.ee).
Dokumendi esimesed 5 versiooni jagati eesti.ee tegevuslubade ja majandustegevusteadete sündmusteenuse liidestujatele PDF dokumendi kujul. Githubi on lisatud versioon 5 alates.

| Versioon | Muutja | Muutmise aeg | Kommentaar | 
|---------|------|-----------|-------------------|
|1.0|Hanna-Liisa Vilbiks (Helmes) Marten Tall (Helmes)|10.06.2024|Dokumendi esimene versioon, mida jagada liidestuvatele osapooltele|
|2.0|Hanna-Liisa Vilbiks (Helmes) Marten Tall (Helmes)|17.07.2024|REST päringu formaadi ja kuju täpsustamine. _*NB!*_ YAML-i versioon on kuni REST teenuse live’ni 1.0.0.|
|3.0|Marten Tall (Helmes)|11.10.2024|X-Tee päiste lisamine päringu tegija identifitseerimiseks. Accept päiste lisamine. REST päringu GET parameetri nimetuse parandus.|
|4.0|Marten Tall (Helmes)|21.10.2024|Mitte-kohustusliku X-Tee päise X-Road-Issue lisamine. HTTP koodi 403 Forbidden lisamine.|
|5.0|Jens-Kristjan Liivand (Helmes)|13.11.2024|REST päringu vastuse kuju defineerimine ja täpsustus|


## 1 Sissejuhatus

See dokument annab ülevaate eesti.ee’s paikneva uue ärisündmusteenuse „Ettevõtja tegevusload ja majandustegevusteated“ andmemudelist ja äriloogikast. Dokument on mõeldud eelkõige eesti.ee tegevuslubade ja majandustegevusteadete ärisündmusteenusega liidestuvatele osapooltele. Lisaks sellele dokumendile on arendustööde sisendiks REST teenuse OpenAPI 3.0 spetsifikatsioon YAML faili kujul.


## 2 Sõnastik

| Mõiste | Selgitus |
| -- | -- |
| Tegevusluba (TL)lühend 'luba' | Tegevusluba on ametlik dokument, mis annab õigusemajandustegevuseks erinõuetega reguleeritud valdkondades, nagunäiteks toitlustus, meditsiin jm valdkonnad. Tegevusloa taotleminehölmab mitmetele erinevatele nõuetele ja regulatsioonidele vastamist.Üldjuhul tuleb tegevusloa taotlusega esitada lisadokumente. |
| Majandustegevusteade (MTT)lühend 'teade' | Majandustegevusega alustamisest teavitamine on kohustuslik kuisoovitakse alustadaettevõtlusega seotud tegevust teatudteavitamiskohustusega valdkondades.Majandustegevusteade sisaldabinformatsiooni ettevõtte sisulise tegevuse ja tegutsemiskoha kohta.Üldjuhul ei ole lisadokumente vaja esitada. |
| Sündmusteenus | Sündmusteenus on otsene avalik teenus, mida mitu asutust osutabühiselt,et inimene saaks täita köikkohustused ja kasutada köiki öigusi,mis talle tekivad ühe sündmuse või olukorra tõttu. Sündmusteenuskoondab mitu sama sündmusega seotud kasutajale üheks teenuseks.Sündmusteenuste eesmärk on muuta avalike teenuste kasutamineinimese jaoks võimalikult lihtsaks ja mugavaks. |
| Liidestuv osapool | Liidestumine ja andmevahetus toimub selles kontekstis eesti.ee ningliidestuvate osapoolte vahel. Liidestuv osapool on erinevas valdkonnasMTT-sid ja TL-sid väljastav asutus. Ehk eesti.ee pärib liidestuvaltosapoolelt läbi X-Tee REST teenuse kindla kasutaja ettevõttetegevuslubade ja majandustegevusteadete andmeid. |
| ADS OID | Aadressiobjekti identifikaator (Aadressiandmete süsteem (ADS) onorganisatsiooniliste, tehnilisteja õiguslike vahendite raamistik,mistagab aadressiobjektide ühese identifitseerimise nii nende asukohaskui ka erinevates andmekogudes ning koha-aadresside määramiseja aadressiandmete töötlemise ühtse korralduse. |
| EHIS | Eesti Hariduse Infosüsteem |
| Lihtlink | Link teisele süsteemile, mis ei edasta süsteemide vahel kasutaja egateenuse andmeid ja ei eelda lisaarendusi. |
| MEDRE2 | Tervishoiukorralduse infosüsteem |
| MeM | Maaeluministeerium |
| MTR | Majandustegevuse register |
| PMAIS | Pöllumajandusameti infosüsteem (üks PTA menetlussüsteemidest) |
| PRIA | Pöllumajanduse Registrite ja Informatsiooni Amet |
| e-PRIA | PRIA iseteeninduskeskkond |
| PTA | Pöllumajandus-ja Toiduamet |
| SSO/GOVSSO | Pääsu reguleerimise funktsioon (SSO ehk single sign-on), misvõimaldab kasutajal pöörduda üheainsa logimisega paljude eriressursside poole. |
| Süvalink | Link otse vormile või teenuse lehele, lahendus võimaldab andmeteedastamist jatagasikutsumise funktsionaalsuse kasutamist.Parimtulemus saavutatakse SSO olemasolul ja teenusepakkuja süsteemivõimekusega kasutada edastatud tagasikutse aadressi. |
| TTJA | Tarbijakaitse ja Tehnilise Järelevalve Amet |
| X-Tee | X-Tee on andmevahetuse platvorm, mis võimaldab turvaliselt asutustevahel teavet pärida ja vahetada. |


## 3 Äriloogika

Tegevuslubade ja majandustegevusteadete ärisündmuse eesmärk on koondada ühte eesti.ee vaatesse kõik ettevõttele väljastatud tegevusload ja ettevõtte esitatud majandustegevusteated. See tähendab,  et ettevõtte esindajal on võimalik näha korraga erinevate Eesti riigi asutuste alla käivate tegevuslubade ja majandustegevusteadete andmeid. Neid asutusi nimetatakse selle liidestuse kontekstis „liidestuvateks asutusteks“.
Eesti.ee Tegevuslubade ja majandustegevusteadete alamsüsteem kogub kõikidest liidestunud asutustest REST teenuse kaudu kõik kehtivad kasutaja tegevusload ja majandustegevusteated ning kuvab lõppkasutajale mugavalt ühes kohas.
Täpsemalt loe ärisündmusteenuste kohta Riigi Infosüsteemi Ameti kodulehelt https://www.ria.ee/riigi-infosusteem/personaalriik/sundmusteenuste-platvorm ning Majandus- ja Kommunikatsiooniministeeriumi lehelt:  https://mkm.ee/digiriik-ja-uhenduvus/digiteenused/ettevotja-digivarav-ja-sundmusteenused .  

## 4 Liidestumine

Tegevuslubade ja majandustegevusteadete alamsüsteem liidestub iga asutuse infosüsteemiga üle REST teenuse, mis on vaja igal asutusel implementeerida eraldi. Selle ühtlustamiseks ning lihtsustamiseks oleme loonud OpenAPI spetsifikatsiooni, mille  alusel on võimalik automaatselt genereerida toimiv REST teenuse põhi koos andmemudeliga. Andmete täitmise eest vastutab iga asutus ise. Tegevuslubade ja majandustegevusteadete alamsüsteem ning asutus(t)e-vaheline suhtlus toimub üle X-Tee, et tagada andmevahetuse turvalisus, seda illustreerib Joonis 1.

Joonis 1. Riigiportaali ja asutus(t)e-vaheline liidestus.
![image](https://github.com/user-attachments/assets/859450ea-60b6-4592-9f10-06e15fd442b8)

### 4.1 Nõuded tehnoloogiatele

* X-Tee v6/v7

  * https://x-tee.ee/service-catalog/organizations/70006317/subsystems/sun-activity-licences

* OpenAPI 3.0

  * YAML

### 4.2 HTTP koodid:

* 500 Internal Server Error-Süsteemi viga millest ei ole võimalik taastuda

* 400 Bad Request-Päringu töötlemisest keeldumine (viga päringu sisus, kujus vms)

* 403 Forbidden -Päringu töötlemisest keeldumine (teostajal puuduvad pääsuõigused)

* 200 OK-Päringu töötlemine õnnestus ja tagastatakse vastus (ka tühi massiiv)

### 4.3 API visualiseerimiseks:

* https://editor.swagger.io/

* https://editor-next.swagger.io/  (uuem kuid alpha versioonis)

### 4.4 Päringu URL-i kuju

| Teekond | `/activity-licences` |  |
| -- | -- | -- |
| GET parameeter | `registrationCode` | ettevõtte / juriidilise isiku äriregistri koodvõi eraisiku isikukood |
| Näide | `/activity-licences?registrationCode=10364097`|  |


### 4.5 Päised

| Päis | Lubatud väärtused | Näide |
| -- | -- | -- |
| Accept | application/json; charset=utf-8 |  |
| Accept-Language | ET/EN/RU | ET |
| X-Road-UserId | <ISO 3166-1 alpha-2> < personalCode > | EE48302129533 |
| X-Road-Issue | Tegevuslubade ja majandustegevusteadetepäring |  |

Inglise ja vene keeles vastuse puudumisel palume vastata eesti keeles.
Täpsemalt saab X-Tee päiste põhimõtete ja kasutamise kohta lugeda siit: https://docs.x-road.global/Protocols/pr-rest_x-road_message_protocol_for_rest.html#43-use-of-http-headers 

### 4.6 Päringu vastuse kuju

Päringu vastuse oodatav kuju on PRIA loomreg (Loomade registri infosüsteem) näitel järgmine:
```
[
  {
    "lang": "ET",
    "type": "LICENCE",
    "principalActivity": "Mitteloomse toidu ja liittoidu käitlemine",
    "status": "VALID",
    "documentCode": "6433",
    "documentCodeType": "Tegevusloa number",
    "validFrom": "12.04.2014",
    "validTo": "Tähtajatu",
    "validDateType": "Tegevusloa kehtivusaja vahemik",
    "locations": [
      {
        "adsOid": "ME01656795",
        "addressText": "Laki 11a, Mustamäe linnaosa, Tallinn, Harjumaa, 12915",
        "locationName": "string",
        "status": "VALID",
        "fieldOfActivities": [
          {
            "fieldOfActivityName": "Kala käitlemine",
            "status": "VALID"
          }
        ]
      }
    ],
    "publishedByOrganizationName": "Põllumajandus- ja Toiduamet",
    "publishedByOrganizationLink": "https://pta.agri.ee/",
    "deepLink": {
      "deepLinkRead": "https://portaal.agri.ee/epm-portal-ng/esileht.html",
      "deepLinkUpdate": "https://portaal.agri.ee/epm-portal-ng/esileht.html",
      "deepLinkSuspend": "https://portaal.agri.ee/epm-portal-ng/esileht.html",
      "deepLinkCancel": "https://portaal.agri.ee/epm-portal-ng/esileht.html"
    },
    "contactInformation": {
      "organizationContactFirstName": "Mari",
      "organizationContactLastName": "Tamm",
      "organizationContactRole": "Tartu peaekspert",
      "organizationContactPhoneNr": "6208346",
      "organizationContactEmail": "mari.tamm@agri.pta.ee",
      "registryName": "Järelevalve infosüsteem",
      "registryContactPhoneNr": "+372 605 1710",
      "registryContactEmail": "info@pta.agri.ee",
      "supervisionText": "Toidu ja toidu käitlemise nõuetekohasust kontrollib PTA. Toidu käitlemise nõuetekohasuse hindamise eest tasutakse järelevalvetasu.",
      "supervisionLink": "https://pta.agri.ee/jarelevalve-korraldus-ja-tulemused/jarelevalve-korraldus/tasud-ja-riigiloivud"
    }
  }
]
```

Juhul kui pole midagi tagastada, siis tagastatakse tühi massiiv.
```
[ ]
```

## 5 Andmemudel

Ärisündmusteenuse arenduse käigus on RIA koos arenduspartneriga saanud valmis andmemudeli, mille alusel on loodud X-Tee REST teenuse spetsifikatsioon. Liidestuva asutuse ülesandeks on võtta aluseks andmemudel ning kaardistada see oma registri / infosüsteemi tegevuslubade ja majandustegevusteadete andmemudeli vastu. 

### 5.1 Kohustuslikkus

Erinevate asutuste andmemudeli ja andmekomplektide terviklikkuse tõttu ei ole tegevuslubade ja majandustegevusteadete andmemudelis tehnilises mõistes*  pea ükski väli kohustuslik. Küll aga on all tabelis lisatud veerg „Kohustuslikkus“, mis annab ülevaate, **millised väljad võiks ideaalis täidetud olla.** 
<br>
*Tehnilises mõistes ehk REST spetsifikatsiooni järgi pole ükski väli kohustuslik ehk välja mitte täitmisel ei teki veaolukorda. 

### 5.2 Andmekoosseis

Andmemudeli analüüsi käigus, kus RIA sündmusteenuse tooteomanik ning arenduspartneri osapooled kohtusid liidestuvate asutustega, leiti, **et kehtetuid MTT-sid ega TL-isid ei ole vaja eesti.ee-sse laadida.** 2024. sündmusteenuse projekti raames pole vaja andmevahetuses Kehtetuid lubasid ega teateid eesti.ee-le saata. Lisaks loa andmetele kuvatakse kasutajaliideses välja ka selle loa väljastanud asutusega ning algandmete registriga seonduvad kontaktandmed, mis on väljadel `contactInformation`.

### 5.3 Süvalink

Andmevahetuse üks osa on nn süvalinkide edastamine eesti.ee-le, mis võimaldab kasutaja suunata eesti.ee vaatest otse loa / teate väljastanud asutuse registrisse / kliendiportaali. Süvalink võiks võimaldada:

* Suunata kasutaja konkreetse loa / teate vaatesse:
  * vaatamiseks,
  * muutmiseks, (peatamiseks, tühistamiseks)
* *Autendituna* - GovSSO võimaldab asutuste-vahelist autentimist, kuid eeldab GovSSO-ga liitumist ja **ei ole seega kohustuslik**
* *Korrektses (ettevõtja) rollis* - GovSSO juuni 2024 veel ei võimalda rolli/esindatava valikut, võib tähendada lisaarendusi ja **ei ole seega kohustuslik**

Liidestumise esmane plaan on edastada eesti.ee-le linke, **mis ei eelda asutuse infosüsteemis lisaarendusi**. 
Süvalinkide võimekuse puudumisel täita „deepLink“ andmestik parima võimalikuga, isegi kui see tähendab **lihtlinki** asutuse portaali otsingusse, sisselogimise vaatesse vms.


### 5.4 Andmemudeli väljad

Joonisel 2 on näha andmemudeli väljad ja nende hierarhia. Põhiline tähelepanek on locations ja fieldOfActivities kollektsioonide juures. Tulenevalt asutuste lubade ja teadete andmekoosseisu erinevustest võib **osadel lubadel / teadetel olla 0 kuni mitu tegevuskohta, millel omakorda erinevad tegevusalad.** Enamuste asutuste puhul ei ole tegevuskohtade ja tegevusalade seosed nii keerulised. 

0 kuni mitu tegevuskohta tähendab, et osa lubadel / teadetel pole eraldi tegevuskohta (-kohti). Sellistel juhtudel edastatakse tegevuskohana ettevõtte juriidiline aadress.

Joonis 2. Tegevuslubadeja majandustegevusteadete andmemudel eesti.ee-s koos majandustegevusteate näitega.
![image](https://github.com/user-attachments/assets/ce4793db-069c-4b80-b774-0dda69f0e44e)


Tabel 1 annab ülevaate andmemudeli väljadest koos selgitusega.

Tabel 1.Tegevuslubade ja majandustegevusteadete andmemudeli kohustuslikkus ja selgitused.
| Andmeväli | Kohustuslikkus | Selgitus |
| -- | -- | -- |
|	lang	|	JAH	|	EN, RU või EE ehk millises keeles* päringu vastuse andmed on. <br> *2024. aasta projekti raames on enamik liidestuvatel osapooltel võimalik lubade ja teadete andmeid edastada vaid eesti keeles. See väli on andmemudelisse lisatud eelkõige tuleviku muudatuste jaoks, kui tekib ka võimalus andmeid inglise keeles edastada.	|
|	type 	|	JAH	|	Näitab, kas tegemist on majandustegevusteate või tegevusloaga.	| NOTICE - Majandustegevusteade (Economic Activity Notice) <br> LICENCE - Tegevusluba (Business Licence) 	
|	principalActivity	|	JAH	|	 Põhiline tegevusala – üldisem tegevusala või valdkond. Tegevusalad ehk field of activities on sellest eraldi. (Näited on eraldi peatükis)	|
|	status	|	JAH	|	Tegevusloa või majandustegevusteate staatus, mis võivad olla 4 kindlat varianti:	<br>1.VALID - Kehtiv	<br>2.PENDING - Ootel	<br>3.SUSPENDED - Peatatud	<br>4.INVALID - Kehtetu* <br> * *kehtetuid lube ega teateid 2024. aasta tegevuslubade ja majandustegevusteadete sündmusteenuse projekti raames eesti.ee-sse ei laeta. See staatuse valik jääb sisse potentsiaalsete tulevikutööde tõttu	|
|	documentCode	|	JAH	|	Loa või teate number ehk tunnus. Enamik juhtudel on selle välja väärtus kas „Majandustegevusteate number“ või „Tegevusloa number“. Kuid osadel juhtudel võib loa /  teate tunnuseks olla mingi muu dokument või kood.	|
|	documentCodeType	|	EI - Täpsustamise eesmärgil|	Loa või teate tunnuse tüüp. Seda välja võib kasutada, kui eelmine väli `documentCode` erineb tavalisetest loa / teate tunnuse tüüpidest. Iga asutus saab ise otsustada ja paika panna reeglid, mille alusel „documentCodeType“ väli väärtustada.	|
|	validFrom	|	EI	|	Loa / teate kehtivusaja alguse kuupäev formaadis dd.mm.yyyy. See on tavaline tekstiväli mitte kindla formaadiga kuupäeva väli. Kui loal / teatel pole kehtivuse alguse kuupäeva siis eesti.ee-s seda välja ei kuvata.|
|	validTo	|	EI	|	Loa / teate kehtivusaja lõpu  kuupäev formaadis dd.mm.yyyy. See on tavaline tekstiväli mitte kindla formaadiga kuupäeva väli. Mitmel juhul on välja väärtus „Tähtajatu“.	Kui loal / teatel pole kehtivuse lõpu väärtust siis eesti.ee-s seda välja ei kuvata.		|
|	validDateType	|	EI - Täpsustamise eesmärgil|	Loa / teate kehtivusvahemiku tüüp. Seda välja võib kasutada, kui validTo ja validFrom väljad ei näita loa / teate kehtivust, vaid mingit muud ajavahemikku.	Iga asutus saab ise otsustada ja paika panna reeglid, mille alusel `validDateType` väli väärtustada.	 |
|	locations.adsOid	|	EI	|	Loa / teate tegevuskoha ADS OID ehk aadressiandmete infosüsteemi konkreetse aadressi objekti ID. Iga asutus saab ise otsustada, kas neil on võimalik tegevuskoha aadressi ADS OID eesti.ee-le saata. Liidestumise projekti esmaprioriteet on saada andmevahetuse kaudu aadress tekstikujul ehk vaja väärtustada väli „addressText“.|
|	locations[].addressText	|	JAH	|	Loa / teate tegevuskoha aadress teksti kujul. Ideaalis võiks formaat olla järgnev: <br>„Lõõtsa tn 6, Lasnamäe linnaosa, Tallinn, Harju maakond“;	<br> „Veskitammi tn 4, Laagri alevik, Saue vald, Harju maakond“		|
|	locations[].locationName	|	JAH*	|	Loa / teate tegevuskoha nimetus. Nimetus võib olla füüsilise või veebipoe nimi, meditsiiniasutuse üldnimetus jms. <br> *Selle välja väärtustamine on äriliselt kohustuslik, kui loal / teatel puudub tekstikujul aadress või ADS OID. Näiteks taksojuhi sõidukikaartidel ei ole konkreetset tegevuskohta ega juriidilist aadressi, vaid teeninduspiirkond.	|
|	locations[].status	|	EI	|	Loa / teate tegevuskoha staatus. 	Tegevuskohtade puhul eristame 2 staatust:	<br>1.VALID (Kehtiv) – tegevuskoht on kehtiv. Vaikimisi on tegevuskoht VALID staatuses.	<br> 2. SUSPENDED (Peatatud) – osa asutuste andmemudeli kohaselt võib tegevusluba olla kehtivas staatuses, kuid selle 1 kuni mitu tegevuskohta on peatatud staatuses.	<br> Iga asutus saab ise otsustada ja paika panna reeglid, millal saata eesti.ee-le tegevuskoha staatus „SUSPENDED“. Kui tegemist on asutusega, mille lubade / teadete tegevuskohtadel pole staatust, siis andmevahetuses tuleb edastada „status“: null. Küll aga, kui „mõtteliselt“ või äriliselt on siiski võimalik eristada tegevuskoha staatust kui „VALID“, siis võiks saata selle väärtuse. Kui `locations[].status = null` või „“, siis eesti.ee loeb selle vaikimisi „VALID“ staatuseks.	|
|	locations[].fieldOfActivities[].fieldOfActivityName	|	JAH	|	 Loa /  teate tegevusala nimetus eesti keeles. 	Kui loal / teatel on **mitu tegevuskohta, aga ainult 1 tegevusala**, mis kehtib kõikide kohtade puhul, siis lisada see tegevusala iga location[].fieldOfActivites alla. Kui loal / teatel on **mitu tegevuskohta ning iga tegevuskoha käivad vastavad erinevad tegevusalad**, siis lisada need vastavalt õige tegevuskoha alla.			|
|	locations[].fieldOfActivities[].status	|	EI	|	 Loa / teate tegevusala staatus			Tegevusalade puhul eristame 2 staatust: <br>1.VALID (Kehtiv) – tegevusala on kehtiv. **Vaikimisi on tegevusala VALID staatuses**. <br>	2.SUSPENDED (Peatatud) – osa asutuste andmemudeli kohaselt võib tegevusluba olla kehtivas staatuses, kuid selle 1 kuni mitu tegevusala on peatatud staatuses.	<br> Kui tegemist on asutusega, mille lubade / teadete tegevusaladel pole staatust, siis andmevahetuses tuleb edastada „status“: null. 	Küll aga, kui „mõtteliselt“ või äriliselt on siiski võimalik eristada tegevusala staatust kui „VALID“, siis võiks saata selle väärtuse.	Kui locations[].fieldOfActivities[].status = null või „“, siis eesti.ee loeb selle vaikimisi „VALID“ staatuseks.	|
|	publishedByOrganizationName	|	JAH	|	 Loa / teate väljastanud asutuse täispikk nimetus* <br> *vastavalt keele parameetrile kas eesti või inglise keeles. Vaikimisi võib nimetus olla alati eesti keeles.		|
|	publishedByOrganizationLink	|	JAH	|	 Loa / teate väljastanud asutuse kodulehe URL	|
|	deepLinks.deepLinkRead	|	JAH	|	Loa / teate algandmete registrisse / kliendiportaali suunav URL, kus kasutaja saab loa / teate andmeid vaadata. Iga asutus otsustab ise, millise registri / portaali linki on kõige mõistlikum deepLinks väljadel eesti.ee-le edastada. **Liidestumise esmases faasis võiks asutus edastada eesti.ee-le vähemalt loa / teate vaatamise URL-i (liht-või süvalink)**	|
|	deepLinks.deepLinkUpdate	|	EI	|	Loa / teate algandmete registrisse / kliendiportaali suunav URL, kus kasutaja saab luba / teadet muuta.	|
|	deepLinks.deepLinkSuspend	|	EI	|	Loa / teate algandmete registrisse / kliendiportaali suunav URL, kus kasutaja saab luba / teadet peatada.	|
|	deepLinks.deepLinkCancel	|	EI	|	Loa / teate algandmete registrisse / kliendiportaali suunav URL, kus kasutaja saab luba / teadet tühistada.	|
|	contactInformation.organizationContactFirstName	|	EI	|	Loa / teate väljastanud asutuse loa / teate tegevusalaga seotud valdkonna kontakti eesnimi.	|
|	contactInformation.organizationContactLastName	|	EI	|	Loa / teate väljastanud asutuse loa / teate tegevusalaga seotud valdkonna kontakti perekonnanimi.	|
|	contactInformation.organizationContactRole	|	EI	|	 Loa / teate väljastanud asutuse loa / teate tegevusalaga seotud valdkonna kontakti roll.	|
|	contactInformation.organizationContactPhoneNr	|	JAH	|	 Loa / teate väljastanud asutuse loa / teate tegevusalaga seotud valdkonna kontakti telefoninumber.	|	
|	contactInformation.organizationContactEmail	|	JAH	|	 Loa / teate väljastanud asutuse loa / teate tegevusalaga seotud valdkonna kontakti emaili aadress.	|		
|	contactInformation.registryName	|	JAH	|	 Loa / teate algandmete registri nimetus* <br> *vastavalt keele parameetrile kas eesti või inglise keeles. Vaikimisi võib nimetus olla alati eesti keeles. 	|
|	contactInformation.registryContactPhoneNr	|	JAH	|	 Loa / teate algandmete registri kontakti telefoni number	|
|	contactInformation.registryContactEmail	|	JAH	|	 Loa / teate algandmete registri kontakti emaili aadress	|				
|	contactInformation.supervisionText	|	EI	|	 Loa / teate valdkonnale rakenduvatest erinõuetest tulenev järelevalve infotekst.	Infotekst võiks olla maksimaalselt 2 lühikest lauset.	|
|	contactInformation.supervisionLink	|	EI	|	Järelevalve infotekstiga kokku käiv asutuse portaali URL, kus on pikemalt ja täpsemalt selle loa / teate valdkonna järelevalvest juttu.	|


## 6 Andmemudeli näited JSON kujul

Järgmises peatükis on toodud andmemudeli näited JSON kujul erinevate asutuste tegevuslubadest ja majandustegevusteadetest. Näidete eesmärk on näidata, kuidas asutuste andmekomplektid eesti.ee sündmusteenuse andmemudelisse kaardistada. 

Andmed on võetud avalikult kättesaadavatest registritest.

### 6.1 Näide 1: TTJA MTR-i taksoveo sõidukikaardi tegevusluba

Näite algandmed on võetud MTR-ist [https://mtr.ttja.ee/taotluse_tulemus/633855](https://mtr.ttja.ee/taotluse_tulemus/633855) 
```
[
  {
    "lang": "ET",
    "type": "LICENCE",
    "principalActivity": "Taksoveo sõidukikaart", // MTR väli "Tegevusala"
    "status": "VALID",
    "documentCode": "TVSK035325",
    "documentCodeType": "Tegevusloa number",
    "validFrom": "05.06.2024",
    "validTo": "Tähtajatu",
    "validDateType": "Tegevusloa kehtivusaja vahemik",
    "locations": [
      {
        "adsOid": "",
        "addressText": "Madara tn 33/2, Kristiine linnaosa, Tallinn, Harju maakond, 10613", // ettevõtte juriidiline aadress
        "locationName": "Põhiline teeninduspiirkond Saku vald, Harju maakond", // Teeninduspiirkond
        "status": "VALID",
        "fieldOfActivities": [
          {
            "fieldOfActivityName": "Taksoveo sõidukikaart", // selle näite puhul samuti MTR "Tegevusala", kuna Tegevuse liike pole eraldi välja toodud
            "status": "VALID"
          }
        ]
      }
    ],
    "publishedByOrganizationName": "Tarbijakaitse- ja Tehnilise Järelevalve amet",
    "publishedByOrganizationLink": "https://www.ttja.ee/",
    "deepLink": {
      "deepLinkRead": "https://mtr.ttja.ee/taotluse_tulemus/633855",
      "deepLinkUpdate": "https://mtr.ttja.ee/taotluse_tulemus/633855",
      "deepLinkSuspend": "https://mtr.ttja.ee/taotluse_tulemus/633855",
      "deepLinkCancel": "https://mtr.ttja.ee/taotluse_tulemus/633855"
    },
    "contactInformation": {
      "organizationContactFirstName": "",
      "organizationContactLastName": "",
      "organizationContactRole": "",
      "organizationContactPhoneNr": "",
      "organizationContactEmail": "",
      "registryName": "Majandustegevuse register",
      "registryContactPhoneNr": "6687080",
      "registryContactEmail": "register@ttja.ee",
      "supervisionText": "Järelevalvet kaubanduse valdkonnas teevad Tarbijakaitse- ja Tehnilise Järelevelave amet, Terviseamet ja valla- või linnavalituses.",
      "supervisionLink": ""
    }
  }
]
]

```
### 6.2	Näide 2: TTJA MTR-i toitlustuse majandustegevusteade
Näite algandmed on võetud MTR-ist  [https://mtr.ttja.ee/taotluse_tulemus/29592](https://mtr.ttja.ee/taotluse_tulemus/295926) 
```
[
    {
      "lang": "ET",
      "type": "NOTICE",
      "principalActivity": "Toitlustamine", // MTR väli "Tegevusala"
      "status": "VALID",
      "documentCode": "KTO006340",
      "documentCodeType": "Majandustegevusteate number",
      "validFrom": "01.03.2007",
      "validTo": "Tähtajatu",
      "validDateType": "Majandustegevusteate kehtivusaja vahemik",
      "locations": [
        {
          "adsOid": "",
          "addressText": "Müürivahe tn 14, Kesklinna linnaosa, Tallinn, Harju maakond", // tegevukoht
          "locationName": "Reval Cafe", // toitlustuskoha nimetus
          "status": "VALID",
          "fieldOfActivities": [
            {
              "fieldOfActivityName": "Toitlustamine", // selle näite puhul samuti MTR "Tegevusala", kuna Tegevuse liike pole eraldi välja toodud
              "status": "VALID"
            }
          ]
        },
        {
            "adsOid": "",
            "addressText": "Rävala pst 3, Kesklinna linnaosa, Tallinn, Harju maakond, 10143", // tegevukoht
            "locationName": "Reval Cafe", // toitlustuskoha nimetus
            "status": "VALID",
            "fieldOfActivities": [
              {
                "fieldOfActivityName": "Toitlustamine", // selle näite puhul samuti MTR "Tegevusala", kuna Tegevuse liike pole eraldi välja toodud
                "status": "VALID"
              }
            ]
          },
          {
            "adsOid": "",
            "addressText": "Riia tn 4-1, Tartu linn, Tartu linn, 51004", // tegevukoht
            "locationName": "Reval Cafe", // toitlustuskoha nimetus
            "status": "VALID",
            "fieldOfActivities": [
              {
                "fieldOfActivityName": "Toitlustamine", // selle näite puhul samuti MTR "Tegevusala", kuna Tegevuse liike pole eraldi välja toodud
                "status": "VALID"
              }
            ]
          },
          {
            "adsOid": "",
            "addressText": "Estonia pst 9-1, Kesklinna linnaosa, Tallinn, Harju maakond", // tegevukoht
            "locationName": "Reval Gelato", // toitlustuskoha nimetus
            "status": "VALID",
            "fieldOfActivities": [
              {
                "fieldOfActivityName": "Toitlustamine", // selle näite puhul samuti MTR "Tegevusala", kuna Tegevuse liike pole eraldi välja toodud
                "status": "VALID"
              }
            ]
          },
          {
            "adsOid": "",
            "addressText": "Rotermanni tn 14, Kesklinna linnaosa, Tallinn, Harju maakond, 10111", // tegevukoht
            "locationName": "R14", // toitlustuskoha nimetus
            "status": "VALID",
            "fieldOfActivities": [
              {
                "fieldOfActivityName": "Toitlustamine", // selle näite puhul samuti MTR "Tegevusala", kuna Tegevuse liike pole eraldi välja toodud
                "status": "VALID"
              }
            ]
          },
          {
            "adsOid": "",
            "addressText": "Paju tn 2, Tartu linn, Tartu linn, 50603", // tegevukoht
            "locationName": "Reval Cafe", // toitlustuskoha nimetus
            "status": "VALID",
            "fieldOfActivities": [
              {
                "fieldOfActivityName": "Toitlustamine", // selle näite puhul samuti MTR "Tegevusala", kuna Tegevuse liike pole eraldi välja toodud
                "status": "VALID"
              }
            ]
          },
          {
            "adsOid": "",
            "addressText": "Laki tn 11a, Mustamäe linnaosa, Tallinn, Harju maakond, 12915", // tegevukoht
            "locationName": "Reval Cafe", // toitlustuskoha nimetus
            "status": "VALID",
            "fieldOfActivities": [
              {
                "fieldOfActivityName": "Toitlustamine", // selle näite puhul samuti MTR "Tegevusala", kuna Tegevuse liike pole eraldi välja toodud
                "status": "VALID"
              }
            ]
          }
      ],
      "publishedByOrganizationName": "Tarbijakaitse- ja Tehnilise Järelevalve amet",
      "publishedByOrganizationLink": "https://www.ttja.ee/",
      "deepLink": {
        "deepLinkRead": "https://mtr.ttja.ee/taotluse_tulemus/295926",
        "deepLinkUpdate": "https://mtr.ttja.ee/taotluse_tulemus/295926",
        "deepLinkSuspend": "https://mtr.ttja.ee/taotluse_tulemus/295926",
        "deepLinkCancel": "https://mtr.ttja.ee/taotluse_tulemus/295926"
      },
      "contactInformation": {
        "organizationContactFirstName": "",
        "organizationContactLastName": "",
        "organizationContactRole": "",
        "organizationContactPhoneNr": "",
        "organizationContactEmail": "",
        "registryName": "Majandustegevuse register",
        "registryContactPhoneNr": "6687080",
        "registryContactEmail": "register@ttja.ee",
        "supervisionText": "Järelevalvet kaubanduse valdkonnas teevad Tarbijakaitse- ja Tehnilise Järelevelave amet, Terviseamet ja valla- või linnavalituses.",
        "supervisionLink": ""
      }
    }
  ]

```

### 6.3 Näide 3: Terviseameti haigla tegevusluba

Näide pärineb Terviseameti MEDRE registrist [https://medre.tehik.ee/activity-licenses/2328914d-aefe-4765-829e-a3c4a22e651f](https://medre.tehik.ee/activity-licenses/2328914d-aefe-4765-829e-a3c4a22e651f)

```
[
    {
      "lang": "ET",
      "type": "LICENCE",
      "principalActivity": "Piirkondlik haigla", // Tervisemaeti MEDRE registri väli "Haigla liik"
      "status": "VALID",
      "documentCode": "L03287",
      "documentCodeType": "Tegevusloa number",
      "validFrom": "03.12.2013",
      "validTo": "Tähtajatu",
      "validDateType": "Tegevusloa kehtivusaja vahemik",
      "locations": [
        {
          "adsOid": "",
          "addressText": "Harju maakond, Kose vald, Kose alevik, Ravila mnt 27", // tegevuskoht
          "locationName": "", 
          "status": "SUSPENDED", // Haigla tegevusloa ühe tegevuskoha staatus on "Peatatud"
          "fieldOfActivities": [
            {
              "fieldOfActivityName": "Eriarstiabi teenus", // MEDRE "Tegevusala"
              "status": "VALID"
            }
          ]
        },
        {
            "adsOid": "",
            "addressText": "Harju maakond, Tallinn, Põhja-Tallinna linnaosa, Paldiski mnt 52", // tegevuskoht
            "locationName": "", 
            "status": "VALID",
            "fieldOfActivities": [
              {
                "fieldOfActivityName": "Eriarstiabi teenus", 
                "status": "VALID"
              }
            ]
          },
          {
            "adsOid": "",
            "addressText": "Harju maakond, Tallinn, Nõmme linnaosa, Hiiu tn 44", // tegevuskoht
            "locationName": "", 
            "status": "VALID",
            "fieldOfActivities": [
              {
                "fieldOfActivityName": "Eriarstiabi teenus", 
              }
            ]
          },
          {
            "adsOid": "",
            "addressText": "Harju maakond, Tallinn, Nõmme linnaosa, Hiiu tn 39", // tegevuskoht
            "locationName": "", 
            "status": "VALID",
            "fieldOfActivities": [
              {
                "fieldOfActivityName": "Eriarstiabi teenus", 
                "status": "VALID"
              }
            ]
          },
          {
            "adsOid": "",
            "addressText": "Harju maakond, Tallinn, Mustamäe linnaosa, J. Sütiste tee 19", // tegevuskoht
            "locationName": "", 
            "status": "VALID",
            "fieldOfActivities": [
              {
                "fieldOfActivityName": "Eriarstiabi teenus", 
                "status": "VALID"
              }
            ]
          }
      ],
      "publishedByOrganizationName": "Tarbijakaitse- ja Tehnilise Järelevalve amet",
      "publishedByOrganizationLink": "https://www.ttja.ee/",
      "deepLink": {
        "deepLinkRead": "https://mtr.ttja.ee/taotluse_tulemus/295926",
        "deepLinkUpdate": "https://mtr.ttja.ee/taotluse_tulemus/295926",
        "deepLinkSuspend": "https://mtr.ttja.ee/taotluse_tulemus/295926",
        "deepLinkCancel": "https://mtr.ttja.ee/taotluse_tulemus/295926"
      },
      "contactInformation": {
        "organizationContactFirstName": "",
        "organizationContactLastName": "",
        "organizationContactRole": "",
        "organizationContactPhoneNr": "",
        "organizationContactEmail": "",
        "registryName": "Majandustegevuse register",
        "registryContactPhoneNr": "6687080",
        "registryContactEmail": "register@ttja.ee",
        "supervisionText": "Järelevalvet kaubanduse valdkonnas teevad Tarbijakaitse- ja Tehnilise Järelevelave amet, Terviseamet ja valla- või linnavalituses.",
        "supervisionLink": ""
      }
    }
  ]

```

