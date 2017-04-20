<properties
    pageTitle="Het activiteitenlogboek Azure naar gebeurtenis Hubs streamen | Microsoft Azure"
    description="Leer hoe u het gebeurtenissenlogboek van Azure op gebeurtenis Hubs streamen."
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
    ms.date="10/03/2016"
    ms.author="johnkem"/>

# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Het activiteitenlogboek Azure naar gebeurtenis Hubs streamen
Het [**Gebeurtenissenlogboek van Azure**](./monitoring-overview-activity-logs.md) kan worden streamen nabije realtime voor alle toepassingen met de ingebouwde 'Exporteren' optie in de portal of doordat de Service Bus regel-Id in een profiel Log via de PowerShell-Cmdlets Azure of Azure CLI.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Wat u kunt doen met de activiteit Log en de gebeurtenis Hubs
Hier volgen een paar manieren kunt u de streaming mogelijkheid voor het logboek:

- **Stream naar externe systemen voor logboekregistratie en telemetrielogboek** – na verloop van tijd gebeurtenis Hubs streaming worden de om uw activiteit Log in de derde partij SIEMs pipe en aanmelden analytics-oplossingen.
- **Maakt u een aangepaste telemetrielogboek en logboekregistratie platform** – als u al een platform op maat telemetrielogboek of hebben alleen nadenkt over het samenstellen van een, de ten zeerste scalable publiceren abonnementen aard van gebeurtenis Hubs kunt u het gebeurtenissenlogboek flexibel te nemen. [Zie Dan Rosanova de handleiding voor het gebruik van gebeurtenis Hubs in een hier wereldwijde schaal telemetrielogboek-platform.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Streaming van het logboek activiteit activeren
U kunt het inschakelen van het logboek activiteit streaming via programmacode of via de portal. In beide gevallen kiest u een Service Bus Namespace en een gedeelde-beleid voor die naamruimte en de Hub van een gebeurtenis in die naamruimte wordt gemaakt wanneer de eerste nieuwe activiteit Log gebeurtenis plaatsvindt. Als u niet een Service Bus Namespace hebt, moet u eerst een account maakt. Als u eerder hebt activiteit gebeurtenissen naar deze Service Bus Namespace streamen, wordt de gebeurtenis Hub die eerder is gemaakt opnieuw worden gebruikt. Het gedeelde-beleid Hiermee definieert u de machtigingen die de streaming om heeft. Tegenwoordig kunnen vereist streaming met een gebeurtenis Hubs machtigingen **beheren**, **lezen**en **verzenden** . U kunt maken of wijzigen van het clienttoegangsbeleid Service Bus Namespace gedeeld in de klassieke portal onder het tabblad 'Configureren' voor uw Service Bus Namespace. Als u het profiel van de activiteit Log log om op te nemen streaming bijwerken, moet de gebruiker de wijziging gemachtigd zijn het ListKey op die Service Bus autorisatieregel.

### <a name="via-azure-portal"></a>Via Azure-portal 
1. Ga naar het **Gebeurtenissenlogboek** blad gebruik het menu aan de linkerkant van de portal.

    ![Navigeer naar de activiteit Log in de portal](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klik op de knop **exporteren** aan de bovenkant van het blad.

    ![Knop exporteren in de portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. In het blad dat wordt weergegeven, kunt u de regio's waarvan u wilt naartoe stream evenementen en de Service Bus Namespace waarin u een Hub voor de gebeurtenis moet worden gemaakt voor deze gebeurtenissen streaming wilt selecteren.

    ![Activiteit Log blade exporteren](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klik op **Opslaan** als deze instellingen wilt opslaan. De instellingen worden direct worden toegepast op uw abonnement.


### <a name="via-powershell-cmdlets"></a>Via PowerShell-Cmdlets
Als er al een logboek-profiel bestaat, moet u eerst dat profiel verwijderen.

1. Gebruik `Get-AzureRmLogProfile` om aan te geven als een logboek-profiel bestaat
2. Als dat het geval is, gebruikt u `Remove-AzureRmLogProfile` om deze te verwijderen.
3. Gebruik `Set-AzureRmLogProfile` een profiel maken:

```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

De Service Bus regel-ID is een tekenreeks met deze indeling: {service bus resource-ID} /authorizationrules/ {belangrijke naam}, bijvoorbeeld 

### <a name="via-azure-cli"></a>Via Azure CLI
Als er al een logboek-profiel bestaat, moet u eerst dat profiel verwijderen.

1. Gebruik `azure insights logprofile list` om aan te geven als een logboek-profiel bestaat
2. Als dat het geval is, gebruikt u `azure insights logprofile delete` om deze te verwijderen.
3. Gebruik `azure insights logprofile add` een profiel maken:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

De Service Bus regel-ID is een tekenreeks met deze indeling: `{service bus resource ID}/authorizationrules/{key name}`.
 
## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Hoe ik de logboekgegevens van gebeurtenis Hubs gebruiken?
[Het schema voor het logboek is hier beschikbaar](./monitoring-overview-activity-logs.md). Elke gebeurtenis bevindt zich in een matrix van JSON BLOB's genoemd "records."

## <a name="next-steps"></a>Volgende stappen
- [Het logboek met een account opslag archiveren](./monitoring-archive-activity-log.md)
- [Lees het overzicht van het gebeurtenissenlogboek van Azure](./monitoring-overview-activity-logs.md)
- [Een waarschuwing op basis van een activiteit logboekgebeurtenis instellen](./insights-auditlog-to-webhook-email.md)
