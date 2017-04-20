<properties
   pageTitle="Controleren bewerkingen, gebeurtenissen en items NSGs | Microsoft Azure"
   description="Informatie over het inschakelen van items, gebeurtenissen en operationele logboekregistratie voor NSGs"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/14/2016"
   ms.author="jdial" />

#<a name="log-analytics-for-network-security-groups-nsgs"></a>Log analytics voor netwerk-beveiligingsgroepen (NSGs)

U kunt verschillende soorten logboeken in Azure wordt aangegeven voor het beheren en problemen met NSGs. Sommige van deze logboeken kunnen worden geopend via de portal en alle logboeken kunnen worden opgehaald uit een Azure-blobopslag van, en in andere hulpprogramma's, zoals [Log analyses](../log-analytics/log-analytics-azure-networking-analytics.md), Excel en PowerBI bekeken. U kunt meer informatie over de verschillende soorten logboeken in de onderstaande lijst.

- **Controlelogboeken:** [Azure controlelogboeken bijhouden](../monitoring-and-diagnostics/insights-debugging-with-events.md) (voorheen bekend als operationele Logboeken) kunt u alle bewerkingen die wordt verzonden naar uw Azure abonnementen, en hun status weergeven. Controlelogboeken bijhouden zijn standaard ingeschakeld en kunnen worden weergegeven in de portal Azure preview.
- **Gebeurtenislogboeken:** U kunt dit logboek gebruiken om weer te geven welke NSG regels worden toegepast op VMs en exemplaar rollen gebaseerd op de MAC-adres. De status van deze regels worden verzameld om de 60 seconden.
- **Teller Logboeken:** U kunt dit logboek gebruiken om weer te geven hoe vaak elke regel NSG is toegepast als u wilt weigeren of dat verkeer is toegestaan.

>[AZURE.WARNING] Logboeken zijn alleen beschikbaar voor resources die zijn geïmplementeerd in het implementatiemodel resourcemanager. U kunt Logboeken niet gebruiken voor resources in het implementatiemodel klassieke. Voor een beter begrip van de twee modellen, verwijzingen maken naar het artikel [Wat resourcemanager implementatie- en klassieke implementatie](../resource-manager-deployment-model.md) .

##<a name="enable-logging"></a>Logboekregistratie inschakelen
Het controlelogboek wordt automatisch ingeschakeld bij alle tijden voor elke resource resourcemanager. U moet de gebeurtenis en item om te beginnen met het verzamelen van de gegevens die beschikbaar zijn via deze logboeken logboekregistratie inschakelen. Als u wilt vastleggen, de onderstaande stappen uit te voeren.

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com). Als u een bestaande netwerk-beveiligingsgroep, [een NSG maken](virtual-networks-create-nsg-arm-ps.md) voordat u verdergaat nog geen hebt.

2.  Klik op **Bladeren**in de preview-portal >> **beveiligingsgroepen netwerk**.

    ![Voorbeeld portal - netwerk beveiligingsgroepen](./media/virtual-network-nsg-manage-log/portal-enable1.png)

3. Selecteer een bestaande netwerk-beveiligingsgroep.

    ![Voorbeeld portal - netwerk groep beveiligingsinstellingen](./media/virtual-network-nsg-manage-log/portal-enable2.png)

4. In het blad **Instellingen** op **Diagnostische gegevens**en klik vervolgens in het deelvenster **Diagnostisch hulpprogramma** naast **de Status**, op **in**
5. Klik in het blad **Instellingen** op **Opslag-Account**en selecteer een bestaande opslag-account of een nieuw account te maken.  

>[AZURE.INFORMATION] controlelogboeken bijhouden is niet vereist een aparte opslag-account. Het gebruik van opslagruimte voor de gebeurtenis en regel logboekregistratie, worden de kosten in rekening gebracht.

6. Selecteer in de vervolgkeuzelijst onder **Opslag-Account**, of u wilt melden gebeurtenissen, items of beide en klik op **Opslaan**.

    ![Voorbeeld portal - diagnostische logboeken](./media/virtual-network-nsg-manage-log/portal-enable3.png)

## <a name="audit-log"></a>Controlelogboek
Dit logboek (voorheen bekend als de "operationele log") wordt gegenereerd door Azure al dan niet standaard.  De logboeken blijven behouden 90 dagen in van de Azure gebeurtenislogboeken store. Meer informatie over deze logboeken Lees het artikel [gebeurtenissen weergeven en controlelogboeken](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="counter-log"></a>Item log
Dit logboek wordt alleen gegenereerd als u deze hebt ingeschakeld op basis van per NSG zoals beschreven. De gegevens worden opgeslagen in het opslag-mailaccount dat u hebt opgegeven toen u de logboekregistratie ingeschakeld. Elke regel toegepast op resources zijn vastgelegd in de indeling van JSON, zoals hieronder wordt weergegeven.

    {
        "time": "2015-09-11T23:14:22.6940000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupRuleCounter",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupCounters",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"DenyAllOutBound",
            "direction":"Out",
            "type":"block",
            "matchedConnections":0
            }
    }

## <a name="event-log"></a>Gebeurtenislogboek
Dit logboek wordt alleen gegenereerd als u deze hebt ingeschakeld op basis van per NSG zoals beschreven. De gegevens worden opgeslagen in het opslag-mailaccount dat u hebt opgegeven toen u de logboekregistratie ingeschakeld. De volgende gegevens worden geregistreerd:

    {
        "time": "2015-09-11T23:05:22.6860000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupEvent",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupEvents",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"AllowVnetOutBound",
            "direction":"Out",
            "priority":65000,
            "type":"allow",
            "conditions":{
                "destinationPortRange":"0-65535",
                "sourcePortRange":"0-65535",
                "destinationIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32",
                "sourceIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32"
            }
        }
    }

## <a name="view-and-analyze-the-audit-log"></a>Bekijken en analyseren van het controlelogboek
U kunt weergeven en analyseren controlelogboekgegevens met een van de volgende methoden:

- **Azure extra:** Informatie ophalen uit de controlelogboeken bijhouden via Azure PowerShell, de Azure Command Line Interface (CLI), het Azure REST API of de portal Azure preview.  Stapsgewijze instructies voor elke methode zijn gedetailleerde in het artikel [controle bewerkingen met bronbeheer](../resource-group-audit.md) .
- **Power BI:** Als u nog geen een [Power BI](https://powerbi.microsoft.com/pricing) -account hebt, kunt u deze gratis proberen. Gebruik van de [Azure controlelogboeken bijhouden inhoud pack voor Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/) kunt u uw gegevens met vooraf geconfigureerde dashboards die u als gebruiken kunt analyseren-is of aanpassen.

## <a name="view-and-analyze-the-counter-and-event-log"></a>Bekijken en analyseren van de teller en gebeurtenislogboek

Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) de teller kunt verzamelen en gebeurtenislogboek bestanden van uw Blob storage-account en visualisaties en krachtige zoekmogelijkheden te analyseren van uw Logboeken bevat.

U kunt ook verbinding maken met uw account voor de opslag en ophalen van de JSON-logboekvermeldingen voor logboeken aan de gebeurtenis en item. Nadat u de JSON-bestanden downloaden, kunt u deze converteren naar een CSV- en de weergave in Excel, PowerBI of een ander gegevensvisualisatie-hulpmiddel.

>[AZURE.TIP] Als u bekend met Visual Studio en basisbegrippen bent van het wijzigen van de waarden voor de constanten en variabelen in C#, kunt u de [Hulpmiddelen voor het conversieprogramma van log](https://github.com/Azure-Samples/networking-dotnet-log-converter) verkrijgbaar via Github.

## <a name="next-steps"></a>Volgende stappen

- Item en gebeurtenislogboeken met [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) visualiseren
- Blogbericht [visualiseren uw Azure controlelogboeken bijhouden met Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Weergeven en analyseren van Azure controlelogboeken bijhouden in Power BI en nog veel meer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbericht.
