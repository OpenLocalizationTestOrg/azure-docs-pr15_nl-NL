<properties
   pageTitle="Tolerantie technische richtlijnen voor het herstellen van beschadigde gegevens of per ongeluk wordt verwijderd | Microsoft Azure"
   description="Artikel met informatie over het herstellen van beschadiging van gegevens of gegevens per ongeluk verwijdering en ontwerpen fouttolerante toepassingen robuuste, beschikbaar zijn, problemen, evenals planning voor herstel"
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

#<a name="azure-resiliency-technical-guidance-recovery-from-data-corruption-or-accidental-deletion"></a>Technische ondersteuning van Azure tolerantie: herstel van beschadiging of per ongeluk wordt verwijderd

Onderdeel van een plan voor robuuste bedrijfscontinuïteit heeft een abonnement als uw gegevens wordt beschadigd of per ongeluk wordt verwijderd. Hier volgt informatie over het herstellen nadat gegevens is beschadigd of per ongeluk hebt verwijderd, vanwege toepassingsfouten of operator fout.

##<a name="virtual-machines"></a>Virtuele Machines

Als u wilt voorkomen dat uw Azure virtuele Machines (ook wel infrastructuur-als-een-service VMs genoemd) toepassingsfouten of per ongeluk wordt verwijderd, [Azure back-up](https://azure.microsoft.com/services/backup/)te gebruiken. Azure back-up kunt maken van back-ups die consistent over meerdere VM schijven zijn. Bovendien kan de back-up-kluis worden gerepliceerd in regio's te leveren herstel uit regio verlies.

##<a name="storage"></a>Opslag

Houd er rekening mee dat terwijl Azure opslag kunt u gegevens tolerantie tot en met geautomatiseerde replica's, dit niet dat voorkomt uw toepassingscode (of ontwikkelaars/gebruikers) van de gegevens te beschadigen door per ongeluk of onbedoelde verwijdering, update, enzovoort. Meer geavanceerde technieken, zoals de gegevens kopiëren naar een secundaire opslaglocatie met een controlelogboek onderhouden van gegevens leiden vlak van toepassing of gebruiker fout worden vereist. Ontwikkelaars kunnen profiteren van de blob [momentopname videomogelijkheden](https://msdn.microsoft.com/library/azure/ee691971.aspx), die alleen-lezen point-in-time momentopnamen blob inhoudsopgave kunt maken. Dit kan worden gebruikt als basis voor een oplossing gegevens beeldkwaliteit voor de opslag van Azure BLOB's.

###<a name="blob-and-table-storage-backup"></a>BLOB en opslag back-up van tabel

Hoewel BLOB's en tabellen uiterst duurzame zijn, staan altijd de huidige status van de gegevens. Herstel van ongewenste wijzigingen of verwijderen van gegevens mogelijk nodig upgegevens terugzetten naar een eerdere staat. Dit kan door te profiteren van de mogelijkheden van Azure op te slaan en bewaren van kopieën van de punt-in-tijd worden bereikt.

Voor Azure BLOB's, kunt u point-in-time back-ups met de [blob momentopname functie](https://msdn.microsoft.com/library/ee691971.aspx)uitvoeren. Voor elke momentopname, wordt alleen geheven voor de opslag vereist voor de opslag van de verschillen in de blob sinds de laatste momentopname staat. De momentopnamen zijn afhankelijk van de aanwezigheid van de oorspronkelijke blob die ze zijn gebaseerd, dus is het raadzaam een bewerking kopiëren naar een andere blob of zelfs een ander opslag-account. Dit zorgt ervoor dat de back-upgegevens correct is beschermd tegen per ongeluk wordt verwijderd. Voor Azure-tabellen, kunt u point-in-time kopieën maken naar een andere tabel of naar Azure BLOB's. Meer gedetailleerde richtlijnen en voorbeelden van back-ups maken van toepassing op gebruikersniveau van tabellen en BLOB's vindt u hier:

  * [Uw tabellen tegen toepassingsfouten beveiligen](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/05/03/protecting-your-tables-against-application-errors/)
  * [Beveiligen van uw BLOB's tegen toepassingsfouten](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/04/29/protecting-your-blobs-against-application-errors/)

##<a name="database"></a>Database

Er zijn verschillende [bedrijfscontinuïteit](../sql-database/sql-database-business-continuity.md) (back-up en herstellen) opties beschikbaar voor Azure SQL-Database. Databases kunnen worden gekopieerd met behulp van de [Kopie van de Database](../sql-database/sql-database-copy.md) -functionaliteit, of door te [exporteren](../sql-database/sql-database-export.md) en [importeren van](https://msdn.microsoft.com/library/hh710052.aspx) een SQL Server Bacpac-bestand. Databasekopie biedt transactioneel consistente resultaten, maar niet door een bacpac (via de service importeren/exporteren). Beide van de volgende opties uitvoeren als wachtrij gebaseerde services binnen het datacenter en ze momenteel bieden een SLA tijd om te voltooien.

>[AZURE.NOTE]De opties voor de database kopiëren en importeren/exporteren plaats belangrijke mate van laden op de brondatabase. Ze kunnen bronnen wordt geminimaliseerd of gebeurtenissen beperken activeren.

###<a name="sql-database-backup"></a>Back-up van SQL-Database

Back-ups punt-in-tijd voor Microsoft Azure SQL-Database worden bereikt door het [kopiëren van uw Azure SQL-database](../sql-database/sql-database-copy.md). U kunt deze opdracht om te maken van een transactioneel consistente kopie van een database op dezelfde logische databaseserver of naar een andere server. In beide gevallen wordt de databasekopie volledig functioneel en volledig onafhankelijk van de brondatabase. Elk exemplaar dat u maakt vertegenwoordigt een punt-in-tijd hersteloptie. U kunt de status van de database volledig herstellen door de naam van de nieuwe database maken met de naam van de bron-database te. U kunt ook een specifiek deel van de gegevens van de nieuwe database herstellen met behulp van Transact-SQL-query's. Zie [overzicht van bedrijfscontinuïteit met Azure SQL-Database](../sql-database/sql-database-business-continuity.md)voor meer informatie over de SQL-Database.

###<a name="sql-server-on-virtual-machines-backup"></a>SQL Server-back-up virtuele Machines

Voor SQL Server gebruikt met Azure-infrastructuur als een service virtuele machines (vaak IaaS of IaaS VMs genoemd), zijn er zijn twee opties: traditionele back-ups en logboekbestanden. Gebruik van traditionele back-ups, kunt u om te zetten naar een specifiek punt in tijd, maar het herstelproces is traag. Traditionele back-ups terugzetten moet beginnen met een eerste volledige back-up en vervolgens een back-ups die u hebt gemaakt daarna toe te passen. De tweede optie is voor het configureren van een sessie als u wilt uitstellen het herstellen van logboekbestanden (bijvoorbeeld eerst op twee uur) voor de logboekbestanden. Hierdoor wordt een venster herstellen uit de fouten die zijn aangebracht op de primaire.

##<a name="other-azure-platform-services"></a>Andere services Azure platform

Sommige services Azure platform Sla de gegevens in een door de gebruiker ingestelde opslag-account of Azure SQL-Database. Als de account of opslag resource is verwijderd of beschadigd, kan dit leiden tot ernstige fouten met de service. In dat geval is het belangrijk te bewaren back-ups waarmee u kunt deze resources opnieuw maken als ze zijn verwijderd of beschadigd zou doen.

Voor websites van Azure en Azure Mobile Services, moet u back-up maken en onderhouden van de bijbehorende databases. Voor Azure Media-Service en virtuele Machines, moet u de bijbehorende opslag van Azure-account en alle resources in dat account bijhouden. U moet voor virtuele Machines, bijvoorbeeld back-up maken en beheren van de schijven VM in Azure-blobopslag.

##<a name="checklists-for-data-corruption-or-accidental-deletion"></a>Controlelijsten voor beschadiging of per ongeluk wordt verwijderd

##<a name="virtual-machines-checklist"></a>Virtuele Machines controlelijst

  1. Raadpleeg de sectie virtuele Machines van dit document.
  2. Back-up maken en onderhouden van de schijven VM met Azure back-up (of uw eigen back-systeem met behulp van Azure-blobopslag en VHD momentopnamen).

##<a name="storage-checklist"></a>Opslag controlelijst

  1. Raadpleeg de sectie opslag van dit document.
  2. Regelmatig een back-up kritieke opslag resources.
  3. Houd rekening met de functie momentopname voor BLOB's.

##<a name="database-checklist"></a>Controlelijst voor database

  1. Raadpleeg de sectie Database van dit document.
  2. Point-in-time back-ups maken met behulp van de opdracht Database kopiëren.

##<a name="sql-server-on-virtual-machines-backup-checklist"></a>SQL Server op back-up van virtuele Machines controlelijst

  1. Raadpleeg de SQL Server in de sectie van virtuele Machines back-up van dit document.
  2. Gebruik van traditionele back-up en herstellen van technieken.
  3. Een sessie voor vertraagde logboekbestanden maken.

##<a name="web-apps-checklist"></a>Controlelijst voor Web-Apps

  1. Back-up maken en onderhouden van de gekoppelde database, indien van toepassing.

##<a name="media-services-checklist"></a>Controlelijst voor Media Services

  1. Een back-up en de bijbehorende opslagbronnen beheren.

##<a name="more-information"></a>Meer informatie

Zie voor meer informatie over de functies van de back-up maken en deze herstellen in Azure wordt aangegeven, [opslag, back-up- en herstelbestanden scenario's](https://azure.microsoft.com/documentation/scenarios/storage-backup-recovery/).

##<a name="next-steps"></a>Volgende stappen

In dit artikel maakt deel uit van een reeks die zijn gericht op [Azure tolerantie technische richtlijnen](./resiliency-technical-guidance.md). Als u meer tolerantie, herstel en beschikbaarheid resources zoekt, raadpleegt u de Azure tolerantie technische richtlijnen [aanvullende informatie](./resiliency-technical-guidance.md#additional-resources).