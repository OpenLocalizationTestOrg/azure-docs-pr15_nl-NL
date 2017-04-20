<properties
   pageTitle="Een VM met een statische openbare IP-adres met een sjabloon in resourcemanager implementeren | Microsoft Azure"
   description="Informatie over het implementeren van VMs met een statische openbare IP-adres met een sjabloon in resourcemanager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-a-template"></a>Een VM met een statische openbare IP-adres met een sjabloon implementeren

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-resources-in-a-template-file"></a>Openbare IP-resources in een sjabloonbestand

U kunt weergeven en de [voorbeeldsjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json)downloaden.

Het gedeelte hierna ziet u de definitie van de openbare IP-resource, op basis van de bovenstaande scenario.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[variables('webVMSetting').pipName]",
        "location": "[variables('location')]",
        "properties": {
          "publicIPAllocationMethod": "Static"
        },
        "tags": {
          "displayName": "PublicIPAddress - Web"
        }
      },

U ziet u de eigenschap **publicIPAllocationMethod** , die is ingesteld op *statische*. Deze eigenschap kan zijn *dynamische* (standaardwaarde) of *statische*. Als u deze op statische garanties die het openbare IP-adres dat is toegewezen nooit wordt gewijzigd.

Het gedeelte hierna ziet u de koppeling van het openbare IP-adres met een netwerkinterface.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[variables('webVMSetting').nicName]",
        "location": "[variables('location')]",
        "tags": {
          "displayName": "NetworkInterface - Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
          "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
        ],
        "properties": {
          "ipConfigurations": [
            {
              "name": "ipconfig1",
              "properties": {
                "privateIPAllocationMethod": "Static",
                "privateIPAddress": "[variables('webVMSetting').ipAddress]",
                "publicIPAddress": {
                  "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
                },
                "subnet": {
                  "id": "[variables('frontEndSubnetRef')]"
                }
              }
            }
          ]
        }
      },

Zoals u ziet de **publicIPAddress** -eigenschap die verwijst naar de **Id** van een resource met de naam **variables('webVMSetting').pipName**. Dat is de naam van de openbare IP-resource hierboven.

Ten slotte wordt de bovenstaande netwerkinterface weergegeven in de eigenschap **networkProfile** van VM wordt gemaakt.

      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }

## <a name="deploy-the-template-by-using-click-to-deploy"></a>De sjabloon implementeren met behulp van Klik om te implementeren

De voorbeeldsjabloon die beschikbaar zijn in de openbare opslagplaats gebruikt een parameterbestand met de standaard waarden gebruikt voor het genereren van het scenario hierboven is beschreven. Als u wilt implementeren van deze sjabloon met Klik om te implementeren, klikt u op **Deploy naar Azure** in het bestand Readme.md voor de [VM met statische PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) -sjabloon. Vervang de standaardwaarden van de parameter desgewenst en voer waarden in voor de lege parameters.  Volg de instructies in de portal voor het maken van een virtuele machine met een statische openbare IP-adres.

## <a name="deploy-the-template-by-using-powershell"></a>De sjabloon implementeren via PowerShell

Als u wilt implementeren van de sjabloon die u hebt gedownload via PowerShell, de onderstaande stappen uit te voeren.

1. Als u nooit Azure PowerShell gebruikt nog, raadpleegt u [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) en volg de instructies in stap 1 tot en met 3.

2. In een PowerShell-console, voert u de cmdlet **New-AzureRmResourceGroup** als u wilt maken van een nieuwe resourcegroep, indien nodig. Als u al een resourcegroep die is gemaakt, gaat u naar stap 3.

        New-AzureRmResourceGroup -Name PIPTEST -Location westus

    Verwachte output:

        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. In een PowerShell-console, voert u de cmdlet **New-AzureRmResourceGroupDeployment** als u wilt implementeren van de sjabloon.

        New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
            -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
            -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json

    Verwachte output:

        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : <Deployment date> <Deployment time>
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0

        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         

        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>De sjabloon met behulp van de Azure CLI implementeren

Als u wilt implementeren de sjabloon met behulp van de Azure CLI, de onderstaande stappen uit te voeren.

1. Als u nooit Azure CLI gebruikt nog, volgt u de stappen in het artikel [installeren en configureren van de Azure CLI](../xplat-cli-install.md) en vervolgens de stappen voor de CLI verbinden met uw abonnement in het gedeelte 'Azure login gebruiken om te verifiëren interactief' van het artikel [verbinding maken met een Azure abonnement vanaf de opdrachtregel van Azure (Azure CLI)](../xplat-cli-connect.md) .
2. Voer de opdracht **azure config modus** om te schakelen naar resourcemanager-modus, zoals hieronder wordt weergegeven.

        azure config mode arm

    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

        info:    New mode is arm

3. Open het [parameterbestand](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), selecteert u de inhoud en sla deze op een bestand in uw computer. In dit voorbeeld worden de parameters naar een bestand met de naam *parameters.json*opgeslagen. De betreffende parameterwaarden in het bestand desgewenst wijzigen, maar ten minste het wordt aanbevolen dat u de waarde voor de parameter beheerderswachtwoord in een unieke, complexe wachtwoord wijzigen.

4. Voer de cmdlet **implementatie van azure groep maken** als u wilt implementeren van de nieuwe VNet met behulp van de sjabloon en parameter bestanden die u hebt gedownload en gewijzigd hierboven. Vervang in het onderstaande opdracht <path> door het pad u het bestand hebt opgeslagen. 

        azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json

    Verwachte output (lijsten parameterwaarden gebruikt):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/<Subscription ID>/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
