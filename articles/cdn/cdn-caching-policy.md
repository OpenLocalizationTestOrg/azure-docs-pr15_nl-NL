<properties
    pageTitle="CDN Caching van beleid in Media Services-extensie"
    description="Dit onderwerp biedt een overzicht van een CDN caching van beleid in Media Services-extensie."
    services="media-services,cdn"
    documentationCenter=".NET"
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>
 
#<a name="cdn-caching-policy-in-media-services-extension"></a>CDN Caching van beleid in Media Services-extensie

Azure Media Services biedt dat HTTP op basis van geavanceerde Streaming en geleidelijke downloaden. HTTP gebaseerd streaming is ten zeerste scalable met voordelen van caching in proxy en CDN lagen, alsmede het caching aan clientzijde. Streaming eindpunten bevat algemene streaming mogelijkheden en configuratie voor HTTP-cache headers. HTTP Cachebeheer streaming eindpunten worden ingesteld: max-leeftijd en verloopt kopteksten. U kunt meer informatie voor HTTP-cache headers krijgen van [W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

##<a name="default-caching-headers"></a>Standaard cache kopteksten

Standaard toepassen streaming eindpunten 3-daagse cache kopteksten voor streaming op aanvraag-gegevens (werkelijke media fragmenten/stukken) en manifest(playlist). Voor live-streaming, streaming eindpunten toepassen 3-daagse cache kopteksten voor gegevens (werkelijke media fragmenten/stukken) en 2 seconden ingedrukt koptekst voor manifest(playlist) aanvragen in cache. Wanneer het live programma verandert in op aanvraag (persoonlijke archief) toepassen op aanvraag streaming cache kopteksten.

##<a name="azure-cdn-integration"></a>Azure CDN-integratie

Azure Media Services biedt [geïntegreerde CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) voor streaming-eindpunten. Cachebeheer kopteksten is van toepassing op dezelfde manier als streaming eindpunten naar CDN ingeschakeld streaming eindpunten. Azure CDN gebruikt streaming eindpunt cachewaarden om te definiëren de levensduur van de objecten intern in de cache geconfigureerd en ook gebruikt deze waarde om in te stellen de bezorging cache kopteksten. Het verdient aanbeveling geen kleine cachewaarden instellen wanneer streaming eindpunten CDN gebruiken ingeschakeld. Kleine waarden instellen verkleinen van de prestaties en het voordeel van CDN verkleinen. Het is niet toegestaan om in te stellen cache kopteksten kleiner dan 600 seconden voor CDN ingeschakeld streaming eindpunten.

>[AZURE.IMPORTANT] Azure Media Services-integratie met Azure CDN is geïmplementeerd op **Azure CDN van Verizon**.  Als u gebruiken van **Azure CDN van Akamai** voor Azure Media Services wilt, moet u [het eindpunt handmatig configureren](cdn-create-new-endpoint.md).  Zie voor meer informatie over de functies van Azure CDN [CDN overzicht](cdn-overview.md).

##<a name="configuring-cache-headers-with-azure-media-services"></a>Cache kopteksten met Azure Media Services configureren

U kunt Azure-beheerportal of Azure Media Services API's gebruiken voor het configureren van cache koptekst waarden.

1. Als u wilt configureren cache kopteksten met management portal Raadpleeg [Streaming eindpunten beheren](../media-services/media-services-portal-manage-streaming-endpoints.md) sectie configureren van het eindpunt Streaming.
2. Azure Media Services REST API, [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK, [StreamingEndpointCacheControl eigenschappen](http://go.microsoft.com/fwlink/?LinkId=615302).

##<a name="cache-configuration-precedence-order"></a>Cache configuratie voorrangsregels

1. Azure Media Services is geconfigureerd cachewaarde overschrijft standaardwaarde.
2. Als er geen handmatige configuratie, geldt de standaardwaarden.
3. Door standaard 2 seconden ingedrukt cache kopteksten van toepassing op live streaming manifest(playlist) ongeacht de configuratie van Azure Media of Azure Storage en overschrijven van deze waarde is niet beschikbaar.
