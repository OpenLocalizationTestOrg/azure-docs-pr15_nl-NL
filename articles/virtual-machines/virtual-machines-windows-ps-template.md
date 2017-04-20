<properties
    pageTitle="Een VM maken met een sjabloon resourcemanager | Microsoft Azure"
    description="Gebruik een resourcemanager sjabloon en een PowerShell eenvoudig maken een nieuwe virtuele Windows-computer."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-with-a-resource-manager-template"></a>Een virtuele Windows-computer met een resourcemanager-sjabloon maken

In dit artikel maakt u kennis met een sjabloon Azure resourcemanager en ziet u hoe u PowerShell gebruiken om te implementeren. De sjabloon implementeert één virtuele computer met Windows Server in een nieuwe virtuele netwerk met één subnet.

Het moet ongeveer 20 minuten duren moet de stappen in dit artikel.

> [AZURE.IMPORTANT] Als u wilt dat uw VM wilt toevoegen aan een set beschikbaarheid, dit toevoegen aan de set wanneer u de VM maakt. Er is momenteel niet een manier om een VM toevoegen aan een beschikbaarheid instellen nadat deze is gemaakt.

## <a name="step-1-create-the-template-file"></a>Stap 1: Het sjabloonbestand maken

U kunt uw eigen sjabloon met behulp van de informatie die zijn gevonden in [Azure resourcemanager Authoring sjablonen](../resource-group-authoring-templates.md)maken. U kunt ook sjablonen die zijn gemaakt voor u van [Azure QuickStart sjablonen](https://azure.microsoft.com/documentation/templates/)implementeren.

1. Open uw favoriete teksteditor en het vereiste schema-element en de vereiste contentVersion-element toevoegen:

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
        }
2. [Parameters](../resource-group-authoring-templates.md#parameters) zijn niet altijd vereist, maar ze zelf een formule invoerwaarden wanneer de sjabloon wordt geïmplementeerd. Het element parameters en de onderliggende elementen na het element contentVersion toevoegen:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

3. [Variabelen](../resource-group-authoring-templates.md#variables) kunnen worden gebruikt in een sjabloon kunt u waarden die regelmatig veranderen of waarden die moeten worden gemaakt op basis van een combinatie van parameterwaarden opgeven. Het element variabelen na de sectie parameters toevoegen:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }
        
4. [Resources](../resource-group-authoring-templates.md#resources) , zoals de virtuele machine, het virtuele netwerk en het account opslag worden vervolgens gedefinieerd in de sjabloon. De sectie bronnen na de sectie variabelen toevoegen:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvn1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
          
    >[AZURE.NOTE] In dit artikel wordt gemaakt van een virtuele machine met een versie van het besturingssysteem Windows Server. Zie voor meer informatie over het selecteren van de andere afbeeldingen, [navigeren en selecteer Azure virtuele machines afbeeldingen met Windows PowerShell en de CLI Azure](virtual-machines-linux-cli-ps-findimage.md).  
            
2. Sla het sjabloonbestand als *VirtualMachineTemplate.json*.

## <a name="step-2-create-the-parameters-file"></a>Stap 2: De parameterbestand maken

Waarden voor de resourceparameters die zijn gedefinieerd in de sjabloon wilt opgeven, een parameterbestand met de waarden die worden gebruikt wanneer de sjabloon wordt geïmplementeerd te maken.

1. In de teksteditor, kopieert u deze JSON-inhoud naar een nieuw bestand *Parameters.json*genoemd:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Zie meer informatie over [de vereisten voor de gebruikersnaam en wachtwoord](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).

2. Sla het parameterbestand.

## <a name="step-3-install-azure-powershell"></a>Stap 3: Azure PowerShell installeren

Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor informatie over de nieuwste versie van Azure PowerShell installeert, uw abonnement te selecteren en aanmelden bij uw account.

## <a name="step-4-create-a-resource-group"></a>Stap 4: Een resourcegroep maken

Alle resources moeten worden geïmplementeerd in een [resourcegroep](../azure-resource-manager/resource-group-overview.md).

1. Haal een lijst met beschikbare locaties waar resources kunnen worden gemaakt.

        Get-AzureRmLocation | sort DisplayName | Select DisplayName

2. Vervang de waarde van **$locName** door een locatie in de lijst, bijvoorbeeld **Central US**. Maak de variabele.

        $locName = "location name"
        
3. Vervang de waarde van **$rgName** door de naam van de nieuwe resourcegroep. Maak de variabele en de resourcegroep.

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
        
    Worden er ongeveer zo in dit voorbeeld:
    
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{subscription-id}/resourceGroups/myrg1

## <a name="step-5-create-the-resources-with-the-template-and-parameters"></a>Stap 5: De resources maken met de sjabloon en parameters

Vervang de waarde van **$templateFile** door het pad en de naam van het sjabloonbestand. Vervang de waarde van **$parameterFile** door het pad en de naam van het parameterbestand. Maak de variabelen en vervolgens implementeren de sjabloon. 

        $templateFile = "template file"
        $parameterFile = "parameter file"
        New-AzureRmResourceGroupDeployment -ResourceGroupName $rgName -TemplateFile $templateFile -TemplateParameterFile $parameterFile

    You will see something like this:

        DeploymentName    : VirtualMachineTemplate
        ResourceGroupName : myrg1
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2016 8:11:37 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            adminUsername    String                     mytestacct1
                            adminPassword    SecureString

        Outputs           :

>[AZURE.NOTE] U kunt ook sjablonen en parameters van een account Azure opslag implementeren. Zie [Azure PowerShell gebruiken met Azure opslagruimte](../storage/storage-powershell-guide-full.md)meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Als er problemen met de implementatie zijn, zou een volgende stap nagaan [Probleemoplossing resource groep implementaties met Azure-portal](../resource-manager-troubleshoot-deployments-portal.md)
- Informatie over het beheren van de virtuele machine die u aan de hand van [beheren virtuele machines met Azure resourcemanager en PowerShell](virtual-machines-windows-ps-manage.md)hebt gemaakt.
