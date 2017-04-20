<properties
    pageTitle="SQL Server-toepassing patronen op VMs | Microsoft Azure"
    description="In dit artikel behandelt toepassing patronen voor SQL Server op Azure VMs. Biedt oplossing versneld een foundation voor goede toepassingsarchitectuur en een ontwerp."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="luisherring"
    manager="jhubbard"
    editor=""
    tags="azure-service-management,azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="lvargas" />

# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Toepassing patronen en ontwikkelingsstrategieën voor SQL Server in Azure virtuele Machines

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="summary"></a>Overzicht:
Vaststellen welke toepassing patroon of patronen gebruiken voor uw SQL Server-gebaseerde toepassingen in Azure wordt aangegeven omgeving is een belangrijk ontwerp beslissing en dit is een goed begrip van hoe SQL Server en elk onderdeel van de infrastructuur van Azure samenwerken vereist. Met SQL Server in Azure-infrastructuurservices, kunt u eenvoudig migreren, onderhouden en controleren van uw bestaande SQL Server-toepassingen ingebouwd bij Windows Server virtuele machines in Azure wordt aangegeven.

Het doel van dit artikel is zodat deze kunnen oplossing versneld voor goede toepassingsarchitectuur en ontwerp, dat ze volgen kunnen bij het migreren van bestaande toepassingen Azure, evenals ontwikkelen van nieuwe toepassingen in Azure wordt aangegeven.

Voor elke toepassing patroon vindt u een mogelijkheid on-premises implementatie, de desbetreffende cloud ingeschakelde oplossing en de gerelateerde technische aanbevelingen. Bovendien het artikel wordt beschreven hoe Azure / regiospecifieke ontwikkelingsstrategieën zodat u kunt uw toepassingen correct ontwerpen. Vanwege de vele mogelijke toepassing patronen, wordt het aanbevolen dat versneld het meest geschikte patroon voor hun toepassingen en gebruikers moet kiezen.

**Technische inzenders:** Luis Jeroen Vargas haring, Madhan Arumugam Ramakrishnan

**Technische revisoren:** Corey schuurmachines, Drew McDaniel, Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani, Stefan Schackow, Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>Inleiding

U kunt verschillende soorten n lagen toepassingen ontwikkelen door te scheiden van de onderdelen van de andere toepassingslagen op verschillende computers ook zoals in afzonderlijke onderdelen. Bijvoorbeeld, kunt u de clienttoepassing plaatsen en bedrijfsregels onderdelen op één computer, front weblaag en laag voor gegevenstoegang in een andere computer en een back-enddatabase laag in een andere computer. Dit soort structureren helpt isoleren elke laag van elkaar verschillen. Als u de waar gegevens vandaan wijzigt, moet u niet wijzigen van de client of web-toepassing, maar alleen de laag voor gegevenstoegang.

Een typische *n lagen* -toepassing bevat de presentatielaag, de laag bedrijven en de gegevenslaag:


| Laag              | Beschrijving                                                                                                                                                                     |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Presentatie** | De *presentatielaag* (weblaag, front laag) is de laag waarop gebruikers interactief met een toepassing werken.                                                                      |
| **Bedrijven**     | De *business trapsgewijs* (middelste laag) is de laag die de presentatie trapsgewijs en de gegevenslaag gebruiken om te communiceren met elkaar en bevat de belangrijkste functies van het systeem. |
| **Gegevens**         | De *gegevenslaag* is in feite de server waarin gegevens worden van een toepassing (bijvoorbeeld een server met SQL Server).                                                             |

Toepassingslagen beschrijven de logische groeperingen van de functionaliteit en onderdelen in een toepassing. terwijl lagen de fysieke verdeling van de functionaliteit en de onderdelen op een aparte fysieke servers, computers, netwerken of externe locaties beschrijft. De lagen van een toepassing kunnen zich bevinden op dezelfde fysieke computer (hetzelfde niveau) of kunnen worden verdeeld over de afzonderlijke computers (n-laag) en de onderdelen in elke laag communiceren met elementen in andere lagen via goed gedefinieerde interfaces. U kunt de laag term als verwijzingen naar fysieke verdeling patronen zoals twee niveaus, drie lagen en n lagen zien. Een **patroon met 2 niveaus toepassing** bevat twee lagen: toepassingsserver en database-server. Er gebeurt directe communicatie tussen de toepassingsserver en de database-server. De toepassingsserver bevat zowel web lagen en bedrijven laag onderdelen. Bij het **patroon van laag 3-toepassing**, zijn er drie toepassing lagen: webserver, toepassingsserver, waarin de zakelijke logica laag en/of bedrijven laag voor gegevenstoegang, en de database-server. De communicatie tussen de webserver en de database-server gebeurt via de toepassingsserver. Zie [Microsoft Application architectuur Guide](https://msdn.microsoft.com/library/ff650706.aspx)voor gedetailleerde informatie over de toepassingslagen en lagen.

Voordat u dit artikel leest, kunt u knowledge op de basisconcepten van SQL Server en Azure nodig hebt. Zie [SQL Server Books Online](https://msdn.microsoft.com/library/bb545450.aspx), [SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md) en [Azure.com](https://azure.microsoft.com/)voor informatie.

In dit artikel worden de verschillende toepassing patronen die geschikt zijn voor uw eenvoudige toepassingen, evenals de complexe bedrijfstoepassingen kunnen zijn. Voordat u met informatie over elke patroon, wordt u aangeraden in dat moet u vertrouwd raken met de beschikbare gegevens storage-services in Azure wordt aangegeven, zoals [Azure opslag](../storage/storage-introduction.md), [Azure SQL-Database](../sql-database/sql-database-technical-overview.md)en [SQL Server in een Azure virtuele machines](virtual-machines-windows-sql-server-iaas-overview.md). Als u wilt de beste beslissingen ontwerp voor uw toepassingen, wanneer gebruikt u welke opslagservice gegevens duidelijk te begrijpen.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>SQL Server kiezen in een Azure virtuele machines, wanneer:

- Besturingselement voor SQL Server en Windows hebt u nodig. Dit kan bijvoorbeeld de versie van SQL Server, speciale hotfixes, performance-configuratie, enzovoort.

- U moet een volledig compatibiliteit met SQL Server on-premises implementatie en wilt verplaatsen van Azure als bestaande toepassingen-is.

- U wilt om te profiteren van de mogelijkheden van de Azure-omgeving maar Azure SQL-Database biedt geen ondersteuning voor alle functies die moeten worden uw toepassing. Dit kan onder andere de volgende stadia:

    - **Omvang van database**: op het moment dat dit artikel is bijgewerkt, SQL-Database van een database van maximaal 1 TB van gegevens ondersteunt. Als uw toepassing meer dan 1 TB aan gegevens kost en u niet wilt dat om aangepaste sharding oplossingen te implementeren, wordt het wordt aanbevolen dat u SQL Server in een Azure virtuele Machine gebruiken. Zie voor de meest recente informatie, [Schaalbaarheid van Azure SQL-Databases](https://msdn.microsoft.com/library/azure/dn495641.aspx) en [Azure SQL Database Servicelagen en prestaties](../sql-database/sql-database-service-tiers.md).
    - **HIPAA-naleving**: gezondheidszorg klanten en onafhankelijke softwareleveranciers (ISV's) mogelijk [SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md) in plaats van [Azure SQL-Database](../sql-database/sql-database-technical-overview.md) kiezen omdat SQL Server in een Azure virtuele machines wordt bedekt door de HIPAA bedrijven koppelen overeenkomst (BAA). Zie voor informatie over naleving, [Microsoft Azure Trust Center: naleving](https://azure.microsoft.com/support/trust-center/compliance/).
    - **Exemplaar niveau functies**: op dit moment kan SQL-Database functies die live buiten de database niet wordt ondersteund (zoals gekoppelde-Servers Agent banen, FileStream, Service makelaar, enz.). Zie [Azure SQL Database-richtlijnen en beperkingen](https://msdn.microsoft.com/library/azure/ff394102.aspx)voor meer informatie.

## <a name="1-tier-simple-single-virtual-machine"></a>1-laag (eenvoudig): één VM

In dit patroon toepassing implementeren u uw SQL Server-toepassing en de database met een zelfstandig product virtuele machine in Azure wordt aangegeven. Dezelfde virtuele machine bevat uw client/webtoepassing, onderdelen van business data access-laag en de database-server. De presentatie, business en gegevens toegangscode logisch gescheiden maar fysiek zich bevinden in een machine één server. De meeste klanten beginnen met het patroon van deze toepassing en kies vervolgens een schaal af door meer web rollen of virtuele machines toe te voegen aan hun systeem.

Het patroon van deze toepassing is handig wanneer:

- Wilt u een eenvoudige migratie naar Azure platform te beoordelen of het platform vindt u antwoorden op de vereisten van uw toepassing al dan niet uitvoeren.

- U wilt behouden alle toepassingslagen die worden gehost in dezelfde virtuele machine in het dezelfde Azure Datacenter verkleinen van de latentie tussen lagen.

- U wilt snel inrichten ontwikkelen en testen van omgevingen voor korte perioden.

- U wilt uitvoeren belasting testen op verschillende niveaus van de werklast maar op hetzelfde moment u niet wilt naar de eigenaar bent en onderhouden veel fysieke machines altijd.

In het volgende diagram ziet u een eenvoudige on-premises implementatie scenario en hoe u kunt de cloud ingeschakeld-oplossing implementeert in één virtuele machines in Azure wordt aangegeven.

![1-laag toepassing patroon](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

De laag bedrijven (bedrijfslogica en data access-onderdelen) op de hetzelfde fysieke niveau als de presentatie laag implementeert kunt prestaties toepassing, tenzij u een afzonderlijke laag vanwege schaalbaarheid of beveiliging bezwaren moet gebruiken.

Aangezien dit een veelvoorkomende patroon beginnen, kunt u het volgende artikel voor migratie nuttig vinden voor het verplaatsen van uw gegevens naar uw SQL Server-VM: [migreren van een Database met SQL Server op een VM Azure](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3-laag (eenvoudig): meerdere virtuele machines

In het patroon van deze toepassing, kunt u een toepassing 3 niveaus in Azure wordt aangegeven door elke toepassingslaag plaatsen in een andere virtuele machine implementeren. Dit biedt een flexibele omgeving voor een eenvoudig schaal-up en schalen scenario's. Wanneer een virtuele machine uw client/webtoepassing bevat, een host van uw business-onderdelen en een host fungeert voor de database-server.

Het patroon van deze toepassing is handig wanneer:

- Wilt u een migratie van complexe databasetoepassingen naar Azure virtuele Machines uitvoeren.

- Wilt u andere toepassing niveaus die moeten worden gehost in verschillende regio's. Zoals u mogelijk hebt gedeeld databases die zijn geïmplementeerd in meerdere gebieden voor rapportage.

- U wilt verplaatsen bedrijfstoepassingen van on-premises implementatie gevirtualiseerde platforms naar Azure virtuele Machines. Zie [Wat is een bedrijfstoepassing](https://msdn.microsoft.com/library/aa267045.aspx)voor een gedetailleerde discussie op enterprise-toepassingen.

- U wilt snel inrichten ontwikkelen en testen van omgevingen voor korte perioden.

- U wilt uitvoeren belasting testen op verschillende niveaus van de werklast maar op hetzelfde moment u niet wilt naar de eigenaar bent en onderhouden veel fysieke machines altijd.

In het volgende diagram ziet u hoe u een eenvoudige 3 lagen-toepassing kunt plaatsen in Azure wordt aangegeven door elke toepassingslaag plaatsen in een andere virtuele machine.

![3-laag toepassing patroon](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

Klik in het patroon van deze toepassing, moet u er slechts één virtuele machine (VM) is in elke laag. Als er meerdere VMs in Azure wordt aangegeven, wordt u aangeraden dat u een virtueel netwerk instellen. [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) wordt gemaakt van de rand van een vertrouwde beveiliging en kunt ook VMs communiceren onderling via het privé IP-adres. Daarnaast altijd Zorg ervoor dat alle verbindingen met Internet alleen gaat u naar de presentatielaag. Wanneer het patroon van deze toepassing te volgen, moet u de netwerk beveiliging groep regels toegangsregels beheren. Zie [externe toegang toestaan voor uw VM met behulp van de Azure portal](virtual-machines-windows-nsg-quickstart-portal.md)voor meer informatie.

In het diagram mag internetprotocollen TCP, UDP, HTTP of HTTPS.

>[AZURE.NOTE] Instellen van een virtueel netwerk in Azure is gratis. Echter wordt geheven voor de gateway VPN die is verbonden met on-premises implementatie. Deze kosten is gebaseerd op de hoeveelheid tijd die verbinding ingerichte en beschikbaar is.

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>laag 2 en 3 laag bij uw presentatie trapsgewijs schalen

In het patroon van deze toepassing, kunt u 2 laag of lagen 3 databasetoepassing naar Azure virtuele Machines implementeren door elke toepassingslaag plaatsen in een andere virtuele machine. Daarnaast u de schaal van de presentatielaag vanwege extra volume van binnenkomende clientaanvragen.

Het patroon van deze toepassing is handig wanneer:

- U wilt verplaatsen van zakelijke toepassingen van on-premises implementatie gevirtualiseerde platforms naar Azure virtuele Machines.

- U wilt schalen uit de presentatielaag vanwege extra volume van binnenkomende clientaanvragen.

- U wilt snel inrichten ontwikkelen en testen van omgevingen voor korte perioden.

- U wilt uitvoeren belasting testen op verschillende niveaus van de werklast maar op hetzelfde moment u niet wilt naar de eigenaar bent en onderhouden veel fysieke machines altijd.

- Wilt u de eigenaar bent van een infrastructuur-omgeving die omhoog en omlaag op aanvraag kan worden aangepast.

In het volgende diagram wordt getoond hoe kunt u de lagen in meerdere virtuele machines in Azure wordt aangegeven plaatsen door de presentatielaag vanwege extra volume van binnenkomende clientaanvragen schalen. Zoals u in het diagram, is Azure taakverdeling verantwoordelijk voor verkeer distribueren over meerdere virtuele machines en ook vaststellen welke webserver verbinding maken met. Met meerdere exemplaren van de endwebservers achter een taakverdeling zorgt ervoor dat de beschikbaarheid van de presentatielaag.

![Toepassing patroon - presentatie laag schaal af](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Aanbevolen procedures voor het laag 2, 3 laag of n lagen patronen met meerdere VMs in één laag

Het wordt aanbevolen dat u de virtuele machines die deel uitmaakt van hetzelfde niveau in de dezelfde cloudservice en in dezelfde de beschikbaarheid plaatsen instellen. Bijvoorbeeld: een set-endwebservers in **CloudService1** en **AvailabilitySet1** en een set van database-servers in **CloudService2** en **AvailabilitySet2**plaatsen. Een beschikbaarheid instellen in Azure kunt u de knooppunten beschikbaarheid in afzonderlijke foutenstructuuranalyse domeinen plaatsen en het upgraden van domeinen.

Als u wilt gebruikmaken van meerdere VM exemplaren van een laag, moet u het configureren van taakverdeling Azure tussen lagen. Voor informatie over het configureren van taakverdeling in elke laag een taakverdeling eindpunt maken op van elke laag VMs afzonderlijk. Voor een specifieke laag, moet u eerst VMs maken in de dezelfde cloudservice. Dit zorgt ervoor dat er hetzelfde openbare virtuele IP-adres. Maak vervolgens een eindpunt op een van de virtuele machines op dat niveau. Hetzelfde eindpunt vervolgens toewijzen aan de andere virtuele machines op die laag voor taakverdeling. Maakt een set verdeeld, moet u verkeer verdelen over meerdere virtuele machines en kunt ook de taakverdeling om te bepalen welke knooppunt verbinding maken wanneer u een back-end VM knooppunt mislukt. Bijvoorbeeld, met meerdere exemplaren van de endwebservers achter een taakverdeling zorgt ervoor dat de beschikbaarheid van de presentatielaag.

Als een goede gewoonte altijd Zorg dat alle verbindingen met internet eerst gaat u naar de presentatielaag. De presentatie laag toegang heeft tot de zakelijke laag en vervolgens de laag bedrijven toegang heeft tot de gegevenslaag. Zie voor meer informatie over het toestaan van toegang tot de laag presentatie [toestaan externe toegang tot uw VM met behulp van de Azure portal](virtual-machines-windows-nsg-quickstart-portal.md).

Houd er rekening mee dat de verdeling van de laden in Azure strekking netwerktaakverdelers in een on-premises omgeving werkt. Zie [taakverdeling voor Azure infrastructuurservices](virtual-machines-windows-load-balance.md)voor meer informatie.

Bovendien is het raadzaam dat u een persoonlijke netwerk instellen voor uw virtuele machines met behulp van Azure Virtual Network. Hierdoor kunnen ze communiceren onderling via het privé IP-adres. Zie [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)voor meer informatie.

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>laag 2 en 3 laag met bedrijven laag schalen

In het patroon van deze toepassing, kunt u 2 laag of lagen 3 databasetoepassing naar Azure virtuele Machines implementeren door elke toepassingslaag plaatsen in een andere virtuele machine. U wilt bovendien de toepassing serveronderdelen distribueren naar meerdere virtuele machines vanwege de complexiteit van uw toepassing.

Het patroon van deze toepassing is handig wanneer:

- U wilt verplaatsen van zakelijke toepassingen van on-premises implementatie gevirtualiseerde platforms naar Azure virtuele Machines.

- Wilt u de toepassing serveronderdelen distribueren naar meerdere virtuele machines vanwege de complexiteit van uw toepassing.

- U wilt verplaatsen en bedrijven logica dik on-premises implementatie LOB (-LOB)-toepassingen naar Azure virtuele Machines. LOB-toepassingen zijn een reeks kritieke computertoepassingen die essentieel zijn voor een onderneming, zoals financieel, HR (human resources), salarisadministratie, levering ketting management en resourceplanning toepassingen uitgevoerd.

- U wilt snel inrichten ontwikkelen en testen van omgevingen voor korte perioden.

- U wilt uitvoeren belasting testen op verschillende niveaus van de werklast maar op hetzelfde moment u niet wilt naar de eigenaar bent en onderhouden veel fysieke machines altijd.

- Wilt u de eigenaar bent van een infrastructuur-omgeving die omhoog en omlaag op aanvraag kan worden aangepast.

In het volgende diagram ziet u een scenario on-premises implementatie en de oplossing cloud ingeschakeld. In dit scenario kunt u de lagen in meerdere virtuele machines in Azure wordt aangegeven door de laag bedrijven, met de zakelijke logica laag en gegevenstoegang schalen plaatsen. Zoals u in het diagram, is Azure taakverdeling verantwoordelijk voor verkeer distribueren over meerdere virtuele machines en ook vaststellen welke webserver verbinding maken met. Met meerdere exemplaren van de toepassingsservers achter een taakverdeling zorgt ervoor dat de beschikbaarheid van de laag bedrijven. Zie [Aanbevolen procedures voor laag 2, 3-laag of n lagen toepassing patronen die meerdere virtuele machines in één laag hebt](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier)voor meer informatie.

![Toepassing patroon met bedrijven laag schaal af](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>laag 2 en 3 laag met de presentatie en bedrijven lagen schalen en HADR

In het patroon van deze toepassing, kunt u 2 laag of lagen 3 databasetoepassing naar Azure virtuele Machines implementeren door het distribueren van de presentatielaag (webserver) en de business laag (toepassingsserver)-onderdelen naar meerdere virtuele machines. Daarnaast implementeren u hoge beschikbaarheid en noodgevallen oplossingen voor uw databases in Azure virtuele machines.

Het patroon van deze toepassing is handig wanneer:

- U wilt verplaatsen bedrijfstoepassingen vanuit gevirtualiseerde platforms on-premises naar Azure door te implementeren beschikbaarheid van SQL Server en noodgevallen herstelmogelijkheden.

- Wilt u de schaal van de presentatielaag en de laag bedrijven vanwege extra volume van de client verzoeken voor oproepen en de complexiteit van uw toepassing aanpassen.

- U wilt snel inrichten ontwikkelen en testen van omgevingen voor korte perioden.

- U wilt uitvoeren belasting testen op verschillende niveaus van de werklast maar op hetzelfde moment u niet wilt naar de eigenaar bent en onderhouden veel fysieke machines altijd.

- Wilt u de eigenaar bent van een infrastructuur-omgeving die omhoog en omlaag op aanvraag kan worden aangepast.

In het volgende diagram ziet u een scenario on-premises implementatie en de oplossing cloud ingeschakeld. In dit scenario u de schaal van de presentatielaag en de onderdelen van de laag bedrijven in meerdere virtuele machines in Azure wordt aangegeven. U kunt bovendien hoge beschikbaarheid (HADR) technieken voor SQL Server-databases implementeren in Azure wordt aangegeven.

Meerdere exemplaren van een toepassing uitgevoerd in verschillende zorg VMs ervoor dat u aanvragen voor taakverdeling tussen deze. Wanneer u meerdere virtuele machines hebt, moet u om ervoor te zorgen dat alle uw VMs toegankelijke en wordt uitgevoerd op een punt in tijd zijn. Als u taakverdeling configureert, wordt Azure taakverdeling wordt de status van VMs bijgehouden en wordt u omgeleid zodat binnenkomende oproepen aan de orde functioneel VM knooppunten behoren. Zie [taakverdeling voor Azure infrastructuurservices](virtual-machines-windows-load-balance.md)voor informatie over het instellen van taakverdeling van de virtuele machines. Met meerdere exemplaren van web- en -servers achter een taakverdeling zorgt ervoor dat de beschikbaarheid van de presentatie en bedrijven niveaus.

![Schalen en hoge beschikbaarheid](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>Aanbevolen procedures voor het toepassing patronen SQL HADR vereisen

Wanneer u SQL Server-beschikbaarheid en noodgevallen hersteloplossingen in Azure virtuele Machines hebt ingesteld, is het instellen van een virtueel netwerk voor uw virtuele machines met [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) verplicht.  Virtuele machines in een virtueel netwerk heeft een stabiele privé IP-adres zelfs na een downtime service, dus kunt u de update-tijd die nodig is voor DNS-naamresolutie voorkomen. Bovendien het virtuele netwerk kunt u uw on-premises netwerk uitbreiden naar Azure en Hiermee maakt u de rand van een vertrouwde beveiliging. Bijvoorbeeld, als uw toepassing heeft bedrijfsdomein beperkingen (zoals Windows-verificatie, Active Directory), het instellen van [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) is nodig.

Meest van klanten, die op Azure worden productiecode worden uitgevoerd, zijn zowel primaire als secundaire replica's behouden in Azure wordt aangegeven.

Zie voor uitgebreide informatie en zelfstudies op beschikbaarheid en technieken voor noodgevallen-herstel, [beschikbaarheid en herstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>laag 2 en 3 laag met Azure VMs en Cloud Services

In dit patroon toepassing u Azure 2 laag of lagen 3 toepassing implementeren met behulp van beide [Azure-Cloudservices](../cloud-services/cloud-services-choose-me.md#tellmecs) (web en werknemer rollen - Platform als een Service (PaaS)) en [Azure virtuele Machines](virtual-machines-windows-about.md) (infrastructuur als een Service (IaaS)). Het gebruik van [Azure Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/) voor de presentatie laag/bedrijven laag en SQL Server in [Azure virtuele Machines](virtual-machines-windows-about.md) voor de gegevenslaag is handig voor de meeste toepassingen op Azure. De reden is dat met een berekeningscluster exemplaar Cloudservices waarop biedt een eenvoudig beheer, implementatie, bewaken en schalen.

Met Cloudservices, Azure onderhoudt de infrastructuur voor u, voert dagelijks onderhoud patches de besturingssystemen en probeert te herstellen van service en hardware fouten. Wanneer uw toepassing schalen moet, is automatisch en handmatig schalen opties zijn beschikbaar voor uw project cloud-service door te vergroten of verkleinen van het aantal exemplaren of virtuele machines die worden gebruikt door uw toepassing. Bovendien kunt u on-premises implementatie Visual Studio om te implementeren van uw toepassing aan een project cloud-service in Azure wordt aangegeven.

In het overzicht, als u niet wilt dat de eigenaar bent van de uitgebreide beheertaken voor de presentatie/bedrijven trapsgewijs en uw toepassing niet een complexe configuratie van de software of het besturingssysteem vereist, Azure Cloud Services gebruiken. Als Azure SQL-Database geen ondersteuning biedt voor alle functies die u zoekt, SQL Server in een Azure virtuele Machine gebruiken voor de gegevenslaag. Een toepassing uitvoeren op Azure Cloud Services en gegevens opslaat in Azure virtuele Machines combineert de voordelen van beide services. Zie de sectie in dit onderwerp op [vergelijking ontwikkelingsstrategieën in Azure wordt aangegeven](#comparing-web-development-strategies-in-azure)voor een gedetailleerde vergelijking.

In dit patroon toepassing bevat de presentatielaag een Webrol, dat is een onderdeel Cloud Services in de omgeving Azure uitgevoerd en deze is aangepast voor het web application programming ondersteund door IIS en ASP.NET. De laag en grote ondernemingen of backend bevat een rol werknemer, dat is een onderdeel Cloud Services in de omgeving Azure uitgevoerd en het is handig voor algemene ontwikkeling en achtergrond voor een Webrol kan uitvoeren. De databaselaag bevindt zich in een SQL Server-VM in Azure wordt aangegeven. De communicatie tussen de presentatielaag en de databaselaag gebeurt rechtstreeks of via de laag bedrijven – werknemer rol onderdelen.

Het patroon van deze toepassing is handig wanneer:

- U wilt verplaatsen bedrijfstoepassingen vanuit gevirtualiseerde platforms on-premises naar Azure door te implementeren beschikbaarheid van SQL Server en noodgevallen herstelmogelijkheden.

- Wilt u de eigenaar bent van een infrastructuur-omgeving die omhoog en omlaag op aanvraag kan worden aangepast.

- Azure SQL-Database biedt geen ondersteuning voor alle functies die uw toepassing of een database nodig heeft.

- U wilt uitvoeren belasting testen op verschillende niveaus van de werklast maar op hetzelfde moment u niet wilt naar de eigenaar bent en onderhouden veel fysieke machines altijd.

In het volgende diagram ziet u een scenario on-premises implementatie en de oplossing cloud ingeschakeld. In dit scenario plaats u de presentatielaag in web rollen, de laag bedrijven werknemer rollen, maar de gegevenslaag in de virtuele machines in Azure wordt aangegeven. Meerdere exemplaren van de presentatielaag uitgevoerd in verschillende web rollen zorgt ervoor dat als u wilt laden balance '-aanvragen over deze. Wanneer u Azure Cloud Services met Azure virtuele Machines combineren, wordt u aangeraden dat u [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) ook instellen. U kunt met [Azure Virtual Network](../virtual-network/virtual-networks-overview.md), stabiele en permanente privé IP-adressen binnen de dezelfde cloudservice hebben in de cloud. Nadat u een virtueel netwerk voor uw virtuele machines definiëren en cloud services, kan worden begonnen met onderling communiceren via het privé IP-adres. Daarnaast biedt met virtuele machines en Azure web/werknemer rollen in hetzelfde [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) lage latentie en meer beveiligde verbindingen. Zie [Wat is een cloudservice](../cloud-services/cloud-services-choose-me.md)voor meer informatie.

Zoals gezien in het diagram, wordt de taakverdeling Azure verkeer verdeeld over meerdere virtuele machines en ook bepaalt welke webserver of toepassingsserver verbinding maken met. Met meerdere exemplaren van de web- en -servers achter een taakverdeling zorgt ervoor dat de beschikbaarheid van de presentatielaag en de laag bedrijven. Zie [Aanbevolen procedures voor het vereisen van SQL HADR toepassing-patronen](#best-practices-for-application-patterns-requiring-sql-hadr)voor meer informatie.

![Toepassing patronen met Cloudservices](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Een andere manier voor het implementeren van deze toepassing patroon is via een Webrol geconsolideerde met presentatielaag en business laag onderdelen, zoals wordt weergegeven in het volgende diagram. Het patroon van deze toepassing is handig voor toepassingen waarvoor statuscontrole ontwerpen. Aangezien Azure stateless berekeningscluster knooppunten op web en werknemer rollen, het is raadzaam dat u een logica om op te slaan status van de sessie met een van de volgende technologieën implementeren: [Caching van Azure](https://azure.microsoft.com/documentation/services/redis-cache/), [Azure Table Storage](../storage/storage-dotnet-how-to-use-tables.md) of [Azure SQL-Database](../sql-database/sql-database-technical-overview.md).

![Toepassing patronen met Cloudservices](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Patroon met Azure VMs, Azure SQL-Database en Azure App Service (WebApps)

Het primaire doel van deze toepassing patroon is om aan te geven hoe u het combineren van Azure-infrastructuur als de onderdelen van een service (IaaS) met Azure platform-als-een-service onderdelen (PaaS) in uw oplossing. Dit patroon is gericht op Azure SQL-Database voor de opslag van relationele gegevens. Deze bevat geen SQL Server in een Azure virtuele computer die deel uitmaakt van de Azure infrastructuur als een serviceaanbod.

In het patroon van deze toepassing, kunt u een databasetoepassing naar Azure implementeren door te plaatsen van de presentatie en bedrijven niveaus in dezelfde virtuele machine en een database in Azure SQL-Database (SQL-Database)-servers te openen. U kunt de presentatielaag implementeren met behulp van traditionele IIS-webservices oplossingen. Of u kunt een samengevoegde presentatie en bedrijven laag met behulp van [Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/)implementeren.

Het patroon van deze toepassing is handig wanneer:

- Een bestaande geconfigureerd in Azure SQL Database-server hebt u al en u wilt snel uw toepassing testen.

- U wilt testen van de mogelijkheden van Azure-omgeving.

- U wilt snel inrichten ontwikkelen en testen van omgevingen voor korte perioden.

- Uw zakelijke logica en gegevens access-onderdelen zijn zichzelf staan binnen een webtoepassing.

In het volgende diagram ziet u een scenario on-premises implementatie en de oplossing cloud ingeschakeld. In dit scenario plaats u de toepassing niveaus in een enkel virtuele machine in Azure en access-gegevens in Azure SQL-Database.

![Gemengde toepassing patroon](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Als u besluit om een gecombineerde web en toepassingslaag met behulp van Azure Web Apps implementeren, wordt u aangeraden dat u de middelste laag of toepassing laag als dll-bestanden (DLL's) in de context van een webtoepassing behouden.

Raadpleeg bovendien de aanwijzingen in de sectie [vergelijking web ontwikkelingsstrategieën in Azure wordt aangegeven](#comparing-web-development-strategies-in-azure) aan het einde van dit artikel voor meer informatie over het programmeren technieken.

## <a name="n-tier-hybrid-application-pattern"></a>N lagen hybride toepassing patroon

In n lagen hybride toepassing patroon, moet u uw toepassing in meerdere niveaus verdeeld on-premises en Azure implementeren. Hiervoor kunt u een systeem flexibele en herbruikbare hybride maken die u kunt wijzigen of een specifieke laag toevoegen zonder te wijzigen van de andere niveaus. Het netwerk van uw bedrijf in de cloud projecteren, gebruikt u [Azure virtuele](../virtual-network/virtual-networks-overview.md) netwerkservice.

Deze hybride toepassing patroon is handig wanneer:

- Wilt u toepassingen die gedeeltelijk in de cloud worden uitgevoerd en gedeeltelijk on-premises maken.

- U wilt migreren van sommige of alle elementen van een bestaande on-premises implementatie-toepassing in de cloud.

- U wilt verplaatsen van zakelijke toepassingen van on-premises implementatie gevirtualiseerde platforms naar Azure.

- Wilt u de eigenaar bent van een infrastructuur-omgeving die omhoog en omlaag op aanvraag kan worden aangepast.

- U wilt snel inrichten ontwikkelen en testen van omgevingen voor korte perioden.

- Wilt u een efficiënte manier om u te maken van back-ups voor enterprise databasetoepassingen.

In het volgende diagram ziet u een patroon van n lagen hybride-toepassing die zich uitstrekt over over on-premises en Azure. Zoals u in het diagram, bevat de on-premises implementatie-infrastructuur [Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) domeincontroller ter ondersteuning van gebruikersverificatie en autorisatie. Houd er rekening mee dat het diagram ziet u een scenario voor waar sommige onderdelen van de gegevenslaag zich in een on-premises implementatie-datacenter dat sommige onderdelen van de gegevenslaag live in Azure wordt aangegeven. Afhankelijk van de behoeften van uw toepassing, kunt u verschillende scenario's van andere hybride implementeren. Bijvoorbeeld u mogelijk de presentatielaag en de laag bedrijven behouden in een on-premises omgeving, maar de gegevenslaag in Azure wordt aangegeven.

![N lagen toepassing patroon](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

In Azure Active Directory kunt u gebruiken als een zelfstandig product cloud-map voor uw organisatie of u kunt ook bestaande lokale Active Directory integreren met [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Zoals gezien in het diagram, kunnen de onderdelen van de laag bedrijven toegang tot meerdere gegevensbronnen, zoals met [SQL Server in Azure wordt aangegeven](virtual-machines-windows-sql-server-iaas-overview.md) via een persoonlijk interne IP-adres, met lokale SQL Server via [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)of met [SQL-Database](../sql-database/sql-database-technical-overview.md) met behulp van de .NET Framework data provider technologieën. In dit diagram is Azure SQL-Database een optionele gegevens storage-service.

U kunt de volgende werkstroom in de opgegeven volgorde implementeren in n lagen hybride toepassing patroon:

1. Enterprise-databasetoepassingen die worden teruggezet moeten naar de cloud met behulp van [Microsoft Assessment and Planning (MAP) Toolkit](http://microsoft.com/map)identificeren. De MAP Toolkit voorraad en prestaties gegevens worden verzameld van computers die bent u van plan voor virtualisatie en aanbevelingen van capaciteit en evaluatie planning.

1. De resources en de configuratie nodig in het Azure platform, zoals opslag accounts en virtuele machines plannen.

1. Het instellen van de netwerkverbindingen tussen het bedrijfsnetwerk on-premises en [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Als u de verbinding tussen het bedrijfsnetwerk on-premises en een virtuele machine in Azure instelt, gebruikt u een van de volgende twee methoden:

    1. Een verbinding maken tussen on-premises en Azure via openbare eindpunten van een virtuele machine in Azure wordt aangegeven. Deze methode biedt een eenvoudige installatie en kunt u SQL Server-verificatie in uw virtuele computer gebruiken. Bovendien uw netwerk beveiliging groep regels instellen om te bepalen van openbare verkeer voor VM. Zie [externe toegang toestaan voor uw VM met behulp van de Azure portal](virtual-machines-windows-nsg-quickstart-portal.md)voor meer informatie.

    1. Een verbinding maken tussen on-premises en Azure via Azure Virtual Private network (VPN) tunnel. Deze methode kunt u beleid voor het domein aan een virtuele machine in Azure uitbreiden. Bovendien kunt u firewallregels instellen en gebruiken van Windows-verificatie in uw VM. Azure ondersteunt momenteel veilig naar website VPN en punt-naar-site VPN-verbindingen:

        - Met beveiligde verbinding van de site-naar-site, kunt u geen netwerkverbinding tussen uw on-premises netwerk en uw virtual netwerk in Azure wordt aangegeven. Het wordt aanbevolen voor uw on-premises gegevens center omgeving verbinden met Azure.

        - Met beveiligde verbinding punt-naar-site, kunt u geen netwerkverbinding tussen uw virtual netwerk in Azure wordt aangegeven en de afzonderlijke computers nergens uitgevoerd. Het verdient voornamelijk bedoeld voor ontwikkelen en testdoeleinden.

    Zie voor informatie over het verbinding maken met SQL Server in Azure wordt aangegeven, [verbinden aan een virtuele Machine op Azure van SQL Server](virtual-machines-windows-classic-sql-connect.md).

1. Geplande taken en waarschuwingen die back-up van on-premises gegevens in een schijf VM in Azure instellen. Zie [SQL Server-back-up en herstellen met Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx) en [back-up en herstellen voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-backup-recovery.md)voor meer informatie.

1. Afhankelijk van de behoeften van uw toepassing, kunt u een van de volgende drie veelvoorkomende scenario's implementeren:

    1. U kunt de webserver, toepassingsserver en weinig gevoelige gegevens in een database-server in Azure wordt aangegeven houden terwijl u de lokale gevoelige gegevens behouden.

    1. U kunt uw webserver bewaart en toepassingsserver on-premises terwijl de database-server in een virtuele machine in Azure wordt aangegeven.

    1. U kunt laten uw database-server, de webserver, en toepassingsserver on-premises terwijl u de databasereplica in de virtuele machines in Azure wordt aangegeven behouden. Deze instelling kunt de on-premises implementatie-endwebservers of rapporttoepassingen voor toegang tot de databasereplica in Azure wordt aangegeven. U kunt er daarom als u de werkbelasting in een lokale database lager wilt bereiken. Het is raadzaam dat u dit scenario voor dik implementeren werkbelasting en ontwikkeling doeleinden lezen. Zie voor informatie over het maken van databasereplica in Azure AlwaysOn beschikbaarheidsgroepen op [beschikbaarheid en herstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="comparing-web-development-strategies-in-azure"></a>Vergelijking van web ontwikkelingsstrategieën in Azure wordt aangegeven

Als u wilt implementeren en te gebruiken van een meerlagige SQL Server-toepassing in Azure wordt aangegeven, kunt u een van de volgende twee programming methoden:

- Een traditionele webserver (IIS - Internet Information Services) instellen in Azure en access-databases in SQL Server in Azure virtuele Machines.

- Implementeren en te gebruiken van een cloudservice naar Azure. Zorg dat deze cloudservice kunt access-databases in SQL Server in Azure virtuele machines. Een cloudservice kan meerdere web en werknemer rollen bevatten.

De volgende tabel bevat een vergelijking van traditionele websites ontwikkelen met Azure-Cloudservices en Azure Web-Apps met betrekking tot SQL Server in Azure virtuele Machines. De tabel bevat Azure Web Apps, zoals is het mogelijk te gebruiken SQL Server in Azure VM als gegevensbron voor Azure-WebApps via de openbare virtuele IP-adres of de DNS-naam.

||Traditionele webontwikkeling in Azure virtuele Machines|Cloudservices in Azure wordt aangegeven|Webhosting met Azure WebApps|
|---|---|---|---|
|**Toepassingen migreren vanuit on-premises**|Bestaande toepassingen als-is.|Toepassingen moeten rollen web en werknemer.|Bestaande toepassingen als-maar geschikt is voor zelfstandig webtoepassingen en webservices waarvoor snelle schaalbaarheid.|
|**Ontwikkeling en implementatie**|Visual Studio, WebMatrix, Visual Web Developer, WebDeploy, FTP, TFS, IIS-beheer PowerShell.|Visual Studio, Azure SDK, TFS, PowerShell. Elke cloudservice heeft twee omgevingen waaraan u de servicepakket en de configuratie kunt implementeren: ontwikkel- en. U kunt een cloudservice dashboard implementeren naar de testomgeving te testen voordat u een niveau naar productie verhogen.|Visual Studio, WebMatrix, Visual Web Developer, FTP, cijfer, BitBucket, CodePlex, DropBox, GitHub, volgt, TFS, Web implementeert, PowerShell.|
|**Beheer en configuratie**|U bent verantwoordelijk voor beheertaken op de toepassing, gegevens, firewallregels, virtuele netwerk en besturingssysteem gebruikt.|U bent verantwoordelijk voor beheertaken op de toepassing, gegevens, firewallregels en virtueel netwerk.|U bent verantwoordelijk voor beheertaken op de toepassing en alleen gegevens.|
|**Beschikbaarheid en herstel (HADR)**|Het is raadzaam dat u virtuele machines in bepaalde beschikbaarheid en in de dezelfde cloudservice plaatsen. Ervoor dat uw VMs in dezelfde kunt beschikbaarheid verzameling u Azure voor de knooppunten beschikbaarheid in afzonderlijke foutenstructuuranalyse domeinen plaatsen en het upgraden van domeinen. Op dezelfde manier ervoor dat uw VMs in dezelfde cloudservice kunt taakverdeling en VMs kunnen rechtstreeks met elkaar communiceren via het lokale netwerk binnen een Azure Datacenter.<br/><br/>U bent verantwoordelijk voor de uitvoering van een beschikbaarheid en noodgevallen hersteloplossing voor SQL Server in Azure virtuele Machines om te voorkomen dat een downtime. Zie [beschikbaarheid en herstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md)voor ondersteunde HADR technologieën.<br/><br/>U bent verantwoordelijk voor het back-ups van uw eigen gegevens en toepassingen.<br/><br/>Azure kan uw virtuele machines verplaatsen als de host in het midden van gegevens vanwege hardwareproblemen mislukt. Bovendien kan er geplande downtime van uw VM wanneer de host wordt bijgewerkt voor beveiliging of software updates. Daarom, is het raadzaam ten minste twee VMs in elke toepassingslaag om ervoor te zorgen de continue beschikbaarheid bij te houden. Azure biedt geen SLA voor een enkele virtuele machine. Zie [Azure tolerantie technische richtlijnen](../resiliency/resiliency-technical-guidance.md)voor meer informatie.|Azure beheert de fouten die resulteert uit de onderliggende besturingssysteem of hardware software. Het is raadzaam dat u meerdere exemplaren van een rol web of werknemer om ervoor te zorgen de beschikbaarheid van de toepassing implementeren. Zie voor informatie [Cloud Services, virtuele Machines en virtuele netwerk Service Level Agreement](http://www.microsoft.com/download/details.aspx?id=38427) en [herstel en beschikbaarheid van Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>U bent verantwoordelijk voor het back-ups van uw eigen gegevens en toepassingen.<br/><br/>Voor databases die zich in een SQL Server-database in een VM Azure, bent u verantwoordelijk voor de uitvoering van een hoge beschikbaarheid hersteloplossing om te voorkomen dat een downtime. Zie beschikbaarheid en herstel voor SQL Server in Azure virtuele Machines voor ondersteunde HDAR technologieën.<br/><br/>**SQL Server spiegelen**: gebruik met Azure Cloud Services (web/werknemer rollen). SQL Server VMs en een cloud-service project kunnen worden in hetzelfde Azure virtuele netwerk. Als SQL Server VM zich niet in hetzelfde virtuele netwerk bevindt, moet u een SQL Server-mailalias communicatie om naar te routeren het exemplaar van SQL Server maken. Daarnaast moet de aliasnaam van de overeenkomen met de naam van de SQL Server.|Beschikbaarheid wordt overgenomen van Azure werknemer rollen, Azure-blobopslag en Azure SQL-Database. Azure Storage heeft bijvoorbeeld drie kopieën van alle blob, tabel en wachtrijgegevens. Op elk gewenst moment Azure SQL-Database drie kopieën van gegevens actief blijft, één primaire replica en twee secundaire replica's. Zie [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) en [Azure SQL-Database](../sql-database/sql-database-technical-overview.md)voor meer informatie.<br/><br/>Wanneer u SQL Server in Azure VM als gegevensbron voor Azure Web Apps, houd er rekening mee dat Azure-WebApps Azure Virtual Network niet ondersteunt. Alle verbindingen van Azure-Web-Apps met SQL Server VMs in Azure moeten met andere woorden, leest u over de openbare eindpunten van virtuele machines. Dit kan ertoe leiden dat enkele beperkingen voor grote beschikbaarheid en herstel scenario's voor noodgevallen. Bijvoorbeeld zou de clienttoepassing op Azure Web-Apps verbinden met SQL Server VM met spiegelen van de database niet verbinding maken met de nieuwe primaire server zoals spiegelen van de database is vereist dat u instelt op de Azure Virtual Network tussen SQL Server host VMs in Azure wordt aangegeven. Daarom is met Azure Web-Apps met behulp van **SQL Server spiegelen** momenteel niet ondersteund.<br/><br/>**SQL Server AlwaysOn beschikbaarheidsgroepen**: U kunt AlwaysOn beschikbaarheidsgroepen instellen wanneer u Azure Web-Apps met SQL Server VMs in Azure wordt aangegeven. Maar u moet AlwaysOn beschikbaarheid van de groep luisteraar ervan af om te sturen van de communicatie naar de primaire replica via openbare taakverdeling eindpunten configureren.|
|**Cross-premises-connectiviteit**|Verbinding maken met on-premises implementatie kunt u Azure Virtual Network.|Verbinding maken met on-premises implementatie kunt u Azure Virtual Network.|Azure Virtual Network wordt ondersteund. Zie [Integratie van de Web Apps virtuele-netwerken](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)voor meer informatie.|
|**Schaalbaarheid**|Schaal-up is beschikbaar door te verhogen van de grootte VM of meer schijven toe te voegen. Zie voor meer informatie over VM grootte [VM grootten voor Azure](virtual-machines-windows-sizes.md).<br/><br/>**Voor de Database-Server**: schalen is beschikbaar via de database partities technieken en beschikbaarheid van SQL Server-AlwaysOn groepen.<br/><br/>Voor veel meer werkbelastingen, kunt u [AlwaysOn beschikbaarheidsgroepen](https://msdn.microsoft.com/library/hh510230.aspx) gebruiken op meerdere secundaire knooppunten, evenals SQL Server replicatie.<br/><br/>Voor veel schrijven-werkbelastingen, kunt u de horizontale partities gegevens implementeren voor meerdere fysieke servers te leveren toepassing schalen.<br/><br/>Bovendien kunt u een schalen met behulp van [SQL Server met gegevens afhankelijke routering](https://technet.microsoft.com/library/cc966448.aspx)implementeren. Met gegevens afhankelijke routering (DDR), moet u de partities om implementeren in de clienttoepassing, meestal in de laag laag voor bedrijven, voor het routeren van de database aanvragen naar meerdere SQL Server-knooppunten. De laag bedrijven bevat toewijzingen aan hoe de gegevens is partitioneren en welk knooppunt de gegevens bevat.<br/><br/>U kunt schalen toepassingen die virtuele machines worden uitgevoerd. Zie [hoe u de schaal van een toepassing](../cloud-services/cloud-services-how-to-scale.md)voor meer informatie.<br/><br/>**Belangrijke opmerking**: de functie **automatisch schalen** in Azure wordt aangegeven kunt u automatisch vergroten of verkleinen van de virtuele Machines die worden gebruikt door uw toepassing. Deze functie zorgt ervoor dat de eindgebruiker is niet negatief beïnvloed piek perioden en VMs worden afgesloten wanneer de vraag laag is. Het wordt aanbevolen dat u niet de optie automatisch schalen voor uw cloudservice instellen doen als hierin SQL Server VMs. De reden is dat de functie automatisch schalen Azure een virtuele machine inschakelen kunt wanneer de CPU-gebruik in die VM hoger zijn dan de drempel sommige is en een virtuele machine uitschakelen wanneer het CPU-gebruik lager is dan deze hoort. De functie automatisch schalen is handig voor stateless toepassingen, zoals endwebservers, waar een VM de werklast zonder verwijzingen naar een vorige staat kunt beheren. De functie automatisch schalen is echter niet handig voor statuscontrole toepassingen, zoals SQL Server, waarbij slechts één exemplaar schrijven met de database zijn toegestaan.|Schaal-up is beschikbaar met behulp van meerdere web en werknemer rollen. Zie voor meer informatie over VM-grootte voor het web rollen en werknemer rollen [Grootten configureren voor Cloudservices](../cloud-services/cloud-services-sizes-specs.md).<br/><br/>Wanneer u een **Cloud Services**gebruikt, kunt u meerdere rollen om te distribueren verwerking en ook bereiken flexibele schaalbaarheid van de toepassing kunt definiëren. Elke cloudservice bevat een of meer web rollen en/of werknemer rollen, elk voorzien van een eigen toepassingsbestanden en configuratie. U kunt een cloudservice schaal-up maken door te vergroten van het aantal exemplaren van de rol (virtuele machines) voor een rol is geïmplementeerd en schaal omlaag een cloudservice door te verminderen van het aantal exemplaren van de rol. Zie [Azure Execution modellen](../cloud-services/cloud-services-choose-me.md)voor gedetailleerde informatie.<br/><br/>Schalen is beschikbaar via de ingebouwde Azure beschikbaarheid ondersteuning via [Cloud Services, virtuele Machines en virtuele netwerk Service Level Agreement](http://www.microsoft.com/download/details.aspx?id=38427) en taakverdeling.<br/><br/>Voor een toepassing met meerdere niveaus, is het raadzaam dat u verbinding maken web/werknemer rollen toepassing databaseserver VMs via Azure Virtual Network. Daarnaast biedt Azure taakverdeling voor VMs in de cloudservice van dezelfde spreiden aanvragen van gebruikers over deze. Virtuele machines verbonden op deze manier kunt rechtstreeks met elkaar communiceren via het lokale netwerk binnen een Azure Datacenter.<br/><br/>U kunt **automatisch schalen** instellen op de portal van Azure klassieke, evenals de tijden planning. Zie [hoe u de schaal van een toepassing](../cloud-services/cloud-services-how-to-scale.md)voor meer informatie.|**Omhoog en omlaag schaal**: U kunt vergroten/verkleinen de grootte van het exemplaar (VM) gereserveerd voor uw website.<br/><br/>Schaal af: U kunt meer gereserveerde exemplaren (VMs) toevoegen voor uw website.<br/><br/>U kunt **automatisch schalen** instellen op de portal, evenals de tijden planning. Lees [hoe u de schaal Web Apps](../app-service-web/web-sites-scale.md)voor meer informatie.|

Zie voor meer informatie over het kiezen tussen deze programming methoden [Azure Web Apps, Cloudservices en VMs: gebruik hiervan](../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het uitvoeren van SQL Server in Azure virtuele machines [SQL Server Azure virtuele Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md).
