<properties 
   pageTitle="Lokaal StorSimple vastgemaakt volumes Veelgestelde vragen over | Microsoft Azure"
   description="Vindt u antwoorden op veelgestelde vragen over StorSimple lokaal vastgemaakt volumes."
   services="storsimple"
   documentationCenter="NA"
   authors="manuaery"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/16/2016"
   ms.author="manuaery" />

# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>Lokaal StorSimple vastgemaakt volumes: veelgestelde vragen (FAQ)

## <a name="overview"></a>Overzicht

Hier volgen vragen en antwoorden hebt wanneer u een volume StorSimple lokaal vastgemaakt maken, een doorverbonden volume naar een lokaal vastgemaakte volume (en vice versa), converteren of een back-up en herstellen van een lokaal vastgemaakte volume.

Vragen en antwoorden zijn in de volgende categorieën gerangschikt

- Een lokaal vastgemaakte volume maken
- Een back-up een lokaal vastgemaakte
- Een doorverbonden volume converteren naar een lokaal vastgemaakte volume
- Een lokaal vastgemaakte volume herstellen
- Storing worden overgenomen een lokaal vastgemaakte volume

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Vragen over het maken van een lokaal vastgemaakte volume

**Q.** Wat is de maximale grootte van een lokaal vastgemaakte volume dat ik op de apparaten 8000 reeks maken kan?

**A** kunt u lokaal vastgemaakte volumes inrichten maximaal 8,5 TB of doorverbonden volumes maximaal 200 TB op het apparaat 8100. Klik op het apparaat dat groter 8600, kunt u lokaal vastgemaakte volumes inrichten maximaal 22,5 TB of doorverbonden volumes maximaal 500 TB.

**Q.** Ik mijn apparaat 8100 onlangs bijgewerkt naar Update 2 en wanneer ik wil een lokaal vastgemaakte volume maken, de maximumgrootte blijft die beschikbaar is alleen 6 TB en niet 8,5 TB. Waarom kan niet ik een volume 8,5 TB maken?

**A** kunt u lokaal vastgemaakte volumes maximaal 8.5 inrichten TB of doorverbonden volumes maximaal 200 TB op het apparaat 8100. Als uw apparaat al heeft doorverbonden volumes, is de ruimte die beschikbaar zijn voor het maken van een lokaal vastgemaakte volume proportioneel lager is dan de maximale limiet. Bijvoorbeeld als 100 TB van trapsgewijze hoeveelheden hebben al is ingericht op uw 8100 apparaat (dit is de helft van de trapsgewijze capaciteit), klikt u vervolgens de maximale grootte van een lokaal volume die u op het apparaat 8100 maken kunt dienovereenkomstig verlaagd tot 4 TB (ongeveer de helft van het maximale aantal lokaal vastgemaakt volumecapaciteit).

Omdat sommige lokale ruimte op het apparaat wordt gebruikt voor het hosten van de werkset van trapsgewijze hoeveelheden, wordt de beschikbare ruimte voor het maken van een lokaal vastgemaakte volume verminderd als het apparaat heeft volumes doorverbonden is. Daarentegen proportioneel maken van een lokaal vastgemaakte volume Hiermee reduceert u de beschikbare ruimte voor trapsgewijze volumes. De volgende tabel worden de beschikbare doorverbonden capaciteit op de apparaten 8100 en 8600 wanneer lokaal vastgemaakte volumes worden gemaakt.

|Lokaal vastgemaakte volumes ingerichte capaciteit|Beschikbare capaciteit kan worden opgenomen voor trapsgewijze volumes - 8100|Beschikbare capaciteit kan worden opgenomen voor trapsgewijze volumes - 8600|
|-----|------|------|
|0 | 200 TB | 500 TB |
|1 TB | 176,5 TB | 477.8 TB|
|4 TB | 105.9 TB | 411.1 TB |
|8.5 TB | 0 TB | 311.1 TB|
|10 TB | NB | 277.8 TB |
|15 TB | NB | 166.7 TB |
|22.5 TB | NB | 0 TB |


**Q.** Waarom wordt een langdurige bewerking lokaal vastgemaakte volume maken? 

**A.** Lokaal vastgemaakte volumes is dik ingericht. Sommige gegevens van een bestaand doorverbonden volume mogelijk om ruimte te maken op de lokale lagen van het apparaat, verplaatst naar de cloud tijdens het inrichten. En omdat deze afhankelijk van de grootte van het volume wordt ingericht, de bestaande gegevens op uw apparaat en de beschikbare bandbreedte in de cloud is, is de tijd om te maken van een lokaal volume mogelijk enkele uren.

**Q.** Hoe lang duurt het om een lokaal vastgemaakte volume te maken?

**A.** Omdat lokaal vastgemaakte volumes dik is ingericht, kan sommige bestaande gegevens uit doorverbonden worden verplaatst naar de cloud tijdens het inrichten. De tijd een lokaal vastgemaakte volume maken is dus, afhankelijk van meerdere factoren, inclusief de grootte van het volume, de gegevens op uw apparaat en de beschikbare bandbreedte. Op een vers geïnstalleerde apparaat waarop geen volumes, is de tijd voor het maken van een lokaal vastgemaakte volume ongeveer 10 minuten per terabyte van gegevens. Het maken van lokale hoeveelheden duurt echter enkele uren op basis van de factoren uitleg boven op een apparaat wordt gebruikt.

**Q.** Ik wil een lokaal vastgemaakte volume maken. Zijn er een aanbevolen procedures die ik wil weten?

**A.** Lokaal vastgemaakte volumes zijn geschikt voor werkbelasting met lokale garanties van gegevens te allen tijde vereisen gevoelige naar cloud vertragingstijden zijn. Bij het gebruik van lokale hoeveelheden naar een of meer van uw werkbelasting beoordelen, zorg rekening met het volgende:

- Lokaal vastgemaakte volumes dik is ingericht en maken van lokale volumes heeft gevolgen voor de beschikbare ruimte voor trapsgewijze volumes. Daarom wordt u aangeraden u bent begonnen met delen kleiner en als uw toename van de vereiste opslagruimte vergroten.

- Inrichten van lokale hoeveelheden is een langdurige bewerking die mogelijk gebruikmaakt van bestaande gegevens uit gelaagde drukken in de cloud. Hierdoor kunnen optreden verminderde prestaties van deze volumes.

- Inrichten van lokale hoeveelheden is erg lang duren. De werkelijke tijd betrokken, is afhankelijk van meerdere factoren: de grootte van het volume wordt ingericht, de gegevens op uw apparaat en beschikbare bandbreedte. Als u niet reservekopie uw bestaand volume in de cloud hebt gemaakt, klikt u vervolgens is volume maken langzamer afspelen. Het is raadzaam om dat u momentopnamen cloud van uw bestaande hoeveelheden voordat u een lokaal volume inrichten.
 
- U kunt een bestaand doorverbonden volume converteren lokaal vastgemaakte volume en deze conversie heeft betrekking op het inrichten van ruimte op het apparaat voor de resulterende lokaal vastgemaakte volume (naast het brengen omlaag doorverbonden gegevens [eventuele] vanuit de cloud). Klik nogmaals is dit een langdurige bewerking die is afhankelijk van factoren die eerder hebt beschreven. Het is raadzaam om back-up van uw bestaand volume vóór conversie terwijl het proces zelfs trager is als bestaand volume zijn niet back-up gemaakt. Uw apparaat tegenkomen verminderde prestaties tijdens dit proces ook.
    
Meer informatie over het [maken van een lokaal vastgemaakte volume](storsimple-manage-volumes-u2.md#add-a-volume)

**Q.** Kan ik meerdere lokaal vastgemaakte volumes op hetzelfde moment maken?

**A.** Ja, maar alle lokaal vastgemaakte volume aanmaken en uitbreiden taken sequentieel verwerkt.

Lokaal vastgemaakte volumes dik is ingericht en hiervoor is het maken van lokale ruimte op het apparaat (dit kan leiden tot bestaande gegevens uit doorverbonden hoeveelheden die moeten worden verplaatst naar de cloud tijdens het inrichten) vereist. Daarom als een inrichten uitgevoerd wordt, andere lokale volume maken van taken wordt worden in de wachtrij totdat deze taak is voltooid.

Op dezelfde manier als een bestaand lokale volume is is uitgevouwen of een doorverbonden volume wordt geconverteerd naar een lokaal vastgemaakte volume, klikt u vervolgens het maken van een nieuw lokaal vastgemaakte volume is in de wachtrij totdat de vorige taak is voltooid. Het vergroten van het volume van een lokaal vastgemaakte heeft betrekking op de uitbreiding van de bestaande lokale ruimte voor dat volume. Conversie van een doorverbonden naar lokaal vastgemaakt volume ook betrekking heeft op het maken van lokale ruimte voor het volume van de resulterende lokaal vastgemaakt. In zowel van deze bewerkingen, maken of uitbreiding van lokale ruimte wordt een lang uitgevoerd taak.

U kunt deze taken weergeven op de pagina **taken** van de Azure StorSimple Manager-service. De taak die actief wordt verwerkt is voortdurend bijgewerkt met de voortgang van de inrichting van de ruimte. De overige lokaal vastgemaakte volume-taken is gemarkeerd als actief is, maar hun voortgang is vastgelopen en ze in de volgorde waarin dat ze zijn in de wachtrij zijn verzameld.

**Q.** Ik heb verwijderd een lokaal vastgemaakte volume. Waarom zie ik niet de geregenereerde ruimte doorgevoerd in de beschikbare ruimte wanneer ik wil een nieuw volume maken? 

**A.** Als u een lokaal vastgemaakte volume verwijdert, worden de beschikbare schijfruimte voor nieuwe volumes mogelijk niet direct bijgewerkt. De Service StorSimple Manager bijgewerkt de lokale schijfruimte nagenoeg elk uur. Het is raadzaam wachten op een uur voordat u probeert te maken van het nieuwe volume.

**Q.** Ondersteunt het toestel cloud lokaal vastgemaakte volumes?

**A.** Lokaal vastgemaakte volumes worden niet ondersteund op de cloud-toestel (8010 en 8020 apparaten voorheen bekend als het virtuele apparaat StorSimple).

**Q.** Kan ik de Azure PowerShell-cmdlets gebruiken om te maken en lokaal vastgemaakte volumes beheren? 

**A.** U kunt geen Nee, lokaal vastgemaakte volumes via Azure PowerShell-cmdlets (een volume die u via Azure PowerShell maken is doorverbonden) maken. Ook het is raadzaam dat u geen de Azure PowerShell-cmdlets gebruikt voor het wijzigen van de eigenschappen van een lokaal vastgemaakte volume, zoals het ongewenste effect van het wijzigen van het volumetype heeft tot doorverbonden.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Vragen over een back-up een lokaal vastgemaakte volume

**Q.** Zijn lokale momentopnamen van lokaal vastgemaakte hoeveelheden ondersteund?

**A.** Ja, u kunt momentopnamen lokale van uw lokaal vastgemaakte hoeveelheden. Echter raden wij dat u regelmatig een back-up lokaal vastgemaakte begint met de cloud momentopnamen om ervoor te zorgen dat uw gegevens in de deze van een noodgevallen is beveiligd.

**Q.** Zijn er een richtlijnen voor het beheren van lokale momentopnamen voor lokaal vastgemaakte volumes?

**A.** Veelgebruikte lokale momentopnamen samen met een grote hoeveelheid gegevens lospeuteren in het lokaal vastgemaakte volume kunnen ertoe leiden dat lokale ruimte op het apparaat voor snel worden verbruikt, en er gegevens uit doorverbonden wordt verplaatst naar de cloud. Daarom raadzaam dat u het aantal lokale momentopnamen minimaliseren.

**Q.** Ik heb ontvangen een melding met de mededeling dat mijn lokale momentopnamen van lokaal vastgemaakte hoeveelheden mogelijk ongeldig. Wanneer kan dit gebeuren?

**A.** Veelgebruikte lokale momentopnamen samen met een grote hoeveelheid gegevens lospeuteren in het lokaal vastgemaakte volume kunnen ertoe leiden dat lokale ruimte op het apparaat te snel worden gebruikt. Als de lokale lagen van het apparaat zijn intensief wordt gebruikt, een uitgebreide cloud-storing kan leiden tot het apparaat dat u volledige en binnenkomende schrijven naar het volume kunnen leiden tot ongeldigmaking van de momentopnamen (zoals geen spatie om bij te werken de momentopnamen te verwijzen naar de oudere blokken met gegevens die overschreven). In een dergelijke situatie het schrijven naar het volume blijft worden verzonden, maar het is mogelijk dat de lokale momentopnamen ongeldig. Er is geen gevolgen voor uw bestaande cloud momentopnamen.

De waarschuwing waarschuwing is om u te waarschuwen dat een dergelijke situatie kan zich voordoen en controleer of dat u hetzelfde adres tijdig door ze te reviseren van uw lokale momentopnamen planningen naar minder veelgebruikte lokale momentopnamen of verwijderen van oudere lokale momentopnamen die niet meer nodig zijn.

Als de lokale momentopnamen zijn ongeldig, ontvangt u een melding dat de lokale momentopnamen voor de specifieke back-beleid zijn ongeldig gemaakt samen met de lijst met tijdstempel van de lokale momentopnamen die zijn ongeldig informatie-waarschuwing. Deze momentopnamen worden automatisch verwijderd en u niet langer kunt u deze in de **Back-up catalogi** pagina in de portal van Azure klassieke weergeven.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Vragen over het converteren van een doorverbonden volume naar een lokaal vastgemaakte volume

**Q.** Ik ben sommige traagheid op het apparaat naleving bij het converteren van een doorverbonden volume naar een lokaal vastgemaakte volume. Waarom wordt dit foutbericht gegeven? 

**A.** Het conversieproces bestaat uit twee stappen: 

  1. Inrichten van ruimte op het apparaat voor het snel-naar--geconverteerd lokaal vastgemaakt volume.
  2. Gelaagde gegevens downloaden uit de cloud om ervoor te zorgen lokale garandeert.

Beide stappen zijn te lang uitgevoerd bewerkingen die afhankelijk van de grootte van het volume worden omgezet zijn, gegevens op het apparaat, en de beschikbare bandbreedte. Als u bepaalde gegevens uit een bestaand doorverbonden volume mogelijk in de cloud Mors als onderdeel van het inrichten, mogelijk verminderde prestaties tijdens deze periode ervaart u uw apparaat. Bovendien het conversieproces duurt mogelijk als:

- Bestaand volume is niet reservekopie in de cloud; Zo wordt u dat u begint aangeraden alvorens een conversie tot back-up maken.

- Bandbreedte bandbreedteregeling beleidsregels zijn toegepast, waarin de beschikbare bandbreedte in de cloud; mogelijk beperken daarom aangeraden dat u een speciale 40 Mbps of meer verbinding hebt met de cloud.

- Het conversieproces kan enkele uren vanwege de meerdere factoren uitleg hierboven; duren Daarom wordt u aangeraden dat u deze bewerking niet pieken momenten of op een weekend om te voorkomen dat de invloed op consumenten einde uitvoeren.

Meer informatie over hoe u [een volume doorverbonden naar een lokaal vastgemaakte volume](storsimple-manage-volumes-u2.md#change-the-volume-type) converteren

**Q.** Kan ik de volume conversiebewerking annuleren?

**A.** Nee, u kunt niet de annuleren de bestandsconversie eenmaal gestart. Zoals in de vorige vraag besproken, worden op de hoogte van de potentiële prestatieproblemen die u mogelijk tijdens het proces optreden en volgt u de aanbevolen procedures die hierboven wordt weergegeven wanneer u van plan de conversie bent.

**Q.** Wat gebeurt er met mijn volume als de conversiebewerking mislukt?

**A.** Volumeconversie kan mislukken vanwege verbindingsproblemen met de cloud. Het apparaat loopt uiteindelijk het conversieproces na een bepaald aantal mislukte pogingen om over te brengen naar beneden doorverbonden gegevens vanuit de cloud. In een dergelijke situatie, het volumetype blijft het type bron volume vóór conversie en:

- Een melding voor een kritieke wordt melding krijgt van de fout bij het converteren van het volume worden verhoogd. Meer informatie over [meldingen gerelateerd lokaal vastgemaakte volume](storsimple-manage-alerts.md#locally-pinned-volume-alerts)

- Als u een doorverbonden naar een lokaal vastgemaakte volume converteren wilt, blijft het volume eigenschappen van een doorverbonden volume vertonen zoals gegevens mogelijk nog steeds bevinden zich op de cloud. Het is raadzaam dat u de verbindingsproblemen oplossen en probeer de conversiebewerking.
 
- Op dezelfde manier als de conversie van een lokaal vastgemaakte naar een doorverbonden volume mislukt, hoewel het volume wordt gemarkeerd als een lokaal vastgemaakte volume, werkt dit als een doorverbonden volume (omdat gegevens in de cloud terechtgekomen kan). Deze blijven echter inneemt op de lokale niveaus van het apparaat. Hier worden niet beschikbaar voor ander lokaal vastgemaakte volume. Het is raadzaam dat u opnieuw deze bewerking om ervoor te zorgen proberen dat de volumeconversie voltooid is en de lokale ruimte op het apparaat opnieuw kan worden gebruikt.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Vragen over het herstellen van een lokaal vastgemaakte volume

**Q.** Lokaal vastgemaakte volumes direct hersteld zijn?

**A.** Ja, lokaal vastgemaakte volumes worden direct hersteld. Zodra de metagegevens voor het volume is als onderdeel van het terugzetten vanuit de cloud opgehaald, wordt het volume online is opgenomen en kan worden geopend door de host. Lokale garanties met betrekking tot het volume gegevens worden niet aanwezig totdat alle gegevens is gedownload vanuit de cloud, en kunt u waarnemen beperkte echter prestaties op deze volumes voor de duur van de terugzetten.

**Q.** Hoe lang duurt het herstellen van een lokaal vastgemaakte volume?

**A.** Lokaal vastgemaakte volumes zijn direct hersteld en online overgebracht zodra de volume-metagegevens wordt opgehaald uit de cloud, terwijl de volumegegevens blijft op de achtergrond worden gedownload. Deze laatste onderdeel van het terugzetten--aan de lokale garanties weer voor de volume--een langdurige bewerking is en enkele uren duren voordat alle de gegevens worden lokale opnieuw worden gemaakt, kan duren. De tijd om te voltooien hetzelfde is afhankelijk van meerdere factoren, zoals de grootte van het volume wordt teruggezet en de beschikbare bandbreedte. Als het oorspronkelijke volume die wordt teruggezet is verwijderd, wordt extra tijd worden gehouden met het maken van de lokale ruimte op het apparaat als onderdeel van het terugzetten.

**Q.** Ik wil mijn bestaande lokaal vastgemaakte volume in een oudere momentopname (die u hebt gemaakt wanneer het volume is doorverbonden) herstellen. Het volume teruggezet zoals in dit geval doorverbonden?

**A.** Nee, het volume als een lokaal vastgemaakte volume worden hersteld. Hoewel de datums momentopname naar de tijd waarop het volume is doorverbonden, bij het herstellen van bestaand volume, StorSimple altijd het type volume op de schijf momenteel aanwezig gebruikt.

**Q.** Ik mijn lokaal vastgemaakte volume onlangs uitgebreid, maar ik wil nu herstellen van de gegevens naar een tijd waarop het volume kleiner is. Wordt herstellen de grootte van het huidige volume en heb ik nodig om uit te breiden de grootte van het volume wanneer de herstellen is voltooid?

**A.** Ja, de herstellen wordt de grootte van het volume en moet u de grootte van het volume uitbreiden nadat de herstellen is voltooid.

**Q.** Kan ik het type van een volume wijzigen tijdens herstellen?

**A.** U kunt het volumetype Nee, niet wijzigen tijdens het terugzetten.

- Als het type die zijn opgeslagen in de momentopname hersteld volumes die zijn verwijderd.

- Bestaand volume worden teruggezet op basis van hun huidige type, ongeacht de die zijn opgeslagen in de momentopname (verwijzen naar de vorige twee vragen).
 
**Q.** Ik wil mijn lokaal vastgemaakte volume herstellen, maar ik heb een onjuiste punt in een momentopname van de tijd. Kan ik het huidige terugzetten Annuleren?

**A.** Ja, kunt u een voortdurende herstellen bewerking annuleren. De status van het volume worden teruggezet naar de stand aan het begin van de terugzetten weergegeven. Een schrijft die zijn aangebracht in het volume terwijl het terugzetten goed bezig is zijn echter verloren.

**Q.** Ik heb een bewerking voor terugzetten op een van mijn lokaal vastgemaakte hoeveelheden gestart en nu zie ik een momentopname in mijn achterstallig werk catalogus die ik niet nu maken. Waar is dit voor gebruikt?

**A.** Dit is de tijdelijke momentopname die is gemaakt voordat het terugzetten en voor ongedaan maken wordt gebruikt voor het geval de terugzetten wordt geannuleerd of mislukt. Verwijder deze momentopname; Deze worden automatisch verwijderd als het herstellen voltooid is. Dit kan gebeuren als uw terugzettaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en doorverbonden hoeveelheden vastgemaakt heeft. Als de terugzettaak alleen doorverbonden volumes bevat, klikt u vervolgens plaats dit gedrag niet.

**Q.** Kan ik een lokaal vastgemaakte volume klonen?

**A.** Ja, kunt u. Het lokaal vastgemaakte volume worden echter als een doorverbonden volume al dan niet standaard worden gekopieerd. Meer informatie over hoe u [een lokaal vastgemaakte volume klonen](storsimple-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Vragen over storing worden overgenomen een lokaal vastgemaakte volume

**Q.** Ik wil mijn apparaat aan een ander fysiek apparaat mislukken. Mijn lokaal vastgemaakte volumes mislukt meer dan zoals lokaal vastgemaakt of doorverbonden?

**A.** Afhankelijk van de softwareversie van het doelapparaat, lokaal vastgemaakte volumes via mislukt als:

- Lokaal vastgemaakt als het doelapparaat wordt uitgevoerd StorSimple 8000 bijwerken reeks 2
- Als het doelapparaat wordt uitgevoerd StorSimple doorverbonden bijwerken 8000 reeks 1.x
- Als het apparaat de cloud-toestel is doorverbonden (software versie-update 2 of 1.x bijwerken)

Meer informatie over [failover en DR van lokaal vastgemaakt volumes in versies](storsimple-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**Q.** Lokaal vastgemaakte volumes direct hersteld tijdens herstel (DR)?

**A.** Ja, lokaal vastgemaakte volumes direct tijdens een overname hersteld. Zodra de metagegevens voor het volume wordt als onderdeel van de bewerking failover binnengehaald vanuit de cloud, wordt het volume online is opgenomen op het apparaat en kan worden geopend door de host. Ondertussen de volumegegevens nog steeds downloaden op de achtergrond en verminderde prestaties op deze volumes voor de duur van de overname kunnen optreden.

**Q.** Ik zie de failover-taak is voltooid, hoe kan ik de voortgang van lokaal vastgemaakte volume die wordt teruggezet bijhouden op het doelapparaat?

**A.** Tijdens een failover is de taak failover gemarkeerd als voltooid eenmaal alle in de set failover het volume zijn direct hersteld en overgebracht online op het doelapparaat. Deze groep omvat alle lokaal vastgemaakte volumes die mogelijk is mislukt lokale garanties van de gegevens worden alleen wel beschikbaar wanneer u alle gegevens voor het volume is gedownload. U kunt deze voortgang voor elk lokaal vastgemaakte volume dat door de bijbehorende herstellen taken die zijn gemaakt als onderdeel van de overname voor controle is mislukt bijhouden. Deze taken afzonderlijke herstellen wordt alleen worden gemaakt voor lokaal vastgemaakte volumes.

**Q.** Kan ik het type van een volume wijzigen tijdens een overname?

**A.** U kunt het volumetype Nee, niet wijzigen tijdens een een overname. Als u waarschijnlijk boven aan een ander fysiek apparaat met StorSimple 8000 reeks bijwerken 2, de hoeveelheden mislukt via op basis van het volumetype die zijn opgeslagen in de momentopname. Wanneer u niet een andere apparaatversie, raadpleegt u de vraag hierboven van het volumetype na een failover.

**Q.** Kan ik niet over een container volume met lokaal vastgemaakte delen op het toestel cloud?

**A.** Ja, kunt u. De lokaal vastgemaakte hoeveelheden via mislukt als doorverbonden volumes. Meer informatie over [failover en DR van lokaal vastgemaakt volumes in versies](storsimple-device-failover-disaster-recovery.md#considerations-for-device-failover)


