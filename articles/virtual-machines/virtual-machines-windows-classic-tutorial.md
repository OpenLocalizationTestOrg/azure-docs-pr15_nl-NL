<properties
    pageTitle="Een VM maken in de klassieke portal | Microsoft Azure"
    description="Maak een virtuele Windows-computer in de portal van Azure klassieke."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="cynthn"/>

# <a name="create-a-virtual-machine-running-windows-in-the-azure-classic-portal"></a>Een virtuele machine waarop Windows wordt uitgevoerd in de portal van Azure klassieke maken

> [AZURE.SELECTOR]
- [Azure klassieke portal](virtual-machines-windows-classic-tutorial.md)
- [PowerShell: Klassieke implementatie](virtual-machines-windows-classic-create-powershell.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe u [deze stappen met het implementatiemodel resourcemanager uitvoeren](virtual-machines-windows-hero-tutorial.md) met behulp van de **nieuwe portal van Azure**. 

Deze zelfstudie ziet u hoe een Azure virtuele machine (VM) waarop Windows wordt uitgevoerd in de portal van Azure klassieke maken. We gebruiken de afbeelding van een Windows Server als voorbeeld, maar dat is slechts een van de vele afbeeldingen die Azure biedt. Houd er rekening mee dat uw keuzen afbeelding is afhankelijk van uw abonnement. Windows-bureaublad-installatiekopieën mogelijk bijvoorbeeld MSDN-abonnees beschikbaar.

In deze sectie wordt uitgelegd hoe u de optie **Uit galerie** in de portal van Azure klassieke de virtuele machine maken. Deze optie biedt meer configuratie opties dan de optie **Snelle maken** . Bijvoorbeeld als u deelnemen aan een virtuele machine met een virtueel netwerk wilt, moet u de optie **Uit galerie** te gebruiken.

U kunt ook [uw eigen afbeeldingen](virtual-machines-windows-classic-createupload-vhd.md)gebruiken VMs maken. Zie voor meer informatie over deze en andere methoden, [verschillende manieren om te maken van een virtuele Windows-computer](virtual-machines-windows-creation-choices.md).



## <a name="video-walkthrough"></a>Video Stapsgewijze instructies

Hier ziet u stapsgewijze instructies voor deze zelfstudie.

[AZURE.VIDEO creating-a-windows-vm-on-microsoft-azure-classic-portal]

## <a id="createvirtualmachine"> </a>De virtuele machine maken

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Volgende stappen

- Leer hoe u [een VM met het implementatiemodel resourcemanager maken](virtual-machines-windows-hero-tutorial.md) in de nieuwe portal van Azure. 

- Meld u aan bij de virtuele machine. Zie [aanmelden bij een virtuele machine met Windows Server](virtual-machines-windows-classic-connect-logon.md)voor instructies.

- Een schijf opslaan van gegevens hebt toegevoegd. U kunt zowel lege schijven en schijven met gegevens koppelen. Zie de [bijvoegen een gegevensschijf aan een virtuele Windows-computer die zijn gemaakt met het implementatiemodel klassieke](virtual-machines-windows-classic-attach-disk.md)voor instructies.
