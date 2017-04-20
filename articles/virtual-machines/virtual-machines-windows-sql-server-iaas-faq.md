<properties
    pageTitle="SQL Server Azure virtuele Machines Veelgestelde vragen over | Microsoft Azure"
    description="In dit artikel vindt u antwoorden op veelgestelde vragen over het uitvoeren van SQL Server op Azure VMs."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-server-on-azure-virtual-machines-faq"></a>SQL Server Azure virtuele Machines Veelgestelde vragen

In dit onderwerp vindt u antwoorden op enkele veelgestelde vragen over het uitvoeren van [SQL Server op Azure virtuele Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

1. **Hoe maak ik een Azure virtuele machines met SQL Server?**

    Er zijn twee manieren u dit wilt doen. De gemakkelijkste oplossing is het opzetten van een virtuele Machine met SQL Server. Zie voor een zelfstudie over zich registreert voor Azure en een SQL VM maken op basis van de portal, [inrichten SQL Server in de Portal Azure virtuele machines](virtual-machines-windows-portal-sql-server-provision.md). U hebt ook de mogelijkheid om handmatig de installatie van SQL Server op een VM en hergebruiken van een on-premises implementatie-licentie met [Licentie mobiliteit via Software Assurance op Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. **Wat is het verschil tussen de SQL-VMs en de SQL-Database-service?**

    Conceptueel, SQL Server uitgevoerd op een Azure virtuele machine niet die verschilt van het SQL Server uitgevoerd in een externe datacenter. [SQL-Database](../sql-database/sql-database-technical-overview.md) biedt daarentegen database-als-een-service. Met SQL-Database, moet u toegang tot de machines die als host van uw databases niet hebben. Zie voor een volledige vergelijking [een wolk SQL Server-optie kiezen: (PaaS) van Azure SQL-Database of SQL Server Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Hoe kan ik mijn lokale SQL Server-database migreren naar de Cloud?**

    Maak eerst een Azure virtuele machines met een SQL Server-instantie. Uw on-premises implementatie-databases vervolgens migreren naar dit exemplaar. Zie voor gegevens migratiestrategieën, [migreren een SQL Server-database met SQL Server in een VM Azure](virtual-machines-windows-migrate-sql.md).

2. **Kan ik de geïnstalleerde functies wijzigen of een tweede exemplaar van SQL Server installeren op de dezelfde VM?**

    Ja. De SQL Server-installatiemedia bevindt zich in een map op station **C** . Hiermee voert u **Setup.exe** vanaf die locatie nieuwe exemplaren van SQL Server toevoegen of wijzigen van andere geïnstalleerde functies van SQL Server op de computer.

3. **Hoe kan ik upgraden naar een nieuwe versie/editie van de SQL Server in een VM Azure?**

    Er is momenteel geen in-place upgrade voor SQL Server wordt uitgevoerd in een VM Azure. Een nieuwe Azure virtuele machine maken met de gewenste versie/editie van SQL Server, en vervolgens uw databases te migreren naar de nieuwe server standard [gegevens migratie technieken](virtual-machines-windows-migrate-sql.md)gebruiken.

4. **Hoe kan ik mijn gelicentieerde exemplaar van SQL Server op een VM Azure installeren?**

    De SQL Server-installatiemedia voor Windows Server VM kopiëren en klik vervolgens op de VM installeren SQL Server. Voor de licenties redenen, moet u de [Licentie mobiliteit via Software Assurance op Azure](https://azure.microsoft.com/pricing/license-mobility/)hebben.

5. **Hebt u de te betalen van de SQL-kosten van een VM als dit alleen voor stand-by/failover gebruikt wordt?**

    Als u de SQL VM door de galerie maakt, moet u een aparte licentie hebt voor de stand-by SQL-VM en de prijzen is dezelfde. Als u SQL Server handmatig op een virtuele machine met [Licentie mobiliteit installeren](https://azure.microsoft.com/pricing/license-mobility/), hebt u de optie om een gratis passieve SQL-exemplaar voor failover. Raadpleeg de vereisten en beperkingen.

6. **Hoe worden updates en servicepacks toegepast op een SQL Server-VM?**

    Virtuele machines geven u meer controle over de host, zoals wanneer en hoe u updates toepassen. U kunt handmatig de windows-updates toepassen voor het besturingssysteem, of kunt u een planning service met de naam [Wordt automatisch patches](virtual-machines-windows-classic-sql-automated-patching.md)inschakelen. Automatische patch installaties alle updates die zijn gemarkeerd als belangrijk, met inbegrip van SQL Server-updates in die categorie. Andere optionele updates voor SQL Server, moeten handmatig worden geïnstalleerd.

7. **Is het mogelijk om in te stellen configuraties niet wordt weergegeven in de galerie VM (voor bijvoorbeeld Windows 2008 R2 + SQL Server 2012)?**

    Nee. Voor VM galerie plaatjes met SQL Server, moet u een van de meegeleverde afbeeldingen selecteren.

9. **Hoe installeer ik hulpmiddelen voor SQL-gegevens op mijn VM Azure?**

    Download en installeer de hulpmiddelen voor de SQL-gegevens van [Microsoft SQL Server Data Tools - Business Intelligence voor Visual Studio-2013 ](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Resources

Bekijk de video [Azure VM is de beste platform voor SQL Server-2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016)voor een overzicht van SQL Server op Azure virtuele Machines. U kunt ook een goede inleiding in het onderwerp, [SQL Server Azure virtuele Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md)openen.

Andere informatiebronnen:

- [Een SQL Server virtuele machine in de Portal Azure inrichten](virtual-machines-windows-portal-sql-server-provision.md)
- [Een Database migreren naar SQL Server op een Azure VM](virtual-machines-windows-migrate-sql.md)
- [Beschikbaarheid en herstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md)
- [Prestaties aanbevolen procedures voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-performance.md)
- [Toepassing patronen en ontwikkelingsstrategieën voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)
