<properties
   pageTitle="Logboeken verzamelen met behulp van Diagnostisch hulpprogramma Azure | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe voor het instellen van Azure diagnostische hulpprogramma's voor het verzamelen van Logboeken vanaf een Service stof-cluster uitvoert in Azure wordt aangegeven."
   services="service-fabric"
   documentationCenter=".net"
   authors="ms-toddabel"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="toddabel"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Logboeken verzamelen met behulp van Azure diagnostische gegevens

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Wanneer u een cluster Azure-Service stof uitvoert, is het een goed idee om de logboeken verzamelen van alle knooppunten in een centrale locatie. De logboeken op een centrale locatie hebt, kunt u analyseren en oplossen van problemen in uw cluster of problemen in de toepassingen en services worden uitgevoerd in het cluster.

Er is een manier om te uploaden en logboeken verzamelen gebruik van de extensie Azure diagnostische hulpprogramma's, waarin logboeken naar de opslag van Azure uploadt. De logboeken zijn niet dat handig rechtstreeks in de opslagruimte. Maar kunt u een extern proces voor het lezen van de gebeurtenissen van opslag en deze in een product zoals [Log analyses](../log-analytics/log-analytics-service-fabric.md), [Elastische zoeken](service-fabric-diagnostic-how-to-use-elasticsearch.md)of een andere log parseren oplossing te plaatsen.

## <a name="prerequisites"></a>Vereisten voor
Gebruikt u deze hulpmiddelen om enkele van de bewerkingen in dit document uit te voeren:

* [Azure diagnostische gegevens](../cloud-services/cloud-services-dotnet-diagnostics.md) (betrekking hebben op Azure-Cloudservices maar bevat nuttige informatie en voorbeelden)
* [Azure resourcemanager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](../powershell-install-configure.md)
* [Azure resourcemanager-client](https://github.com/projectkudu/ARMClient)
* [Azure resourcemanager-sjabloon](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)


## <a name="log-sources-that-you-might-want-to-collect"></a>Log bronnen die u wilt verzamelen
- **Logboeken aan de service stof**: geeft het platform op standaard gebeurtenis Aanwijzen voor Windows (ETW) en EventSource kanalen. Logboeken kunnen verschillende typen zijn:
  - Operationele gebeurtenissen: logboeken voor bewerkingen die het platform Service stof kunt uitvoeren. Voorbeelden hiervan zijn het maken van toepassingen en -services, de status van verandert knooppunt en upgradegegevens.
  - [Betrouwbare betrokkenen programming model gebeurtenissen](service-fabric-reliable-actors-diagnostics.md)
  - [Betrouwbare Services programming model gebeurtenissen](service-fabric-reliable-services-diagnostics.md)
- **Toepassingsgebeurtenissen**: gebeurtenissen geeft code van uw service en geschreven met behulp van de EventSource helperklasse die beschikbaar zijn in de Visual Studio-sjablonen. Zie voor meer informatie over het schrijven van Logboeken uit uw toepassing [beeldscherm en een diagnose stellen bij services in de instellingen voor een lokale computer ontwikkeling](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).


## <a name="deploy-the-diagnostics-extension"></a>De extensie diagnostische gegevens implementeren
De eerste stap in de logboeken verzamelen is om te implementeren van de extensie diagnostische gegevens op elk van de VMs in het cluster Service stof. De extensie naar diagnostische logboeken verzamelt op elke VM en uploadt ze naar de opslag-account dat u opgeeft. De stappen afhankelijk iets van of u de Azure beheerportal of Azure Resource Manager gebruiken. De stappen wordt ook afhankelijk van of de implementatie maakt deel uit van maken van het cluster of voor een cluster die al bestaat. Bekijk de stappen voor elk scenario.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a>De extensie diagnostische gegevens als onderdeel van het maken van het cluster via de portal implementeren
Als u wilt implementeren de extensie diagnostische gegevens op de VMs in het cluster als onderdeel van het maken van het cluster, gebruikt u het deelvenster instellingen voor diagnostisch hulpprogramma dat wordt weergegeven in de volgende afbeelding. Zorg ervoor dat diagnostische gegevens is ingesteld op **op** (de standaardinstelling) om in te schakelen betrouwbare betrokkenen of betrouwbare Services gebeurtenis siteverzameling. Nadat u het cluster hebt gemaakt, kunt u deze instellingen niet wijzigen met behulp van de portal.

![Instellingen van de Azure diagnostische gegevens in de portal voor cluster maken](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

De ondersteuning van Azure team *vereist* ondersteuning Logboeken om op te lossen ondersteuningsaanvragen die u maakt. Deze logboeken worden verzameld in realtime en zijn opgeslagen in een van de opslag-accounts in de resourcegroep is gemaakt. De instellingen van de diagnostische gegevens configureren welke gebeurtenissen niveau van de webtoepassing. Deze gebeurtenissen bevatten [Betrouwbare betrokkenen](service-fabric-reliable-actors-diagnostics.md) gebeurtenissen, [Betrouwbare Services](service-fabric-reliable-services-diagnostics.md) gebeurtenissen en sommige systeem niveau Service stof moet worden opgeslagen in Azure Storage.

Producten, zoals [Elastische zoeken](service-fabric-diagnostic-how-to-use-elasticsearch.md) of uw eigen proces kunnen de gebeurtenissen krijgen van de opslag-account. Er is momenteel geen manier om de gebeurtenissen die zijn verzonden naar de tabel te verzorgen of hierop wilt filteren. Als u een proces om gebeurtenissen te verwijderen uit de tabel niet implementeert, blijft de tabel om te laten groeien.

Als u een cluster met behulp van de portal maakt, wordt ten zeerste aangeraden downloaden waarmee u de sjabloon *voordat u op * *OK*** de cluster maken. Voor meer informatie raadpleegt u voor het [instellen van een Service stof cluster met behulp van een sjabloon resourcemanager Azure](service-fabric-cluster-creation-via-arm.md). U moet de sjabloon te wijzigen later, omdat u geen enkele wijzigingen aanbrengen met behulp van de portal.

U kunt sjablonen uit de portal exporteren met behulp van de volgende stappen uit. Deze sjablonen kunnen echter zijn moeilijker te gebruiken, omdat ze null-waarden die de vereiste gegevens ontbreken mogelijk hebben.

1. Open de resourcegroep.
2. Selecteer **Instellingen** om het deelvenster instellingen openen.
3. Selecteer **implementaties** om het deelvenster van de geschiedenis implementatie openen.
4. Selecteer een implementatie om weer te geven van de details van de implementatie.
5. Selecteer **Sjabloon exporteren** om het deelvenster sjabloon openen.
6. Selecteer **opslaan naar bestand** exporteren van een ZIP-bestand met de sjabloon, parameter en PowerShell-bestanden.

Nadat u de bestanden hebt geëxporteerd, moet u een wijziging aanbrengt. Bewerk het bestand parameters.json en verwijder het element **beheerderswachtwoord** . Hierdoor wordt een wachtwoord wordt gevraagd wanneer de implementatiescript wordt uitgevoerd. Wanneer u de implementatiescript uitvoert, wordt u mogelijk om op te lossen null parameterwaarden.

Met de gedownloade sjabloon een configuratie bij te werken:

1. Pak de inhoud naar een map op uw lokale computer.
2. De inhoud zodat de nieuwe configuratie wijzigen.
3. PowerShell Start en Ga naar de map waarin u de inhoud hebt uitgepakt.
4. **Deploy.ps1** uitvoeren en vul de abonnements-ID, de naam van de resource groep (Gebruik dezelfde naam bij de configuratie) en de naam van een unieke implementatie.


### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>De extensie diagnostische gegevens als onderdeel van het maken van het cluster met behulp van Azure resourcemanager implementeren
Als u wilt een cluster met behulp van resourcemanager maakt, moet u de configuratie van de diagnostische JSON toevoegen aan de volledige cluster resourcemanager sjabloon voordat u het cluster maakt. We voorzien een voorbeeldsjabloon vijf-VM cluster resourcemanager door de configuratie van de diagnostische toegevoegd als onderdeel van de voorbeelden van onze resourcemanager sjabloon. Kunt u deze zien op deze locatie in de galerie met Azure voorbeelden: [cluster met vijf knooppunten met hulpprogramma's voor diagnose resourcemanager sjabloon steekproef](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

Als u wilt zien van de instelling diagnostische gegevens in de sjabloon resourcemanager, open de azuredeploy.json bestands- en zoeken naar **IaaSDiagnostics**. Als u wilt een cluster met deze sjabloon maakt, selecteert u de knop **distribueren naar Azure** beschikbaar op de vorige koppeling.

U kunt ook u kunt downloaden van de steekproef resourcemanager, wijzigen en een cluster maken met de gewijzigde sjabloon met behulp van de `New-AzureRmResourceGroupDeployment` command in een Azure PowerShell-venster. Zie de volgende code voor de parameters die u aan de opdracht doorgeeft. Zie het artikel [Deploy een resourcegroep met de sjabloon Azure resourcemanager](../resource-group-template-deploy.md)voor gedetailleerde informatie over het implementeren van een resourcegroep via PowerShell.

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>De extensie diagnostische gegevens implementeren naar een bestaand cluster
Als u een bestaand cluster die geen diagnostische gegevens die zijn geïmplementeerd, of als u wilt wijzigen van een bestaande configuratie, kunt u deze kunt toevoegen of bijwerken. Wijzig de sjabloon resourcemanager die wordt gebruikt voor het bestaande cluster maakt of de sjabloon downloaden van de portal zoals eerder is beschreven. Het bestand template.json wijzigen door de volgende taken uit te voeren.

Een nieuwe opslag-resource toevoegen aan de sjabloon door te voegen aan de sectie bronnen.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Voeg nu naar de sectie parameters direct na de definities van het account opslag, tussen `supportLogStorageAccountName` en `vmNodeType0Name`. Vervang de tijdelijke aanduiding voor tekst *opslagaccountnaam hier* met de naam van de opslag-account.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Past u de `VirtualMachineProfile` sectie van het bestand template.json door toe te voegen van de volgende code in de matrix extensies. Zorg ervoor dat u een komma toe te voegen aan het begin of het einde, afhankelijk van waar deze wordt ingevoegd.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Nadat u het bestand template.json wijzigen zoals wordt beschreven, opnieuw publiceren de resourcemanager-sjabloon. Als de sjabloon is geëxporteerd, het bestand deploy.ps1 rapport opnieuw publiceert de sjabloon. Nadat u geïmplementeerd, zorgen ervoor dat **ProvisioningState** **is voltooid**.


## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a>Diagnostische hulpprogramma's om te verzamelen en uploadt u logboeken van nieuwe EventSource kanalen bijwerken
Diagnostische gegevens te verzamelen logboeken van nieuwe EventSource kanalen die staan voor een nieuwe toepassing die u wilt implementeren, voert de stappen die in de [vorige sectie](#deploywadarm) voor het instellen van diagnostische hulpprogramma's voor een bestaand cluster bijwerken.

Werk de `EtwEventSourceProviderConfiguration` sectie in het bestand template.json vermeldingen toevoegen voor de nieuwe EventSource kanalen voordat u de configuratie toepast met behulp van bijwerken de `New-AzureRmResourceGroupDeployment` PowerShell-opdracht. De naam van de gebeurtenisbron is gedefinieerd als onderdeel van de code in het bestand ServiceEventSource.cs Visual Studio is gegenereerd.

Als de gebeurtenisbron mijn Eventsource heet, bijvoorbeeld de volgende code als u wilt de gebeurtenissen uit mijn Eventsource plaatsen in een tabel met de naam MyDestinationTableName toevoegen.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Als u wilt verzamelen prestatiemeteritems of gebeurtenislogboeken, wijzig u de sjabloon resourcemanager met behulp van de voorbeelden die beschikbaar zijn in [een virtuele Windows-computer met cmdlets voor controle en diagnostische gegevens met behulp van een resourcemanager Azure-sjabloon maken](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md). Vervolgens opnieuw publiceren de resourcemanager-sjabloon.

## <a name="next-steps"></a>Volgende stappen
Als u wilt weten over uitgebreider welke gebeurtenissen die u letten moet tijdens het oplossen van problemen, raadpleegt u de diagnostische gebeurtenissen gegenereerd voor [Betrouwbare betrokkenen](service-fabric-reliable-actors-diagnostics.md) en [Betrouwbare Services](service-fabric-reliable-services-diagnostics.md).


## <a name="related-articles"></a>Verwante artikelen
* [Leer hoe u de van prestatiemeteritems of Logboeken verzamelen met behulp van de extensie diagnostische gegevens](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)
* [Service stof oplossing in Log Analytics](../log-analytics/log-analytics-service-fabric.md)
