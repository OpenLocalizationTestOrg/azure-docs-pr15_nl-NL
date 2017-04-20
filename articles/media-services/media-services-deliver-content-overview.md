<properties
    pageTitle="Inhoud aan klanten te bieden | Microsoft Azure"
    description="Dit onderwerp biedt een overzicht van wat is betrokken bij het geven van de inhoud met Azure Media Services."
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
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>


# <a name="deliver-content-to-customers"></a>Inhoud ervoor zorgen dat klanten
Wanneer u uw inhoud streaming of video-on-demand aan klanten te bieden bent, wordt uw doel is om aan te geven van hoogwaardige video voor verschillende apparaten onder andere netwerkcondities.

Als u wilt realiseren, kunt u het volgende doen:

- Uw stream op een multi-(geavanceerde bitsnelheid) video bitsnelheid coderen. Hiermee wordt kunt verrichten kwaliteit en netwerk voorwaarden.
- Gebruik Microsoft Azure Media Services [dynamische verpakking](media-services-dynamic-packaging-overview.md) dynamisch de uw stream in verschillende protocollen opnieuw inpakken. Hiermee wordt kunt verrichten streaming op verschillende apparaten. Media Services ondersteunt bezorging van de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS), vloeiende Streaming, MPEG-streepje en schijven (voor Adobe WIP/Access houders).

In dit artikel geeft een overzicht van belangrijke inhoud leveren concepten.

Zie bekende problemen controleren, [bekende problemen](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dynamische verpakking

Met de dynamische verpakking Media Services vindt, kunt u de inhoud van het geavanceerde bitsnelheid MP4 of vloeiende Streaming codering in door Services Media (MPEG-streepje, HLS, vloeiende Streaming, schijven) Encoder ondersteunde streaming zonder dat u moet opnieuw inpakken in deze indelingen streaming leveren. Het is raadzaam om het geven van de inhoud met dynamische verpakking.

Om te profiteren van dynamische verpakking, moet u als volgt te werk:

- Uw bestand mezzanine (bron) in een reeks geavanceerde-bitrate MP4-bestanden of geavanceerde bitsnelheid vloeiende Streaming bestanden coderen.
- Ten minste één op aanvraag streaming eenheid voor het streaming eindpunt waaruit u van plan bent om uit te voeren van uw inhoud downloaden. Lees [hoe u het op aanvraag streaming gereserveerde eenheden schalen](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

Met dynamische verpakking, opslaan en voor de bestanden in één opslagindeling betalen. Media Services wordt bouwen en dienen het juiste antwoord op basis van uw aanvragen.

Naast het toegang hiertoe te mogelijkheden voor dynamische verpakking, bieden op aanvraag streaming gereserveerde eenheden u speciale egress capaciteit die kan worden aangeschaft in stappen van 200 Mbps. Standaard is op aanvraag streaming geconfigureerd in een gedeeld exemplaar model voor welke server resources (bijvoorbeeld een berekeningscluster of egress capaciteit) met alle andere gebruikers worden gedeeld. U kunt een op aanvraag streaming doorvoer verbeteren door aan te schaffen op aanvraag streaming gereserveerde eenheden.

Zie [dynamische verpakking](media-services-dynamic-packaging-overview.md)voor meer informatie.

## <a name="filters-and-dynamic-manifests"></a>Filters en dynamische manifesten

U kunt filters definiëren voor uw activa met Media-Services. Deze filters zijn serverregels om uw klanten doen zoals een specifieke sectie van een video afspelen of een subset van audio- en -weergaven van uw klant-apparaat kan verwerken (in plaats van alle weergaven die gekoppeld aan de activa zijn) opgeven. Deze filteren is tot en met *dynamische manifesten* die zijn gemaakt wanneer uw klant aanvraagt om te streamen van een video op basis van een of meer van de opgegeven filters bereikt.

Zie voor meer informatie, [Filters en dynamische manifesten](media-services-dynamic-manifest-overview.md).

## <a name="locators"></a>Locator

Als u wilt uw gebruikers-voorzien van een URL die kan worden gebruikt om te streamen of downloaden van uw inhoud, moet u eerst uw activa door te maken van een locator publiceren. Een locator biedt een ingangspunt voor toegang tot de bestanden in een actief. Media Services ondersteunt twee soorten Locator:

- OnDemandOrigin Locator. Deze worden gebruikt om te stream media (bijvoorbeeld MPEG-streepje, HLS of vloeiende Streaming) of geleidelijk bestanden downloaden.
-  Gedeelde toegang handtekening (SA's) URL Locator. Deze worden gebruikt voor het media-bestanden naar uw lokale computer downloaden.

Een *access-beleid* wordt gebruikt om te bepalen welke machtigingen (zoals lezen, schrijven en lijst) en duur waarvoor een client toegang voor een bepaald activum heeft. Houd er rekening mee dat de machtiging (AccessPermissions.List) niet bij het maken van een locator OrDemandOrigin moet worden gebruikt.

Locator hebben verloopdatum. De portal van Azure Hiermee stelt u een vervaldatum 100 jaar in de toekomst voor Locator.

>[AZURE.NOTE] Als u de portal van Azure Locator vóór maart 2015 gemaakt, zijn deze Locator verloopt na twee jaar instellen.

Als u een vervaldatum voor een locator bijwerken, [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) of [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) -API's te gebruiken. Houd er rekening mee dat wanneer u de vervaldatum van een locator SA's bijwerkt, de URL wordt gewijzigd.

Locator zijn niet bedoeld voor het beheren van toegangsbeheer per gebruiker. U kunt verschillende toegangsrechten voor individuele gebruikers geven met behulp van Digital Rights Management (DRM) oplossingen. Zie [Media beveiligen](http://msdn.microsoft.com/library/azure/dn282272.aspx)voor meer informatie.

Wanneer u een locator maakt, is het mogelijk dat er een vertraging van 30 seconden vanwege vereiste opslagruimte en doorgeven processen in Azure opslag.


## <a name="adaptive-streaming"></a>Aangepast streaming

Geavanceerde bitsnelheid technologieën kunnen videospeler toepassingen netwerkcondities bepalen en selecteer in de verschillende bitsnelheid. Bij het netwerkcommunicatie afnemen, selecteren de client een lagere bitsnelheid zodat afspelen kunt doorgaan met het onderste videokwaliteit. Zoals netwerkcondities verbeteren, wordt de client kunt overschakelen naar een hoger bitsnelheid met de verbeterde videokwaliteit. Azure Media Services ondersteunt de volgende geavanceerde bitsnelheid technologieën: HTTP Live Streaming (HLS), vloeiende Streaming, MPEG-streepje en schijven.

Als gebruikers wilt voorzien streaming van URL's, moet u eerst een locator OnDemandOrigin maken. Maken de locator krijgt u het grondtal pad naar de activa met de inhoud die u wilt streamen. Als u wilt kunnen om te streamen van dit inhoudstype, moet u echter dit pad verder wijzigen. Als u wilt samenstellen volledige URL naar het bestand dat streaming manifest, moet u van de locator padwaarde en het manifest (filename.ism) concatenate bestandsnaam. Vervolgens **toevoegen/bestandenlijst** en de juiste indeling (indien nodig) aan het pad locator.

>[AZURE.NOTE]U kunt ook uw inhoud streamen via een SSL-verbinding. Zorg dat de URL van uw streaming beginnen met HTTPS hiervoor.

U kunt alleen streamen via SSL als het streaming eindpunt waaruit u de inhoud bezorgen na 10 September 2014 is gemaakt. Als de URL van uw streaming zijn gebaseerd op de streaming eindpunten gemaakt na 10 September 2014, bestaat de URL uit "streaming.mediaservices.windows.net." Streaming URL's met 'origin.mediaservices.windows.net' (de oude notatie) worden niet ondersteund voor SSL. Als de URL in de oude indeling is en u wilt mogelijk om te streamen via SSL, maakt u een nieuw streaming-eindpunt. URL's op basis van het nieuwe streaming endpoint gebruiken om te streamen van uw inhoud via SSL.


## <a name="streaming-url-formats"></a>Streaming URL-indelingen

### <a name="mpeg-dash-format"></a>MPEG-streepje opmaken

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=MPD-time-csf)



### <a name="apple-http-live-streaming-hls-v4-format"></a>Apple HTTP Live Streaming (HLS) V4-indeling

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Apple HTTP Live Streaming (HLS) V3-indeling

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Opmaak van Apple HTTP Live Streaming (HLS) met alleen geluid filter

Standaard alleen geluid nummers zijn opgenomen in de HLS manifest. Dit is vereist voor Apple Store-certificering voor mobiele netwerken. In dit geval als een client geen voldoende bandbreedte of verbinding via een verbinding 2G maakt, schakelt afspelen u naar alleen geluid. Hiermee houdt inhoud streaming zonder buffer, maar er is geen video. In sommige gevallen speler buffer mogelijk voorkeur via alleen geluid. Als u verwijderen van de alleen geluid bijhouden wilt, voegt u **alleen geluid = false** naar de URL.

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3,audio-Only=False)

Zie [ondersteuning voor dynamische bestandenlijst samenstelling en HLS uitvoerindelingen aanvullende functies](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/)voor meer informatie.


### <a name="smooth-streaming-format"></a>Vloeiende Streaming opmaken

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Voorbeeld:

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest

### <a id="fmp4_v20"></a>Vloeiende 2.0 Streaming manifest (oudere manifest)

Standaard bevat vloeiende Streaming manifest format de herhaald tag (r-tag). Sommige spelers ondersteunen echter niet de r-markering. Clients met deze spelers kunnen een indeling die wordt uitgeschakeld de r-tag gebruiken:

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

### <a name="hds-for-adobe-primetimeaccess-licensees-only"></a>Schijven (voor Adobe WIP/Access houders)

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f)

## <a name="progressive-download"></a>Geleidelijke downloaden

Met geleidelijke downloaden, kunt u beginnen met het afspelen van media voordat het hele bestand is gedownload. U kunt geen geleidelijk .ism * (ismv, isma, ismt of ismc) bestanden downloaden.

Geleidelijk om inhoud te downloaden, gebruikt u het type OnDemandOrigin locator. Het volgende voorbeeld ziet de URL die is gebaseerd op het type OnDemandOrigin locator:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

U moet opslag versleuteld elementen die u wilt streamen van de service origin geleidelijke downloaden ontsleutelen.

## <a name="download"></a>Downloaden

Uw om inhoud te downloaden naar een clientapparaat worden gebruikt, moet u een Locator SA's maken. De locator SA's hebt u toegang tot de container Azure Storage waar het bestand zich bevindt. Als u de URL downloaden, moet u de bestandsnaam tussen de host en SA's handtekening insluiten.

Het volgende voorbeeld ziet de URL die is gebaseerd op de locator SA's:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

De volgende punten zijn van toepassing:

- U moet opslag versleuteld elementen die u wilt streamen van de service origin geleidelijke downloaden ontsleutelen.
- Het downloaden van een die niet binnen 12 uur is voltooid, mislukt.

## <a name="streaming-endpoints"></a>Streaming eindpunten

Een streaming eindpunt vertegenwoordigt een streaming service waarop inhoud rechtstreeks naar een speler clienttoepassing of naar een netwerk voor contentlevering (CDN) voor verdere verdeling geven kunt. De uitgaande stroom van een streaming eindpunt-service kan zijn een live gegevensstroom of een activum video-on-demand in uw Media Services-account. U kunt ook de capaciteit van de streaming service endpoint worden afgehandeld groeiende bandbreedte behoeften door aan te passen streaming gereserveerde eenheden bepalen. U moet ten minste één gereserveerde eenheid toewijzen voor toepassingen in een productieomgeving. Zie [hoe u de schaal van een media-service](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

## <a name="known-issues"></a>Bekende problemen

### <a name="changes-to-smooth-streaming-manifest-version"></a>Wijzigingen in vloeiende Streaming bestandenlijst versie

Vóór de juli 2016 servicerelease--wanneer activa geproduceerd door Media Encoder Standard, Media Encoder Premium werkstroom of de eerdere Azure Media Encoder zijn streamen via dynamische verpakking--de vloeiende Streaming manifest geretourneerd zou voldoen aan versie 2.0. De duur van het fragment gebruik in versie 2.0, niet de labels zogenaamde herhalen ('r'). Bijvoorbeeld:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

In de versie van de service juli 2016 voldoet de gegenereerde vloeiende Streaming manifest aan versie 2.2, met een fragment duur herhaald-labels gebruiken. Bijvoorbeeld:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Enkele van de oudere vloeiende Streaming clients mogelijk geen ondersteuning voor de herhaald tags en, mislukt het manifest laden. Om dit probleem beperken, kunt u de parameter verouderde manifest indeling **(opmaak = fmp4-v20)** of de klant bijwerken naar de nieuwste versie, die ondersteuning biedt voor Herhaaltoetsen tags. Zie [vloeiende Streaming 2.0](media-services-deliver-content-overview.md#fmp4_v20)voor meer informatie.

## <a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Verwante onderwerpen

[Media Services Locator na opslag toetsen schuivend bijwerken](media-services-roll-storage-access-keys.md)
