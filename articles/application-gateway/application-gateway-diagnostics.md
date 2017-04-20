<properties 
   pageTitle="Access en prestaties logboeken en aan de doelstellingen voor toepassingsgateway controleren | Microsoft Azure"
   description="Meer informatie over het inschakelen en beheren van Access en prestaties logboeken voor de Gateway-toepassing"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="amitsriva" />

# <a name="diagnostics-logging-and-metrics-for-application-gateway"></a>Diagnostische gegevens vastleggen en aan de doelstellingen voor toepassingsgateway

Azure biedt de mogelijkheid om de resource met logboekregistratie en aan de doelstellingen te houden

[**Registratie**](#enable-logging-with-powershell) - logboekregistratie kan voor prestaties, access en andere logboeken opslaan of uit een resource voor verbruikt.

[**Aan de doelstellingen**](#metrics) - toepassingsgateway momenteel heeft één meetwaarde. Deze metrisch meet de doorvoer van de toepassingsgateway in Bytes per seconde.

U kunt verschillende soorten logboeken in Azure wordt aangegeven voor het beheren en problemen met Toepassingsgateways. Sommige van deze logboeken kunnen worden geopend via de portal en alle logboeken kunnen worden opgehaald uit een Azure-blobopslag van, en in andere hulpprogramma's, zoals [Log analyses](../log-analytics/log-analytics-azure-networking-analytics.md), Excel en PowerBI bekeken. U kunt meer informatie over de verschillende soorten logboeken uit de volgende lijst:

- **Controlelogboeken:** [Azure controlelogboeken bijhouden](../monitoring-and-diagnostics/insights-debugging-with-events.md) (voorheen bekend als operationele Logboeken) kunt u alle bewerkingen die wordt verzonden naar uw Azure-abonnement, en hun status weergeven. Controlelogboeken bijhouden zijn standaard ingeschakeld en kunnen worden weergegeven in de portal Azure preview.
- **Toegang tot logboeken:** U kunt dit logboek toepassing gateway access patroon bekijken en analyseren belangrijke informatie van beller IP, URL verzoek is antwoord latentie, inclusief retourneren code, bytes in-en uitfaden. Log in Access worden verzameld elke 300 seconden. Dit logboek bevat Eén record per exemplaar van de toepassingsgateway. De gateway-exemplaar van de toepassing kan worden geïdentificeerd door eigenschap 'instanceId'.
- **Prestatielogboeken:** U kunt dit logboek gebruiken om weer te geven hoe toepassing gateway-exemplaren uitvoert. Dit logboek wordt vastgelegd voor informatie over de prestaties op per exemplaar basis totale verzoek served, inclusief doorvoersnelheid in bytes, totaal aantal aanvragen served, mislukte aanvraag telling, orde en beschadigd back-enddatabase exemplaar tellen. Prestaties log verzameld om de 60 seconden.
- **Firewall Logboeken:** U kunt dit logboek gebruiken om weer te geven van de aanvragen die worden vastgelegd door de modus voor detectie of preventie van een toepassingsgateway die is geconfigureerd met web application firewall.

>[AZURE.WARNING] Logboeken zijn alleen beschikbaar voor resources die zijn geïmplementeerd in het implementatiemodel resourcemanager. U kunt Logboeken niet gebruiken voor resources in het implementatiemodel klassieke. Voor een beter begrip van de twee modellen, verwijzingen maken naar het artikel [Wat resourcemanager implementatie- en klassieke implementatie](../resource-manager-deployment-model.md) .

## <a name="enable-logging-with-powershell"></a>Inschakelen van logboekregistratie met PowerShell

Het controlelogboek wordt automatisch ingeschakeld voor elke resource resourcemanager. U moet access en prestaties om te beginnen met het verzamelen van de gegevens die beschikbaar zijn via deze logboeken logboekregistratie inschakelen. Als u wilt vastleggen, raadpleegt u de volgende stappen uit: 

1. Houd er rekening mee van de account van uw opslagruimte Resource-ID, waarin de logboekgegevens is opgeslagen. Is dit van het formulier: /subscriptions/\<subscriptionId\>/resourceGroups/\<groep resourcenaam\>/providers/Microsoft.Storage/storageAccounts/\<opslagaccountnaam\>. Een account opslag van uw abonnement kan worden gebruikt. U kunt de preview-portal gebruiken om deze informatie te vinden.

    ![Voorbeeld van de portal - toepassing Gateway diagnostische gegevens](./media/application-gateway-diagnostics/diagnostics1.png)

2. Opmerking de Resource-ID van de toepassingsgateway van uw voor welke logboekregistratie is zijn ingeschakeld. Is dit van het formulier: /subscriptions/\<subscriptionId\>/resourceGroups/\<groep resourcenaam\>/providers/Microsoft.Network/applicationGateways/\<gateway toepassingsnaam\>. U kunt de preview-portal gebruiken om deze informatie te vinden.

    ![Voorbeeld van de portal - toepassing Gateway diagnostische gegevens](./media/application-gateway-diagnostics/diagnostics2.png)

3. Diagnostische gegevens vastleggen met de volgende powershell-cmdlet:

        Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true  

>[AZURE.INFORMATION] controlelogboeken bijhouden is niet vereist een aparte opslag-account. Het gebruik van opslagruimte voor toegang en prestaties logboekregistratie bijhoudt kosten.

## <a name="enable-logging-with-azure-portal"></a>Inschakelen van logboekregistratie met Azure-portal

### <a name="step-1"></a>Stap 1

Navigeer naar de bron in de portal van Azure. Klik op **Diagnostische logboeken**. Als dit de eerste keer diagnostische gegevens configureren ziet het blad uit de volgende afbeelding:

3 logboeken zijn van toepassingsgateway, beschikbaar.

- Log in Access
- Prestaties Log
- Logboek firewall

Klik op **inschakelen diagnostische hulpprogramma's** om te beginnen met het verzamelen van gegevens.

![Diagnostisch hulpprogramma instelling blade][1]

### <a name="step-2"></a>Stap 2

Klik op het blad **Instellingen diagnostische gegevens** , de instellingen voor de realisatie de diagnostische logboeken worden ingesteld. In dit voorbeeld wordt Log analytics gebruikt voor de opslag van de logboeken. Klik op **configureren** onder **Log Analytics** voor het configureren van uw werkruimte. Gebeurtenis hubs en een opslag-account kunnen worden gebruikt als u ook de hulpprogramma's voor diagnose-Logboeken opslaan.

![Diagnostisch hulpprogramma blade][2]

### <a name="step-3"></a>Stap 3

Kies een bestaande OMS-werkruimte of een nieuw account te maken. In dit voorbeeld wordt een bestaande gebruikt.

![OMS werkruimten][3]

### <a name="step-4"></a>Stap 4

Als alles compleet is, Controleer de instellingen en klik op **Opslaan** als de instellingen wilt opslaan.

![Bevestig de selectie][4]

## <a name="audit-log"></a>Controlelogboek

Dit logboek (voorheen bekend als de "operationele log") wordt gegenereerd door Azure al dan niet standaard.  De logboeken blijven behouden 90 dagen in van de Azure gebeurtenislogboeken store. Meer informatie over deze logboeken Lees het artikel [gebeurtenissen weergeven en controlelogboeken](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="access-log"></a>Log in Access

Dit logboek alleen wordt gegenereerd als u deze hebt ingeschakeld op een per per Gateway-toepassing zoals wordt beschreven in de voorgaande stappen. De gegevens worden opgeslagen in het opslag-mailaccount dat u hebt opgegeven toen u de logboekregistratie ingeschakeld. Elke toegang die van toepassingsgateway is vastgelegd in JSON-indeling, zoals gezien in het volgende voorbeeld:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayAccess",
        "time": "2016-04-11T04:24:37Z",
        "category": "ApplicationGatewayAccessLog",
        "properties": {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIP":"37.186.113.170",
            "clientPort":"12345",
            "httpMethod":"HEAD",
            "requestUri":"/xyz/portal",
            "requestQuery":"",
            "userAgent":"-",
            "httpStatus":"200",
            "httpVersion":"HTTP/1.0",
            "receivedBytes":"27",
            "sentBytes":"202",
            "timeTaken":"359",
            "sslEnabled":"off"
        }
    }

## <a name="performance-log"></a>Prestaties log

Dit logboek alleen wordt gegenereerd als u deze hebt ingeschakeld op een per per Gateway-toepassing zoals wordt beschreven in de voorgaande stappen. De gegevens worden opgeslagen in het opslag-mailaccount dat u hebt opgegeven toen u de logboekregistratie ingeschakeld. De volgende gegevens worden geregistreerd:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayPerformance",
        "time": "2016-04-09T00:00:00Z",
        "category": "ApplicationGatewayPerformanceLog",
        "properties": 
        {
            "instanceId":"ApplicationGatewayRole_IN_1",
            "healthyHostCount":"4",
            "unHealthyHostCount":"0",
            "requestCount":"185",
            "latency":"0",
            "failedRequestCount":"0",
            "throughput":"119427"
        }
    }


## <a name="firewall-log"></a>Logboek firewall

Dit logboek alleen wordt gegenereerd als u deze hebt ingeschakeld op een per gateway toepassing zoals wordt beschreven in de voorgaande stappen. Dit logboek ook moet u deze web application-firewall op een application gateway. De gegevens worden opgeslagen in het opslag-mailaccount dat u hebt opgegeven toen u de logboekregistratie ingeschakeld. De volgende gegevens worden geregistreerd:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
        "operationName": "ApplicationGatewayFirewall",
        "time": "2016-09-20T00:40:04.9138513Z",
        "category": "ApplicationGatewayFirewallLog",
        "properties":     {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIp":"108.41.16.164",
            "clientPort":1815,
            "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
            "ruleId":"OWASP_973336",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "action":"Logged",
            "site":"Global",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
    }

## <a name="view-and-analyze-the-audit-log"></a>Bekijken en analyseren van het controlelogboek

U kunt weergeven en analyseren controlelogboekgegevens met een van de volgende methoden:

- **Azure extra:** Informatie ophalen uit de controlelogboeken bijhouden via Azure PowerShell, de Azure Command Line Interface (CLI), het Azure REST API of de portal Azure preview.  Stapsgewijze instructies voor elke methode zijn gedetailleerde in het artikel [controle bewerkingen met bronbeheer](../resource-group-audit.md) .
- **Power BI:** Als u nog geen een [Power BI](https://powerbi.microsoft.com/pricing) -account hebt, kunt u deze gratis proberen. Gebruik van de [Azure controlelogboeken bijhouden inhoud pack voor Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) kunt u uw gegevens met vooraf geconfigureerde dashboards die u als gebruiken kunt analyseren-is of aanpassen.

## <a name="view-and-analyze-the-access-performance-and-firewall-log"></a>Bekijken en analyseren van het logboek access, prestaties en firewall

Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) het item kunt verzamelen en gebeurtenislogboek bestanden van uw Blob storage-account en visualisaties en krachtige zoekmogelijkheden te analyseren van uw Logboeken bevat.

U kunt ook verbinding maken met uw account voor de opslag en ophalen van de JSON-logboekvermeldingen voor toegang en prestaties Logboeken. Nadat u de JSON-bestanden downloaden, kunt u deze converteren naar een CSV- en de weergave in Excel, PowerBI of een ander gegevensvisualisatie-hulpmiddel.

>[AZURE.TIP] Als u bekend met Visual Studio en basisbegrippen bent van het wijzigen van de waarden voor de constanten en variabelen in C#, kunt u de [Hulpmiddelen voor het conversieprogramma van log](https://github.com/Azure-Samples/networking-dotnet-log-converter) verkrijgbaar via Github.

## <a name="metrics"></a>Aan de doelstellingen

Aan de doelstellingen is een functie voor bepaalde Azure resources waar u items in de portal bekijken kunt. Voor de Gateway-toepassing is één meetwaarde beschikbaar op het moment van het schrijven van dit artikel. Deze metrisch doorvoer is en ziet u in de portal. Ga naar de toepassingsgateway van een en klikt u op **de doelstellingen**.  Selecteer doorvoer in de sectie **beschikbaar maatstaven** om weer te geven van de waarden. In de volgende afbeelding ziet u een voorbeeld met de filters die kunnen worden gebruikt om de gegevens in andere tijd bereiken weergeven.

Een overzicht van de huidige ondersteuning aan de doelstellingen, gaat u naar [de doelstellingen van de ondersteunde met Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md)

![metrische weergave][5]

## <a name="alert-rules"></a>Waarschuwingsregels

Waarschuwingsregels kunnen worden gestart op basis van van op een resource aan de doelstellingen. Dit betekent voor de toepassingsgateway, kan een waarschuwing bellen van een webhook of e-beheerder als de doorvoer van de toepassingsgateway boven, onder of op een drempelwaarde voor een opgegeven periode is.

Het volgende voorbeeld begeleidt u bij het maken van een regel voor de waarschuwing die een e-mailbericht met een beheerder stuurt nadat een drempelwaarde doorvoer heeft geschonden.

### <a name="step-1"></a>Stap 1

Klik op **metrische waarschuwing toevoegen** om te beginnen. Deze blade kan ook worden bereikt vanaf het blad de doelstellingen.

![waarschuwingsregels blade][6]

### <a name="step-2"></a>Stap 2

In het blad **regel toevoegen** , vul de naam van de voorwaarde, en secties een melding en klik op **OK** wanneer u klaar.

De **voorwaarde** Gegevenskiezer kan 4 waarden, **groter is dan**, **groter dan of gelijk**, **kleiner dan**of **kleiner dan of gelijk aan**.

De kiezer **termijn** , kunt u kiezen van een periode van 5 minuten tot en met 6 uur.

Door in te schakelen **E-mail eigenaren, inzenders, en lezers** kan het e-mailbericht zijn dynamische op basis van de gebruikers die toegang tot de desbetreffende resource hebben. Anders kan een door komma's gescheiden lijst met gebruikers worden verstrekt in het tekstvak **Extra beheerder email(s)** .

![regel blade toevoegen][7]

Als de drempelwaarde voor wordt gekraakt, ontvangen een e-mailbericht vergelijkbaar is met de in de volgende afbeelding:

![drempelwaarde voor nagekomen e][8]

Een lijst met waarschuwingen wordt weergegeven wanneer een melding voor een metrische is gemaakt en een overzicht gegeven van de waarschuwingsregels wordt.

![waarschuwingsregels weergeven][9]

Meer informatie over waarschuwingen, gaat u naar [de meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Als u wilt weten over meer informatie over webhooks en hoe u ze kunt gebruiken met waarschuwingen, gaat u naar [een webhook over een Azure metrische waarschuwing configureren](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="next-steps"></a>Volgende stappen

- Item en gebeurtenislogboeken met [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) visualiseren 
- Blogbericht [visualiseren uw Azure controlelogboeken bijhouden met Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Weergeven en analyseren van Azure controlelogboeken bijhouden in Power BI en nog veel meer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbericht.

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png