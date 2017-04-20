<properties
    pageTitle="Back-up maken en terugzetten voor SQL Server | Microsoft Azure"
    description="Aandachtspunten voor de back-up en herstellen voor SQL Server-databases die zijn uitgevoerd op Azure virtuele Machines wordt beschreven."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Back-up en herstellen voor SQL Server in Azure virtuele Machines

## <a name="overview"></a>Overzicht

Back-ups van gegevens in SQL Server-databases, is een belangrijk onderdeel van de strategie in bescherming tegen verlies van gegevens door toepassing of gebruiker fouten. Dit geldt gelijkmatig voor SQL Server wordt uitgevoerd op Microsoft Azure virtuele Machines (VMs).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Voor SQL Server wordt uitgevoerd in Azure VMs, kunt u met systeemeigen back-up en herstellen met behulp van gekoppelde schijven voor de bestemming van de back-upbestanden technieken. Er is echter een beperking voor het aantal schijven die u aan een Azure virtuele machines toevoegen kunt, op basis van de [grootte van de virtuele machine](virtual-machines-linux-sizes.md). Er is ook de realiseren van schijf management u rekening moet houden.

Beginnen met SQL Server-2014, kunt u een back-up en herstellen met Microsoft Azure-blobopslag. SQL Server-2016 biedt ook verbeteringen voor deze optie. Daarnaast voor databasebestanden die zijn opgeslagen in Microsoft Azure-blobopslag, biedt SQL Server-2016 een optie voor bijna onmiddellijk opgeleverd back-ups en snelle Hiermee herstelt met Azure momentopnamen. In dit artikel krijgt u een overzicht van de volgende opties en aanvullende informatie vindt u op [back-up van SQL Server en deze herstellen met Microsoft Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx).

>[AZURE.NOTE] Zie voor een discussie van de opties voor back-ups van zeer grote databases, [Multi-Terabyte back-up SQL Server-Database strategieën voor Azure virtuele Machines](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).

De volgende secties bevatten informatie over de verschillende versies van SQL Server worden ondersteund in een Azure virtuele machines.

## <a name="sql-server-virtual-machines"></a>SQL Server virtuele Machines

Wanneer uw SQL Server-instantie wordt uitgevoerd op een Azure virtuele machines, kunnen databasebestanden al op gegevens voorkomen in Azure wordt aangegeven. Deze schijven live in Azure-blobopslag. Dus duren de redenen om een back-up van uw database en de benaderingen u wijzigen enigszins. Houd rekening met het volgende. 

- U langer niet back-ups om aan te bieden bescherming tegen hardware of media omdat Microsoft Azure deze bescherming als onderdeel van de Microsoft Azure-service biedt.

- Nog steeds moet u back-ups voor een bescherming tegen gebruikersfouten of voor archiveren, regulatorische redenen of administratieve doeleinden.

- U kunt de back-upbestand opslaan rechtstreeks in Azure wordt aangegeven. Zie de volgende secties die richtlijnen voor de verschillende versies van SQL Server geven voor meer informatie.

## <a name="sql-server-2016"></a>SQL Server 2016

Microsoft SQL Server-2016 ondersteunt functies voor [back-up en herstellen met Azure BLOB's](https://msdn.microsoft.com/library/jj919148.aspx) zijn gevonden in SQL Server-2014. Maar bevat ook de volgende verbeteringen:

| 2016 schaduweffecten               | Meer informatie                          |
|---------------------|-------------------------------|
| **Gesegmenteerd te verdelen**              | Wanneer u een back-up met Microsoft Azure-blobopslag, ondersteunt SQL Server-2016 een reservekopie op meerdere BLOB's om te schakelen back-ups van grote databases, tot maximaal 12,8 TB aan.      |
| **Momentopname back-up maken**                | Met behulp van Azure momentopnamen biedt back-up van SQL Server-bestand-momentopname bijna onmiddellijk opgeleverd back-ups en snelle Hiermee herstelt voor databasebestanden die zijn opgeslagen met behulp van de Azure Blob storage-service. Deze functie kunt u vereenvoudigen van de back-up en herstellen van beleid. Momentopname van de bestanden back-up ondersteunt ook punt in tijd herstellen. Zie voor meer informatie [Momentopname back-ups voor databasebestanden in Azure wordt aangegeven](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx).   |
| **Beheerde back-ups plannen**            | SQL Server beheerd back-up Azure biedt nu ondersteuning voor aangepaste schema's. Zie [SQL Server beheerd back-up Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx)voor meer informatie.   |

Zie voor een zelfstudie van de mogelijkheden van SQL Server-2016 bij gebruik van Azure-blobopslag [Zelfstudie: met de Microsoft Azure Blob storage-service met SQL Server-2016 databases](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL Server 2014

SQL Server-2014 bevat de volgende verbeteringen:

1. **Back-up en op Azure herstellen**:

 - *SQL Server back-up op URL* bevat nu ondersteuning in SQL Server Management Studio. De optie voor het back-up op Azure is nu beschikbaar wanneer u een back-up of herstellen taak of onderhoud abonnement wizard gebruikt in SQL Server Management Studio. Zie voor meer informatie, [SQL Server back-up op URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
 - *SQL Server beheerd Backup Azure* heeft nieuwe functies waarmee automatische back-management. Dit is vooral handig voor het automatiseren van back-beheer voor SQL Server-2014 exemplaren op een computer Azure. Zie [SQL Server beheerd back-up Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx)voor meer informatie.
 - *Automatische back-up* biedt aanvullende automatisering zodat u *SQL Server beheerd back-up Azure* automatisch voor alle bestaande als nieuwe databases voor een SQL Server-VM in Azure wordt aangegeven. Zie [Automatische back-up maken voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-automated-backup.md)voor meer informatie.
 - Zie voor een overzicht van de opties voor SQL Server 2014 Backup Azure, [SQL Server-back-up en herstellen met Microsoft Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).

1. **Versleuteling**: SQL Server 2014 ondersteunt coderen van gegevens bij het maken van een back-up. Ondersteunt verschillende versleutelingsalgoritmen en het gebruik een certificaat osf of asymmetrische sleutel. Zie [Back-up-codering](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx)voor meer informatie.

## <a name="sql-server-2012"></a>SQL Server 2012

Zie voor gedetailleerde informatie over het back-up van SQL Server en deze herstellen in SQL Server 2012, [back-up en herstellen van SQL Server-Databases (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Begin in SQL Server 2012 SP1 cumulatieve Update 2 en kunt u maximaal een back- en herstellen van de Azure Blob Storage-service. Deze uitbreiding kan worden gebruikt voor het back-up van SQL Server-databases op een SQL Server waarop een Azure virtuele machines of een exemplaar van de on-premises implementatie. Zie [SQL Server-back-up en herstellen met Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)voor meer informatie.

Enkele van de voordelen van het gebruik van de Azure Blob storage-service omvatten de mogelijkheid, worden omzeild de 16 limiet voor gekoppelde schijven gemakkelijk beheer, de directe beschikbaarheid van de back-upbestand met een ander exemplaar van SQL Server-instantie uitgevoerd op een Azure virtuele machines, of een exemplaar van de on-premises implementatie voor migratie of belangrijke bestanden verloren. Zie de sectie de *voordelen* van in [SQL Server-back-up en herstellen met Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)voor een volledige lijst van voordelen voor het gebruik van een Azure blob storage-service voor back-ups van SQL Server.

Zie voor aanbevolen aanbevelingen en informatie over probleemoplossing [back-up en herstellen aanbevolen procedures (Azure Blob Storage-Service)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008

Zie voor SQL Server back-up maken en deze herstellen in SQL Server 2008 R2, [back-ups maken en terugzetten Databases in SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

Zie voor SQL Server back-up maken en deze herstellen in SQL Server 2008, [back-ups maken en terugzetten Databases in SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Volgende stappen

Als u van plan bent de implementatie van SQL Server in een VM Azure, kunt u inrichten richtlijnen vinden in de volgende zelfstudie: [een SQL Server virtuele Machine op Azure met Azure resourcemanager inrichten](virtual-machines-windows-portal-sql-server-provision.md).

Hoewel back-up en herstellen kunnen worden gebruikt om uw gegevens te migreren, zijn er mogelijk beter migratiepaden naar SQL Server op een VM Azure. Zie [migreren van een Database met SQL Server op een VM Azure](virtual-machines-windows-migrate-sql.md)voor een volledige discussie van opties voor de migratie en aanbevelingen.

Overige [resources voor het uitvoeren van SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md)controleren.
