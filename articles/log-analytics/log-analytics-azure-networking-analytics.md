<properties
    pageTitle="Azure toegang Analytics-oplossing in Log Analytics | Microsoft Azure"
    description="U kunt de oplossing Azure toegang Analytics gebruiken in Log Analytics te bekijken Azure-toepassingsgateway-logboeken en Azure netwerk beveiliging groep zich aanmeldt."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="richrund"/>

# <a name="azure-networking-analytics-preview-solution-in-log-analytics"></a>Azure toegang Analytics (Preview)-oplossing in Log Analytics

>[AZURE.NOTE] Dit is een [voorbeeld-oplossing](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

U kunt de oplossing Azure toegang Analytics in Log Analytics moet worden gereviseerd Azure-toepassingsgateway-logboeken en Azure netwerk beveiliging groep zich aanmeldt.

U kunt logboekregistratie voor Azure-toepassingsgateway-logboeken en Azure netwerk beveiligingsgroepen inschakelen. Deze logboeken worden naar Blob storage waar ze kunnen vervolgens worden geïndexeerd door Log Analytics om te zoeken en analyse geschreven.

De logboeken aan de volgende voorwaarden wordt voldaan voor Gateways van toepassing:

+ ApplicationGatewayAccessLog
+ ApplicationGatewayPerformanceLog

De logboeken aan de volgende voorwaarden wordt voldaan voor netwerk-beveiligingsgroepen:

+ NetworkSecurityGroupEvent
+ NetworkSecurityGroupRuleCounter

## <a name="install-and-configure-the-solution"></a>Installeren en configureren van de oplossing

Gebruik de volgende instructies voor het installeren en configureren van de Azure toegang Analytics-oplossing:

1.  Diagnostische gegevens vastleggen voor de resources die u wilt controleren:
  + [Toepassingsgateway](../application-gateway/application-gateway-diagnostics.md)
  + [Beveiligingsgroep netwerk](../virtual-network/virtual-network-nsg-manage-log.md)
2.  Log Analytics als u wilt de logboekbestanden van Blob storage lezen met behulp van het proces wordt beschreven in de [JSON-bestanden in-blobopslag](../log-analytics/log-analytics-azure-storage-json.md)configureren.
3.  Schakel de Azure toegang Analytics-oplossing met behulp van het proces wordt beschreven in [de oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md).  

Als u diagnostische gegevens vastleggen voor een bepaald brontype niet inschakelt, wordt de bladen dashboard voor de desbetreffende resource leeg zijn.

## <a name="review-azure-networking-analytics-data-collection-details"></a>Azure toegang Analytics-detailgegevens siteverzameling controleren

De oplossing Azure toegang Analytics verzamelt diagnostische logboeken van Azure-blobopslag voor Azure-Toepassingsgateways en beveiligingsgroepen netwerk.
Geen-agent is vereist voor het verzamelen van gegevens.

De volgende tabel ziet u methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld van Azure toegang analysegegevens.

| Platform | Directe agent | Agent van systemen Center Operations Manager (SCOM) | Azure-opslag | SCOM vereist? | SCOM agent gegevens die per management groep worden verzonden | Frequentie van de siteverzameling |
|---|---|---|---|---|---|---|
|Azure|![Nee](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Nee](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Ja](./media/log-analytics-azure-networking/oms-bullet-green.png)|            ![Nee](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Nee](./media/log-analytics-azure-networking/oms-bullet-red.png)| 10 minuten|

## <a name="use-azure-networking-analytics"></a>Azure netwerken Analytics gebruiken

Nadat u de oplossing hebt geïnstalleerd, kunt u het overzicht van de client van weergeven en serverfouten voor uw gecontroleerde Toepassingsgateways met behulp van de **Azure toegang Analytics** -tegel op de pagina **Overzicht** in Log Analytics.

![afbeelding van Azure toegang Analytics-tegel](./media/log-analytics-azure-networking/log-analytics-azurenetworking-tile.png)

Nadat u op de tegel **Overzicht** , kunt u samenvattingen van uw logboeken weergeven en vervolgens analyseren voor de volgende categorieën:

+ Toegang tot de Gateway van toepassing Logboeken
  - Clients en servers fouten voor logboeken aan de access-toepassingsgateway
  - Aanvragen per uur voor elke toepassingsgateway
  - Mislukte aanvragen per uur voor elke toepassingsgateway
  - Fouten door gebruikersagent voor Toepassingsgateways
+ Prestaties van de Gateway toepassingen
  - Status van de host voor de Gateway-toepassing
  - Maximum- en 95e percentiel voor toepassingsgateway mislukte aanvragen
+ Beveiligingsgroep netwerk geblokkeerde loopt
  - Groep beveiligingsregels voor netwerken met geblokkeerde stromen
  - MAC-adressen met geblokkeerde stromen
+ Netwerk beveiligingsgroep loopt toegestaan
  - Groep beveiligingsregels voor netwerken met toegestane stromen
  - MAC-adressen met toegestane stromen


![afbeelding van de dashboard voor gebruiksanalyses van Azure toegang](./media/log-analytics-azure-networking/log-analytics-azurenetworking01.png)

![afbeelding van de dashboard voor gebruiksanalyses van Azure toegang](./media/log-analytics-azure-networking/log-analytics-azurenetworking02.png)

![afbeelding van de dashboard voor gebruiksanalyses van Azure toegang](./media/log-analytics-azure-networking/log-analytics-azurenetworking03.png)

![afbeelding van de dashboard voor gebruiksanalyses van Azure toegang](./media/log-analytics-azure-networking/log-analytics-azurenetworking04.png)

### <a name="to-view-details-for-any-log-summary"></a>Om voor een overzicht te bekijken

1. Klik op de tegel **Azure toegang Analytics** op de pagina **Overzicht** .
2. Klik op het dashboard voor **Gebruiksanalyses van Azure toegang** , de samenvatting van de gegevens in een van de bladen controleren en klik op een om gedetailleerde informatie over deze op de pagina log weer te geven.

    Klik op alle pagina's log zoeken, kunt u de resultaten bekijken door de tijd, gedetailleerde resultaten en de geschiedenis van log zoeken. U kunt ook filteren op aspecten en de resultaten te beperken.

## <a name="next-steps"></a>Volgende stappen

- Gebruik [zoekopdrachten in het logboek in Log Analytics](log-analytics-log-searches.md) gedetailleerde Azure toegang Analytics-gegevens moeten worden weergegeven.
