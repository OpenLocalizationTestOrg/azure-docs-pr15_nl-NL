<!--author=alkohli last changed: 12/15/15-->

| Limiet-ID | Limiet | Opmerkingen |
|----------------- | ------|--------- |
| Maximum aantal accountreferenties opslag | 64 | |
| Maximum aantal volume containers | 64 | |
| Maximum aantal van hoeveelheden | 255 | |
| Maximum aantal planningen per bandbreedte sjabloon | 168 | Een planning voor elk uur wordt elke dag van de week (24 * 7). |
| Maximale grootte van een doorverbonden volume op fysieke apparaten | 64 TB voor 8100 en 8600 | 8100 en 8600 zijn fysieke apparaten. |
| Maximale grootte van een doorverbonden volume op virtuele apparaten in Azure wordt aangegeven | 30 TB voor 8010 <br></br> 64 TB voor 8020 | 8010 en 8020 zijn virtuele apparaten in Azure wordt aangegeven die standaardopslag en Premium opslag respectievelijk gebruiken. |
| Maximale grootte van een lokaal vastgemaakte volume op fysieke apparaten | 9 TB voor 8100 <br></br> 24 TB voor 8600 | 8100 en 8600 zijn fysieke apparaten. |
| Maximum aantal iSCSI-verbindingen | 512 | |
| Maximum aantal iSCSI-verbindingen vanuit initiators | 512 | |
| Maximum aantal access besturingselement records per apparaat | 64 | |
| Maximum aantal van hoeveelheden per back-beleid | 24 | |
| Maximum aantal back-ups bewaard per back-beleid | 64 | |
| Maximum aantal planningen per back-beleid | 10 | |
| Maximum aantal momentopnamen van elk type dat kan worden bewaard per volume | 256 | Dit geldt ook voor lokale momentopnamen en cloud momentopnamen. |
| Maximum aantal momentopnamen die kunnen voorkomen in elk apparaat | 10.000 | |
| Maximum aantal van hoeveelheden die kunnen worden verwerkt parallel voor back-up en herstellen, of klonen | 16 |<ul><li>Als er meer dan 16 volumes, worden ze verwerkt opeenvolging verwerking sleuven beschikbaar komen.</li><li>Nieuwe back-ups van een gekloonde of het volume van een herstelde doorverbonden kan pas de bewerking is voltooid. Back-ups zijn echter voor een lokale volume, nadat het volume online is toegestaan.</li></ul>|
| Terugzetten en klonen herstellen tijd voor trapsgewijze volumes | < 2 minuten | <ul><li>Het volume is beschikbaar binnen 2 minuten van terugzetten of klonen, ongeacht de grootte van het volume gemaakt.</li><li>De prestaties van het volume is mogelijk in eerste instantie langzamer dan normaal terwijl de meeste gegevens en metagegevens nog steeds bevindt zich in de cloud. Prestaties kan vergroten als gegevensstromen vanuit de cloud naar het StorSimple-apparaat.</li><li>De totale tijd voor het downloaden van metagegevens, is afhankelijk van de grootte van de toegewezen volume. Metagegevens is automatisch overgebracht naar het apparaat op de achtergrond met de snelheid van 5 minuten per TB aan toegewezen volumegegevens. Dit tarief kan worden beïnvloed door Internetbandbreedte die in de cloud.</li><li>De terugzetten of klonen bewerking is voltooid als alle metagegevens op het apparaat is.</li><li>Back-bewerkingen kunnen niet worden uitgevoerd totdat de terugzetten of klonen volledig voltooid is.|
| Herstellen tijd voor lokaal vastgemaakte volumes herstellen | < 2 minuten | <ul><li>Het volume is beschikbaar binnen twee minuten na het terugzetten, ongeacht de grootte van het volume gemaakt.</li><li>De prestaties volume in eerste instantie mogelijk langzamer dan normaal terwijl de meeste gegevens en metagegevens is nog steeds zich bevindt in de cloud. Prestaties kan vergroten als gegevensstromen vanuit de cloud naar het StorSimple-apparaat.</li><li>De totale tijd voor het downloaden van metagegevens, is afhankelijk van de grootte van de toegewezen volume. Metagegevens is automatisch overgebracht naar het apparaat op de achtergrond met de snelheid van 5 minuten per TB aan toegewezen volumegegevens. Dit tarief kan worden beïnvloed door Internetbandbreedte die in de cloud.</li><li>In tegenstelling tot doorverbonden volumes, in het geval van lokaal vastgemaakte volumes, wordt ook lokaal de volumegegevens gedownload op het apparaat. De bewerking voor terugzetten is voltooid als alle volumegegevens is ingesteld bij het apparaat.</li><li>Het is mogelijk dat de bewerkingen terugzetten lang en de totale tijd in beslag de terugzetten hangt af van de grootte van het ingerichte lokale volume, de bandbreedte van uw Internet en de bestaande gegevens op het apparaat. Back-bewerkingen op het volume van het lokaal vastgemaakte zijn toegestaan terwijl het terugzetten uitgevoerd wordt.|
| Beschikbaarheid van de dunne terugzetten | Laatste failover | |
| Maximum aantal clients alleen-lezen/schrijven doorvoer (wanneer served uit de laag SSD) * | 920/720 MB/s met een enkel 10GbE netwerk interface | Maximaal 2 x met MPIO en twee netwerkinterfaces. |
| Maximum aantal clients alleen-lezen/schrijven doorvoer (indien van de harde schijf laag bediend) * | 120/250 MB/s |
| Maximum aantal clients alleen-lezen/schrijven doorvoer (wanneer served vanuit de cloud-laag) * | 11/41 MB/s | Alleen doorvoer, hangt af van clients genereren en bijhouden van voldoende i/o-wachtrij diepteas. |

& #42; Maximumdoorvoer per i/o-type werd gemeten met 100 procent lezen en schrijven van 100 procent scenario's. Werkelijke doorvoer mogelijk lager en is afhankelijk van I/O mengt en netwerk voorwaarden.
