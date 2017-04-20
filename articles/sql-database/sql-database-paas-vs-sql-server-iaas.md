<properties
    pageTitle="SQL (PaaS) versus SQL-databaseserver in de cloud op VMs (IaaS) | Microsoft Azure"
    description="Informatie over welke cloud SQL Server-optie past uw toepassing: (PaaS) van Azure SQL-Database of SQL Server in de cloud op Azure virtuele Machines."
    services="sql-database, virtual-machines"
    keywords="SQL Server cloud, SQL Server in de cloud, PaaS database, SQL Server, DBaaS cloud"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="cjgronlund"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Kies een wolk SQL Server-optie: (PaaS) van Azure SQL-Database of SQL Server Azure VMs (IaaS)

Azure heeft twee opties voor het hosten van SQL Server-werkbelastingen in Microsoft Azure:

* [Azure SQL-Database](https://azure.microsoft.com/services/sql-database/): A SQL-database systeemeigen in de cloud, ook bekend als een platform als een (PaaS)-servicedatabase of een database als een service (DBaaS) die is geoptimaliseerd voor het ontwikkelen van Apps voor software-als-een-service (SaaS). Deze optie biedt compatibiliteit met de meeste functies van SQL Server. Zie [Wat is PaaS](https://azure.microsoft.com/overview/what-is-paas/)voor meer informatie over PaaS.
* [SQL Server op Azure virtuele Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server is geïnstalleerd en gehost in de cloud op Windows Server virtuele Machines (VMs) waarop Azure, ook bekend als een infrastructuur voor als een service (IaaS).
SQL Server op Azure virtuele machines is geoptimaliseerd voor het migreren van bestaande SQL Server-toepassingen. Alle versies en edities van SQL Server zijn beschikbaar. 100% compatibel met SQL Server, zodat u voor het hosten zoveel databases als nodig en uitvoeren cross-databasetransacties biedt. Volledig beheer in SQL Server en Windows biedt.

Leer hoe u elke optie in het Microsoft-platform gegevens past en hulp overeenkomen met de juiste optie op uw zakelijke vereisten. Of u kosten te besparen of beheer van de minimale ligt rest prioriteit, kunt in dit artikel u bepalen welke methode u biedt tegen de zakelijke behoeften dat u meest bent geïnteresseerd.


## <a name="microsofts-data-platform"></a>Microsoft gegevens platform

Een van de eerste dingen om te begrijpen aan een discussie van Azure versus lokale SQL Server-databases is dat u deze alle kunt gebruiken. Microsoft gegevens platform maakt gebruik van SQL Server-technologie en beschikbaar verkoopeenheden fysiek lokale computers, privé cloud omgevingen, derden gehoste privé cloud omgevingen en openbare cloud. SQL Server Azure virtual mchines kunt u unieke en diverse zakelijke behoeften via een combinatie van on-premises en cloud gehoste implementaties bij het gebruik van dezelfde reeks serverproducten, hulpmiddelen voor de ontwikkeling en expertise in deze omgevingen.

   ![Opties voor SQL Server cloud: SQL server op IaaS of SaaS SQL-database in de cloud.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Zoals u in het diagram, kan elke aanbod worden gekenmerkt door het niveau van beheer er via de infrastructuur (op de X-as) en de mate van kosten efficiency bereikt door database niveau samenvoeging en automatisering (op de Y-as).

Bij het ontwerpen van een toepassing, zijn vier eenvoudige opties beschikbaar voor het hosten van de SQL Server-onderdeel van de toepassing:

- SQL Server op niet-gevirtualiseerde fysieke machines
- SQL Server on-premises gevirtualiseerde machines (privé cloud)
- SQL Server in Microsoft Azure Virtual Machine (Microsoft openbare cloud)
- Azure SQL-Database (Microsoft openbare cloud)

In de volgende secties vindt u meer informatie over SQL Server in de Microsoft openbare cloud: Azure SQL-Database en SQL Server Azure VMs. Daarnaast verkennen u veelvoorkomende bedrijven motivators om te bepalen welke optie geschikt is voor uw toepassing.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Azure SQL-Database en SQL Server Azure VMs van nabij bekijken

**Azure SQL-Database** is een relationele database-als-een-service (DBaaS) die worden gehost in de Azure cloud die in de categorieën industrie van *Software-als-een-Service (SaaS)* en *Platform-als-een-Service (PaaS)*valt. [SQL-database](sql-database-technical-overview.md) is gebaseerd op gestandaardiseerde hardware en software die is eigenaar, die worden gehost en beheerd door Microsoft. Met SQL-Database, kunt u direct op de service met ingebouwde functies en functionaliteit ontwikkelen. Wanneer u SQL-Database, u pay-as-you-go met opties voor het up- of uitzoomen schaal voor groter power met niet onderbroken.

**SQL Server op Azure virtuele Machines (VMs)** in de categorie industriële *Infrastructuur-als-een-Service (IaaS)* valt en kunt u SQL Server wordt uitgevoerd binnen een virtuele machine in de cloud. Net als met SQL-Database, het is gebaseerd op gestandaardiseerde hardware die wordt eigenaar, die worden gehost en beheerd door Microsoft. Wanneer u SQL Server op een VM, kunt u een van beide betaald-als u overal voor een SQL Server-licentie al opgenomen in een SQL Server-afbeelding of een bestaande licentie eenvoudig te gebruiken. U kunt ook eenvoudig schaal-omhoog/omlaag- en de VM naar wens onderbreken/hervatten.

Deze twee SQL-opties zijn in het algemeen is geoptimaliseerd voor verschillende doeleinden:

- **SQL-Database** is geoptimaliseerd voor de totale kosten voor het inrichten en beheren van verschillende databases beperken. Deze verlaagt lopende beheerkosten omdat er geen om een virtuele machines, het besturingssysteem of de database-software te beheren. U hoeft niet te upgrades, beschikbaarheid of [back-ups](sql-database-automated-backups.md)beheren. Azure SQL-Database kunt in het algemeen, aanzienlijk vergroten het aantal databases die worden beheerd door één IT of development resources.
- **SQL Server wordt uitgevoerd op Azure VMs** is geoptimaliseerd voor het migreren van bestaande toepassingen Azure of bestaande on-premises implementatie toepassingen in de cloud in hybride implementaties uitbreiden. Bovendien kunt u SQL Server in een virtuele machine ontwikkelen en testen van traditionele SQL Server-toepassingen. Met SQL Server op Azure VMs, kunt u de volledige beheerdersrechten hebt op een speciale SQL Server-instantie en een VM cloudgebaseerde. Dit is een juiste keuze wanneer een organisatie al IT-bronnen die beschikbaar zijn heeft voor het behoud van de virtuele machines. Deze mogelijkheden kunnen u een hoge mate aangepaste systeem om de specifieke prestaties en de beschikbaarheid van de vereisten van uw toepassing op te lossen.

De volgende tabel worden de belangrijkste kenmerken van de SQL-Database en SQL Server Azure VMs:

|       | SQL-Database | SQL Server in een Azure virtuele machines|
| -------------- | ------------ | ---------------------- |
| **Uitstekend voor:** | Nieuwe cloud ontworpen toepassingen die tijdsbeperkingen in ontwikkeling en marketing hebben. |Bestaande toepassingen die opgevolgd snel migratie naar de cloud met minimale wijzigingen moeten. Snelle ontwikkeling en test-scenario's als u niet wilt kopen van lokale niet-productieve SQL Server hardware. |
|| Teams die ingebouwde beschikbaarheid, herstel en upgrade nodig voor de database. |Teams die kunnen configureren en beheren van beschikbaarheid, herstel en patches voor SQL Server. Sommige opgenomen geautomatiseerde functies aanzienlijk vereenvoudigen. |
||Teams die niet wilt beheren van de onderliggende besturingssysteem en configuratie-instellingen.| Als u een aangepaste omgeving met volledige beheerdersrechten nodig hebt.|
||Databases van maximaal 1 TB of grotere databases die kunnen worden [horizontaal of verticaal partities](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) met een patroon schalen.|SQL Server-exemplaren met maximaal 64 TB opslagruimte op kwijt. Het exemplaar kan zo veel databases naar wens ondersteunen. |
||[Building Software-als-een-Service (SaaS)-toepassingen](sql-database-design-patterns-multi-tenancy-saas-applications.md).| Migreren en bouwen enterprise en hybride-toepassingen.|
|||||
|**Bronnen:**|U niet wilt gebruiken IT-informatiebronnen voor configuratie en beheer van de onderliggende infrastructuur, maar u wilt de toepassingslaag te belichten.|Er zijn enkele bronnen IT voor configuratie en beheer. Sommige opgenomen geautomatiseerde functies aanzienlijk vereenvoudigen.|
|**Totale kosten van eigendom:**|Hoeft u kosten voor hardware en Hiermee reduceert u administratieve kosten.|Hiermee voorkomt u hardwarekosten.|
|**Bedrijfscontinuïteit:**|Naast de mogelijkheden voor infrastructuur ingebouwde fouttolerantie biedt Azure SQL-Database functies, zoals [Automatische back-ups](sql-database-automated-backups.md), [Point-In-Time herstellen](sql-database-recovery-using-backups.md#point-in-time-restore), [Geografische herstellen](sql-database-recovery-using-backups.md#geo-restore)en [Actieve geografische-herhaling](sql-database-geo-replication-overview.md) om uit te breiden bedrijfscontinuïteit. Zie de [SQL-Database bedrijven bedrijfscontinuïteit overzicht](sql-database-business-continuity.md)voor meer informatie.|SQL Server Azure VMs kunt u een hoge beschikbaarheid hersteloplossing voor specifieke behoeften van uw database instellen. U kunt een systeem die is geoptimaliseerd daarom hebben voor uw toepassing. U kunt testen en failovers macro's starten met uzelf als dat nodig is. Zie [beschikbaarheid en herstel voor SQL Server op Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)voor meer informatie.|
|**Hybride cloud:**|Uw on-premises implementatie-toepassing toegang heeft tot gegevens in Azure SQL-Database.|Met SQL Server op Azure VMs, kunt u de toepassingen die gedeeltelijk in de cloud worden uitgevoerd en gedeeltelijk on-premises hebben. U kunt bijvoorbeeld uw on-premises netwerk en Active Directory-domein in de cloud via [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)uitbreiden. Bovendien kunt u gegevensbestanden on-premises implementatie opslaan in een [SQL Server-gegevensbestanden in Azure wordt aangegeven](http://msdn.microsoft.com/library/dn385720.aspx)met Azure-opslag. Zie [Inleiding tot SQL Server 2014 hybride Cloud](http://msdn.microsoft.com/library/dn606154.aspx)voor meer informatie.|
||Ondersteunt [transacties replicatie van SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) als abonnee van gegevens worden gerepliceerd.|Volledig ondersteund [transacties herhaling van SQL Server](https://msdn.microsoft.com/library/mt589530.aspx), [AlwaysOn beschikbaarheidsgroepen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)Integration Services en Log verzenden als u wilt repliceren gegevens. Bovendien worden traditionele back-ups van SQL Server volledig ondersteund|
|||||
|||||

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Zakelijke motivaties voor het kiezen van Azure SQL-Database of SQL Server Azure VMs

### <a name="cost"></a>Kosten

Of u nu een opstarten die is strapped voor cashflow of een team bij een waarop deze is opgezet bedrijf die wordt uitgevoerd onder beperkt budget beperkingen, is beperkte financiële middelen het meestal de primaire-stuurprogramma bij het kiezen van hoe u het hosten van uw databases. In dit gedeelte vindt u meer informatie over de facturering en Licentieverlening algemeen in Azure wordt aangegeven met betrekking tot deze twee opties voor relationele database: SQL-Database en SQL Server Azure VMs. Ook leert u over het berekenen van de toepassing totale kosten.

#### <a name="billing-and-licensing-basics"></a>Facturering en basisinformatie over licenties

**SQL-Database** wordt als een service, niet met een licentie aan klanten verkocht.  [SQL Server Azure VMs](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md) wordt verkocht bij een opgenomen licentie die u betaalt per minuut. Als u een bestaande licentie hebt, kunt u deze ook gebruiken.  

**SQL-Database** is momenteel beschikbaar zijn in verschillende niveaus van de service, die per uur met een vaste snelheid op basis van de servicelaag zijn gefactureerd en prestatieniveau die u kiest. Bovendien kunt u zijn rekening voor uitgaande internetverkeer op normale [gegevens tarieven overbrengen](https://azure.microsoft.com/pricing/details/data-transfers/). De service Basic, Standard en Premium lagen zijn ontworpen voor overzichtelijk prestaties met meerdere prestatieniveaus om te voldoen aan vereisten voor piek van uw toepassing. U kunt schakelen tussen service niveaus en prestaties aan de behoeften gevarieerde doorvoer van uw toepassing. Als uw database hoog transacties volume en behoeften ter ondersteuning van veel gebruikers tegelijkertijd heeft, wordt aangeraden de Premium-servicelaag. Zie voor de meest recente informatie over de huidige ondersteunde service voor lagen, [Lagen Azure SQL Database-Service](sql-database-service-tiers.md). U kunt ook [elastische database van toepassingen](sql-database-elastic-pool.md) als u wilt delen prestaties bronnen tussen exemplaren van de database maken.

Met **SQL-Database**, wordt de databasesoftware automatisch geconfigureerd, hersteld en bijgewerkt door Microsoft, waardoor het van de beheerkosten. Bovendien kunnen de mogelijkheden van de [ingebouwde back-up](sql-database-automated-backups.md) u bereiken aanzienlijk kosten te besparen, met name als er een groot aantal databases.

U kunt met **SQL Server Azure VMs**, gebruikt u een van de geleverde platform SQL Server-afbeeldingen (waaronder een licentie) of uw SQL Server-licentie weer. Alle ondersteunde SQL Server-versies (2008R2, 2012, 2014, 2016) en edities (ontwikkelaars, Express, Web, standaard, Enterprise) zijn beschikbaar. Daarnaast zijn voren-uw-eigenaar bent van-licentie versies (BYOL) van de afbeeldingen beschikbaar. Wanneer u het gebruik van de Azure afbeeldingen opgeeft, wordt de operationele kosten is afhankelijk van het formaat van de VM en de editie van SQL Server die u kiest. Ongeacht VM grootte of SQL Server-editie betaalt u per minuut volumelicenties kosten van SQL Server en Windows Server, samen met de opslag van Azure kosten voor de VM schijven. De facturering per minuut-optie kunt u SQL Server gebruiken om zo lang maken als u licenties nodig heb zonder kopen toevoeging SQL Server. Als u uw eigen SQL Server-licentie aan Azure, wordt geheven voor Windows Server en alleen de kosten voor de opslag. Zie voor meer informatie over licenties brengen zelf, [Licentie mobiliteit via Software Assurance op Azure](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="calculating-the-total-application-cost"></a>Berekening van de totale toepassing-kosten

Wanneer u met een cloud-platform begint, bevat de kosten van het uitvoeren van uw toepassing de ontwikkeling en beheer van de kosten, plus de kosten van openbare cloud platform-service.

Hier ziet u de berekening van de gedetailleerde kosten voor uw toepassing wordt uitgevoerd in SQL-Database en SQL Server Azure VMs:

**Bij gebruik van Azure SQL-Database:**

*Totale kosten van toepassing = ten zeerste geminimaliseerde beheerkosten + software development kosten + SQL-Database servicekosten*

**Wanneer u SQL Server op Azure VMs:**

*Totale kosten van toepassing ten zeerste geminimaliseerde software development kosten beheerkosten + SQL Server en Windows Server volumelicenties kosten + Azure opslagkosten =*

Zie de volgende bronnen voor meer informatie over prijzen:

- [Prijzen van de SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/)
- [VM prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/) voor [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) en [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Azure Rekenmachine prijzen](https://azure.microsoft.com/pricing/calculator/)

> [AZURE.NOTE] Er is een klein aantal functies op SQL Server die niet van toepassing of niet beschikbaar met SQL-Database. Zie [beperkingen voor SQL Database-algemeen en richtlijnen](sql-database-general-limitations.md) en [SQL Database Transact-SQL-informatie](sql-database-transact-sql-information.md) voor meer informatie. Als u een bestaande SQL Server-oplossing naar de cloud verplaatst, raadpleegt u [een SQL Server-database met Azure SQL-Database migreren](sql-database-cloud-migrate.md). Wanneer u een bestaande on-premises implementatie SQL Server-toepassing met SQL-Database migreren, kunt u de toepassing om te profiteren van de mogelijkheden die cloud services-aanbieding bijwerken. U kunt bijvoorbeeld overwegen [Azure Web App-Service](https://azure.microsoft.com/services/app-service/web/) of [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) gebruikt voor het hosten van uw toepassingslaag om de kostenvoordelen te verbeteren.

### <a name="administration"></a>Beheer

Voor veel bedrijven is het besluit om een overgang naar een cloudservice zoals veel over offloading complexiteit van beheer zoals deze zijn de kosten. Met **SQL-Database**, hiermee de onderliggende hardware beheerd door Microsoft. Microsoft automatisch wordt overgenomen door alle gegevens voor maximale beschikbaarheid, configureert en upgrades van de databasesoftware, beheer van taakverdeling en overname heeft als er een serverfout. U kunt doorgaan met het beheren van uw database, maar u niet meer nodig hebt voor het beheren van de database engine, server-besturingssysteem of hardware.  Voorbeelden van items die u kunt blijven beheren zijn databases en aanmeldingen, index en query optimaliseren, en controle en beveiliging.

Met **SQL Server Azure VMs**hebt u volledige controle over het besturingssysteem en de configuratie van de SQL Server-exemplaar. Met een VM is het aan u bepalen wanneer update/upgrade het besturingssysteem en de database-software en wanneer u extra software zoals anti-virus uitvoert installeert. Sommige geautomatiseerde functies zijn bedoeld om de beschikbaarheid van patch, back-up en hoog aanzienlijk te vereenvoudigen. Bovendien kunt u de grootte van de VM, het aantal schijven en hun opslagconfiguraties bepalen. Azure kunt u de grootte van een VM desgewenst wijzigen. Zie voor informatie [VM en Cloud Service grootten voor Azure](../virtual-machines/virtual-machines-linux-sizes.md). 

### <a name="service-level-agreement-sla"></a>Serviceovereenkomst (SLA)

Voor veel IT-afdelingen is omhoog-tijd verplichtingen van een Service Level (Agreement) een bovenste prioriteit. In dit gedeelte kijken we naar wat SLA is van toepassing op elke optie hostingprovider-database.

Microsoft biedt voor **SQL-Database** Basic, Standard en Premium Servicelagen een beschikbaarheid SLA van 99,99%. Zie voor actuele informatie [Service Level Agreement](https://azure.microsoft.com/support/legal/sla/sql-database/). Zie voor de meest recente informatie over de SQL-Database service niveaus en de ondersteunde bedrijfscontinuïteit bedrijfsplannen, [Servicelagen](sql-database-service-tiers.md).

Microsoft biedt voor **SQL Server wordt uitgevoerd op Azure VMs**, een beschikbaarheid SLA van 99.95% dat alleen de virtuele Machine bedekt. Deze SERVICEOVEREENKOMST processen (zoals SQL Server) op de VM niet besproken en is vereist dat u ten minste twee VM exemplaren in een set beschikbaarheid host. Zie de [VM SERVICEOVEREENKOMST](https://azure.microsoft.com/support/legal/sla/virtual-machines/)voor de meest recente gegevens. Database beschikbaarheid (HA) binnen VMs, moet u een van de ondersteunde beschikbaarheidopties configureren in SQL Server, zoals [AlwaysOn beschikbaarheid van de groepen](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx). Met de optie van een ondersteunde beschikbaarheid niet zelf een extra SLA, maar kunt u bereiken > beschikbaarheid van de database 99,99%.

### <a name="market"></a>Tijd naar market

**SQL-Database** is de juiste oplossing voor cloud ontworpen toepassingen wanneer ontwikkelaars productiviteit en snel tijd op de markt kritiek zijn. Met programma DBA-achtige-functionaliteit is dit ideaal voor cloud versneld terwijl deze Hiermee verlaagt u dat u voor het beheer van de onderliggende besturingssysteem en database. U kunt bijvoorbeeld de [REST API](http://msdn.microsoft.com/library/azure/dn505719.aspx) en de [PowerShell-Cmdlets](http://msdn.microsoft.com/library/azure/dn546726.aspx) gebruiken automatiseren en beheerdersbewerkingen voor duizendtallen van databases beheren. Functies zoals [Elastische Database-toepassingen](sql-database-elastic-pool.md) kunnen u zich richten op de toepassingslaag en uw oplossing kunt overbrengen op de markt sneller.

**SQL Server wordt uitgevoerd op Azure VMs** is perfect als uw toepassingen bestaand of nieuw grote databases, onderling gerelateerde databases of toegang tot alle in SQL Server- of Windows-functies vereist. Het is ook een goede aanpassen wanneer u de bestaande wilt migreren on-premises implementatie-toepassingen en -databases aan Azure als-is. Aangezien u niet hoeft te wijzigen van de presentatie, toepassingen en gegevens lagen, bespaart u tijd en budget op uw bestaande oplossing rearchitecting. In plaats daarvan kunt u zich richten op alle uw oplossingen migreren naar Azure en klik in het doen van enkele prestatieverbeteringen die nodig is door het Azure platform. Zie [Prestaties aanbevolen procedures voor SQL Server op Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-performance.md)voor meer informatie.

## <a name="summary"></a>Overzicht

In dit artikel verkend SQL-Database en SQL Server op Azure virtuele Machines (VMs) en algemene motivators voor bedrijven die mogelijk van invloed zijn op uw beslissing besproken. Hier volgt een overzicht van suggesties voor oplossingen doornemen:

Kies **Azure SQL-Database** als:

- U bij het maken van nieuwe cloud-toepassingen om te profiteren van de kosten te besparen en prestaties optimaliseren die cloud services bieden. Deze methode biedt de voordelen van een volledig beheerde cloudservice, helpt lagere eerste tijd op de markt en langdurige optimalisatie van kosten kunt bieden.

- Wilt u hebt Microsoft algemene management bewerkingen uitvoeren op uw databases en betere beschikbaarheid serviceovereenkomsten vereisen voor databases.



Kies **SQL Server Azure VMs** als:

- Bestaande on-premises implementatie-toepassingen die u wilt migreren of uit te breiden naar de cloud, of als u wilt maken van zakelijke toepassingen groter is dan 1 TB. Deze methode biedt het voordeel van 100% SQL compatibiliteit, grote database capaciteit, volledige controle over SQL Server en Windows en secure tunnel naar on-premises implementatie. Hierdoor minimaliseert kosten voor de ontwikkeling en wijzigingen van bestaande toepassingen.

- U hebt bestaande IT-bronnen en kunt uiteindelijk de eigenaar bent van patches, back-ups en beschikbaarheid van de database. Zoals u ziet dat sommige geautomatiseerde functies deze bewerkingen aanzienlijk te vereenvoudigen. 


## <a name="next-steps"></a>Volgende stappen
- Zie [SQL-Database zelfstudie: een SQL-database maken in minuten met behulp van de Azure portal](sql-database-get-started.md) aan de slag met SQL-Database.
- Zie [SQL-Database prijzen] (https://azure.microsoft.com/pricing/details/sql-database/).
- Zie [een SQL Server virtuele machine in Azure inrichten](../virtual-machines/virtual-machines-windows-portal-sql-server-provision.md) aan de slag met SQL Server op Azure VMs.
- Zie [SQL Server op een Azure virtuele machines: leerpad](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).
