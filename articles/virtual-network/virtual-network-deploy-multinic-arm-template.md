<properties
   pageTitle="Meerdere NIC VMs met een sjabloon in resourcemanager implementeren | Microsoft Azure"
   description="Leer hoe u meerdere NIC VMs met een sjabloon in resourcemanager implementeren"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="deploy-multi-nic-vms-using-a-template"></a>Meerdere NIC VMs met een sjabloon implementeren

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Aangezien op dit moment niet kunt u VMs met een enkel NIC en VMs met meerdere NIC's in dezelfde resourcegroep hebt, voert u de back-end-servers in een resourcegroep en alle andere onderdelen in een andere beveiligingsgroep. De onderstaande stappen gebruiken een resourcegroep benoemde *IaaSStory* voor de belangrijkste resourcegroep en *IaaSStory-BackEnd* voor de back-end-servers.

## <a name="prerequisites"></a>Vereisten voor

Voordat u de back-end-servers implementeren kunt, moet u de belangrijkste resourcegroep met alle benodigde bronnen voor dit scenario implementeren. Als u wilt deze resources implementeert, de onderstaande stappen uit te voeren.

1. Navigeer naar [de sjabloonpagina](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Klik op **Deploy naar Azure**op de sjabloonpagina aan de rechterkant van de **bovenliggende resourceveld groep**.
3. Indien nodig, de betreffende parameterwaarden wijzigen en voert u de stappen in de portal Azure voorbeeld om te implementeren van het resourceveld groep.

> [AZURE.IMPORTANT] Zorg ervoor dat uw opslagruimte accountnamen uniek zijn. Er geen dubbele opslag accountnamen in Azure wordt aangegeven.

## <a name="understand-the-deployment-template"></a>Meer informatie over de implementatiesjabloon

Controleer voordat u de sjabloon van deze documentatie implementeert, of dat u begrijpen hoe het werkt. De onderstaande stappen biedt een goed overzicht van de betreffende sjabloon.

1. Navigeer naar [de sjabloonpagina](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Klik op **azuredeploy.json** om de sjabloonbestand te openen.
3. U ziet u de onderstaande *waardereeksen* -parameter. Deze parameter wordt gebruikt om te selecteren welk VM afbeelding wilt gebruiken voor de database-server, samen met meerdere besturingssysteem gerelateerde instellingen.

        "osType": {
          "type": "string",
          "defaultValue": "Windows",
          "allowedValues": [
            "Windows",
            "Ubuntu"
          ],
          "metadata": {
            "description": "Type of OS to use for VMs: Windows or Ubuntu."
          }
        },

4. Schuif omlaag naar de lijst met variabelen en schakel de definitie voor de variabelen **dbVMSetting** onderstaande. Deze ontvangt een van de matrixelementen in de variabele **dbVMSettings** . Als u bekend met software development terminologie bent, kunt u de variabele **dbVMSettings** als hashtable of een dictionay weergeven.

        "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"

5. Stel dat u wilt implementeren Windows VMs SQL uitgevoerd in de back-end. Vervolgens de waarde voor **waardereeksen** *Windows*wordt, en de variabele **dbVMSetting** zou bevatten het element hieronder worden weergegeven dat de eerste waarde in de **dbVMSettings** variabele vertegenwoordigt.

          "Windows": {
            "vmSize": "Standard_DS3",
            "publisher": "MicrosoftSQLServer",
            "offer": "SQL2014SP1-WS2012R2",
            "sku": "Standard",
            "version": "latest",
            "vmName": "DB",
            "osdisk": "osdiskdb",
            "datadisk": "datadiskdb",
            "nicName": "NICDB",
            "ipAddress": "192.168.2.",
            "extensionDeployment": "",
            "avsetName": "ASDB",
            "remotePort": 3389,
            "dbPort": 1433
          },

6. U ziet dat de **vmSize** bevat de waarde *Standard_DS3*. Alleen bepaalde grootte VM toestaan voor het gebruik van meerdere NIC's. U kunt controleren welke groottes VM zijn meerdere NIC die via het [overzicht van meerdere NIC](virtual-networks-multiple-nics.md)is ingeschakeld.
7. Schuif omlaag naar de **resources** en kijk naar het eerste element. Hierin wordt een opslag-account. Dit account opslag wordt worden gebruikt voor het behoud van de gegevensschijven die worden gebruikt door elke VM-database. In dit scenario heeft elke database VM een OS schijf opgeslagen in de opslagruimte van de normale en twee gegevensschijven opgeslagen in de opslagruimte van SSD (premium).

        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('prmStorageName')]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "Storage Account - Premium"
          },
          "properties": {
            "accountType": "[parameters('prmStorageType')]"
          }
        },

8. Schuif omlaag naar de volgende resource, zoals hieronder wordt weergegeven. Deze resource staat de NIC die wordt gebruikt voor toegang tot de database in elke VM-database. Let op het gebruik van de functie **kopiëren** in deze resource. De sjabloon kunt u net zo veel VMs als u wilt gebruiken, op basis van de parameter **dbCount** implementeren. U moet dus evenveel NIC voor toegang tot de database, één voor elke VM maken.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB DA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

9. Schuif omlaag naar de volgende resource, zoals hieronder wordt weergegeven. Deze resource staat de NIC gebruikt voor beheer in elke VM-database. Nogmaals, moet u een van deze NIC voor elke database VM. Zoals u ziet het element **networkSecurityGroup** , een NSG voor toegang tot RDP/SSH naar deze NIC alleen koppelen.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB RA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
                  },
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

10. Schuif omlaag naar de volgende resource, zoals hieronder wordt weergegeven. Deze resource vertegenwoordigt een beschikbaarheid moet worden gedeeld door alle database VMs instellen. Zodat u zeker dat er altijd worden één VM in de set uitvoeren tijdens onderhoud.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/availabilitySets",
          "name": "[variables('dbVMSetting').avsetName]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "AvailabilitySet - DB"
          }
        },

11. Schuif omlaag naar de volgende resource. Deze resource staat voor de database VMs, zoals gezien in de eerste paar regels hieronder ziet u. Let op het gebruik van de functie **kopiëren** nogmaals ervoor te zorgen dat meerdere VMs zijn gemaakt op basis van de parameter **dbCount** . Verder zijn er de verzameling **dependsOn** . Hiermee geeft u twee netwerkadapters wordt moet worden gemaakt voordat de VM wordt geïmplementeerd, samen met de set beschikbaarheid en de opslag-account.

          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
          "location": "[variables('location')]",
          "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
            "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
            "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
          ],
          "tags": {
            "displayName": "VMs - DB"
          },
          "copy": {
            "name": "dbvmcount",
            "count": "[parameters('dbCount')]"
          },

12. Schuif omlaag in de bron VM op de **networkProfile** -element, zoals hieronder wordt weergegeven. U ziet dat er twee NIC verwijzing voor elke VM wordt. Wanneer u meerdere NIC's voor een VM maakt, moet u de **primaire** eigenschap van een van de NIC's instellen op *true*en de rest in *Onwaar*.

        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
              "properties": { "primary": true }
            },
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
              "properties": { "primary": false }
            }
          ]
        }
      }

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>De sjabloon ARM implementeren met behulp van Klik om te implementeren

> [AZURE.IMPORTANT] Zorg ervoor dat u de stappen [minimumvereisten](#Pre-requisites) voordat u de onderstaande instructies.

De voorbeeldsjabloon die beschikbaar zijn in de openbare opslagplaats gebruikt een parameterbestand met de standaard waarden gebruikt voor het genereren van het scenario hierboven is beschreven. Als u wilt deze sjabloon met klik te implementeren, volgt u [deze koppeling](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC)implementeert, aan de rechterkant van de **Backend resource groeperen (Zie de documentatie)** op **implementeren naar Azure**, vervangt u de waarden van de parameter standaard zo nodig en volg de instructies in de portal.

De onderstaande afbeelding ziet de inhoud van de resourcegroep nieuw na implementatie.

![Back-end-resourcegroep](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-the-template-by-using-powershell"></a>De sjabloon implementeren via PowerShell

Als u wilt implementeren van de sjabloon die u hebt gedownload via PowerShell, de onderstaande stappen uit te voeren.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

3. Voer de **`New-AzureRmResourceGroup`** cmdlet voor het maken van een resourcegroep met de sjabloon.

        New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'

    Verwachte output:

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                 Type                                 Location
                            ===================  ===================================  ========
                            ASDB                 Microsoft.Compute/availabilitySets   westus  
                            DB1                  Microsoft.Compute/virtualMachines    westus  
                            DB2                  Microsoft.Compute/virtualMachines    westus  
                            NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                            wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-the-template-by-using-the-azure-cli"></a>De sjabloon met behulp van de Azure CLI implementeren

Als u wilt implementeren de sjabloon met behulp van de Azure CLI, de onderstaande stappen uit te voeren.

1. Als u nooit Azure CLI gebruikt nog, Zie [installeren en configureren van de Azure CLI](../xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en een abonnement selecteren.
2. Voer de **`azure config mode`** opdracht om te schakelen naar resourcemanager-modus, zoals hieronder wordt weergegeven.

        azure config mode arm

    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

        info:    New mode is arm

3. Open het [parameterbestand](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), selecteert u de inhoud ervan en sla deze op een bestand in uw computer. In dit voorbeeld wordt de parameterbestand hebt opgeslagen naar *parameters.json*.

4. Voer de **`azure group deployment create`** implementeren van de nieuwe VNet met behulp van de sjabloon en de parameter-cmdlet bestanden u hebt gedownload en gewijzigd hierboven. De lijst die wordt weergegeven na de uitvoer wordt uitgelegd van de parameters die worden gebruikt.

        azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json

    Verwachte output:

        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
