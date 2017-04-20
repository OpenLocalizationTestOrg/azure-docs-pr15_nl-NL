<properties 
    pageTitle="Overzicht van de beveiligen | Microsoft Azure" 
    description="In dit artikel geeft een overzicht van de beveiliging van inhoud met Media-Services." 
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
    ms.date="09/27/2016" 
    ms.author="juliako"/>

#<a name="protecting-content-overview"></a>Overzicht van de beveiligen


Microsoft Azure Media Services kunt u uw media vanaf het moment vanaf uw computer via opslag, verwerking en bezorging secure. Media-Services kunt u voor het leveren van uw live en op aanvraag inhoud dynamisch versleuteld met geavanceerde versleuteling Standard (AES) (via 128-bit versleuteling toetsen) of een van de belangrijkste DRMs: Microsoft PlayReady, Google Widevine en Apple FairPlay. Media-Services ook biedt een service voor het bezorgen van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties geautoriseerde clients. 

De volgende afbeelding ziet u de inhoudsbeveiliging werkstromen die ondersteuning biedt voor AMS. 

![Beveiligen met PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[AZURE.NOTE]Als u wilt kunnen dynamische versleuteling, moet u eerst ten minste één eenheid voor streaming gereserveerde ophalen op het streaming eindpunt waaruit u wilt streamen versleutelde inhoud.

In dit onderwerp wordt uitgelegd [concepten en terminologie](media-services-content-protection-overview.md) relevant zijn voor informatie over inhoudsbeveiliging met AMS. Het onderwerp bevat ook [koppelingen](media-services-content-protection-overview.md#common-scenarios) naar onderwerpen waarin wordt aangegeven hoe bereiken inhoudsbeveiliging taken. 

##<a name="dynamic-encryption"></a>Dynamische versleuteling

Microsoft Azure Media Services kunt u voor het leveren van uw inhoud dynamisch versleuteld met AES wissen key of DRM versleuteling: Microsoft PlayReady, Google Widevine en Apple FairPlay.

Op dit moment kunt u de volgende streaming indelingen coderen: HLS, MPEG-streepje en vloeiende Streaming. U kunt geen schijven streaming format versleutelen of progressief downloads.

Als u voor Media Services wilt voor het coderen van activa, moet u een versleutelingssleutel (CommonEncryption of EnvelopeEncryption) koppelen aan uw activum en ook configureren autorisatie beleidsregels voor de sleutel.

U moet ook van de activa bezorging beleid configureren. Als u een versleutelde activum opslag streamen wilt, moet u opgeven hoe u wilt dat door het configureren van activa bezorging beleid.

Een stroom door een speler is aangevraagd, wordt de opgegeven sleutel door Media Services dynamisch versleutelen uw met behoud van AES wissen key of DRM versleuteling gebruikt. Als u wilt versleutelen de stream, wordt de speler de toets vraag aan de belangrijkste bezorgingsservice. Om te bepalen of de gebruiker is gemachtigd om de toets, evalueert de service de autorisatie beleidsregels die u hebt opgegeven voor de sleutel.

>[AZURE.NOTE]Om te profiteren van dynamische versleuteling, moet u eerst ten minste één op aanvraag streaming eenheid voor het streaming eindpunt waaruit u wilt gaan bezorging uw versleutelde inhoud ophalen. Lees [hoe u de schaal Media Services](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

##<a name="storage-encryption"></a>Opslag-versleuteling

Opslag versleuteling versleutelen wissen inhoud lokaal met AES 256 bitsversleuteling en uploadt dit naar Azure Storage waar deze zijn opgeslagen in rust versleuteld. Activa die zijn beveiligd met opslag versleuteling zijn automatisch niet versleuteld en geplaatst in een versleutelde bestandssysteem vóór codering en desgewenst opnieuw worden versleuteld voordat u het uploadt weer als een nieuwe uitvoer actief. De primaire use-case voor opslag-versleuteling is wanneer u wilt beveiligen van uw hoge kwaliteit voor de invoer mediabestanden met codering in rust op schijf.

Voor het leveren van een versleutelde activum opslag, moet u van het actief bezorging beleid configureren zodat Media Services weet hoe u wilt uw inhoud te geven. Voordat uw activa kan worden streamen, wordt de streaming server Hiermee verwijdert u de opslag-versleuteling en streamt uw met behoud van het opgegeven bezorging beleid (bijvoorbeeld AES, algemene versleuteling of geen codering).

## <a name="common-encryption-cenc"></a>Algemene versleuteling (CENC)

Algemene versleuteling wordt gebruikt om de inhoud met PlayReady te versleutelen of / en Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Cbcs-aapl-versleuteling gebruikt

Cbcs-aapl wordt gebruikt om de inhoud met FairPlay te versleutelen.

## <a name="envelope-encryption"></a>Envelop-versleuteling 

Gebruik deze optie als u wilt beveiligen van uw inhoud met wissen AES-128-toets. Als u wilt dat de optie voor een beter kunt beveiligen, kiest u een van de DRMs vermeld in dit onderwerp. 

##<a name="licenses-and-keys-delivery-service"></a>Bezorgingsservice licenties en sleutels

Media Services biedt een service voor het leveren van DRM (PlayReady, Widevine, FairPlay)-licenties en AES toetsen met geautoriseerde clients wissen. U kunt [de Azure-portal](media-services-portal-protect-content.md), REST API of Media Services SDK voor .NET autorisatie en verificatie beleidsregels voor uw licenties en sleutels configureren.

##<a name="token-restriction"></a>Token beperking

Het autorisatiebeleid belangrijke voor inhoud kan zijn een of meer autorisatie-beperkingen: openen of token beperking. Het beleid voor token beperkte vergezeld van een token dat is uitgegeven door een Secure Token Service (STS). Media Services ondersteunt tokens in de eenvoudige Web Tokens (SWT) en indeling van JSON Web Token (JWT). Media Services biedt geen Secure Token Services. U kunt een aangepaste STS maken of gebruikmaken van Microsoft Azure ACS naar probleem tokens. De STS moeten worden geconfigureerd voor het maken van een token ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie token beperking. De belangrijkste bezorgingsservice Media Services wordt de gevraagde sleutel (of de licentie) terug naar de client als het token geldig is en de claims in de token overeenkomen die geconfigureerd voor het sleutel (of de licentie).

Wanneer het beleid voor het configureren van de token worden beperkt, moet u de primaire verificatie-toets, de uitgever en de doelgroep parameters. De primaire verificatie-sleutel bevat de sleutel die het token is ondertekend met, uitgever is de beveiligde token service die het token problemen. Het publiek (ook wel bereik genoemd) worden de bedoeling het token of de resource het token gemachtigd voor toegang tot. De belangrijkste bezorgingsservice Media Services is gevalideerd dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

##<a name="streaming-urls"></a>Streaming URL 's

Als uw activa is versleuteld met meer dan één DRM, moet u een tag versleuteling gebruiken in de streaming URL: (opmaak = 'm3u8-aapl', versleuteling = 'lengte').

De volgende punten zijn van toepassing:

- Alleen nul of één versleutelingstype kan worden opgegeven.
- Coderingstype hoeft te worden opgegeven in de url als er slechts één versleuteling op de activa is toegepast.
- Coderingstype is hoofdlettergevoelig.
- De volgende coderingstypen u kunt opgeven:  
    - **cenc**: algemene codering (Playready of Widevine)
    - **cbcs-aapl**: Fairplay
    - **CBC**: AES envelop-versleuteling gebruikt.

##<a name="common-scenarios"></a>Gebruikelijke scenario 's

De volgende onderwerpen laten zien hoe u inhoud in opslag beveiligen, geven dynamisch versleutelde streaming media, AMS sleutel/licentie bezorgingsservice gebruiken

- [Beveiligen met AES](media-services-protect-with-aes128.md) 
- [Beveiligen met PlayReady en/of Widevine](media-services-protect-with-drm.md)
- [Uw inhoud beveiligde HLS met Apple FairPlay en/of PlayReady streamen](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>Extra scenario 's

- [Het integreren van Azure PlayReady License-service met uw eigen coderen/streaming-server](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
- [CastLabs moet geven DRM licenties voor Azure Media Services gebruiken](media-services-castlabs-integration.md)
 
##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Verwante koppelingen

[PlayReady aankondigen als een service en dynamische AES-versleuteling gebruikt met Azure Media Services](http://mingfeiy.com/playready)

[Uitleg over Azure Media Services PlayReady licentie bezorging prijzen](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Hoe u fouten opsporen in voor de versleutelde stream AES in Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT token authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integreren Azure Media Services OWIN MVC op basis van de app met Azure Active Directory en belangrijke contentlevering op basis van claims JWT beperken](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Gebruik Azure ACS naar probleem tokens](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
