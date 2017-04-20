<properties 
    pageTitle="Filters en dynamische manifesten | Microsoft Azure" 
    description="In dit onderwerp wordt beschreven hoe filters maken zodat de klant ze stream bepaalde secties van een stream kan gebruiken. Media Services Hiermee maakt u dynamische manifesten deze selectief streaming wordt gearchiveerd." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="cenkdin;juliako"/>

# <a name="filters-and-dynamic-manifests"></a>Filters en dynamische manifesten

Media Services kunt vanaf 2.11 versie, u filters definiëren voor uw activa. Deze filters zijn serverregels, zodat uw klanten doen items, zoals kiezen: afspelen alleen een gedeelte van een video (in plaats van de hele video afspelen), of alleen een subset van audio- en -weergaven van uw klant-apparaat kan verwerken (in plaats van alle weergaven die gekoppeld aan de activa zijn) opgeven. Deze filteren van uw activa wordt gearchiveerd tot en met **Dynamische bestandenlijst**s die zijn gemaakt op verzoek van de klant om te streamen van een video op basis van de opgegeven filters.

In dit onderwerp wordt beschreven hoe veelvoorkomende scenario's waarin met behulp van filters zou zeer nuttig om uw klanten en koppelingen naar onderwerpen waarin wordt getoond hoe via programmacode maken van filters (momenteel, kunt u filters maken met REST API's alleen).

##<a name="overview"></a>Overzicht

Tijdens het geven van uw inhoud naar klanten (streaming live gebeurtenissen of video-on-demand) wordt uw doel is om aan te geven van een video van hoge kwaliteit voor verschillende apparaten onder andere netwerkcondities. Om dit doel doet u het volgende:

- coderen van de stream op multi-([Geavanceerde bitsnelheid](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) video bitsnelheid (dit wordt gedaan van kwaliteit en netwerk voorwaarden) en 
- Gebruik Media Services [Dynamische verpakking](media-services-dynamic-packaging-overview.md) dynamisch opnieuw pakket opslaan van uw stream in verschillende protocollen (dit wordt gedaan streaming op verschillende apparaten). Media Services ondersteunt bezorging van de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS), vloeiende Streaming, MPEG-streepje en schijven (voor Adobe WIP/Access houders). 

###<a name="manifest-files"></a>Manifest-bestanden 

Wanneer u een actief voor geavanceerde bitsnelheid streaming coderen, een bestand **bestandenlijst** (afspeellijst) wordt gemaakt (het bestand is op tekst gebaseerde of op XML gebaseerde). De **bestandenlijst** -bestand bevat metagegevens, zoals streaming: bijhouden type (audio, video of tekst), naam, begintijd en eindtijd, bitsnelheid (eigenschappen), bijhouden talen, bijhouden presentatievenster (schuifregelaar vensters van vaste duur), video-codec (code). Dit wordt ook aangegeven wanneer de speler naar het volgende fragment ophalen op basis van informatie over de volgende afspeelbaar video fragmenten beschikbaar en de locatie. Fragmenten (of segmenten) zijn de werkelijke 'stukken' van een video-inhoud.


Hier volgt een voorbeeld van een manifest bestand: 

    
    <?xml version="1.0" encoding="UTF-8"?>  
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">
    
    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />
    
    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    </SmoothStreamingMedia>
    
###<a name="dynamic-manifests"></a>Dynamische manifesten

[Scenario's](media-services-dynamic-manifest-overview.md#scenarios) zijn wanneer de klant meer flexibiliteit moet dan wat wordt beschreven in de standaard-activa manifest-bestand. Bijvoorbeeld:

- Apparaat specifieke: geven alleen de opgegeven weergaven en/of opgegeven taal nummers die worden ondersteund door het apparaat dat is gebruikt voor het afspelen van de inhoud ("Afbeeldingsweergave filteren"). 
- Verminder het manifest om weer te geven van een onderliggend clip van live-gebeurtenis ("onderliggend clip filteren").
- Knippen het begin van een video ('inkorten een video').
- Presentatie-venster (DVR) alleen worden aangeboden als een beperkte lengte van het venster DVR in de speler ("aanpassen presentatievenster") aanpassen.
 
Als u wilt bereiken deze flexibiliteit, biedt Media Services **Dynamische manifesten** op basis van vooraf gedefinieerde [filters](media-services-dynamic-manifest-overview.md#filters).  Nadat u de filters definieert, kunnen uw klanten deze gebruiken om te streamen een specifieke Afbeeldingsweergave of onderliggend clips van uw video. Ze zou filters in de streaming URL opgeeft. Filters kunnen worden toegepast op geavanceerde bitsnelheid streaming protocollen die worden ondersteund door [Dynamische verpakking](media-services-dynamic-packaging-overview.md): HLS, MPEG-streepje vloeiende Streaming en schijven. Bijvoorbeeld:

MPEG-streepje URL met filter

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Vloeiende Streaming URL met filter

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


Zie [overzicht van de bezorgen](media-services-deliver-content-overview.md)voor meer informatie over het geven van uw inhoud en URL's streaming maken.


>[AZURE.NOTE]Houd er rekening mee dat dynamische manifesten niet de activa en de standaard-manifest voor dat actief wijzigen beter. De klant kunt kiezen om het aanvragen van een stream met of zonder filters. 


###<a id="filters"></a>Filters 

Er zijn twee soorten activa filters: 

- Algemene filters (kunnen worden toegepast op alle activa in het Azure Media Services-account, een levensduur van het account hebt) en 
- Lokale filters (kunnen alleen worden toegepast op activa waarmee het filter gekoppeld na het maken was, hebt u de levensduur van de activa). 

Globale en lokale filtertypen hebben precies dezelfde eigenschappen. Het belangrijkste verschil tussen de twee is voor welke scenario's welk type een filer geschikter. Algemene filters zijn in het algemeen geschikt is voor apparaatprofielen (Afbeeldingsweergave filteren) waar lokale filters kunnen worden gebruikt om een specifieke activa bijsnijden.


##<a id="scenarios"></a>Gebruikelijke scenario 's 

Zoals is voordat uw inhoud leveren aan klanten (streaming live gebeurtenissen of video-on-demand) uw doel is om u te geven van een video van hoge kwaliteit voor verschillende apparaten onder andere netwerkcondities is vermeld. Daarnaast is het mogelijk hebben andere vereisten waarbij u gebruikmaakt van uw activa filteren en het gebruik van **Dynamische bestandenlijst**s. De volgende secties geven een beknopt overzicht van verschillende filteren scenario's.

- Geef alleen een subset van audio- en -weergaven dat bepaalde apparaten kunnen worden verwerkt (in plaats van alle weergaven die gekoppeld aan de activa zijn). 
- Alleen een gedeelte van een video (in plaats van de hele video's afspelen) afspelen.
- DVR presentatievenster aanpassen.

##<a name="rendition-filtering"></a>Afbeeldingsweergave filteren 

U kunt uw activa voor meerdere codering profielen (H.264 basislijn H.264 hoog, AACL, AACH, Dolby Digital Plus) en meerdere kwaliteit bitsnelheid coderen. Niet alle clientapparaten ondersteunen echter de profielen van uw activa en de bitsnelheid. Oudere Android-apparaten ondersteunt bijvoorbeeld alleen H.264 basislijn + AACL. Hogere bitsnelheid sturen naar een telefoon waarop de voordelen is niet gevonden, verspild bandbreedte en apparaat berekenen. Deze apparaat moet decoderen alle opgegeven informatie, alleen wilt schalen omlaag voor weergave.

U kunt apparaatprofielen maken met dynamische bestandenlijst, zoals uw mobiele telefoon, console, HD/SD, enzovoort, maar ook de nummers en kenmerken die u wilt geen deel uitmaken van elk profiel.

 
![Voorbeeld van de afbeeldingsweergave filteren][renditions2]

Een encoder is in het volgende voorbeeld wordt gebruikt voor het coderen van een activum mezzanine in zeven ISO MP4s video weergaven (van 180p naar 1080p). De gecodeerde activa kunt dynamisch worden verpakt tot een van de volgende streaming protocollen: HLS, vloeiend, MPEG-streepje en schijven.  Aan de bovenkant van het diagram, het manifest HLS voor de activa zonder filters wordt weergegeven (bevat alle zeven weergaven).  In de linkerbenedenhoek naar links, wordt het HLS manifest waarop een filter met de naam "ott" is toegepast weergegeven. Hiermee geeft u het filter 'ott' als u wilt verwijderen, alle bitsnelheid onder 1 Mbps, die ontstaan in onder twee kwaliteitsniveaus worden verwijderd uit in de reactie.  In de linkerbenedenhoek rechts, wordt het HLS manifest waarop een filter met de naam 'mobiel' is toegepast weergegeven. Hiermee geeft u het filter 'mobiel' als u wilt verwijderen, waar de resolutie groter is dan 720p, die ontstaan in de twee weergaven 1080p weergaven worden verwijderd uit.

![Afbeeldingsweergave filteren][renditions1]

##<a name="removing-language-tracks"></a>Verwijderen taal nummers

Uw activa bevatten mogelijk meerdere audio talen zoals Engels, Spaans, Frans, enzovoort. Normaal gesproken de managers Windows Media Player SDK audionummer standaardselectie en beschikbaar audio wordt bijgehouden per Gebruikersselectie. Het is lastige ontwikkelen van dergelijke Player SDK's, er verschillende implementaties moeten over apparaat-specifieke speler-kaders. Daarnaast Player-API's op sommige platforms, kennis zijn beperkt en Neem audio selectie functie waar gebruikers niet selecteren of wijzigen van de standaard-nummer niet. Met filters voor activa, kunt u het gedrag bepalen door te maken van filters die alleen de gewenste audio talen omvatten.

![Taal nummers filteren][language_filter]


##<a name="trimming-start-of-an-asset"></a>Het inkorten van het begin van activa 

In de meeste live streaming gebeurtenissen uitgevoerd operatoren sommige tests vóór de werkelijke gebeurtenis. Ze kunnen bijvoorbeeld een leeg project als volgt vóór het begin van de gebeurtenis: 'Programma begint tijdelijk'. Als het programma is archiveren, worden de test en slate-gegevens ook worden gearchiveerd en worden opgenomen in de presentatie. Deze gegevens moet worden echter niet aan de clients weergegeven. U kunt met dynamische bestandenlijst maken van een begin-tijdfilter en verwijder de ongewenste gegevens uit het manifest.

![Het inkorten van het starten][trim_filter]

##<a name="creating-sub-clips-views-from-a-live-archive"></a>Submenu clips (weergaven) maken vanuit een live archief

Veel live gebeurtenissen zijn te lang uitgevoerd en persoonlijke archief, bevatten mogelijk meerdere gebeurtenissen. Na de live gebeurtenis uiteinden omroeporganisaties mogelijk wilt opheffen het persoonlijke archief in logische programma starten en stoppen reeksen. Vervolgens publiceren afzonderlijk deze virtuele programma's zonder bericht verwerking van het persoonlijke archief en geen afzonderlijke activa (dat krijgt geen voordeel van de bestaande in de cache fragmenten in de CDN's) te maken. Voorbeelden van dergelijke virtuele programma's (onderliggend clips) zijn de kwartalen van een kampioenschap of basketbal spel, de innings in honkbalcompetitie of afzonderlijke gebeurtenissen van een namiddag van Olympische Spelen programma.

U kunt met dynamische bestandenlijst, filters met begin/einde tijden maken en virtuele weergaven maken boven aan uw persoonlijke archief. 

![Subclip filter][subclip_filter]

Gefilterde activa:

![Skiën][skiing]

##<a name="adjusting-presentation-window-dvr"></a>Presentatievenster (DVR) aanpassen

Azure Media Services biedt momenteel, ronde archief waarop de duur kan worden geconfigureerd tussen 5 minuten - 25 uur. Manifest filteren kan worden gebruikt voor het maken van een lopende DVR venster boven aan het archief, zonder media verwijderen. Zijn er veel scenario's waarin omroeporganisaties wilt bieden een beperkte DVR venster welke verplaatst met de live rand en tegelijkertijd een groter archivering venster behouden. Een tv-station mogelijk wilt u de gegevens die buiten het venster DVR clips markeren gebruiken of he\she wilt bieden verschillende DVR vensters voor verschillende apparaten. Grootste deel van de mobiele apparaten verwerken niet zo groot DVR windows (u kunt een venster van de DVR 2 minuten voor mobiele apparaten en 1 uur voor bureaubladclients hebben).

![DVR-venster][dvr_filter]

##<a name="adjusting-livebackoff-live-position"></a>Aanpassen LiveBackoff (live positie)

Manifest filteren kan worden gebruikt voor enkele seconden verwijderen uit de live rand van een live programma. Hierdoor omroeporganisaties om de presentatie op het punt van de publicatie voorbeeld bekijken en te maken advertentie invoegpositie punten voordat de viewers de stream ontvangen (meestal back-uit 30 seconden). Omroeporganisaties vervolgens push deze advertenties naar hun kaders client in tijd te ontvangen en de gegevens voordat u de verkoopkans advertentie.

Naast de advertentie-ondersteuning, kan LiveBackoff worden gebruikt voor het aanpassen van client live downloaden positie, zodat wanneer clients nemen en klik op de live rand kan nog steeds worden verkregen fragmenten van server in plaats van 404 of 412 HTTP-fouten krijgt.

![livebackoff_filter][livebackoff_filter]


##<a name="combining-multiple-rules-in-a-single-filter"></a>Meerdere regels in één filter combineren

U kunt meerdere filteren regels in één filter combineren. U kunt een regel bereik slate verwijderen uit een persoonlijke archief en ook filteren beschikbaar bitsnelheid definiëren als voorbeeld. Voor meerdere filteren regels is het eindresultaat de samenstelling (alleen snijpunt) van deze regels.

![meerdere regels][multiple-rules]

##<a name="create-filters-programmatically"></a>Filters maken via programmacode

Het volgende onderwerp wordt beschreven hoe Media Services entiteiten die betrekking op filters hebben. Het onderwerp ziet ook hoe u via programmacode filters maken.  

[Filters maken met REST API's](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Meerdere filters (filter samenstelling) combineren

U kunt ook meerdere filters in een enkel URL combineren. 

De volgende scenario laat zien waarom u wilt mogelijk filters combineren:

1. Moet u uw video eigenschappen voor mobiele apparaten, zoals Android- of iPAD filteren (om te beperken video eigenschappen). Als u wilt verwijderen de ongewenste eigenschappen, maakt u een globale filter die geschikt voor apparaatprofielen is. Bovengenoemde, kunnen de globale filters worden gebruikt voor alle activa onder de dezelfde media services-account zonder verdere koppeling. 
2. Wilt u ook de begin- en -tijd van activa bijsnijden. Daartoe zou u een lokale filter maken en de begin-/ eindtijd instellen. 
3. U wilt combineren beide van deze filters (zonder combinatie moet u aan het inkorten van het filter waardoor filter gebruik moeilijk toevoegen kwaliteit filteren).

Als u wilt combineren filters, moet u de filternamen instellen aan de manifest/afspeellijst URL met puntkomma als scheidingsteken. Stel dat u hebt een filter met de naam *MyMobileDevice* dat filters kwaliteit en u een andere benoemde *MyStartTime* voor het instellen van een bepaalde begintijd hebben. U kunt deze combineren als volgt:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

U kunt maximaal 3 filters combineren. 

Zie voor meer informatie [in dit](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog.


##<a name="know-issues-and-limitations"></a>Weten problemen en beperkingen

- Dynamische manifest werkt in GOP grenzen (toets Frames), dus inkorten GOP nauwkeurigheid heeft. 
- U kunt dezelfde filternaam voor lokale en algemene filters gebruiken. Houd er rekening mee dat lokale filter hebben een hogere prioriteit en wordt overschreven door globale filters.
- Als u een filter bijwerkt, kan het maximaal 2 minuten duren voordat streaming eindpunt te vernieuwen van de regels. Als de inhoud is aangeboden door middel van sommige filters (en met cache in proxy's en CDN cache), kan het bijwerken van deze filters resulteren in speler fouten. Is het aanbevolen om de cache leeg na het bijwerken van het filter. Als deze optie niet mogelijk is, kunt u overwegen een ander filter.


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Zie ook

[Inhoud aan klanten overzicht te bieden](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
 