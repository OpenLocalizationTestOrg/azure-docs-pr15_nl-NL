<properties
   pageTitle="Overzicht van de StorSimple virtuele matrix | Microsoft Azure"
   description="Beschrijving van de StorSimple virtuele matrix, een geïntegreerde opslag-oplossing die worden beheerd opslagtaken tussen een virtueel apparaat on-premises implementatie en Microsoft Azure-cloudopslag."
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

# <a name="introduction-to-the-storsimple-virtual-array"></a>Inleiding tot de virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

Welkom bij de Microsoft Azure StorSimple virtuele matrix, een geïntegreerde opslag-oplossing die worden beheerd opslagtaken tussen een on-premises implementatie van een virtueel apparaat uitgevoerd in een hypervisor en Microsoft Azure-cloudopslag. De virtuele matrix (ook wel bekend als het StorSimple on-premises implementatie virtuele apparaat) is een efficiënte, efficiënt en eenvoudig beheerbare bestandsserver of iSCSI-serveroplossing die veel van de problemen en uitgaven die is gekoppeld aan de enterprise-opslag en gegevensbeveiliging. De virtuele matrix is met name geschikt voor externe office/tak-scenario's voor office (ROBO).

Dit overzicht ligt de nadruk op de virtuele matrix. 

- Voor een overzicht van de reeks StorSimple 8000, gaat u naar [StorSimple 8000-serie: een hybride cloud-oplossing](storsimple-overview.md). 

- Ga naar het [StorSimple Online-Help](http://onlinehelp.storsimple.com/)voor informatie over het apparaat van de reeks StorSimple 5000/7000.

De virtuele matrix ondersteunt de iSCSI of bericht blokkeren SMB protocol (Server). Het wordt uitgevoerd op de infrastructuur van uw bestaande hypervisor en biedt trapsgewijs schakelen voor de cloud, cloud back-up, snel terugzetten, itemniveau herstel, en noodgevallen herstelfuncties.

De volgende tabel worden de belangrijkste functies van de virtuele matrix.

| Functie | Virtuele matrix |
| ------- | ------------- |
|Installatievereisten | Gebruik virtualization infrastructuur (Hyper-V of VMware)|
| Beschikbaarheid | Één knooppunt |
| Totale capaciteit (inclusief de cloud) |Maximaal 64 TB best capaciteit per virtueel apparaat |
| Lokale capaciteit | 390 GB met best capaciteit 6,4 TB per virtueel apparaat (nodig voor het inrichten van 500 GB tot 8 TB schijfruimte)|
| Systeemeigen protocollen | iSCSI of SMB |
| Herstel tijd doelstelling (RTO) | iSCSI: minder dan twee minuten ongeacht de grootte |
| Herstel punt doelstelling (vrijgegeven Productieorder) | Dagelijkse back-ups en back-ups op aanvraag |
| Opslag trapsgewijs schakelen | Wordt gebruikt voor heatmap toewijzing om te bepalen welke gegevens moeten worden doorverbonden in- of uitzoomen |
| Ondersteuning | Virtualization infrastructuur ondersteund door de leverancier |
| Prestaties | Varieert afhankelijk van de onderliggende infrastructuur |
| Gegevensmobiliteit | Naar hetzelfde apparaat kunt herstellen of itemniveau herstel (bestandsserver) |
| Opslag lagen | Lokale hypervisor opslagcapaciteit en de cloud |
| Grootte delen |Doorverbonden: tot 20 TB; lokaal vastgemaakt: maximaal 2 TB |
| Volumegrootte |Doorverbonden: tot 5 TB; lokaal vastgemaakt: maximaal 500 GB |
| Momentopnamen | Vastlopen consistente |
| Itemniveau herstel | Ja; gebruikers kunnen van de waarden voor aandelen terugzetten |

## <a name="why-use-storsimple"></a>Waarom StorSimple gebruiken?

StorSimple verbindt gebruikers en -servers met Azure storage in minuten, met geen wijzigingen in de toepassing.

De volgende tabel worden enkele van de belangrijkste voordelen van de oplossing virtuele matrix bevat.

| Functie | Voordeel |
|---------|---------|
| Transparant-integratie | De virtuele matrix ondersteunt de iSCSI of het SMB-protocol. De verplaatsing van gegevens tussen de lokale laag en de cloud-laag is naadloze en transparant voor de gebruiker.|
| Kosten voor verminderde opslag | Met StorSimple, moet u voldoende lokale opslag om te voldoen aan de huidige vraag voor de meest gebruikte warm inrichten. Opslag behoeften groeien, cloud StorSimple lagen koudwatersystemen gegevens efficiënt opslag. De gegevens worden deduplicated en gecomprimeerd voor verzenden naar de cloud naar opslagvereisten en onkosten verder te beperken.|
| Vereenvoudigd opslagbeheer | StorSimple biedt centraal beheer in de cloud StorSimple Manager gebruiken voor het beheren van meerdere apparaten.| 
| Verbeterde herstel en naleving | StorSimple vergemakkelijkt sneller herstel door de metagegevens onmiddellijk herstellen en de upgegevens terugzetten naar wens. Dit betekent normaal gesproken kunnen doorgaan met minimale onderbrekingen.|
| Gegevensmobiliteit | Gegevens in de cloud doorverbonden zijn toegankelijk vanaf andere sites voor herstel en migratie. Houd er rekening mee dat u gegevens alleen in de oorspronkelijke virtuele matrix terugzetten kunt. U kunt echter noodgevallen herstelfuncties gebruiken om te zetten van de hele virtuele matrix naar een andere virtuele matrix.|

## <a name="storsimple-workload-summary"></a>StorSimple werkbelasting samenvatting

Een overzicht van de ondersteunde StorSimple werkbelasting tabelindeling hieronder.

| Scenario                | Werkbelasting              | Ondersteund |  Beperkingen                                  | Versie              |
|-------------------------|-----------------------|-----------|------------------------------------------------|----------------------|
| ROBO samenwerking      | Bestanden delen          | Ja       | Zie [beperkingen voor maximale voor bestandsserver](storsimple-ova-limits.md). <br>Zie [systeemvereisten voor ondersteunde SMB-versies](storsimple-ova-system-requirements.md).   | Alle versies      |


## <a name="workflows"></a>Werkstromen

De StorSimple virtuele matrix is vooral geschikt is voor de volgende werkstromen:

- [Beheer van cloudgebaseerde opslag](#cloud-based-storage-management)
- [Locatie-onafhankelijke back-up maken](#location-independent-backup)
- [Beveiliging en noodgevallen herstel van gegevens](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>Beheer van cloudgebaseerde opslag

U kunt de StorSimple Manager service wordt uitgevoerd in de portal van Azure klassieke gebruiken voor het beheren van gegevens die zijn opgeslagen op meerdere apparaten en op meerdere locaties. Dit is vooral handig in verdeelde tak scenario's. Houd er rekening mee dat u afzonderlijke exemplaren van de StorSimple Manager-service voor het beheren van virtuele matrices en fysieke StorSimple-apparaten moet maken. 

![beheer van cloudgebaseerde opslag](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Locatie-onafhankelijke back-up maken

Met de virtuele matrix bieden cloud momentopnamen een locatie ongeacht, point-in-time exemplaar van een volume of delen. Cloud momentopnamen zijn standaard ingeschakeld en kunnen niet worden uitgeschakeld. Alle volumes en waarden voor aandelen zijn back-up op hetzelfde moment via een enkele dagelijkse back-beleid en u aanvullende ad hoc back-ups van elk gewenst moment kunt uitvoeren.

### <a name="data-protection-and-disaster-recovery"></a>Beveiliging en noodgevallen herstel van gegevens

De virtuele matrix ondersteunt de volgende gegevens protection en noodgevallen herstel scenario's:

- **Volume of delen herstellen** : Gebruik de terugzetten als nieuwe werkstroom voor het herstellen van een volume of delen. Gebruik deze methode voor het herstellen van het hele volume of delen.
- **Itemniveau herstel** – waarden voor aandelen vereenvoudigde toegang tot recente back-ups toestaan. U kunt een afzonderlijk bestand eenvoudig herstellen van een map speciaal .backup beschikbaar in de cloud. Deze mogelijkheid herstellen is gebruiker op basis van hoeveelheid werk en niet tussenkomst is vereist.
- **Herstel** : Gebruik de uitvalbeveiligingsmogelijkheden herstellen van alle volumes of waarden voor aandelen naar een nieuwe virtuele matrix. U de nieuwe virtuele matrix maken en deze met de StorSimple Manager-service registreren en mislukt via de oorspronkelijke virtuele matrix. De nieuwe virtuele matrix wordt vervolgens wordt ervan uitgegaan dat de ingerichte resources. 

## <a name="virtual-array-components"></a>Virtuele matrixelementen

De virtuele matrix bevat de volgende onderdelen:

- [Virtuele matrix](#virtual-array) – een hybride cloud opslagapparaat op basis van een virtuele machine deze is ingericht in uw gevirtualiseerde omgeving of hypervisor.  
- Een of meer StorSimple apparaten beheren [StorSimple Manager-service](#storsimple-manager-service) – een uitbreiding van de Azure klassieke portal waarmee u kunt in een enkel webservice-interface waartoe u toegang hebt vanuit verschillende geografische locaties. U kunt de StorSimple Manager-service maken en beheren van services, weergeven en apparaten en waarschuwingen beheren en volumes, waarden voor aandelen en bestaande momentopnamen beheren.
- [Gebruikersinterface van lokale web](#local-web-user-interface) – een web gebaseerde gebruikersinterface die wordt gebruikt voor het configureren van het apparaat, zodat u kunt verbinding met het lokale netwerk maken en klikt u vervolgens het apparaat met de StorSimple Manager-service registreren. 
- [Opdrachtregel](#command-line-interface) – een Windows PowerShell-interface die u wilt een support-sessie starten op de virtuele matrix kunt gebruiken.
De volgende secties worden deze onderdelen uitgebreider beschreven en wordt uitgelegd hoe de oplossing rangschikt gegevens, wijst opslagruimte toe en opslagbeheer en gegevensbescherming vergemakkelijkt.

### <a name="virtual-array"></a>Virtuele matrix

De virtuele matrix is een oplossing voor één knooppunt opslagruimte biedt primaire opslag, communicatie met cloudopslag beheert en helpt bij de beveiliging en vertrouwelijkheid van alle gegevens die is opgeslagen op het apparaat.

De virtuele matrix is beschikbaar in een model dat is gedownload. De matrix opslag heeft een maximale capaciteit van 6,4 TB op het apparaat (met een onderliggende opslag vereiste van 8 TB) en met inbegrip van 64 TB opslagruimte cloud. 

De virtuele matrix heeft de volgende functies:

- Kosten effectieve is. Het maakt gebruik van de infrastructuur van uw bestaande virtualization en kan worden geïmplementeerd op uw bestaande Hyper-V of VMware hypervisor.
- Er bevindt zich in het datacenter en kan worden geconfigureerd als een iSCSI-server of een bestandsserver. 
- Dit is geïntegreerd met de cloud.
- Back-ups worden opgeslagen in de cloud, die u kunt herstel vergemakkelijken en vereenvoudigen itemniveau herstel (ILR). 
- U kunt updates toepassen op de virtuele matrix, net zoals u deze op een fysiek apparaat toepassen wilt.

>[AZURE.NOTE] Een virtuele matrix kunnen niet worden uitgevouwen. Het is dus belangrijk voldoende opslagruimte inrichten wanneer u het virtuele apparaat maakt. 

### <a name="storsimple-manager-service"></a>StorSimple Manager-service

Microsoft Azure StorSimple biedt een web gebaseerde gebruikersinterface (de StorSimple Manager-service) waarmee u kunt centraal beheren datacenter en cloud opslag. De service StorSimple Manager kunt u de volgende taken uitvoeren:

- Beheer meerdere StorSimple virtuele matrices van een service voor eenmalige. 
- Configureren en beheren van beveiligingsinstellingen voor apparaten met StorSimple. (Versleuteling in de cloud is afhankelijk van Microsoft Azure-API's).
- Opslag-accountreferenties en eigenschappen configureren.
- Configureren en beheren van volumes of waarden voor aandelen.
- Een back-up en herstellen van gegevens op volumes of waarden voor aandelen.
- Prestaties controleren.
- Controleer systeeminstellingen en mogelijke problemen.

De service StorSimple Manager kunt u uitvoeren van dagelijkse beheer van uw virtuele matrix.

Ga naar [de StorSimple Manager-service voor het beheren van uw apparaat StorSimple gebruiken](storsimple-manager-service-administration.md)voor meer informatie.

### <a name="local-web-user-interface"></a>Lokale web-gebruikersinterface

De virtuele matrix bevat een web gebaseerde gebruikersinterface die wordt gebruikt voor eenmalige configuratie en registratie van het apparaat met de service StorSimple Manager. U kunt deze afsluiten en opnieuw starten van de virtuele matrix, diagnostische tests uitvoeren, software bijwerken, het wachtwoord van de beheerder van apparaat wijzigen, systeemlogboeken weergeven en contact opnemen met Microsoft Support een serviceaanvraag indienen. 

Voor informatie over het gebruik van de gebruikersinterface op het web, gaat u naar [gebruiken de gebruikersinterface op het web beheren van uw StorSimple virtuele matrix](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Opdrachtregel

De opgenomen Windows PowerShell-interface kunt u een ondersteuningssessie met Microsoft Support initiëren zodat deze helpt u bij het oplossen van problemen die op uw virtuele apparaat optreden kunnen.

## <a name="storage-management-technologies"></a>Opslag management technologieën

Naast de virtuele matrix en andere onderdelen, wordt de oplossing StorSimple gebruikt de volgende softwaretechnologieën snel toegang bieden tot belangrijke gegevens, het beperken van opslag verbruik en het beschermen van gegevens die zijn opgeslagen op uw virtuele matrix:

- [Automatische opslag trapsgewijs schakelen](#automatic-storage-tiering) 
- [Lokaal vastgemaakte waarden voor aandelen en volumes](#locally-pinned-shares-and-volumes)
- [Deduplication en compressie van gegevens doorverbonden of back-up gemaakt in de cloud](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
- [De back-ups van een geplande en op aanvraag](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Automatische opslag trapsgewijs schakelen

Een nieuwe trapsgewijs om de virtuele matrix gebruikt voor het beheren van opgeslagen gegevens in de virtuele matrix en de cloud. Er zijn slechts twee niveaus: de lokale virtuele matrix en Azure cloud opslag. De StorSimple virtuele matrix wordt automatisch gegevens in de lagen op basis van een heatmap, waarin wordt bijgehouden huidige gebruik, leeftijd en relaties op andere gegevens geplaatst. Gegevens die Actiefste (gegevensbeheer) lokaal is opgeslagen, terwijl minder actieve en niet-actieve gegevens worden automatisch gemigreerd naar de cloud. (Alle back-ups zijn opgeslagen in de cloud.) StorSimple wordt aangepast en gegevens opnieuw worden ingedeeld en opslag toewijzingen als gebruikspatronen wijzigen. Meer informatie over kan bijvoorbeeld worden minder actieve na verloop van tijd. Als dit actief geleidelijk minder, is het doorverbonden af in de cloud. Als die dezelfde gegevens opnieuw geactiveerd wordt, is het doorverbonden de matrix opslag.

Gegevens voor een bepaalde doorverbonden delen of het volume is een eigen ruimte lokale laag gegarandeerd. (ongeveer 10% van de totale ingerichte ruimte voor die delen of volume). Terwijl deze wordt beperkt door de beschikbare ruimte op het apparaat dat voor die delen of het volume is virtual, zorgt u ervoor dat trapsgewijs schakelen voor één delen of volume niet worden beïnvloed door de trapsgewijs behoeften van andere waarden voor aandelen of volumes. Een zeer onoverzichtelijk werkbelasting op één delen of volume niet kan dus zo instellen dat alle werkbelasting in de cloud. 

![automatische opslag trapsgewijs schakelen](./media/storsimple-ova-overview/automatic-storage-tiering.png)

>[AZURE.NOTE] U kunt een volume als lokaal vastgemaakt opgeven, zodat de gegevens in de virtuele matrix, blijft en nooit doorverbonden naar de cloud. Ga naar het [lokaal vastgemaakt waarden voor aandelen en volumes](#locally-pinned-shares-and-volumes)voor meer informatie.

### <a name="locally-pinned-shares-and-volumes"></a>Lokaal vastgemaakte waarden voor aandelen en volumes

U kunt de juiste waarden voor aandelen en volumes als lokaal vastgemaakt maken. Deze mogelijkheid zorgt ervoor dat de gegevens die nodig zijn voor kritieke toepassingen in de virtuele matrix blijft en nooit naar de cloud doorverbonden is. Lokaal vastgemaakte waarden voor aandelen en volumes bestaan uit de volgende functies: 

- Ze zijn niet onderhevig aan cloud vertragingstijden of verbindingsproblemen.
- Ze nog steeds werken van StorSimple cloud back-up en noodgevallen herstelfuncties.

U kunt een lokaal vastgemaakte delen of volume want doorverbonden of een doorverbonden delen of herstellen als lokaal volume vastgemaakt. 

Voor meer informatie over lokaal vastgemaakte volumes, gaat u naar [gebruiken de StorSimple Manager-service voor het beheren van volumes](storsimple-manage-volumes-u2.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Deduplication en compressie van gegevens doorverbonden of back-up gemaakt in de cloud

StorSimple wordt deduplication en gegevens compressie opslagvereisten in de cloud verder te beperken. Deduplication Hiermee reduceert u de totale hoeveelheid gegevens die zijn opgeslagen doordat redundantie in de opgeslagen gegevensset. Terwijl de informatie overgaat, StorSimple de ongewijzigd gegevens, worden genegeerd en wordt alleen de wijzigingen vastgelegd. Bovendien minder StorSimple opgeslagen gegevens door te identificeren en verwijderen van dubbele gegevens. 

>[AZURE.NOTE] Gegevens die zijn opgeslagen in de virtuele matrix is niet deduplicated of gecomprimeerd. Alle deduplication en compressie vindt plaats voordat de gegevens worden verzonden naar de cloud.

### <a name="scheduled-and-on-demand-backups"></a>De back-ups van een geplande en op aanvraag

Functies voor gegevensbeveiliging StorSimple kunnen u op aanvraag back-ups maken. Daarnaast een back-standaardplanning zorgt ervoor dat de back-up dagelijks. Back-ups zijn die u hebt gemaakt in de vorm van incrementele momentopnamen, die zijn opgeslagen in de cloud. Momentopnamen waarin het opnemen van alleen de wijzigingen sinds de laatste back-up moeten worden gemaakt en snel hersteld. Deze momentopnamen mag ernstige belangrijke in noodgevallen herstel scenario's, omdat ze secundaire opslag-systemen (zoals band) vervangen, en kunnen u gegevens naar uw datacenter of naar andere sites terugzetten indien nodig.

## <a name="next-steps"></a>Volgende stappen

Informatie over het [voorbereiden van de portal virtuele matrix](storsimple-ova-deploy1-portal-prep.md).


