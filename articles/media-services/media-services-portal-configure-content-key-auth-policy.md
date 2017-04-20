<properties 
    pageTitle="Inhoud toets autorisatiebeleid met behulp van de Azure portal configureren | Microsoft Azure" 
    description="Leer hoe u een autorisatiebeleid inhoud sleutels configureren." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="juliako"/>



#<a name="configure-content-key-authorization-policy"></a>Inhoud belangrijke autorisatiebeleid configureren
[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]


##<a name="overview"></a>Overzicht

Microsoft Azure Media Services kunt u voor het leveren van de MPEG-streepje, vloeiende Streaming en HTTP-Live-Streaming (HLS) streams die zijn beveiligd met geavanceerde versleuteling Standard (AES) (via 128-bit versleuteling toetsen) of [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS kunt u ook voor het leveren van streepje streams versleuteld met Widevine DRM. PlayReady zowel Widevine zijn versleuteld per specificatie van de algemene versleuteling (CENC ISO/IEC 23001 tot en met 7).

Media Services bevat ook een **Toets/licentie bezorgingsservice** waaruit clients AES-sleutels of PlayReady/Widevine licenties voor het afspelen van de versleutelde inhoud kunnen ophalen.

Dit onderwerp wordt uitgelegd hoe u met de portal van Azure het autorisatiebeleid inhoud belangrijke configureren. De toets kan later worden gebruikt voor het coderen van dynamisch uw inhoud. Momenteel kunt versleutelen de volgende streaming notitie indelingen: HLS, MPEG-streepje en vloeiende Streaming. U kunt geen schijven streaming format versleutelen of progressief downloads.

Als een speler een stream die is ingesteld aanvraagt op dynamisch worden gecodeerd, gebruikt Media Services de geconfigureerde sleutel dynamisch versleutelen uw inhoud AES of DRM-versleuteling gebruikt. Als u wilt versleutelen de stream, wordt de speler de toets vraag aan de belangrijkste bezorgingsservice. Om te bepalen of de gebruiker is gemachtigd om de toets, evalueert de service de autorisatie beleidsregels die u hebt opgegeven voor de sleutel.


Als u van plan bent meerdere inhoud toetsen of de URL van een **Sleutel/licentie bezorgingsservice** dan de belangrijkste bezorgingsservice Media Services wilt opgeven, gebruikt u Media Services .NET SDK of REST API's.

[Inhoud toets autorisatiebeleid met behulp van Media Services .NET SDK configureren](media-services-dotnet-configure-content-key-auth-policy.md)

[Inhoud toets autorisatiebeleid met Media Services REST API configureren](media-services-rest-configure-content-key-auth-policy.md)

###<a name="some-considerations-apply"></a>Enkele punten van toepassing:

- Als u wilt kunnen dynamische verpakking en dynamische versleuteling gebruiken, moet u ervoor zorgen dat ten minste één streaming gereserveerde per eenheid. Zie [hoe u de schaal van een Media-Service](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.
- Uw activa moet een reeks geavanceerde bitsnelheid MP4s of geavanceerde bitsnelheid vloeiende Streaming-bestanden bevatten. Zie [coderen activa](media-services-encode-asset.md)voor meer informatie.
- De toets bezorgingsservice slaat ContentKeyAuthorizationPolicy en de bijbehorende gerelateerde objecten (beleidsopties en beperkingen) gedurende 15 minuten.  Als u een ContentKeyAuthorizationPolicy maken en geef een "Token" beperking worden gebruikt, test deze, en klikt u vervolgens het beleid bijwerken naar 'Openen' beperking, duurt het ongeveer 15 minuten voordat het beleid schakelt u naar de "Geopende" versie van het beleid.


##<a name="how-to-configure-the-key-authorization-policy"></a>Hoe: het autorisatiebeleid belangrijke configureren

De belangrijkste als autorisatiebeleid wilt configureren, selecteer de pagina **INHOUDSBEVEILIGING** .

Media Services ondersteunt meerdere manieren gebruikers die belangrijke aanvragen te verifiëren. Het autorisatiebeleid inhoud belangrijke kunt **openen**, **token**of beperkingen voor **IP-** autorisatie (**IP** kan worden geconfigureerd met REST of .NET SDK) hebben.

###<a name="open-restriction"></a>Open beperking

De beperking **open** betekent dat het systeem de sleutel voor iedereen die een belangrijke aanvraag gaat geven. Deze beperking kan handig zijn voor testdoeleinden.

![OpenPolicy][open_policy]

###<a name="token-restriction"></a>Token beperking

Om het token beperkte beleid te selecteren, drukt u op de knop **TOKEN** .

Het **token** beperkte beleid vergezeld van een token uitgegeven door een **Secure Token Service** (STS). Media Services ondersteunt tokens in de **Eenvoudige Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) en **JSON Web Token** (JWT)-indeling. Zie voor informatie [JWT token-verificatie](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Services biedt geen **Secure Token Services**. U kunt een aangepaste STS maken of gebruikmaken van Microsoft Azure ACS naar probleem tokens. De STS moeten worden geconfigureerd voor het maken van een token ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie token beperking. De belangrijkste bezorgingsservice Media Services wordt de sleutel terug naar de client als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de inhoud sleutel. Zie [Gebruik Azure ACS naar probleem tokens](http://mingfeiy.com/acs-with-key-services)voor meer informatie.

Bij het configureren van de **TOKEN** beperkt beleid, moet u de waarden voor **verificatie-toets**, **uitgever** en **publiek**instellen. De primaire verificatie-sleutel bevat de sleutel die het token is ondertekend met, uitgever is de beveiligde token service die het token problemen. Het publiek (ook wel bereik genoemd) worden de bedoeling het token of de resource het token gemachtigd voor toegang tot. De belangrijkste bezorgingsservice Media Services is gevalideerd dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

###<a name="playready"></a>PlayReady

Wanneer de inhoud met **PlayReady**beveiligt, is een van de dingen die u wilt opgeven in het autorisatiebeleid voor een XML-tekenreeks die de PlayReady licentie sjabloon definieert. Het volgende beleid is standaard ingesteld:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <LicenseType>Nonpersistent</LicenseType>
          <PlayRight>
            <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
          </PlayRight>
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

U kunt op de knop **beleid xml importeren** en geef een verschillende XML dat voldoet aan de XML-Schema gedefinieerd [hier](https://msdn.microsoft.com/library/azure/dn783459.aspx).


##<a name="next-step"></a>Volgende stap

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

