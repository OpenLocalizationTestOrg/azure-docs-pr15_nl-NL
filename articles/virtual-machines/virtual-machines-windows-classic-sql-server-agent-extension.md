<properties
    pageTitle="SQL Server-Agent-extensie voor SQL Server VMs (klassieke) | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe de SQL Server-agent-uitbreiding, die specifieke SQL Server-beheerderstaken automatiseert beheren. Het gaat hierbij om automatische back-up, wordt automatisch patches en Azure-toets kluis integratie. In dit onderwerp wordt gebruikt voor de implementatie van klassieke modus."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-classic"></a>SQL Server-Agent-extensie voor SQL Server VMs (klassieke)

> [AZURE.SELECTOR]
- [Resourcemanager](virtual-machines-windows-sql-server-agent-extension.md)
- [Klassieke](virtual-machines-windows-classic-sql-server-agent-extension.md)

De SQL Server IaaS agentextensie (SQLIaaSAgent) wordt uitgevoerd op Azure virtuele machines van de beheertaken kunt automatiseren. In dit onderwerp biedt een overzicht van de services die worden ondersteund door de extensie, evenals de instructies voor het installeren, status en verwijderen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zie [SQL Server Agent-extensie voor SQL Server VMs Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)de resourcemanager-versie van dit artikel.

## <a name="supported-services"></a>Ondersteunde services

De SQL Server IaaS agentextensie ondersteunt de volgende beheerderstaken:

| Van de beheerfunctie | Beschrijving |
|---------------------|-------------------------------|
| **SQL automatische back-up maken** | Automatiseert de planning van back-ups voor alle databases voor het standaardexemplaar van SQL Server in de VM. Zie [Automatische back-up voor SQL Server in Microsoft Azure virtuele Machines (klassieke)](virtual-machines-windows-classic-sql-automated-backup.md)voor meer informatie.|
| **SQL wordt automatisch herstellen** | Hiermee configureert u een onderhoudsvenster waarin updates voor uw VM plaatsvinden kunnen, zodat u updates tijdens tijden voor uw werkbelasting kunt voorkomen. Zie [Automatische patches voor SQL Server in Microsoft Azure virtuele Machines (klassieke)](virtual-machines-windows-classic-sql-automated-patching.md)voor meer informatie.|
| **Azure belangrijke kluis-integratie** | Kunt u automatisch installeren en configureren van Azure-toets kluis op uw SQL Server-VM. Zie [Azure toets kluis integratie configureren voor SQL Server op Azure VMs (klassieke)](virtual-machines-windows-classic-ps-sql-keyvault.md)voor meer informatie.|

## <a name="prerequisites"></a>Vereisten voor

Vereisten voor het gebruik van de SQL Server IaaS agentextensie op uw VM:

### <a name="operating-system"></a>Besturingssysteem:

- Windows Server 2012
- Windows Server 2012 R2

### <a name="sql-server-versions"></a>SQL Server-versies:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

### <a name="azure-powershell"></a>Azure PowerShell:

[Download en configureren van de meest recente Azure PowerShell-opdrachten](../powershell-install-configure.md).

Windows PowerShell starten en verbindt u deze aan uw Azure-abonnement met de opdracht **Toevoegen-AzureAccount** .

    Add-AzureAccount

Als u meerdere abonnementen hebt, gebruikt u **Selecteer AzureSubscription** om te selecteren van het abonnement dat met het doel-klassieke VM.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

U gaat nu een lijst met de klassieke virtuele machines en hun bijbehorende servicenamen met de opdracht **Get-AzureVM** .

    Get-AzureVM

## <a name="installation"></a>Installatie

Voor klassieke VMs, moet u PowerShell gebruiken voor het installeren van de SQL Server IaaS agentextensie en de bijbehorende services configureren. Gebruik de PowerShell-cmdlet **Set-AzureVMSqlServerExtension** voor het installeren van de extensie. Bijvoorbeeld de volgende opdracht uit de extensie installeert op een Windows Server VM (klassieke) en naam 'SQLIaaSExtension'.

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Als u naar de nieuwste versie van de extensie SQL IaaS Agent bijwerkt, moet u uw virtuele computer opnieuw opstarten na het bijwerken van de extensie.

>[AZURE.NOTE] Klassieke virtuele machines hebben geen optie voor het installeren en configureren van de extensie SQL IaaS Agent via de portal.

## <a name="status"></a>Status

Een manier om te controleren of de extensie is geïnstalleerd, is de status agent weergeven in de Portal Azure. **Alle instellingen** te selecteren in het blad VM en klik vervolgens op in **uitbreidingen**. Hier ziet u de extensie **SQLIaaSAgent** is vermeld.

![SQL Server IaaS Agent extensie in Azure-Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

U kunt ook de Azure Powershell-cmdlet **Get-AzureVMSqlServerExtension** gebruiken.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Verwijderen   

Klik in de Portal Azure kunt u de extensie verwijderen door te klikken op het beletselteken op het blad **uitbreidingen** van uw VM-eigenschappen. Klik vervolgens op **verwijderen**.

![Verwijderen van de SQL Server IaaS Agent extensie in Azure-Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

U kunt ook de **Verwijderen AzureVMSqlServerExtension** Powershell-cmdlet gebruiken.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Volgende stappen

Beginnen met een van de services die worden ondersteund door de extensie. Zie de onderwerpen waarnaar wordt verwezen in de sectie [services ondersteund](#supported-services) in dit artikel voor meer informatie.

Zie voor meer informatie over het uitvoeren van SQL Server op Azure virtuele Machines [SQL Server Azure virtuele Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md).
