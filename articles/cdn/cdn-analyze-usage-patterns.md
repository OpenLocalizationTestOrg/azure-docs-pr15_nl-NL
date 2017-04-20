<properties
    pageTitle="Azure CDN gebruikspatronen analyseren | Microsoft Azure"
    description="U kunt gebruikspatronen voor het gebruik van de volgende rapporten CDN bekijken: bandbreedte, gegevens worden overgedragen, treffers, Cache statussen, verhouding tussen de Cache raken, IPV4/IPV6-gegevens worden overgedragen."
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
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="analyze-azure-cdn-usage-patterns"></a>Azure CDN gebruikspatronen analyseren

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

U kunt gebruikspatronen voor het gebruik van de volgende rapporten CDN bekijken:

- Bandbreedte
- Gegevens worden overgedragen
- Treffers
- Statussen die cache
- Cache treffers
- IPv4/IPV6-gegevens worden overgedragen

## <a name="accessing-advanced-http-reports"></a>Geavanceerde HTTP-rapporten openen

1. Klik in het blad CDN-profiel op de knop **beheren** .

    ![CDN profiel blade knop beheren](./media/cdn-reports/cdn-manage-btn.png)

    De beheerportal CDN wordt geopend.

2. Plaats de muisaanwijzer boven de tab **Analytics** en klik vervolgens muis boven de **Core rapporten** : flyout.  Klik op het gewenste rapport in het menu.

    ![CDN-beheerportal - menu Core-rapporten](./media/cdn-reports/cdn-core-reports.png)


## <a name="bandwidth"></a>Bandbreedte

Het rapport bandbreedte bestaat uit een grafiek en gegevens tabel waarin wordt aangegeven dat het bandbreedtegebruik voor HTTP en HTTPS over een bepaalde periode. U kunt het bandbreedtegebruik weergeven over alle pop CDN's of een bepaalde POP. Hiermee kunt u naar de verkeer pieken- en distributiegroepen in het CDN POP's in Mbps weergeven.

- Selecteer alle rand knooppunten kunnen zien verkeer uit alle knooppunten of kies een bepaald land/knooppunt in de vervolgkeuzelijst.
- Selecteer datumbereik weergeven van gegevens voor deze vandaag/week/deze maand, enzovoort of aangepaste datums invoeren en vervolgens klikt u op 'Ga"om ervoor te zorgen dat uw selectie wordt bijgewerkt.
- U kunt exporteren en downloaden van de gegevens door te klikken op het pictogram excel-blad volgende 'om 'te gaan.

Het rapport wordt elke 5 minuten bijgewerkt.

![Bandbreedte rapport](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Gegevens worden overgedragen

Dit rapport bestaat uit een grafiek en gegevens tabel waarin wordt aangegeven dat het verkeer gebruik voor HTTP en HTTPS over een bepaalde periode. U kunt het verkeer gebruik weergeven over alle pop CDN's of een bepaalde POP. Hiermee kunt u naar de verkeer pieken- en distributiegroepen in het CDN POP's in GB weergeven.

- Selecteer alle rand knooppunten kunnen zien verkeer van alle notities of kies een bepaald land/knooppunt in de vervolgkeuzelijst.
- Selecteer datumbereik weergeven van gegevens voor deze vandaag/week/deze maand, enzovoort of aangepaste datums invoeren en vervolgens klikt u op 'Ga"om ervoor te zorgen dat uw selectie wordt bijgewerkt.
- U kunt exporteren en downloaden van de gegevens door te klikken op het pictogram excel-blad volgende 'om 'te gaan.

Het rapport wordt elke 5 minuten bijgewerkt.

![Gegevens worden overgedragen rapport](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Treffers (statuscodes)

Dit rapport beschrijft de verdeling van de aanvraag statuscodes voor uw inhoud. Een verzoek om inhoud genereert een HTTP-statuscode. De statuscode wordt beschreven hoe de aanvraag in rand POP's worden afgehandeld. Bijvoorbeeld 2xx statuscodes aangeven dat de aanvraag is aangeboden aan een client, terwijl een statuscode 4xx geeft aan dat is een fout opgetreden. Zie voor meer informatie over HTTP-statuscode [statuscodes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

- Selecteer datumbereik weergeven van gegevens voor deze vandaag/week/deze maand, enzovoort of aangepaste datums invoeren en vervolgens klikt u op 'Ga"om ervoor te zorgen dat uw selectie wordt bijgewerkt.
- U kunt exporteren en downloaden van de gegevens door te klikken op het excel-werkblad bevinden volgende 'om 'te gaan.

![Treffers rapport](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Statussen die cache

Dit rapport beschrijft de verdeling van treffers en de cache Cachemissers voor clientverzoek. Aangezien de snelste prestaties afkomstig zijn uit de cachetreffers, kunt u gegevens bezorging snelheden door Minimalisatie van het aantal Cachemissers en verlopen cachetreffers kunt optimaliseren. Cache mislukte kunnen worden verminderd door het configureren van uw oorspronkelijke server om te voorkomen dat het toewijzen van "no-cache" antwoord kopteksten, door te vermijden queryreeks caching, behalve indien strikt noodzakelijk is en door te vermijden van niet-cache antwoord codes. Verlopen cache treffers kunnen worden voorkomen door een actief is max-leeftijd zo lang mogelijk om te minimaliseren van het aantal aanvragen met de oorspronkelijke server.

![Cache statussen rapport](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Belangrijkste cache statussen zijn:

- TCP_HIT: Served vanaf rand. Het object in de cache is en niet de max-leeftijd had overschreden.
- TCP_MISS: Served van het beginpunt. Het object is niet in cache en de reactie terug naar origin is.
- TCP_EXPIRED _MISS: Served uit origin na gevalideerd met origin. Het object in de cache is maar de max-leeftijd had overschreden. Het resultaat is een gevalideerd met origin in de cache-object wordt vervangen door een nieuwe antwoord van origin.
- TCP_EXPIRED _HIT: Served vanaf rand na gevalideerd met origin. Het object in de cache is maar de max-leeftijd had overschreden. Het resultaat is een gevalideerd met de server origin in de cache-object wordt niet gewijzigd.

- Selecteer datumbereik weergeven van gegevens voor deze vandaag/week/deze maand, enzovoort of aangepaste datums invoeren en vervolgens klikt u op 'Ga"om ervoor te zorgen dat uw selectie wordt bijgewerkt.
- U kunt exporteren en downloaden van de gegevens door te klikken op het pictogram excel-blad volgende 'om 'te gaan.

### <a name="full-list-of-cache-statuses"></a>Volledige lijst met cache statussen

- TCP_HIT - deze status wordt vermeld, wanneer een verzoek om rechtstreeks van de POP wordt bediend naar de klant. Activa wordt direct van een POP wanneer deze is in de cache op het dichtst bevindt bij de client POP en er een geldige time to live of TTL bediend. TTL wordt bepaald door de volgende antwoordheaders:

    - Cache-Control:-s-maxage
    - Cache-Control: max-leeftijd
    - Verloopt

- TCP_MISS - deze status wordt aangegeven dat een in de cache opgeslagen versie van de gevraagde activa niet is gevonden op het dichtst bevindt bij de client POP. De activa wordt van een bronserver of een bronserver schild gevraagd. Als de originele server of de oorspronkelijke schild server als resultaat de activa, wordt er served naar de klant en in de cache op de client en de edgeserver. Anders een niet-200 statuscode (bijvoorbeeld 403 verboden, 404 niet gevonden, enzovoort) wordt geretourneerd.

- TCP_EXPIRED _HIT - deze status wordt vermeld, wanneer een nieuw vergaderverzoek die activa met een verlopen TTL gericht, zoals wanneer van het actief max-leeftijd is verlopen, rechtstreeks vanuit de POP is served naar de klant.

    Een verlopen aanvraag meestal levert een aanvraag gevalideerd met de oorspronkelijke server. In de volgorde voor een _HIT TCP_EXPIRED voordoen, moet de bronserver aangeven dat een nieuwere versie van de activa niet bestaat. Dit soort situatie werkt meestal de Cache-Control en verloopt kopteksten van dat actief.

- TCP_EXPIRED _MISS - deze status wordt vermeld, wanneer een nieuwere versie van een verlopen actief in de cache van de POP wordt bediend naar de klant. Dit gebeurt wanneer de TTL-waarde voor een in de cache activa is verlopen (bijvoorbeeld verlopen max-leeftijd) en de oorspronkelijke server geeft als resultaat een nieuwere versie van dat actief. Deze nieuwe versie van de activa moet worden verzonden naar de klant in plaats van de in de cache opgeslagen versie. Daarnaast kunnen deze wordt in de cache op de randserver en de client.

- CONFIG_NOCACHE - deze status wordt aangegeven dat de configuratie van een klant-specifiek op onze rand POP uitgesloten van de activa in cache wordt opgeslagen.

- GEEN: deze status wordt aangegeven dat een cache inhoud fris is niet gecontroleerd.

- TCP_ CLIENT_REFRESH _MISS - deze status wordt vermeld, wanneer een HTTP-client (bijvoorbeeld browser) forceert u een rand POP om op te halen van een nieuwe versie van een verouderde activa van de originele server.

    Standaard voorkomen onze servers dat een HTTP-client afdwingen van onze edge-servers om op te halen van een nieuwe versie van de activa van de originele server.

- TCP_ PARTIAL_HIT - deze status wordt vermeld, wanneer een aanvraag voor het bereik van byte in een treffer voor een gedeeltelijk in de cache activa resulteert. Het bytebereik van de gevraagde wordt naar de klant direct van de POP bediend.

- UNCACHEABLE - deze status wordt vermeld wanneer de Cache-Control en verloopt kopteksten van activa aangeeft dat deze mag niet worden opgeslagen in een POP- of door de HTTP-client. Dit soort van aanvragen worden van de originele server verwerkt

## <a name="cache-hit-ratio"></a>Cache treffers

Dit rapport bevat het percentage van in de cache aanvragen die zijn dienden rechtstreeks vanuit de cache.

Het rapport bevat de volgende informatie:

- De gevraagde inhoud is in de cache op het dichtst bevindt bij de aanvrager POP.
- Het verzoek is served rechtstreeks vanuit de rand van het netwerk.
- Het verzoek hebt niet moet worden gevalideerd met de bronserver.

Het rapport niet zijn opgenomen:

- Vergaderverzoeken die u wilt weigeren vanwege land filteropties.
- Aanvragen voor activa waarvan kopteksten aangeven dat ze mag niet worden opgeslagen. Bijvoorbeeld Cachebeheer: privé, Cachebeheer: geen-cache of Pragma: geen-cache kopteksten wordt voorkomen dat de activa in cache wordt opgeslagen.
- Byte bereikaanvragen voor gedeeltelijk in de cache opgeslagen inhoud.

De formule is: (TCP_ TREFFER / (TCP_ TREFFER + TCP_MISS)) * 100

- Selecteer datumbereik weergeven van gegevens voor deze vandaag/week/deze maand, enzovoort of aangepaste datums invoeren en vervolgens klikt u op 'Ga"om ervoor te zorgen dat uw selectie wordt bijgewerkt.
- U kunt exporteren en downloaden van de gegevens door te klikken op het pictogram excel-blad volgende 'om 'te gaan.


![Treffers verhouding rapport in de cache](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>IPv4/IPv6-gegevens worden overgedragen

Dit rapport toont de verdeling van de gebruik verkeer in IPV4 tegenover IPV6.

![IPv4/IPv6-gegevens worden overgedragen](./media/cdn-reports/cdn-ipv4-ipv6.png)

- Selecteer datumbereik weergeven van gegevens voor deze vandaag/week/deze maand, enzovoort of aangepaste datums invoeren.
- Klik vervolgens op "Ga" om ervoor te zorgen dat uw selectie wordt bijgewerkt.


## <a name="considerations"></a>Overwegingen

Rapporten kunnen alleen worden gegenereerd binnen de laatste 18 maanden.
