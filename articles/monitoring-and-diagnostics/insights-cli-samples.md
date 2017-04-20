<properties
    pageTitle="Azure Monitor CLI snel aan de slag voorbeelden. | Microsoft Azure"
    description="Voorbeeld CLI opdrachten voor het beeldscherm van Azure-functies. Azure Monitor is een Microsoft Azure-service waarmee u voor het verzenden van waarschuwingen, bellen web-URL's op basis van waarden van geconfigureerde telemetriegegevens, en automatisch schalen Cloud Services, virtuele Machines en Web Apps."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor--cross-platform-cli-quick-start-samples"></a>Voorbeelden van Azure Monitor platforms CLI snel aan de slag

In dit artikel leest u voorbeeld opdrachtregel-interface (CLI) opdrachten kunt u toegang tot de functies van Azure Monitor. Azure Monitor kunt u automatisch schalen Cloud Services, virtuele Machines en Web Apps en waarschuwingen verzenden of web-URL's op basis van waarden van geconfigureerde telemetriegegevens bellen.

>[AZURE.NOTE] Azure Monitor is de nieuwe naam voor het zogenoemde "Azure inzichten" tot 25e sep 2016. Echter bevatten de naamruimten en dus de onderstaande opdrachten nog steeds de "inzichten".


## <a name="prerequisites"></a>Vereisten voor

Als u de CLI Azure al dat nog niet hebt geïnstalleerd, raadpleegt u [de CLI Azure installeren](../xplat-cli-install.md). Als u niet bekend met Azure CLI bent, kunt u meer informatie over deze bij het [gebruik van de Azure CLI voor Mac, Linux, en Windows met Azure Resource Manager](../xplat-cli-azure-resource-manager.md).


Klik in Windows npm te installeren via de [website van Node.js](https://nodejs.org/). Nadat u de installatie voltooid met CMD.exe met bevoegdheden voor als Administrator uitvoeren, moet u de volgende handelingen uit uitvoeren uit de map waarop npm is geïnstalleerd:

```console
npm install azure-cli --global
```

Ga vervolgens naar de gewenste map/locatie gewenste naam in en typt u in de opdrachtregel:

```console
azure help
```

## <a name="log-in-to-azure"></a>Meld u aan bij Azure

De eerste stap is Meld u aan bij uw Azure-account.

```console
azure login
```

Na het uitvoeren van deze opdracht, moet u zich aanmelden via de instructies op het scherm. Hierna ziet u uw Account, TenantId en standaard abonnement-id. Alle opdrachten werken in de context van uw standaardabonnement.

U kunt de details van uw huidige abonnement door de volgende opdracht uit te gebruiken.

```console
azure account show
```

Gebruik de volgende opdracht uit als u wilt werken context wijzigen in een ander abonnement.

```console
azure account set "subscription ID or subscription name"
```

Azure resourcemanager en Azure Monitor als opdrachten wilt gebruiken, moet u zich in de modus van Azure resourcemanager.

```console
azure config mode arm
```

Ga als volgt te werk om een lijst met alle ondersteunde Azure Monitor opdrachten weergeven.

```console
azure insights
```

## <a name="view-audit-logs-for-a-subscription"></a>Weergave controlelogboeken bijhouden voor een abonnement

Ga als volgt te werk als u wilt weergeven in een lijst met controlelogboeken bijhouden.

```console
azure insights logs list [options]
```

Probeer de volgende handelingen uit om alle beschikbare opties weer te geven.

```console
azure insights logs list -help
```

Hier volgt een voorbeeld naar Logboeken aan de lijst door een resourceGroup

```console
azure insights logs list --resourceGroup "myrg1"
```

Voorbeeld naar Logboeken aan de lijst door beller

```console
azure insights logs list --caller "myname@company.com"
```

Voorbeeld naar Logboeken aan de lijst door beller op een type resource, binnen een begin- en -datum

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Werken met waarschuwingen
U kunt de gegevens in de sectie om te werken met waarschuwingen.

### <a name="get-alert-rules-in-a-resource-group"></a>Waarschuwingsregels ophalen in een resourcegroep

```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Een metrische Waarschuwing regel maken

```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-a-log-alert-rule"></a>Een waarschuwing log-regel maken

```console
azure insights alerts rule log set ruleName eastus resourceGroupName someOperationName
```

### <a name="create-webtest-alert-rule"></a>Webtest waarschuwingsregels maken

```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Een waarschuwing regel verwijderen

```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Log profielen
Gebruik de informatie in deze sectie om te werken met log profielen.

### <a name="get-a-log-profile"></a>Een profiel log ophalen

```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Een profiel log zonder bewaarbeleid toevoegen

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Een profiel log verwijderen

```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Een profiel log met bewaarbeleid toevoegen

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Een profiel log met bewaren en EventHub toevoegen

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Diagnostische hulpprogramma 's
Gebruik de informatie in deze sectie om te werken met diagnostische instellingen.

### <a name="get-a-diagnostic-setting"></a>Een instelling voor diagnostische gegevens ophalen

```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Een diagnostische instelling uitschakelen

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Een diagnostische instelling zonder bewaarbeleid inschakelen

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Automatisch schalen
Gebruik de informatie in deze sectie om te werken met automatisch schalen-instellingen. U moet deze voorbeelden wijzigen.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Automatisch schalen instellingen voor een resourcegroep

```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>U automatisch schalen instellingen op naam in een resourcegroep

```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Instellingen voor auotoscale instellen

```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
