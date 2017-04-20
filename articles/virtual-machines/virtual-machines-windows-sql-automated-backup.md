<properties
    pageTitle="Automatische back-up maken voor SQL Server virtuele Machines (resourcemanager) | Microsoft Azure"
    description="Dit artikel wordt uitgelegd de functie voor automatische back-up maken voor SQL Server in Azure virtuele Machines met Resource Manager wordt uitgevoerd. "
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
    ms.date="07/14/2016"
    ms.author="jroth" />

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatische back-up maken voor SQL Server in Azure virtuele Machines (resourcemanager)

> [AZURE.SELECTOR]
- [Resourcemanager](virtual-machines-windows-sql-automated-backup.md)
- [Klassieke](virtual-machines-windows-classic-sql-automated-backup.md)

[Back-up beheerd naar Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) automatische back-up automatisch geconfigureerd voor alle bestaande als nieuwe databases op een Azure VM met SQL Server 2014 Standard of Enterprise. Hiermee kunt u regelmatige back-ups die gebruikmaken van duurzame Azure-blobopslag configureren. Automatische back-up is afhankelijk van de [SQL Server IaaS agentextensie](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel. Zie [Automatische back-up maken voor SQL Server in Azure virtuele Machines klassieke](virtual-machines-windows-classic-sql-automated-backup.md)de lightversie van dit artikel.

## <a name="prerequisites"></a>Vereisten voor

Als u wilt gebruiken wordt automatisch back-up maken, kunt u overwegen de volgende vereisten:

**Besturingssysteem**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-versie/edition**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

**Configuratie van de database**:

- Doeldatabases moeten het model volledig herstel gebruiken

**Azure PowerShell**:

- [Installeer de meest recente Azure PowerShell-opdrachten](../powershell-install-configure.md) als u van plan bent voor het configureren van automatische back-up met PowerShell.

>[AZURE.NOTE] Automatische back-up is afhankelijk van de SQL Server IaaS agentextensie. Huidige SQL-VM afbeeldingen toevoegen dit toestel al dan niet standaard. Zie [SQL Server IaaS agentextensie](virtual-machines-windows-sql-server-agent-extension.md)voor meer informatie.

## <a name="settings"></a>Instellingen

De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor automatische back-up. De werkelijke configuratiestappen varieert afhankelijk van of u de Azure beheerportal of Azure Windows PowerShell-opdrachten gebruiken.

|Instelling|Bereik (standaard)|Beschrijving|
|---|---|---|
|**Automatische back-up**|(Uitgeschakeld) in-/ uitschakelen|Hiermee schakelt automatische back-up maken voor een Azure VM met SQL Server 2014 Standard of Enterprise of.|
|**Bewaarperiode**|1-30 dagen (30 dagen)|Het aantal dagen worden bewaard een back-up.|
|**Opslag-Account**|Azure opslag-account (de opslag-account hebt gemaakt voor de opgegeven VM)|Een Azure opslag-mailaccount wilt gebruiken voor het opslaan van bestanden van de back-up wordt automatisch in-blobopslag. Een container is op deze locatie voor de opslag van alle back-upbestanden gemaakt. De naamgevingsconventie op back-upbestand bevat de datum, tijd en de computernaam van de.|
|**Versleuteling**|(Uitgeschakeld) in-/ uitschakelen|Hiermee schakelt versleuteling of. Als versleuteling is ingeschakeld, wordt de certificaten gebruikt u de back-up herstelt bevinden zich in het opgegeven opslag-account in dezelfde automaticbackup container met de dezelfde naamgevingsconventie. Als het wachtwoord wijzigt, wordt een nieuw certificaat wordt gegenereerd met dat wachtwoord, maar wordt het oude certificaat blijft als u wilt herstellen voorafgaande back-ups.|
|**Wachtwoord**|Wachtwoord-tekst (geen)|Een wachtwoord voor versleuteling toetsen. Deze functie is alleen vereist als versleuteling is ingeschakeld. Als u wilt een versleutelde back-up herstellen, moet u het juiste wachtwoord en de gerelateerde certificaat dat is gebruikt op het moment dat de back-up is gemaakt.|

## <a name="configuration-in-the-portal"></a>Configuratie van in de Portal
U kunt de Portal Azure automatische back-up tijdens het inrichten of voor bestaande VMs configureren.

### <a name="new-vms"></a>Nieuwe VMs
Gebruik de Portal Azure configureren voor automatische back-up maken wanneer u een nieuwe SQL Server 2014 virtuele Machine in het implementatiemodel resourcemanager maakt.

Selecteer **Automatische back-up**in het blad **SQL Server-instellingen** . De volgende Azure portal schermafbeelding ziet u het blad **SQL automatische back-up** .

![Configuratie van de SQL automatische back-up in Azure-portal](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Zie het onderwerp voltooid op de [inrichting van een SQL Server-VM in Azure wordt aangegeven](virtual-machines-windows-portal-sql-server-provision.md)voor context.

### <a name="existing-vms"></a>Bestaande VMs
Voor bestaande SQL Server virtuele machines, selecteert u uw VM SQL Server. Selecteer het gedeelte **SQL Server-configuratie** van het blad **Instellingen** .

![SQL automatische back-up voor bestaande VMs](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

Klik in het blad **SQL Server configuration** op de knop **bewerken** in de automatische back-sectie.

![SQL automatische back-up configureren voor bestaande VMs](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Wanneer u klaar bent, klikt u op de knop **OK** vanaf de onderkant van het blad **SQL Server configuration** uw wijzigingen op te slaan.

Als u automatische back-up maken voor de eerste keer inschakelt, configureert Azure SQL Server IaaS Agent op de achtergrond. Tijdens deze periode, de portal van Azure mogelijk niet weergegeven dat automatische back-up is geconfigureerd. Wacht enkele minuten de agent worden geïnstalleerd, geconfigureerd. De portal van Azure worden daarna de nieuwe instellingen worden doorgevoerd.

>[AZURE.NOTE] U kunt ook automatische back-up maken van een sjabloon. Zie [Azure quickstart sjabloon voor automatische back-up](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update)voor meer informatie.

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell

Nadat uw SQL VM is geïnstalleerd, moet u PowerShell gebruiken voor het configureren van automatische back-up.

In het volgende PowerShell-voorbeeld, is automatische back-up geconfigureerd voor een bestaande 2014 VM voor SQL Server. De opdracht **AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig** configureert de automatische back-up-instellingen voor het opslaan van back-ups in het Azure opslag-account dat is gekoppeld aan de virtuele machine. Deze back-ups wordt tien dagen worden bewaard. De opdracht **Set-AzureRmVMSqlServerExtension** werkt de opgegeven Azure VM met deze instellingen.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriodInDays 10 -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Het kan enkele minuten duren installeren en configureren van de SQL Server IaaS Agent.

Wijzig het vorige script om aan te geven van de parameter **EnableEncryption** samen met een wachtwoord (secure tekenreeks) voor de parameter **CertificatePassword** om in te schakelen versleuteling. Het volgende script worden de instellingen voor automatische back-up in het vorige voorbeeld ingeschakeld en versleuteling toegevoegd.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Schakel automatische back-up uitvoeren hetzelfde script zonder de **-inschakelen** -parameter voor de opdracht **AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig** . Gebrek aan de **-inschakelen** parameter geeft de opdracht voor het uitschakelen van de functie. Als u met de installatie, kan het enkele minuten duren om uit te schakelen automatische back-up.

>[AZURE.NOTE] De SQL Server IaaS Agent worden niet verwijderd de eerder geconfigureerde automatische back-up-instellingen. Voordat u uitschakelen of verwijderen van de SQL Server IaaS Agent, moet u eerst automatische back-up uitschakelen.

## <a name="next-steps"></a>Volgende stappen

Automatische back-up configureren beheerde back-up op Azure VMs Dus is het belangrijk te [bekijken van de documentatie voor beheerde back-up maken](https://msdn.microsoft.com/library/dn449496.aspx) voor meer informatie over het gedrag en de implicaties.

U kunt zoeken naar extra back-up maken en terugzetten van richtlijnen voor SQL Server Azure VMs in het volgende onderwerp: [back-up en herstellen voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-backup-recovery.md).

Zie voor informatie over andere automatiseringstaken beschikbaar, [SQL Server IaaS agentextensie](virtual-machines-windows-sql-server-agent-extension.md).

Zie voor meer informatie over het uitvoeren van SQL Server op Azure VMs [SQL Server Azure virtuele Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md).
