<properties
   pageTitle="Afhandeling van logica Apps uitzonderingen | Microsoft Azure"
   description="Informatie over patronen voor fout en uitzondering verwerken met Azure logica Apps"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-error-and-exception-handling"></a>Logische Apps fout en afhandeling van uitzonderingen

Logica Apps biedt een uitgebreide reeks hulpmiddelen en patronen om te zorgen dat uw integraties zijn robuuste en robuuste tegen fouten.  Een van de uitdagingen met een integratiearchitectuur is ervoor zorgen dat downtime of problemen van afhankelijke systemen correct worden afgehandeld.  Logica Apps zorgt ervoor dat foutafhandeling een eerste-klas ervaring, zodat u de hulpmiddelen die u moet reageren op uitzonderingen en fouten binnen uw werkstromen.

## <a name="retry-policies"></a>Beleid voor het opnieuw

Het belangrijkste type uitzondering en foutafhandeling is een beleid voor het opnieuw.  Dit beleid wordt gedefinieerd als de actie moet opnieuw als eerste aanvraag verlopen is of is mislukt (elk verzoek waardoor er een 429 5xx antwoord of).  Standaard opnieuw alle acties 4 extra keer via 20 seconden intervallen.  Dat het geval is als het eerste verzoek ontvangen een `500 Internal Server Error` antwoord, de werkstroom-engine onderbreekt voor 20 seconden en poging het verzoek opnieuw.  Als nadat alle pogingen het antwoord nog steeds een uitzondering of mislukt is, de werkstroom wordt gaan en de status van de actie als markeren `Failed`.

U kunt beleid voor het opnieuw configureren in de **invoer** van een bepaalde actie.  Een nieuwe poging-beleid kan worden geconfigureerd om te proberen dan 4 keer meer dan 1 uur intervallen.  Volledige informatie over de eigenschappen voor de invoer kunnen worden [gevonden op MSDN][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Als u de actie HTTP 4 keer opnieuw en wacht 10 minuten tussen elke poging hebt u de volgende definitie:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Voor meer informatie over de syntaxis van de ondersteunde, raadpleegt u de [sectie opnieuw-beleid op MSDN][retryPolicyMSDN].

## <a name="runafter-property-to-catch-failures"></a>De eigenschap RunAfter onderschept fouten

Elke logica app actie gedeclareerd welke acties moeten voltooien voordat u de actie wordt gestart.  U kunt zien dit als u de volgorde van stappen in de werkstroom.  Deze volgorde heet het `runAfter` eigenschap in de definitie actie.  Dit is een object waarmee wordt beschreven welke acties en actie statussen zou de actie uitvoeren.  Alle acties toegevoegd via de ontwerpfunctie zijn standaard ingesteld op `runAfter` de vorige stap als de vorige stap is `Succeeded`.  U kunt deze waarde om acties uitgevoerd als eerdere bewerkingen zijn echter aanpassen `Failed`, `Skipped`, of een mogelijke reeks deze waarden.  Als u wilt toevoegen van een item aan een aangewezen Service Bus onderwerp na een bepaalde actie `Insert_Row` mislukt, gebruikt u de volgende `runAfter` configuratie:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Kennisgeving de `runAfter` eigenschap is ingesteld op gestart als de `Insert_Row` actie moet worden `Failed`.  U kunt de actie uitvoeren als de actiestatus `Succeeded`, `Failed`, of `Skipped` de syntaxis van de zou:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

>[AZURE.TIP] Acties die worden uitgevoerd nadat de actie van een voorgaande is mislukt en voltooid, wordt gemarkeerd als `Succeeded`.  Dit probleem betekent dat als u succes variabel alle fouten in een werkstroom voor het uitvoeren zelf is gemarkeerd als `Succeeded`.

## <a name="scopes-and-results-to-evaluate-actions"></a>Bereiken en resultaten acties wordt berekend

Vergelijkbaar met hoe u kan worden uitgevoerd nadat afzonderlijke acties kunt u ook acties in de groep samen binnen een [bereik](app-service-logic-loops-and-scopes.md) - die als een logische groepering van acties fungeert.  Bereiken zijn handig voor het ordenen van uw acties van de app logica, zowel voor het uitvoeren van statistische evaluaties op de status van een bereik.  Het bereik zelf ontvangt een status nadat alle acties binnen een bereik hebt voltooid.  De status van de reikwijdte wordt bepaald met dezelfde criteria als een uitvoeren--als de laatste actie in een tak execution `Failed` of `Aborted` de status ervan is `Failed`.

U kunt `runAfter` een bereik is gemarkeerd `Failed` gestart bepaalde acties voor eventuele fouten die zijn aangebracht in het bereik.  Uitgevoerd nadat een bereik is mislukt, kunt u één enkele actie om fouten als *bewerkingen in het bereik* mislukt maken.

### <a name="getting-the-context-of-failures-with-results"></a>De context van fouten met zoekresultaten ophalen

Fouten in een bereik vangen kan bijzonder nuttig zijn, maar u kunt ook de context voor meer informatie over precies welke acties is mislukt, en eventuele fouten of de statuscodes die zijn geretourneerd.  De `@result()` werkstroom functie biedt context in het resultaat van alle acties binnen een bereik.

`@result()`een enkele parameter, de naam van de reikwijdte wordt ingevoegd en geeft als resultaat een matrix met alle actie resultaten uit in dat bereik.  Deze actie objecten opnemen dezelfde kenmerken als de `@actions()` object, met inbegrip van actie begintijd, de eindtijd actie Actiestatus, actie invoeritems, actie correlatie-id's en actie uitvoer.  U kunt eenvoudig koppelen een `@result()` correct met een `runAfter` naar het verzenden van de context van de acties die is mislukt binnen een bereik.

Als u wilt een actie *voor elke* actie uitvoeren in een bereik die `Failed`, u kunt koppelen `@result()` met de actie van een **[Matrix Filter](../connectors/connectors-native-query.md)** en een lus **[ForEach](app-service-logic-loops-and-scopes.md)** .  Hiermee kunt u voor het filteren van de matrix van resultaten op acties die is mislukt.  U kunt de gefilterde resultaat-matrix en een actie uitvoeren voor elke fout bij het gebruik van de lus **ForEach** .  Hier volgt een voorbeeld hieronder, gevolgd door een gedetailleerde uitleg.  In dit voorbeeld stuurt een HTTP POST-aanvraag met de hoofdtekst van het antwoord van de acties die is mislukt binnen het bereik `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Hier volgt een gedetailleerd overzicht van wat er gebeurt:

1. **Filter matrix** actie voor het filteren van de `@result('My_Scope')` om het resultaat van alle acties binnen`My_Scope`
1. Voorwaarde in de **Matrix Filter** is alle `@result()` item met de status is gelijk aan `Failed`.  Hiermee wordt de matrix van alle actie resultaten van filteren `My_Scope` naar alleen een matrix van resultaten mislukte actie.
1. Een actie **voor elk** uitvoeren op de uitvoer van de **Matrix gefilterd** .  Hiermee wordt een actie *voor elk* is mislukt actie resultaat we hierboven hebt gefilterd uitvoeren.
    - Als er een enkele actie in het bereik dat is mislukt is, de acties in de `foreach` slechts één keer wilt uitvoeren.  Groot aantal mislukte acties zou leiden tot één actie per is mislukt.
1. Een HTTP-bericht verzenden op de `foreach` antwoord hoofdtekst, item of `@item()['outputs']['body']`.  De `@result()` item vorm is hetzelfde als de `@actions()` structureren en kan op dezelfde manier worden geparseerd.
1. Twee aangepaste headers met de naam van de mislukte actie ook opgenomen `@item()['name']` en de mislukte uitvoeren client bijhouden ID `@item()['clientTrackingId']`.

Voor een verwijzing naar Hier volgt een voorbeeld van een enkel `@result()` item.  U kunt zien de `name`, `body`, en `clientTrackingId` eigenschappen geparseerde in het bovenstaande voorbeeld.  Deze moet worden vermeld die buiten een `foreach`, `@result()` geeft als resultaat een matrix van deze objecten.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/foo/bar does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

U kunt de bovenstaande expressies kunt gebruiken om uit te voeren verschillende patronen de verwerking van uitzonderingen.  U kunt uitvoeren van een enkele uitzondering afhandelen actie buiten het bereik waarin u de hele gefilterde matrix van fouten en verwijder de `foreach`.  U kunt ook andere nuttige eigenschappen van de `@result()` antwoord hierboven.

## <a name="azure-diagnostics-and-telemetry"></a>Azure diagnostisch hulpprogramma en telemetrielogboek

De bovenstaande patronen uitstekende manier om te verwerken van fouten en uitzonderingen binnen een klaar zijn, maar u kunt ook identificeren en reageren op fouten onafhankelijk van het zelf uitvoeren.  [Diagnostisch hulpprogramma Azure](app-service-logic-monitor-your-logic-apps.md) biedt een eenvoudige manier om alle werkstroomgebeurtenissen (inclusief alle uitvoeren en actie status) verzenden naar een Azure Storage-account of een Hub Azure-gebeurtenis.  U kunt de logboeken en aan de doelstellingen controleren of ze te publiceren in een programma voor bewaking die u liever, uitvoeren statussen die wordt berekend.  Eén mogelijke optie is om te streamen alle gebeurtenissen via Azure gebeurtenis Hub naar [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).  U kunt in Stream Analytics live query's mensen uit een afwijkingen, gemiddelden of fouten schrijven uit de diagnostische logboeken.  Stream Analytics kunt eenvoudig naar andere gegevensbronnen, zoals wachtrijen, onderwerpen, SQL, DocumentDB en Power BI uitvoeren.

## <a name="next-steps"></a>Volgende stappen
- [Zie hoe één customer ingebouwd robuuste foutverwerking met logica Apps](app-service-logic-scenario-error-and-exception-handling.md)
- [Meer voorbeelden van de logica Apps en scenario's zoeken](app-service-logic-examples-and-scenarios.md)
- [Informatie over het maken van geautomatiseerde implementaties van logica-apps](app-service-logic-create-deploy-template.md)
- [Ontwerpen en implementeren van logica apps van Visual Studio](app-service-logic-deploy-from-vs.md)


<!-- References -->
[retryPolicyMSDN]: https://msdn.microsoft.com/library/azure/mt643939.aspx#Anchor_9