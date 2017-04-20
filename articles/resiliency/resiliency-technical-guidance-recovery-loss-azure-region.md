<properties
   pageTitle="Tolerantie voor herstel na verlies van de technische ondersteuning van een Azure regio | Microsoft Azure"
   description="Artikel op begrijpen en ontwerpen fouttolerante toepassingen robuuste, beschikbaar zijn, problemen, evenals planning voor herstel"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-a-region-wide-service-disruption"></a>Technische ondersteuning van Azure tolerantie: herstel uit een regio hele service-onderbreking

Azure is fysiek en logisch in eenheden, de zogenaamde regio's verdeeld. Een gebied bestaat uit een of meer datacenters dicht. Op het moment van deze schrijven heeft Azure 24 regio's overal ter wereld.

In bepaalde gevallen is het mogelijk dat faciliteiten in een hele gebied kunnen worden omgezet in toegankelijk zijn, bijvoorbeeld vanwege netwerkstoringen. Of faciliteiten kunnen verloren gaan geheel, bijvoorbeeld vanwege een natuurlijke noodgevallen. In deze sectie wordt uitgelegd van de mogelijkheden van Azure voor het maken van toepassingen die zijn verdeeld over de regio's. Deze verdeling kunt u voorkomen dat een fout in een regio kan van invloed zijn op andere regio's.

##<a name="cloud-services"></a>Cloudservices

###<a name="resource-management"></a>Resourcebeheer

U kunt berekeningscluster exemplaren verdelen over regio's door te maken van een afzonderlijke cloudservice in elke doelregio, en vervolgens het implementatiepakket publiceren naar elke cloudservice. Bedenk wel dat verkeer distribueren op cloudservices in verschillende regio's moet worden uitgevoerd door de toepassingsontwikkelaar of met een verkeer management-service.

Bepalen hoeveel exemplaren van de vrije rol om te implementeren vooraf voor herstel is een belangrijk aspect van het plannen van capaciteit. Met de implementatie van een volledige secundaire zorgt ervoor dat capaciteit al beschikbaar is als dat nodig is; echter dit effectief wordt verdubbeld de kosten. Er is een algemene patroon dat een kleine, secundaire implementatie, net groot genoeg kritieke services uit te voeren. Deze kleine secundaire implementatie is een goed idee om objecten te reserveren capaciteit, en voor het testen van de configuratie van de secundaire-omgeving.

>[AZURE.NOTE]Het abonnementenquotum voor is niet een garantie capaciteit. Het quotum is gewoon de limiet van een creditcard. Om te garanderen capaciteit, het vereiste aantal rollen moet worden gedefinieerd in het servicemodel en de rollen moeten worden geïmplementeerd.

###<a name="load-balancing"></a>Taakverdeling

Als u wilt taakverdeling vereist verkeer tussen regio's een oplossing voor het beheer van verkeer. Azure biedt [Azure verkeer Manager](https://azure.microsoft.com/services/traffic-manager/). U kunt ook profiteren van derden-services die vergelijkbaar verkeer beheermogelijkheden opgeeft.

###<a name="strategies"></a>Strategieën

Veel alternatieve strategieën zijn beschikbaar voor de uitvoering van verdeelde berekeningscluster tussen regio's. Deze moeten worden aangepast aan de specifieke bedrijfsbehoeften en omstandigheden van de toepassing. Op hoog niveau, kunnen de benaderingen wordt gedeeld in de volgende categorieën:

  * __Implementeer deze opnieuw op noodgevallen__: In deze methode de toepassing wordt geïmplementeerd maken op het moment van noodgevallen. Dit is geschikt voor niet-kritieke toepassingen die niet per keer gegarandeerd herstel vereisen.

  * __Warme vrije (actief/passieve)__: een secundaire gehoste service wordt gemaakt in een alternatieve regio en rollen zijn geïmplementeerd om te garanderen minimale capaciteit; de rollen ontvangen echter geen productie-verkeer is toegestaan. Deze methode is handig voor toepassingen die niet zijn ontworpen om verkeer verdelen over regio's.

  * __Warm vrije (actief/actief)__: de toepassing is ontworpen voor het ontvangen van productie laden in meerdere regio's. De cloudservices in elke regio is mogelijk geconfigureerd voor hoger capaciteit dan vereist voor belangrijke bestanden verloren. U kunt ook de cloudservices mogelijk schaal out zo nodig op het moment van een noodgevallen en overname bij storing. Deze methode vereist substantiële investering in het toepassingsontwerp van de, maar er grote voordelen. Hierbij lage en gegarandeerd hersteltijd, continue testen van alle herstel locaties en efficiënt gebruik van de capaciteit.

Een volledige discussie verdeelde ontwerp valt buiten het bereik van dit document. Zie [problemen oplossen en beschikbaarheid van Azure-toepassingen](https://aka.ms/drtechguide)voor meer informatie.

##<a name="virtual-machines"></a>Virtuele Machines

Herstel van infrastructuur als een service (IaaS) virtuele machines (VMs) is vergelijkbaar met platform, zoals een service (PaaS) herstel van veel respecteert berekenen. Zijn er belangrijke verschillen, maar vanwege het feit dat een VM IaaS uit de VM zowel de schijf VM bestaat.

  * __Gebruik Azure back-up maken van back-ups cross regio die toepassing consistent zijn__.
  [Back-up van Azure](https://azure.microsoft.com/services/backup/) kunnen klanten maken van back-ups toepassing consistent over meerdere VM schijven en ondersteuning voor replicatie van back-ups tussen regio's. U kunt dit doen door te kiezen om het back-kluis naar geografische repliceren op het moment van maken. Houd er rekening mee dat replicatie van de back-up kluis moet worden geconfigureerd op het moment van maken. Deze kan niet later worden ingesteld. Als u een gebied gaat verloren, stelt Microsoft de back-ups beschikbaar voor klanten. Klanten is mogelijk om te zetten naar een van de geconfigureerde herstellen wordt verwezen.

  * __Scheid de gegevensschijf van het besturingssysteem schijf worden verwijderd__. Een belangrijke overweging voor IaaS VMs is dat u de schijf besturingssysteem niet wijzigen zonder de VM opnieuw te maken. Dit is niet een probleem als uw herstelstrategie is te implementeren na noodgevallen. Deze mogelijk een probleem echter als u de warme vrije methode gebruikt om capaciteit te reserveren. Als u wilt dit correct implementeren, moet u de juiste besturingssysteem schijf geïmplementeerd in de primaire en secundaire locaties en de toepassingsgegevens moeten worden opgeslagen op een apart station. Gebruik indien mogelijk de configuratie van een standaard besturingssysteem die kan worden verstrekt op beide locaties. Na een failover, moet u vervolgens het gegevensstation koppelen aan uw bestaande IaaS VMs in de secundaire domeincontroller. Gebruik AzCopy momentopnamen van de schijven gegevens kopiëren naar een externe site.

  * __Houd rekening met mogelijke consistentie problemen na een geografische-overname van meerdere VM schijven__. VM schijven als opslag van Azure BLOB's worden geïmplementeerd en de dezelfde geografische-replicatie-kenmerk. Tenzij [Azure back-up](https://azure.microsoft.com/services/backup/) wordt gebruikt, zijn er geen garanties van consistentie over schijven, omdat geografische-replicatie asynchroon is en wordt overgenomen door onafhankelijk. Afzonderlijke VM schijven zijn gegarandeerd in een vastgelopen programma consistente provinciale na een geografische-failover, maar niet consistent over schijven. Dit kan problemen veroorzaken in sommige gevallen (bijvoorbeeld bij schijfsegmentering).

##<a name="storage"></a>Opslag

###<a name="recovery-by-using-geo-redundant-storage-of-blob-table-queue-and-vm-disk-storage"></a>Herstel met behulp van geografische-redundante opslag van blob, tabel, wachtrij en VM schijfruimte

In Azure BLOB's, tabellen, wachtrijen en VM zijn schijven alle geografische gerepliceerd al dan niet standaard. Hiermee wordt aangeduid als geografische-redundante opslag (GRS). Opslaggegevens van de GRS worden gerepliceerd naar een gepaarde datacenter honderden van mijl spreiden binnen een bepaalde geografische regio. GRS is ontworpen voor extra levensduur geval er een datacenter van de belangrijkste noodgevallen is. Microsoft besturingselementen bij een storing en overname is beperkt tot de belangrijkste systeemfouten waarin de oorspronkelijke primaire locatie is geacht onherstelbare in een redelijk tijdsbestek. In sommige gevallen kan dit zijn enkele dagen. Gegevens worden meestal gerepliceerd binnen enkele minuten, hoewel de synchronisatie-interval nog valt buiten een service level agreement.

Bij een geografische-failover zal er geen wijziging van hoe het account wordt geraadpleegd (de toets URL en account wordt niet gewijzigd). Het account opslag, echter worden in een andere regio na failover. Dit kan van invloed zijn op toepassingen die opgevolgd regionale affiniteit met hun opslag-account moeten. Zelfs voor services en toepassingen die niet nodig een account opslagruimte in het dezelfde datacenter hebben de cross-datacenter latentie en de bandbreedte kosten mogelijk worden dwingende redenen tijdelijk verkeer naar het gebied failover verplaatsen. Dit kan rekening houden met factoren een algemene noodherstelplan.

Naast het automatische failover verstrekt door GRS, heeft Azure een geïntroduceerd waarmee u alleen toegang hebt tot de kopie van uw gegevens in de opslaglocatie van secundaire. Dit wordt leestoegang geografische-redundante opslag (AB-GRS) genoemd.

Zie voor meer informatie over GRS zowel AB-GRS opslag [Azure Storage herhaling](../storage/storage-redundancy.md).

###<a name="geo-replication-region-mappings"></a>Geografische herhaling regio toewijzingen:

Het is belangrijk te weten waar uw gegevens zich geografische gerepliceerd, om te weten waar ze moeten het implementeren van de andere exemplaren van uw gegevens waarvoor regionale affiniteit met uw opslag. De volgende tabel staan de koppelingen primaire en secundaire locatie:

[AZURE.INCLUDE [paired-region-list](../../includes/paired-region-list.md)]

###<a name="geo-replication-pricing"></a>Geografische herhaling prijzen:

Geografische-replicatie is opgenomen in de huidige prijzen voor Azure opslag. Dit wordt geografische-redundante opslag (GRS) genoemd. Als u niet dat uw gegevens geografische gerepliceerd wilt kunt u geografische herhaling uitschakelen voor uw account. Heet dit lokaal redundante opslag en deze wordt geheven voor een verdisconteerd prijs vergeleken met GRS.

###<a name="determining-if-a-geo-failover-has-occurred"></a>Als een geografische-heeft plaatsgevonden vaststellen

Als een geografische-storing, wordt dit in het [Dashboard voor servicestatus: Azure](https://azure.microsoft.com/status/)wordt geplaatst. Toepassingen kunnen implementeren een geautomatiseerde betekent dit echter gedetecteerd door controleren of de geografische regio voor hun rekening opslag. Dit kan worden gebruikt voor het starten van andere herstelbewerkingen, zoals de activering van berekeningscluster resources in de geografische-de regio waar de opslag verplaatst. U kunt een query uitvoeren voor deze uit de API, voor het servicebeheer met behulp van de [Eigenschappen van opslag Account ophalen](https://msdn.microsoft.com/library/ee460802.aspx). De relevante eigenschappen zijn:

    <GeoPrimaryRegion>primary-region</GeoPrimaryRegion>
    <StatusOfPrimary>[Available|Unavailable]</StatusOfPrimary>
    <LastGeoFailoverTime>DateTime</LastGeoFailoverTime>
    <GeoSecondaryRegion>secondary-region</GeoSecondaryRegion>
    <StatusOfSecondary>[Available|Unavailable]</StatusOfSecondary>

###<a name="vm-disks-and-geo-failover"></a>VM schijven en geografische-overname bij storing

Zoals beschreven in de sectie op VM schijven, zijn er geen garanties met betrekking tot de consistentie van de gegevens over VM schijven na een failover. Om ervoor te zorgen correct van back-ups, moet een back-product zoals Data Protection Manager worden gebruikt voor een back-up maken en terugzetten toepassing.

##<a name="database"></a>Database

###<a name="sql-database"></a>SQL-Database

Azure SQL-Database biedt twee soorten herstel: geografische-terugzetten en actieve geografische-herhaling.

####<a name="geo-restore"></a>Geografische herstellen

[Geografische herstellen](../sql-database/sql-database-recovery-using-backups.md#geo-restore) is ook beschikbaar met Basic, Standard en Premium-databases. De standaard-hersteloptie krijgen wanneer de database is niet beschikbaar vanwege een incident in de regio waar uw database wordt gehost. Net zoals bij het herstellen van punt-In-tijd, geografische herstellen is afhankelijk van back-ups in geografische-redundante Azure opslag. Deze herstelt u uit de geografische gerepliceerd back-up en daarom robuuste naar de bijvoorbeeld opslag in de primaire regio. Zie [een Azure SQL-Database of failover naar een secundair herstellen](../sql-database/sql-database-disaster-recovery.md)voor meer informatie.

####<a name="active-geo-replication"></a>Actieve geografische-herhaling

[Actieve geografische-replicatie](../sql-database/sql-database-geo-replication-overview.md) is beschikbaar voor alle niveaus van de database. Dit bedoeld voor toepassingen die meer agressieve herstelvereisten hebben dan geografische herstellen kunt bieden. Actieve geografische-replicatie gebruikt, kunt u maximaal vier leesbare ontvangen maken op servers in verschillende regio's. U kunt failover naar een van de secundaire servers starten. Bovendien kan actieve geografische-replicatie worden gebruikt voor ondersteuning voor de toepassing upgrade of verplaatsing van scenario's, evenals taakverdeling voor alleen-lezen-werkbelastingen. Voor meer informatie, Zie [geografische-replicatie configureren](../sql-database/sql-database-geo-replication-portal.md) en worden [uitgevoerd met de secundaire database](../sql-database/sql-database-geo-replication-failover-portal.md). Raadpleeg [ontwerp van een toepassing voor de cloud-herstel met het actieve geografische-herhaling in SQL-Database](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [de lopende upgrades beheren van cloud-toepassingen gebruiken SQL actieve geografische-databasereplicatie](../sql-database/sql-database-manage-application-rolling-upgrade.md) voor meer informatie over het ontwerpen en implementeren-toepassingen en toepassingen upgrade zonder uitvaltijd.

###<a name="sql-server-on-virtual-machines"></a>SQL Server op virtuele Machines

Een verscheidenheid aan opties zijn beschikbaar voor herstel en beschikbaarheid van SQL Server 2012 (en later) uitgevoerd in Azure virtuele Machines. Zie [beschikbaarheid en herstel voor SQL Server in Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)voor meer informatie.

##<a name="other-azure-platform-services"></a>Andere services Azure platform

Tijdens een poging om uw cloudservice in meerdere Azure regio's uitvoeren, moet u rekening houden met de gevolgen voor elk van uw afhankelijkheden. In de volgende secties, de service-specifieke richtlijnen wordt ervan uitgegaan dat u dezelfde Azure service in een alternatieve Azure datacenter gebruiken moet. Dit houdt in zowel configuratie en gegevens-herhaling taken.

>[AZURE.NOTE]In sommige gevallen kunt deze stappen te verminderen van een storing service / regiospecifieke in plaats van een gebeurtenis hele datacenter. Vanuit het perspectief toepassing een storing service / regiospecifieke mogelijk net zo beperken en waarvoor de service tijdelijk te migreren naar een alternatieve Azure regio.

###<a name="service-bus"></a>Service Bus

Azure-Service Bus gebruikt een unieke naamruimte die wordt niet Azure regio's beslaan. De eerste behoefte staat dus voor het instellen van de benodigde service bus naamruimten in de alternatieve regio. Er zijn echter ook aandachtspunten voor de levensduur van berichten in de wachtrij. Er zijn verschillende strategieën voor het repliceren van berichten tussen Azure regio's. Zie [Aanbevolen procedures voor het isolerende toepassingen tegen Service Bus bijvoorbeeld en systeemfouten](../service-bus-messaging/service-bus-outages-disasters.md)voor een overzicht van deze strategieën herhaling en andere strategieën voor noodgevallen. Zie voor andere aandachtspunten voor de beschikbaarheid, [Service Bus (beschikbaarheid)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="app-service"></a>App-Service

Als u wilt een Azure App Service-toepassing, zoals Web Apps of Mobile-Apps, migreren naar een secundaire Azure regio, moet u een back-up van de website die beschikbaar zijn voor publicatie. Als de storing tot het hele Azure datacenter leidt, is het mogelijk dat deze FTP gebruiken om te downloaden van een recente back-up van de site-inhoud. Maakt een nieuwe app in de alternatieve regio, tenzij u dit als u wilt reserveren capaciteit eerder hebt gedaan. De site publiceren naar de nieuwe regio en breng de benodigde configuratiewijzigingen. Deze wijzigingen kunnen zijn tekenreeksen van database-verbinding of andere land / regiospecifieke-instellingen. Indien nodig, van de site SSL-certificaat toevoegen en wijzigen van de DNS CNAME-record, zodat de naam van het aangepaste domein naar de opnieuw gedistribueerde Azure Web App-URL verwijst.

###<a name="hdinsight"></a>HDInsight

De gegevens die zijn gekoppeld aan HDInsight wordt standaard opgeslagen in Azure-blobopslag. HDInsight is vereist dat een Hadoop-cluster processing MapReduce taken samen moet zich bevinden in hetzelfde gebied, als de opslagruimte rekening met de gegevens geanalyseerd. Mits u de functie geografische herhaling beschikbaar voor de opslag van Azure gebruikt, kunt u uw gegevens in de tweede regio waar de gegevens als voor een reden de primaire regio niet langer beschikbaar is is gerepliceerd kunt openen. U kunt een nieuw Hadoop-cluster maken in de regio waar de gegevens is gerepliceerd en doorgaan met het verwerken van deze. Zie voor andere aandachtspunten voor de beschikbaarheid, [HDInsight (beschikbaarheid)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="sql-reporting"></a>SQL-rapportage

Op dit moment kan herstellen van het verlies van een Azure regio is vereist meerdere SQL Reporting exemplaren in verschillende Azure regio's. Deze exemplaren SQL Reporting moeten toegang heeft tot dezelfde gegevens moet, en die gegevens een eigen herstel van plan bent een storing. U kunt ook externe back-ups van het RDL-bestand voor elk rapport behouden.

###<a name="media-services"></a>Mediaservices

Azure Media Services heeft een andere herstel methode voor het coderen en streaming. Streaming is meestal belangrijker tijdens een regionale storing. Als u wilt voorbereiden voor deze, moet u Media Services-account hebt in twee verschillende Azure regio's. De gecodeerde inhoud zich bevinden in beide regio's. Tijdens een fout, kunt u de streaming verkeer omleiden naar de alternatieve regio. Codering kan worden uitgevoerd in een Azure regio. Als codering tijdgevoelig, bijvoorbeeld tijdens het verwerken van live-gebeurtenis, moet u ervoor dat taken kunnen verzenden naar een alternatieve datacenter tijdens fouten zijn.

###<a name="virtual-network"></a>Virtuele netwerk

Configuratiebestanden vindt u de snelste manier voor het instellen van een virtueel netwerk in een alternatieve Azure regio. Nadat het virtuele netwerk is geconfigureerd in de primaire Azure regio, [de virtuele netwerkinstellingen exporteren](../virtual-network/virtual-networks-create-vnet-classic-portal.md) voor het huidige netwerk in een netwerk configuratie-bestand. In het geval van een storing in de primaire regio, [het virtuele netwerk herstellen](../virtual-network/virtual-networks-create-vnet-classic-portal.md) uit het bestand opgeslagen configuratie. Andere cloudservices, virtuele machines of cross-premises-instellingen om te werken met het nieuwe virtuele netwerk configureert.

##<a name="checklists-for-disaster-recovery"></a>Controlelijsten voor herstel

##<a name="cloud-services-checklist"></a>Cloud Services controlelijst

  1. Raadpleeg de sectie Cloudservices van dit document.
  2. Maak een noodherstelplan cross-regio.
  3. Voor-en nadelen in worden gereserveerd capaciteit in alternatieve regio's te begrijpen.
  4. Verkeer routeren hulpmiddelen gebruiken, zoals Azure verkeer Manager.

##<a name="virtual-machines-checklist"></a>Virtuele Machines controlelijst

  1. Raadpleeg de sectie virtuele Machines van dit document.
  2. Met [Azure back-up](https://azure.microsoft.com/services/backup/) maken van back-ups toepassing consistente tussen regio's.

##<a name="storage-checklist"></a>Opslag controlelijst

  1. Raadpleeg de sectie opslag van dit document.
  2. Schakel geografische-replicatie van opslag resources niet uit.
  3. Meer informatie over alternatieve regio voor geografische-herhaling in het geval van failover.
  4. Aangepaste back-strategieën voor het door de gebruiker ingestelde failover strategieën maken.

##<a name="sql-database-checklist"></a>Controlelijst voor SQL-Database

  1. Raadpleeg de sectie SQL-Database van dit document.
  2. Gebruik [Geografische-terugzetten](../sql-database/sql-database-recovery-using-backups.md#geo-restore) of [Geografische herhaling](../sql-database/sql-database-geo-replication-overview.md) waar nodig.

##<a name="sql-server-on-virtual-machines-checklist"></a>SQL Server op virtuele Machines controlelijst

  1. Raadpleeg de SQL Server in de virtuele Machines sectie van dit document.
  2. Cross-regio AlwaysOn beschikbaarheid van de groepen of spiegelen van de database te gebruiken.
  3. U kunt ook met back-up en herstellen als u wilt blob storage.

##<a name="service-bus-checklist"></a>Service Bus controlelijst

  1. Raadpleeg de sectie Service Bus van dit document.
  2. Een naamruimte Bus-Service configureren in een andere regio.
  3. Houd rekening met aangepaste herhaling strategieën voor berichten tussen regio's.

##<a name="app-service-checklist"></a>Controlelijst voor App-Service

  1. Raadpleeg de sectie App Service van dit document.
  2. Voor het behoud van site-back-ups buiten de primaire regio.
  3. Als storing gedeeltelijke, probeert op te halen van de huidige site met FTP.
  4. Plan de site naar nieuwe of bestaande site in een alternatieve gebied implementeren.
  5. Plan configuratiewijzigingen voor zowel de toepassing als de DNS CNAME-records.

##<a name="hdinsight-checklist"></a>Controlelijst voor HDInsight

  1. Raadpleeg de sectie HDInsight van dit document.
  2. Maak een nieuw Hadoop-cluster in de regio met gerepliceerde gegevens.

##<a name="sql-reporting-checklist"></a>Controlelijst SQL-rapportage

  1. Raadpleeg de sectie SQL Reporting van dit document.
  2. Voor het behoud van een alternatieve exemplaar van de SQL-rapportage in een andere regio.
  3. Voor het behoud van een afzonderlijk abonnement het doel repliceren regio.

##<a name="media-services-checklist"></a>Controlelijst voor Media Services

  1. Raadpleeg de sectie Media Services van dit document.
  2. Maak een Media Services-account in een andere regio.
  3. De inhoud van beide regio's ter ondersteuning van streaming failover coderen.
  4. Tekstcodering taken naar een andere regio in het geval van een service-onderbreking verzenden.

##<a name="virtual-network-checklist"></a>Virtuele netwerk controlelijst

  1. Raadpleeg de sectie Virtual Network van dit document.
  2. Gebruik geëxporteerd virtuele netwerkinstellingen moet opnieuw maken in een andere regio.

##<a name="next-steps"></a>Volgende stappen

In dit artikel maakt deel uit van een reeks die zijn gericht op [Azure tolerantie technische richtlijnen](./resiliency-technical-guidance.md). Het volgende artikel in deze reeks is gericht op [herstel van een on-premises implementatie-datacenter naar Azure](./resiliency-technical-guidance-recovery-on-premises-azure.md).
