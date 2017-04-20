<properties 
    pageTitle="Overzicht van Azure Media Services en veelvoorkomende scenario's | Microsoft Azure" 
    description="In dit onderwerp biedt een overzicht van Azure Media Services" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>

#<a name="azure-media-services-overview-and-common-scenarios"></a>Overzicht van Azure Media Services en veelvoorkomende scenario 's

Microsoft Azure Media Services is een extensible cloudgebaseerde platform waarmee ontwikkelaars op scalable media management en de bezorging van toepassingen. Media Services is gebaseerd op de REST API's waarmee u kunt veilig uploaden, opslaan, coderen en inpakken van video of audio-inhoud voor zowel op aanvraag en live streaming bezorging bij verschillende clients (bijvoorbeeld tv-PC en mobiele apparaten).

U kunt met geheel Media Services end-to-end-werkstromen maken. U kunt ook onderdelen van derden gebruiken voor sommige onderdelen van de werkstroom. Bijvoorbeeld coderen met een derde partij encoder. Vervolgens uploaden, beveiligen, als pakket inpakken, met behulp van Media Services geven.

U kunt stream uw inhoud live of geven van inhoud op aanvraag. In dit onderwerp ziet u veelvoorkomende scenario's voor het leveren van uw inhoud [live](media-services-overview.md#live_scenarios) of [op aanvraag](media-services-overview.md#vod_scenarios). Het onderwerp ook wordt gekoppeld aan andere relevante onderwerpen.

## <a name="sdks-and-tools"></a>SDK's en hulpprogramma's voor

Als u wilt Media Services-oplossingen bouwen, kunt u het volgende gebruiken:

- [Media-Services REST API](https://msdn.microsoft.com/library/azure/hh973617.aspx)
- Een van de beschikbare client SDK's:
- [Azure mediaservices SDK voor .NET](https://github.com/Azure/azure-sdk-for-media-services),
- [Azure SDK for Java](https://github.com/Azure/azure-sdk-for-java),
- [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),
- [Azure mediaservices voor Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Dit is een niet-Microsoft-versie van een SDK Node.js. Er wordt beheerd door een community en momenteel heeft geen toegang hebt tot een 100% van de APIs AMS).
- Bestaande hulpprogramma's:
- [Azure-portal](https://portal.azure.com/)
- [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) is een Winforms / C#-toepassing voor Windows)

##<a name="media-services-learning-paths"></a>Media Services leerpaden

U kunt hier leerpaden AMS weergeven:

- [AMS Live Streaming werkstroom](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
- [AMS op aanvraag Streaming werkstroom](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

##<a name="prerequisites"></a>Vereisten voor

Als u wilt gaan Azure Media Services gebruiken, moet u hebt het volgende:

3. Een Azure-account. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com)voor meer informatie.
2. Een Azure Media Services-account. Gebruik de Azure-portal, .NET of REST API om Azure Media Services-account te maken. Zie [Account maken](media-services-portal-create-account.md)voor meer informatie.
3. (Optioneel) Ontwikkelomgeving instellen. Kies .NET of REST API voor uw ontwikkelomgeving. Zie [-omgeving instellen](media-services-dotnet-how-to-use.md)voor meer informatie.

Leer ook hoe u verbinding maakt via een programma [verbinding maken](media-services-dotnet-connect-programmatically.md).
4. (Aanbevolen) Een of meer schaaleenheden toewijzen. Het wordt aanbevolen een of meer schaaleenheden voor toepassingen in productieomgeving toewijzen.   Zie [beheren streaming eindpunten](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

##<a name="concepts-and-overview"></a>Concepten en overzicht

Zie voor Azure Media Services concepten, [Concepten](media-services-concepts.md).

Zie [Azure Media Services stapsgewijze zelfstudies](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series)voor een stapsgewijze procedures reeks waarin u met de belangrijkste onderdelen van Azure Media Services kennismaakt. Deze reeks heeft een goed overzicht van concepten en het hulpmiddel AMSE gebruikt om te laten zien AMS taken. Houd er rekening mee dat AMSE tool een Windows-hulpprogramma is. Dit hulpmiddel ondersteunt de meeste van de taken die u via programmacode met [AMS SDK voor .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK for Java](https://github.com/Azure/azure-sdk-for-java)of [Azure PHP SDK bereiken kunt](https://github.com/Azure/azure-sdk-for-php).

##<a id="vod_scenarios"></a>Media op aanvraag met Azure Media Services bieden: veelvoorkomende scenario's en taken

In deze sectie worden veelvoorkomende scenario's beschreven en koppelingen naar relevante onderwerpen. In het volgende diagram ziet u de belangrijkste onderdelen van de Media Services-platform die betrekking hebben op in het geven van inhoud op aanvraag. 

![VoD werkstroom](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)


###<a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a>Beveiliging van inhoud in de opslag en levering van streaming media in de wissen (niet-versleutelde)

1. Een kwalitatief hoogwaardig mezzanine-bestand uploaden naar een actief.
    
    Het wordt aanbevolen toepassen op de optie codering opslag uw activa ter bescherming van uw inhoud tijdens het uploaden en terwijl u op de rest in opslag.
 
1. Coderen naar een reeks geavanceerde bitsnelheid MP4-bestanden. 

    Het wordt aanbevolen toepassen op de optie codering opslag de uitvoer van activa ter bescherming van uw inhoud in rust.
    
1. Configureer activa bezorging beleid (gebruikt door dynamische verpakking). 
    
    Als uw activa is versleuteld opslag, configureren u **moet** activa bezorging beleid. 

1. De activa door te maken van een locator OnDemand publiceren.

    Zorg ervoor dat ten minste één streaming gereserveerde eenheid op de streaming eindpunt waaruit u inhoud wilt hebben.

1. Stream gepubliceerd inhoud.

###<a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Beveiliging van inhoud in opslag, bezorgen dynamisch versleutelde streaming media  

Als u wilt kunnen dynamische versleuteling, moet u eerst ten minste één eenheid voor streaming gereserveerde ophalen op het streaming eindpunt waaruit u wilt streamen versleutelde inhoud.

1. Een kwalitatief hoogwaardig mezzanine-bestand uploaden naar een actief. Optie voor opslag-codering van toepassing op de activa.
1. Coderen naar een reeks geavanceerde bitsnelheid MP4-bestanden. Optie voor opslag-codering van toepassing op de uitvoer van activa.
1. Maak versleuteling inhoud sleutel voor de activa die u wilt dynamisch worden gecodeerd tijdens het afspelen.
2. Inhoud belangrijke autorisatiebeleid configureren.
1. Configureer activa bezorging beleid (gebruikt door dynamische verpakking en dynamische versleuteling).
1. De activa door te maken van een locator OnDemand publiceren.
1. Stream gepubliceerd inhoud. 

###<a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Media-analyses gebruiken om te leiden sneller inzicht krijgen in uw video 's 

Media-analyses is een verzameling spraak en visuele onderdelen die het gemakkelijker voor bedrijven en ondernemingen om te leiden sneller inzicht krijgen in hun videobestanden. Zie [Azure Media Services Analytics overzicht](media-services-analytics-overview.md)voor meer informatie.

1. Een kwalitatief hoogwaardig mezzanine-bestand uploaden naar een actief.
2. Gebruik een van de volgende Media Analytics-services verwerkingstijd van uw video's:
    
    - **Indexering** – [proces video's met Azure Media indexering 2](media-services-process-content-with-indexer2.md)
    - **Hyperlapse** – [Hyperlapse Media-bestanden met Azure Media Hyperlapse](media-services-hyperlapse-content.md)
    - **Beweging detectie** – [Beweging detectie van Azure Media analysegegevens](media-services-motion-detection.md).
    - **Nominale detectie en nominale emoties** – [nominale en emoties detectie van Azure Media analysegegevens](media-services-face-and-emotion-detection.md).
    - **Video samenvatting** – [Azure Media Video miniaturen gebruiken om te maken van een Video-samenvatting](media-services-video-summarization.md)
3. Media-processors Media analyses vlees MP4-bestanden of JSON-bestanden. Als een Mediaprocessor geproduceerd een MP4-bestand, kunt u het bestand geleidelijk downloaden. Als een Mediaprocessor geproduceerd een JSON-bestand, kunt u het bestand downloaden van de Azure-blobopslag. 


###<a name="deliver-progressive-download"></a>Geven geleidelijke downloaden 

1. Een kwalitatief hoogwaardig mezzanine-bestand uploaden naar een actief.
1. Coderen naar een enkel MP4-bestand.
1. De activa door te maken van een locator OnDemand of SA's publiceren.

    Als OnDemand locator gebruikt, zorg er dan voor dat ten minste één streaming gereserveerde eenheid op de streaming eindpunt waaruit u van plan bent geleidelijk inhoud kunnen downloaden.

    Als SA's locator gebruikt, wordt de inhoud wordt gedownload van de Azure-blobopslag. In dit geval hoeft u niet te hebben streaming gereserveerde eenheden.
  
1. Geleidelijk inhoud downloaden.

##<a id="live_scenarios"></a>Live Streaming gebeurtenissen met Azure mediaservices bieden

Tijdens het werken met de volgende onderdelen zijn meestal betrokken Live Streaming:

- Een camera die wordt gebruikt voor het uitzenden van een gebeurtenis.
- Een live video encoder die signalen tussen de camera converteert naar streams die zijn verzonden naar een live streaming service.

U kunt ook meerdere live tijd gesynchroniseerd encoders. Voor bepaalde kritieke live gebeurtenissen die zeer hoog beschikbaarheid van de aanvraag en kwaliteit van ervaring, het wordt aanbevolen om redundante encoders actieve met tijd synchronisatieAls bereiken naadloze failover met geen gegevens verloren gaan.
- Een live streaming service waarmee u het volgende doen:

- live met behoud van verschillende live streaming protocollen (bijvoorbeeld RTMP of vloeiende Streaming), nemen
- (optioneel) coderen van de stream naar geavanceerde bitsnelheid
- Voorbeeld van uw live stream
- opnemen en de aangezogen inhoud opslaan om te worden streamen later (Video-on-Demand)
- rechtstreeks naar uw klanten, of naar een inhoud bezorging netwerk (CDN) voor verdere distributie, kunt u de inhoud via algemene streaming protocollen (bijvoorbeeld MPEG streepje, vloeiend, HLS, schijven) aanbieden.


**Microsoft Azure mediaservices** (AMS) biedt de mogelijkheid om te nemen, coderen, voorbeeld, opslaan en geven van uw live streaming inhoud.

Tijdens het geven van uw inhoud naar klanten wordt uw doel is om aan te geven van een video van hoge kwaliteit voor verschillende apparaten onder andere netwerkcondities. Als u wilt verrichten kwaliteit en netwerk voorwaarden, live encoders te gebruiken om uw stream op multi-bitrate (geavanceerde) video bitsnelheid.  Als u wilt verrichten streaming op verschillende apparaten, gebruik u Media Services [dynamische verpakking](media-services-dynamic-packaging-overview.md) uw stream naar verschillende protocollen dynamisch opnieuw inpakken. Media Services ondersteunt bezorging van de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS), vloeiende Streaming, MPEG-streepje en schijven (voor Adobe WIP/Access houders).

In Azure Media Services verwerken **kanalen**, **programma's**en **StreamingEndpoints** alle live streaming functies ingest, inclusief opmaak, DVR, beveiliging, schaalbaarheid en redundantie.

Een **kanaal** vertegenwoordigt een pijplijn voor het verwerken van live streaming inhoud. Een kanaal kan ontvangen een live streams ingevoerd in de volgende manieren:

- Een on-premises live encoder verzendt multi-bitrate **RTMP** of **Vloeiende Streaming** (gefragmenteerde MP4) naar het kanaal dat is geconfigureerd voor de bezorging van **Pass Through-query** . De bezorging **Pass Through-query** is wanneer de geconsumeerde streams **kanaal**s doorgegeven zonder verdere verwerking. U kunt de volgende live encoders die uitvoer multi-bitrate vloeiende Streaming: vorm van het element, Envivio, zoals Cisco.  De volgende live encoders uitvoer RTMP: Adobe Flash Live, Telestream Wirecast en Tricaster transcoders.  Een live encoder kunt ook een één bitsnelheid verzenden naar een kanaal die niet is ingeschakeld voor het coderen van live, maar die niet wordt aanbevolen. Wanneer een aangevraagd, biedt Media Services de stream naar klanten.

>[AZURE.NOTE] Met behulp van een Pass Through-methode, is de voordeligste manier om te live streaming wanneer u mee bezig zijn meerdere gebeurtenissen via een lange periode en u hebt al worden geïnvesteerd in on-premises implementatie encoders. Zie [prijzen](/pricing/details/media-services/) details.

- Een on-premises implementatie live encoder stuurt een enkel-bitsnelheid naar het kanaal dat is ingesteld voor het uitvoeren van live coderen met Media-Services op een van de volgende indelingen: RTP (MPEG-TS), RTMP of vloeiende Streaming (gefragmenteerd MP4). Het kanaal voert live codering van de binnenkomende één bitsnelheid op een multi-(geavanceerde) video bitsnelheid. Wanneer een aangevraagd, biedt Media Services de stream naar klanten.


###<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Werken met kanalen die live multi-bitsnelheid van on-premises implementatie encoders (Pass Through-query ontvangt)

In het volgende diagram ziet u de belangrijkste onderdelen van het platform AMS die betrekking hebben op in de werkstroom **Pass Through-query** .

![Live werkstroom][live-overview2]

Zie voor meer informatie [werken met kanalen die ontvangen Multi-Live bitsnelheid van On-premises Encoders](media-services-live-streaming-with-onprem-encoders.md).

###<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Werken met kanalen die zijn ingeschakeld om uit te voeren live coderen met Azure Media Services

In het volgende diagram ziet u de belangrijkste onderdelen van het platform AMS die betrekking hebben op in Live Streaming werkstroom waarin een kanaal om uit te voeren live coderen met Media-Services is ingeschakeld.

![Live werkstroom][live-overview1]

Zie [werken met kanalen die voor het uitvoeren van Live coderen met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)voor meer informatie.


##<a name="consuming-content"></a>Die inhoudstypen

Azure Media Services biedt de hulpmiddelen die u maken van krachtige, dynamische speler clienttoepassingen voor de meeste platforms moet, waaronder: iOS-apparaten, Android-apparaten, Windows, Windows Phone, Xbox en Set-top vakken. In dit onderwerp vindt u koppelingen naar SDK's en -speler kaders die u gebruiken kunt voor het ontwikkelen van uw eigen clienttoepassingen die streaming media, van Media Services kunnen gebruiken.

[Ontwikkelen van toepassingen van de videospeler](media-services-develop-video-players.md)

##<a name="enabling-azure-cdn"></a>Azure CDN inschakelen

Media Services ondersteunt integratie met Azure CDN. Voor informatie over het inschakelen van Azure CDN, Zie [hoe u Streaming eindpunten beheren in een Media Services-Account](media-services-portal-manage-streaming-endpoints.md).

##<a name="scaling-a-media-services-account"></a>Schaalbaarheid van een Media Services-account

U kunt **Media Services** schalen door het opgeven van het aantal **Streaming gereserveerde** en **Gereserveerde eenheden codering** die u wilt dat uw account met in te richten.

U kunt ook uw Media Services-account schalen opslag accounts op te tellen. Elk account opslag is beperkt tot 500 TB. Om uit te vouwen uw opslag voorbij de standaardbeperkingen, kunt u meerdere opslag-accounts toevoegen aan een enkele Media Services-account.

[In dit](media-services-portal-scale-streaming-endpoints.md) onderwerp bevat koppelingen naar relevante onderwerpen.

##<a name="support"></a>Ondersteuning

[Azure ondersteuning](https://azure.microsoft.com/support/options/) biedt ondersteuningsopties voor Azure Media Services inclusief.

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="service-level-agreement-sla"></a>Serviceovereenkomst (SLA)

- Voor het coderen van Media Services garanderen we 99,9% beschikbaarheid van de REST API transacties.
- Voor Streaming, worden we doorverbonden serviceaanvragen met een 99,9% beschikbaarheid garantie voor bestaande media-inhoud wanneer u ten minste één eenheid Streaming is gekocht.
- Voor Live kanalen garanderen we dat uitgevoerd kanalen externe connectivity ten minste 99,9% van de tijd hebben.
- Voor inhoudsbeveiliging garanderen we dat we wordt met succes te voldoen aan belangrijke aanvragen ten minste 99,9% van de tijd.
- Voor indexering, we worden doorverbonden serviceaanvragen indexering taak verwerkt met een gereserveerd codering eenheid 99,9% van de tijd.

Zie [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/)voor meer informatie.

<!-- Images -->
[overview]: ./media/media-services-overview/media-services-overview.png
[vod-overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png
 
