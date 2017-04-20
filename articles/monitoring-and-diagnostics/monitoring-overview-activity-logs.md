<properties
    pageTitle="Overzicht van het activiteitenlogboek Azure | Microsoft Azure"
    description="Lees wat het gebeurtenissenlogboek van Azure is en hoe u deze kunt gebruiken voor meer informatie over gebeurtenissen in uw Azure-abonnement."
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
    ms.date="10/25/2016"
    ms.author="johnkem"/>

# <a name="overview-of-the-azure-activity-log"></a>Overzicht van het activiteitenlogboek Azure
Het **Gebeurtenissenlogboek van Azure** is een logboek vindt u inzicht in de bewerkingen die zijn uitgevoerd voor bronnen in uw abonnement. Het gebeurtenissenlogboek was voorheen bekend als "Controlelogboeken" of "Operationele Logboeken," Aangezien het besturingselement vlak-gebeurtenissen voor uw abonnementen rapporten. Het logboek met, kunt u bepalen de ' wat, wie, en wanneer ' voor een bewerkingen (opgeslagen, bericht, DELETE schrijven) die u hebt gemaakt op de informatiebronnen in uw abonnement. U kunt ook de status van de bewerking en andere relevante eigenschappen kennen. Het gebeurtenissenlogboek bevat geen gelezen (GET) bewerkingen.

Het gebeurtenissenlogboek verschilt van [Diagnostische logboeken](./monitoring-overview-of-diagnostic-logs.md), waarin alle logboeken dat door een resource zijn. Deze logboeken leveren gegevens over de werking van deze resource, in plaats van bewerkingen voor die resource.

U kunt gebeurtenissen ophalen uit uw activiteitenlogboek met behulp van de Azure portal, CLI, PowerShell-cmdlets en Azure Monitor REST API.

Bekijk deze [video Inleiding tot het gebeurtenissenlogboek](https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz).  

## <a name="what-you-can-do-with-the-activity-log"></a>Wat u kunt doen met het logboek
Hier volgen enkele dingen die u met het gebeurtenissenlogboek doen kunt:

- Query en te bekijken in de **portal van Azure**.
- Query deze via een REST API, PowerShell-Cmdlet of CLI.
- [Een e-mail of webhook waarschuwing die uit een activiteit logboekgebeurtenis activeert maken.](./insights-auditlog-to-webhook-email.md)
- [Opslaan in een **Account van opslagruimte** voor archiveren of handmatige controle](./monitoring-archive-activity-log.md). Het bewaarbeleid tijd (in dagen) gebruik van **Log profielen**, kunt u opgeven.
- Analyseren in PowerBI met het [**PowerBI inhoud pack**](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/).
- [Streamen deze op een **Gebeurtenis Hub** ](./monitoring-stream-activity-logs-event-hubs.md) voor opname door een derde partij service of aangepaste analytics-oplossing zoals PowerBI.

## <a name="export-the-activity-log-with-log-profiles"></a>Het logboek met Log profielen exporteren
Een **Logboek profiel** bepaalt hoe uw activiteit Log worden geëxporteerd. Een profiel Log gebruikt, kunt u configureren:

- Waar het gebeurtenissenlogboek moet worden gezonden (opslag-Account of gebeurtenis Hubs)
- Welke gebeurteniscategorieën (schrijven, verwijderen, actie) moeten worden verzonden
- Welke regio's (locaties) moeten worden geëxporteerd.
- Hoe lang het gebeurtenissenlogboek moeten worden bewaard in een opslag-Account: een bewaarbeleid van nul dagen betekent dat er logboeken eeuwen worden bewaard. De waarde mag anders een willekeurig aantal dagen tussen 1 en 2147483647. Als bewaarbeleid zijn ingesteld, maar Logboeken opslaan in een opslag-Account is uitgeschakeld (bijvoorbeeld als er slechts gebeurtenis Hubs of OMS opties zijn geselecteerd), wordt in het bewaarbeleid geen gevolgen hebben.

Deze instellingen kunnen worden geconfigureerd via de optie 'Exporteren' in het logboek blad in de portal. Ze kunnen ook worden geconfigureerd via een programma van [de Azure Monitor REST API gebruiken](https://msdn.microsoft.com/library/azure/dn931927.aspx), PowerShell-cmdlets of CLI. Een abonnement kan slechts één log profiel hebben.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Log profielen met behulp van de Azure portal configureren
U kunt het gebeurtenissenlogboek op een gebeurtenis Hub streamen of op te slaan in een opslag-Account met behulp van de optie 'Exporteren' in de portal van Azure.

1. Ga naar het **Gebeurtenissenlogboek** blad gebruik het menu aan de linkerkant van de portal.

    ![Navigeer naar de activiteit Log in de portal](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klik op de knop **exporteren** aan de bovenkant van het blad.

    ![Knop exporteren in de portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. In het blad dat wordt weergegeven, kunt u het volgende selecteren:  
    - regio's waarvan u wilt exporteren gebeurtenissen
    - het opslag-Account waaraan u wilt opslaan, gebeurtenissen
    - het aantal dagen dat u wilt bewaren van deze gebeurtenissen in opslag. Een instelling van 0 dagen behoudt de logboeken eeuwen.
    - de Service Bus Namespace waarin u een gebeurtenis Hub wilt voor streaming deze gebeurtenissen worden gemaakt.

    ![Activiteit Log blade exporteren](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klik op **Opslaan** als deze instellingen wilt opslaan. De instellingen worden direct worden toegepast op uw abonnement.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Gebruik van de Azure PowerShell-Cmdlets log-profielen configureren
#### <a name="get-existing-log-profile"></a>Bestaande log profiel ophalen
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Een profiel log toevoegen
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Eigenschap         | Vereist | Beschrijving   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Naam             | Ja      | De naam van uw profiel log.                                 |
| StorageAccountId | Nee       | Resource-ID van het opslag-Account waaraan het gebeurtenissenlogboek moeten worden opgeslagen.                         |
| serviceBusRuleId | Nee       | Service Bus regel-ID voor de Service Bus naamruimte die u hebben gebeurtenis hubs wilt in hebt gemaakt. Een tekenreeks met deze indeling: `{service bus resource ID}/authorizationrules/{key name}`. |
| Locaties        | Ja      | Door komma's gescheiden lijst met regio's waarvan u wilt verzamelen activiteit gebeurtenissen.              |
| RetentionInDays  | Ja      | Het aantal dagen voor welke gebeurtenissen moeten worden bewaard, tussen 1 en 2147483647. De waarde nul worden opgeslagen de logboeken voor onbepaalde tijd (eeuwen). |
| Categorieën       | Nee       | Door komma's gescheiden lijst met gebeurteniscategorieën die moet worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en actie.                                 |

#### <a name="remove-a-log-profile"></a>Een profiel log verwijderen
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cross-platform-cli"></a>Gebruik van de Azure platforms CLI log-profielen configureren
#### <a name="get-existing-log-profile"></a>Bestaande log profiel ophalen
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
De `name` eigenschap moet de naam van uw profiel log.

#### <a name="add-a-log-profile"></a>Een profiel log toevoegen
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Eigenschap         | Vereist | Beschrijving   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| naam             | Ja      | De naam van uw profiel log.                                 |
| storageId        | Nee       | Resource-ID van het opslag-Account waaraan het gebeurtenissenlogboek moeten worden opgeslagen.                         |
| serviceBusRuleId | Nee       | Service Bus regel-ID voor de Service Bus naamruimte die u hebben gebeurtenis hubs wilt in hebt gemaakt. Een tekenreeks met deze indeling: `{service bus resource ID}/authorizationrules/{key name}`. |
| locaties        | Ja      | Door komma's gescheiden lijst met regio's waarvan u wilt verzamelen activiteit gebeurtenissen.              |
| retentionInDays  | Ja      | Het aantal dagen voor welke gebeurtenissen moeten worden bewaard, tussen 1 en 2147483647. De waarde nul worden opgeslagen de logboeken voor onbepaalde tijd (eeuwen).     |
| categorieën       | Nee       | Door komma's gescheiden lijst met gebeurteniscategorieën die moet worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en actie.                                 |

#### <a name="remove-a-log-profile"></a>Een profiel log verwijderen
```
azure insights logprofile delete --name my_log_profile
```

## <a name="event-schema"></a>Schema van de gebeurtenis
Elke gebeurtenis in het gebeurtenissenlogboek heeft een JSON blob die vergelijkbaar is met het volgende voorbeeld:

```
{
  "value": [ {
    "authorization": {
      "action": "microsoft.support/supporttickets/write",
      "role": "Subscription Admin",
      "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
    },
    "caller": "admin@contoso.com",
    "channels": "Operation",
    "claims": {
      "aud": "https://management.core.windows.net/",
      "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
      "iat": "1421876371",
      "nbf": "1421876371",
      "exp": "1421880271",
      "ver": "1.0",
      "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
      "puid": "20030000801A118C",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
      "name": "John Smith",
      "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
      "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
      "appidacr": "2",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.microsoft.com/claims/authnclassreference": "1"
    },
    "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "description": "",
    "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
    "eventName": {
      "value": "EndRequest",
      "localizedValue": "End request"
    },
    "eventSource": {
      "value": "Microsoft.Resources",
      "localizedValue": "Microsoft Resources"
    },
    "httpRequest": {
      "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
      "clientIpAddress": "192.168.35.115",
      "method": "PUT"
    },
    "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
    "level": "Informational",
    "resourceGroupName": "MSSupportGroup",
    "resourceProviderName": {
      "value": "microsoft.support",
      "localizedValue": "microsoft.support"
    },
    "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
    "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "operationName": {
      "value": "microsoft.support/supporttickets/write",
      "localizedValue": "microsoft.support/supporttickets/write"
    },
    "properties": {
      "statusCode": "Created"
    },
    "status": {
      "value": "Succeeded",
      "localizedValue": "Succeeded"
    },
    "subStatus": {
      "value": "Created",
      "localizedValue": "Created (HTTP Status Code: 201)"
    },
    "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
    "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
    "subscriptionId": "s1"
  } ],
"nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```

| De elementnaam van het         | Beschrijving             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| autorisatie        | BLOB van RBAC eigenschappen van de gebeurtenis. Meestal bevat de eigenschappen "actie", "rol" en "bereik".|
| beller               | E-mailadres van de gebruiker van wie de bewerking, UPN-claim of name claim op basis van beschikbaarheid heeft uitgevoerd.|
| kanalen             | Een van de volgende waarden: 'Beheerder', 'Bewerking'|
| correlationId        | Meestal een GUID in de indeling van de tekenreeks. Gebeurtenissen die delen van een correlationId deel uitmaakt van dezelfde uber actie.   |
| Beschrijving          | De beschrijving van de statische tekst van een gebeurtenis.                              |
| eventDataId          | Unieke id van een gebeurtenis.    |
| eventSource          | De naam van de Azure-service of de infrastructuur dat is gegenereerd door deze gebeurtenis.    |
| httpRequest          | BLOB met een beschrijving van de HTTP-aanvraag. Bevat meestal de "clientRequestId", "clientIpAddress" en "methode" (HTTP-methode. For example, plaatsen).                            |
| niveau                | Niveau van de gebeurtenis. Een van de volgende waarden: 'kritieke","Fout","Waarschuwing", 'Ter informatie' en"Uitgebreid"  |
| resourceGroupName    | De naam van de resourcegroep voor de resource risico.               |
| resourceProviderName | Naam van de resource-provider voor de resource risico             |
| resourceUri          | Resource-id van de risico resource.                               |
| Bewerkingsnummer          | Een GUID die wordt gedeeld door de gebeurtenissen die met één bewerking overeenkomen.         |
| operationName        | De naam van de bewerking.  |
| Eigenschappen           | Set `<Key, Value>` paren (dat wil zeggen een woordenlijst) met een beschrijving van de details van de gebeurtenis.                                |
| status               | Tekenreeks die de status van de bewerking. Sommige algemene waarden zijn: gestart, In de voortgang, geslaagd, mislukt, actief, opgelost.                                |
| subStatus            | Meestal de HTTP-statuscode van de corresponderende REST te bellen, maar kunnen ook andere tekenreeksen met een beschrijving van een substatus, zoals deze algemene waarden bevatten: OK (HTTP-statuscode: 200), gemaakt (HTTP-statuscode: 201), geaccepteerde (HTTP-statuscode: 202), niet inhoud (HTTP-statuscode: 204), ongeldige aanvragen (HTTP-statuscode: 400), niet gevonden (HTTP-statuscode: 404), Conflict (HTTP-statuscode : 409), interne serverfout (HTTP-statuscode: 500), Service niet beschikbaar is (HTTP-statuscode: 503), time-out van de Gateway (HTTP-statuscode: 504). |
| eventTimestamp       | Tijdstip waarop de gebeurtenis is gegenereerd door het verwerken van de aanvraag overeenkomt van de gebeurtenis Azure-service.     |
| submissionTimestamp  | Tijdstip waarop de gebeurtenis kwam beschikbaar voor query's uitvoeren.             |
| subscriptionId       | Azure abonnement-id.  |
| nextLink             | Voortgezet token voor het ophalen van de volgende set resultaten wanneer ze zijn opgedeeld in meerdere antwoorden. Meestal nodig wanneer er meer dan 200 records.     |

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over het gebeurtenissenlogboek (voorheen controlelogboeken)](../resource-group-audit.md)
- [Het activiteitenlogboek Azure naar gebeurtenis Hubs streamen](./monitoring-stream-activity-logs-event-hubs.md)
