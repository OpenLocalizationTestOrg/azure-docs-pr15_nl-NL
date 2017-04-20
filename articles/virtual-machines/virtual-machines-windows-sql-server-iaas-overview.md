<properties
    pageTitle="Overzicht van SQL Server op Azure virtuele Machines | Microsoft Azure"
    description="Meer informatie over het uitvoeren van volledige edities van SQL Server op Azure virtuele machines. Directe koppelingen naar alle SQL Server VM afbeeldingen en gerelateerde inhoud ophalen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/19/2016"
    ms.author="jroth"/>

# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Overzicht van SQL Server op Azure virtuele Machines

In dit onderwerp worden de opties voor het uitvoeren van SQL Server op Azure virtuele machines (VMs), samen met [koppelingen naar portal afbeeldingen](#option-1-create-a-sql-vm-with-per-minute-licensing) en een overzicht van [veelvoorkomende taken](#manage-your-sql-vm).

>[AZURE.NOTE] Als u al vertrouwd met SQL Server bent en u zien hoe wilt u een SQL Server-VM implementeren, raadpleegt u [inrichten SQL Server in de portal van Azure virtuele machines](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="overview"></a>Overzicht
Als u de databasebeheerder van een of een ontwikkelaar bent, bieden Azure VMs een manier om uw on-premises implementatie SQL Server werkbelasting en toepassingen in de Cloud te verplaatsen. De volgende video bevat een technisch overzicht van SQL Server Azure VMs.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

De video heeft betrekking op de volgende stadia:

|Tijd|Gebied|
|---|---|
| 00:21 | Wat zijn Azure VMs? |
| 01:45 | Beveiliging |
| 02:50 | Connectiviteit |
| 03:30 | Opslag betrouwbaarheid en prestaties |
| 05:20 | VM grootte |
| 05:54 | Beschikbaarheid en SLA |
| 07:30 | Configuratie-ondersteuning |
| 08:00 | Cmdlets voor controle |
| 08:32 | Demo: Een SQL Server-2016 VM maken |

>[AZURE.NOTE] De video bevat informatie over SQL Server-2016, maar Azure VM afbeeldingen biedt voor veel versies van SQL Server, inclusief 2008, 2012 2014 en 2016. 

## <a name="scenarios"></a>Scenario 's
Er zijn verschillende redenen die u kiezen mogelijk voor het hosten van uw gegevens in Azure wordt aangegeven. Als uw toepassing wordt verplaatst naar Azure, verbetert de prestaties als u wilt de gegevens ook verplaatsen. Maar er zijn andere voordelen. U hebt automatisch toegang tot meerdere datacenters voor een algemene aanwezigheids- en noodgevallen herstel. De gegevens is ook zeer beveiligde en duurzame.

SQL Server Azure VMs waarop is één optie voor het opslaan van relationele gegevens in Azure wordt aangegeven. Dit is een goede keuze voor verschillende scenario's. U wilt bijvoorbeeld de VM Azure ook mogelijk om een lokale SQL Server-computer te configureren. Of u mogelijk wilt uitvoeren van extra toepassingen en services op dezelfde databaseserver. Er zijn twee belangrijke resources die u nog meer scenario's en aandachtspunten voor de bedenkt kunnen helpen:

 - [SQL Server op Azure virtuele machines](https://azure.microsoft.com/services/virtual-machines/sql-server/) biedt een overzicht van de beste scenario's voor het gebruik van SQL Server op Azure VMs. 
 - [Een wolk SQL Server-optie kiezen: (PaaS) van Azure SQL-Database of SQL Server Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) bevat een gedetailleerde vergelijking tussen SQL-Database en SQL Server wordt uitgevoerd op een VM.

## <a name="create-a-new-sql-vm"></a>Een nieuwe SQL VM maken
De volgende secties bevatten directe koppelingen naar de portal van Azure voor de afbeeldingen van SQL Server VM galerie. Afhankelijk van de afbeelding die u selecteert, kunt u een van beide betaald wordt voor SQL Server licensing kosten op basis van de per minuut of u kunt uw eigen licentie (BYOL) overbrengen.

In de zelfstudie [inrichten SQL Server in de portal van Azure virtuele machines](virtual-machines-windows-portal-sql-server-provision.md)vindt u stapsgewijze instructies voor dit proces. Bekijk tevens de [prestaties aanbevolen procedures voor SQL Server VMs](virtual-machines-windows-sql-performance.md), waarin wordt uitgelegd hoe u de juiste machine grootte en andere functies die beschikbaar zijn tijdens het inrichten selecteren.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>Optie 1: Maak een VM SQL met licenties per minuut
De volgende tabel bevat een matrix van beschikbare SQL Server-afbeeldingen in de galerie VM. Klik op een koppeling naar begint met het maken van een nieuwe SQL VM met de opgegeven versie, edition en besturingssysteem gebruikt.

|Versie|Besturingssysteem|Edition|
|---|---|---|
|**SQL-2016**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [standaard](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [ontwikkelaar](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL-2014 SP1**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [standaard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL-2014**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [standaard](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [standaard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [standaard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [standaard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2 SP3**|Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [standaard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 SP3**|Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## <a name="option-2-create-a-sql-vm-with-an-existing-license"></a>Optie 2: Een VM SQL maken met een bestaande licentie
U kunt ook uw eigen licentie (BYOL) overbrengen. In dit scenario betaalt u alleen voor de VM zonder eventuele extra kosten voor SQL Server-licentieverlening. Om uw eigen licentie de matrix van SQL Server-versies, edities en besturingssystemen onderstaande te gebruiken. Klik in de portal worden deze afbeelding-namen voorafgegaan door **{BYOL}**.

|Versie|Besturingssysteem|Edition|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [standaard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [standaard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [standaard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

> [AZURE.IMPORTANT] Als u wilt BYOL VM afbeeldingen gebruikt, moet u en Enterprise Agreement met [Licentie mobiliteit via Software Assurance op Azure](https://azure.microsoft.com/pricing/license-mobility/). U kunt ook een geldige licentie nodig voor de versie/editie van SQL Server die u wilt gebruiken. Binnen **10** dagen van de inrichting van uw VM moet u [de benodigde BYOL informatie naar Microsoft](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) .

## <a name="manage-your-sql-vm"></a>Uw SQL-VM beheren
Nadat uw SQL Server-VM is geïnstalleerd, zijn er verschillende optioneel beheertaken. In veel aspecten, die u kunt configureren en beheren van SQL Server, precies zoals u zou een lokale SQL Server-instantie beheren. Sommige taken zijn echter specifiek zijn voor Azure. De volgende gedeelten sommige van deze gebieden met koppelingen naar meer informatie.

### <a name="connect-to-the-vm"></a>Verbinding maken met de VM
Een van de meest basisstappen management is verbinding maken met uw SQL Server-VM tot en met hulpprogramma's, zoals SQL Server Management Studio (SSMS). Zie voor instructies over het verbinding maken met uw nieuwe SQL Server-VM, [verbinden aan een virtuele Machine op Azure van SQL Server](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Migreren van uw gegevens
Als u een bestaande database hebt, moet u die verplaatsen naar de nieuw ingerichte SQL VM wilt. Zie [migreren van een Database met SQL Server op een VM Azure](virtual-machines-windows-migrate-sql.md)voor een lijst met opties voor de migratie en instructies.

### <a name="configure-high-availability"></a>Beschikbaarheid configureren
Als u beschikbaarheid vereist, kunt u het configureren van SQL Server-beschikbaarheid van de groepen. Dit heeft betrekking op meerdere Azure VMs in een virtueel netwerk. De Azure-portal bevat een sjabloon die deze configuratie voor u ingesteld. Zie [een groep AlwaysOn beschikbaarheid in resourcemanager Azure virtuele machines configureren](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)voor meer informatie. Als u wilt handmatig configureren van uw groep beschikbaarheid en de bijbehorende luisteraar ervan af, raadpleegt u [AlwaysOn beschikbaarheid van groepen in Azure VM configureren](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Zie voor andere aandachtspunten voor de beschikbaarheid, [beschikbaarheid en herstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Een back-up van uw gegevens
Azure VMs kunnen profiteren van [Automatische back-up](virtual-machines-windows-sql-automated-backup.md)die regelmatig back-ups van uw database met blob storage maakt. U kunt ook handmatig deze techniek gebruiken. Zie [Gebruik Azure opslag voor SQL Server-back-up en herstellen](virtual-machines-windows-use-storage-sql-server-backup-restore.md)voor meer informatie. Zie voor een overzicht van alle opties voor back-up en herstellen, [back-up en herstellen voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Updates automatiseren
Azure VMs kunnen gebruiken [Wordt automatisch herstellen](virtual-machines-windows-sql-automated-patching.md) om een onderhoudsvenster voor de installatie van belangrijke windows plannen en SQL Server automatisch wordt bijgewerkt.

### <a name="customer-experience-improvement-program-ceip"></a>Programma voor kwaliteitsverbetering (CEIP)
Het programma (Kwaliteitsverbetering) is standaard ingeschakeld. Dit verzendt periodiek rapporten naar Microsoft te verbeteren SQL Server. Er is geen beheertaak met CEIP vereist, tenzij u uitschakelen wilt nadat is geïnstalleerd. U kunt aanpassen of het programma voor Kwaliteitsverbetering uitschakelen door te verbinden met de VM met extern bureaublad. Voer het hulpprogramma **SQL Server-fout en gebruik rapporteren** uit. Volg de instructies voor het melden van uitschakelen. 

Zie voor meer informatie de sectie programma voor Kwaliteitsverbetering van het onderwerp [Licentievoorwaarden accepteren](https://msdn.microsoft.com/library/ms143343.aspx) . 

## <a name="next-steps"></a>Volgende stappen
[Verkennen leerpad](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) voor SQL Server op Azure virtuele machines.

Meer vraag? Zie eerst de [SQL Server Azure virtuele Machines Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md). Maar ook uw vragen of opmerkingen toevoegen aan de onderkant van elke SQL VM-onderwerpen om te communiceren met Microsoft en de community.
