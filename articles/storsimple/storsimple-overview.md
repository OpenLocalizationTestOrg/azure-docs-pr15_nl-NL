<properties 
   pageTitle="Wat is StorSimple? | Microsoft Azure" 
   description="Beschrijving van StorSimple trapsgewijs schakelen, het apparaat, virtueel apparaat, services en opslagbeheer en maakt u kennis met belangrijke termen in StorSimple." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="10/05/2016"
   ms.author="v-sharos@microsoft.com"/>

# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>StorSimple 8000-serie: een hybride cloud opslag-oplossing

## <a name="overview"></a>Overzicht

Welkom bij Microsoft Azure StorSimple, een geïntegreerde opslag-oplossing die worden beheerd opslagtaken tussen on-premises implementatie-apparaten en Microsoft Azure-cloudopslag. StorSimple is een efficiënte, efficiënt en eenvoudig beheerbare SAN (SAN)-oplossing die veel van de problemen en uitgaven die is gekoppeld aan de enterprise-opslag en gegevensbeveiliging. Er wordt het, bedrijfseigen StorSimple 8000 reeks apparaat gebruikt, wordt geïntegreerd met cloudservices en biedt een aantal hulpmiddelen voor projectbeheer voor een naadloze weergave van alle enterprise-opslag, waaronder cloudopslag. (De StorSimple implementatie-gegevens die zijn gepubliceerd op de website van Microsoft Azure geldt voor alleen StorSimple 8000 reeks-apparaten. Als u een apparaat van de reeks StorSimple 5000/7000 gebruikt, gaat u naar [StorSimple Help](http://onlinehelp.storsimple.com/).)

StorSimple wordt [opslag trapsgewijs schakelen](#automatic-storage-tiering) gebruikt voor het beheren van opgeslagen gegevens over de verschillende opslagmedia. De huidige werkset wordt opgeslagen on-premises op effen staat stations (SSD), gegevens die minder vaak worden gebruikt, is opgeslagen op vaste schijven (harde schijven) en archivering gegevens is verplaatst naar de cloud. Daarnaast wordt gebruikt StorSimple deduplication en compressie verkleinen van de hoeveelheid opslagruimte die de gegevens in beslag. Ga naar [Deduplication en compressie](#deduplication-and-compression)voor meer informatie. Voor definities van andere belangrijke termen en concepten in de documentatie van de reeks StorSimple 8000 gebruikt, gaat u naar [StorSimple terminologie](#storsimple-terminology) aan het einde van dit artikel.

Met StorSimple Update 2, kunt u de juiste volumes als het *lokaal vastgemaakt* om ervoor te zorgen dat primaire gegevens lokaal op het apparaat blijft en niet in de cloud trapsgewijs identificeren. Hiermee kunt u werkbelasting die gevoelige naar cloud latentie, zoals SQL versus VM-werkbelastingen, op lokaal vastgemaakt volumes, zijn blijven de cloud gebruiken voor back-up uitvoeren. Zie voor meer informatie over lokaal vastgemaakte volumes [gebruiken de StorSimple Manager-service voor het beheren van volumes](storsimple-manage-volumes-u2.md). 

Update 2 kunt u ook StorSimple virtuele apparaten die van de laag vertragingstijden en krachtige verstrekt door Azure premium opslag gebruikmaken maken. Zie voor meer informatie over StorSimple premium virtuele apparaten, [distribueren en beheren van een virtueel apparaat StorSimple in Azure wordt aangegeven](storsimple-virtual-device-u2.md). Voor meer informatie over de opslag van Azure premium, gaat u naar [Premium opslag: High-Performance-opslag voor Azure virtuele machines werkbelasting](../storage/storage-premium-storage.md).

Naast het opslagbeheer, kunnen u op aanvraag maken met functies voor gegevensbeveiliging StorSimple geplande back-ups en deze vervolgens opslaan lokaal of in de cloud. Back-ups zijn die u hebt gemaakt in de vorm van incrementele momentopnamen, wat betekent dat ze worden gemaakt en snel hersteld. Cloud momentopnamen mag ernstige belangrijke in noodgevallen herstel scenario's, omdat ze secundaire opslag-systemen (zoals band) vervangen, en kunnen u gegevens naar uw datacenter of naar andere sites terugzetten indien nodig.

![videopictogram](./media/storsimple-overview/video_icon.png) Bekijk de video voor een snelle Inleiding tot Microsoft Azure StorSimple.

> [AZURE.VIDEO storsimple-hybrid-cloud-storage-solution]

## <a name="why-use-storsimple"></a>Waarom StorSimple gebruiken?

De volgende tabel worden enkele van de belangrijkste voordelen van Microsoft Azure StorSimple biedt.

| Functie | Voordeel |
|---------|---------|
|Transparant-integratie | Microsoft Azure StorSimple gebruikt het protocol iSCSI onzichtbaar koppelen opslagruimte van gegevens. Dit zorgt ervoor dat gegevens die zijn opgeslagen in de cloud, klikt u op het datacenter, of op externe servers wordt weergegeven op één locatie worden opgeslagen.|
|Kosten voor verminderde opslag|Microsoft Azure StorSimple toegewezen voldoende lokaal of cloudopslag om te voldoen aan de huidige vraag en breidt de mogelijkheden cloudopslag alleen indien nodig. Deze verder wordt beperkt opslagvereisten en onkosten doordat overtollige versies van dezelfde gegevens (deduplication) en met behulp van compressie.|
|Vereenvoudigd opslagbeheer|Microsoft Azure StorSimple biedt systeem beheerprogramma's die u gebruiken kunt om te configureren en beheren van gegevens die zijn opgeslagen on-premises op een externe server en in de cloud. U kunt ook back-up beheren en functies terugzetten vanuit een Microsoft Management Console (MMC)-module. StorSimple biedt een afzonderlijke, optionele interface die u gebruiken kunt om uit te breiden StorSimple management en gegevens beveiligingsservices tot inhoud die is opgeslagen op SharePoint-servers. |
|Verbeterde herstel en naleving|Microsoft Azure StorSimple hoeft niet uitgebreid hersteltijd. In plaats daarvan worden hersteld gegevens als dat nodig is. Dit betekent normaal gesproken kunnen doorgaan met minimale onderbrekingen. Bovendien kunt u beleid om back-planningen en Gegevensretentie te geven.
|Gegevensmobiliteit|Gegevens die zijn geüpload naar Microsoft Azure-cloudservices zijn toegankelijk vanaf andere sites voor herstel en migratie. Bovendien kunt u StorSimple StorSimple virtuele apparaten op virtuele machines (VMs) uitgevoerd in Microsoft Azure configureren. De VMs kunnen vervolgens virtuele apparaten gebruiken voor toegang tot gegevens in de cache voor test of herstel doeleinden.|
|Ondersteuning voor andere cloud-mailproviders |De reeks StorSimple 8000 met software-update 1 of later ondersteunt Amazon S3 met records, HP en OpenStack cloud services, evenals Microsoft Azure. (U wordt nog steeds een Microsoft Azure opslag-account nodig voor apparaat management doeleinden.) Ga naar [Wat is er nieuw in Update 1.2](storsimple-update1-release-notes.md#whats-new-in-update-12)voor meer informatie.|
|Bedrijfscontinuïteit | Update 1 of hoger biedt een nieuw migratie-functie waarmee StorSimple 5000-7000 reeks gebruikers te migreren van hun gegevens naar een StorSimple 8000 reeks-apparaat.|
|Beschikbaarheid in de Portal Azure overheid | StorSimple Update 1 of hoger is beschikbaar in de Portal van Azure overheid. Zie [Deploy uw on-premises implementatie StorSimple apparaat in de Portal voor de overheid](storsimple-deployment-walkthrough-gov.md)voor meer informatie.|
|Gegevensbescherming en beschikbaarheid | De reeks StorSimple 8000 met Update 1 of hoger ondersteunt Zone overtollige opslag (ZRS), behalve Lokaal overtollige opslag (LRS) en geografische-redundante opslag (GRS). Raadpleeg [in dit artikel op Azure Storage redundantieopties](https://azure.microsoft.com/documentation/articles/storage-redundancy/) voor ZRS details.|
|Ondersteuning voor toepassingen | U kunt met StorSimple Update 2, passende volumes als lokaal vastgemaakt identificeren. Deze mogelijkheid zorgt ervoor dat de gegevens die nodig zijn voor kritieke toepassingen niet is doorverbonden naar de cloud. Lokaal vastgemaakte volumes zijn onderhevig aan cloud vertragingstijden of verbindingsproblemen. Zie voor meer informatie over lokaal vastgemaakte volumes [gebruiken de StorSimple Manager-service voor het beheren van volumes](storsimple-manage-volumes-u2.md).|
|Lage latentie en krachtige | StorSimple Update 2 kunt u virtuele apparaten die van de krachtige, lage latentie functies van Azure premium opslag gebruikmaken maken. Zie voor meer informatie over StorSimple premium virtuele apparaten, [distribueren en beheren van een virtueel apparaat StorSimple in Azure wordt aangegeven](storsimple-virtual-device-u2.md).|

![videopictogram](./media/storsimple-overview/video_icon.png) Bekijk [deze video](https://www.youtube.com/watch?v=4MhJT5xrvQw&feature=youtu.be) voor een overzicht van de StorSimple 8000 reeks functies en voordelen.

## <a name="storsimple-components"></a>StorSimple onderdelen

De oplossing Microsoft Azure StorSimple bevat de volgende onderdelen:

- **Microsoft Azure StorSimple apparaat** – een on-premises implementatie hybride opslag-matrix met SSD en harde schijven, samen met redundante controllers en automatische failover mogelijkheden. De controllers beheren opslag trapsgewijs schakelen, het plaatsen van momenteel gebruikte (of warm) gegevens op lokale opslag (in de servers apparaat of on-premises), terwijl minder veelgebruikte gegevens verplaatsen naar de cloud.
- **StorSimple virtueel apparaat** – ook bekend als het StorSimple virtuele toestel, dit is een softwareversie van het apparaat StorSimple die wordt overgenomen door de architectuur en de meeste mogelijkheden van het apparaat fysiek hybride opslag. Het virtuele StorSimple-apparaat wordt uitgevoerd op één knooppunt in een Azure virtuele machines. Premium virtuele apparaten die van Azure premium opslag gebruikmaken, zijn beschikbaar in Update 2 en hoger.
- **StorSimple Manager-service** – een uitbreiding van de Azure klassieke portal waarmee u een apparaat StorSimple of StorSimple virtueel apparaat beheren vanuit een één webservice-interface. U kunt de StorSimple Manager-service maken en services beheren, weergeven en apparaten beheren, waarschuwingen weergeven, volumes, beheren en bekijken en beheren van back-beleid en de back-catalogus.
- **Windows PowerShell voor StorSimple** -interface met een opdrachtregel die u gebruiken kunt voor het beheren van het apparaat StorSimple. Windows PowerShell voor StorSimple bevat functies waarmee u kunnen u uw apparaat StorSimple registreren, het netwerkinterface configureren op uw apparaat, bepaalde soorten updates installeren, problemen met uw apparaat via de ondersteuningssessie en de status van het apparaat te wijzigen. Verbinding maken met de seriële console of via Windows PowerShell externe, kunt u Windows PowerShell voor StorSimple openen.
- **StorSimple van azure PowerShell-cmdlets** : een verzameling Windows PowerShell-cmdlets waarmee u taken die service level en migratie vanaf de opdrachtregel kunt automatiseren. Ga naar de [verwijzing naar cmdlets](https://msdn.microsoft.com/library/dn920427.aspx)voor meer informatie over het Azure PowerShell-cmdlets voor StorSimple.
- **StorSimple momentopname Manager** : een module waarin volume-groepen en Windows Volume schaduw kopie voor het genereren van toepassing-consistente back-ups wordt gebruikt. Bovendien kunt u StorSimple momentopname Manager maken van back-planningen en klonen of volumes herstellen. 
- **StorSimple Adapter voor SharePoint** : een hulpprogramma dat transparant breidt mogelijkheden van Microsoft Azure StorSimple opslag en gegevensbeveiliging naar SharePoint Server-farms, terwijl u StorSimple opslag weergegeven en te beheren vanaf de portal Centraal beheer van SharePoint.

Het onderstaande diagram wordt een globaal overzicht van de architectuur van Microsoft Azure StorSimple en onderdelen.

![StorSimple architectuur](./media/storsimple-overview/overview-big-picture.png)

De volgende secties worden deze onderdelen uitgebreider beschreven en wordt uitgelegd hoe de oplossing rangschikt gegevens, wijst opslagruimte toe en opslagbeheer en gegevensbescherming vergemakkelijkt. De laatste sectie bevat definities voor enkele belangrijke termen en begrippen met betrekking tot StorSimple onderdelen en hun beheer.

## <a name="storsimple-device"></a>StorSimple-apparaat

Het apparaat dat Microsoft Azure StorSimple is een on-premises implementatie hybride opslag matrix waarmee primaire opslag en iSCSI toegang tot gegevens die zijn opgeslagen op is geïnstalleerd. Het beheer van de communicatie met cloudopslag en helpt bij de beveiliging en vertrouwelijkheid van alle gegevens die is opgeslagen op de Microsoft Azure StorSimple-oplossing.

Het apparaat StorSimple bevat SSD en vaste schijven harde schijven, evenals de ondersteuning voor clustering en automatische failover. Een gedeelde processor, gedeelde opslag en twee gespiegeld controllers bevat. Elke controller biedt het volgende:

- Verbinding met een hostcomputer
- Maximaal zes netwerkpoorten verbinding maken met het lokale netwerk (LAN)
- Hardware bewaken
- Niet-vluchtige RAM geheugen (NVRAM), welke gegevens worden behouden, zelfs als power wordt onderbroken
- Cluster-hoogte bijwerken als u wilt beheren, software-updates op servers in een failovercluster zodat de updates minimale hebt of geen invloed op beschikbare service
- Cluster-service, welke functies zoals een back-enddatabase cluster, leveren beschikbaarheid en minimaliseren nadelige effecten die optreden kunnen als een harde schijf of SSD mislukt of is gehaald

Slechts één controller is actief op elk gewenst moment in tijd. Als de actieve controller mislukt, actief de tweede controller automatisch. 

Ga naar [StorSimple hardwareonderdelen en de status](storsimple-monitor-hardware-status.md)voor meer informatie.

## <a name="storsimple-virtual-device"></a>StorSimple virtueel apparaat

U kunt StorSimple gebruiken om te maken van een virtueel apparaat die wordt overgenomen door de architectuur en de mogelijkheden van het apparaat fysiek hybride opslag. Het virtuele StorSimple-apparaat (ook wel bekend als de StorSimple virtuele toestel) op één knooppunt in een Azure virtuele machines wordt uitgevoerd. (Een virtueel apparaat kan alleen worden gemaakt op een Azure virtuele machine. U kunt geen maken op een apparaat StorSimple of een on-premises implementatie-server.) 

Het virtuele apparaat heeft de volgende functies:

- Er wordt behandeld als een fysieke toestel en biedt een interface iSCSI aan virtuele machines in de cloud. 
- U kunt een onbeperkte aantal virtuele apparaten in de cloud maken en deze in- of uitschakelen Schakel indien nodig. 
- Er kan helpen simuleren on-premises omgevingen in herstel, ontwikkelen en test scenario's, en kan helpen bij itemniveau voor het ophalen van back-ups. 

Met 2 en hoger het virtuele apparaat StorSimple is beschikbaar in twee modellen: de 8010 (voorheen bekend als het model 1100) en de 8020 apparaat. Het apparaat 8010 heeft een maximale capaciteit van 30 TB. Het 8020 apparaat, dat wordt gebruikgemaakt van opslag van Azure premium, heeft een maximale capaciteit van 64 TB. (Lokale lagen, Azure premium opslag gegevens worden opgeslagen op SSD dat standaard opslag worden gegevens op de harde schijven opgeslagen.) Houd er rekening mee dat er moet een account van de opslagruimte Azure premium premium opslag gebruiken. Voor meer informatie over de premium-opslag, gaat u naar [Premium opslag: High-Performance-opslag voor Azure virtuele machines werkbelasting](../storage/storage-premium-storage.md).

Voor meer informatie over het virtuele StorSimple-apparaat, gaat u naar [distribueren en beheren van een virtueel apparaat StorSimple in Azure wordt aangegeven](storsimple-virtual-device-u2.md).

## <a name="storsimple-manager-service"></a>StorSimple Manager-service

Microsoft Azure StorSimple biedt een web gebaseerde gebruikersinterface (de StorSimple Manager-service) waarmee u kunt centraal beheren datacenter en cloud opslag. De service StorSimple Manager kunt u de volgende taken uitvoeren:

- Systeeminstellingen voor StorSimple apparaten configureren.
- Configureren en beheren van beveiligingsinstellingen voor apparaten met StorSimple.
- Cloud referenties en eigenschappen configureren.
- Configureren en beheren van volumes op een server.
- Configureer volume-groepen.
- Een back-up en herstellen van gegevens.
- Prestaties controleren.
- Controleer systeeminstellingen en mogelijke problemen.

U kunt de service StorSimple Manager gebruiken om uit te voeren van alle beheerderstaken, behalve de waarvoor systeem omlaag tijd, zoals de eerste configuratie en installeren van updates.

Ga naar [de StorSimple Manager-service voor het beheren van uw apparaat StorSimple gebruiken](storsimple-manager-service-administration.md)voor meer informatie.

## <a name="windows-powershell-for-storsimple"></a>Windows PowerShell voor StorSimple

Windows PowerShell voor StorSimple biedt een opdrachtregel-interface waarmee u kunt maken en beheren van de service Microsoft Azure StorSimple en instellen en controleren StorSimple apparaten. Het is een op Windows PowerShell gebaseerde opdrachtregel-interface met speciale cmdlets voor het beheren van uw apparaat StorSimple. Windows PowerShell voor StorSimple heeft functies waarmee u kunt:

- Een apparaat registreren.
- Configureer de netwerkinterface op een apparaat.
- Bepaalde soorten updates installeren.
- Problemen met uw apparaat via de ondersteuningssessie.
- Wijzig de status van het apparaat.

U kunt toegang tot Windows PowerShell voor StorSimple van een seriële console (op een hostcomputer is verbonden met het apparaat rechtstreeks) of extern via Windows PowerShell externe communicatie. Houd er rekening mee dat sommige Windows PowerShell voor StorSimple taken, zoals de eerste apparaatregistratie, alleen kan worden uitgevoerd op de seriële console. 

Ga naar de [Windows PowerShell gebruiken voor StorSimple voor het beheren van uw apparaat](storsimple-windows-powershell-administration.md)voor meer informatie.

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure PowerShell StorSimple-cmdlets

De StorSimple van Azure PowerShell-cmdlets zijn een verzameling Windows PowerShell-cmdlets waarmee u taken die service level en migratie vanaf de opdrachtregel kunt automatiseren. Ga naar de [verwijzing naar cmdlets](https://msdn.microsoft.com/library/dn920427.aspx)voor meer informatie over het Azure PowerShell-cmdlets voor StorSimple.

## <a name="storsimple-snapshot-manager"></a>StorSimple momentopname Manager

StorSimple momentopname Manager is een module voor Microsoft Management Console (MMC) die u gebruiken kunt om te maken consistente, point-in-time back-ups van lokale en cloud-gegevens. De module wordt uitgevoerd op een Windows Server gebaseerde-host. U kunt StorSimple momentopname Manager om het te gebruiken:

- Configureren, een back-up en volumes verwijderen.
- Configureer volume groepen om ervoor te zorgen dat de back-ups van gegevens van toepassing-consistent is.
- Back-beleid beheren zodat gegevens back-up op een vooraf ingestelde planning gemaakt en die zijn opgeslagen in een bepaalde locatie (lokaal of in de cloud).
- Hoeveelheden en afzonderlijke bestanden terugzetten.

Back-ups worden vastgelegd als momentopnamen, dat alleen de wijzigingen vastgelegd sinds de laatste momentopname is gemaakt en uiterst minder opslagruimte dan volledige back-ups nodig hebben. U kunt maken van back-planningen of direct back-ups naar wens. Bovendien kunt u StorSimple momentopname Manager tot stand brengen van bewaarbeleid dat besturingselement hoeveel momentopnamen worden opgeslagen. Als u later wilt terugzetten van gegevens uit een back-up en StorSimple momentopname Manager kunt u vanuit de catalogus van lokale of cloud momentopnamen. 

Als er een noodgevallen optreedt of als u wilt herstellen van gegevens voor een andere reden, weer StorSimple momentopname Manager stapsgewijs als dat nodig is. Gegevens herstellen is niet vereist dat u het hele systeem afsluiten terwijl u een bestand herstellen, apparatuur vervangen of bewerkingen naar een andere site verplaatsen.

Ga voor meer informatie naar [Wat is StorSimple momentopname Manager?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>StorSimple Adapter voor SharePoint

Microsoft Azure StorSimple bevat de StorSimple-Adapter voor SharePoint, een optioneel onderdeel dat transparant breidt de mogelijkheden StorSimple beveiligingsfuncties opslagcapaciteit en de gegevens naar SharePoint server-farms. De adapter werkt met een externe Blob Storage (RBS)-provider en de functie SQL Server Resourcestructuur, zodat u BLOB's verplaatsen naar een back-up gemaakt door het systeem Microsoft Azure StorSimple server. Microsoft Azure StorSimple vervolgens de gegevens zijn opgeslagen BLOB lokaal of in de cloud, op basis van gebruik.

De Adapter StorSimple voor SharePoint wordt beheerd vanuit de portal Centraal beheer van SharePoint. Daarom SharePoint-beheer blijft gecentraliseerde en alle opslag wordt weergegeven in de SharePoint-farm.

Ga naar [StorSimple Adapter voor SharePoint](storsimple-adapter-for-sharepoint.md)voor meer informatie. 

## <a name="storage-management-technologies"></a>Opslag management technologieën
 
Naast het specifieke StorSimple-apparaat, virtueel apparaat en andere onderdelen, Microsoft Azure StorSimple gebruikt de volgende softwaretechnologieën voor snelle toegang tot gegevens en opslag verbruik verkleinen:

- [Automatische opslag trapsgewijs schakelen](#automatic-storage-tiering) 
- [Dunne inrichting](#thin-provisioning) 
- [Deduplication en compressie](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Automatische opslag trapsgewijs schakelen

Microsoft Azure StorSimple rangschikt automatisch gegevens in logische lagen op basis van de huidige gebruik, leeftijd en relatie op andere gegevens. Gegevens die Actiefste is die lokaal zijn opgeslagen, terwijl minder actieve en niet-actieve gegevens worden automatisch gemigreerd naar de cloud. In het volgende diagram ziet u deze methode opslag.
 
![StorSimple opslag lagen](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Voor snelle toegang, slaat StorSimple zeer actieve gegevens (warm gegevens) op SSD in het StorSimple-apparaat. Gegevens die worden gebruikt in sommige gevallen wordt opgeslagen (warme gegevens) op de harde schijven in het apparaat of op servers van het datacenter. Inactieve gegevens, back-upgegevens, wordt dit overgezet en gegevens behouden voor archivering of naleving doeleinden in de cloud. 

>[AZURE.NOTE] In Update 2 of hoger gebruikt, kunt u een volume als lokaal vastgemaakt, zodat de gegevens blijven op het lokale apparaat staan, en niet is doorverbonden naar de cloud. 

StorSimple wordt aangepast en gegevens opnieuw worden ingedeeld en opslag toewijzingen als gebruikspatronen wijzigen. Meer informatie over kan bijvoorbeeld worden minder actieve na verloop van tijd. Als dit actief geleidelijk minder, is deze gemigreerd van SSD naar harde schijven en vervolgens naar de cloud. Als die dezelfde gegevens opnieuw geactiveerd wordt, is het terug naar het opslagapparaat gemigreerd.

De opslagruimte trapsgewijs proces verloopt als volgt:

1. Een beheerder is ingesteld van een Microsoft Azure cloud opslag-account.
2. De beheerder van het gebruik van de seriële console en de StorSimple Manager-service (wordt uitgevoerd in de portal van Azure klassieke) voor het configureren van de server apparaat en de bestandsnaam maken van beleidsregels voor de beveiliging hoeveelheden en gegevens. On-premises implementatie machines (zoals bestandsservers) Gebruik de kleine Computer systeem iSCSI (Internet Interface) voor toegang tot het StorSimple-apparaat.
3. In eerste instantie StorSimple gegevens worden opgeslagen op de fast SSD laag van het apparaat.
4. Als de laag SSD capaciteit nadert, StorSimple deduplicates worden gecomprimeerd de oudste gegevensblokken en wordt deze verplaatst naar de harde schijf laag.
5. Als de harde schijf laag benaderingen capaciteit, StorSimple versleutelt de oudste gegevensblokken en veilig doorgestuurd naar het Microsoft Azure-opslag-account via HTTPS.
6. Microsoft Azure Hiermee maakt u meerdere kopieën van de gegevens in de datacenter en in een externe datacenter, ervoor te zorgen dat de gegevens kunnen worden hersteld als een noodgevallen zich daadwerkelijk voordoet. 
7. Als de bestandsserver gegevens die zijn opgeslagen in de cloud aanvraagt, StorSimple geretourneerd naadloos worden opgeslagen en een kopie op de laag SSD van het apparaat StorSimple.

### <a name="thin-provisioning"></a>Dunne inrichting

Dunne inrichting is een virtualization-technologie waarin beschikbare opslagruimte overschrijdt fysieke bronnen wordt weergegeven. In plaats van voldoende opslagruimte tevoren reserveren, StorSimple gebruikt dunne inrichting net voldoende ruimte om te voldoen aan vereisten voor huidige toe te wijzen. De elastische aard van cloudopslag vergemakkelijkt deze methode omdat StorSimple kunt verhogen of cloudopslag verlagen om aan veranderende voldoen. 

>[AZURE.NOTE] Lokaal vastgemaakte volumes zijn niet dun deze is ingericht. Opslagruimte toegewezen aan het volume van een lokaal alleen-lezen is ingericht in zijn geheel wanneer het volume is gemaakt.

### <a name="deduplication-and-compression"></a>Deduplication en compressie

Microsoft Azure StorSimple wordt deduplication en gegevens compressie opslagvereisten verder te beperken.

Deduplication Hiermee reduceert u de totale hoeveelheid gegevens die zijn opgeslagen doordat redundantie in de opgeslagen gegevensset. Terwijl de informatie overgaat, StorSimple de ongewijzigd gegevens, worden genegeerd en wordt alleen de wijzigingen vastgelegd. Bovendien minder StorSimple opgeslagen gegevens door te identificeren en onnodige gegevens verwijderen. 

>[AZURE.NOTE] Gegevens op lokaal vastgemaakte volumes is niet deduplicated of gecomprimeerd. Back-ups van lokaal vastgemaakte hoeveelheden zijn echter deduplicated en gecomprimeerd.

## <a name="storsimple-workload-summary"></a>StorSimple werkbelasting samenvatting

Een overzicht van de ondersteunde StorSimple werkbelasting tabelindeling hieronder.

| Scenario                  | Werkbelasting                | Ondersteund |  Beperkingen                                  | Versie              |
|---------------------------|-------------------------|-----------|------------------------------------------------|----------------------|
| Samenwerking             | Bestanden delen            | Ja       |                                                | Alle versies         |
| Samenwerking             | Verdeelde bestandsdeling| Ja       |                                                | Alle versies         |
| Samenwerking             | SharePoint              | Ja *      |Ondersteund alleen met lokaal vastgemaakte delen      | Update 2 en hoger   |
| Archivering                  | Eenvoudig bestanden archiveren   | Ja       |                                                | Alle versies         |
| Virtualization            | Virtuele machines        | Ja *      |Ondersteund alleen met lokaal vastgemaakte delen      | Update 2 en hoger   |
| Database                  | SQL                     | Ja *      |Ondersteund alleen met lokaal vastgemaakte delen      | Update 2 en hoger   |
| Video toezicht        | Video toezicht      | Ja *       |Ondersteund wanneer StorSimple apparaat alleen op deze werkbelasting streeft| Update 2 en hoger   |
| Back-up maken                    | Primaire doel back-up maken | Ja *      |Ondersteund wanneer StorSimple apparaat alleen op deze werkbelasting streeft| Update 3 en hoger |
| Back-up maken                    | Secundaire doel back-up maken | Ja *      |Ondersteund wanneer StorSimple apparaat alleen op deze werkbelasting streeft| Update 3 en hoger |

*Ja & #42; -Oplossing richtlijnen en beperkingen moeten worden toegepast.*

De volgende werkbelasting worden niet ondersteund door StorSimple 8000 reeks apparaten. Als op StorSimple geïmplementeerd, resulteert deze werkbelasting in een niet-ondersteunde configuratie.

-  Medisch installatiekopieën
-  Exchange
-  VDI
-  Oracle
-  SAP:
-  Grote gegevens
-  Distributie van inhoud
-  Opstarten vanuit SCSI

Hier volgt een lijst met de onderdelen van de infrastructuur StorSimple ondersteund. 

| Scenario | Werkbelasting      | Ondersteund |  Beperkingen                                 | Versie      |
|----------|---------------|-----------|-----------------------------------------------|--------------|
| Algemene  | Route Express | Ja       |                                               |Alle versies |
| Algemene  | DataCore FC   | Ja *       |Ondersteund met DataCore SANsymphony            | Alle versies |
| Algemene  | DFSR          | Ja *      |Ondersteund alleen met lokaal vastgemaakte delen     | Alle versies |
| Algemene  | Indexeren      | Ja *       |Gelaagde volumes, alleen metagegevens indexeren wordt ondersteund (geen gegevens).<br>Voor lokaal vastgemaakte volumes, wordt voltooid indexeren ondersteund.| Alle versies |
| Algemene  | Anti-virus uitvoert    | Ja *       |Voor trapsgewijze volumes, wordt alleen scan voor openen en sluiten ondersteund.<br> Voor lokaal vastgemaakte volumes, wordt volledige scan ondersteund.| Alle versies |

*Ja & #42; -Oplossing richtlijnen en beperkingen moeten worden toegepast.*

## <a name="storsimple-terminology"></a>StorSimple terminologie 

Voordat u uw Microsoft Azure StorSimple-oplossing implementeert, wordt u aangeraden in de volgende termen en definities te controleren.

### <a name="key-terms-and-definitions"></a>Belangrijke termen en definities

| Term (acroniem of de afkorting) | Beschrijving |
| ------------------------------ | ---------------- |
| Access control-record (ACR)    | Een record die is gekoppeld aan een volume op uw apparaat met Microsoft Azure StorSimple die bepaalt welke hosts verbinding kunnen maken. De bepaling is gebaseerd op de iSCSI Qualified Name (IQN) van de hosts (die zich in de ACR) die zijn verbonden met uw apparaat StorSimple.|
| AES-256                        | Een algoritme 256-bits geavanceerde versleuteling Standard (AES) voor het coderen van gegevens zoals deze wordt verplaatst naar en vanuit de cloud. |
| grootte van de toewijzing (Australië)     | De kleinste hoeveelheid schijfruimte die kan worden toegewezen voor het opslaan van een bestand van uw Windows-systemen bestand. Als een de bestandsgrootte bevindt zich niet in een veelvoud van het formaat van een cluster, extra ruimte moet worden gebruikt voor het opslaan van het bestand (snel aan de volgende veelvoud is van de clustergrootte) met resultaat verloren ruimte en fragmentatie van de harde schijf. <br>De aanbevolen Australië voor Azure StorSimple volumes is 64 KB omdat deze functie ook met de algoritmen deduplication werkt.|
| geautomatiseerde opslag trapsgewijs schakelen      | Minder actieve gegevens automatisch verplaatsen van SSD naar harde schijven en vervolgens naar een laag in de cloud en vervolgens beheer van alle opslag vanuit een centrale gebruikersinterface inschakelen.|
| back-catalogus | Een verzameling back-ups, meestal gerelateerd door het toepassingstype dat is gebruikt. Deze siteverzameling wordt weergegeven in de catalogus met back-up-pagina van de service StorSimple Manager UI.|
| met back-catalogusbestand             | Een bestand met een lijst met beschikbare momentopnamen die momenteel zijn opgeslagen in de back-up van StorSimple momentopname Manager-database. |
| back-beleid                   | Een selectie van volumes, type back-up en een tijdschema waarmee u back-ups maken op een vooraf gedefinieerde planning.|
| binaire grote objecten (BLOB's)    | Een verzameling binaire gegevens die zijn opgeslagen als een eenheid in een database managementsysteem. BLOB's zijn meestal afbeeldings-, audio- of andere objecten multimedia, hoewel soms binaire uitvoerbare code wordt opgeslagen als een BLOB.|
| Vraag handshakeverificatieprotocol (CHAP) | Een protocol dat wordt gebruikt om de peer van een verbinding, op basis van het delen van een wachtwoord of geheim peer te verifiëren. CHAP kunt werken in één richting of onderlinge. Met één richting CHAP, wordt het doel geverifieerd een initiator. Onderlinge CHAP vereist dat het doel het beginpunt verifiëren en dat het beginpunt moeten worden geverifieerd het doel. | 
| klonen                          | Een kopie van een volume. |
|Cloud als een laag (CaaT)          | Cloud opslag geïntegreerd als een laag binnen de opslagarchitectuur, zodat alle opslag wordt weergegeven wilt toevoegen aan één opslag van het bedrijfsnetwerk.|
| (cloud cryptografieprovider)   | Een provider van cloud computing services.|
| momentopname van de cloud                 | Een kopie van de punt-in-tijd van de volumegegevens die zijn opgeslagen in de cloud. Een momentopname cloud is gelijk aan een momentopname gerepliceerd op een andere, externe opslagsysteem. Cloud momentopnamen zijn met name handig voor noodgevallen herstel scenario's.|
| cloud opslag versleuteling-toets   | Een wachtwoord of een toets wordt gebruikt door uw apparaat StorSimple voor toegang tot de versleutelde gegevens door uw apparaat verzonden naar de cloud.|
| Houd rekening met cluster bijwerken         | Software-updates op servers in een failovercluster beheren, zodat de updates minimale hebt of geen invloed op de servicebeschikbaarheid van de.|
| DataPath                       | Een verzameling functionele eenheden die onderling verbonden gegevensverwerking bewerkingen uitvoeren.|
| deactiveren                     | Een permanente actie die de verbinding tussen het apparaat StorSimple en de bijbehorende cloudservice eindemarkeringen. Cloud momentopnamen van het apparaat kunnen blijven na dit proces en worden gekopieerd of die wordt gebruikt voor problemen oplossen.|
| schijf spiegelen                 | Replicatie logische schijfvolume met op afzonderlijke vaste schijven in realtime zodat deze zeker continue beschikbaar.|
| dynamische schijf spiegelen         | Replicatie van logische schijf hoeveelheden op dynamische schijven.|
| dynamische schijven                  | Een indeling van de schijf volume die gebruikmaakt van de logische schijf Manager (LDM) wilt opslaan en beheren van gegevens op meerdere fysieke schijven. Dynamische schijven kunnen worden vergroot meer ruimte vrij te geven.|
| Uitgebreide bundel van schijven (EBOD) insluiting | Een secundaire insluiting van uw apparaat met Microsoft Azure StorSimple die extra harde schijf schijven voor extra opslagruimte bevat.|
| vet inrichting               | Een gebruikelijke opslag inrichting in welke opslag ruimte is toegewezen op basis van behoeften verwacht (en is meestal voorbij de huidige nodig). Zie ook *dunne inrichten*.|
| harde schijf (harde schijf)          | Een station dat draaiende platen gebruikt voor het opslaan van gegevens.|
| hybride cloudopslag           | Een opslagarchitectuur dat lokale en externe resources, inclusief cloudopslag gebruikt.|
| Internet Small Computer System Interface (iSCSI) | Een Internetprotocol – gebruikgemaakt van opslag netwerken standaard voor het koppelen van gegevens opslagapparatuur of faciliteiten.|
| iSCSI-initiator                 | Een softwareonderdeel waarmee een hostcomputer met Windows verbinding maken met een externe iSCSI gebruikgemaakt van opslag-netwerk.|
| iSCSI Qualified Name (IQN)      | Een unieke naam waarmee een iSCSI-doel of initiator.|
| iSCSI-doel                    | Een softwareonderdeel waarmee gecentraliseerde iSCSI schijfsubsystemen in SAN's.|
| Live archiveren                  | Een opslag aanpak waarin archivering gegevens zijn toegankelijk over altijd (deze is niet opgeslagen op een andere locatie op tape, bijvoorbeeld). Microsoft Azure StorSimple maakt gebruik van live archiveren.|
|lokaal vastgemaakte volume | een volume die zich bevindt op het apparaat en nooit is doorverbonden naar de cloud. |
| lokale momentopname                  | Een kopie van de punt-in-tijd van de volumegegevens die zijn opgeslagen op Microsoft Azure StorSimple apparaat.|
| Microsoft Azure StorSimple      | Een krachtige oplossing bestaande uit een datacenter opslag toestel en software waarmee IT-organisaties om te profiteren van cloudopslag alsof het datacenter opslag. StorSimple vergemakkelijkt gegevensbescherming en beheren van gegevens tijdens het kostenverlaging. De oplossing worden samengevoegd primaire opslag, archiveren, back-up en noodgevallen herstel (DR) tot en met naadloze integratie met de cloud. StorSimple apparaten inschakelen door te combineren SAN opslagruimte en cloud beheren van gegevens op een enterprise-klasse-platform, snelheid, eenvoudig en betrouwbaarheid voor alle opslag-gerelateerde behoeften.|
| Power en koeling Module (PCM)  | De hardwareonderdelen van uw apparaat StorSimple die bestaat uit de voorraad power en de koelventilator, dus de naam Power en koeling module. De primaire insluiting van het apparaat heeft twee 764W PCMs terwijl de insluiting EBOD twee 580 w bij PCMs heeft.|
| primaire insluiting               | Belangrijkste insluiting van uw apparaat StorSimple dat de toepassing platform domeincontrollers bevat.|
| herstel tijd doelstelling (RTO)   | De maximale hoeveelheid tijd die moet worden beide voordat u een bedrijfsproces of systeem volledig na een noodgevallen is hersteld.| 
|seriële bijgevoegd SCSI (SA's)       | Een type harde schijf (harde schijf).|
| sleutel service data-versleuteling     | Een sleutel beschikbaar gemaakt voor elk nieuw StorSimple-apparaat, dat wordt geregistreerd met de service StorSimple Manager. De configuratiegegevens worden overgedragen tussen de StorSimple Manager-service en het apparaat is versleuteld met een openbare sleutel en vervolgens alleen op het apparaat met een persoonlijke sleutel kan worden ontsleuteld. Service gegevens versleutelingssleutel kan de service voor deze persoonlijke sleutel voor ontsleutelen.|
| Service registratiegegevens sleutel        | Een sleutel die helpt bij het apparaat StorSimple met de service StorSimple Manager registreren, zodat het wordt weergegeven in de portal van Azure klassieke voor verdere management acties.|
| Small Computer System Interface (SCSI) | Een reeks standaarden voor fysiek verbinding computers en het doorgeven van gegevens.|
| effen state harde schijf (SSD)         | Een schijf waarop geen bewegende onderdelen; bijvoorbeeld, een flashstation.|
| opslag-account                 | Een reeks-referenties die zijn gekoppeld aan uw account opslagruimte voor een bepaald cloud-provider.| 
| StorSimple Adapter voor SharePoint| Een Microsoft Azure StorSimple-onderdeel dat transparant breidt de mogelijkheden StorSimple opslag en gegevensbeveiliging naar SharePoint server-farms.|
| StorSimple Manager-service      | Een uitbreiding van de Azure klassieke portal waarmee u uw Azure StorSimple on-premises implementatie en virtuele apparaten beheren.|
| StorSimple momentopname Manager     | Een Microsoft Management Console (MMC)-module voor het beheren van back-up en herstellen bewerkingen in Microsoft Azure StorSimple.|
| back-up maken                     | Een functie waarmee de gebruiker moet een interactieve back-up van een volume uitvoeren. Dit is een andere manier een handmatige back-up van een volume in plaats van een automatische back-up via een gedefinieerd beleid duurt het duurt.|
| dunne inrichting               | Een methode voor het optimaliseren van de efficiëntie waarmee de beschikbare opslagruimte wordt gebruikt in systemen voor opslag. In de dunne is geïnstalleerd, wordt de opslag verdeeld over meerdere gebruikers op basis van de minimaal vereiste door elke gebruiker op elk gewenst moment schijfruimte. Zie ook *vet inrichten*.|
| trapsgewijs schakelen | Het ordenen van gegevens in logische groepen, op basis van de huidige gebruik, leeftijd en relatie op andere gegevens. StorSimple rangschikt automatisch gegevens in de lagen.   |
| volume                          | Logische opslagplaatsen gepresenteerd in de vorm van stations. StorSimple volumes overeenkomen met de hoeveelheden door de host, inclusief de computers ontdekt door middel van iSCSI en een apparaat StorSimple gekoppeld.|
 | volume container                | Een groepering van volumes en de instellingen die toe te passen. Enkel volume van uw apparaat StorSimple zijn in volume containers gegroepeerd. Volume container-instellingen zijn opslag accounts, versleutelingsinstellingen voor gegevens die zijn verzonden naar cloud met bijbehorende versleuteling toetsen en bandbreedte dat voor bewerkingen die betrekking hebben op personen de cloud.|
| volume-groep                    | Ga in StorSimple momentopname Manager een volume-groep is een verzameling van hoeveelheden geconfigureerd voor het back-verwerking vergemakkelijken.|
| Volume schaduw Copy-Service (VSS)| Een service van de Windows Server-besturingssysteem die zorgt voor een toepassing consistente door te communiceren met VSS-compatibele toepassingen kunnen het maken van incrementele momentopnamen coördineren. VSS zorgt ervoor dat de toepassingen tijdelijk inactief zijn wanneer momentopnamen worden.|
| Windows PowerShell voor StorSimple | Op basis van de Windows PowerShell-interface met een opdrachtregel gebruikt om te werken en uw apparaat StorSimple beheren. Behoud van enkele van de eenvoudige mogelijkheden van Windows PowerShell, heeft dit interface aanvullende speciale cmdlets die zijn gericht op een apparaat StorSimple beheren.|


## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple beveiliging](storsimple-security.md).




 

 
