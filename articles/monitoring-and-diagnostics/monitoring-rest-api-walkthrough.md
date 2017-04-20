<properties
    pageTitle="Azure REST API Stapsgewijze instructies voor controle | Microsoft Azure"
    description="Het aanvragen om te verifiëren en gebruiken van de REST API Azure bewaken."
    authors="mcollier, rboucher"
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
    ms.date="09/27/2016"
    ms.author="mcollier"/>

# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure REST API Stapsgewijze instructies voor controle
Dit artikel leest u hoe u verificatie uitvoert, zodat uw code de [Microsoft Azure Monitor REST API-naslaggids](https://msdn.microsoft.com/library/azure/dn931943.aspx)kunt gebruiken.         

De Monitor-API van Azure kunt ophalen via een programma de beschikbare standaard metrische definities (het type van meting zoals CPU-tijd, aanvragen, enz.), granulatie en metrische waarden. Zodra opgehaald, kunnen de gegevens in een afzonderlijk gegevensopslag zoals Azure SQL-Database, DocumentDB of Lake van Azure-gegevens worden opgeslagen. Daarvandaan kunt desgewenst aanvullende analyse worden uitgevoerd.

Naast het werken met verschillende metrische gegevenspunten, zoals in dit artikel wordt beschreven, kunnen de Monitor-API voor een overzicht van waarschuwingsregels, logboeken van de activiteit weergeven en nog veel meer. Voor een volledige lijst met beschikbare bewerkingen, raadpleegt u [Microsoft Azure Monitor REST API verwijzing](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Verificatie van Azure Monitor aanvragen

De eerste stap is om te verifiëren van het verzoek.

Alle taken uitgevoerd voor de Azure Monitor-API gebruiken het verificatiemodel resourcemanager Azure. Daarom moeten alle aanvragen worden geverifieerd met Azure Active Directory (Azure AD). Eén manier om te verifiëren de clienttoepassing is een Azure AD-service principal maken en het verificatietoken (JWT) op te halen. Het volgende voorbeeldscript ziet u een Azure AD-service principal via PowerShell maken. Voor een gedetailleerde overzicht, raadpleegt u de documentatie over het [gebruik van Azure PowerShell als u wilt maken van een service hoofdsom voor toegang tot resources](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password—powershell). Het is ook mogelijk te [maken van een service-principe via de portal van Azure](../resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Als u wilt de Azure Monitor API een query uitvoert, moet de clienttoepassing de hoofdsom eerder gemaakte service gebruiken om te verifiëren. Het volgende voorbeeld PowerShell-script ziet één methode, de [Bibliotheek van Active Directory-verificatie](../active-directory/active-directory-authentication-libraries.md) (ADAL) om de verificatie JWT token aan te kunnen gebruiken. Het token JWT wordt doorgegeven als onderdeel van een parameter HTTP autorisatie in aanvragen tot de Azure Monitor REST API.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.windows.net/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)
 
$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values 
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Zodra de stap van de instelling verificatie voltooid is, kunnen vervolgens query's worden uitgevoerd voor de Azure Monitor REST API. Er zijn twee handige query's:

1. Lijst van de metrische definities voor een resource

2. De metrische waarden ophalen


## <a name="retrieve-metric-definitions"></a>Metrische definities ophalen
>[AZURE.NOTE] Als u wilt ophalen metrische definities van de Azure Monitor REST API gebruiken, '2016-03-01' als de API-versie te gebruiken.

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
Voor een Azure logica-App, zou de metrische definities vergelijkbaar met de volgende schermafbeelding weergegeven:

![ALT "JSON weergave van metrische definitie antwoord."](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

Zie de documentatie [lijst de metrische definities voor een resource in Azure Monitor REST API](https://msdn.microsoft.com/library/azure/mt743621.aspx) voor meer informatie.

## <a name="retrieve-metric-values"></a>Metrische waarden ophalen
Nadat de beschikbare metrische definities bekend is, is het vervolgens wel kunt ophalen van de verwante metrische waarden. Gebruik van de meetwaarde 'waarde' (niet de ' localizedValue') voor alle filteren aanvragen (bijvoorbeeld de 'CpuTime' en 'Aanvraagt' metrische gegevenspunten ophalen). Als er geen filters zijn opgegeven, wordt de standaard-meetwaarde geretourneerd.

>[AZURE.NOTE] Metrische om waarden te vinden met behulp van de Azure Monitor REST API, '2016-06-01' als de API-versie te gebruiken.

**Methode**: ophalen

**URI aanvragen**: https://management.azure.com/subscriptions/_{abonnements-id}_/resourceGroups/_{groep-bronnaam}_/providers/_{resource-provider-naamruimte}_/_{type resource}_/_{Resourcenaam}_/providers/microsoft.insights/metrics?$filter=_{filter}_en api-versie =_{apiVersion}_

Als u wilt ophalen de RunsSucceeded metrische gegevenspunten voor het opgegeven tijdsbereik en voor een tijd gelijk is aan of 1 uur, zou de aanvraag bijvoorbeeld als volgt:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

Het resultaat lijkt vergelijkbaar met het voorbeeld schermafbeelding te volgen:

![ALT "JSON antwoord met gemiddelde antwoord metrische tijdwaarde"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

Als u wilt ophalen meerdere gegevens of aggregatie punten, toevoegen de namen van de metrische definitie en aggregatietypen aan het filter, zoals gezien in het volgende voorbeeld:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a>ARMClient gebruiken
Een alternatief voor het via PowerShell (zoals hierboven), is het gebruik van [ARMClient](https://github.com/projectkudu/ARMClient) op uw Windows-computer. ARMClient verwerkt automatisch de Azure AD-verificatie (en de resulterende JWT token). Gebruik van ARMClient voor het ophalen van metrische gegevens een overzicht maken van de volgende stappen uit:

1. Installeer [Chocolatey](https://chocolatey.org/) en [ARMClient](https://github.com/projectkudu/ARMClient).

2. Typ in een venster terminal _armclient.exe login_. Dit wordt gevraagd u aan te melden Azure.

3. Type _armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01_

4. Type _armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01_


![ALT 'ARMClient gebruiken om te werken met de Azure Monitoring REST API'](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)


## <a name="retrieve-the-resource-id"></a>De Resource-ID ophalen
Met behulp van de REST API kunt echt voor meer informatie over de beschikbare metrische definities, granulatie en gerelateerde waarden. Deze informatie is handig bij gebruik van de [Azure Management bibliotheek](https://msdn.microsoft.com/library/azure/mt417623.aspx).

Voor de bovenstaande code is de resource-ID gebruiken het volledige pad naar de gewenste Azure resource. Als u wilt query uitvoeren op een Azure-Web-App, zou de resource-ID bijvoorbeeld:

*/Subscriptions/{Subscription-id}/resourceGroups/{resource-Group-Name}/providers/Microsoft.Web/sites/{site-name}/*

De volgende lijst bevat enkele voorbeelden van resource-ID notaties voor verschillende Azure bronnen:

* **IoT Hub** - /subscriptions/_{abonnements-id}_/resourceGroups/_{groep-bronnaam}_/providers/Microsoft.Devices/IotHubs/_{iot-hub-naam}_

* **Elastische SQL-toepassingen** - /subscriptions/_{abonnements-id}_/resourceGroups/_{groep-bronnaam}_/providers/Microsoft.Sql/servers/_{groep-db}_/elasticpools/_{sql-toepassingen-naam}_

* **SQL-Database (v12)** - /subscriptions/_{abonnements-id}_/resourceGroups/_{groep-bronnaam}_/providers/Microsoft.Sql/servers/_{servernaam}_/databases/_{databasenaam}_

* **Service Bus** - /subscriptions/_{abonnements-id}_/resourceGroups/_{groep-bronnaam}_/providers/Microsoft.ServiceBus/_{naamruimte}_/_{servicebus-naam}_

* **VM schaal Sets** - /subscriptions/_{abonnements-id}_/resourceGroups/_{groep-bronnaam}_/providers/Microsoft.Compute/virtualMachineScaleSets/_{vm-naam}_

* **VMs** - /subscriptions/_{abonnements-id}_/resourceGroups/_{groep-bronnaam}_/providers/Microsoft.Compute/virtualMachines/_{vm-naam}_

* **Gebeurtenis Hubs** - /subscriptions/_{abonnements-id}_/resourceGroups/_{groep-bronnaam}_/providers/Microsoft.EventHub/namespaces/_{eventhub-naamruimte}_


Er zijn alternatieve benaderingen tot het ophalen van de resource-ID, inclusief het gebruik van Azure Resource Explorer, de gewenste resource weergeven in de portal van Azure en via PowerShell of de CLI Azure.

### <a name="azure-resource-explorer"></a>Azure Resource Explorer
Als u de resource-ID voor een resource gewenste zoekt, is een handige methode gebruikt u het hulpprogramma [Azure Resource Explorer](https://resources.azure.com) . Ga naar de gewenste resource en kijkt u naar de ID die wordt weergegeven, zoals in de volgende schermafbeelding:

![ALT "Azure Resource Explorer"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure-portal
De resource-ID kan ook worden opgehaald uit de Azure-portal. Als u wilt doen, navigeer naar de gewenste resource en selecteer vervolgens Eigenschappen. De Resource-ID weergegeven in het blad eigenschappen, zoals gezien in de volgende schermafbeelding:

![Alternatieve "Resource-ID weergegeven in het blad eigenschappen in de portal van Azure"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
De resource-ID kan worden opgehaald met ook Azure PowerShell-cmdlets. Bijvoorbeeld voor de resource-ID voor een Azure-Web-App, voert u de cmdlet Get-AzureRmWebApp, zoals in de volgende schermafbeelding uit:

![Alternatieve "Resource-ID verkregen via PowerShell"](./media\monitoring-rest-api-walkthrough\resourceid_powershell.png)

### <a name="azure-cli"></a>Azure CLI
Als u wilt ophalen van het gebruik van de CLI Azure resource-ID, voer de opdracht 'azure webapp weergeven', geven de '--json' optie, zoals wordt weergegeven in de volgende schermafbeelding:

![Alternatieve "Resource-ID verkregen via PowerShell"](./media\monitoring-rest-api-walkthrough\resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Activiteit logboekgegevens ophalen
Naast het werken met metrische definities en gerelateerde waarden is het ook mogelijk om op te halen extra interessante inzichten die betrekking hebben op Azure resources. Als voorbeeld is het mogelijk te query [activiteit](https://msdn.microsoft.com/library/azure/dn931934.aspx) logboekgegevens. Het volgende voorbeeld wordt gedemonstreerd met behulp van de Azure Monitor REST API query activiteit logboekgegevens binnen een bepaald datumbereik voor een Azure-abonnement:

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a>Volgende stappen
* Bekijk het [overzicht van de cmdlets voor controle](monitoring-overview.md).
* De [ondersteunde aan de doelstellingen met Azure Monitor](monitoring-supported-metrics.md)bekijken.
* Bekijk de [Microsoft Azure Monitor REST API verwijzing](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Bekijk de [bibliotheek van Azure Management](https://msdn.microsoft.com/library/azure/mt417623.aspx).
