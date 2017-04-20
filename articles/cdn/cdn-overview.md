<properties
    pageTitle="Overzicht van de Azure CDN | Microsoft Azure"
    description="Leer wat Azure inhoud bezorging netwerk (CDN) is en hoe gebruiken voor het leveren van hoge bandbreedte inhoud door caching BLOB's en statische inhoud."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/30/2016"
    ms.author="casoper"/>

# <a name="overview-of-the-azure-content-delivery-network-cdn"></a>Overzicht van het netwerk Azure Contentlevering (CDN)

> [AZURE.NOTE] In dit document wordt beschreven wat de Azure inhoud bezorging netwerk (CDN) is, hoe het werkt en de functies van elk product Azure CDN.  Als u wilt deze informatie overslaan en rechtstreeks naar een zelfstudie over het maken van een CDN-eindpunt, raadpleegt u [Azure CDN gebruikt](cdn-create-new-endpoint.md).  Als u zien van een lijst met locaties van huidige CDN knooppunt wilt, raadpleegt u [Azure CDN POP locaties](cdn-pop-locations.md).

Azure inhoud bezorging netwerk (CDN) in de cache opgeslagen statische webinhoud op strategische geplaatste locaties te leveren maximumdoorvoer om inhoud te bieden aan gebruikers.  De CDN biedt ontwikkelaars een globale oplossing om hoge bandbreedte inhoud door de cache van de inhoud op fysieke knooppunten kant van de wereld zitten te bieden. 

De voordelen van het gebruik van de CDN aan cache website activa zijn onder andere:

- Betere prestaties en functionaliteit voor eindgebruikers, met name wanneer u toepassingen waarin meerdere terugzenden zijn vereist om inhoud te laden.
- Groot schaalbaarheid beter verwerken momentopname hoge belasting, zoals aan het begin van een evenement product.
- Door gebruikersaanvragen distribueren en inhoud van edge-servers, minder verkeer naar de oorsprong verzonden.


## <a name="how-it-works"></a>Werkwijze

![CDN-overzicht](./media/cdn-overview/cdn-overview.png)

1. Een gebruiker (Lisa) om een bestand (ook wel activa genoemd) vraagt via een URL met een speciale domeinnaam, zoals `<endpointname>.azureedge.net`.  Het verzoek stuurt DNS naar het beste uitvoering van de locatie van de punt-van-aanwezigheid (POP).  Dit is gewoonlijk de POP die is geografisch dichtst bevindt bij de gebruiker.

2. Als de edge-servers in het POP-nog geen het bestand in de cache, vraagt de randserver het bestand uit het oorspronkelijke.  De oorsprong kan zijn een Azure-Web-App, Azure-Cloudservice, opslag van Azure-account of een openbaar webserver.

3. De oorsprong geeft als resultaat het bestand naar de randserver, inclusief optioneel HTTP-headers met een beschrijving van het bestand Time to Live (TTL).

4. De randserver slaat het bestand en geeft als resultaat het bestand naar de oorspronkelijke degene (Lisa).  Het bestand blijft in de cache opgeslagen op de randserver totdat de TTL is verlopen.  Als het oorspronkelijke niet een TTL opgeeft, is de standaardwaarde voor TTL zeven dagen.

5. Extra gebruikers mogelijk met de die dezelfde URL van hetzelfde bestand vervolgens aanvragen en mogelijk ook doorgestuurd naar dezelfde POP.

6. Als de TTL-waarde voor het bestand is niet verlopen, retourneert de randserver het bestand uit de cache.  Dit resulteert in een snellere, meer heeft gereageerd gebruikerservaring.


## <a name="azure-cdn-features"></a>Azure CDN-functies

Er zijn drie Azure CDN-producten: **Azure CDN standaard uit Akamai**, **Azure CDN standaard uit Verizon**en **Azure CDN Premium van Verizon**.  De volgende tabel bevat de functies die beschikbaar zijn met elk product.

|       | Standaard Akamai | Standaard Verizon | Premium Verizon |
|-------|-----------------|------------------|-----------------|
| Eenvoudige integratie met Azure services zoals [opslag](cdn-create-a-storage-account-with-cdn.md), [Cloudservices](cdn-cloud-service-with-cdn.md) [Web Apps](../app-service-web/cdn-websites-with-cdn.md)en [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;**|
| Beheer via een [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](./cdn-app-dev-net.md), [Node.js](./cdn-app-dev-node.md)of [PowerShell](./cdn-manage-powershell.md). | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| HTTPS-ondersteuning | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Taakverdeling | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [DDOS](https://www.us-cert.gov/ncas/tips/ST04-015) -beveiliging | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| IPv4/IPv6-twee-stack | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Ondersteuning voor aangepaste domein naam](cdn-map-content-to-custom-domain.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Caching van query-tekenreeks](cdn-query-string.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Geografische filteren](cdn-restrict-access-by-country.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Snel verwijderen](cdn-purge-endpoint.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Activa vooraf laden](cdn-preload-endpoint.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Core analytics](cdn-analyze-usage-patterns.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Ondersteuning voor HTTP/2](https://msdn.microsoft.com/library/mt762901.aspx) | **& #x 2713;**  |  |  |
| [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md) | | | **& #x 2713;** |
| [Realtime stat](cdn-real-time-stats.md) | | | **& #x 2713;** |
| [Realtime waarschuwingen](cdn-real-time-alerts.md) | | | **& #x 2713;** |
| [Aanpasbare, op basis van een regel voor contentlevering-engine](cdn-rules-engine.md) | | | **& #x 2713;** |
| Instellingen voor cache/kopteksten (met behulp van [regels engine](cdn-rules-engine.md))  | | | **& #x 2713;** |
| URL omleiden/herschrijven (met behulp van [regels engine](cdn-rules-engine.md)) | | | **& #x 2713;** |
| Mobiel apparaat regels (met behulp van [regels engine](cdn-rules-engine.md))  | | | **& #x 2713;** |

>[AZURE.TIP] Is er een functie die u wilt zien in Azure CDN?  [Stuur ons uw feedback](https://feedback.azure.com/forums/169397-cdn)! 

## <a name="next-steps"></a>Volgende stappen

Zie [Azure CDN gebruikt](./cdn-create-new-endpoint.md)om te beginnen met CDN.

Als u een bestaande CDN-klant bent, kunt u nu de eindpunten van uw CDN via de [portal van Microsoft Azure](https://portal.azure.com) of met [PowerShell](cdn-manage-powershell.md)beheren.

Als u wilt de CDN in actie zien, raadpleegt u de [video van onze opbouwen 2016-sessie](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Informatie over het automatiseren van Azure CDN met [.NET](./cdn-app-dev-net.md) of [Node.js](./cdn-app-dev-node.md).

Zie [CDN prijzen](https://azure.microsoft.com/pricing/details/cdn/)voor prijsinformatie.
