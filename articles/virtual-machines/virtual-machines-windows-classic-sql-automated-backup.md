<properties
    pageTitle="Automatische back-up maken voor SQL Server virtuele Machines (klassieke) | Microsoft Azure"
    description="Dit artikel wordt uitgelegd de functie voor automatische back-up maken voor SQL Server in Azure virtuele Machines met Resource Manager wordt uitgevoerd. "
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

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Automatische back-up voor SQL Server in Azure virtuele Machines (klassieke)

> [AZURE.SELECTOR]
- [Resourcemanager](virtual-machines-windows-sql-automated-backup.md)
- [Klassieke](virtual-machines-windows-classic-sql-automated-backup.md)

[Back-up beheerd naar Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) automatische back-up automatisch geconfigureerd voor alle bestaande als nieuwe databases op een Azure VM met SQL Server 2014 Standard of Enterprise. Hiermee kunt u regelmatige back-ups die gebruikmaken van duurzame Azure-blobopslag configureren. Automatische back-up is afhankelijk van de [SQL Server IaaS agentextensie](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zie [Automatische back-up maken voor SQL Server in Azure virtuele Machines resourcemanager](virtual-machines-windows-sql-automated-backup.md)de resourcemanager-versie van dit artikel.

## <a name="prerequisites"></a>Vereisten voor

Als u wilt gebruiken wordt automatisch back-up maken, kunt u overwegen de volgende vereisten:

**Besturingssysteem**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-versie/edition**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

>[AZURE.NOTE] SQL Server-2016 wordt nog niet ondersteund voor automatische back-up.

**Configuratie van de database**:

- Doeldatabases, moeten het model volledig herstel gebruiken.

**Azure PowerShell**:

- [Installeer de meest recente Azure PowerShell-opdrachten](../powershell-install-configure.md).

**SQL Server IaaS extensie**:

- [De SQL Server IaaS extensie installeren](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Instellingen

De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor automatische back-up. Voor klassieke VMs, moet u PowerShell gebruiken om deze instellingen te configureren.

|Instelling|Bereik (standaard)|Beschrijving|
|---|---|---|
|**Automatische back-up**|(Uitgeschakeld) in-/ uitschakelen|Hiermee schakelt automatische back-up maken voor een Azure VM met SQL Server 2014 Standard of Enterprise of.|
|**Bewaarperiode**|1-30 dagen (30 dagen)|Het aantal dagen worden bewaard een back-up.|
|**Opslag-Account**|Azure opslag-account (de opslag-account hebt gemaakt voor de opgegeven VM)|Een Azure opslag-mailaccount wilt gebruiken voor het opslaan van bestanden van de back-up wordt automatisch in-blobopslag. Een container is op deze locatie voor de opslag van alle back-upbestanden gemaakt. De naamgevingsconventie op back-upbestand bevat de datum, tijd en de computernaam van de.|
|**Versleuteling**|(Uitgeschakeld) in-/ uitschakelen|Hiermee schakelt versleuteling of. Als versleuteling is ingeschakeld, wordt de certificaten gebruikt u de back-up herstelt bevinden zich in het opgegeven opslag-account in dezelfde automaticbackup container met de dezelfde naamgevingsconventie. Als het wachtwoord wijzigt, wordt een nieuw certificaat wordt gegenereerd met dat wachtwoord, maar wordt het oude certificaat blijft als u wilt herstellen voorafgaande back-ups.|
|**Wachtwoord**|Wachtwoord-tekst (geen)|Een wachtwoord voor versleuteling toetsen. Deze functie is alleen vereist als versleuteling is ingeschakeld. Als u wilt een versleutelde back-up herstellen, moet u het juiste wachtwoord en de gerelateerde certificaat dat is gebruikt op het moment dat de back-up is gemaakt.|

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell

In het volgende PowerShell-voorbeeld, is automatische back-up geconfigureerd voor een bestaande 2014 VM voor SQL Server. De opdracht **Nieuw AzureVMSqlServerAutoBackupConfig** configureert de automatische back-up-instellingen voor het opslaan van back-ups in het opgegeven door de variabele $storageaccount Azure opslag-account. Deze back-ups wordt tien dagen worden bewaard. De opdracht **Set-AzureVMSqlServerExtension** werkt de opgegeven Azure VM met deze instellingen.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Het kan enkele minuten duren installeren en configureren van de SQL Server IaaS Agent.

Wijzig het vorige script om aan te geven van de parameter EnableEncryption samen met een wachtwoord (secure tekenreeks) voor de parameter CertificatePassword om in te schakelen versleuteling. Het volgende script worden de instellingen voor automatische back-up in het vorige voorbeeld ingeschakeld en versleuteling toegevoegd.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Schakel automatische back-up uitvoeren hetzelfde script zonder de **-inschakelen** -parameter voor de **Nieuwe AzureVMSqlServerAutoBackupConfig**. Als u met de installatie, kan het enkele minuten duren om uit te schakelen automatische back-up.

>[AZURE.NOTE] Verwijdert de eerder geconfigureerde beheerde back-up-instellingen niet uitschakelen en de installatie van SQL Server IaaS Agent ongedaan maken. Voordat u uitschakelen of verwijderen van de SQL Server IaaS Agent, moet u eerst automatische back-up uitschakelen.

## <a name="next-steps"></a>Volgende stappen

Automatische back-up configureren beheerde back-up op Azure VMs Dus is het belangrijk te [bekijken van de documentatie voor beheerde back-up maken](https://msdn.microsoft.com/library/dn449496.aspx) voor meer informatie over het gedrag en de implicaties.

U kunt zoeken naar extra back-up maken en terugzetten van richtlijnen voor SQL Server Azure VMs in het volgende onderwerp: [back-up en herstellen voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-backup-recovery.md).

Zie voor informatie over andere automatiseringstaken beschikbaar, [SQL Server IaaS agentextensie](virtual-machines-windows-classic-sql-server-agent-extension.md).

Zie voor meer informatie over het uitvoeren van SQL Server op Azure VMs [SQL Server Azure virtuele Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md).
