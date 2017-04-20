<properties 
   pageTitle="StorSimple systeemlimieten | Microsoft Azure"
   description="Beschrijving van systeemlimieten en aanbevolen formaten voor StorSimple 8000 reeks onderdelen en verbindingen."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/06/2016"
   ms.author="alkohli" />

# <a name="storsimple-system-limits"></a>Limieten voor StorSimple-systeem

## <a name="overview"></a>Overzicht

StorSimple biedt scalable en flexibele opslagruimte voor uw datacenter. Er zijn echter enkele beperkingen die u in gedachten houden moet terwijl u van plan bent, distribueren en uw oplossing StorSimple gebruiken. De volgende tabel worden deze limieten vermeld en vindt u enkele aanbevelingen zodat u optimaal gebruikmaken van uw oplossing StorSimple kan bereiken.

| Limiet-ID | Limiet | Opmerkingen |
|----------------- | ------|--------- |
| Maximum aantal accountreferenties opslag | 64 | |
| Maximum aantal volume containers | 64 | |
| Maximum aantal van hoeveelheden | 255 | |
| Maximum aantal van lokaal vastgemaakte hoeveelheden | 32 | |
| Maximum aantal planningen per bandbreedte sjabloon | 168 | Een planning voor elk uur wordt elke dag van de week (24 * 7). |
| Maximale grootte van een doorverbonden volume op fysieke apparaten | 64 TB voor 8100 en 8600 | 8100 en 8600 zijn fysieke apparaten. |
| Maximale grootte van een doorverbonden volume op virtuele apparaten in Azure wordt aangegeven | 30 TB voor 8010 <br></br> 64 TB voor 8020 | 8010 en 8020 zijn virtuele apparaten in Azure wordt aangegeven die standaardopslag en Premium opslag respectievelijk gebruiken. |
| Maximale grootte van een lokaal vastgemaakte volume op fysieke apparaten | 8,5 TB voor 8100 <br></br> 22,5 TB voor 8600 | 8100 en 8600 zijn fysieke apparaten. |
| Maximum aantal iSCSI-verbindingen | 512 | |
| Maximum aantal iSCSI-verbindingen vanuit initiators | 512 | |
| Maximum aantal access besturingselement records per apparaat | 64 | |
| Maximum aantal van hoeveelheden per back-beleid | 24 | |
| Maximum aantal back-ups bewaard per planning (in een back-beleid) | 64 | |
| Maximum aantal planningen per back-beleid | 10 | |
| Maximum aantal momentopnamen van elk type dat kan worden bewaard per volume | 256 | Dit nummer lokale momentopnamen bevatten en cloud momentopnamen. |
| Maximum aantal momentopnamen die kunnen voorkomen in elk apparaat | 10.000 | |
| Maximum aantal van hoeveelheden die kunnen worden verwerkt parallel voor back-up en herstellen, of klonen | 16 |<ul><li>Als er meer dan 16 volumes, worden ze opeenvolging verwerkt verwerking sleuven beschikbaar komen.</li><li>Nieuwe back-ups van een gekloonde of het volume van een herstelde doorverbonden kan pas de bewerking is voltooid. Back-ups zijn echter voor een lokale volume, nadat het volume online is toegestaan.</li></ul>|
| Terugzetten en klonen herstellen tijd voor trapsgewijze volumes | < 2 minuten | <ul><li>Het volume is beschikbaar binnen 2 minuten van terugzetten of klonen, ongeacht de grootte van het volume gemaakt.</li><li>De prestaties van het volume is mogelijk in eerste instantie langzamer dan normaal terwijl de meeste gegevens en metagegevens nog steeds bevindt zich in de cloud. Prestaties kan vergroten als gegevensstromen vanuit de cloud naar het StorSimple-apparaat.</li><li>De totale tijd voor het downloaden van metagegevens, is afhankelijk van de grootte van de toegewezen volume. Metagegevens is automatisch overgebracht naar het apparaat op de achtergrond met de snelheid van 5 minuten per TB aan toegewezen volumegegevens. Dit tarief kan worden beïnvloed door Internetbandbreedte die in de cloud.</li><li>De terugzetten of klonen bewerking is voltooid als alle metagegevens op het apparaat is.</li><li>Back-bewerkingen kunnen niet worden uitgevoerd totdat de terugzetten of klonen volledig voltooid is.|
| Herstellen tijd voor lokaal vastgemaakte volumes herstellen | < 2 minuten | <ul><li>Het volume is beschikbaar in twee minuten na het terugzetten, ongeacht de grootte van het volume gemaakt.</li><li>De prestaties van het volume is mogelijk in eerste instantie langzamer dan normaal terwijl de meeste gegevens en metagegevens nog steeds bevindt zich in de cloud. Prestaties kan vergroten als gegevensstromen vanuit de cloud naar het StorSimple-apparaat.</li><li>De totale tijd voor het downloaden van metagegevens, is afhankelijk van de grootte van de toegewezen volume. Metagegevens is automatisch overgebracht naar het apparaat op de achtergrond met de snelheid van 5 minuten per TB aan toegewezen volumegegevens. Dit tarief kan worden beïnvloed door Internetbandbreedte die in de cloud.</li><li>In tegenstelling tot doorverbonden volumes, voor lokaal vastgemaakte volumes, wordt ook lokaal de volumegegevens gedownload op het apparaat. De bewerking voor terugzetten is voltooid als alle volumegegevens is ingesteld bij het apparaat.</li><li>Het is mogelijk dat de bewerkingen herstellen lang. De totale tijd in beslag de terugzetten, is afhankelijk van de grootte van het ingerichte lokale volume, de bandbreedte van uw Internet en de bestaande gegevens op het apparaat. Back-bewerkingen op het volume van het lokaal vastgemaakte zijn toegestaan terwijl het terugzetten uitgevoerd wordt.|
|Verwerking rente voor cloud momentopnamen| 15 minuten/TB | <ul><li>Minimale tijd om aan de cloud snapshot gereed voor uploaden, per TB aan toegewezen volumegegevens in de back-up. </li><li> Totale cloud momentopname tijd wordt berekend door ditmaal toe te voegen aan de tijd voor het uploaden van momentopname, die wordt beïnvloed door de bandbreedte Internet naar cloud.
| Maximum aantal clients alleen-lezen/schrijven doorvoer (indien van de laag SSD bediend) * | 920/720 MB/s met één 10 GbE netwerkinterface | Maximaal 2 x met MPIO en twee netwerkinterfaces. |
| Maximum aantal clients alleen-lezen/schrijven doorvoer (indien van de harde schijf laag bediend) * | 120/250 MB/s |
| Maximum aantal clients alleen-lezen/schrijven doorvoer (wanneer served vanuit de cloud-laag)* voor Update 3 en hoger** | 40/60 MB/s voor doorverbonden volumes<br><br>60/80 MB/s voor volumes doorverbonden met archivering optie geselecteerd tijdens het volume maken | Alleen doorvoer, hangt af van clients genereren en bijhouden van voldoende i/o-wachtrij diepteas. <br><br>Snelheid bereikt, is afhankelijk van de snelheid van het onderliggende opslag-account gebruikt. | 

& #42; Maximumdoorvoer per i/o-type werd gemeten met 100 procent lezen en schrijven van 100 procent scenario's. Werkelijke doorvoer mogelijk lager en is afhankelijk van I/O meng en netwerk voorwaarden.

& #42; & #42; Prestaties getallen vóór Update 3 mogelijk lager.

## <a name="next-steps"></a>Volgende stappen

Bekijk de [systeemvereisten voor StorSimple](storsimple-system-requirements.md). 
