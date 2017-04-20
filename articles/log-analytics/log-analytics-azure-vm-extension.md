<properties
    pageTitle="Azure virtuele machines verbinden met Log Analytics | Microsoft Azure"
    description="Voor Windows en Linux virtuele machines uitgevoerd in Azure wordt aangegeven, is het raadzaam verzamelde logboeken en statistieken door het installeren van de extensie Log Analytics Azure VM. U kunt de portal van Azure of PowerShell gebruiken voor het installeren van de extensie Log Analytics VM naar Azure VMs."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="richrund"/>

# <a name="connect-azure-virtual-machines-to-log-analytics"></a>Azure virtuele machines verbinden met Log Analytics

Voor Windows en Linux computers is de aanbevolen methode voor het verzamelen van Logboeken en statistieken door het installeren van de Log Analytics-agent.

De eenvoudigste manier om de Log Analytics-agent installeren op Azure virtuele machines is tot en met de extensie Log Analytics VM.  Met de extensie installatie eenvoudiger en automatisch geconfigureerd in de agent om gegevens te verzenden naar de Log Analytics-werkruimte die u opgeeft. De-agent is ook automatisch bijgewerkt, hebt u de nieuwste functies en oplossingen.

Voor Windows virtuele machines, moet u de *Microsoft Monitoring Agent* VM extensie inschakelen.
Voor Linux virtuele machines, moet u de *OMS Agent voor Linux* VM extensie inschakelen.

Meer informatie over [Azure virtuele machines extensies](../virtual-machines/virtual-machines-windows-extensions-features.md) en de [Linux-agent] (.. / virtual-machines/virtual-machines-linux-agent-user-guide.md).

Wanneer u op basis van een agent siteverzameling voor logboekgegevens gebruikt, moet u [gegevensbronnen in Log Analytics](log-analytics-data-sources.md) om op te geven van de logboeken en aan de doelstellingen die u wilt verzamelen configureren.

>[AZURE.IMPORTANT] Als u Log analyses met index logboekgegevens configureren via [Azure diagnostisch hulpprogramma](log-analytics-azure-storage.md)en kunt u de agent als u wilt de dezelfde logboeken verzamelen configureren, klikt u vervolgens de logboeken tweemaal verzameld. U wordt geheven voor beide gegevensbronnen. Als u de agent hebt geïnstalleerd, en vervolgens u logboekgegevens verzamelen dient met behulp van de alleen-agent - logboek Analytics om het logboekgegevens verzamelen van Azure diagnostische gegevens niet configureren.

Er zijn drie eenvoudige manieren om in te schakelen van de extensie Log Analytics-VM:

+ Met behulp van de Azure-portal
+ Met behulp van Azure PowerShell
+ Met behulp van een sjabloon resourcemanager van Azure

## <a name="enable-the-vm-extension-in-the-azure-portal"></a>Inschakelen van de extensie VM in de portal van Azure

U kunt de agent van Log analysegegevens installeren en verbinden van de Azure virtuele machine die wordt uitgevoerd op met behulp van de [Azure-portal](https://portal.azure.com).

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a>Voor het installeren van de Log Analytics-agent en de virtuele machine verbinden met een logboek Analytics-werkruimte

1.  Meld u aan bij de [portal van Azure](http://portal.azure.com).
2.  Selecteer **Bladeren** aan de linkerkant van de portal en gaat u naar **Log Analytics (OMS)** en selecteer deze.
3.  Selecteer de fase die u wilt gebruiken met de VM Azure in de lijst met van Log Analytics-werkruimten.  
    ![OMS werkruimten](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4.  Selecteer onder **Log analytics-beheer** **virtuele machines**.  
    ![Virtuele machines](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5.  Selecteer in de lijst met **virtuele machines**, de virtuele machine waarop u wilt installeren van de agent. De **Verbindingsstatus OMS** voor VM wordt aangegeven dat dit **niet verbonden is**.  
    ![VM niet verbonden](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6.  De details in voor uw VM, selecteert u **verbinding maken**. De agent wordt automatisch geïnstalleerd en geconfigureerd voor uw Log Analytics-werkruimte. Dit proces duurt een paar minuten, tijdens welke periode de verbindingsstatus van OMS *verbinding is...*  
    ![Verbinding maken met VM](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7.  Nadat u installeren en verbinden met de agent, wordt de verbindingsstatus van **OMS** bijgewerkt zodat **deze werkruimte**.  
    ![Verbonden](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)


## <a name="enable-the-vm-extension-using-powershell"></a>De VM extensie via PowerShell inschakelen

Er zijn verschillende opdrachten voor Azure klassieke virtuele machines en resourcemanager virtuele machines. Hieronder vindt u voorbeelden voor zowel klassieke en resourcemanager virtuele machines.

Gebruik het volgende PowerShell-voorbeeld voor klassieke virtuele machines:

```
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

Gebruik het volgende PowerShell-voorbeeld voor resourcemanager virtuele machines:

```
Login-AzureRMAccount
Select-AzureSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```
Als u uw VM configureert via PowerShell, moet u de **Werkruimte-ID** en de **Primaire sleutel**. U vindt de-Id en van toetsen op de pagina **Instellingen** van de portal OMS of via PowerShell zoals wordt weergegeven in het voorgaande voorbeeld.

![Werkruimte-ID en de primaire sleutel](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

## <a name="deploy-the-vm-extension-using-a-template"></a>Implementeer de VM extensie met een sjabloon

U kunt een eenvoudige sjabloon (in de indeling van JSON) die welke implementatie en configuratie van uw toepassing bepaalt maken met behulp van Azure resourcemanager. Deze sjabloon is bekend als een sjabloon resourcemanager en biedt een declaratieve manier definiëren implementatie. Met behulp van een sjabloon, kunt u net zo vaak implementeren van uw toepassing gedurende de levenscyclus van de app en vertrouwen dat uw resources zijn die wordt geïmplementeerd in een consistente status hebben.

Door de Log Analytics-agent als onderdeel van uw sjabloon resourcemanager op te nemen, kunt u ervoor zorgen dat elke virtuele machine vooraf is geconfigureerd om te rapporteren aan uw werkruimte Log Analytics.

Zie voor meer informatie over resourcemanager sjablonen [Azure resourcemanager Authoring sjablonen](../resource-group-authoring-templates.md).

Hier volgt een voorbeeld van een sjabloon resourcemanager die wordt gebruikt voor het implementeren van een virtuele machine waarop Windows wordt uitgevoerd met de extensie Microsoft Monitoring Agent is geïnstalleerd. Deze sjabloon voor een normale VM sjabloon is, met de volgende toevoegingen:

+ workspaceId en Werkruimtenaam parameters
+ Sectie Microsoft.EnterpriseCloud.Monitoring resource-extensie
+ Uitvoer van de workspaceId en workspaceSharedKey opzoeken


```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMD workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

U kunt een sjabloon implementeren met behulp van de volgende PowerShell-opdracht:

```
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-windows-virtual-machines"></a>Problemen met Windows virtuele Machines

Als de extensie *Microsoft Monitoring Agent* VM-agent is niet installeren of rapportage kunt u de volgende stappen uit om op te lossen van het probleem kunt uitvoeren.

1. Controleer of de Azure VM-agent is geïnstalleerd en werkt goed met behulp van de stappen in [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
  + U kunt ook het logboekbestand van VM agent bekijken`C:\WindowsAzure\logs\WaAppAgent.log`
  + Als het logboek niet bestaat, wordt de VM-agent is niet geïnstalleerd.
    - [Installeren van de Azure VM-Agent op klassieke VMs](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md)
2. Bevestig dat de Microsoft Monitoring Agent extensie heartbeat taak wordt uitgevoerd met de volgende stappen:
  + Meld u aan bij de virtuele machine
  + Open Taakplanner en Ga naar de `update_azureoperationalinsight_agent_heartbeat` taak
  + Controleer of de taak is ingeschakeld en elke één minuut wordt uitgevoerd
  + Het logboekbestand heartbeat inchecken`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Controleer de logboekbestanden van Microsoft Monitoring Agent VM extensie in`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
3. Controleer of dat de virtuele machine kan PowerShell-scripts worden uitgevoerd
4. Controleer of machtigingen voor C:\Windows\temp niet zijn gewijzigd
5. De status van de Microsoft-Agent Monitoring bekijken door het volgende te typen in een verhoogde PowerShell-venster op de virtuele machine`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
6. Controleer de logboekbestanden van Microsoft Monitoring Agent setup in`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Voor meer informatie raadpleegt u [problemen oplossen uitbreidingen voor Windows](../virtual-machines/virtual-machines-windows-extensions-troubleshoot.md).

## <a name="troubleshooting-linux-virtual-machines"></a>Probleemoplossing Linux virtuele Machines

Als de extensie *OMS Agent voor Linux* VM-agent is niet installeren of rapportage kunt u de volgende stappen uit om op te lossen van het probleem kunt uitvoeren.

1. Als de status van de extensie is *Onbekend* selectievakjes als de Azure VM-agent is geïnstalleerd en werkt goed voor reviseren van het logboekbestand van VM agent`/var/log/waagent.log`
  + Als het logboek niet bestaat, wordt de VM-agent is niet geïnstalleerd.
  - [Installeren van de Azure VM-Agent op Linux VMs](../virtual-machines/virtual-machines-linux-agent-user-guide.md)
2. Voor andere beschadigd statussen, Controleer de OMS-Agent voor Linux VM extensie logboeken op de bestanden in `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` en`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Als de status van de extensie in orde is, maar de gegevens niet wordt geüpload raadpleegt u de OMS-Agent voor de logboekbestanden Linux in`/var/opt/microsoft/omsagent/log/omsagent.log`

Raadpleeg [Probleemoplossing Linux uitbreidingen](../virtual-machines/virtual-machines-linux-extensions-troubleshoot.md)voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

+ [Gegevensbronnen in Log Analytics](log-analytics-data-sources.md) om op te geven van de logboeken en aan de doelstellingen voor het verzamelen van configureren.
+ Om gegevens te verzamelen van virtuele machines [toevoegen Log Analytics-oplossingen uit de galerie met oplossingen](log-analytics-add-solutions.md).
+ [Gegevens verzamelen via Azure diagnostische hulpprogramma's](log-analytics-azure-storage.md) voor andere resources die worden uitgevoerd in Azure wordt aangegeven.

Voor computers die zich niet in Azure wordt aangegeven, kunt u de Log Analytics-agent installeren met behulp van de methoden die worden beschreven in de volgende artikelen:

+ [Windows-computers verbinden met Log Analytics](log-analytics-windows-agents.md)
+ [Linux computers verbinden met Log Analytics](log-analytics-linux-agents.md)
