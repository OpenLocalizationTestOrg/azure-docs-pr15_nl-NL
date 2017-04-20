<properties
    pageTitle="Over installatiekopieën voor Windows virtuele machines | Microsoft Azure"
    description="Meer informatie over hoe afbeeldingen worden gebruikt met Windows virtuele machines in Azure wordt aangegeven."
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

# <a name="about-images-for-windows-virtual-machines"></a>Over installatiekopieën voor Windows virtuele machines

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-about-images](../../includes/virtual-machines-common-classic-about-images.md)]



## <a name="working-with-images"></a>Werken met afbeeldingen

U kunt de module Azure PowerShell gebruiken voor het beheren van de afbeeldingen die beschikbaar zijn voor uw Azure-abonnement. U kunt de portal van Azure klassieke ook gebruiken voor bepaalde taken afbeelding, maar de opdrachtregel geeft u meer opties.


-   **Alle afbeeldingen ophalen**:`Get-AzureVMImage`geeft als resultaat een lijst met alle afbeeldingen die beschikbaar zijn in uw huidige abonnement: uw afbeeldingen, evenals die Azure of partners gegeven. Dit betekent dat u een grote lijst kan worden weergegeven. De volgende voorbeelden wordt getoond hoe kom ik aan een kortere lijst.
-   **Afbeelding gezinnen ophalen**:`Get-AzureVMImage | select ImageFamily` ontvangt een lijst met afbeelding gezinnen door tekenreeksen **ImageFamily** eigenschap weer te geven.
-   **Alle afbeeldingen in een specifieke familie ophalen**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
-   **VM afbeeldingen zoeken**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` dit werkt door de eigenschap DataDiskConfiguration, die alleen van toepassing op VM afbeeldingen filteren. In dit voorbeeld wordt de uitvoer naar alleen de naam van label en afbeelding ook gefilterd.
-   **Een algemene afbeelding opslaan**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
-   **Een afbeelding van gespecialiseerde opslaan**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
>[Azure.Tip] De parameter OSState is vereist als u wilt maken van een afbeelding VM, waaronder gegevensschijven, evenals de schijf besturingssysteem gebruikt. Als u de parameter niet gebruikt, de cmdlet Hiermee maakt u een afbeelding OS. De waarde van de parameter wordt aangegeven of de afbeelding is generalized of speciale, gebaseerd op of de schijf besturingssysteem is bedoeld voor hergebruik.
-   **Een afbeelding verwijderen**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`


## <a name="next-steps"></a>Volgende stappen

U kunt ook [een Windows-computer met behulp van de klassieke portal maken](virtual-machines-windows-classic-tutorial.md)

