<properties
    pageTitle="Webhooks configureren op Azure metrische waarschuwingen | Microsoft Azure"
    description="Azure waarschuwingen met andere niet-Azure-systemen omleiden."
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
    ms.date="09/15/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Een webhook configureren op een Azure metrische waarschuwing

Webhooks kunt u een Azure waarschuwingsbericht doorsturen naar andere systemen voor na verwerking of aangepaste acties. U kunt een webhook over een waarschuwing voor het routeren deze naar de services die SMS-bericht sturen, meld u bugs bij te werken, hoogte van een team via chat/messaging services of voer een willekeurig aantal andere acties. Dit artikel wordt beschreven hoe u een webhook instellen op een Azure metrische waarschuwing en hoe de nettolading voor het HTTP-bericht naar een webhook eruitziet. Voor meer informatie over de installatie en het schema voor een logboek Azure Waarschuw (waarschuwing over alle gebeurtenissen), [Zie deze pagina in plaats daarvan](./insights-auditlog-to-webhook-email.md).

Azure waarschuwingen HTTP POST de inhoud van de waarschuwing in JSON opmaken, schema hieronder, naar een webhook URI die u hebt opgegeven bij het maken van de melding. Deze URI moet een geldige HTTP of HTTPS-eindpunt. Azure berichten één item per aanvraag wanneer een waarschuwing wordt geactiveerd.

## <a name="configuring-webhooks-via-the-portal"></a>Webhooks via de portal configureren

U kunt toevoegen of bijwerken van de webhook URI in het scherm waarschuwingen maken/bijwerken in de [portal](https://portal.azure.com/).

![Een waarschuwing regel toevoegen](./media/insights-webhooks-alerts/Alertwebhook.png)

U kunt ook een melding naar posten naar een webhook URI met de [PowerShell-Cmdlets Azure](./insights-powershell-samples.md#create-alert-rules), [Platforms CLI](./insights-cli-samples.md#work-with-alerts)of [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx)configureren.

## <a name="authenticating-the-webhook"></a>De webhook verifiëren

De webhook kan verifiëren gebruikmaakt van een van de volgende manieren:

1. **Autorisatie voor token-e** - de webhook URI wordt opgeslagen met een token-ID, bijvoorbeeld. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Basisverificatie** - de webhook URI wordt opgeslagen met een gebruikersnaam en wachtwoord, bijvoorbeeld. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Nettolading schema

De POST-bewerking bevat de volgende JSON nettolading en het schema voor alle metrisch gebaseerde waarschuwingen.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| Veld | Verplicht | Vaste Set waarden | Notities |
| :-------------| :-------------   | :-------------   | :-------------   |
|status|Y|"Geactiveerd", "Opgelost"|Status van de waarschuwing die is gebaseerd op de voorwaarden die u hebt ingesteld.|
|context| Y | | De context van de waarschuwing.|
|tijdstempel| Y | | De tijd waarop de waarschuwing is geactiveerd.|
|ID | Y | | Elke waarschuwingsregels heeft een unieke id.|
|naam               |Y                  |                   | De naam van de waarschuwing.|
|Beschrijving        |Y                  |                           |Beschrijving van de waarschuwing.|
|conditionType      |Y                  |"Metrische", "Gebeurtenis"          |Twee soorten waarschuwingen worden ondersteund. Een op basis van een voorwaarde metrische en de andere op basis van een gebeurtenis in het gebeurtenissenlogboek. Gebruik deze waarde om te controleren als de melding is gebaseerd op metrisch of gebeurtenis.|
|voorwaarde          |Y                  |                           | De specifieke velden die u wilt controleren op basis van de conditionType.|
|metricName         |voor metrische waarschuwingen  |                           |De naam van de meetwaarde die definieert wat de regel bewaakt.|
|metricUnit         |voor metrische waarschuwingen  |"Bytes", "BytesPerSecond", "Aantal", "CountPerSecond", "Percent", "Seconden"|     De indeling van de eenheid in de meetwaarde toegestaan. [Toegestane waarden hier staan vermeld](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx).|
|metricValue        |voor metrische waarschuwingen  |                           |De werkelijke waarde van de meetwaarde waardoor de melding.|
|drempelwaarde          |voor metrische waarschuwingen  |                           |De drempelwaarde waarbij de melding wordt geactiveerd.|
|Venstergrootte         |voor metrische waarschuwingen  |                           |De periode die wordt gebruikt om de waarschuwing activiteit op basis van de drempelwaarde te houden. Moet liggen tussen 5 minuten en 1 dag. ISO-8601 duur-notatie.|
|TimeAggregation van    |voor metrische waarschuwingen  |"Gemiddelde', 'Achternaam',"Maximum","Minimum", 'Geen', 'totale" | Hoe moeten de gegevens die worden verzameld worden gecombineerd na verloop van tijd. De standaardwaarde is gemiddelde. [Toegestane waarden hier staan vermeld](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx).|
|operator           |voor metrische waarschuwingen  |                           |De operator waarmee de huidige metrische gegevens aan de drempelwaarde set vergelijken.|
|subscriptionId     |Y                  |                           |Azure abonnement-ID.|
|resourceGroupName  |Y                  |                           |De naam van de resourcegroep voor de resource risico.|
|Bronnaam       |Y                  |                           |De naam van de resource van de risico resource.|
|Brontype       |Y                  |                           |Type resource van de risico resource.|
|resourceId         |Y                  |                           |Resource-ID van de risico resource.|
|resourceRegion     |Y                  |                           |Regio of locatie van het risico resource.|
|portalLink         |Y                  |                           |Directe koppeling naar de overzichtspagina portal resource.|
|Eigenschappen         |N                  |Optionele                   |Set `<Key, Value>` paren (dat wil zeggen `Dictionary<String, String>`) die bevat informatie over de gebeurtenis. Het veld eigenschappen is optioneel. Gebruikers kunnen sleutel/waarden die kan worden doorgegeven via de nettolading invoeren in een aangepaste UI of -logica op basis van een app-werkstroom. De andere methode voor het doorgeven van aangepaste eigenschappen terug naar de webhook is via de webhook uri zelf (als queryparameters)|


>[AZURE.NOTE] Het veld eigenschappen kan alleen worden ingesteld met de [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over Azure waarschuwingen en webhooks in de video [Azure waarschuwingen integreren met PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
- [Azure automatisering scripts (Runbooks) op Azure waarschuwingen uitvoeren](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Logica-App gebruiken om te verzenden van een SMS-bericht via Twilio vanaf een Azure waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
- [Gebruik logica App om een vertraging bericht verzenden namens een Azure waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
- [Logica App een bericht verzenden naar een Azure-wachtrij van een melding in Azure gebruiken](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
