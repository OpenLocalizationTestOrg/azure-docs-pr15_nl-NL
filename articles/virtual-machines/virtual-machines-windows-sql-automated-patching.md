<properties
    pageTitle="Automatische patches voor SQL Server VMs (resourcemanager) | Microsoft Azure"
    description="Dit artikel wordt uitgelegd de functie Automatische patches voor SQL Server virtuele Machines uitgevoerd in Azure wordt aangegeven met Resource Manager."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatische patches voor SQL Server in Azure virtuele Machines (resourcemanager)

> [AZURE.SELECTOR]
- [Resourcemanager](virtual-machines-windows-sql-automated-patching.md)
- [Klassieke](virtual-machines-windows-classic-sql-automated-patching.md)

Geautomatiseerde patch tot stand brengt een onderhoudsvenster voor een met SQL Server Azure virtuele Machine. Automatische Updates kunnen alleen worden geïnstalleerd tijdens dit onderhoudsvenster. Voor SQL Server, deze rescriction zorgt ervoor dat de systeemupdates en alle bijbehorende opnieuw opstarten zich aan het best mogelijke tijdstip voor de database. Geautomatiseerde patch, is afhankelijk van de [SQL Server IaaS agentextensie](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel. Als u wilt weergeven in de lightversie van dit artikel, raadpleegt u [Automatisch patches voor SQL Server in Azure virtuele Machines klassieke](virtual-machines-windows-classic-sql-automated-patching.md).

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

- [Installeer de meest recente Azure PowerShell-opdrachten](../powershell-install-configure.md) als u wilt herstellen wordt automatisch configureren met PowerShell.

>[AZURE.NOTE] Geautomatiseerde patch, is afhankelijk van de SQL Server IaaS agentextensie. Huidige SQL-VM afbeeldingen toevoegen dit toestel al dan niet standaard. Zie [SQL Server IaaS agentextensie](virtual-machines-windows-sql-server-agent-extension.md)voor meer informatie.

## <a name="settings"></a>Instellingen

De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor het automatisch herstellen. De werkelijke configuratiestappen varieert afhankelijk van of u de Azure beheerportal of Azure Windows PowerShell-opdrachten gebruiken.

|Instelling|Mogelijke waarden|Beschrijving|
|---|---|---|
|**Automatische herstellen**|(Uitgeschakeld) in-/ uitschakelen|Hiermee schakelt automatische herstellen voor een Azure virtuele machine of.|
|**Onderhoudsplanning**|Dagelijks maandag, dinsdag, woensdag, donderdag, vrijdag, zaterdag, zondag|De planning van Windows, SQL Server en Microsoft updates voor uw virtuele computer downloaden en installeren.|
|**Onderhoud start uur**|0-24|De lokale begintijd de virtuele machine bijwerken.|
|**Onderhoud venster duur**|30-180|Het aantal minuten toegestaan om uit te voeren van het downloaden en installeren van updates.|
|**Patch categorie**|Belangrijke|De categorie van updates downloaden en installeren.|

## <a name="configuration-in-the-portal"></a>Configuratie van in de Portal
U kunt de portal van Azure wordt automatisch patches tijdens het inrichten of voor bestaande VMs configureren.

### <a name="new-vms"></a>Nieuwe VMs
Gebruik de portal van Azure configureren wordt automatisch herstellen wanneer u een nieuwe SQL Server virtuele Machine in het implementatiemodel resourcemanager maakt.

Selecteer **automatisch herstellen**in het blad **SQL Server-instellingen** . De volgende Azure portal schermafbeelding ziet u het blad **SQL wordt automatisch herstellen** .

![SQL wordt automatisch patches in Azure-portal](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Zie het onderwerp voltooid op de [inrichting van een SQL Server-VM in Azure wordt aangegeven](virtual-machines-windows-portal-sql-server-provision.md)voor context.

### <a name="existing-vms"></a>Bestaande VMs
Voor bestaande SQL Server virtuele machines, selecteert u uw VM SQL Server. Selecteer het gedeelte **SQL Server-configuratie** van het blad **Instellingen** .

![Automatische patches van SQL voor bestaande VMs](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

Klik op de knop **bewerken** in de sectie patches automatisch in het blad **SQL Server configuration** .

![SQL wordt automatisch patches voor bestaande VMs configureren](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Wanneer u klaar bent, klikt u op de knop **OK** vanaf de onderkant van het blad **SQL Server configuration** uw wijzigingen op te slaan.

Als u automatische patches voor de eerste keer inschakelt, configureert Azure SQL Server IaaS Agent op de achtergrond. Tijdens deze periode, de portal van Azure mogelijk niet weergegeven dat automatische patches is geconfigureerd. Wacht enkele minuten de agent worden geïnstalleerd, geconfigureerd. De portal van Azure weerspiegelt daarna de nieuwe instellingen.

>[AZURE.NOTE] U kunt ook configureren met een sjabloon wordt automatisch herstellen. Zie [Azure quickstart sjabloon voor automatische herstellen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update)voor meer informatie.

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell

Nadat uw SQL VM is geïnstalleerd, moet u PowerShell gebruiken voor het configureren van patches wordt automatisch.

In het volgende voorbeeld wordt PowerShell gebruikt voor het configureren van automatische herstellen op een bestaande SQL Server-VM. De opdracht **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig** configureert een nieuw onderhoudsvenster voor automatische updates.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

Op basis van dit voorbeeld, beschrijft de volgende tabel de praktisch effect op het doel Azure VM:

|Parameter|Effect|
|---|---|
|**Dag van de week**|Patches geïnstalleerd elke donderdag.|
|**MaintenanceWindowStartingHour**|Begin bijgewerkt om 11:00 am.|
|**MaintenanceWindowsDuration**|Patches moeten worden geïnstalleerd binnen 120 minuten. Op basis van de begintijd, moeten ze uitvoeren door 1:00 pm.|
|**PatchCategory**|De enige mogelijke instelling voor deze parameter is **Belangrijk**.|

Het kan enkele minuten duren installeren en configureren van de SQL Server IaaS Agent.

Als u wilt herstellen wordt automatisch uitschakelen, voer het dezelfde script zonder de **-inschakelen** -parameter voor de **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig**. Gebrek aan de **-inschakelen** parameter geeft de opdracht voor het uitschakelen van de functie.

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over andere automatiseringstaken beschikbaar, [SQL Server IaaS agentextensie](virtual-machines-windows-sql-server-agent-extension.md).

Zie voor meer informatie over het uitvoeren van SQL Server op Azure VMs [SQL Server Azure virtuele Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md).
