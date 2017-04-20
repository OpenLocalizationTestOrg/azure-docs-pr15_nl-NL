<properties
   pageTitle="Bewerkingen, gebeurtenissen en items taakverdeling controleren | Microsoft Azure"
   description="Leer hoe u de waarschuwing gebeurtenissen inschakelen en status statusregistratie voor taakverdeling Azure zoeken"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="log-analytics-for-azure-load-balancer-preview"></a>Log analytics voor Azure taakverdeling (Preview)

U kunt verschillende soorten logboeken in Azure wordt aangegeven voor het beheren en problemen met netwerktaakverdelers. Sommige van deze logboeken kunnen worden geopend via de portal. Alle logboeken kunnen worden opgehaald uit een Azure-blobopslag, en worden bekeken in andere hulpprogramma's, zoals Excel en PowerBI. U kunt meer informatie over de verschillende soorten logboeken in de onderstaande lijst.

- **Controlelogboeken:** [Azure controlelogboeken bijhouden](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) (voorheen bekend als operationele Logboeken) kunt u alle bewerkingen die wordt verzonden naar uw Azure abonnementen, en hun status weergeven. Controlelogboeken bijhouden zijn standaard ingeschakeld en kunnen worden weergegeven in de portal van Azure.
- **Waarschuw gebeurtenislogboeken:** U kunt dit logboek gebruiken om weer te geven welke waarschuwingen voor taakverdeling zijn verheven. Het veld status voor de taakverdeling verzameld om de vijf minuten. In dit logboek is alleen opgeslagen als een waarschuwing gebeurtenis voor laden-verdeling wordt verheven.
- **Systeemstatus test Logboeken:** U kunt dit logboek gebruiken om te controleren voor test systeemstatus Controleer de status, hoeveel exemplaren zijn online in het laden de verdeling van back-enddatabase en het percentage van virtuele machines netwerkverkeer van de taakverdeling ontvangt. Dit logboek is geschreven test status gebeurtenis wijzigen.

>[AZURE.IMPORTANT] Meld u aan analyses werkt op dit moment alleen voor Internet die tegenover elkaar liggen netwerktaakverdelers. Logboeken zijn alleen beschikbaar voor resources die zijn geïmplementeerd in het implementatiemodel resourcemanager. U kunt Logboeken niet gebruiken voor resources in het implementatiemodel klassieke. Zie [lidmaatschap resourcemanager implementatie en klassieke implementatie](../../articles/resource-manager-deployment-model.md)voor meer informatie over de implementatiemodellen.

## <a name="enable-logging"></a>Logboekregistratie inschakelen

Het controlelogboek wordt automatisch ingeschakeld voor elke resource resourcemanager. U moet de gebeurtenis en systeemstatus test logboekregistratie om te beginnen met het verzamelen van de gegevens die beschikbaar zijn via deze logboeken inschakelen. Gebruik de volgende stappen logboekregistratie inschakelen.

Meld u aan bij de [portal van Azure](http://portal.azure.com). Als u geen al een taakverdeling, [een taakverdeling maken](load-balancer-get-started-internet-arm-ps.md) voordat u verdergaat.

1. Klik op **Bladeren**in de portal.
2. Selecteer **netwerktaakverdelers**.

    ![Portal - verdeling van belasting](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Selecteer een bestaande taakverdeling >> **Alle instellingen**.
4. Aan de rechterkant van het dialoogvenster onder de naam van de verdeling van de belasting, schuif naar **Monitoring**, klikt u op **Diagnostische gegevens**.

    ![Portal - laden-verdeling-instellingen](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. Selecteer in het deelvenster **Diagnostische gegevens** onder **Status**, **op**.
6. Klik op **opslag-Account**.
7. Onder **LOGBOEKEN**, selecteert u een bestaand opslag-account of een nieuw account te maken. Gebruik de schuifregelaar om te bepalen hoeveel dagen gebeurtenis dat in de gebeurtenislogboeken worden bewaard. 8. Klik op **Opslaan**.

    ![Portal - diagnostische logboeken](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

>[AZURE.INFORMATION] controlelogboeken bijhouden is niet vereist een aparte opslag-account. Het gebruik van opslag voor de gebeurtenis en systeemstatus test logboekregistratie kosten wordt oplopen.

## <a name="audit-log"></a>Controlelogboek

Het controlelogboek wordt gegenereerd al dan niet standaard. De logboeken blijven behouden 90 dagen in van de Azure gebeurtenislogboeken store. Meer informatie over deze logboeken Lees het artikel [gebeurtenissen weergeven en controlelogboeken](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="alert-event-log"></a>Waarschuwing gebeurtenislogboek

Dit logboek alleen wordt gegenereerd als u deze hebt ingeschakeld op een per verdeling per laden. De gebeurtenissen worden vastgelegd in de indeling van JSON en opgeslagen in de opslag-account dat u hebt opgegeven toen u de logboekregistratie ingeschakeld. Hier volgt een voorbeeld van een gebeurtenis.

    {
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }

De JSON-uitvoer ziet u de eigenschap *eventname* waarin de reden voor de verdeling van de belasting een waarschuwing gemaakt. In dit geval is de waarschuwing gegenereerd vanwege TCP-poorten uitputting die worden veroorzaakt door de bron-IP NAT limieten (SNAT).

## <a name="health-probe-log"></a>Servicestatus test log

Dit logboek alleen wordt gegenereerd als u deze hebt ingeschakeld op een per laden verdeling soort_jaar als gedetailleerde hierboven. De gegevens worden opgeslagen in het opslag-mailaccount dat u hebt opgegeven toen u de logboekregistratie ingeschakeld. Een container met de naam 'inzichten-logboeken-loadbalancerprobehealthstatus' is nu gemaakt en de volgende gegevens worden geregistreerd:

    {
        "records":
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 1,
                "healthPercentage": 50.000000
            }
        },
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 0,
                "healthPercentage": 100.000000
            }
        }
    }

De JSON-uitvoer ziet u in het veld eigenschappen de basisgegevens voor de status test. De eigenschap *dipDownCount* bevat het totale aantal exemplaren op de back-enddatabase die geen netwerkverkeer vanwege mislukte test antwoorden krijgen.

## <a name="view-and-analyze-the-audit-log"></a>Bekijken en analyseren van het controlelogboek

U kunt weergeven en analyseren controlelogboekgegevens met een van de volgende methoden:

- **Azure extra:** Informatie ophalen uit de controlelogboeken bijhouden via Azure PowerShell, de Azure Command Line Interface (CLI), het Azure REST API of de portal Azure preview. Stapsgewijze instructies voor elke methode zijn gedetailleerde in het artikel [controle bewerkingen met bronbeheer](../../articles/resource-group-audit.md) .
- **Power BI:** Als u nog een [Power BI](https://powerbi.microsoft.com/pricing) -account, kunt u deze gratis proberen. Gebruik van de [Azure controlelogboeken bijhouden inhoud pack voor Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), kunt u uw gegevens met vooraf geconfigureerde dashboards analyseren of u weergaven aanpassen aan uw vereisten kunt aanpassen.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Bekijken en de test voor status en gebeurtenislogboek analyseren

U moet verbinding maken met uw account voor de opslag en de JSON-logboekvermeldingen voor de gebeurtenis en systeemstatus test logboeken op te halen. Nadat u de JSON-bestanden downloaden, kunt u deze converteren naar een CSV- en de weergave in Excel, PowerBI of een ander gegevensvisualisatie-hulpmiddel.

>[AZURE.TIP] Als u bekend met Visual Studio en basisbegrippen bent van het wijzigen van de waarden voor de constanten en variabelen in C#, kunt u de [Hulpmiddelen voor het conversieprogramma van log](https://github.com/Azure-Samples/networking-dotnet-log-converter) verkrijgbaar via Github.

## <a name="additional-resources"></a>Aanvullende informatie

- Blogbericht [visualiseren uw Azure controlelogboeken bijhouden met Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Weergeven en analyseren van Azure controlelogboeken bijhouden in Power BI en nog veel meer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbericht.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over laden verdeling sondes](load-balancer-custom-probe-overview.md)
