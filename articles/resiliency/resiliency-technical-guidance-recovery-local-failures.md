<properties
   pageTitle="Technische ondersteuning: herstel van lokale fouten in Azure | Microsoft Azure"
   description="Klik op lidmaatschap en ontwerpen van robuuste, ten zeerste beschikbaar, fouttolerantie toepassingen, evenals planning voor herstel die zijn gericht op een lokale fouten binnen Azure-artikel."
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

#<a name="azure-resiliency-technical-guidance-recovery-from-local-failures-in-azure"></a>Technische ondersteuning van Azure tolerantie: herstel van lokale fouten in Azure wordt aangegeven

Er zijn twee primaire threats toepassing beschikbaar:

* De fout apparaten, zoals stations en servers
* De uitputting van kritieke resources, zoals berekeningscluster piek laden omstandigheden

Azure biedt een combinatie van resourcebeheer, elasticiteit taakverdeling en partitioneren om in te schakelen beschikbaarheid in deze omstandigheden. Sommige van deze functies worden automatisch uitgevoerd voor alle Azure-services. In sommige gevallen doet de ontwikkelaar van toepassingen echter enkele extra werk profiteren.

##<a name="cloud-services"></a>Cloudservices

Azure Cloudservices bestaat uit verzamelingen van een of meer web of werknemer rollen. Een of meer exemplaren van een rol kunnen tegelijkertijd worden uitgevoerd. De configuratie bepaalt het aantal exemplaren. Rol exemplaren worden gecontroleerd en beheerd via een onderdeel dat de controller stof genoemd. De controller stof worden gedetecteerd en automatisch moet reageren op software-en hardware.

Elk exemplaar van de rol wordt uitgevoerd in een eigen virtuele machine (VM) en communiceert met een controller stof via een agent Gast. De Gast-agent verzamelt resource en knooppunt aan de doelstellingen, inclusief VM gebruik, status, Logboeken, Resourcegebruik, uitzonderingen en voorwaarden is mislukt. De controller stof de Gast-agent configureerbare tijdsintervallen query's en de VM wordt gestart als de Gast-agent niet meer reageert. Bij hardwarestoring wordt de bijbehorende stof controller alle exemplaren van de desbetreffende rol verplaatst naar een nieuw hardware knooppunt en geconfigureerd van het netwerk er route-verkeer is toegestaan.

Als u wilt profiteren van deze functies, ontwikkelaars Zorg ervoor dat alle service rollen Bewaar staat op de exemplaren van de rol. In plaats daarvan moeten alle permanente gegevens zijn toegankelijk vanaf duurzame opslag, zoals Azure Storage of Azure SQL-Database. Hiermee kunt rollen om aanvragen te verwerken. Het betekent ook dat rol exemplaren kunnen omlaag gaan op elk gewenst moment zonder inconsistenties in de tijdelijke of permanente status van de service.

De vereiste om op te slaan staat extern aan de rollen heeft verschillende gevolgen. Dit houdt in, bijvoorbeeld dat alle gerelateerde wijzigingen aan een tabel Azure Storage moeten worden gewijzigd in één entiteit-groep transactie, indien mogelijk. Natuurlijk, is het niet altijd mogelijk alle wijzigingen aanbrengen in één transactie. U moet letten om ervoor te zorgen dat rol exemplaar fouten geen problemen veroorzaken wanneer ze onderbreken langdurige bewerkingen die twee of meer updates voor de permanente status van de service's beslaan. Als een andere rol probeert die bewerking opnieuw uit te voeren, moet deze anticiperen en verwerken waar het werk dat gedeeltelijk is voltooid.

Stel een service die gegevens over meerdere winkels partities. Als een rol werknemer uitvalt terwijl deze wordt een shard verplaatsen, is het mogelijk dat de verplaatsing van de shard niet gereed. Of de verplaatsing mogelijk worden herhaald vanaf het begin van een andere werknemer rol, mogelijk veroorzaakt door zwevende gegevens of gegevens beschadigde bestanden. Voorkom problemen moet langdurige bewerkingen een of beide van de volgende opties:

 * *Idempotency is ingeschakeld*: herhaald zonder bijwerkingen. Als u wilt idempotency is ingeschakeld, moet een bewerking langdurige hetzelfde effect ongeacht hoe vaak deze wordt uitgevoerd, zelfs wanneer deze wordt onderbroken tijdens het uitvoeren van hebben.
 * *Stapsgewijs opnieuw starten*: kunnen blijven van de meest recente punt risico. Als u stapsgewijs opnieuw starten, moet een bewerking langdurige bestaan uit een reeks kleinere atomaire bewerkingen. Dit moet ook de voortgang in duurzame opslag, record, zodat elke volgende aanroep hervat waar de voorafgaande taak is gestopt.

Tot slot moeten alle langdurige bewerkingen worden aangeroepen herhaaldelijk drukken totdat ze worden voltooid. Bijvoorbeeld een inrichten bewerking mogelijk worden in een Azure wachtrij geplaatst en klik vervolgens verwijderd uit de wachtrij door een rol werknemer alleen wanneer dit lukt. Opschonen mogelijk moet u opschonen van gegevens die bewerkingen onderbroken maken.

###<a name="elasticity"></a>Elasticiteit

Het oorspronkelijke aantal exemplaren actief zijn voor elke rol wordt in de configuratie van elke functie bepaald. Beheerders moeten in eerste instantie configureren voor elke rol om uit te voeren met twee of meer exemplaren op basis van de verwachte laden. Maar u kunt eenvoudig rol exemplaren schalen omhoog of omlaag als gebruik patronen wijzigen. U kunt dit handmatig doen in de portal van Azure of kunt u het proces automatiseren met behulp van Windows PowerShell, de Service Management API of hulpprogramma's van derden. Lees [hoe u automatisch schalen een toepassing](../cloud-services/cloud-services-how-to-scale.md)voor meer informatie.

###<a name="partitioning"></a>Partitioneren

De controller Azure stof gebruikt twee soorten partities:

* Een *domein bijwerken* wordt gebruikt voor het upgraden van een service rol exemplaren in groepen. Azure implementeert exemplaren van de service in meerdere domeinen van de update. Een update in-place, de controller stof Hiermee omlaag de overal in één update domain, werkt deze bij en opnieuw gestart voordat u verdergaat naar de volgende update-domein. Deze methode selecteert de hele service niet beschikbaar is niet tijdens het updateproces.
* Een *domein foutenstructuuranalyse* definieert mogelijke punten van hardware of netwerk. Voor een functie die meer dan één exemplaar heeft, de controller stof zorgt ervoor dat de exemplaren zijn verdeeld over meerdere foutenstructuuranalyse domeinen, om te voorkomen dat problemen met de geïsoleerd hardware service te onderbreken. Foutenstructuuranalyse domeinen van toepassing op alle blootgesteld aan server en cluster fouten.

De [Azure service level agreement (SLA)](https://azure.microsoft.com/support/legal/sla/) zorgt ervoor dat wanneer twee of meer web rol exemplaren zijn geïmplementeerd in verschillende foutenstructuuranalyse en domeinen upgraden, ze beschikt over een externe verbinding ten minste 99.95 procent van de tijd. In tegenstelling tot update domeinen is er geen manier om te bepalen van het aantal foutenstructuuranalyse domeinen. Azure wordt automatisch toegewezen foutenstructuuranalyse domeinen en rol exemplaren verdeeld over deze. AT minimaal de eerste twee exemplaren van elke rol in verschillende foutenstructuuranalyse geplaatst en domeinen om ervoor te zorgen dat de SLA nog voldoet aan een rol met ten minste twee instanties upgraden. Dit wordt weergegeven in het volgende diagram.

![Vereenvoudigde weergave van foutenstructuuranalyse domein moeten worden geïsoleerd](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-1.png)

###<a name="load-balancing"></a>Taakverdeling

Alle binnenkomende verkeer aan een Webrol loopt door een stateless taakverdeling, waarin aanvragen van clients tussen de rol exemplaren distribueert. Afzonderlijke rol exemplaren ik heb geen openbare IP-adressen en ze zijn niet rechtstreeks kan van Internet. Web rollen zijn stateless zodat verzoek van een client kan worden doorgestuurd naar een exemplaar van de rol. Een gebeurtenis [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck.aspx) treedt op elke 15 seconden. U kunt dit gebruiken om aan te geven of de rol gereed om te ontvangen verkeer is en of deze is bezet en afmelden bij de draaiing van taakverdeling moet worden genomen.

##<a name="virtual-machines"></a>Virtuele Machines

Azure virtuele Machines verschilt van platform, zoals een service (PaaS) rollen in verschillende opzichten ten opzichte van beschikbaarheid berekenen. In sommige gevallen, moet u extra werk om ervoor te zorgen beschikbaarheid doen.

###<a name="disk-durability"></a>Levensduur van de schijf

In tegenstelling tot PaaS rol exemplaren is gegevens die zijn opgeslagen op VM stations permanente, zelfs wanneer de virtuele machine is verplaatst. Azure virtuele machines VM schijven gebruiken die bestaat als BLOB's in Azure opslag. Vanwege de kenmerken van de beschikbaarheid van Azure-opslag is de gegevens die zijn opgeslagen op een VM stations ook zeer beschikbaar.

Houd er rekening mee dat station D (in Windows VMs) de uitzondering op deze regel is. Station D is werkelijk fysieke opslag op de rek-server waarop de VM en de gegevens niet verloren als de VM herhaald wordt. Station D is bedoeld voor alleen tijdelijke opslag. In Linux stelt Azure "meestal" (maar niet altijd) de lokale tijdelijke schijf als /dev/sdb blokkeren apparaat. Het is vaak gekoppeld door de Azure Linux-Agent als /mnt/resource of mnt punten (te configureren in /etc/waagent.conf).

###<a name="partitioning"></a>Partitioneren

Azure native begrijpt de niveaus in een toepassing voor PaaS (Webrol en werknemer rol) en dus kunt correct deze verdelen over domeinen foutenstructuuranalyse en bijwerken. Daarentegen moeten de niveaus in een infrastructuur voor als een servicetoepassing (IaaS) worden handmatig gedefinieerd door beschikbaarheid sets. Beschikbaarheid sets zijn vereist voor een SLA onder IaaS.

![Beschikbaarheid wordt ingesteld voor Azure virtuele machines](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-2.png)

De laag Internet Information Services (IIS) (dat werkt als een weblaag-app) en de SQL-laag (dat werkt als een gegevenslaag) in het diagram zijn toegewezen aan de beschikbaarheid van de verschillende sets. Dit zorgt ervoor dat alle exemplaren van elke laag hardware redundantie hebben door te distribueren virtuele machines voor foutenstructuuranalyse domeinen en dat hele lagen niet worden genomen ingedrukt tijdens een update.

###<a name="load-balancing"></a>Taakverdeling

Als de VMs verkeer verdeeld over hen hebt moeten, moet u de VMs in een toepassing en laden saldo groeperen in een specifieke TCP of UDP-eindpunt. Zie [virtuele machines van taakverdeling](../virtual-machines/virtual-machines-linux-load-balance.md)voor meer informatie. Als de VMs ontvangt invoer van een andere bron (bijvoorbeeld een wachtrijmechanisme), is een taakverdeling niet vereist. De taakverdeling gebruikt een eenvoudige statuscontrole om te bepalen of verkeer moet worden verzonden naar het knooppunt. Het is ook mogelijk te maken van uw eigen sondes implementatie van toepassingsspecifieke systeemstatus aan de doelstellingen die bepalen of de VM verkeer moet ontvangen.

##<a name="storage"></a>Opslag

Azure opslag is de basislijn duurzame gegevens-service voor Azure. Deze biedt blob, tabel, wachtrij en VM schijf opslagruimte. Site gebruikmaakt van een combinatie van replicatie- en resourcebeheer beschikbaarheid binnen een één datacenter. De beschikbaarheid van Azure Storage SLA zorgt u ervoor dat ten minste 99,9 procent van de tijd:

* Correct opgemaakt aanvragen voor het toevoegen, bijwerken, lezen, en verwijderen van gegevens worden correct en correct verwerkt.
* Opslag accounts heeft connectiviteit met de gateway Internet.

###<a name="replication"></a>Herhaling

Azure opslag vergemakkelijkt levensduur van gegevens door het onderhouden van meerdere exemplaren van alle gegevens op verschillende stations op volledig onafhankelijke fysieke opslagsubsystemen in het gebied. Gegevens synchroon worden gerepliceerd en alle kopieën zijn vastgelegde voordat het schrijven is bevestigd. Azure opslag is ten zeerste consistente, wat betekent dat leest gegarandeerd om de meest recente schrijft aan te geven. Kopieën van gegevens worden bovendien continu gescand om te detecteren en herstellen van bits rot, een vaak hoofd wordt gezien bedreiging voor de integriteit van gegevens in cache.

Services profiteren van herhaling alleen met behulp van Azure-opslag. De ontwikkelaar van de service hoeft niet te doen extra werk aan een lokale herstellen.

###<a name="resource-management"></a>Resourcebeheer

Opslag-accounts die zijn gemaakt nadat mei 2014, uitgroeien tot maximaal 500 TB (het vorige maximale aantal was 200 TB). Als u meer ruimte nodig is, moeten toepassingen zijn ontworpen voor het gebruik van meerdere opslag-accounts.

###<a name="virtual-machine-disks"></a>VM schijven

Een VM schijf wordt opgeslagen als een pagina blob in Azure-opslag, hieraan dezelfde levensduur en schaalbaarheid eigenschappen als blobopslag. Dit ontwerp zorgt ervoor dat de gegevens op een VM schijf permanente, zelfs als de server met de VM mislukt en de VM moet opnieuw worden gestart op een andere server.

##<a name="database"></a>Database

###<a name="sql-database"></a>SQL-Database

Azure SQL-Database biedt database als een service. Dit geeft toepassingen snel inrichten, gegevens in en relationele databases query invoegen. Het biedt veel van de vertrouwde SQL Server-functies en functionaliteit, terwijl de last van hardware, configuratie, herstellen en tolerantie samenvatten.

>[AZURE.NOTE] Azure SQL-Database biedt geen-op-een functie overeenkomst met SQL Server. Dit is bedoeld om te voldoen aan een andere set vereisten--dat een unieke geschikt is voor de cloud-toepassingen (elastische schaal, database als een service die u heeft kunt de onderhoudskosten verkleinen, enzovoort). Zie voor meer informatie [een wolk SQL Server-optie kiezen: (PaaS) van Azure SQL-Database of SQL Server Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

####<a name="replication"></a>Herhaling

Azure SQL-Database biedt ingebouwde tolerantie naar knooppunt niveau is mislukt. Alle geschreven in een database worden automatisch gerepliceerd naar achtergrond knooppunten van twee of meer via een quorum doorvoeren techniek. (De primaire taal en ten minste één secundaire moeten bevestigen dat de activiteit naar het transactielogboek is geschreven voordat de transactie geslaagd geacht is en geeft als resultaat.) Wanneer een knooppunt optreedt wordt de database automatisch overgenomen door een van de secundaire replica's. Hierdoor wordt een onderbroken tijdelijke verbinding voor clienttoepassingen. Daarom moeten alle Azure SQL Database-clients enkele vorm van een tijdelijke verbinding afhandeling implementeren. Zie de [richtlijnen voor specifieke services opnieuw](../best-practices-retry-service-specific.md)voor meer informatie.

####<a name="resource-management"></a>Resourcebeheer

Elke database, wanneer gemaakt, is geconfigureerd met een limiet. De maximumgrootte blijft die momenteel beschikbaar is 1 TB (grootte limieten variëren op basis van de servicelaag van uw, raadpleegt u de [service niveaus en prestaties van Azure SQL-Databases](../sql-database/sql-database-resource-limits.md#service-tiers-and-performance-levels). Wanneer een database de limiet raakt, weigert deze extra opdrachten voor invoegen of bijwerken. (Query's uitvoeren en verwijderen van gegevens is nog steeds mogelijk.)

Azure SQL-Database wordt in een database, een stof gebruikt voor het beheren van resources. Echter, in plaats van een stof controller wordt de topologie van een bellen fouten opsporen. Elke replica in een cluster heeft twee neighbors en is verantwoordelijk voor het detecteren ze omlaag gaan. Wanneer een replica uitvalt, de aangrenzende te starten een agent opnieuw configureren om opnieuw te maken op een andere computer. Om ervoor te zorgen dat een logische server niet te veel bronnen op een computer gebruiken of van de machine-fysiek overschrijden is engine beperking opgegeven.

###<a name="elasticity"></a>Elasticiteit

Als de toepassing meer dan de limiet van 1 TB database nodig heeft, moet deze een aanpak schalen implementeren. U met wilt verkleinen met Azure SQL-Database handmatig partitioneren, ook bekend als sharding, gegevens over meerdere SQL-databases. Deze methode schalen biedt de mogelijkheid om te bereiken bijna lineaire kosten groei met schaal. Elastische groei of capaciteit op aanvraag kan worden uitgebreid met incrementele kosten zo nodig omdat databases zijn gefactureerd op basis van de gemiddelde werkelijke grootte per dag, gebruikt niet is gebaseerd op maximale grootte.

##<a name="sql-server-on-virtual-machines"></a>SQL Server op virtuele Machines

Door de installatie van SQL Server (versie 2014 of hoger) op Azure virtuele Machines, kunt u profiteren van de van de traditionele beschikbaarheidfuncties van SQL Server. Deze functies zijn AlwaysOn beschikbaarheid van de groepen en spiegelen van de database. Houd er rekening mee dat Azure VMs, opslag en netwerken andere operationele kenmerken dan een on-premises, niet-gevirtualiseerde IT-infrastructuur hebben. Een succesvolle implementatie van een hoge beschikbaarheid/herstel (HA/DR) SQL Server-oplossing in Azure wordt aangegeven, moet u deze verschillen begrijpen en uw oplossing zo ins dat ze ontwerpen.

###<a name="high-availability-nodes-in-an-availability-set"></a>Hoge beschikbaarheid knooppunten in een set beschikbaarheid

Wanneer u een hoge beschikbaarheid-oplossing in Azure wordt aangegeven implementeert, kunt u de beschikbaarheid in Azure instellen voor de knooppunten hoge beschikbaarheid in afzonderlijke foutenstructuuranalyse domeinen plaatsen en het upgraden van domeinen. Als u wilt wissen, is de beschikbaarheid van de set een Azure concept. Het is een goede gewoonte die u volgen moet om ervoor te zorgen dat uw databases daadwerkelijk ten zeerste beschikbaar zijn, of u nu AlwaysOn beschikbaarheidsgroepen, spiegelen of iets anders. Als u deze aanbevolen procedure niet volgt, wordt u mogelijk onder ONWAAR aangenomen dat uw systeem ten zeerste beschikbaar is. Maar in feite de knooppunten kunnen alle mislukt tegelijk omdat deze worden aangebracht in hetzelfde foutenstructuuranalyse domein in de regio Azure worden geplaatst.

Deze aanbeveling is niet zoals van toepassing met logboekbestanden. Als een functie noodgevallen herstel, moet u ervoor zorgen dat de servers in afzonderlijke Azure regio's worden uitgevoerd. Deze gebieden zijn per definitie afzonderlijk foutenstructuuranalyse domeinen.

Voor Azure Cloud Services VMs geïmplementeerd via de klassieke portal worden in bepaalde beschikbaarheid, moet u deze in de dezelfde Cloudservice implementeren. VMs geïmplementeerd via Azure resourcemanager (de huidige portal) hebben geen deze beperking. Voor klassieke portal geïmplementeerd VMs in Azure-Cloudservice, kunnen alleen knooppunten in dezelfde Cloud Service dezelfde beschikbaarheid set deelnemen. Daarnaast moet de Cloud Services-VMs in hetzelfde virtuele netwerk om ervoor te zorgen dat ze hun IP-adressen zelfs na de service retoucheren onderhouden. Hiermee voorkomt u DNS-update onderbrekingen.

###<a name="azure-only-high-availability-solutions"></a>Azure alleen-lezen: hoge beschikbaarheid oplossingen

U kunt een hoge beschikbaarheid-oplossing voor SQL Server-databases in Azure wordt aangegeven met behulp van AlwaysOn beschikbaarheid van de groepen of spiegelen van de database hebben.

In het volgende diagram ziet u de architectuur van AlwaysOn beschikbaarheidsgroepen op Azure virtuele Machines uitgevoerd. In dit diagram is overgenomen van het artikel met uitgebreide informatie over dit onderwerp, [beschikbaarheid en herstel voor SQL Server op Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![AlwaysOn beschikbaarheid van groepen in Microsoft Azure](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-1.png)

U kunt ook automatisch een AlwaysOn beschikbaarheidsgroepen implementatie end-to-end op Azure VMs inrichten met behulp van de sjabloon AlwaysOn in de portal van Azure. Zie [SQL Server AlwaysOn aanbod in de galerie met Microsoft Azure Portal](https://blogs.technet.microsoft.com/dataplatforminsider/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery/)voor meer informatie.

In het volgende diagram ziet u het gebruik van de database op Azure virtuele Machines spiegelen. Het is ook overgenomen van de uitgebreide onderwerp [beschikbaarheid en herstel voor SQL Server op Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![In Microsoft Azure spiegelen](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-2.png)

>[AZURE.NOTE] Beide architecturen een domeincontroller nodig is. Met het spiegelen van de database is het echter servercertificaten gebruiken om te voorkomen dat u een domeincontroller.

##<a name="other-azure-platform-services"></a>Andere services Azure platform

Toepassingen die zijn gebaseerd op Azure profiteren van platformmogelijkheden herstellen uit lokale fouten. In sommige gevallen kunt u bepaalde acties om uit te breiden beschikbaarheid voor uw specifieke scenario uitvoeren.

###<a name="service-bus"></a>Service Bus

Als u wilt beperken ten opzichte van een tijdelijke storing van Azure Service Bus, voor het maken van een duurzame aan de clientzijde wachtrij overwegen. Dit wordt een alternatieve, lokale opslag om tijdelijk gebruikt voor het opslaan van berichten die naar de wachtrij Service Bus kunnen niet worden toegevoegd. De toepassing kunt bepalen hoe u omgaat met de tijdelijk opgeslagen berichten nadat de service is hersteld. Zie [Aanbevolen procedures voor het prestatieverbeteringen Service Bus gebruikt brokered messaging](../service-bus-messaging/service-bus-performance-improvements.md) en [Service Bus (herstel)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services)voor meer informatie.

###<a name="hdinsight"></a>HDInsight

De gegevens die is gekoppeld aan Azure HDInsight wordt standaard opgeslagen in Azure-blobopslag. Azure opslag Hiermee geeft u eigenschappen voor hoge beschikbaarheid en levensduur voor blobopslag. De verwerking van meerdere knooppunten dat is gekoppeld aan Hadoop MapReduce taken doet zich voor op een tijdelijke Hadoop Distributed bestand System (HDFS) die is ingericht wanneer HDInsight nodig heeft. Resultaten van een taak MapReduce worden ook opgeslagen al dan niet standaard in Azure-blobopslag, zodat de verwerkte gegevens blijvend is en ten zeerste beschikbaar blijft nadat het cluster Hadoop wordt opgeheven. Zie [HDInsight (herstel)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services)voor meer informatie.

##<a name="checklists-for-local-failures"></a>Controlelijsten voor lokale fouten

###<a name="cloud-services"></a>Cloudservices

  1. Raadpleeg de sectie Cloudservices van dit document.
  2. Ten minste twee instanties voor elke rol configureren.
  3. Persistent staat in duurzame opslag, niet op rol exemplaren.
  4. De gebeurtenis StatusCheck goed verwerkt.
  5. Gerelateerde wijzigingen in transacties indien mogelijk laten teruglopen.
  6. Controleer of werknemer rol taken zijn idempotency is ingeschakeld en opnieuw starten.
  7. Gaat u verder met de bewerkingen aanroepen totdat ze worden voltooid.
  8. Houd rekening met autoscaling strategieën.

###<a name="virtual-machines"></a>Virtuele Machines

  1. Raadpleeg de sectie virtuele Machines van dit document.
  2. Gebruik geen station D voor permanente opslag.
  3. Machines in een servicelaag groeperen in een set beschikbaarheid.
  4. Taakverdeling en optioneel sondes configureren.

###<a name="storage"></a>Opslag

  1. Raadpleeg de sectie opslag van dit document.
  2. Gebruik meerdere opslag accounts wanneer gegevens of de bandbreedte groter is dan de quota's.

###<a name="sql-database"></a>SQL-Database

  1. Raadpleeg de sectie SQL-Database van dit document.
  2. Implementeer een beleid opnieuw tijdelijke fouten.
  3. Partitioneren/sharding als een strategie schalen gebruiken.

###<a name="sql-server-on-virtual-machines"></a>SQL Server op virtuele Machines

  1. Raadpleeg de SQL Server in de virtuele Machines sectie van dit document.
  2. Volg de bovenstaande aanbevelingen voor virtuele Machines.
  3. Gebruik de functies van het SQL Server beschikbaarheid, zoals AlwaysOn.

###<a name="service-bus"></a>Service Bus

  1. Raadpleeg de sectie Service Bus van dit document.
  2. Kunt u een duurzame aan de clientzijde wachtrij als een back-up maken.

###<a name="hdinsight"></a>HDInsight

  1. Raadpleeg de sectie HDInsight van dit document.
  2. Geen extra beschikbaarheid stappen zijn vereist voor lokale fouten.

##<a name="next-steps"></a>Volgende stappen

In dit artikel maakt deel uit van een reeks die zijn gericht op [Azure tolerantie technische richtlijnen](./resiliency-technical-guidance.md). Het volgende artikel in deze reeks is [herstel uit een regio hele service-onderbreking](./resiliency-technical-guidance-recovery-loss-azure-region.md).
