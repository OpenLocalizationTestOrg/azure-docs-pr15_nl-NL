<properties
    pageTitle="Beschikbaarheid en herstel voor SQL Server | Microsoft Azure"
    description="Een discussie van de verschillende soorten HADR strategieën voor het SQL Server in Azure virtuele Machines wordt uitgevoerd."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Hoge beschikbaarheid herstel voor SQL Server in Azure virtuele Machines

## <a name="overview"></a>Overzicht

Microsoft Azure virtuele machines (VMs) met SQL Server kunt lagere kosten voor een hoge beschikbaarheid herstel (HADR) databaseoplossing. De meeste SQL Server HADR oplossingen worden ondersteund in Azure virtuele machines als Azure alleen-lezen en als hybride oplossingen. In een Azure-only-oplossing, de hele HADR wordt uitgevoerd in Azure wordt aangegeven. Een hybride configuratie heeft, onderdeel van de oplossing wordt in Azure en de andere deel wordt uitgevoerd on-premises in uw organisatie worden uitgevoerd. De flexibiliteit van de Azure-omgeving kunt u geheel of gedeeltelijk naar Azure gaan om te voldoen aan de budget en HADR vereisten van uw SQL Server-database-systemen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="understanding-the-need-for-an-hadr-solution"></a>Wat betekent dat u een oplossing HADR

Dit is aan u om ervoor te zorgen dat uw databasesysteem beschikt over de mogelijkheden van HADR die de service level agreement (SLA) is vereist. Het feit dat Azure biedt beschikbaarheid methoden, zoals service retoucheren voor cloudservices en mislukt herstel detectie voor de virtuele Machines, betekent niet zelf dat kunt u de gewenste SLA vergaderen. Deze regelingen beveiligen de beschikbaarheid van de VMs, maar niet de beschikbaarheid van SQL Server uitgevoerd binnen de VMs. Het is mogelijk voor de SQL Server-instantie mislukt terwijl de VM online en de orde is. Daarnaast wordt zelfs de beschikbaarheid regelingen verstrekt door Azure toestaan voor downtime van de VMs vanwege gebeurtenissen zoals herstel van software of hardwarefouten en upgrades voor besturingssystemen.

Daarnaast geografische overtollige opslag (GRS) in Azure wordt aangegeven, die wordt geïmplementeerd met een functie geografische-replicatie genoemd, zijn mogelijk niet een oplossing voor het herstellen van voldoende noodgevallen voor uw databases. Omdat geografische herhaling asynchroon gegevens verzendt, worden recente updates in het geval van noodgevallen verloren. Meer informatie aangaande geografische herhaling beperkingen worden beschreven in de sectie [geografische-replicatie wordt niet ondersteund voor gegevens en logboekbestanden op afzonderlijke schijven](#geo-replication-support) .

## <a name="hadr-deployment-architectures"></a>HADR implementatie architecturen

SQL Server HADR technologieën die worden ondersteund in Azure opnemen:

- [Altijd op van beschikbaarheidsgroepen](https://technet.microsoft.com/library/hh510230.aspx)
- [Spiegelen](https://technet.microsoft.com/library/ms189852.aspx)
- [Logboekbestanden](https://technet.microsoft.com/library/ms187103.aspx)
- [Back-up en herstellen met Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx)
- [Altijd op failovercluster exemplaren](https://technet.microsoft.com/library/ms189134.aspx)

Het is mogelijk om de technologieën samen om het implementeren van een SQL Server-oplossing met zowel beschikbaarheid en noodgevallen herstelmogelijkheden combineren. Afhankelijk van de technologie die u gebruikt, kan een hybride implementatie vragen om een VPN-tunnel met het Azure virtuele netwerk. De secties hierna uitgelegd u enkele van de architectuur van de implementatie voorbeeld.

## <a name="azure-only-high-availability-solutions"></a>Azure alleen-lezen: beschikbaarheid oplossingen

U kunt een oplossing beschikbaarheid hebt voor uw SQL Server-databases in Azure wordt aangegeven met altijd op beschikbaarheid van de groepen of database spiegelen.

| Technologie                               | Voorbeeld architecturen                    |
| ---------------------------------------- | ---------------------------------------- |
| **Altijd op van beschikbaarheidsgroepen**        | Alle beschikbaarheid replica's uitgevoerd in Azure VMs beschikbaarheid binnen dezelfde regio. U moet configureren van een domeincontroller VM, omdat Windows Server-Failover cluster (WSFC) een Active Directory-domein vereist.<br/> ![Altijd op van beschikbaarheidsgroepen](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Zie [Configureren altijd op beschikbaarheid van groepen in Azure-grafische (gebruikersinterface)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)voor meer informatie. |
| **Altijd op failovercluster exemplaren** | Failover Cluster exemplaren (FCI), waarvoor u gedeelde opslag, kunnen worden gemaakt op verschillende manieren 2.<br/><br/>1. een FCI op een twee knooppunten WSFC uitgevoerd in Azure VMs met opslag wordt ondersteund door een derde partij clustering oplossing. Zie voor een specifieke voorbeeld met SIOS DataKeeper, [beschikbaarheid van op een bestandsshare WSFC en 3e partijen software SIOS Datakeeper gebruiken](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>2. een FCI op een twee knooppunten WSFC met externe iSCSI Target in Azure VMs gedeelde blokkeren opslag via ExpressRoute. NetApp persoonlijke opslag (NPS) beschrijft bijvoorbeeld een iSCSI-doel via ExpressRoute met Equinix naar Azure VMs.<br/><br/>Voor gedeelde opslag van derden en gegevens herhaling oplossingen, moet u contact opnemen met de leverancier voor eventuele problemen die betrekking hebben op toegang tot gegevens op failover.<br/><br/>Houd er rekening mee dat gebruik FCI boven op [Azure bestandsopslag](https://azure.microsoft.com/services/storage/files/) wordt niet ondersteund nog, omdat deze oplossing geen gebruik van Premium opslag. We werken om te dit snel ondersteunen. |

## <a name="azure-only-disaster-recovery-solutions"></a>Azure alleen-lezen: noodgevallen hersteloplossingen

U kunt een oplossing voor het herstellen van noodgevallen voor uw SQL Server-databases in Azure wordt aangegeven met altijd op beschikbaarheidsgroepen, spiegelen van de database of back-up en herstellen met opslag BLOB's.

| Technologie                               | Voorbeeld architecturen                    |
| ---------------------------------------- | ---------------------------------------- |
| **Altijd op van beschikbaarheidsgroepen**        | Beschikbaarheid van replica's die over meerdere datacenters in Azure VMs voor herstel. Deze oplossing cross-regio beschermen tegen volledige site storing. <br/> ![Altijd op van beschikbaarheidsgroepen](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>In een gebied, moeten alle replica's worden opgenomen in de dezelfde cloudservice en de dezelfde VNet. Omdat elke regio een afzonderlijke VNet hebt wordt, moeten deze oplossingen VNet VNet connectivity. Zie [een VPN-verbinding van het Site-naar-Site in de portal van Azure klassieke configureren](../vpn-gateway/vpn-gateway-site-to-site-create.md)voor meer informatie. |
| **Spiegelen**                   | Hoofdsom en gespiegelde en wordt uitgevoerd in verschillende datacenters voor herstel servers. U moet implementeren met een servercertificaten omdat u een active directory-domein kan niet meerdere datacenters beslaan.<br/>![Spiegelen](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Back-up en herstellen met Azure Blob Storage-Service** | Productiedatabases back-up rechtstreeks met blob storage in een ander datacenter voor herstel.<br/>![Back-up en herstellen](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Zie [back-up en herstellen voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-backup-recovery.md)voor meer informatie. |

## <a name="hybrid-it-disaster-recovery-solutions"></a>Hybride IT: Noodgevallen hersteloplossingen

U kunt een noodgevallen hersteloplossing voor SQL Server-databases in een hybride-IT-omgeving met altijd op beschikbaarheidsgroepen, spiegelen, logboekbestanden en back-up en herstellen met Azure blog opslagmedia.

| Technologie                               | Voorbeeld architecturen                    |
| ---------------------------------------- | ---------------------------------------- |
| **Altijd op van beschikbaarheidsgroepen**        | Sommige beschikbaarheid replica's uitgevoerd in Azure VMs en andere replica's on-premises voor herstel van meerdere sites uitgevoerd. De productiesite beide on-premises implementatie kan zijn of in een Azure datacenter.<br/>![Altijd op van beschikbaarheidsgroepen](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Omdat alle beschikbaarheid replica in hetzelfde WSFC cluster zijn moeten, moet het cluster WSFC beide netwerken (een meerdere subnet WSFC cluster) omvatten. Deze configuratie is vereist een VPN-verbinding tussen Azure en de on-premises netwerk.<br/><br/>Voor een succesvolle herstel van uw databases, moet u ook een replicadomeincontroller op de site met noodgevallen herstel installeren.<br/><br/>Het is mogelijk gebruik van de Wizard Replica toevoegen in SSMS een Azure replica toevoegen aan een bestaande altijd op beschikbaarheid van de groep. Zie voor meer informatie zelfstudie: uw groep altijd-Klik op van beschikbaarheid aan Azure uitbreiden. |
| **Spiegelen**                   | Een partner uitgevoerd in een VM Azure en de andere actieve on-premises voor meerdere sites herstel servercertificaten gebruiken. Partners hoeft te worden in hetzelfde Active Directory-domein en geen VPN-verbinding is vereist.<br/>![Spiegelen](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Een partner uitgevoerd in een VM Azure en de andere actieve on-premises in hetzelfde Active Directory-domein voor meerdere sites herstel heeft betrekking op een andere database scenario spiegelen. Een [VPN-verbinding tussen het Azure virtuele netwerk en de on-premises netwerk](../vpn-gateway/vpn-gateway-site-to-site-create.md) is vereist.<br/><br/>Voor een succesvolle herstel van uw databases, moet u ook een replicadomeincontroller op de site met noodgevallen herstel installeren. |
| **Logboekbestanden**                         | Één server worden uitgevoerd in een VM Azure en de andere actieve on-premises voor herstel van meerdere sites. Logboekbestanden is afhankelijk van het Windows-bestanden delen, dus hoeft u een VPN-verbinding tussen het Azure virtuele netwerk en de on-premises netwerk.<br/>![Logboekbestanden](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Voor een succesvolle herstel van uw databases, moet u ook een replicadomeincontroller op de site met noodgevallen herstel installeren. |
| **Back-up en herstellen met Azure Blob Storage-Service** | On-premises implementatie productiedatabases back-up gemaakt rechtstreeks naar Azure-blobopslag voor herstel.<br/>![Back-up en herstellen](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Zie [back-up en herstellen voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-backup-recovery.md)voor meer informatie. |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Belangrijke overwegingen voor SQL Server HADR in Azure wordt aangegeven

Azure VMs, opslag en netwerken hebt verschillende operationele kenmerken dan een on-premises, niet-gevirtualiseerde IT-infrastructuur. Een succesvolle implementatie van een HADR SQL Server-oplossing in Azure wordt aangegeven, moet u deze verschillen begrijpen en uw oplossing zo ins dat ze ontwerpen.

### <a name="high-availability-nodes-in-an-availability-set"></a>Beschikbaarheid knooppunten in een set beschikbaarheid

Beschikbaarheid sets in Azure wordt aangegeven kunnen u de knooppunten beschikbaarheid in afzonderlijke foutenstructuuranalyse domeinen (FDs) en Update domeinen (UDs) plaatsen. Voor Azure VMs moet worden geplaatst in bepaalde beschikbaarheid, moet u deze in de dezelfde cloudservice implementeren. Alleen knooppunten in dezelfde cloudservice kunnen deelnemen aan dezelfde beschikbaarheid set. Zie [de beschikbaarheid van virtuele Machines beheren](virtual-machines-windows-manage-availability.md)voor meer informatie.

### <a name="wsfc-cluster-behavior-in-azure-networking"></a>WSFC cluster gedrag in Azure netwerken

De niet-RFC-compatibele DHCP-service in Azure kan leiden tot het maken van bepaalde WSFC clusterconfiguraties mislukt vanwege de naam van het cluster-netwerk een dubbele IP-adres, bijvoorbeeld hetzelfde IP-adres als een van de knooppunten wordt toegewezen. Dit is een probleem wanneer u altijd op beschikbaarheid van de groepen die afhankelijk van de functie WSFC is implementeren.

Houd rekening met het scenario wanneer een cluster met twee knooppunten wordt gemaakt en online gebracht:

1. Het cluster wordt geleverd online en klik vervolgens knooppunt1 aanvraagt een dynamisch toegewezen IP-adres van de naam van het cluster-netwerk.

2. Geen IP-adres dan knooppunt1 eigen IP-adres is opgegeven door de service DHCP, aangezien de DHCP-service herkent dat het verzoek afkomstig van knooppunt1 is zelf.

3. Windows wordt gedetecteerd dat een dubbel adres wordt toegewezen zowel knooppunt1 als de naam van het cluster-netwerk, of de standaardgroep cluster niet meer online komt.

4. De standaardgroep cluster wordt verplaatst naar knooppunt2, die wordt verwerkt van knooppunt1 IP-adres als het IP-adres van de cluster en levert met de cluster standaardgroep online.

5. Wanneer Knooppunt2 probeert om verbinding met knooppunt1, laat pakketten die zijn gericht op knooppunt1 nooit Knooppunt2 omdat deze wordt omgezet van knooppunt1 IP-adres in zelf. Knooppunt2 kan geen verbinding met knooppunt1, klikt u vervolgens quorum verliest en het cluster afgesloten.

6. Ondertussen knooppunt1 pakketten kunt verzenden naar knooppunt2, maar Knooppunt2 niet beantwoorden. Knooppunt1 quorum verliest en het cluster afgesloten.

Dit scenario kan worden voorkomen door het toewijzen van een niet-gebruikte statische IP-adres, zoals een link-local IP-adres zoals 169.254.1.1, aan de naam van het cluster netwerk om de naam van het cluster online te brengen. Als u wilt dit proces vereenvoudigen, raadpleegt u [Windows-failovercluster configureren in Azure wordt aangegeven voor altijd op beschikbaarheid van de groepen](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

Zie [Configureren altijd op beschikbaarheid van groepen in Azure-grafische (gebruikersinterface)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)voor meer informatie.

### <a name="availability-group-listener-support"></a>Beschikbaarheid van de groep luisteraar ervan af ondersteuning

Beschikbaarheid van de groep listeners worden ondersteund op Azure VMs met Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 of Windows Server 2016. Deze ondersteuning is mogelijk gemaakt door het gebruik van taakverdeling eindpunten de Azure VMs die beschikbaarheid van de groepsknooppunten zijn ingeschakeld. U kunt speciale configuratiestappen voor de listeners om te werken voor beide clienttoepassingen die worden uitgevoerd in Azure, evenals die met een on-premises implementatie moet volgen.

Er zijn twee belangrijkste opties voor het instellen van uw luisteraar ervan af: extern (openbaar) of interne. De externe (openbaar) luisteraar ervan af beschikt over een internetverbinding van taakverdeling en is gekoppeld aan een openbare virtuele IP-(VIP) die toegankelijk is via internet. Een interne luisteraar ervan af beschikt over een interne taakverdeling en alleen clients binnen hetzelfde virtuele netwerk ondersteunen. Voor de verdeling van type laden, moet u inschakelen directe Server als resultaat. 

Als de beschikbaarheid van de groep zich uitstrekt over meerdere Azure subnetten (zoals een distributie die Azure regio's kruist), de verbindingsreeks van de client moet opnemen "**MultisubnetFailover = True**". Hierdoor parallelle verbindingen met de replica's in de verschillende subnetten. Zie voor instructies over het instellen van een luisteraar ervan af

- [Configureren een ILB luisteraar ervan af voor altijd op beschikbaarheid van groepen in Azure wordt aangegeven](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
- [Een externe luisteraar ervan af voor altijd op beschikbaarheid van groepen in Azure configureren](virtual-machines-windows-classic-ps-sql-ext-listener.md).

U kunt nog steeds verbinden elke replica beschikbaarheid afzonderlijk door verbinding te maken rechtstreeks naar het exemplaar van de service. Aangezien altijd op beschikbaarheidsgroepen compatibel met clients spiegelen van de database zijn, kunt u kunt ook voor verbinden met de beschikbaarheid van replica's zoals partners spiegelen, zolang de replica's zijn geconfigureerd vergelijkbaar met het spiegelen van de database van de database:

- Een primaire replica en een secundaire

- De secundaire replica is geconfigureerd als niet-leesbare (**Leesbare secundaire** optie ingesteld op **Nee**)

Er is een voorbeeld van de client-verbindingsreeks die overeenkomt met deze database spiegelen-achtige-configuratie met ADO.NET- of SQL Server Native Client hieronder:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Zie voor meer informatie over clientconnectiviteit:

- [Gebruik van de verbinding tekenreeks trefwoorden met SQL Server Native Client](https://msdn.microsoft.com/library/ms130822.aspx)
- [Clients verbinden met een Database spiegelen sessie (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
- [Verbinding maken met de beschikbaarheid van de groep luisteraar ervan af in hybride IT](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
- [Beschikbaarheid van de groep Listeners, clientconnectiviteit en overname van de toepassing (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
- [Gebruik van spiegelen van de Database verbindingstekenreeksen met de van de beschikbaarheidsgroepen](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Netwerklatentie in hybride IT

U moet uw HADR-oplossing met aangenomen dat er mogelijk perioden met een met hoge netwerklatentie tussen uw on-premises netwerk en Azure implementeren. Wanneer u de replica's implementatie in Azure, moet u asynchroon doorvoeren in plaats van synchroon doorvoeren gebruiken voor de Synchronisatiemodus. Wanneer implementeert spiegelen servers van de database zowel on-premises en in Azure wordt aangegeven, kunt u de krachtige-modus gebruiken in plaats van de modus Hoog.

### <a name="geo-replication-support"></a>Geografische-replicatie-ondersteuning

Geografische-herhaling in Azure schijven biedt geen ondersteuning voor het gegevensbestand en het logboekbestand van dezelfde database worden opgeslagen op afzonderlijke schijven. GRS wordt overgenomen door wijzigingen weergeven op elke schijf onafhankelijk en asynchroon. Deze methode zorgt ervoor dat de volgorde schrijven binnen één schijf in het exemplaar geografische gerepliceerd, maar niet tussen geografische gerepliceerd kopieën van meerdere schijven. Als u een database voor het opslaan van het gegevensbestand en het logboekbestand op afzonderlijke schijven configureert, bevatten de herstelde schijven na een noodgevallen een bijgewerkte kopie van het gegevensbestand dan het logboekbestand, waarin de vooraf geschreven log in SQL Server en de ACID-eigenschappen van transacties eindemarkeringen. Als u niet de optie voor het uitschakelen van geografische-herhaling voor de opslag-account hebt, moet u alle gegevens- en logbestanden bestanden voor een opgegeven database op dezelfde schijf behouden. Als u meer dan één schijf vanwege de grootte van de database gebruiken moet, moet u een van de noodgevallen hersteloplossingen om ervoor te zorgen overbodige bovenstaande implementeren.

## <a name="next-steps"></a>Volgende stappen

Als u een Azure virtuele machines maken met SQL Server moet, raadpleegt u de [inrichting van een SQL Server virtuele Machine op Azure](virtual-machines-windows-portal-sql-server-provision.md).

Als u de beste prestaties vanuit SQL Server wordt uitgevoerd op een VM Azure, raadpleegt u de instructies in de [Prestaties aanbevolen procedures voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-performance.md).

Zie voor andere onderwerpen over met SQL Server in Azure VMs, [SQL Server op Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Overige informatiebronnen

- [Een nieuwe Active Directory-bos installeren in Azure wordt aangegeven](../active-directory/active-directory-new-forest-virtual-machine.md)
- [WSFC Cluster voor altijd op van beschikbaarheidsgroepen in Azure VM maken](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)
