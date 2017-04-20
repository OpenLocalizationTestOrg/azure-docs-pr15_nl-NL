<properties 
    pageTitle="Toepassingen van de videospeler ontwikkelen" 
    description="Het onderwerp bevat koppelingen naar speler kaders en -invoegtoepassingen die u gebruiken kunt voor het ontwikkelen van uw eigen clienttoepassingen die streaming media, van Media Services kunnen gebruiken." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="develop-video-player-applications"></a>Toepassingen van de videospeler ontwikkelen

##<a name="overview"></a>Overzicht

Azure Media Services biedt de hulpmiddelen die u maken van krachtige, dynamische speler clienttoepassingen voor de meeste platforms moet, waaronder: iOS-apparaten, Android-apparaten, Windows, Windows Phone, Xbox en Set-top vakken. In dit onderwerp bevat ook koppelingen naar SDK's en -speler kaders die u gebruiken kunt voor het ontwikkelen van uw eigen clienttoepassingen die streaming media, van Azure Media Services kunnen gebruiken.

##<a name="azure-media-player"></a>Azure MediaPlayer

[Azure Media Player](http://aka.ms/ampinfo) is een web-videospeler ingebouwd om af te spelen media-inhoud uit Microsoft Azure Media Services op een groot aantal browsers en apparaten. Azure mediaspeler gebruikt industrienormen, zoals HTML5, Media bron extensies (MSE) en versleuteld Media extensies (EME) een uitgebreide geavanceerde streaming ervaring te bieden. Wanneer deze normen niet beschikbaar op een apparaat of in een browser zijn, Azure Media Player Flash en Silverlight gebruikt als fallback technologie. Ongeacht de technologie afspelen, hebben ontwikkelaars een geïntegreerd JavaScript-interface voor toegang tot de API's. Hiermee aangeboden door Azure Media Services inhoud moet worden afgespeeld in een hele-scala van apparaten en browsers zonder eventuele extra inspanning.

Microsoft Azure Media Services kunnen inhoud moet worden aangeboden met streepje, vloeiende Streaming en HLS streaming indelingen afspelen van inhoud. Azure Media Player rekening te houden met deze verschillende indelingen en automatisch wordt afgespeeld de beste koppeling op basis van de mogelijkheden platform/browser. Microsoft Azure Media Services kunnen ook voor dynamische codering van activa met PlayReady versleuteling of AES-128-bits envelop-codering. Azure Media Player kunt decoderen van PlayReady en AES-128-bit versleutelde inhoud wanneer correct geconfigureerd. 

Voor meer informatie:

- [Azure MediaPlayer](http://aka.ms/ampinfo)
- [Azure Media Player documentatie](http://aka.ms/ampdocs) 
- [Azure MediaPlayer aan de slag Blog](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
- [Aanmelden voor het up-to-date houden met de meest recente van Azure Media Player](http://aka.ms/ampsignup)
- [Nieuwe functieverzoeken verzenden, ideeën, feedback toevoegen](http://aka.ms/ampuservoice ) 


##<a name="other-tools-for-creating-player-applications"></a>Andere hulpmiddelen voor het maken van speler-toepassingen

U kunt ook een van de volgende SDK's gebruiken:

- [Vloeiend Streaming Client SDK](http://www.iis.net/downloads/microsoft/smooth-streaming) 
- [Vloeiende Streaming Windows Store-App](media-services-build-smooth-streaming-apps.md)
- [Microsoft Media-Platform: Speler Framework](http://playerframework.codeplex.com/) 
- [HTML5 Speler Framework documentatie](http://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
- [Microsoft-vloeiend Plug Streaming voor OSMF](https://www.microsoft.com/download/details.aspx?id=36057) 
- [Volumelicenties Microsoft® vloeiende Streaming Client Kit overdragen](http://aka.ms/sspk) 
- [Ontwikkeling van de Video toepassingen XBOX](http://xbox.create.msdn.com/) 
 

##<a name="advertising"></a>Reclame

Azure Media Services biedt ondersteuning voor advertenties opnemen via het Windows Media-Platform: speler kaders. Speler kaders met ad-ondersteuning zijn beschikbaar voor Windows 8, Silverlight, Windows Phone 8 en iOS-apparaten. Elke speler framework bevat steekproef-code die u hoe ziet u het implementeren van een toepassing voor de speler. Er zijn drie verschillende soorten advertenties die kunt u uw media invoegen:

Lineair – volledige frame advertenties die de belangrijkste video onderbreken

Niet-lineair – overlay advertenties die worden weergegeven terwijl de belangrijkste video wordt afgespeeld, meestal een logo of andere statische afbeelding in de speler geplaatst

Companion – advertenties die buiten de speler worden weergegeven

Advertenties kunnen worden geplaatst op een willekeurige plaats in de tijdlijn van de belangrijkste video. U moet de speler zien wanneer om af te spelen de ad en welke advertenties om af te spelen. Dit is voltooid met een set standaard XML-bestanden: Video Ad-Service-sjabloon (VAST), digitale Video meerdere Ad afspeellijst (VMAP) Media abstracte volgordebepaling sjabloon (MAST) en digitale Video Player Ad Interface definitie (VPAID). GROTE bestanden opgeven welke advertenties om weer te geven. VMAP bestanden opgeven wanneer u verschillende advertenties afspelen en VAST XML bevatten. MAST bestanden zijn een andere manier om sequentie advertenties die ook VAST XML kan bevatten. VPAID bestanden definiëren een interface tussen de videospeler en de ad of Active Directory-server. Zie [Advertenties invoegen](https://msdn.microsoft.com/library/dn387398.aspx)voor meer informatie.

Zie [ondersteund gesloten ondertiteling en Ad invoegpositie standaarden](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad)voor informatie over gesloten ondertiteling en advertenties-ondersteuning in Live streaming van video's.


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zie ook

[Een MPEG-streepje geavanceerde Streaming van Video insluiten in een toepassing HTML5 met DASH.js](media-services-embed-mpeg-dash-in-html5.md)

[GitHub dash.js opslagplaats](https://github.com/Dash-Industry-Forum/dash.js)
 
