<properties
    pageTitle="Een Windows virtuele machine maken met de cmdlets voor controle en diagnostische gegevens met behulp van Azure resourcemanager sjabloon | Microsoft Azure"
    description="Een sjabloon van de manager Azure resource gebruiken om te maken van een nieuwe virtuele Windows-computer met Azure diagnostisch hulpprogramma extensie."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sbtron"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>

# <a name="create-a-windows-virtual-machine-with-monitoring-and-diagnostics-using-azure-resource-manager-template"></a>Een Windows virtuele machine controle en diagnostische gegevens met behulp van Azure resourcemanager sjabloon maken

De extensie van de diagnostisch hulpprogramma Azure biedt de cmdlets voor controle en mogelijkheden voor diagnostische gegevens op een Windows Azure virtuele machines gebaseerd. U kunt deze mogelijkheden op de virtuele machine inschakelen door de extensie als onderdeel van de azure resource manager sjabloon op te nemen. Zie [Authoring Azure resourcemanager sjablonen met VM uitbreidingen](virtual-machines-windows-extensions-authoring-templates.md) voor meer informatie over het toevoegen van willekeurige toestellen als onderdeel van een sjabloon VM. In dit artikel wordt beschreven hoe u de extensie Azure diagnostische gegevens kunt toevoegen aan een virtuele machine-sjabloon van windows.  
  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>De uitbreiding Azure diagnostische hulpprogramma's voor de definitie van de resource VM toevoegen 

Om te schakelen van de extensie diagnostische gegevens op een virtuele Windows-computer moet u de extensie toevoegen als een resource VM in de sjabloon voor het beheren van Resource.

Voor een eenvoudige resourcemanager gebaseerd VM toevoegen de configuratie van de extensie aan de matrix *resources* voor de virtuele Machine: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


Een andere algemene congres is de configuratie van de extensie toevoegen bij het knooppunt van de resources hoofdsite van de sjabloon in plaats van deze onder de VM resources knooppunt definiëren. Met deze methode die u moet expliciet een hiërarchische relatie tussen de extensie en de virtuele machine op te geven met de *naam* en *type* waarden. Bijvoorbeeld: 
  
    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

De extensie wordt altijd gekoppeld aan de virtuele machine, u kunt rechtstreeks direct onder de VM resource knooppunt te definiëren of definiëren op het niveau van base en de hiërarchische naamgevingsconventie gebruiken om te koppelen aan de virtuele machine.

De configuratie van de extensies is voor VM schaal Sets opgegeven in de eigenschap *extensionProfile* van de *VirtualMachineProfile*.
   
De eigenschap *publisher* met de waarde van **Microsoft.Azure.Diagnostics** en de eigenschap *type* met de waarde van **IaaSDiagnostics** is unieke identificatie van de extensie diagnostisch hulpprogramma Azure.

De waarde van *de eigenschap* kan worden gebruikt om te verwijzen naar de extensie in de resourcegroep. Instelling voor het specifiek aan **Microsoft.Insights.VMDiagnosticsSettings** , wordt deze eenvoudig worden geïdentificeerd door de Azure klassieke portal portal ervoor te zorgen dat de controle grafieken correct in de portal van Azure klassieke weergegeven uitgeschakeld.

De *typeHandlerVersion* geeft de versie van de extensie die u wilt gebruiken. *AutoUpgradeMinorVersion* instellen secundaire versie op **true** zorgt ervoor dat krijgt u de meest recente secundaire versie van de extensie die beschikbaar is. Het wordt ten zeerste aanbevolen dat u altijd *autoUpgradeMinorVersion* moeten altijd **true** zodat u altijd toegang hebt tot de meest recente beschikbaar diagnostisch hulpprogramma extensie gebruiken met de nieuwe functies en correcties instellen. 

Het element *Instellingen* bevat configuraties eigenschappen voor de extensie die kunnen worden ingesteld en lezen weer van de extensie (ook wel openbare configuratie genoemd). De eigenschap *xmlcfg* bevat xml gebaseerd configuratie voor de logboeken diagnostisch hulpprogramma prestatiemeteritems enzovoort die worden verzameld door de agent diagnostische gegevens. Zie [Diagnostisch hulpprogramma configuratieschema](https://msdn.microsoft.com/library/azure/dn782207.aspx) voor meer informatie over het XML-schema zelf. Een gebruikelijk is voor het opslaan van de werkelijke XML-configuratie als een variabele in de sjabloon Azure resourcemanager en daarna samengevoegd en base64 coderen kunt de waarde voor *xmlcfg*instellen. Zie het gedeelte van [de variabelen diagnostische hulpprogramma's](#diagnostics-configuration-variables) voor meer informatie over meer informatie over het opslaan van de xml in variabelen. De eigenschap *storageAccount* geeft de naam van de opslag-account waaraan naar diagnostische gegevens worden overgebracht. 
 
De eigenschappen in *protectedSettings* (ook wel privé configuratie genoemd) kunnen worden ingesteld, maar kunnen niet worden gelezen terug na wordt ingesteld. De alleen-schrijven aard van *protectedSettings* is het handig voor het opslaan van geheimen zoals de accountsleutel opslag waar de gegevens van de diagnostische gegevens worden geschreven.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Diagnostisch hulpprogramma opslag account opgeven als parameters 

Het hulpprogramma's voor diagnose extensie json codefragment van de bovenstaande wordt ervan uitgegaan twee parameters *existingdiagnosticsStorageAccountName* en *existingdiagnosticsStorageResourceGroup* om op te geven van het hulpprogramma's voor diagnose opslag account waar naar diagnostische gegevens worden opgeslagen. Het account van de opslagruimte diagnostische gegevens opgeven als een parameter het maakt gemakkelijk te wijzigen van het account van de opslagruimte diagnostische gegevens in verschillende omgevingen bijvoorbeeld u mogelijk wilt gebruiken een andere hulpprogramma's voor diagnose opslag-account voor het testen en een andere naam voor uw productie-implementatie.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "The name of an existing storage account to which diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
            }
        }

Dit is een goede gewoonte om op te geven van een account van de opslagruimte diagnostische gegevens in een andere resourcegroep dan de resourcegroep voor de virtuele machine. Een resourcegroep kan worden beschouwd als een eenheid implementatie met een eigen levensduur, een virtuele machine kan worden geïmplementeerd en geïmplementeerd als nieuwe configuraties updates deze in deze worden aangebracht, maar u kunt doorgaan met het hulpprogramma's voor diagnose gegevens op te slaan in hetzelfde account opslag in deze virtuele machine-implementaties. Zonder dat de opslag-account in een andere resource hebt, kunt het account opslag accepteren gegevens uit verschillende VM-implementaties, zodat u gemakkelijk kunt oplossen van problemen met over de verschillende versies.

>[AZURE.NOTE] Als u een windows-VM-sjabloon vanuit Visual Studio maken mogelijk het standaardaccount voor de opslag zijn ingesteld op dezelfde opslag account gebruiken waar de virtuele machine VHD wordt geüpload. Dit is de eerste configuratie van de VM vereenvoudigen. U moet een verschillende opslag-account dat kan worden doorgegeven als u een parameter gebruiken zodat de sjabloon opnieuw factor. 

## <a name="diagnostics-configuration-variables"></a>Diagnostisch hulpprogramma configuratievariabelen
 
Het hulpprogramma's voor diagnose extensie json codefragment van de bovenstaande definieert een variabele *accountid* om te vereenvoudigen aan de accountsleutel opslag voor de opslag diagnostische hulpprogramma's:   
    
    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


De eigenschap *xmlcfg* voor de extensie diagnostische gegevens is gedefinieerd voor het gebruik van meerdere variabelen die worden samengevoegd. De waarden van deze variabelen zijn in XML-bestand, zodat ze correct worden voorafgegaan moeten bij het instellen van de json-variabelen.

Volgt een beschrijving van de XML-code configuratie van diagnostische gegevens die worden verzameld standaard systeem niveau prestatiemeteritems samen met enkele gebeurtenislogboeken van windows en hulpprogramma's voor diagnose infrastructuur Logboeken. Dit is voorafgegaan en correct opgemaakt zodat de configuratie rechtstreeks kan worden geplakt in de sectie variabelen van uw sjabloon. Zie de [Hulpprogramma's voor diagnose configuratieschema](https://msdn.microsoft.com/library/azure/dn782207.aspx) voor een meer menselijke leesbare voorbeeld van de configuratie-xml.
    
        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

Het knooppunt aan de doelstellingen definitie xml in de bovenstaande configuratie is een belangrijke configuratie-element, zoals deze wordt gedefinieerd hoe de items die eerder in de xml in *PerformanceCounter* knooppunt gedefinieerd wordt samengevoegd en opgeslagen. 

> [AZURE.IMPORTANT] Deze gegevens station de controle grafieken en waarschuwingen in de portal van Azure.  Het knooppunt **aan de doelstellingen** met de *resourceID* en **MetricAggregation** moet worden opgenomen in de configuratie diagnostische hulpprogramma's voor uw VM als u wilt zien van de VM controlegegevens in de portal van Azure. 

Hier volgt een voorbeeld van de xml voor definities voor statistieken: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

Het kenmerk *resourceID* identificatie unieke van de virtuele machine in uw abonnement. Controleer of gebruik de functies subscription() en resourceGroup() zodat de sjabloon wordt automatisch deze waarden op basis van het abonnement en u implementeert bijgewerkt op resourcegroep.

Als u meerdere virtuele Machines in een lus maakt hebt u de waarde *resourceID* met een functie copyIndex() om correct onderscheid elke afzonderlijke VM te vullen. De waarde *xmlCfg* kan worden bijgewerkt om te ondersteunen dit als volgt:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

De waarde MetricAggregation van *PT1H* en *PT1M* geven aan een aggregatie gedurende een minuut en een aggregatie via een uur.

## <a name="wadmetrics-tables-in-storage"></a>WADMetrics tabellen in opslag

De bovenstaande parameters-configuratie genereert tabellen in uw account van de opslagruimte diagnostische gegevens met de volgende naamgevingsconventies:

- **WADMetrics** : standaardvoorvoegsel voor alle WADMetrics-tabellen
- **PT1H** of **PT1M** : duidt op dat de tabel statistische gegevens meer dan 1 uur of 1 minuut bevat
- **P10D** : duidt op de tabel bevat gegevens tien dagen uit wanneer het verzamelen van gegevens van de tabel wordt gestart
- **V2S** : tekenreeksconstante
- **JJJJMMDD** : de datum waarop de tabel gestart verzamelen van gegevens

Voorbeeld: *WADMetricsPT1HP10DV2S20151108* bevat de doelstellingen gegevens samengevoegd via een uur tien dagen starten op 11-november-2015    

Elke tabel WADMetrics bevat de volgende kolommen:

- **PartitionKey**: de partitionkey is samengesteld op basis van de waarde *resourceID* ter identificatie van de resource VM. voor bijvoorbeeld: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
- **RowKey** : de indeling volgt <Descending time tick>:<Performance Counter Name>. De berekening van de aflopende maatstreepjes is de maximale tijd maatstreepjes minus het tijdstip waarop het begin van de periode aggregatie. Bijvoorbeeld Als de periode steekproef op 10-november-2015 gestart en 00:00Hrs UTC en vervolgens de berekening zou: DateTime.MaxValue.Ticks - (nieuwe DateTime(2015,11,10,0,0,0,DateTimeKind.Utc). Tikken). Voor het geheugen beschikbare bytes prestaties teller de rij-toets eruit komen te zien zoals: 2519551871999999999__:005CMemory:005CAvailable:0020 Bytes
- **CounterName** : de naam van de teller prestaties. Dit komt overeen met de *counterSpecifier* gedefinieerd in het XML-config.
- **Maximale** : de maximumwaarde van de teller prestaties tijdens deze periode aggregatie.
- **Minimale** : de minimumwaarde van de teller prestaties tijdens deze periode aggregatie.
- **Totale** : de som van alle waarden van de teller prestaties gerapporteerd in de periode aggregatie te geven.
- **Aantal** : het totale aantal waarden voor de teller prestaties gerapporteerd.
- **Gemiddelde** : het gemiddelde (totaal/aantal)-waarde van de teller prestaties tijdens deze periode aggregatie.


## <a name="next-steps"></a>Volgende stappen

- Zie voor een volledige voorbeeldsjabloon van een virtuele Windows-computer met hulpprogramma's voor diagnose extensie [201-vm-cmdlets voor controle-hulpprogramma's voor diagnose-extensie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
- De resource manager-sjabloon met [Azure PowerShell](virtual-machines-windows-ps-manage.md) of [Azure-opdrachtregel](virtual-machines-linux-cli-deploy-templates.md) implementeren
- Meer informatie over [een presentatie resourcemanager Azure-sjablonen](../resource-group-authoring-templates.md)







