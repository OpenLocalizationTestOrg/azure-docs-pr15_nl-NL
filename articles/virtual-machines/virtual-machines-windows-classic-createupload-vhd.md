<properties
    pageTitle="Maken en uploaden van de afbeelding van een VM via Powershell | Microsoft Azure"
    description="Leer hoe maken en een algemene afbeelding met Windows Server (VHD) te gebruiken met het implementatiemodel klassieke en Azure Powershell uploaden."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Maken en uploaden van een Windows Server VHD naar Azure

In dit artikel leest u hoe u uw eigen generalized VM afbeelding uploaden als een virtuele harde schijf (VHD), zodat u deze gebruiken kunt om te maken van virtuele machines. Zie voor meer informatie over schijven en VHD's in Microsoft Azure [over schijven en VHD's voor virtuele Machines](virtual-machines-linux-about-disks-vhds.md).


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]. U kunt ook [uploaden](virtual-machines-windows-upload-image.md) een virtuele machine met het model resourcemanager. 

## <a name="prerequisites"></a>Vereisten voor

In dit artikel wordt ervan uitgegaan dat u hebt:

- **Abonnement op een Azure** - als u niet hebt, kunt u de [gratis een Azure-account opent](/pricing/free-trial/?WT.mc_id=A261C142F).

- **[Microsoft Azure PowerShell](../powershell-install-configure.md)** - hebt u de Microsoft Azure PowerShell-module hebt geïnstalleerd en geconfigureerd voor gebruik van uw abonnement. 

- **A . VHD-bestand** : ondersteunde Windows-besturingssysteem die zijn opgeslagen in een VHD-bestand en die zijn gekoppeld aan een virtuele machine. Controleer als de serverrollen waarop de VHD worden ondersteund door Sysprep. Zie [Sysprep-ondersteuning voor serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)voor meer informatie.

> [AZURE.IMPORTANT] De indeling VHDX wordt niet ondersteund in Microsoft Azure. U kunt de schijf converteren naar VHD cv opmaken met behulp van Hyper-V Manager of de [converteren VHD cmdlet](http://technet.microsoft.com/library/hh848454.aspx). Zie voor meer informatie dit [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-the-vhd"></a>Stap 1: Voorbereiding de VHD 

Voordat u de VHD naar Azure uploaden, moet dit worden generalized met behulp van het hulpprogramma Sysprep. Dit zorgt ervoor dat de VHD moet worden gebruikt als een afbeelding. Zie voor meer informatie over Sysprep, [hoe u gebruik Sysprep: een inleiding](http://technet.microsoft.com/library/bb457073.aspx). Back-up van de VM voordat u Sysprep uitvoert.

Voer de volgende procedure uit de virtuele computer waar het besturingssysteem is geïnstalleerd:

1. Meld u aan bij het besturingssysteem.

2. Open een opdrachtpromptvenster als beheerder. De map wijzigen in **%windir%\system32\sysprep**en voer vervolgens `sysprep.exe`.

    ![Open een opdrachtpromptvenster](./media/virtual-machines-windows-classic-createupload-vhd/sysprep_commandprompt.png)

3.  Het dialoogvenster **Hulpprogramma voor het voorbereiden van systeem** wordt weergegeven.

    ![Sysprep starten](./media/virtual-machines-windows-classic-createupload-vhd/sysprepgeneral.png)

4.  In het **Hulpprogramma voor het voorbereiden van systeem**Selecteer **Voer systeem afwezigheidsberichten Box Experience (OOBE)** en controleert u of **Generalize** is ingeschakeld.

5.  Selecteer bij **Opties voor afsluiten** **Afsluiten**.

6.  Klik op **OK**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Stap 2: Maak een opslag-account en een container

Zodat u een locatie voor het uploaden van het VHD-bestand hebt, hebt u nodig met een account opslagruimte in Azure wordt aangegeven. Deze stap ziet u hoe u een account maken, of Ontvang de informatie die u nodig hebt van een bestaand account. Vervang de variabelen in &lsaquo; haken &rsaquo; door uw eigen gegevens.

1. Aanmelden

        Add-AzureAccount

1. Stel uw Azure-abonnement.

        Select-AzureSubscription -SubscriptionName <SubscriptionName> 

2. Maak een nieuw account voor de opslag. De naam van de opslag-account moet uniek zijn, 3-24 tekens. De naam is een combinatie van letters en cijfers. U moet ook Geef een locatie, zoals "Oost opnemen"
        
        New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>

3. Het nieuwe account voor de opslag als standaard instellen.
        
        Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>

4. Maak een nieuwe container.

        New-AzureStorageContainer -Name <ContainerName> -Permission Off

 

## <a name="step-3-upload-the-vhd-file"></a>Stap 3: Het VHD-bestand uploaden

Gebruik de [Toevoegen-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) de VHD uploaden.

Typ in het venster Azure PowerShell u in de vorige stap hebt gebruikt de volgende opdracht en vervangen van de variabelen in &lsaquo; haken &rsaquo; door uw eigen gegevens.

        Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>


## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>Stap 4: De afbeelding aan uw lijst met aangepaste afbeeldingen toevoegen

Gebruik de cmdlet [Toevoegen-AzureVMImage](https://msdn.microsoft.com/library/mt589167.aspx) naar de afbeelding toevoegen aan de lijst van uw aangepaste afbeeldingen.

        Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"


## <a name="next-steps"></a>Volgende stappen

U kunt nu [een aangepaste VM maken](virtual-machines-windows-classic-createportal.md) op basis van de afbeelding die u hebt geüpload.

