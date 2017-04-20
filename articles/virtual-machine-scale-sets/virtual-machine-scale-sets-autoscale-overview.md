<properties
    pageTitle="Automatische schalen en VM schaal sets | Microsoft Azure"
    description="Informatie over het gebruik van Diagnostisch hulpprogramma en automatisch schalen resources aan de virtuele machines automatisch nieuwe schaal in een set schaal."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="automatic-scaling-and-virtual-machine-scale-sets"></a>Automatische schalen en VM schaal sets

Automatische schaalbaarheid van virtuele machines in een set schaal, is het maken of verwijderen van computers in de set desgewenst om te voldoen aan vereisten voor prestaties. Als de hoeveelheid werk in omvang groeit, kan een toepassing vragen om aanvullende informatie te kunnen effectief taken uitvoeren.

Automatische schaling is een geautomatiseerde proces dat gemakkelijk management realiseren. Verlaagt belasting, hoeft u niet te continu systeemprestaties controleren of bepalen hoe bronnen beheren. Schaalbaarheid is een elastische proces. Meer resources kunnen worden toegevoegd als het toeneemt laden, maar als vraag verlagingen resources kunnen worden verwijderd als u wilt minimaliseren kosten en prestaties onderhouden.

Stel automatische schaling op schaal instellen met behulp van een sjabloon Azure resourcemanager, Azure PowerShell, Azure CLI of de Azure-portal.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Een schaal met behulp van resourcemanager sjablonen instellen

In plaats van implementeert en elke resource van uw toepassing afzonderlijk beheren, een sjabloon gebruikt die alle resources in een enkel, gecoördineerde bewerking implementeert. Toepassing resources zijn gedefinieerd in de sjabloon en implementatie parameters zijn opgegeven voor verschillende omgevingen. De sjabloon bestaat uit JSON en expressies die u gebruiken kunt om samen te stellen van de waarden voor de implementatie. Meer informatie, kijkt u naar [Azure resourcemanager Authoring sjablonen](../resource-group-authoring-templates.md).

In de sjabloon, moet u het element capaciteit opgeven:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 3
    },

Capaciteit geeft het aantal virtuele machines in de set. U kunt handmatig de capaciteit wijzigen door het implementeren van een sjabloon met een andere waarde. Als u een sjabloon om alleen de capaciteit wijzigen implementeert, kunt u alleen het SKU-element met de bijgewerkte capaciteit opnemen.

Automatisch de capaciteit van schaal instellen met behulp van de combinatie van de resource autoscaleSettings en de extensie diagnostische gegevens wijzigen.

### <a name="configure-the-azure-diagnostics-extension"></a>De extensie Azure diagnostische gegevens configureren

Automatische schaling kan alleen worden uitgevoerd als de doelstellingen siteverzameling voltooid op elke virtuele machine in de set schaal is. De uitbreiding van de diagnostisch hulpprogramma Azure biedt de cmdlets voor controle en hulpprogramma's voor diagnose mogelijkheden die voldoet aan de behoeften van de siteverzameling aan de doelstellingen van de resource automatisch schalen. U kunt de extensie installeren als onderdeel van de sjabloon resourcemanager.

In dit voorbeeld ziet u de variabelen die worden gebruikt in de sjabloon voor het configureren van de extensie diagnostische hulpprogramma's:

    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

Parameters zijn opgegeven als de sjabloon wordt geïmplementeerd. In dit voorbeeld worden de naam van de opslag-account waar gegevens worden opgeslagen en de naam van de schaal set waaruit u gegevens worden verzameld gegeven. In dit voorbeeld Windows Server alleen het aantal threads prestatie-item ook verzameld. Alle beschikbare prestaties tellers in Windows of Linux kunnen worden gebruikt voor het verzamelen van diagnostische informatie en kunnen worden opgenomen in de configuratie van de extensie.

In dit voorbeeld ziet u de definitie van de extensie in de sjabloon:

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
              "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
              "storageAccountEndPoint": "https://core.windows.net"
            }
          }
        }
      ]
    }

Wanneer de extensie diagnostische gegevens wordt uitgevoerd, worden de gegevens worden verzameld in een tabel die zich bevindt in de opslagruimte-account dat u opgeeft. U kunt de verzamelde gegevens vinden in de tabel WADPerformanceCounters:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-the-autoscalesettings-resource"></a>De resource autoScaleSettings configureren

De resource autoscaleSettings gebruikt de gegevens van de extensie diagnostische hulpprogramma's om te bepalen of u wilt vergroten of verkleinen van het aantal virtuele machines in de set schaal.

In dit voorbeeld ziet u de configuratie van de resource in de sjabloon:

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
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 650
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 550
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
      }
    }

In het bovenstaande voorbeeld worden twee regels worden gemaakt voor het definiëren van de automatische schaal acties. De eerste regel definieert de actie schalen en de tweede regel definieert de actie schaal in. Deze waarden zijn beschikbaar in de regels:

- **metricName** - met deze waarde is hetzelfde als de prestatie-item dat u hebt gedefinieerd in de variabele wadperfcounter voor de extensie diagnostische gegevens. In het bovenstaande voorbeeld wordt wordt de teller aantal threads gebruikt.  
- **metricResourceUri** - met deze waarde is de resource-id van de set van de schaal VM. Deze id bevat de naam van de resourcegroep, de naam van de resource-provider en de naam van de schaal instellen aan de nieuwe schaal.
- **timeGrain** – deze waarde is de granulatie van de criteria die zijn verzameld. Klik in het voorgaande voorbeeld, worden gegevens op een interval van één minuut. Deze waarde wordt gebruikt met de waarde voor timeWindow.
- **statistische** – deze waarde bepaalt hoe de aan de doelstellingen worden gecombineerd zodat de automatische schaal actie. De mogelijke waarden zijn: gemiddelde, Min en Max.
- **waarde voor timeWindow** – deze waarde is het bereik van tijd waarin exemplaargegevens worden verzameld. Dit moet liggen tussen 5 minuten en 12 uur.
- **TimeAggregation van** – deze waarde bepaalt hoe de gegevens die worden verzameld na verloop van tijd moet worden gecombineerd. De standaardwaarde is gemiddelde. De mogelijke waarden zijn: gemiddelde, Minimum, Maximum, laatste, totaal, tellen.
- **operator** – deze waarde is de operator die wordt gebruikt om de metrische gegevens en de drempelwaarde te vergelijken. De mogelijke waarden zijn: gelijk is aan, NotEquals, groter dan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
- **drempelwaarde voor** – deze waarde is de waarde die de actie schaal activeert. Zorg ervoor dat op te geven van een voldoende verschil tussen de drempelwaarde voor de actie schalen en de drempelwaarde voor de actie schaal in. Als u de waarden niet hetzelfde zijn ingesteld, wordt in het systeem constante wijzigen, waardoor deze bij de implementatie van een schaal actie verwacht. Bijvoorbeeld werkt op 600 threads instellen in het voorgaande voorbeeld niet.
- **richting** – deze waarde bepaalt de actie die wordt uitgevoerd wanneer de drempelwaarde wordt bereikt. De mogelijke waarden zijn vergroten of verkleinen.
- **type** : deze waarde is het type actie die moet worden uitgevoerd en moet zijn ingesteld op ChangeCount.
- **waarde** – deze waarde is het aantal virtuele machines die zijn toegevoegd aan of verwijderd uit de set schaal. Deze waarde moet 1 of groter.
- **cooldown** – deze waarde is de hoeveelheid tijd sinds de laatste schaal actie wachten voordat de volgende actie plaatsvindt. Deze waarde moet liggen tussen een minuut en één week.

Afhankelijk van de prestatie-item dat u gebruikt, worden aantal elementen in de sjabloonconfiguratie anders gebruikt. In het voorgaande voorbeeld wordt de prestatie-item is aantal threads, de drempelwaarde is 650 voor een actie schalen en de drempelwaarde is 550 voor de actie schaal in. Als u een item zoals % processortijd gebruikt, wordt de drempelwaarde is ingesteld op het percentage van CPU-gebruik die een schaal actie bepaalt.

Wanneer een laden is gemaakt in de virtuele machines waarmee een schaal actie wordt geactiveerd, verhoogd de capaciteit van de set op basis van de waarde in de sjabloon. Set waarvan de capaciteit is ingesteld op 3 en de waarde van de actie schaal is bijvoorbeeld in een schaal ingesteld op 1:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Wanneer wordt het selectievakje laden waardoor het gemiddelde aantal threads aan hoger dan de drempelwaarde voor 650 gemaakt:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

Een actie schalen wordt geactiveerd waardoor de capaciteit van de set kan worden verhoogd door een:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 4
    },

En een virtuele machine wordt toegevoegd aan de schaal instellen:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Na een periode cooldown van vijf minuten, dat als het gemiddelde aantal threads op de computers meer dan 600, blijft een andere computer wordt toegevoegd aan de set. Als het gemiddelde aantal threads onder 550 blijft, de capaciteit van de set schaal met een wordt verminderd en een machine is verwijderd uit de set.

## <a name="set-up-scaling-using-azure-powershell"></a>Schaalbaarheid via Azure PowerShell instellen

Als u wilt zien voorbeelden van het gebruik van PowerShell voor het instellen van autoscaling, kijkt u naar [Azure Monitor PowerShell snel starten voorbeelden](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Schaling met Azure CLI instellen

Als u wilt zien voorbeelden van het gebruik van Azure CLI voor het instellen van autoscaling, kijkt u naar [Azure Monitor platforms CLI snel starten voorbeelden](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-the-azure-portal"></a>Schaling met behulp van de Azure portal instellen

Als u wilt zien van een voorbeeld van het gebruik van de Azure-portal voor het instellen van autoscaling, kijkt u naar [een virtuele Machine schaal instellen maken met behulp van de Azure portal](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Schaal acties onderzoeken

- [Azure-portal]() - krijgt u momenteel een beperkte tijdsduur van gegevens met behulp van de portal.
- [Azure Resource Explorer]() - dit hulpmiddel is het beste werkt voor het verkennen van de huidige status van een set schaal. Volg dit pad en ziet u de weergave exemplaar van de schaal instellen die u hebt gemaakt: abonnementen > {uw abonnement} > resourceGroups > {uw resourcegroep} > providers > Microsoft.Compute > virtualMachineScaleSets > {uw schaal instellen} > virtualMachines
- Azure PowerShell - Gebruik deze opdracht om bepaalde informatie te verkrijgen:

        Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
        Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput

- Verbinding maken met de jumpbox virtuele machine net zoals u zou doen met een andere computer en klikt u op afstand toegang hebt tot de virtuele machines in de schaal instellen om de afzonderlijke processen te houden.

## <a name="next-steps"></a>Volgende stappen

- Bekijk [automatisch schalen computers in een virtuele Machine schaal instellen](virtual-machine-scale-sets-windows-autoscale.md) om een voorbeeld van het maken van een set met automatische schaling geconfigureerd schaal weer te geven.
- Voorbeelden van Azure Monitor controlefuncties in [Azure Monitor PowerShell snel starten voorbeelden](../monitoring-and-diagnostics/insights-powershell-samples.md) zoeken
- Meer informatie over functies voor melding [automatisch schalen acties om te verzenden van e-mail en webhook waarschuwingen in Azure beeldscherm gebruiken](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).
- Meer informatie over het [Gebruik controle meldt zich bij het verzenden van e-mail en webhook waarschuwingen in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
- Lees meer over [Geavanceerde automatisch schalen scenario's](./virtual-machine-scale-sets-advanced-autoscale.md).
