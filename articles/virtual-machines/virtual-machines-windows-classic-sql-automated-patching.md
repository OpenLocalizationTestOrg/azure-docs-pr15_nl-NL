<properties
    pageTitle="Automatische patches voor SQL Server VMs (klassieke) | Microsoft Azure"
    description="Wordt de functie Automatische patches voor SQL Server virtuele Machines uitgevoerd in de klassieke implementatie-modus met Azure uitgelegd."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Automatische patches voor SQL Server in Azure virtuele Machines (klassieke)

> [AZURE.SELECTOR]
- [Resourcemanager](virtual-machines-windows-sql-automated-patching.md)
- [Klassieke](virtual-machines-windows-classic-sql-automated-patching.md)

Geautomatiseerde patch tot stand brengt een onderhoudsvenster voor een met SQL Server Azure virtuele Machine. Automatische Updates kunnen alleen worden geïnstalleerd tijdens dit onderhoudsvenster. Voor SQL Server, dit het zorgt ervoor dat de systeemupdates en alle bijbehorende opnieuw opstarten zich aan het best mogelijke tijdstip voor de database. Geautomatiseerde patch, is afhankelijk van de [SQL Server IaaS agentextensie](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]De Resource Manager-versie van dit artikel, Zie [Automatische patches voor SQL Server in Azure virtuele Machines resourcemanager](virtual-machines-windows-sql-automated-patching.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt gebruiken wordt automatisch herstellen, kunt u overwegen de volgende vereisten:

**Besturingssysteem**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-versie**:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

**Azure PowerShell**:

- [Installeer de meest recente Azure PowerShell-opdrachten](../powershell-install-configure.md).

**SQL Server IaaS extensie**:

- [De SQL Server IaaS extensie installeren](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Instellingen

De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor het automatisch herstellen. Voor klassieke VMs, moet u PowerShell gebruiken om deze instellingen te configureren.

|Instelling|Mogelijke waarden|Beschrijving|
|---|---|---|
|**Automatische herstellen**|(Uitgeschakeld) in-/ uitschakelen|Hiermee schakelt automatische herstellen voor een Azure virtuele machine of.|
|**Onderhoudsplanning**|Dagelijks maandag, dinsdag, woensdag, donderdag, vrijdag, zaterdag, zondag|De planning van Windows, SQL Server en Microsoft updates voor uw virtuele computer downloaden en installeren.|
|**Onderhoud start uur**|0-24|De lokale begintijd de virtuele machine bijwerken.|
|**Onderhoud venster duur**|30-180|Het aantal minuten toegestaan om uit te voeren van het downloaden en installeren van updates.|
|**Patch categorie**|Belangrijke|De categorie van updates downloaden en installeren.|

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell

In het volgende voorbeeld wordt PowerShell gebruikt voor het configureren van automatische herstellen op een bestaande SQL Server-VM. De opdracht **Nieuw AzureVMSqlServerAutoPatchingConfig** configureert een nieuw onderhoudsvenster voor automatische updates.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Op basis van dit voorbeeld, beschrijft de volgende tabel de praktisch effect op het doel Azure VM:

|Parameter|Effect|
|---|---|
|**Dag van de week**|Patches geïnstalleerd elke donderdag.|
|**MaintenanceWindowStartingHour**|Begin bijgewerkt om 11:00 am.|
|**MaintenanceWindowsDuration**|Patches moeten worden geïnstalleerd binnen 120 minuten. Op basis van de begintijd, moeten ze uitvoeren door 1:00 pm.|
|**PatchCategory**|De enige mogelijke instelling voor deze parameter is 'Belangrijk'.|

Het kan enkele minuten duren installeren en configureren van de SQL Server IaaS Agent.

Als u wilt herstellen wordt automatisch uitschakelen, uitvoeren hetzelfde script zonder de - parameter inschakelen voor de nieuwe-AzureVMSqlServerAutoPatchingConfig. Als u met de installatie, kan het enkele minuten duren om te herstellen wordt automatisch uitschakelen.

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over andere automatiseringstaken beschikbaar, [SQL Server IaaS agentextensie](virtual-machines-windows-classic-sql-server-agent-extension.md).

Zie voor meer informatie over het uitvoeren van SQL Server op Azure VMs [SQL Server Azure virtuele Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md).
