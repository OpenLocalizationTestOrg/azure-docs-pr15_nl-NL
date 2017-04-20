<properties
    pageTitle="Hiermee stelt u automatisch schalen Windows VM schaal | Microsoft Azure"
    description="Autoscaling instellen voor een Windows VM schaal instellen via Azure PowerShell"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Automatisch de schaal van machines in een virtuele machine schaal instellen

VM schaal sets kunnen u eenvoudig kunt implementeren en identieke virtuele machines beheren als een set. Schaal sets bieden een laag zeer scalable en aanpasbare berekeningscluster voor ' hyperscale '-toepassingen en ze ondersteuning voor Windows-platform afbeeldingen, Linux platform afbeeldingen, aangepaste afbeeldingen en uitbreidingen. Zie [VM schaal ingesteld](virtual-machine-scale-sets-overview.md)voor meer informatie over schaal sets.

Deze zelfstudie ziet u hoe u een reeks schaal van Windows virtuele machines maken en automatisch de schaal van de machines in de set. U maakt de schaal instellen en schaling met een resourcemanager Azure-sjabloon maken en implementeren van deze via Azure PowerShell instellen. Zie voor meer informatie over sjablonen, [Azure resourcemanager Authoring sjablonen](../resource-group-authoring-templates.md). Meer informatie over automatische schaalbaarheid van schaal gebruikt, raadpleegt u [automatisch schalen en VM schaal ingesteld](virtual-machine-scale-sets-autoscale-overview.md).

In dit artikel u de volgende bronnen en implementeren extensies:

- Microsoft.Storage/storageAccounts
- Microsoft.Network/virtualNetworks
- Microsoft.Network/publicIPAddresses
- Microsoft.Network/loadBalancers
- Microsoft.Network/networkInterfaces
- Microsoft.Compute/virtualMachines
- Microsoft.Compute/virtualMachineScaleSets
- Microsoft.Insights.VMDiagnosticsSettings
- Microsoft.Insights/autoscaleSettings

Zie voor meer informatie over resourcemanager resources, [Azure berekenen, netwerk, en opslag Providers onder de resourcemanager van Azure](../virtual-machines/virtual-machines-windows-compare-deployment-models.md).

## <a name="step-1-install-azure-powershell"></a>Stap 1: Azure PowerShell installeren

Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor informatie over de nieuwste versie van Azure PowerShell installeert, uw abonnement te selecteren en aanmelden bij Azure.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Stap 2: Een resourcegroep en een opslag-account maken

1. **Een resourcegroep maken** – alle resources moeten worden geïmplementeerd op een resourcegroep. [Nieuw-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) gebruik maken van een resourcegroep met de naam **vmsstestrg1**.

2. **Een account opslag maken** – met dit account opslag is waarin de sjabloon is opgeslagen. [Nieuw-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) gebruiken om een benoemde **vmsstestsa**opslag-account te maken.

## <a name="step-3-create-the-template"></a>Stap 3: De sjabloon maken
Een sjabloon Azure resourcemanager kunt mogelijk te implementeren en beheren van Azure resources samen met behulp van een JSON-beschrijving van de resources en de bijbehorende implementatie parameters.

1. In uw favoriete editor het bestand C:\VMSSTemplate.json maken en voeg de eerste JSON-structuur ter ondersteuning van de sjabloon.

        {
          "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
          ]
        }

2. Parameters zijn niet altijd vereist, maar ze zelf een formule invoerwaarden wanneer de sjabloon wordt geïmplementeerd. Voeg deze parameters onder het bovenliggende element van parameters die u hebt toegevoegd aan de sjabloon.

        "vmName": { "type": "string" },
        "vmSSName": { "type": "string" },
        "instanceCount": { "type": "string" },
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" },
        "resourcePrefix": { "type": "string" }

    - Een naam voor de afzonderlijke virtuele machine die wordt gebruikt voor toegang tot de machines in de schaal instellen.
    - De naam van de opslag-account waarin de sjabloon is opgeslagen.
    - Het aantal virtuele machines eerst te maken in de schaal instellen.
    - De naam en het wachtwoord van het beheerdersaccount op de virtuele machines.
    - Een voorvoegsel naam voor de resources die zijn gemaakt voor de schaal instellen.

3. Variabelen kunnen worden gebruikt in een sjabloon kunt u waarden die regelmatig veranderen of waarden die moeten worden gemaakt op basis van een combinatie van parameterwaarden opgeven. Voeg deze variabelen onder het bovenliggende element van variabelen die u hebt toegevoegd aan de sjabloon.

        "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
        "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
        "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
        "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
        "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
        "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
        "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
        "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
        "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
          "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
        "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

  - DNS-namen die worden gebruikt door de netwerkinterfaces.
    - De IP-Adresnamen en voorvoegsels voor de virtuele netwerk en subnetten.
    - De namen en id van het virtuele netwerk, laad de verdeling van en netwerkinterfaces.
    - Opslag accountnamen voor de accounts die zijn gekoppeld aan de machines in de schaal instellen.
    - Instellingen voor de extensie diagnostisch hulpprogramma dat op de virtuele machines is geïnstalleerd. Zie voor meer informatie over de extensie diagnostische hulpprogramma's, [maken een Windows virtuele machine controle en diagnostische gegevens met behulp van Azure resourcemanager sjabloon](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).

4. Voeg de resource voor het account van opslag onder het bovenliggende element van resources die u hebt toegevoegd aan de sjabloon. Deze sjabloon worden een lus vormen om de aanbevolen vijf opslag-accounts maken waar de besturingssysteem schijven en diagnostische gegevens worden opgeslagen. Deze reeks accounts kan maximaal 100 virtuele machines in een reeks schaal, dat wil het huidige maximum zeggen ondersteunen. Elk account opslag heet met een letter statusaanduidingen die is gedefinieerd in de variabelen gecombineerd met het voorvoegsel dat u in de parameters voor de sjabloon opgeeft.

        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. De resource virtueel netwerk toevoegen. Zie [Resource netwerkprovider](../virtual-network/resource-groups-networking.md)voor meer informatie.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[variables('virtualNetworkName')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "subnet1",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },

6. Het openbare IP-adres resources die worden gebruikt door de taakverdeling en netwerkinterface toevoegen.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP1')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName1')]"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP2')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName2')]"
            }
          }
        },

7. De belasting voor gelijkmatige verdeling resource die wordt gebruikt door de set schaal toevoegen. Zie [Azure resourcemanager ondersteuning voor taakverdeling](../load-balancer/load-balancer-arm.md)voor meer informatie.

        {
          "apiVersion": "2015-06-15",
          "name": "[variables('loadBalancerName')]",
          "type": "Microsoft.Network/loadBalancers",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
          ],
          "properties": {
            "frontendIPConfigurations": [
              {
                "name": "loadBalancerFrontEnd",
                "properties": {
                  "publicIPAddress": {
                    "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
                  }
                }
              }
            ],
            "backendAddressPools": [ { "name": "bepool1" } ],
            "inboundNatPools": [
              {
                "name": "natpool1",
                "properties": {
                  "frontendIPConfiguration": {
                    "id": "[variables('frontEndIPConfigID')]"
                  },
                  "protocol": "tcp",
                  "frontendPortRangeStart": 50000,
                  "frontendPortRangeEnd": 50500,
                  "backendPort": 3389
                }
              }
            ]
          }
        },

8. Het netwerk interface resource die wordt gebruikt door de afzonderlijke virtuele machine toevoegen. Omdat computers in een set schaal niet toegankelijk is via een openbare IP-adres, is een afzonderlijke virtuele machine gemaakt in hetzelfde virtuele netwerk voor externe toegang tot de machines.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[variables('nicName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
            "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": {
                    "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
                  },
                  "subnet": {
                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                  }
                }
              }
            ]
          }
        },

9. De afzonderlijke virtuele machine in hetzelfde netwerk als de set schaal toevoegen.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[parameters('vmName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "storageLoop",
            "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_A1" },
            "osProfile": {
              "computername": "[parameters('vmName')]",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "[concat(parameters('resourcePrefix'), 'os1')]",
                "vhd": {
                  "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
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
            }
          }
        },

10. De schaal virtuele machine instellen resources toevoegen en geef de extensie diagnostisch hulpprogramma dat is geïnstalleerd op alle virtuele machines in de set schaal. Veel van de instellingen van deze resource zijn vergelijkbaar met de bron van de virtuele machine. De belangrijkste verschillen zijn het element capaciteit waarmee het aantal virtuele machines in de schaal instellen en upgradePolicy waarmee wordt opgegeven hoe updates in virtuele machines worden aangebracht. De set schaal is niet gemaakt totdat alle toegevoegde opslag-accounts zijn gemaakt met het element dependsOn zoals is opgegeven.

            {
              "type": "Microsoft.Compute/virtualMachineScaleSets",
              "apiVersion": "2016-03-30",
              "name": "[parameters('vmSSName')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "storageLoop",
                "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
              ],
              "sku": {
                "name": "Standard_A1",
                "tier": "Standard",
                "capacity": "[parameters('instanceCount')]"
              },
              "properties": {
                "upgradePolicy": {
                  "mode": "Manual"
                },
                "virtualMachineProfile": {
                  "storageProfile": {
                    "osDisk": {
                      "vhdContainers": [
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
                      ],
                      "name": "vmssosdisk",
                      "caching": "ReadOnly",
                      "createOption": "FromImage"
                    },
                    "imageReference": {
                      "publisher": "MicrosoftWindowsServer",
                      "offer": "WindowsServer",
                      "sku": "2012-R2-Datacenter",
                      "version": "latest"
                    }
                  },
                  "osProfile": {
                    "computerNamePrefix": "[parameters('vmSSName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                  },
                  "networkProfile": {
                    "networkInterfaceConfigurations": [
                      {
                        "name": "networkconfig1",
                        "properties": {
                          "primary": "true",
                          "ipConfigurations": [
                            {
                              "name": "ip1",
                              "properties": {
                                "subnet": {
                                  "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                                },
                                "loadBalancerBackendAddressPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                                  }
                                ],
                                "loadBalancerInboundNatPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                                  }
                                ]
                              }
                            }
                          ]
                        }
                      }
                    ]
                  },
                  "extensionProfile": {
                    "extensions": [
                      {
                        "name": "Microsoft.Insights.VMDiagnosticsSettings",
                        "properties": {
                          "publisher": "Microsoft.Azure.Diagnostics",
                          "type": "IaaSDiagnostics",
                          "typeHandlerVersion": "1.5",
                          "autoUpgradeMinorVersion": true,
                          "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                            "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                          },
                          "protectedSettings": {
                            "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                          }
                        }
                      }
                    ]
                  }
                }
              }
            },

11. Voeg de resource autoscaleSettings waarmee wordt gedefinieerd hoe de schaal instellen op basis van de processorgebruik op de computers in de set schaal aanpast.

            {
              "type": "Microsoft.Insights/autoscaleSettings",
              "apiVersion": "2015-04-01",
              "name": "[concat(parameters('resourcePrefix'),'as1')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              ],
              "properties": {
                "enabled": true,
                "name": "[concat(parameters('resourcePrefix'),'as1')]",
                "profiles": [
                  {
                    "name": "Profile1",
                    "capacity": {
                      "minimum": "1",
                      "maximum": "10",
                      "default": "1"
                    },
                    "rules": [
                      {
                        "metricTrigger": {
                          "metricName": "\\Processor(_Total)\\% Processor Time",
                          "metricNamespace": "",
                          "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                          "timeGrain": "PT1M",
                          "statistic": "Average",
                          "timeWindow": "PT5M",
                          "timeAggregation": "Average",
                          "operator": "GreaterThan",
                          "threshold": 50.0
                        },
                        "scaleAction": {
                          "direction": "Increase",
                          "type": "ChangeCount",
                          "value": "1",
                          "cooldown": "PT5M"
                        }
                      }
                    ]
                  }
                ],
                "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              }
            }

    Voor deze zelfstudie zijn deze waarden belangrijk:

    - **metricName** - met deze waarde is hetzelfde als de prestatie-item, zoals gedefinieerd in de variabele wadperfcounter. De extensie diagnostische gegevens worden verzameld die variabele gebruikt, de **processor(_Totaal)\% processortijd** item.
    - **metricResourceUri** - met deze waarde is de resource-id van de set van de schaal VM.
    - **timeGrain** – deze waarde is de granulatie van de criteria die zijn verzameld. In deze sjabloon, is dat het geconfigureerd met één minuut.
    - **statistische** – deze waarde bepaalt hoe de aan de doelstellingen worden gecombineerd zodat de automatische schaal actie. De mogelijke waarden zijn: gemiddelde, Min en Max. In deze sjabloon, worden het gemiddelde totale CPU-gebruik van de virtuele machines verzameld.
    - **waarde voor timeWindow** – deze waarde is het bereik van tijd waarin exemplaargegevens worden verzameld. Dit moet liggen tussen 5 minuten en 12 uur.
    - **TimeAggregation van** – deze waarde bepaalt hoe de gegevens die worden verzameld na verloop van tijd moet worden gecombineerd. De standaardwaarde is gemiddelde. De mogelijke waarden zijn: gemiddelde, Minimum, Maximum, laatste, totaal, tellen.
    - **operator** – deze waarde is de operator die wordt gebruikt om de metrische gegevens en de drempelwaarde te vergelijken. De mogelijke waarden zijn: gelijk is aan, NotEquals, groter dan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    - **drempelwaarde voor** – deze waarde is de waarde die de actie schaal activeert. In deze sjabloon worden machines toegevoegd aan de schaal instellen wanneer het gemiddelde CPU-gebruik tussen computers in de set meer dan 50 is %.
    - **richting** – deze waarde bepaalt de actie die wordt uitgevoerd wanneer de drempelwaarde wordt bereikt. De mogelijke waarden zijn vergroten of verkleinen. In deze sjabloon, wordt het aantal virtuele machines in de set schaal of de drempelwaarde meer dan 50% in het gedefinieerde tijdvenster verhoogd.
    - **type** : deze waarde is het type actie die moet worden uitgevoerd en moet zijn ingesteld op ChangeCount.
    - **waarde** – deze waarde is het aantal virtuele machines die worden toegevoegd of verwijderd uit de set schaal. Deze waarde moet 1 of groter. De standaardwaarde is 1. In deze sjabloon Stel het aantal computers in de schaal toeneemt door 1 wanneer de drempelwaarde is voldaan.
    - **cooldown** – deze waarde is de hoeveelheid tijd sinds de laatste schaal actie wachten voordat de volgende actie plaatsvindt. Deze waarde moet liggen tussen een minuut en één week.

12. Sla het sjabloonbestand.    

## <a name="step-4-upload-the-template-to-storage"></a>Stap 4: De sjabloon uploaden naar opslag

De sjabloon kan worden geüpload zo lang maken als u weet dat de naam en de primaire sleutel van het opslag-mailaccount dat u in stap 1 hebt gemaakt.

1.  Stel een variabele waarmee de naam van het opslag-mailaccount dat u hebt gemaakt in stap 1 in het venster Microsoft Azure PowerShell.

            $storageAccountName = "vmstestsa"

2.  Stel een variabele die de primaire sleutel van de opslag-account bevat.

            $storageAccountKey = "<primary-account-key>"

    U kunt deze toets krijgen door te klikken op het pictogram van de sleutel bij het weergeven van de resource van de account opslag in de portal van Azure.

3.  Het object opslag account context die wordt gebruikt voor het valideren van bewerkingen met de opslag-account maken.

            $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

4.  Maak de container voor het opslaan van de sjabloon.

            $containerName = "templates"
            New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob

5.  De sjabloonbestand uploaden naar de nieuwe container.

            $blobName = "VMSSTemplate.json"
            $fileName = "C:\" + $BlobName
            Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx

## <a name="step-5-deploy-the-template"></a>Stap 5: De sjabloon implementeren

Nu dat u de sjabloon hebt gemaakt, kunt u beginnen met het implementeren van de resources. Gebruik deze opdracht om het proces te starten:

    New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"

Wanneer u drukt op invoeren, wordt u gevraagd waarden op te geven voor de variabelen die aan u toegewezen. Geef deze waarden:

    vmName: vmsstestvm1
      vmSSName: vmsstest1
      instanceCount: 5
      adminUserName: vmadmin1
      adminPassword: VMpass1
      resourcePrefix: vmsstest

Duurt ongeveer 15 minuten voor alle resources om te worden geïmplementeerd.

>[AZURE.NOTE] U kunt ook de mogelijkheid van de portal gebruiken om te implementeren van de resources. Gebruik deze koppeling: "https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>"

## <a name="step-6-monitor-resources"></a>Stap 6: Monitor resources

U kunt bepaalde informatie ophalen over VM schaal sets met behulp van deze methoden:

 - De portal Azure - kunt u momenteel een beperkte tijdsduur van gegevens met behulp van de portal verkrijgen.
 - De [Azure Resource Explorer](https://resources.azure.com/) - dit hulpprogramma is het beste werkt voor het verkennen van de huidige status van een set schaal. Volg dit pad en ziet u de weergave exemplaar van de schaal instellen die u hebt gemaakt:

        subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

 - Azure PowerShell - Gebruik deze opdracht om bepaalde informatie te verkrijgen:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

        Or

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView

 - Verbinding maken met de afzonderlijke virtuele machine net zoals u zou doen met een andere computer en klikt u op afstand toegang hebt tot de virtuele machines in de schaal instellen om de afzonderlijke processen te houden.

>[AZURE.NOTE] Een volledige REST API voor het verkrijgen van informatie over schaal sets vindt u in de [Virtuele Machine schaal Sets](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-the-resources"></a>Stap 7: De resource verwijderen

Omdat u worden berekend voor resources die worden gebruikt in Azure wordt aangegeven, maar het is altijd een goede gewoonte om het verwijderen van resources die niet meer nodig zijn. U hoeft niet te elke resource afzonderlijk verwijderen uit een resourcegroep. U kunt de resourcegroep verwijderen en alle bijbehorende resources worden automatisch verwijderd.

    Remove-AzureRmResourceGroup -Name vmsstestrg1

Als u uw resourcegroep behouden wilt, kunt u de schaal instellen alleen verwijderen.

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"

## <a name="next-steps"></a>Volgende stappen

- Het beheren van de schaal instellen dat u zojuist hebt gemaakt met behulp van de informatie in het [beheren van virtuele machines in een virtuele Machine schaal instellen](virtual-machine-scale-sets-windows-manage.md).
- Meer informatie over de verticale schaling met [verticale automatisch schalen met VM schaal sets](virtual-machine-scale-sets-vertical-scale-reprovision.md) reviseren
- Voorbeelden van Azure Monitor controlefuncties in [Azure Monitor PowerShell snel starten voorbeelden](../monitoring-and-diagnostics/insights-powershell-samples.md) zoeken
- Meer informatie over functies voor melding [automatisch schalen acties om te verzenden van e-mail en webhook waarschuwingen in Azure beeldscherm gebruiken](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
- Leer hoe u [Gebruik controle meldt zich bij het verzenden van e-mail en webhook waarschuwingen in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
