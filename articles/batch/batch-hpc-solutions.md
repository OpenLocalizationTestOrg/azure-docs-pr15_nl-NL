<properties
   pageTitle="Batch en HPC oplossingen in de cloud | Microsoft Azure"
   description="Meer informatie over de batch en high-performance computing (HPC en groot berekenen) scenario's en oplossingsopties in Azure wordt aangegeven"
   services="batch, virtual-machines, cloud-services"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-compute"
   ms.date="07/27/2016"
   ms.author="danlep"/>

# <a name="batch-and-hpc-solutions-in-the-azure-cloud"></a>Batch en HPC oplossingen in de cloud Azure

Azure biedt efficiënte, scalable cloud-oplossingen voor batch en high performance computing (HPC) - ook genoemd *Groot berekenen*. Lees hier over grote berekenen werkbelasting en van Azure services worden ondersteund, of Ga rechtstreeks naar de [oplossing scenario's](#scenarios) verderop in dit artikel. In dit artikel is hoofdzakelijk voor technische beslissingsbevoegden, IT-beheerders en onafhankelijke softwareleveranciers, maar andere IT-professionals en ontwikkelaars deze kunnen gebruiken om te leren over deze oplossingen.

Organisaties hebben grootschalige computing problemen: engineering ontwerp en analyse, weergave van afbeeldingen, complexe modellen, Monte Carlo-simulaties, risico financiële berekeningen en anderen. Azure helpt organisaties die deze problemen oplossen met de resources, schaal en planning die zij nodig hebben. Met Azure organisaties kunt doen:

* Hybride oplossingen, voor het uitbreiden van een on-premises implementatie HPC cluster offload piek werkbelasting in de cloud maken

* HPC cluster hulpprogramma's en werkbelasting volledig in Azure uitvoeren

* Beheerde en scalable Azure services zoals [Batch](https://azure.microsoft.com/documentation/services/batch/) gebruiken om te computerintensieve werkbelasting uitvoeren zonder te gebruiken en beheren berekeningscluster infrastructuur

Hoewel buiten het bereik van dit artikel, Azure ook ontwikkelaars biedt en partners een volledige reeks mogelijkheden, architectuur keuzen en ontwikkeling's grootschalige, aangepaste groot berekenen werkstromen maken. En een groeiende partner-systeem is klaar om te helpen u uw werkbelasting groot berekenen productief kunt aanbrengen in de cloud Azure.


## <a name="batch-and-hpc-applications"></a>Batch en HPC-toepassingen

In tegenstelling tot de web-toepassingen en veel LOB-toepassingen, batch en HPC toepassingen hebt gedefinieerde begin en einde en deze kunnen worden uitgevoerd op een planning of op aanvraag, soms voor uren of langer. Meest in twee belangrijkste categorieën vallen: *intrinsiek parallelle* (ook wel 'embarrassingly parallelle', omdat de problemen die ze oplossen parallel op meerdere computers of processors uitgevoerd) en *nauw verbonden*. Zie de volgende tabel voor meer informatie over het toepassingstypen van deze. Sommige Azure oplossing benadert werk beter voor het ene type of de andere.

>[AZURE.NOTE] Een actieve exemplaar van een toepassing heet meestal een *taak*in de Batch en HPC-oplossingen, en elke project mogelijk krijgen onderverdeeld in *taken*. En de gegroepeerde berekeningscluster bronnen voor de toepassing heten vaak *knooppunten berekenen*.

Type | Kenmerken | Voorbeelden
------------- | ----------- | ---------------
**Intrinsiek parallelle**<br/><br/>![Intrinsiek parallelle][parallel] |• Afzonderlijke computers voeren toepassingslogica onafhankelijk<br/><br/> • Toe te voegen computers kan de toepassing schaal en krijg sneller berekenen<br/><br/>• Toepassing bestaat uit afzonderlijke uitvoerbare bestanden of bestaat uit een groep services aangeroepen door een client (een service gerichte architectuur of SOA, toepassing) |• Financiële risico modellering<br/><br/>• Weergave en de verwerking van afbeeldingen<br/><br/>• Media-codering en transcodering<br/><br/>• Monte Carlo-simulaties<br/><br/>• Software testen
**Nauw verbonden**<br/><br/>![Nauw verbonden][coupled] |• Toepassing berekeningscluster knooppunten om te werken of wisselen tussenliggende resultaten is vereist<br/><br/>• Berekeningscluster knooppunten kunnen communiceren met het bericht Passing Interface (MPI), een algemeen communicatieprotocol voor parallelle computing<br/><br/>• De toepassing wordt vertrouwelijke netwerklatentie en bandbreedte<br/><br/>• Toepassingsprestaties kan worden verbeterd met behulp van snelle netwerken technologieën zoals InfiniBand en externe directe geheugentoegang (RDMA) |• Olive oil en gas reservoirmodellen<br/><br/>• Technische ontwerp en analyse, zoals rekenkundige flexibel dynamics<br/><br/>• Fysiek simulaties zoals auto loopt en nucleaire reacties<br/><br/>• Weer prognose maken

### <a name="considerations-for-running-batch-and-hpc-applications-in-the-cloud"></a>Overwegingen voor het uitvoeren van batch en HPC toepassingen in de cloud

U kunt gemakkelijk veel toepassingen die zijn ontworpen om uit te voeren in on-premises implementatie HPC-clusters Azure of een hybride (cross lokale) omgeving migreren. Echter, kunnen er enkele beperkingen of overwegingen, waaronder:


* **Beschikbaarheid van resources cloud** - afhankelijk van het type cloud berekeningscluster bronnen die u gebruikt, u mogelijk niet vertrouwen op doorlopend machine beschikbaarheid terwijl een taak wordt uitgevoerd. Provinciale afhandeling en de voortgang controleren wijzen algemene technieken worden afgehandeld mogelijke tijdelijke fouten en meer nodig bij het gebruik van cloud resources zijn.


* **Data access** - gegevens access technieken vaak beschikbaar in de enterprise kolomgroepen zoals NFS, kunnen vragen om speciale configuratie in de cloud. Of, moet u mogelijk bij het gebruik van verschillende access-procedures en patronen voor de cloud.

* **Gegevens verplaatsen** - zijn voor toepassingen die verwerken van grote hoeveelheden gegevens, strategieën nodig om de gegevens in cloudopslag schakelen en te berekenen van resources. Mogelijk moet u snelle cross-premises netwerk zoals [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/). Ook rekening houden met legal, wetgeving of beleid beperkingen wilt opslaan of openen die gegevens.


* **Licenties** - Neem contact op met de leverancier van een commerciële toepassing voor volumelicenties of andere beperkingen voor het uitvoeren van in de cloud. Niet alle leveranciers bieden pay-as-you-go licenties. Mogelijk moet u plannen voor een licentieserver in de cloud voor uw oplossing of verbinding maken met een on-premises licentie-server.


### <a name="big-compute-or-big-data"></a>Groot berekenen of Big Data?

De scheidingslijn tussen grote berekenen en Big Data-toepassingen niet altijd wissen en sommige toepassingen werken kenmerken van beide. Beide gebruikmaakt van grootschalige berekeningen, meestal uitgevoerd op clusters van computers. Maar de oplossing methoden en ondersteunende hulpmiddelen kunnen verschillen.

• **Groot berekenen** vaak gebruikmaakt van toepassingen die afhankelijk zijn van power processor en geheugen, zoals technische simulaties, financiële risico modeling en digitale weergave. De infrastructuur voor een oplossing groot berekenen, bevatten mogelijk computers met gespecialiseerde multicore processors om uit te voeren onbewerkte berekenen en gespecialiseerde, snelle netwerkhardware om verbinding maken met de computers.

• **Big Data** is opgelost analyse gegevensproblemen waarbij u gebruikmaakt van grote hoeveelheden gegevens die door één computer of database managementsysteem kunnen niet worden beheerd. Voorbeelden hiervan zijn grote hoeveelheden weblogs of andere business intelligence-gegevens. BIG Data vaak meer vertrouwen op schijf en o prestaties dan op CPU power. Er zijn ook gespecialiseerde Big Data-hulpprogramma's zoals Apache Hadoop om de cluster en partition de gegevens te beheren. (Zie [Hadoop](https://azure.microsoft.com/solutions/hadoop/)voor informatie over Azure HDInsight en andere oplossingen Azure Hadoop.)

## <a name="compute-management-and-job-scheduling"></a>Management en taakplanning berekenen

Het uitvoeren van Batch en HPC toepassingen vaak bevat een *cluster manager* en een *taak scheduler* om te helpen beheren gegroepeerd berekeningscluster resources en ze toewijst aan de toepassingen die de taken worden uitgevoerd. Deze functies mogelijk worden uitgevoerd door afzonderlijke hulpprogramma's, of een geïntegreerde hulpmiddel of de service.

* **Cluster manager** - bepalingen, loslaat en beheert resources berekenen (of berekenen van knooppunten). Een manager cluster mogelijk de installatie van besturingssysteem afbeeldingen automatiseren en toepassingen op berekeningscluster knooppunten, schaal berekeningscluster resources op basis van de vraag en de prestaties van de knooppunten controleren.

* **Taak scheduler** - Hiermee geeft u de resources (zoals processors of geheugen) een toepassing behoeften en de voorwaarden wanneer deze wordt uitgevoerd. Een taak scheduler onderhoudt een wachtrij van taken en hen op basis van een toegewezen prioriteit of andere kenmerken bronnen worden toegewezen.

Cluster en hulpprogramma's voor Windows- of Linux gebaseerde clusters voor taakplanning kunnen u ook naar Azure migreren. Bijvoorbeeld, biedt [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029), Microsoft gratis berekeningscluster clusteroplossing voor Windows en Linux HPC-werkbelastingen, diverse opties voor het uitvoeren van in Azure wordt aangegeven. U kunt ook Linux clusters om uit te voeren open source-hulpprogramma's zoals koppel en SLURM maken. U kunt ook commerciële raster oplossingen overbrengen naar Azure, zoals [TIBCO DataSynapse GridServer](http://www.tibco.com/company/news/releases/2016/tibco-to-accelerate-cloud-adoption-of-banking-and-capital-markets-customers-via-microsoft-collaboration), [IBM Platform Symphony](http://www-01.ibm.com/support/docview.wss?uid=isg3T1023592)en [Univa raster Engine](http://www.univa.com/products/grid-engine).

Zoals u ziet in de volgende secties, kunt u ook profiteren van Azure services voor het beheren van berekeningscluster resources en planning van taken zonder (of naast) traditionele cluster beheerprogramma's.


## <a name="scenarios"></a>Scenario 's

Hier vindt u drie veelvoorkomende scenario's om uit te voeren werkbelasting groot berekenen in Azure wordt aangegeven met behulp van bestaande HPC clusteroplossingen, Azure services of een combinatie van beide. Belangrijke overwegingen voor het kiezen van elk scenario worden vermeld, maar zijn niet volledig. Meer is informatie over de beschikbare Azure services die u in uw oplossing gebruiken kunt later in het artikel.

  | Scenario | Waarom kiezen?
------------- | ----------- | ---------------
**Een HPC cluster Azure burst**<br/><br/>[! [Cluster burst] [burst_cluster]](./media/batch-hpc-solutions/burst_cluster.png) <br/><br/> Meer informatie:<br/>• [Burst in Azure werknemer exemplaren met HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)<br/><br/>• [Instellen een hybride cluster met HPC Pack berekenen](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)<br/><br/>• [Burst aan Azure Batch met HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)<br/><br/>|• Combineert u uw [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) of andere on-premises implementatie cluster met extra Azure resources in een hybride-oplossing.<br/><br/>• Breid uw werkbelasting groot berekenen om uit te voeren op Platform als een Service (PaaS) VM-exemplaren (momenteel alleen Windows Server).<br/><br/>• Toegang tot een on-premises implementatie-licentie-archief server of gegevens met behulp van een optioneel Azure virtuele netwerk|• U een bestaand HPC cluster en wilt meer informatiebronnen <br/><br/>• Die u niet wilt aanschaffen en aanvullende HPC cluster infrastructuur beheren<br/><br/>• Er tijdelijke piek aanvraag perioden of speciale projecten
**Een HPC-cluster volledig in Azure maken**<br/><br/>[! [Cluster in IaaS] [iaas_cluster]](./media/batch-hpc-solutions/iaas_cluster.png)<br/><br/>Meer informatie:<br/>• [HPC cluster-oplossingen in Azure wordt aangegeven](./big-compute-resources.md)<br/><br/>|• Snel en consistente implementeren uw toepassingen en cluster hulpmiddelen voor op standaard- of aangepaste Windows of Linux-infrastructuur als een service (IaaS) virtuele machines.<br/><br/>• Verschillende werkbelasting groot berekenen met behulp van de oplossing van uw keuze voor taakplanning uitvoeren.<br/><br/>• Extra Azure-services zoals netwerken en opslag gebruiken om voltooid cloudgebaseerde oplossingen te maken. |• Die u niet wilt aanschaffen en aanvullende Linux of Windows HPC cluster-infrastructuur beheren<br/><br/>• Er tijdelijke piek aanvraag perioden of speciale projecten<br/><br/>• Moet u een extra cluster voor per keer, maar niet wilt te investeren in computers en ruimte te implementeren<br/><br/>• Die u wilt laten uw toepassing computerintensieve uitvoeren zodat deze wordt uitgevoerd als een service volledig in de cloud
**De schaal van een parallelle toepassing Azure aanpassen**<br/><br/>[! [Azure Batch] [batch_proc]](./media/batch-hpc-solutions/batch_proc.png)<br/><br/>Meer informatie:<br/>• [Basisprincipes van Azure Batch](./batch-technical-overview.md)<br/><br/>• [Aan de slag met de bibliotheek Azure Batch voor .NET](./batch-dotnet-get-started.md)|• Ontwikkel met [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) aan de nieuwe schaal uit verschillende groot berekenen werkbelasting uitvoeren op groepen van Windows of Linux virtuele machines.<br/><br/>• Een Azure platform-service gebruiken om implementatie en autoscaling van virtuele machines, taakplanning, herstel, verplaatsing van gegevens, afhankelijkheid management en de toepassingsimplementatie van de te beheren.|• Die u niet wilt beheren berekenen of een taak scheduler; resources gewenste in plaats daarvan kunt richten op het uitvoeren van uw toepassingen<br/><br/>• Die u wilt laten uw toepassing computerintensieve uitvoeren zodat deze wordt uitgevoerd als een service die u in de cloud<br/><br/>• Die u wilt schalen automatisch uw resources berekeningscluster zodat deze overeenkomen met de werklast berekenen


## <a name="azure-services-for-big-compute"></a>Azure services voor groot berekenen

Hier vindt u meer informatie over het berekenen, gegevens, netwerken en gerelateerde services die u kunt combineren voor oplossingen groot berekenen en werkstromen. Zie de [documentatie](https://azure.microsoft.com/documentation/)van Azure services voor gedetailleerde hulp bij het Azure services. De [scenario's](#scenarios) eerder in dit artikel weergeven enkele voorbeelden van hoe van het gebruik van deze services.

>[AZURE.NOTE] Azure introduceert regelmatig nieuwe services die kunnen nuttig zijn voor uw scenario. Als u vragen hebt, neem contact op met een [Azure partner](https://pinpoint.microsoft.com/en-US/search?keyword=azure) of e-mail *bigcompute@microsoft.com*.

### <a name="compute-services"></a>Services berekenen

Azure berekeningscluster services vormen een belangrijk onderdeel van een oplossing groot berekenen en de voordelen van verschillende berekeningscluster services aanbieding voor verschillende scenario's. Deze services bieden op een eenvoudige niveau, verschillende modi voor toepassingen uitvoeren op berekenen op basis van een virtuele machine exemplaren Azure vindt u met behulp van Windows Server Hyper-V technologie. Deze exemplaren kunnen worden uitgevoerd standaardkleuren of aangepaste Linux en Windows-besturingssystemen en hulpprogramma's voor. Azure kunt u een selectie van [exemplaar formaten](../virtual-machines/virtual-machines-windows-sizes.md) met verschillende configuraties van CPU cores, geheugen schijf en andere kenmerken. U kunt afhankelijk van uw behoeften, de schaal van de exemplaren tot duizenden cores aanpassen en klik vervolgens schalen omlaag als u minder bronnen nodig hebt.

>[AZURE.NOTE] Gebruik maken van de Azure computerintensieve-exemplaren wilt verbeteren de prestaties en schaalbaarheid van HPC werkbelasting inclusief parallelle MPI toepassingen waarvoor een lage latentie en hoge doorvoer toepassingennetwerk meenemen. Zie [informatie over H-reeks en computerintensieve A-reeks VMs](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).  

Service | Beschrijving
------------- | -----------
**[Virtuele machines](https://azure.microsoft.com/documentation/services/virtual-machines/)**<br/><br/> |• Bieden infrastructuur berekenen als een service (IaaS) met behulp van Microsoft Hyper-V technologie<br/><br/>• Kunt u flexibel inrichten en permanente cloud computers beheren vanuit standaardafbeeldingen Windows Server- of Linux van [Azure Marketplace](https://azure.microsoft.com/marketplace/), of afbeeldingen en gegevensschijven die u opgeeft<br/><br/>• Kan worden geïmplementeerd en beheerd als [VM schaal Sets](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/) grootschalige services van identieke virtuele machines met autoscaling te vergroten of verkleinen capaciteit automatisch maken<br/><br/>• Uitgevoerd on-premises implementatie berekenen cluster hulpprogramma's en toepassingen volledig in de cloud<br/><br/>
**[Cloudservices](https://azure.microsoft.com/documentation/services/cloud-services/)**<br/><br/> |• Kan worden uitgevoerd toepassingen groot berekenen in werknemer rol exemplaren, die zijn virtuele machines met een versie van Windows Server en worden beheerd door Azure geheel<br/><br/>• Scalable, betrouwbare toepassingen met lage administratieve realiseren, uitgevoerd in een platform als servicemodel (PaaS) inschakelen<br/><br/>• Mogelijk extra's of ontwikkeling voor integratie met on-premises implementatie HPC cluster-oplossingen
**[Batch](https://azure.microsoft.com/documentation/services/batch/)**<br/><br/> |• Grootschalige parallel en batch werkbelasting in een volledig beheerde service wordt uitgevoerd<br/><br/>• Biedt taakplanning en autoscaling van een beheerde resourcegroep virtuele machines<br/><br/>• Kunnen ontwikkelaars maken en uitvoeren van toepassingen als een service of bestaande toepassingen cloud-inschakelen<br/>

### <a name="storage-services"></a>Opslagservices

Een oplossing groot berekenen meestal werkt op een reeks invoergegevens en genereert van gegevens voor de resultaten. Enkele van de Azure storage-services gebruikt in grote berekenen oplossingen zijn:

* [Blob, tabel, en wachtrij opslag](https://azure.microsoft.com/documentation/services/storage/) - grote hoeveelheden ongestructureerde gegevens, NoSQL gegevens en berichten voor de werkstroom en communicatie, respectievelijk beheren. Bijvoorbeeld, kunt u blobopslag voor grote technische gegevenssets of voor de invoer afbeeldingen of mediabestanden uw toepassing verwerkt. U kunt wachtrijen voor asynchrone communicatie in een oplossing. Zie [Inleiding tot Microsoft Azure-opslag](../storage/storage-introduction.md).

* [Azure bestandsopslag](https://azure.microsoft.com/services/storage/files/) - deelt algemene bestanden en gegevens in Azure wordt aangegeven met de standaard SMB-protocol, die nodig is voor bepaalde HPC cluster-oplossingen.

* [Gegevensopslag Lake](https://azure.microsoft.com/services/data-lake-store/) - biedt een ' hyperscale ' Apache Hadoop Distributed File System voor de cloud, handig voor batch, realtime, en interactieve analytics.

### <a name="data-and-analysis-services"></a>Gegevens en analysis services

Bepaalde scenario's met grote berekenen gebruikmaakt van grootschalige gegevensstromen of gegevens die moeten worden nadere verwerking of analyse genereren. Azure biedt verschillende gegevens en analysis services, waaronder:

* [Gegevens Factory](https://azure.microsoft.com/documentation/services/data-factory/) - Builds gegevensgestuurde werkstromen pijpleidingen () die join, aggregatie en transformeren gegevens uit de on-premises cloudgebaseerde, en Internet gegevens winkels.

* [SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/) - biedt de belangrijkste functies van een systeem voor projectmanagement relationele database Microsoft SQL Server in een beheerde service.

* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/) - wordt geïmplementeerd en bepalingen Windows Server- of Linux-gebaseerde Apache Hadoop clusters in de cloud te beheren, analyseren en te rapporteren van grote gegevens.

* [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) - helpt u bij het maken, testen, werken en bekijk analytische oplossingen in een volledig beheerde service beheren.

### <a name="additional-services"></a>Aanvullende services

Uw oplossing groot berekenen mogelijk andere Azure services verbinding maken met lokale resources of in andere omgevingen. Voorbeelden hiervan zijn:

* Een logisch geïsoleerd sectie [Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) - gemaakt in Azure Azure bronnen verbinden met elkaar of naar uw on-premises implementatie-Datacenter. Met een cross-premises virtueel netwerk, groot berekenen toepassingen toegang tot on-premises gegevens, Active Directory-services en licentieservers

* [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) - Hiermee maakt u een persoonlijke verbinding tussen Microsoft datacenters en de infrastructuur van on-premises of in een omgeving met anderen samenwerkt locatie. ExpressRoute biedt betere beveiliging, meer betrouwbaarheid sneller snelheden en lagere vertragingstijden dan de normale verbindingen via Internet.

* [Service Bus](https://azure.microsoft.com/documentation/services/service-bus/) - biedt verschillende methoden voor toepassingen om te communiceren of exchange-gegevens, ongeacht of ze zich bevinden op Azure op een ander cloud-platform of in een datacenter.

## <a name="next-steps"></a>Volgende stappen

* Zie [Technische bronnen voor Batch en HPC](big-compute-resources.md) naar technische richtlijnen voor het maken van uw oplossing.

* Bespreek Azure opties met partners inclusief cyclus Computing en UberCloud.

* Lees meer over Azure groot berekenen oplossingen geleverd door [Torens Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222), [Altair](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/) [ANSYS](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)en [d3VIEW](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088).

* Zie het [Microsoft HPC en Batch teamblog](http://blogs.technet.com/b/windowshpc/) en de [Azure-blog](https://azure.microsoft.com/blog/tag/hpc/)voor aankondigingen van de nieuwste.

<!--Image references-->
[parallel]: ./media/batch-hpc-solutions/parallel.png
[coupled]: ./media/batch-hpc-solutions/coupled.png
[iaas_cluster]: ./media/batch-hpc-solutions/iaas_cluster.png
[burst_cluster]: ./media/batch-hpc-solutions/burst_cluster.png
[batch_proc]: ./media/batch-hpc-solutions/batch_proc.png
