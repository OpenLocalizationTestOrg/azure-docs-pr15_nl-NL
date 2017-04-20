<properties
    pageTitle="Aan de slag met rollen, machtigingen en beveiliging met Azure Monitor | Microsoft Azure"
    description="Informatie over het gebruik van de ingebouwde rollen en machtigingen van Azure Monitor beperken van toegang aan resources cmdlets voor controle."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Aan de slag met rollen, machtigingen en beveiliging met Azure Monitor

Veel teams moeten strikt regelen toegang tot gegevens en instellingen voor controle. Als er teamleden die bezig bent met uitsluitend monitoring (ondersteuningsmedewerkers, devops engineers) of als u een beheerde serviceprovider gebruikt, u kunt ze toegang tot het alleen-gegevens bewaken terwijl de beperkingen voor de mogelijkheid om te maken, wijzigen of verwijderen van resources. In dit artikel leest hoe u snel een ingebouwde controle RBAC rol toepassen op een gebruiker in Azure wordt aangegeven of uw eigen aangepaste rol voor een gebruiker die beperkte controleren machtigingen moet maken. Deze vervolgens besproken beveiligingsoverwegingen voor uw resources Azure Monitor-gerelateerde en hoe u toegang tot de gegevens die erin kunnen alleen.

## <a name="built-in-monitoring-roles"></a>Ingebouwde controleren rollen

Ingebouwde rollen van Azure Monitor zijn ontworpen om u te helpen beperken van toegang tot bronnen in een abonnement terwijl u toch degenen die verantwoordelijk zijn voor het controleren van infrastructuur verkrijgen en configureren van de gegevens die zij nodig hebben. Azure Monitor biedt twee out-van-het-box-rollen: A Monitoring Reader en een inzender bewaken.

### <a name="monitoring-reader"></a>Monitoring Reader

Personen die de lezer Monitoring rol toegewezen kunt alle controlegegevens weergeven in een abonnement, maar kan wijzigen een resource of een gerelateerd aan resources cmdlets voor controle-instellingen bewerken. Deze rol is geschikt voor gebruikers in een organisatie, zoals ondersteuning of bewerkingen engineers willen zichtbaar mogen zijn:

- Controle dashboards in de portal weergeven en hun eigen persoonlijke controleren dashboards maken.
- Query met de [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx), [PowerShell-cmdlets](insights-powershell-samples.md)of [meerdere platforms CLI](insights-cli-samples.md)aan de doelstellingen.
- De activiteit-logboek met de portal Azure Monitor REST API, PowerShell-cmdlets of meerdere platforms CLI zoeken.
- De [Diagnostische instellingen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) voor een resource weergeven.
- Bekijk het [logboek profiel](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) voor een abonnement.
- Automatisch schalen instellingen weergeven.
- Waarschuwing activiteit weergeven en instellingen.
- Toegang tot gegevens van de toepassing inzichten en gegevens weergeven in AI Analytics.
- Zoek Log Analytics Kantoorbeheersysteem werkruimtegegevens inclusief gegevens over het gebruik van de werkruimte.
- Log Analytics Kantoorbeheersysteem management groepen weergeven.
- Het zoekschema Log Analytics Kantoorbeheersysteem ophalen.
- Lijst Log Analytics Kantoorbeheersysteem intelligence packs.
- Ophalen en Log Analytics Kantoorbeheersysteem opgeslagen zoekopdrachten uitvoeren.
- Hiermee kunt u de configuratie van de opslagruimte Log Analytics Kantoorbeheersysteem ophalen.

> [AZURE.NOTE] Deze rol heeft niet alleen toegang geven tot logboekgegevens die is streamen op de hub van een gebeurtenis of die zijn opgeslagen in een opslag-account. [Zie hieronder](#security-considerations-for-monitoring-data) voor informatie over het configureren van toegang tot deze resources.

### <a name="monitoring-contributor"></a>Cmdlets voor controle Inzender

Personen die de rol Inzender Monitoring toegewezen alle controlegegevens kunt weergeven in een abonnement en maken of wijzigen van de controle-instellingen, maar andere bronnen niet wijzigen. Deze rol is een uitbreiding van de functie Lezer cmdlets voor controle en is geschikt te maken voor leden van de controle team of beheerde serviceproviders die, naast de bovenstaande, machtigingen ook moeten kunnen van een organisatie:

- Controle dashboards publiceren als een gedeelde dashboard.
- [Diagnostische instellingen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) voor een resource.* instellen
- Het [logboek profiel](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) voor een subscription.* instellen
- Stel waarschuwingen activiteit en instellingen.
- Maak toepassing inzichten web tests en onderdelen.
- Lijst Log Analytics Kantoorbeheersysteem werkruimte gedeeld toetsen.
- In- of uitschakelen van Log Analytics Kantoorbeheersysteem intelligence packs.
- Maken en verwijderen en Log Analytics Kantoorbeheersysteem opgeslagen zoekopdrachten uitvoeren.
- Maken en verwijderen van de configuratie van de opslagruimte Log Analytics Kantoorbeheersysteem.

* gebruiker moet ook afzonderlijk machtiging ListKeys op de doel-resource (opslag-account of gebeurtenis hub naamruimte) of een logboek profiel of diagnostische instelling wilt instellen.

> [AZURE.NOTE] Deze rol heeft niet alleen toegang geven tot logboekgegevens die is streamen op de hub van een gebeurtenis of die zijn opgeslagen in een opslag-account. [Zie hieronder](#security-considerations-for-monitoring-data) voor informatie over het configureren van toegang tot deze resources.

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Machtigingen en aangepaste RBAC rollen voor controle
Als de bovenstaande ingebouwde rollen niet met de exacte behoeften van uw team overeenkomen, kunt u [een aangepaste RBAC rol maken](../active-directory/role-based-access-control-custom-roles.md) met meer gedetailleerde machtigingen. Hieronder vindt u de algemene Azure Monitor RBAC bewerkingen met de bijbehorende beschrijvingen.

| Bewerking                                                   | Beschrijving                                                                                                                                               |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Microsoft.Insights/AlertRules/[Read, schrijven, verwijderen]         | De waarschuwingsregels lezen/schrijven/verwijderen.                                                                                                                            |
| Microsoft.Insights/AlertRules/Incidents/Read                | Incidenten (geschiedenis van de huidige regel wordt geactiveerd) voor waarschuwingsregels vermeld. Dit geldt alleen voor de portal.                                              |
| Microsoft.Insights/AutoscaleSettings/[Read, schrijven, verwijderen]  | Instellingen voor het automatisch schalen lezen/schrijven/verwijderen.                                                                                                                     |
| Microsoft.Insights/DiagnosticSettings/[Read, schrijven, verwijderen] | Diagnostische instellingen lezen/schrijven/verwijderen.                                                                                                                    |
| Microsoft.Insights/eventtypes/digestevents/Read             | Met deze machtiging is vereist voor gebruikers die toegang tot de activiteitenlogboeken via de portal hebben.                                                                   |
| Microsoft.Insights/eventtypes/values/Read                   | Lijst met activiteit gebeurtenissen (gebeurtenissen) in een abonnement. Met deze machtiging is van toepassing op programma zowel portal toegang tot het gebeurtenissenlogboek. |
| Microsoft.Insights/LogDefinitions/Read                      | Met deze machtiging is vereist voor gebruikers die toegang tot de activiteitenlogboeken via de portal hebben.                                                                   |
| Microsoft.Insights/MetricDefinitions/Read                   | Lees metrische definities (lijst met beschikbare metrische typen voor een resource).                                                                                  |
| Microsoft.Insights/Metrics/Read                             | Lees de doelstellingen voor een resource.                                                                                                                              |

> [AZURE.NOTE] Toegang tot uw waarschuwingen, diagnostische instellingen en aan de doelstellingen voor een resource is vereist dat de gebruiker alleen toegang tot de resourcetype en het bereik van deze resource heeft. ('Schrijven') maken moet een diagnostische instelling of log profiel dat aan een account opslag of gegevensstromen naar gebeurtenis hubs-archieven de gebruiker ook de ListKeys machtiging voor de resource doel hebben.

Bijvoorbeeld, met behulp van de bovenstaande tabel kon u een aangepaste RBAC rol maken voor een 'activiteit Log Reader"als volgt:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Beveiligingsoverwegingen voor gegevens bewaken
-Gegevens bewaken, met name logboekbestanden, vertrouwelijke informatie, zoals IP-adressen of gebruikersnamen kan bevatten. Gegevens van Azure is beschikbaar in drie basisformulieren:
1. Het logboekbestand activiteit, dat wordt beschreven acties voor alle besturingselement vlak voor uw abonnement op Azure.
2. Diagnostische logboeken, logboeken dat door een resource zijn.
3. Aan de doelstellingen, die zijn gegenereerd door alle resources.

Alle drie van deze gegevenstypen kan worden opgeslagen in een opslag-account of streamen op gebeurtenis-Hub, die beide algemene Azure resources zijn. Omdat dit zijn de algemene resources, is maken, verwijderen en toegang tot deze een bevoegde bewerking meestal gereserveerd voor een beheerder. Het is raadzaam dat u de volgende procedures voor resources monitoring-gerelateerde kiosk gebruiken:

- Gebruik een eenmalige, speciale opslag-account voor gegevens bewaken. Als u scheiden van controlegegevens in meerdere opslag-accounts wilt, nooit delen gebruik van een account opslag tussen cmdlets voor controle en niet-cmdlets voor controle gegevens, als dit mogelijk per ongeluk bieden die alleen toegang tot (bv-gegevens bewaken.) een derde partij SIEM) toegang tot de niet-cmdlets voor controle gegevens.
- Een eenmalige, speciale Service Bus of gebeurtenis Hub-naamruimte over alle diagnostische instellingen gebruiken om dezelfde reden als hierboven.
- Toegang tot opslag monitoring-gerelateerde accounts of gebeurtenis hubs doordat ze op een afzonderlijke resourcegroep, en [bereik gebruiken](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) op uw controleren rollen beperken van toegang tot alleen die resourcegroep beperken.
- Nooit de machtiging ListKeys voor opslag-accounts of gebeurtenis hubs in het bereik van abonnement wanneer een gebruiker alleen toegang moet tot gegevens bewaken. In plaats daarvan deze machtigingen geven aan de gebruiker aan een resource of resourcegroep (als u een speciale controle resourcegroep hebt) bereik.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>Beperken van toegang tot opslag monitoring-gerelateerde accounts
Wanneer een gebruiker of een toepassing moet toegang tot gegevens in een account opslagruimte bewaken, moet u [een Account SA's genereren](https://msdn.microsoft.com/library/azure/mt584140.aspx) voor de opslag-account met controlegegevens met service level alleen-lezen toegang tot blobopslag. In PowerShell, dit als volgt uitzien:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Vervolgens kunt u het token geven tot het bedrijf dat moet te lezen die opslag-account, waarna ze lijst vanaf kunt lezen en alle BLOB's in dat account opslag.

U kunt ook als u nodig hebt om te bepalen met deze machtiging met RBAC, kunt u verlenen die entiteit de machtiging Microsoft.Storage/storageAccounts/listkeys/action dat bepaalde opslag-account. Dit is nodig zijn voor gebruikers die kunnen een diagnostische instellen of meld profiel moeten archiveren met een opslag-account. U kunt de volgende aangepaste RBAC rol voor een gebruiker of een toepassing die alleen moet lezen van de ene opslag-account, bijvoorbeeld kunnen maken:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [AZURE.WARNING] De machtiging ListKeys kan de gebruiker voor een overzicht van de primaire en secundaire opslag-account te gebruiken. Deze toetsen Verleen de gebruiker alle machtigingen ondertekende (lezen, schrijven, BLOB's maken, verwijderen BLOB's, enzovoort) voor alle ondertekend services (blob, wachtrij, tabel, bestand) in dat account opslag. We raden u aan een Account SA's die hierboven wordt beschreven, indien mogelijk gebruiken.

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>Beperken van toegang tot de cmdlets voor controle-gerelateerde gebeurtenis hubs
Een soortgelijke patroon met gebeurtenis hubs kan worden uitgevoerd, maar moet u eerst een speciale luisteren autorisatieregel maken. Als u verlenen van toegang tot een toepassing die alleen moet beluisteren monitoring-gerelateerde gebeurtenis hubs wilt, als volgt:

1. Maak een gedeelde-beleid op de hub van de gebeurtenis die zijn gemaakt voor streaming controlegegevens met alleen beluisteren van claims. Dit kunt doen in de portal. Zoals misschien u Roep deze "monitoringReadOnly." Indien mogelijk, zult u naar die toets geven rechtstreeks naar de consument en slaat u de volgende stap.
2. Als de consument kunnen bereiken van de belangrijkste ad-hoc moet, Verleen de gebruiker de actie ListKeys voor deze hub gebeurtenis. Dit is ook nodig zijn voor gebruikers die moeten kunnen een diagnostische instellen of meld u profiel bij stream met hubs gebeurtenis. U kunt bijvoorbeeld een RBAC-regel maken:

   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```


## <a name="next-steps"></a>Volgende stappen
- [Lees meer over RBAC en machtigingen in resourcemanager](../active-directory/role-based-access-control-what-is.md)
- [Lees het overzicht van de cmdlets voor controle in Azure wordt aangegeven](monitoring-overview.md)
