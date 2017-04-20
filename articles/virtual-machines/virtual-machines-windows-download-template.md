<properties
    pageTitle="Een afbeelding VM maken vanuit een VM Azure | Microsoft Azure"
    description="Informatie over het maken van de afbeelding van een generalized VM uit een bestaande Azure VM gemaakt in het implementatiemodel resourcemanager"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>


# <a name="download-the-template-for-a-vm"></a>Download de sjabloon voor een VM

Wanneer u een VM in Azure wordt aangegeven via de portal of PowerShell maakt, kan een sjabloon resourcemanager automatisch voor u wordt gemaakt. Gebruik deze sjabloon kunt u snel een implementatie dupliceren. De sjabloon bevat informatie over alle resources in een resourcegroep. Voor een virtuele machine, betekent dit dat de sjabloon containers alles dat wordt gemaakt ter ondersteuning van de VM in die resourcegroep, inclusief de netwerken resources.

## <a name="download-the-template-using-the-portal"></a>Download de sjabloon met behulp van de portal

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2. Een het menu hub **virtuele Machines**selecteren.
3. Selecteer de virtuele machine in de lijst.
5. Selecteer **automatiseringsscript**.
6. Selecteer **downloaden** en sla het ZIP-bestand op uw lokale computer.
7. Open het ZIP-bestand en pak de bestanden naar een map. Het ZIP-bestand bevat:
    
    - Deploy.ps1
    - Deploy.sh 
    - deployer.RB
    - DeploymentHelper.cs
    - parameters.JSON
    - Template.JSON

Het bestand .json is de sjabloon.
    
## <a name="download-the-template-using-powershell"></a>De sjabloon via PowerShell downloaden

U kunt ook het .json-sjabloonbestand met de cmdlet [Exporteren-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) downloaden. U kunt de `-path` -parameter voor de bestandsnaam en pad voor het bestand .json bieden. In dit voorbeeld ziet hoe u de sjabloon voor de resourcegroep met de naam **myResourceGroup** naar de map **C:\users\public\downloads** op uw lokale computer downloaden.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het implementeren van resources met sjablonen, [resourcemanager sjabloon Stapsgewijze instructies](../resource-manager-template-walkthrough.md).