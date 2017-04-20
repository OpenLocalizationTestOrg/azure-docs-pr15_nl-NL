<properties
   pageTitle="Log Analytics waarschuwing REST API"
   description="Het logboek Analytics waarschuwing REST API kunt u waarschuwingen in bewerkingen Management Suite Kantoorbeheersysteem maken en beheren.  In dit artikel bevat informatie van verschillende voorbeelden en de API voor de uitvoering van verschillende bewerkingen."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="log-analytics-alert-rest-api"></a>Log Analytics Waarschuw REST API

Het logboek Analytics waarschuwing REST API kunt u waarschuwingen in bewerkingen Management Suite Kantoorbeheersysteem maken en beheren.  In dit artikel bevat informatie van verschillende voorbeelden en de API voor de uitvoering van verschillende bewerkingen.

Het logboek Analytics zoeken REST API RESTful is en kunnen worden geraadpleegd via het Azure resourcemanager REST API. In dit document vindt u voorbeelden waarin de API toegankelijk is vanuit een PowerShell-opdrachtregel [ARMClient](https://github.com/projectkudu/ARMClient), een bron openen opdrachtregel hulpmiddel waardoor het eenvoudiger wordt aanroepen van de Azure resourcemanager-API gebruiken. Het gebruik van ARMClient en PowerShell is een van de vele opties voor toegang tot de API Log Analytics zoeken. U kunt de RESTful Azure resourcemanager API om te bellen OMS werkruimten en uitvoeren van de opdracht zoekopdracht daarin gebruiken met deze hulpmiddelen. De API uitvoer zoekresultaten voor u in de indeling van JSON, zodat u kunt de lijst met zoekresultaten via een programma op veel verschillende manieren gebruiken.

## <a name="prerequisites"></a>Vereisten voor
Waarschuwingen kunnen op dit moment alleen worden gemaakt met een opgeslagen zoekactie in Log Analytics.  U kunt verwijzen naar de [Log zoeken REST API](log-analytics-log-search-api.md) voor meer informatie.

## <a name="schedules"></a>Schema 's
Een opgeslagen zoekactie kan een of meer schema's bevatten. De planning wordt gedefinieerd hoe vaak de tijdens het zoeken is uitvoeren en het tijdsinterval aanduidt waarop de criteria wordt aangegeven.
Schema's hebben de eigenschappen in de volgende tabel.

| Eigenschap  | Beschrijving |
|:--|:--|
| Interval | Hoe vaak de zoekopdracht wordt uitgevoerd. Gemeten in minuten. |
| QueryTimeSpan | Het tijdsinterval waarvoor de criteria wordt geëvalueerd. Moet gelijk is aan of groter is dan Interval. Gemeten in minuten. |
| Versie | De API-versie die wordt gebruikt.  Op dit moment moet dit altijd worden ingesteld op 1. |

Stel een gebeurtenisquery met een Interval van 15 minuten en een tijdspanne van 30 minuten. In dit geval zou de query wordt uitgevoerd elke 15 minuten en een waarschuwing zou worden geactiveerd als de criteria bleef omgezet in waar over een reeks 30 minuten.

### <a name="retrieving-schedules"></a>Planningen ophalen
De Get-methode gebruiken om op te halen, alle planningen voor een opgeslagen zoekactie.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Gebruik de methode Get met een planning-ID om op te halen van een bepaald schema voor een opgeslagen zoekactie.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Hier volgt een voorbeeld-antwoord voor een betalingsschema.

    {
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
        "Interval": 15,
        "QueryTimeSpan": 15
    }

### <a name="creating-a-schedule"></a>Een planning maken
Gebruik de methode opslag met een unieke planning-ID naar een nieuwe planning maken.  Houd er rekening mee dat twee planningen dezelfde-ID kunnen geen bevatten, zelfs als ze gekoppeld aan andere zijn opgeslagen zoekopdrachten.  Wanneer u een schema in de OMS-console maakt, wordt een GUID gemaakt voor de planning-ID.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Een planning bewerken
Gebruik de methode opslag met een bestaande planning-ID voor dezelfde opgeslagen zoekactie die planning wijzigen.  De hoofdtekst van de aanvraag moet bevatten de etag van de planning.

    $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Schema's verwijderen
Gebruik de methode verwijderen met een planning-ID verwijderen van een planning.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Acties
Een planning kunt meerdere acties hebben. Een actie mogelijk een of meer processen om uit te voeren zoals een e-mailbericht verzenden of starten van een runbook definiëren of deze mogelijk een drempel die bepaalt wanneer de resultaten van een zoekopdracht voldoen aan bepaalde criteria definiëren.  Bepaalde acties worden beide definiëren, zodat de processen worden uitgevoerd wanneer de drempelwaarde is voldaan.

De eigenschappen hebben alle acties in de volgende tabel.  Verschillende soorten waarschuwingen hebben verschillende aanvullende eigenschappen die worden hieronder beschreven.

| Eigenschap | Beschrijving |
|:--|:--|
| Type | Type actie.  De mogelijke waarden zijn momenteel een waarschuwing en Webhook. |
| Naam | De weergavenaam voor de melding. |
| Versie | De API-versie die wordt gebruikt.  Op dit moment moet dit altijd worden ingesteld op 1. |

### <a name="retrieving-actions"></a>Acties laden
De Get-methode gebruiken om op te halen, alle acties voor een betalingsschema.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Gebruik de methode ophalen met de actie-ID om op te halen van een bepaalde actie voor een betalingsschema.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Maken of bewerken van acties
Gebruik de methode opslag met een actie-ID die uniek is voor de planning te maken van een nieuwe actie.  Wanneer u een actie in de OMS-console maakt, een GUID is bedoeld voor de actie-ID.

Gebruik de methode opslag met een bestaande actie-ID voor dezelfde opgeslagen zoekactie die planning wijzigen.  De hoofdtekst van de aanvraag moet bevatten de etag van de planning.

De indeling van het verzoek voor het maken van een nieuwe actie verschilt per actietype zodat deze voorbeelden worden gegeven in de onderstaande secties.

### <a name="deleting-actions"></a>Acties verwijderen
Gebruik de methode verwijderen met de actie-id waarmee een actie verwijderen.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Meldingen
Een planning kan slechts één waarschuwing actie moet hebben.  Meldingen zijn een of meer van de secties in de volgende tabel.  Nader hieronder worden beschreven.

| Sectie | Beschrijving |
|:--|:--|
| Drempelwaarde | Criteria voor wanneer de actie wordt uitgevoerd. |  
| EmailNotification | E-mail verzenden naar meerdere geadresseerden. |
| Remediation | Start een runbook in Azure automatisering geprobeerd om geïdentificeerd probleem te verhelpen. |

#### <a name="thresholds"></a>Drempelwaarden
Een waarschuwing actie mag slechts één drempel hebben.  Wanneer de resultaten van een opgeslagen zoekactie overeenkomt met de drempel in een actie die is gekoppeld aan die zoeken, worden klikt u vervolgens de andere processen in deze actie uitgevoerd.  Een actie kan ook alleen een drempelwaarde bevatten, zodat deze kan worden gebruikt met acties van andere typen die drempelwaarden niet bevatten.

Drempelwaarden hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Operator | De operator voor de drempelwaarde voor vergelijking. <br> BT = groter dan <br> lt = minder dan |
| Waarde | De waarde voor de drempelwaarde. |

Stel een gebeurtenisquery met een Interval van 15 minuten, een tijdspanne van 30 minuten en een drempelwaarde voor groter is dan 10. In dit geval zou de query worden uitgevoerd om de 15 minuten en een waarschuwing zou worden geactiveerd, als deze 10 gebeurtenissen die zijn gemaakt gedurende een bepaalde 30 minuten als resultaat gegeven.

Hier volgt een voorbeeld-antwoord voor een actie aan alleen een drempelwaarde.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Gebruik de methode opslag met een unieke actie-ID te maken van een nieuwe actie drempelwaarde voor een betalingsschema.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Gebruik de methode opslag met een bestaande actie-ID voor het wijzigen van de actie van een drempelwaarde voor een betalingsschema.  De hoofdtekst van de aanvraag moet de etag van de actie bevatten.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>E-mailmelding
E-mail verzenden meldingen per e-mail naar een of meer geadresseerden.  Ze bevatten de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Geadresseerden | Lijst met e-mailadressen. |
| Onderwerp | Het onderwerp van het e-mailbericht. |
| Bijlage | Bijlagen worden momenteel niet ondersteund, zodat dit altijd een waarde van "Geen hebben". |

Hier volgt een voorbeeld-antwoord voor een actie van de melding e-mailbericht met een drempelwaarde.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Gebruik de methode opslag met een unieke actie-ID te maken van een nieuwe e-actie voor een betalingsschema.  Het volgende voorbeeld wordt een e-mailbericht met een drempelwaarde zodat de e-mail wordt verzonden wanneer de resultaten van de opgeslagen zoekactie groter is dan de drempelwaarde voor.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

Gebruik de methode opslag met een bestaande actie-ID voor het wijzigen van een e-actie voor een betalingsschema.  De hoofdtekst van de aanvraag moet de etag van de actie bevatten.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

#### <a name="remediation-actions"></a>Remediation acties
Remediations starten een runbook in Azure automatisering die kan worden opgelost die wordt aangeduid met de melding om het probleem.  U moet een webhook voor het runbook gebruikt in een actie remediation maken en geef vervolgens de URI in de eigenschap WebhookUri.  Wanneer u deze actie in met de OMS-console maakt, wordt automatisch een nieuwe webhook gemaakt voor het runbook.

Remediations bevatten de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| RunbookName | De naam van het runbook. Dit moet overeenkomen met een gepubliceerde runbook in het account automatisering is geconfigureerd in de oplossing automatisering in uw werkruimte OMS. |
| WebhookUri | URI van de webhook.
| Vervaldatum | De vervaldatum en de tijd van de webhook.  Als de webhook geen een verlooptijd, kan dit een ongeldige datum in de toekomst zijn. |

Hier volgt een voorbeeld-antwoord voor een actie remediation met een drempelwaarde.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Gebruik de methode opslag met een unieke actie-ID te maken van een nieuwe remediation actie voor een betalingsschema.  Het volgende voorbeeld wordt een remediation gemaakt met een drempelwaarde, zodat het runbook wordt gestart wanneer de resultaten van de opgeslagen zoekactie groter is dan de drempelwaarde voor.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Gebruik de methode opslag met een bestaande actie-ID voor het wijzigen van een actie remediation voor een betalingsschema.  De hoofdtekst van de aanvraag moet de etag van de actie bevatten.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Voorbeeld
Hier volgt een volledig voorbeeld om te maken van een melding voor een nieuwe e-mail.  Hiermee maakt u een nieuwe planning samen met een actie die drempel en e-mail.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $workspaceId    = "MyWorkspace"
    $searchId       = "51cf0bd9-5c74-6bcb-927e-d1e9080b934e"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule?api-version=2015-03-20 $scheduleJson

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/mythreshold?api-version=2015-03-20 $thresholdJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/myemailaction?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Webhook acties
Een proces starten Webhook acties door te bellen van een URL en desgewenst een nettolading te kunnen verzenden.  Ze zijn vergelijkbaar met Remediation acties, behalve ze zijn bedoeld voor webhooks die processen dan Azure automatisering runbooks kan worden gestart.  Ze bieden ook de extra optie aan te bieden een nettolading is bezorgd op de externe proces.

Webhook acties geen een drempelwaarde, maar in plaats daarvan moeten worden toegevoegd aan een planning met een waarschuwing actie aan een drempelwaarde.  U kunt meerdere Webhook acties die al worden uitgevoerd wanneer de drempelwaarde is voldaan toevoegen.

Webhook acties bevatten de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| WebhookUri | Het onderwerp van het e-mailbericht. |
| CustomPayload | Aangepaste nettolading worden verzonden naar de webhook.  De indeling afhankelijk van wat het webhook verwacht. |

Hier volgt een voorbeeld-antwoord voor webhook actie en een bijbehorende waarschuwing aan een drempelwaarde.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Maak of bewerk een actie webhook
Gebruik de methode opslag met een unieke actie-ID te maken van een nieuwe webhook actie voor een betalingsschema.  Het volgende voorbeeld wordt een actie Webhook en een waarschuwing aan een drempelwaarde gemaakt zodat de webhook wordt geactiveerd wanneer de resultaten van de opgeslagen zoekactie groter is dan de drempelwaarde voor.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Gebruik de methode opslag met een bestaande actie-ID voor het wijzigen van een actie webhook voor een betalingsschema.  De hoofdtekst van de aanvraag moet de etag van de actie bevatten.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Volgende stappen

- Gebruik de [REST API log zoekopdrachten uitvoeren](log-analytics-log-search-api.md) in Log Analytics.
