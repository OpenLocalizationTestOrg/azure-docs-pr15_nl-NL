<properties 
    pageTitle="Azure Media Services concepten | Microsoft Azure" 
    description="Dit onderwerp biedt een overzicht van Azure Media Services concepten" 
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

#<a name="azure-media-services-concepts"></a>Azure Media Services concepten 

Dit onderwerp biedt een overzicht van de belangrijkste Media Services concepten.

##<a id="assets"></a>Activa en opslag

###<a name="assets"></a>Activa

Een [activum](https://msdn.microsoft.com/library/azure/hh974277.aspx) bevat digitale bestanden (inclusief video, audio, afbeeldingen, miniaturen verzamelingen, tekst nummers en ondertiteling bestanden) en de metagegevens over deze bestanden. Nadat de digitale bestanden worden geüpload naar een actief, kunnen ze worden gebruikt in het Media-Services versleuteling en de streaming van werkstromen.

Activa is toegewezen aan een container blob in de opslag van Azure-account en de bestanden in de activa worden opgeslagen als BLOB's in dat onderdeel.

Bij het kiezen welke mediainhoud voor uploaden en bewaren in activa, zijn de volgende punten van toepassing:

- Activa mogen alleen een unieke exemplaar van media-inhoud bevatten. Bijvoorbeeld een enkel bewerken van een TV aflevering, film of advertentie.
- Activa mogen geen meerdere weergaven of bewerkingen van een audiovisuele-bestand. Een voorbeeld van een onjuiste gebruik van de activa zou probeert meer dan één TV aflevering, advertentie of meerdere camerahoeken uit een enkel binnen activa op te slaan. Meerdere weergaven of bewerkingen van een audiovisuele-bestand opslaan in een actief kan resulteren in problemen die u verzendt codering taken, streaming en beveiligen van de bezorging van de activa later in de werkstroom.  

###<a name="asset-file"></a>Activa-bestand 
Een [AssetFile](https://msdn.microsoft.com/library/azure/hh974275.aspx) vertegenwoordigt een werkelijke video of audio-bestand dat is opgeslagen in een container blob. Een activum-bestand is altijd gekoppeld aan activa en activa een of meer bestanden kan bevatten. De taak Media Services Encoder mislukt als een activum bestand-object niet gekoppeld aan een digitale bestand in een container blob is.

Het exemplaar **AssetFile** en het werkelijke mediabestand zijn in twee afzonderlijke objecten. Het exemplaar AssetFile bevat metagegevens over het mediabestand, terwijl het mediabestand de werkelijke media-inhoud bevat.

U moet niet proberen om de inhoud van blob containers die zijn gegenereerd door Services Media zonder Media Service API's te wijzigen.

###<a name="asset-encryption-options"></a>Opties voor activa-versleuteling

Afhankelijk van het type inhoud die u wilt uploaden, opslaan en geven, vindt Media Services u verschillende versleutelingsopties voor die u kunt kiezen uit.

**Geen** Geen versleuteling wordt gebruikt. Dit is de standaardwaarde. Houd er rekening mee dat wanneer u deze optie gebruikt de inhoud niet is beveiligd tijdens overdracht of op rest in opslag.

Als u van plan bent om uit te voeren een MP4 met geleidelijke downloaden, gebruik deze optie uw inhoud te uploaden.

**StorageEncrypted** – met deze optie kunt u uw wissen met behoud van lokaal AES 256 bits codering versleutelen en uploadt dit naar Azure Storage waar deze zijn opgeslagen in rust versleuteld. Activa die zijn beveiligd met opslag versleuteling zijn automatisch niet versleuteld en geplaatst in een versleutelde bestandssysteem vóór codering en desgewenst opnieuw worden versleuteld voordat u het uploadt weer als een nieuwe uitvoer actief. De primaire use-case voor opslag-versleuteling is wanneer u wilt beveiligen van uw hoge kwaliteit voor de invoer mediabestanden met codering in rust op schijf. 

Voor het leveren van een versleutelde activum opslag, moet u van het actief bezorging beleid configureren zodat Media Services weet hoe u wilt uw inhoud te geven. Voordat uw activa kan worden streamen, wordt de streaming server Hiermee verwijdert u de opslag-versleuteling en streamt uw met behoud van het opgegeven bezorging beleid (bijvoorbeeld AES, PlayReady of geen codering). 

**CommonEncryptionProtected** - Gebruik deze optie als u wilt versleutelen (of uploaden al gecodeerd) inhoud met algemene versleuteling of PlayReady DRM (bijvoorbeeld vloeiende Streaming beveiligd met DRM PlayReady).

**EnvelopeEncryptionProtected** – Gebruik deze optie als u wilt beveiligen (of uploaden al beschermd) HTTP Live Streaming (HLS) versleuteld met geavanceerde versleuteling Standard (AES). Houd er rekening mee dat als u al zijn versleuteld met AES HLS uploaden wilt, deze moet wel zijn versleuteld met transformeren Manager.

###<a name="access-policy"></a>-Beleid 

Een [AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx) definieert machtigingen (zoals lezen, schrijven en lijst) en duur van access naar activa. Normaal zou u een object AccessPolicy doorgeven aan een locator die vervolgens worden gebruikt voor toegang tot de bestanden in een actief.


###<a name="blob-container"></a>BLOB container

Een container blob biedt een groepering van een set BLOB's. BLOB containers worden gebruikt in Media Services als grens punt voor toegangsbeheer, en gedeeld Access handtekening (SA's) Locator op activa. Een account Azure Storage kan een onbeperkt aantal blob containers bevatten. Een onbeperkt aantal BLOB's kan worden opgeslagen in een container.

>[AZURE.NOTE]U moet niet proberen om de inhoud van blob containers die zijn gegenereerd door Services Media zonder Media Service API's te wijzigen.

###<a id="locators"></a>Locator

[Locator](https://msdn.microsoft.com/library/azure/hh974308.aspx)s een ingangspunt voor toegang tot de bestanden in een actief opgeven. Een access-beleid wordt gebruikt om de machtigingen en duur dat een client toegang tot een bepaald activum heeft te definiëren. Locator kunnen beschikken over een veel-op een access-beleid, zodat verschillende Locator bieden kunnen verschillende begin- en verbindingstypen verschillende clients terwijl alle met de dezelfde machtiging en de instellingen van de duur; u kunt echter meer dan vijf unieke Locator die is gekoppeld aan een bepaald activum in één keer geen vanwege de beperking van een gedeelde toegang beleid is ingesteld door Azure storage-services. 

Media Services ondersteunt twee soorten Locator: OnDemandOrigin Locator, gebruikt om te streamen media (bijvoorbeeld MPEG streepje, HLS of vloeiende Streaming) of geleidelijk downloaden media en SA's URL Locator, die is gebruikt om te uploaden of downloaden van media-bestanden to\from Azure opslag. 

Houd er rekening mee dat de machtiging (AccessPermissions.List) niet worden gebruikt bij het maken van een locator OrDemandOrigin. 

###<a name="storage-account"></a>Opslag-account

Alle toegang tot Azure Storage vindt plaats via een opslag-account. Een Media-account kunt koppelen aan een of meer opslagruimte-accounts. Een account kan een onbeperkt aantal containers, bevatten, zolang de totale grootte onder 500TB per opslag-account.  Media Services biedt SDK niveau tooling zodat u kunt meerdere opslag beheren en taakverdeling de verdeling van uw activa tijdens uploaden naar deze accounts op basis van de doelstellingen of willekeurige distributie. Zie Werken met [Azure-opslag](https://msdn.microsoft.com/library/azure/dn767951.aspx)voor meer informatie. 

##<a name="jobs-and-tasks"></a>Projecten en taken

Een [taak](https://msdn.microsoft.com/library/azure/hh974289.aspx) wordt meestal gebruikt om te proces (bijvoorbeeld, index of coderen) één presentatie met audio-of videogesprek. Als u meerdere video's verwerkt, maakt u een taak voor elke video om te worden gecodeerd.

Een taak bevat metagegevens over de verwerking moet worden uitgevoerd. Elke taak bevat een of meer [taak](https://msdn.microsoft.com/library/azure/hh974286.aspx)s die uitvoer van een verwerkingstaak atomaire, de invoer activa, activa, een Mediaprocessor en de bijbehorende instellingen opgeven. Taken binnen een taak kunnen worden geschakeld, waar de activa uitvoer van een taak wordt uitgedrukt als de invoer actief tot de volgende taak. Op deze manier kunt één taak alle de verwerking nodig voor een presentatie media bevatten.

##<a id="encoding"></a>Coderen

Azure Media Services biedt meerdere opties voor het coderen van media in de cloud.

Wanneer u begint met Media-Services, is het belangrijk dat u het verschil tussen codecs en bestandsindelingen.
Codecs zijn de software waarmee de algoritmen compressie/decompressie dat bestandsindelingen containers waarin de gecomprimeerde video.

Media Services biedt dynamische verpakking waarmee u voor het leveren van de inhoud van het geavanceerde bitsnelheid MP4 of vloeiende Streaming codering in door Services Media (MPEG streepje, HLS, vloeiende Streaming, schijven) Encoder ondersteunde streaming zonder dat u hoeft opnieuw inpakken in deze indelingen streaming.

Om te profiteren van [dynamische verpakking](media-services-dynamic-packaging-overview.md), moet u als volgt te werk:

- Coderen uw mezzanine (bron)-bestand in een reeks geavanceerde bitsnelheid MP4-bestanden of geavanceerde bitsnelheid vloeiende Streaming-bestanden (de codering stappen worden verderop in deze zelfstudie gedemonstreerd).
- Ten minste één op aanvraag streaming eenheid voor het streaming eindpunt waaruit u wilt gaan bezorging van uw inhoud downloaden. Lees [hoe u de schaal op aanvraag Streaming gereserveerde eenheden](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

Media Services ondersteunt de volgende handelingen uit op aanvraag encoders die worden beschreven in dit artikel:

- [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
- [Media Encoder Premium werkstroom](media-services-encode-asset.md#media-encoder-premium-workflow)

Zie voor informatie over ondersteunde encoders [Encoders](media-services-encode-asset.md).


##<a name="live-streaming"></a>Live Streaming

In Azure Media Services vertegenwoordigt een kanaal een pijplijn voor het verwerken van live streaming inhoud. Een kanaal ontvangt live invoer gegevensstromen op twee manieren:

- Een on-premises implementatie live encoder verzendt multi-bitrate RTMP of vloeiende Streaming (gefragmenteerd MP4) naar het kanaal. U kunt de volgende live encoders die uitvoer multi-bitrate vloeiende Streaming: vorm van het element, Envivio, zoals Cisco. De volgende live encoders uitvoer RTMP: Adobe Flash Live, Telestream Wirecast en Tricaster transcoders. De geconsumeerde streams worden verzonden via kanalen zonder verdere verwerking. Wanneer een aangevraagd, biedt Media Services de stream naar klanten.

- Een enkel bitsnelheid (in een van de volgende indelingen: RTP (MPEG-TS)), RTMP of vloeiende Streaming (gefragmenteerd MP4)) wordt verzonden naar het kanaal dat is ingesteld voor het uitvoeren van live coderen met Media-Services. Het kanaal voert live codering van de binnenkomende één bitsnelheid op een multi-(geavanceerde) video bitsnelheid. Wanneer een aangevraagd, biedt Media Services de stream naar klanten.

###<a name="channel"></a>Kanaal

In Media Services zijn [kanaal](https://msdn.microsoft.com/library/azure/dn783458.aspx)s verantwoordelijk voor het verwerken van live streaming inhoud. Een kanaal biedt een invoer eindpunt (nemen URL) dat u vervolgens naar een live transcoder opgeven. Het kanaal live invoer gegevensstromen ontvangt van de live transcoder en beschikbaar zijn voor het streamen via een of meer StreamingEndpoints. Kanalen bieden ook een voorbeeld-eindpunt (voorbeeld-URL) die u kunt een voorbeeld bekijken en de stream valideren voordat nadere verwerking en de bezorging van.

U kunt ophalen de URL ingest en de preview-URL wanneer u het kanaal maakt. Als u deze URL's, moet het kanaal niet worden gestart. Wanneer u klaar om te beginnen om gegevens uit een live transcoder in het kanaal bent, kan het kanaal moet worden gestart. Wanneer het live transcoder wordt gestart ingesting gegevens, kunt u uw stream bekijken.

Elk Media Services-account kan bevatten meerdere kanalen, meerdere programma's en meerdere StreamingEndpoints. Afhankelijk van de behoeften bandbreedte en beveiliging kunnen StreamingEndpoint services worden toegewezen aan een of meer kanalen. Een StreamingEndpoint kunt ophalen uit een kanaal.


###<a name="program"></a>Programma

Een [programma](https://msdn.microsoft.com/library/azure/dn783463.aspx) kunt u de publicatie en opslag van segmenten in een live gegevensstroom. Kanalen beheren programma's. De relatie kanaal en hetzelfde programma is vergelijkbaar met traditionele media waar een kanaal heeft een constante stroom van inhoud en een programma is beperkt tot enkele vervaging gebeurtenis op dat kanaal.
U kunt het aantal uren die u behouden de opgenomen inhoud voor het programma wilt door de eigenschap **ArchiveWindowLength** opgeven. Deze waarde kan worden ingesteld van een minimum van 5 minuten tot maximaal 25 uur.

ArchiveWindowLength bepaalt ook dat de maximale hoeveelheid tijd clients terug in tijd kunt zoeken vanaf de huidige live positie. Programma's kunnen worden uitgevoerd op de opgegeven tijdsduur, maar de inhoud die achter de lengte van het venster valt continu worden verwijderd. Deze waarde van deze eigenschap wordt ook bepaalt hoe lang de client-manifesten te vergroten.

Elk programma is gekoppeld aan een actief. Als u wilt publiceren van het programma, moet u een locator voor de bijbehorende activa maken. Met deze locator, kunt u de URL van een streaming dat u naar uw klanten kan leveren maken.

Een kanaal ondersteunt maximaal drie gelijktijdig actieve programma's, zodat u kunt meerdere archieven van de dezelfde binnenkomende stream maken. Hiermee kunt u publiceren en archiveren van verschillende gedeelten van een gebeurtenis naar wens. Het vereiste voor uw bedrijf is bijvoorbeeld 6 uur van een programma archiveren, maar alleen laatste 10 minuten uitzenden. Hiervoor moet u twee gelijktijdig actieve programma's te maken. Een programma is ingesteld op het archiveren van 6 uur van de gebeurtenis, maar het programma niet wordt gepubliceerd. De andere programma's is ingesteld op archiveren voor 10 minuten en dit programma is gepubliceerd.


Zie voor meer informatie:

- [Werken met kanalen die voor het uitvoeren van Live coderen met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)
- [Werken met kanalen die Live Multi-bitsnelheid van On-premises Encoders ontvangen](media-services-live-streaming-with-onprem-encoders.md)
- [Quota en beperkingen](media-services-quotas-and-limitations.md).

##<a name="protecting-content"></a>Inhoud beveiligen

###<a name="dynamic-encryption"></a>Dynamische versleuteling

Azure Media Services kunt u uw media vanaf het moment vanaf uw computer via opslag, verwerking en bezorging secure. Media Services kunt u de inhoud dynamisch versleuteld met geavanceerde versleuteling Standard (AES) (via 128-bit versleuteling toetsen) en algemene codering (CENC) met PlayReady en/of Widevine DRM bezorgen. Media Services bevat ook een service om AES-sleutels en PlayReady licenties aan gemachtigde clients te bieden.

Op dit moment kunt u de volgende streaming indelingen coderen: HLS, MPEG-streepje en vloeiende Streaming. U kunt geen schijven streaming format versleutelen of progressief downloads.

Als u voor Media Services wilt voor het coderen van activa, moet u een versleutelingssleutel (CommonEncryption of EnvelopeEncryption) koppelen aan uw activum en ook configureren autorisatie beleidsregels voor de sleutel.

Als u een versleutelde activum opslag streamen wilt, moet u van het actief bezorging beleid configureren om aan te geven hoe voor het leveren van de activa.

Een stroom door een speler is aangevraagd, gebruikt Media Services de opgegeven sleutel dynamisch versleutelen uw met behoud van een envelop-codering (met AES) of algemene codering (met PlayReady of Widevine). Als u wilt versleutelen de stream, wordt de speler de toets vraag aan de belangrijkste bezorgingsservice. Om te bepalen of de gebruiker is gemachtigd om de toets, evalueert de service de autorisatie beleidsregels die u hebt opgegeven voor de sleutel.


###<a name="token-restriction"></a>Token beperking

Het autorisatiebeleid belangrijke voor inhoud kan zijn een of meer autorisatie-beperkingen: open, token beperking of IP-beperking. Het beleid voor token beperkte vergezeld van een token dat is uitgegeven door een Secure Token Service (STS). Media Services ondersteunt tokens in de eenvoudige Web Tokens (SWT) en indeling van JSON Web Token (JWT). Media Services biedt geen Secure Token Services. U kunt een aangepaste STS maken of gebruikmaken van Microsoft Azure ACS naar probleem tokens. De STS moeten worden geconfigureerd voor het maken van een token ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie token beperking. De belangrijkste bezorgingsservice Media Services wordt de gevraagde sleutel (of de licentie) terug naar de client als het token geldig is en de claims in de token overeenkomen die geconfigureerd voor het sleutel (of de licentie).

Wanneer het beleid voor het configureren van de token worden beperkt, moet u de primaire verificatie-toets, de uitgever en de doelgroep parameters. De primaire verificatie-sleutel bevat de sleutel die het token is ondertekend met, uitgever is de beveiligde token service die het token problemen. Het publiek (ook wel bereik genoemd) worden de bedoeling het token of de resource het token gemachtigd voor toegang tot. De belangrijkste bezorgingsservice Media Services is gevalideerd dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

Zie de volgende artikelen voor meer informatie:

[Beveiligen met een overzicht van de](media-services-content-protection-overview.md)
[beveiligen met AES-128](media-services-protect-with-aes128.md)
[beveiligen met DRM](media-services-protect-with-drm.md)

##<a name="delivering"></a>Geven

###<a id="dynamic_packaging"></a>Dynamische verpakking

Wanneer u werkt met wordt het wordt aanbevolen voor het coderen van uw mezzanine-bestanden in een geavanceerde bitsnelheid MP4 Media-Services instellen en vervolgens de set converteren naar de gewenste indeling met de [Dynamische verpakking](media-services-dynamic-packaging-overview.md).


###<a name="streaming-endpoint"></a>Streaming eindpunt

Een StreamingEndpoint vertegenwoordigt een streaming service die inhoud kunt geven rechtstreeks naar een speler clienttoepassing of naar een inhoud bezorging netwerk (CDN) voor verdere distributie (Azure Media Services biedt nu de integratie van Azure CDN.) De uitgaande stroom van een StreamingEndpoint-service kan zijn een live gegevensstroom of een video op aanvraag activa in uw Media Services-account. Bovendien kunt u de capaciteit van de service StreamingEndpoint worden afgehandeld groeiende bandbreedte behoeften door aan te passen schaaleenheden (ook wel bekend als streaming eenheden) bepalen. Het wordt aanbevolen een of meer schaaleenheden voor toepassingen in productieomgeving toewijzen. Schaaleenheden bieden u speciale egress capaciteit die kan worden aangeschaft in stappen van 200 Mbps zowel extra functionaliteit waaronder momenteel gebruik dynamische verpakking.

Het wordt aanbevolen versleuteling dynamische verpakking en/of dynamische. Als u wilt deze functies gebruikt, moet u ten minste één streaming eenheid voor het eindpunt van waaruit u van plan bent om te streamen hebben. Zie de [schaal streaming eenheden](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.

U kunt maximaal 2 streaming eindpunten in uw Media Services-account hebben al dan niet standaard. Zie [quota en beperkingen](media-services-quotas-and-limitations.md)voor het aanvragen van een hogere limiet.

Afschrijving alleen wanneer uw StreamingEndpoint in staat uitgevoerd.

###<a name="asset-delivery-policy"></a>Activa bezorging beleid

Een van de stappen in de Media Services contentlevering werkstroom is [Bezorging beleidsregels voor activa ](https://msdn.microsoft.com/library/azure/dn799055.aspx)die u wilt worden streamen configureren. Het beleid van de bezorging van activa wordt Media Services uitgelegd hoe u uw activum wilt is bezorgd: in welke streaming protocol moet uw activa worden dynamisch verpakt (voor bijvoorbeeld MPEG streepje, HLS, vloeiende Streaming of alle), al dan niet u wilt versleutelen dynamisch uw activa en hoe (envelop of algemene versleuteling).

Als er een versleutelde activa opslag voordat uw activa kan worden streamen, wordt de streaming server Hiermee verwijdert u de opslag-versleuteling en uw met behoud van het beleid opgegeven bezorging streamt. Bijvoorbeeld, om uw activa versleuteld met geavanceerde versleuteling Standard (AES) versleutelingssleutel, het beleidstype ingesteld op DynamicEnvelopeEncryption. Als u wilt opslag versleuteling verwijderen en de activa in de wissen streamen, Stel in het type op NoDynamicEncryption.

###<a name="progressive-download"></a>Geleidelijke downloaden

Geleidelijke downloaden kunt u beginnen met het afspelen van media voordat het hele bestand is gedownload. U kunt alleen geleidelijk een MP4-bestand downloaden.

Houd er rekening mee dat u versleutelde activa ontsleutelen moet als u deze wilt beschikbaar is voor geleidelijke downloaden.

Als gebruikers wilt voorzien geleidelijke downloaden URL's, moet u eerst een locator OnDemandOrigin maken. U maakt de locator, kunt u het grondtal pad naar de activa. U moet de naam van MP4-bestand toe te voegen. Bijvoorbeeld:

http://amstest1.streaming.mediaservices.Windows.NET/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

###<a name="streaming-urls"></a>Streaming URL 's

Uw inhoud naar clients streaming. Als gebruikers wilt voorzien streaming van URL's, moet u eerst een locator OnDemandOrigin maken. U maakt de locator, kunt u het grondtal pad naar de activa met de inhoud die u wilt streamen. De mogelijkheid om te streamen van dit inhoudstype moet u echter dit pad verder wijzigen. Als u wilt samenstellen volledige URL naar het bestand dat streaming manifest, moet u van de locator padwaarde en het manifest (filename.ism) concatenate bestandsnaam. /Manifest en (indien nodig) de juiste indeling vervolgens toevoegen aan het pad locator.

U kunt ook uw inhoud streamen via een SSL-verbinding. Zorg dat de URL van uw streaming beginnen met HTTPS hiervoor.

Houd er rekening mee dat u alleen via SSL streamen kunt als het streaming eindpunt waaruit u de inhoud bezorgen na 10 September 2014 is gemaakt. Als de URL van uw streaming zijn gebaseerd op de streaming eindpunten na September 10 hebt gemaakt, bevat de URL 'streaming.mediaservices.windows.net' (de nieuwe indeling). Streaming URL's met 'origin.mediaservices.windows.net' (de oude notatie) worden niet ondersteund voor SSL. Als de URL in de oude indeling is en u wilt mogelijk om te streamen via SSL, maakt u een nieuw streaming-eindpunt. URL's die zijn gemaakt op basis van het nieuwe streaming endpoint gebruiken om te streamen van uw inhoud via SSL.

De volgende lijst wordt beschreven verschillende streaming notaties en voorbeelden:

- Vloeiende Streaming

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest


- MPEG-STREEPJE

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=MPD-time-csf)



- Apple HTTP Live Streaming (HLS) V4

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)



- Apple HTTP Live Streaming (HLS) V3

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

- Schijven (voor Adobe WIP/Access houders)

{streaming eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=f4m-F4F)


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
