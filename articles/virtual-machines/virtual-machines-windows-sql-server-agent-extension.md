<properties
    pageTitle="SQL Server-Agent-extensie voor SQL Server VMs (resourcemanager) | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe de SQL Server-agent-uitbreiding, die specifieke SQL Server-beheerderstaken automatiseert beheren. Het gaat hierbij om automatische back-up, wordt automatisch patches en Azure-toets kluis integratie. In dit onderwerp wordt gebruikt voor de Resource Manager implementatie-modus."
    services="virtual-machines-windows"
    documentationCenter=""
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
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-resource-manager"></a>SQL Server-Agent-extensie voor SQL Server VMs (resourcemanager)

> [AZURE.SELECTOR]
- [Resourcemanager](virtual-machines-windows-sql-server-agent-extension.md)
- [Klassieke](virtual-machines-windows-classic-sql-server-agent-extension.md)

De SQL Server IaaS agentextensie (SQLIaaSExtension) wordt uitgevoerd op Azure virtuele machines van de beheertaken kunt automatiseren. In dit onderwerp biedt een overzicht van de services die worden ondersteund door de extensie, evenals de instructies voor het installeren, status en verwijderen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel. Zie [SQL Server Agent-extensie voor SQL Server VMs klassieke](virtual-machines-windows-classic-sql-server-agent-extension.md)de lightversie van dit artikel.

## <a name="supported-services"></a>Ondersteunde services

De SQL Server IaaS agentextensie ondersteunt de volgende beheerderstaken:

| Van de beheerfunctie | Beschrijving |
|---------------------|-------------------------------|
| **SQL automatische back-up maken** | Automatiseert de planning van back-ups voor alle databases voor de standaardexemplaar van SQL Server in de VM. Zie [Automatische back-up voor SQL Server in Microsoft Azure virtuele Machines (resourcemanager)](virtual-machines-windows-sql-automated-backup.md)voor meer informatie.|
| **SQL wordt automatisch herstellen** | Hiermee configureert u een onderhoudsvenster waarin updates voor uw VM plaatsvinden kunnen, zodat u updates tijdens tijden voor uw werkbelasting kunt voorkomen. Zie [automatisch patches voor SQL Server in Microsoft Azure virtuele Machines (resourcemanager)](virtual-machines-windows-sql-automated-patching.md)voor meer informatie.|
| **Azure belangrijke kluis-integratie** | Kunt u automatisch installeren en configureren van Azure-toets kluis op uw SQL Server-VM. Zie [Azure toets kluis integratie configureren voor SQL Server op Azure VMs (resourcemanager)](virtual-machines-windows-ps-sql-keyvault.md)voor meer informatie.|

## <a name="prerequisites"></a>Vereisten voor

Vereisten voor het gebruik van de SQL Server IaaS agentextensie op uw VM:

**Besturingssysteem**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-versies**:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

**Azure PowerShell**:

- [Download en configureren van de meest recente Azure PowerShell-opdrachten](../powershell-install-configure.md)

## <a name="installation"></a>Installatie

De SQL Server IaaS agentextensie wordt automatisch geïnstalleerd wanneer u een van de afbeeldingen van SQL Server VM galerie inrichten.

Als u een VM alleen OS Windows Server hebt gemaakt, kunt u de extensie handmatig installeren met behulp van de PowerShell-cmdlet **Set-AzureVMSqlServerExtension** uit te voeren. Bijvoorbeeld de volgende opdracht uit de extensie installeert op een VM alleen OS Windows Server en naam 'SQLIaaSExtension'.

    Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2"

Als u naar de nieuwste versie van de extensie SQL IaaS Agent bijwerkt, moet u uw virtuele computer opnieuw opstarten na het bijwerken van de extensie.

>[AZURE.NOTE] Als u de SQL Server IaaS agentextensie handmatig op een Windows Server VM installeren, moet u gebruiken en beheren van de functies PowerShell-opdrachten gebruiken. De portal interface is alleen beschikbaar voor SQL Server-afbeeldingen.

## <a name="status"></a>Status

Een manier om te controleren of de extensie is geïnstalleerd, is de status agent weergeven in de Portal Azure. **Alle instellingen** te selecteren in het blad VM en klik vervolgens op in **uitbreidingen**. Hier ziet u de extensie **SQLIaaSExtension** is vermeld.

![SQL Server IaaS Agent extensie in Azure-Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

U kunt ook de Azure Powershell-cmdlet **Get-AzureVMSqlServerExtension** gebruiken.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

De vorige opdracht bevestigt de-agent is geïnstalleerd en vindt u informatie van de algemene status. U kunt ook de van de specifieke statusinformatie over automatische back-up- en patch met de volgende opdrachten openen.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Verwijderen   

Klik in de Portal Azure kunt u de extensie verwijderen door te klikken op het beletselteken op het blad **uitbreidingen** van uw VM-eigenschappen. Klik vervolgens op **verwijderen**.

![Verwijderen van de SQL Server IaaS Agent extensie in Azure-Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

U kunt ook de **Verwijderen AzureRmVMSqlServerExtension** Powershell-cmdlet gebruiken.

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Volgende stappen

Beginnen met een van de services die worden ondersteund door de extensie. Zie de onderwerpen waarnaar wordt verwezen in de sectie [services ondersteund](#supported-services) in dit artikel voor meer informatie.

Zie voor meer informatie over het uitvoeren van SQL Server op Azure virtuele Machines [SQL Server Azure virtuele Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md).
