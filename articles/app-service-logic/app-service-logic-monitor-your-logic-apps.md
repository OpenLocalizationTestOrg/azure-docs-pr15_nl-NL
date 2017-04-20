<properties 
    pageTitle="Uw apps logica in Azure App-Service controleren | Microsoft Azure" 
    description="Hoe u kunt zien wat de logica-apps hebt gedaan" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="monitor-your-logic-apps"></a>Uw apps logica controleren

Nadat u [een app logica maakt](app-service-logic-create-a-logic-app.md), kunt u de volledige geschiedenis van de uitvoering in de portal van Azure zien.  U kunt ook services zoals Azure diagnostisch hulpprogramma en Azure waarschuwingen instellen om de gebeurtenissen realtime te houden en meldingen voor gebeurtenissen wens "wanneer meer dan 5 uitgevoerd mislukt binnen een uur."

## <a name="monitor-in-the-azure-portal"></a>Monitor in de Portal van Azure

De geschiedenis weergeven, selecteert u **Bladeren**en selecteer **Logica Apps**. Een lijst met alle logica apps in uw abonnement wordt weergegeven.  Selecteer de logica-app die u wilt controleren.  U ziet een lijst met alle acties en triggers die hebben plaatsgevonden voor deze app logica.

![Overzicht](./media/app-service-logic-monitor-your-logic-apps/overview.png)

Er zijn een paar gedeelten over deze blade die handig zijn:

- **Overzicht van** lijsten **al wordt uitgevoerd** en de **Inwerkingtreding geschiedenis**
    - **Alle wordt uitgevoerd** lijst de meest recente logica-app wordt uitgevoerd.  U kunt op elke rij voor meer informatie over het uitvoeren of klik op de tegel voor een overzicht van meer wordt uitgevoerd.
    - **Trigger geschiedenis** toont alle trigger-activiteiten voor deze app logica.  Trigger activiteit kan worden van een 'Overgeslagen' controleren op nieuwe gegevens (bijvoorbeeld projectinformatie om te zien als een nieuw bestand is toegevoegd aan FTP), "is geslaagd' waarbij de gegevens in een app logica fire is opgehaald of"Mislukt"een fout in de configuratie overeenkomt.
- **Diagnostische hulpprogramma's** kunt u runtime details en gebeurtenissen weergeven en u hierop abonneren [Azure waarschuwingen](#adding-azure-alerts)

>[AZURE.NOTE] Alle runtime details en gebeurtenissen worden versleuteld in rust binnen de logica App-service. Deze worden alleen ontsleuteld desgevraagd een weergave van een gebruiker. Toegang tot deze gebeurtenissen kan ook worden beheerd door Azure Role-Based Access besturingselement RBAC ().

### <a name="view-the-run-details"></a>De uitvoeren details bekijken

Deze lijst met uitgevoerd ziet u de **Status**, de **Begintijd**en de **duur** van de betreffende uitvoeren. Selecteer een rij details kunnen zien op die worden uitgevoerd.

De controle weergave ziet u elke stap van de uitvoeren, de invoer en uitvoer en foutberichten die occurre wellicht.

![Uitvoeren en acties](./media/app-service-logic-monitor-your-logic-apps/monitor-view.png)

Als u aanvullende details zoals uitvoeren van de **Correlatie-ID** (die kunnen worden gebruikt voor de REST API) nodig hebt, kunt u de knop **Details uitvoeren** .  Deze groep omvat alle stappen, status en invoer/uitvoer voor het uitvoeren.

## <a name="azure-diagnostics-and-alerts"></a>Azure diagnostisch hulpprogramma en waarschuwingen

Naast de informatie van de Azure-Portal en de REST API bovenstaande, kunt u uw app logica voor het gebruik van Azure diagnostische hulpprogramma's voor meer uitgebreide informatie en voor foutopsporing in configureren.

1. Klik op de sectie **diagnostische hulpprogramma's** van het blad logica-app
1. Klik op de **Diagnostische instellingen** configureren
1. Een gebeurtenis Hub of opslag-Account als u wilt uitzenden van gegevens configureren

    ![Instellingen van Azure diagnostische gegevens](./media/app-service-logic-monitor-your-logic-apps/diagnostics.png)

### <a name="adding-azure-alerts"></a>Azure waarschuwingen toevoegen

Nadat diagnostische hulpprogramma's zijn geconfigureerd, kunt u Azure waarschuwingen gestart wanneer bepaalde drempels zijn Gekruiste kunt toevoegen.  Selecteer de tegel van **waarschuwingen** en **melding toevoegen**in het blad **Diagnostische gegevens** .  Hiermee wordt u begeleid configureren van een melding op basis van een aantal drempelwaarden en aan de doelstellingen.

![Azure waarschuwing doelstellingen](./media/app-service-logic-monitor-your-logic-apps/alerts.png)

U kunt de **voorwaarde**, **drempel**en **periode** configureren desgewenst.  U kunt tot slot configureren van een e-mailadres als u wilt een melding voor het verzenden of een webhook configureren.  U kunt de [aanvraag trigger](../connectors/connectors-native-reqres.md) in een app logica uitvoeren op een waarschuwing als u ook (acties uit te voeren zoals [posten naar marge](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app), [verzenden van een tekst](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)of [een bericht naar een wachtrij toevoegen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)).

### <a name="azure-diagnostics-settings"></a>Instellingen Azure diagnostische gegevens

Elk van deze gebeurtenissen bevat informatie over de logica-app en de gebeurtenis zoals status.  Hier volgt een voorbeeld van een gebeurtenis *ActionCompleted* :

```javascript
{
            "time": "2016-07-09T17:09:54.4773148Z",
            "workflowId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
            "resourceId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-06-01",
                "startTime": "2016-07-09T17:09:53.4336305Z",
                "endTime": "2016-07-09T17:09:53.5430281Z",
                "status": "Succeeded",
                "code": "OK",
                "resource": {
                    "subscriptionId": "80d4fe69-ABCD-EFGH-a938-9250f1c8ab03",
                    "resourceGroupName": "MyResourceGroup",
                    "workflowId": "cff00d5458f944d5a766f2f9ad142553",
                    "workflowName": "MyLogicApp",
                    "runId": "08587361146922712057",
                    "location": "eastus",
                    "actionName": "Http"
                },
                "correlation": {
                    "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
                    "clientTrackingId": "my-custom-tracking-id"
                },
                "trackedProperties": {
                    "myProperty": "<value>"
                }
            }
        }
```

De twee eigenschappen die met name handig zijn voor het bijhouden en controle zijn *clientTrackingId* en *trackedProperties*.  

#### <a name="client-tracking-id"></a>Client-ID is bijhouden

De client voor het bijhouden van ID is een waarde die gebeurtenissen over een logica-app wordt uitgevoerd relateren wordt, inclusief geneste werkstromen als onderdeel van een app logica genoemd.  Deze-ID wordt worden automatisch gegenereerd als dit niet wordt opgegeven, maar u kunt handmatig de ID van een trigger bijhouden door gewoon opgeven een `x-ms-client-tracking-id` koptekst met de waarde-ID in het verzoek trigger (verzoek trigger, HTTP-trigger of webhook trigger).

#### <a name="tracked-properties"></a>Bijgehouden eigenschappen

Bijgehouden eigenschappen kunnen worden toegevoegd op acties in de werkstroomdefinitie voor het bijhouden van de invoer of uitvoer in diagnostische gegevens.  Dit kan het handig als u wilt bijhouden van gegevens, zoals een 'order-ID"in het telemetrielogboek zijn.  Als u een bijgehouden eigenschap toevoegt, moet de `trackedProperties` eigenschap van een actie.  Bijgehouden eigenschappen kunnen alleen bijhouden een invoeritems één acties en uitvoer, maar u kunt de `correlation` eigenschappen van de gebeurtenissen om te relateren over acties in een uitvoeren.

```javascript
{
    "myAction": {
        "type": "http",
        "inputs": {
            "uri": "http://uri",
            "headers": {
                "Content-Type": "application/json"
            },
            "body": "@triggerBody()"
        },
        "trackedProperties":{
            "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
            "myActionHTTPValue": "@action()['outputs']['body']['foo']",
            "transactionId": "@action()['inputs']['body']['bar']"
        }
    }
}
```

### <a name="extending-your-solutions"></a>Uw oplossingen uitbreiden

U kunt gebruikmaken van deze telemetrielogboek vanuit de Hub van de gebeurtenis of opslag in andere services, zoals [Bewerkingen Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite), [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)en [Power BI](https://powerbi.com) heeft realtime cmdlets voor controle van uw werkstromen integratie.

## <a name="next-steps"></a>Volgende stappen
- [Algemene voorbeelden en scenario's voor logica-apps](app-service-logic-examples-and-scenarios.md)
- [Maken van een sjabloon logica App-implementatie](app-service-logic-create-deploy-template.md)
- [Onderdelen van de Enterprise-integratie](app-service-logic-enterprise-integration-overview.md)
