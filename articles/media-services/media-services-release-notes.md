<properties 
    pageTitle="Releaseopmerkingen voor Media Services | Microsoft Azure" 
    description="Media Services releaseopmerkingen" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="media" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>

# <a name="azure-media-services-release-notes"></a>Releaseopmerkingen voor Azure Media Services

Deze releaseopmerkingen samenvatten wijzigingen uit eerdere versies en bekende problemen.

>[AZURE.NOTE] We wilt krijgen van onze klanten en richten op problemen oplossen die betrekking hebben op u. Als u wilt rapporteren van een probleem of vragen, post u in het [Azure Media Services MSDN-Forum].


##<a id="issues"></a>Bekende problemen

### <a id="general_issues"></a>Problemen met het algemeen van Media Services.

Probleem|Beschrijving
---|---
Verschillende veelvoorkomende HTTP-headers niet beschikbaar in de REST API.|Als u Media Services met behulp van de REST API ontwikkelen, vindt u die enkele veelvoorkomende HTTP-berichtvelden (met inbegrip van CLIENT-aanvraag-ID, aanvraag-ID en RETURN-CLIENT-aanvraag-ID) worden niet ondersteund. De koppen worden, toegevoegd in een toekomstige update.
Percentagecodering is niet toegestaan.|Media Services gebruikt de waarde van de eigenschap IAssetFile.Name bij het maken van URL's voor de streaming inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Daarom is percentagecodering niet toegestaan. De waarde van de eigenschap **Name** geen van de volgende [tekens percentage-codering-gereserveerd](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Bovendien kunnen alleen er een '.' voor de bestandsnaamextensie.
De methode ListBlobs die deel uitmaakt van de Azure opslag SDK versie 3.x mislukt.|Media Services genereert SA's URL's op basis van de [2012-02-12](http://msdn.microsoft.com/library/azure/dn592123.aspx) -versie. Als u gebruiken van Azure opslag SDK aan lijst BLOB's in een container blob wilt, gebruikt u de methode [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) die deel uitmaakt van Azure opslag SDK versie 2.x. De methode ListBlobs die deel uitmaakt van de versie van Azure opslag SDK 3.x, mislukt.
Media-Services om beperken beperkt het gebruik van de bronnen voor toepassingen die overtollige verzoek in de service aanbrengt. De service kan de Service niet beschikbaar (503) HTTP-statuscode retourneren.|Zie de beschrijving van de 503 HTTP-statuscode in het onderwerp [Foutcodes van Azure Media Services](http://msdn.microsoft.com/library/azure/dn168949.aspx) voor meer informatie.
Query's naar entiteiten, moet u er een limiet van 1000 entiteiten in één keer wordt geretourneerd, omdat openbare REST v2 queryresultaten tot 1000 resultaten beperkt is. | U moet gebruiken **overslaan** en **uitvoeren** (.NET) / **boven** (REST) als beschreven in [dit voorbeeld .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) en [in dit voorbeeld REST API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
Bij sommige clients kunnen afkomstig zijn via een probleem herhaald tag in het manifest vloeiende Streaming.|Zie voor meer informatie [in deze](media-services-deliver-content-overview.md#known-issues) sectie.
Azure Media Services .NET SDK objecten kunnen niet worden omgezet, en daardoor niet werken met Caching van Azure.|Als u probeert het object SDK AssetCollection toe te voegen aan het Caching van Azure serialiseren, wordt een uitzondering opgetreden.
Codering taken mislukken met een tekenreeks bericht "fase: DownloadFile. Code: System.NullReferenceException ".|De normale versleuteling werkstroom is invoer video bestanden uploaden naar invoer activa en dien een of meer codering taken voor de invoer activa, zonder verder kan worden gewijzigd activa invoer. Echter als u de invoer actief (bijvoorbeeld door toevoegen/verwijderen/bestandsnamen binnen de activa) wijzigt, kunnen klikt u vervolgens volgende taken mislukken met een fout DownloadFile. De oplossing is het verwijderen van de invoer activa en invoer bestanden die naar een nieuwe activum opnieuw te uploaden. 

##<a id="rest_version_history"></a>REST API versiegeschiedenis

Zie voor informatie over de versiegeschiedenis van Media Services REST API [Azure Media Services REST API verwijzing].

##<a id="july_changes16"></a>Juli 2016 Release

###<a name="updates-to-manifest-file-ism-generated-by-encoding-tasks"></a>Updates voor de bestandenlijst bestand (*. Console) die zijn gegenereerd door het coderen van taken

Wanneer een codering taak wordt verzonden naar Media Encoder standaard of Azure Media Encoder, de codering taak genereert een [streaming manifest-bestand](media-services-deliver-content-overview.md) (* .ism) bestand in de uitvoer van activa. Met de meest recente versie van de service, is de syntaxis van deze streaming manifest-bestand bijgewerkt.

>[AZURE.NOTE]De syntaxis van de streaming manifest (.ism)-bestand is gereserveerd voor interne gebruik en kan in toekomstige versies worden gewijzigd. Neem wijzigen of de inhoud van dit bestand bewerken.

###<a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Een nieuwe client manifest (*. ISMC)-bestand is gegenereerd in de uitvoer activa wanneer de taak in een codering Hiermee kunt u een of meer MP4-bestanden

Beginnen met de meest recente versie van de service, na de voltooiing van een taak in een codering die een genereert meer MP4-bestanden, de uitvoer bevat van activa ook een streaming client manifest (*.ismc)-bestand. Het bestand .ismc helpt te verbeteren de prestaties van het dynamische streaming. 

>[AZURE.NOTE]De syntaxis van de client manifest (.ismc)-bestand is gereserveerd voor interne gebruik en kan in toekomstige versies worden gewijzigd. Neem wijzigen of de inhoud van dit bestand bewerken.

Zie voor meer informatie [in dit](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) blog.

### <a name="known-issues"></a>Bekende problemen

Bij sommige clients kunnen afkomstig zijn aross een probleem Herhaaltoetsen tag in het manifest vloeiende Streaming. Zie voor meer informatie [in deze](media-services-deliver-content-overview.md#known-issues) sectie.

##<a id="apr_changes16"></a>April 2016 Release

### <a name="azure-media-analytics"></a>Azure Media-analyses

Azure Media Servces geïntroduceerd Azure Media Analytics voor krachtige video intelligence. Zie [Azure Media Services Analytics overzicht](media-services-analytics-overview.md)voor gedetailleerde informatie.

### <a name="apple-fairplay-preview"></a>Apple FairPlay (Preview)

Azure Media Services kunt u nu uw HTTP Live Streaming (HLS) inhoud met Apple FairPlay dynamisch versleutelen. U kunt ook AMS licentie bezorgingsservice gebruiken voor het leveren van FairPlay licenties aan clients. Zie [Gebruik Azure Media Services om te streamen uw HLS inhoud beveiligde met Apple FairPlay ](media-services-protect-hls-with-fairplay.md)voor meer informatie.
  
##<a id="feb_changes16"></a>Februari 2016 Release

De nieuwste versie van Azure Media Services SDK voor .NET (3.5.3) bevat een Widevine gerelateerde bug oplossing. Het probleem is: AssetDeliveryPolicy opnieuw voor meerdere activa versleuteld met Widevine kan niet worden gebruikt. Als onderdeel van deze fout correctie de volgende eigenschap is toegevoegd aan de SDK: **WidevineBaseLicenseAcquisitionUrl**.
    
    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},
        
    };

##<a id="jan_changes_16"></a>Release januari 2016

Gereserveerde eenheden codering gewijzigd in verwarring met Encoder namen verkleinen.

De namen van de Basic, Standard en Premium codering gereserveerde eenheden worden gewijzigd naar S1, S2 en S3 eenheden, respectievelijk gereserveerd.  Klanten die gebruikmaken van eenvoudige codering RUs vandaag S1 zien als het label in Azure-Portal (en in de factuur), terwijl standaard en Premium respectievelijk de etiketten S2 en S3 wilt zien. 

##<a id="dec_changes_15"></a>December 2015 Release

Het team van Azure SDK gepubliceerd een nieuwe versie van de [Azure SDK voor PHP](http://github.com/Azure/azure-sdk-for-php) -pakket dat updates en nieuwe functies voor Microsoft Azure Media Services bevat. De SDK van Azure Media Services voor PHP ondersteunt met name nu de nieuwste functies van [inhoudsbeveiliging](media-services-content-protection-overview.md) : dynamische versleuteling met AES en DRM (PlayReady en Widevine) met en zonder beperking Token. Ondersteunt ook schaalbaarheid [Codering eenheden](media-services-dotnet-encoding-units.md).

Zie voor meer informatie:

- De [SDK van Microsoft Azure Media Services voor PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) -blog.
- Snel in de volgende [voorbeelden van programmacode](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) kunt u aan de slag:
    - **vodworkflow_aes.php**: dit is een PHP-bestand dat wordt uitgelegd hoe u dynamische AES-128-versleuteling en toets bezorgingsservice. Dit is gebaseerd op de .NET-steekproef die in [Dit](media-services-protect-with-aes128.md) artikel worden beschreven.
    - **vodworkflow_aes.php**: dit is een PHP-bestand dat wordt uitgelegd hoe u PlayReady dynamische versleuteling en licentie bezorgingsservice. Dit is gebaseerd op de .NET-steekproef die in [Dit](media-services-protect-with-drm.md) artikel worden beschreven.
    - **scale_encoding_units.php**: dit is een PHP-bestand dat ziet u hoe u de schaal van codering gereserveerde eenheid.


##<a id="nov_changes_15"></a>November 2015 Release

Azure Media Services biedt nu Google Widevine licentie bezorgingsservice in de cloud. Raadpleeg [deze aankondiging-blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/)voor meer informatie. Zie ook [deze zelfstudie](media-services-protect-with-drm.md) en [GitHub opslagplaats](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Houd er rekening mee dat Widevine licentie bezorging services van Azure Media Services wordt uitgevoerd in de proefversie van is. Zie voor meer informatie [in dit blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

##<a id="oct_changes_15"></a>Oktober 2015 Release

Azure Media Services (AMS) is nu live in de volgende datacenters: Brazilië Zuid, India West, India Zuid en India centraal. U kunt nu de Azure klassieke Portal gebruiken om [Media-Service-accounts maken](media-services-portal-create-account.md) en uitvoeren van verschillende taken beschreven [hier](https://azure.microsoft.com/documentation/services/media-services/). Live-codering is niet ingeschakeld in deze datacenters. Verder, zijn niet alle typen gereserveerde eenheden codering beschikbaar in deze datacenters.

- Brazilië Zuid: Alleen standaard en eenvoudige codering gereserveerde eenheden beschikbaar
- India West, India Zuid en India centraal: alleen eenvoudige codering gereserveerde eenheden beschikbaar zijn


##<a id="september_changes_15"></a>September 2015 Release 

- AMS biedt nu de mogelijkheid om zowel Video-On-Demand (VOD) en Live gegevensstromen met Widevine modulaire DRM technologie te beschermen. U kunt de volgende bezorging services partners gebruiken om te leveren Widevine licenties: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Zie voor meer informatie [in dit blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).

    U kunt [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (beginnend met de versie 3.5.1) of REST API gebruiken voor het configureren van uw AssetDeliveryConfiguration als u wilt gebruiken, Widevine.  

- AMS toegevoegd ondersteuning voor Apple ProRes video's. U kunt nu de QuickTime-video's bronbestanden die gebruikmaken van Apple ProRes of andere codecs uploaden. Zie voor meer informatie [in dit blog](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).

- Nu kunt u Media Encoder standaard sub knippen en persoonlijke archief extractie van doen. Zie voor meer informatie [in dit blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

- De volgende filteren updates zijn aangebracht: 

    - U kunt nu Apple HTTP Live Streaming (HLS) opmaken met alleen geluid filter. Deze update kunt u alleen geluid bijhouden verwijderen door op te geven (alleen geluid = ONWAAR) in de URL.
    - Bij het definiëren van filters voor uw activa, hebt u nu de mogelijkheid om meerdere combineren (maximaal 3) filters in een enkel URL.

    Zie voor meer informatie [in dit](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog.

- AMS ondersteunt nu ik-Frames in HLS v4. I-Frame ondersteuning optimaliseert vooruitspoelen en terugspoelen bewerkingen. Standaard zijn alle HLS v4 uitvoer I-Frame afspeellijst (EXT-X-I-FRAME-STREAM-INF).
 
    Zie voor meer informatie [in dit](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog.

##<a id="august_changes_15"></a>Augustus 2015 Release

- Azure Media Services SDK voor Java V0.8.0 release en nieuwe voorbeelden zijn nu beschikbaar. Zie voor meer informatie:

    - [Blogbericht](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
    - [Java voorbeelden opslagplaats](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- Azure Media Player bijwerken met ondersteuning voor meerdere audiostroom. Zie voor meer informatie:
    - [Blogbericht](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

##<a id="july_changes_15"></a>Juli 2015 Release

- De algemene beschikbaarheid van Media Encoder standaard aankondigen. Zie voor meer informatie [dit blogbericht](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).

    Media Encoder standaard gebruikmaakt van vooraf ingestelde die in [deze](http://go.microsoft.com/fwlink/?LinkId=618336) sectie worden beschreven. Houd er rekening mee dat wanneer u met behulp van een vooraf ingestelde voor 4k worden gecodeerd, u het type van de eenheid **Premium** gereserveerd ontvangt. Lees [hoe u het schaal codering](media-services-scale-media-processing-overview.md)voor meer informatie.
- Realtime bijschriften maken met Azure Media Services en een speler draaien of spiegelen Zie voor meer informatie [dit blogbericht](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK Updates

Azure Media Services .NET SDK is nu versie 3.4.0.0. De volgende functionaliteit is toegevoegd in deze release:  

- Geïmplementeerd ondersteuning voor persoonlijke archief. Houd er rekening mee dat u activa met een live archief niet downloaden.
- Geïmplementeerd ondersteuning voor dynamische filters.
- Geïmplementeerd functionaliteit waarmee gebruikers kunnen opslag container houden bij het verwijderen van de activa.
- Correcties gerelateerd aan het beleid in kanalen opnieuw.
- **Media Encoder Premium werkstroom**ingeschakeld.

##<a id="june_changes_15"></a>Juni 2015 Release

###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK Updates

Azure Media Services .NET SDK is nu versie 3.3.0.0. De volgende functionaliteit is toegevoegd in deze release:  

- ondersteuning voor OpenId verbinding Discovery-specificatie
- ondersteuning voor het verwerken van toetsen rollover op identiteit provider kant. 

Als u een identiteitsprovider waarmee discovery-document OpenID verbinding gebruikt (zoals de volgende voorzieningen doen: Azure Active Directory, Google, Salesforce), kunt u opdracht geven Azure Media Services voor Certificaatondertekening toetsen voor validatie van JWT token uit OpenID discovery specificatie. 

Zie [Json Web-sneltoetsen met behulp van discovery-specificatie OpenID verbinding maken voor gebruik met JWT token verificatie in Azure Media Services](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/)voor meer informatie.


##<a id="may_changes_15"></a>Versie van mei 2015

Aankondiging van de volgende nieuwe functies:

- [Een voorbeeld van Live coderen met Media-Services](media-services-manage-live-encoder-enabled-channels.md)
- [Dynamische manifest](media-services-dynamic-manifest-overview.md)
- [Een voorbeeld van Azure Media Hyperlapse Mediaprocessor](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

##<a id="april_changes_15"></a>Release april 2015

 ###<a name="general-media-services-updates"></a>Algemeen Media Services-Updates

- [Aankondigen Azure MediaPlayer](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
- Beginnen met Media Services REST 2.10, kanalen die zijn geconfigureerd voor het nemen van een protocol RTMP zijn gemaakt met primair en secundair nemen URL's. Zie voor meer informatie [kanaal nemen configuraties](media-services-live-streaming-with-onprem-encoders.md#channel_input)
- Azure Media indexering-updates
- Ondersteuning voor Spaans
- Nieuwe configuratie XML-indeling

Zie voor meer informatie [in dit blog](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).
###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK Updates

Azure Media Services .NET SDK is nu versie 3.2.0.0.

Hier volgen enkele van de klant die tegenover elkaar liggen updates:

- **Verbreken wijzigen**: **TokenRestrictionTemplate.Issuer** en **TokenRestrictionTemplate.Audience** een type tekenreeks gewijzigd.
- Updates van alles voor het maken van aangepaste beleidsregels voor het opnieuw.
- Correcties die betrekking hebben op bestanden uploaden/downloaden.
- De klasse **MediaServicesCredentials** accepteert nu controle-eindpunt verifiëren van primaire en secundaire access.



##<a id="march_changes_15"></a>Release maart 2015

### <a name="general-media-services-updates"></a>Algemeen Media Services-Updates

- Media Services biedt nu Azure CDN-integratie. Als u wilt de integratie wordt ondersteund, is de eigenschap **CdnEnabled** toegevoegd aan **StreamingEndpoint**.  **CdnEnabled** kan worden gebruikt met REST API's vanaf versie 2,9 (Zie [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx)voor meer informatie).  **CdnEnabled** kan worden gebruikt met .NET SDK vanaf versie 3.1.0.2 (Zie [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint(v=azure.10).aspx)voor meer informatie).
- Aankondiging van **Media Encoder Premium werkstroom**. Zie [Inleiding tot Premium codering in Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)voor meer informatie.
 


##<a id="february_changes_15"></a>Release februari 2015

### <a name="general-media-services-updates"></a>Algemeen Media Services-Updates

Media Services REST API is nu versie 2.9. Beginnen met deze versie, kunt u de Azure CDN-integratie met streaming eindpunten inschakelen. Zie [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx)voor meer informatie.

##<a id="january_changes_15"></a>Januari 2015 Release

### <a name="general-media-services-updates"></a>Algemeen Media Services-Updates

Aankondiging van algemene beschikbaarheid (GA) van de beveiliging van inhoud met dynamische versleuteling. Zie [Azure Media Services verbetert de streaming beveiliging met algemene beschikbaarheid van DRM technologie](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/)voor meer informatie.

###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK Updates

Azure Media Services .NET SDK is nu versie 3.1.0.1.

Deze release dat de standaardconstructor Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate als verouderd is gemarkeerd. De nieuwe constructor wordt TokenType als een argument.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


##<a id="december_changes_14"></a>Versie van december 2014

###<a name="general-media-services-updates"></a>Algemeen Media Services-Updates

- Sommige updates en nieuwe functies zijn toegevoegd aan de processor Azure indexering Media. Zie [Azure Media-versie voor indexering 1.1.6.7 releaseopmerkingen](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/)voor meer informatie.
- Een nieuwe REST API waarmee u kunt bijwerken codering gereserveerd eenheden toegevoegd: [EncodingReservedUnitType met REST](http://msdn.microsoft.com/library/azure/dn859236.aspx).
- Toegevoegde CORS ondersteuning voor belangrijke bezorgingsservice.
- Prestatieverbeteringen van query's uitvoeren autorisatie beleidsopties waren.
- In China Datacenter, dat de [Sleutel bezorging URL](http://msdn.microsoft.com/library/azure/ef4dfeeb-48ae-4596-ab28-44d6b36d8769#get_delivery_service_url) is nu per klant (net als op andere datacenters).
- Toegevoegde HLS automatisch doel duur. Bij het uitvoeren van live streaming, wordt dynamisch HLS altijd geleverd. Standaard berekent Media Services automatisch HLS segment verpakking verhouding (FragmentsPerSegment) op basis van het interval hoofdframe (KeyFrameInterval), ook wel groep van afbeeldingen – GOP, die is ontvangen van de Live encoder genoemd. Zie [werken met Azure Media Services Live Streaming]voor meer informatie.
 
###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK Updates

- [Azure Media Services.NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) is nu versie 3.1.0.0.
- De afhankelijkheid .net SDK bijgewerkt naar .NET 4.5 Framework.
- Een nieuwe API waarmee u kunt het bijwerken van codering gereserveerde eenheden toegevoegd. Zie [Type gereserveerde eenheid bijwerken en codering RUs verhogen met behulp van .NET](http://msdn.microsoft.com/library/azure/jj129582.aspx)voor meer informatie.
- Toegevoegde JWT (JSON Web Token) ondersteuning voor token-verificatie. Zie [JWT token verificatie in Azure Media Services en dynamische versleuteling](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)voor meer informatie.
- Toegevoegde relatieve verschuivingen voor BeginDate en ExpirationDate in de sjabloon PlayReady-licentie.


##<a id="november_changes_14"></a>Versie van november 2014

- Media-Services kunt u nu een live vloeiende Streaming (FMP4) inhoud via een SSL-verbinding nemen. Zorg dat u de URL ingest bijwerken naar HTTPS om op te nemen via SSL.  Voor meer informatie over live streaming, raadpleegt u [werken met Azure Media Services Live Streaming].
- Houd er rekening mee dat momenteel, u kunt geen een live gegevensstroom RTMP via een SSL-verbinding nemen.
- U kunt ook uw inhoud streamen via een SSL-verbinding. Zorg dat de URL van uw streaming beginnen met HTTPS hiervoor.
- Houd er rekening mee dat u alleen via SSL streamen kunt als het streaming eindpunt waaruit u de inhoud bezorgen na 10 September 2014 is gemaakt. Als de URL van uw streaming zijn gebaseerd op de streaming eindpunten na September 10 hebt gemaakt, bevat de URL 'streaming.mediaservices.windows.net' (de nieuwe indeling). Streaming URL's met 'origin.mediaservices.windows.net' (de oude notatie) worden niet ondersteund voor SSL. Als de URL in de oude indeling is en u wilt mogelijk te streamen via SSL, [Maak een nieuwe streaming eindpunt](media-services-portal-manage-streaming-endpoints.md). URL's die zijn gemaakt op basis van het nieuwe streaming endpoint gebruiken om te streamen van uw inhoud via SSL.

##<a id="october_changes_14"></a>Versie van oktober 2014

### <a id="new_encoder_release"></a>Media Services Encoder Release

De nieuwe versie van Media Services Azure Media Encoder aankondigen. Met de meest recente Azure Media Encoder alleen wordt geheven voor uitvoer GB, maar anders de nieuwe encoder is compatibel met de vorige encoder functie. Voor meer informatie [Media Services prijzen Details]).

### <a id="oct_sdk"></a>Media-Services .NET SDK 

Media Services SDK voor .NET Extensions is nu versie 2.0.0.3.

Media Services SDK voor .NET is nu versie 3.0.0.8.

De volgende wijzigingen zijn aangebracht:

- Refactoring opnieuw beleid lessen.
- Tekenreeks gebruikersagent toevoegen aan HTTP-verzoek headers.
- Toe te voegen nuget herstellen opbouwen stap.
- Scenario tests als u wilt gebruiken, x509 op te lossen certificaat uit bibliotheek.
- Instellingen valideren bij het bijwerken van kanaal en streaming einde.
 

### <a name="new-github-repository-to-host-media-services-samples"></a>Nieuwe GitHub opslagplaats naar voorbeelden van de Media Services host

Voorbeelden zich bevinden in [Azure Media Services voorbeelden GitHub opslagplaats](https://github.com/Azure/Azure-Media-Services-Samples).


##<a id="september_changes_14"></a>Versie van september 2014

Metagegevens van de REST van Media Services is nu versie 2.7. Zie voor meer informatie over de meest recente updates voor de REST [Azure Media Services REST API verwijzing].

Media Services SDK voor .NET is nu versie 3.0.0.7
 
### <a id="sept_14_breaking_changes"></a>Wijzigingen verbreken

* **Origin** is gewijzigd in [StreamingEndpoint].
* Een wijziging in het standaardgedrag bij gebruik van de **Azure klassieke Portal** coderen en vervolgens publiceren MP4-bestanden.

Eerder, wanneer een MP4 video activum de URL van een SA's van één bestand publiceren met behulp van de Azure klassieke Portal moet worden gemaakt (SA's URL's kunt u de video downloaden van een blobopslag). Op dit moment dat wanneer u de klassieke Azure-Portal gebruikt voor het coderen en vervolgens publiceren een activum van één bestand-MP4-video, de gegenereerde URL verwijst naar een Azure Media Services streaming eindpunt.  Deze wijziging heeft geen invloed op MP4-video's die rechtstreeks zijn geüpload naar Media Services en gepubliceerd zonder Azure Media Services worden gecodeerd.

Op dit moment hebt u de volgende twee opties om het probleem te verhelpen.

* Streaming eenheden inschakelen en dynamische verpakking gebruiken om te streamen van de activa .mp4 als een vloeiende streaming-presentatie.

* Maak de url van een SA's om de .mp4 downloaden (of geleidelijk afspelen). Zie [Inhoud geven]voor meer informatie over het maken van een locator SA's.


### <a id="sept_14_GA_changes"></a>Nieuwe functies/scenario's die deel uitmaken van GA release

* **Indexering Mediaprocessor**. Zie [Mediabestanden indexeren met Azure Media indexering]voor meer informatie.

* De entiteit [StreamingEndpoint] nu kunt u aangepaste domeinnamen (host) toevoegen.

    Voor een aangepaste domeinnaam moet worden gebruikt als de naam van het Media Services streaming, moet u aangepaste hostnamen toevoegen aan uw streaming eindpunt. Gebruik de Media Services REST API's of .NET SDK aangepaste hostnamen toevoegen.
    
    De volgende punten zijn van toepassing:
    
    * U bent de eigenaar bent van de aangepaste domeinnaam.
    
    * De eigenaar bent van de domeinnaam moet worden gevalideerd door Azure Media Services. Valideren van het domein, maak een CName die kaarten <MediaServicesAccountId>.<parent domain> verifydns. < mediaservices-dns-zone >. 
    
    * Moet u nog een CName de aangepaste hostnaam (bijvoorbeeld sports.contoso.com) om uw Media Services-StreamingEndpont host (bijvoorbeeld amstest.streaming.mediaservices.windows.net) toe.


    Zie de eigenschap **CustomHostNames** in het onderwerp [StreamingEndpoint] voor meer informatie.

### <a id="sept_14_preview_changes"></a>Nieuwe functies/scenario's die deel uitmaken van de openbare preview-versie

* Streaming Livevoorbeeld. Zie [werken met Azure Media Services Live Streaming]voor meer informatie.

* Belangrijke bezorgingsservice. Zie [Gebruik dynamische AES-128-versleuteling en bezorgingsservice sleutel]voor meer informatie.

* Dynamische AES-versleuteling. Zie [Gebruik dynamische AES-128-versleuteling en bezorgingsservice sleutel]voor meer informatie.

* Bezorgingsservice PlayReady-licentie. Zie [Gebruik PlayReady dynamische versleuteling en bezorgingsservice licentie]voor meer informatie.

* PlayReady dll-codering. Zie [Gebruik PlayReady dynamische versleuteling en bezorgingsservice licentie]voor meer informatie.

* Media-Services PlayReady licentie sjabloon. Zie [Overzicht van Media Services PlayReady licentie sjabloon]voor meer informatie.

* Opslag streaming versleuteld activa. Zie [Streaming opslag versleuteld inhoud]voor meer informatie.

##<a id="august_changes_14"></a>Versie van augustus 2014

Wanneer u een actief coderen, wordt activa uitvoer geproduceerd na voltooiing van de codering taak. Totdat deze release geproduceerd Azure Media Services Encoder metagegevens over uitvoer activa. Beginnen met deze versie van de encoder produceert ook metagegevens over invoer activa. Zie de onderwerpen [Invoer metagegevens] en [Uitvoer metagegevens] voor meer informatie.


##<a id="july_changes_14"></a>Versie van juli 2014

De volgende correcties zijn aangebracht voor de Azure Media Services Packager en coderen:

* Alleen geluid wordt afgespeeld wanneer een activum persoonlijke archief naar HTTP Live Streaming – transmuxing dit probleem is opgelost en nu zowel audio en video worden afgespeeld.

* Wanneer de verpakking activa HTTP Live Streaming en AES 128-bit envelop versleuteling gebruikt, worden verpakt stromen doen niet afgespeeld op Android-apparaten – deze fout is verholpen en de verpakt stream afgespeeld op Android-apparaten die ondersteuning bieden voor HTTP Live Streaming.

##<a id="may_changes_14"></a>Versie van mei 2014

### <a id="may_14_changes"></a>Algemeen Media Services-Updates

U kunt nu [Dynamische verpakking] naar stream HTTP Live Streaming (HLS) v3. Om te streamen HLS v3, de volgende indeling toevoegen aan het pad van de locator origin: * .ism/manifest(format=m3u8-aapl-v3). Zie [van niek Drouin-Blog]voor meer informatie.

Dynamische verpakking nu ook ondersteunt HLS (v3 en v4) versleuteld met PlayReady leveren op basis van het vloeiende Streaming statisch versleuteld met PlayReady. Zie voor informatie over hoe versleutelen vloeiende Streaming met PlayReady, [beveiligen vloeiende Stream PlayReady].

### <a name="may_14_donnet_changes"></a>Media Services .NET SDK Updates

De volgende verbeteringen zijn opgenomen in de Media Services .NET SDK 3.0.0.5-versie:

* Betere snelheid en flexibiliteit voor het media-activa uploaden/downloaden.

* Verbeteringen in de afhandeling van opnieuw logica en tijdelijk uitzonderingen: 

    * Tijdelijke fout detectie en probeer het opnieuw logica zijn verbeterd voor uitzonderingen die worden veroorzaakt door query's uitvoeren, wijzigingen opslaan, bestanden uploaden of downloaden. 
    
    * Wanneer dat Web uitzonderingen (bijvoorbeeld tijdens een ACS token aanvraag), ziet u dat fatale fouten niet goed sneller nu werkt worden.

Zie [Logica opnieuw in de Media Services SDK voor .NET]voor meer informatie.

##<a id="april_changes_14"></a>April 2014 Encoder Release

### <a name="april_14_enocer_changes"></a>Media Services Encoder Updates

* Ondersteuning voor ingesting AVI bestanden geschreven met behulp van de niet-lineair editor gras Valley EDIUS waar de video licht is gecomprimeerd met gras Valley hoofdkantoor/HQX codec. Zie voor meer informatie [Gras Valley wordt gemeld EDIUS 7 Streaming tot en met de Cloud].

* Ondersteuning voor het opgeven van de naamgevingsconventie op voor de bestanden die met de Media Encoder toegevoegd. Zie [Bepalen Media Service Encoder uitvoer bestandsnamen]voor meer informatie.

* Ondersteuning voor video-en/of audio overlays toegevoegd. Zie [Overlays maken]voor meer informatie.

* Ondersteuning voor wilt samenvoegen meerdere video segmenten. Zie voor meer informatie [Wilt Video segmenten].

* Vaste een fout die betrekking hebben op transcodering van MP4s waar het geluid is gecodeerd met MPEG-1 Audio layer 3 (of MP3).


##<a id="jan_feb_changes_14"></a>Versies van januari/februari 2014

### <a name="jan_fab_14_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.1, 3.0.0.2 en 3.0.0.3

De wijzigingen in 3.0.0.1 en 3.0.0.2 opnemen:

* Vaste problemen met het gebruik van LINQ query's met OrderBy-instructies.

* Gesplitste testoplossingen in [GitHub] in tests per eenheid en testen op basis van een Scenario.

Zie voor meer informatie over de wijzigingen: [Azure Media Services .NET SDK 3.0.0.1 en 3.0.0.2 loslaat].

De volgende wijzigingen zijn aangebracht in 3.0.0.3:

* Bijgewerkte Azure opslag afhankelijkheden versie 3.0.3.0 te gebruiken. 

* Compatibiliteit met eerdere versies het probleem voor 3.0 opgelost. *.* versies. 


##<a id="december_changes_13"></a>Release van december 2013

### <a name="dec_13_donnet_changes"></a>Azure Media Services .NET SDK (3.0.0.0)

>[AZURE.NOTE] versies van 3.0.x.x zijn niet compatibel met 2.4.x.x versies.

De nieuwste versie van de Media Services SDK is nu (3.0.0.0). U kunt de meest recente downloaden uit Nuget of krijgen van de bits van [GitHub].

Beginnen met de Media Services SDK versie (3.0.0.0), kunt u opnieuw gebruiken de tokens [Azure Active Directory Access besturingselement ACS (Service)] . Zie de sectie 'Hergebruiken Access besturingselement Service Tokens' in het [verbinding maken met Media-Services met de Media Services SDK voor .NET] -onderwerp voor meer informatie.

### <a name="dec_13_donnet_ext_changes"></a>Azure Media Services-.NET SDK uitbreidingen 2.0.0.0

Azure Media Services .NET SDK extensies is een verzameling extensie methoden en Help-functies die uw code te vereenvoudigen en kunt u eenvoudiger ontwikkelen met Azure Media Services. U kunt de meest recente bits openen via [Azure Media Services .NET SDK uitbreidingen].

##<a id="november_changes_13"></a>Release november 2013

### <a name="nov_13_donnet_changes"></a>Azure Media Services .NET SDK wijzigingen

Beginnen met deze versie, verwerkt de Media Services SDK voor .NET tijdelijke veroorzaakt fouten die optreden kunnen bij het plaatsen van oproepen aan de laag Media Services REST API.

##<a id="august_changes_13"></a>Augustus 2013 Release

### <a name="aug_13_powershell_changes"></a>PowerShell-Cmdlets voor Media Services is opgenomen in Azure Sdk-programma 's

De volgende Media Services PowerShell-cmdlets worden nu opgenomen in [azure-sdk-hulpmiddelen].

* Get-AzureMediaServices 

    Bijvoorbeeld `Get-AzureMediaServicesAccount`.

* Nieuwe AzureMediaServicesAccount 

    Bijvoorbeeld `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.

* Nieuwe AzureMediaServicesKey 

    Bijvoorbeeld `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.

* Verwijderen AzureMediaServicesAccount 

    Bijvoorbeeld `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

##<a id="june_changes_13"></a>Release juni 2013

### <a name="june_13_general_changes"></a>Azure Media Services wijzigingen

De wijzigingen die worden genoemd in deze sectie zijn opgenomen in de versies van juni 2013 Media Services-updates.

* Mogelijkheid om meerdere opslag-accounts koppelen aan een Media-Service-account. 

    StorageAccount
    
    Asset.StorageAccountName en Asset.StorageAccount

* Mogelijkheid om te Job.Priority bijwerken. 

* Melding gerelateerde entiteiten en eigenschappen: 

    JobNotificationSubscription
    
    NotificationEndPoint
    
    Taak

* Asset.Uri 

* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Azure Media Services .NET SDK wijzigingen

De volgende wijzigingen zijn opgenomen in juni 2013 Media Services SDK loslaat. De meest recente Media Services SDK is beschikbaar op GitHub.

* Beginnen met de versie 2.3.0.0, de Media Services SDK ondersteunt het koppelen van meerdere opslag-mailaccounts naar een Media Services-account. Deze functie wordt ondersteund door de volgende API's:
    
    Het type IStorageAccount.
    
    De eigenschap Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts.
    
    De eigenschap StorageAccount.
    
    De eigenschap StorageAccountName.
    
    Zie [Media Services-activa beheren van meerdere opslag-accounts]voor meer informatie.

* Melding gerelateerde API's. Beginnen met de versie 2.2.0.0 die u hebt de mogelijkheid om te luisteren naar Azure wachtrij opslag meldingen. Zie voor meer informatie [Voor de verwerking van Media Services taak meldingen].
    
    De eigenschap Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions.
    
    Het type Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint.
    
    Het type Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription.
    
    Het type Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection.
    
    Het type Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType.
    
    Het type Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState.

* Afhankelijkheid van de Azure opslag-Client SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll).

* Afhankelijkheid van OData 5.5 (Microsoft.Data.OData.dll).


##<a id="december_changes_12"></a>December 2012-versie

### <a name="dec_12_dotnet_changes"></a>Azure Media Services .NET SDK wijzigingen

* IntelliSense: Toegevoegd ontbrekende Intellisense-documentatie voor veel verschillende typen.

* Microsoft.Practices.TransientFaultHandling.Core: Een probleem met de SDK zijn waar nog steeds een afhankelijkheid van een oude versie van deze constructie opgelost. De SDK verwijzingen nu versie 5.1.1209.1 van deze constructie.

Oplossingen voor problemen die zijn gevonden in de November 2012 SDK:

* IAsset.Locators.Count: Dit aantal wordt nu correct gerapporteerd op nieuwe IAsset interfaces nadat alle Locator zijn verwijderd.

* IAssetFile.ContentFileSize: Deze waarde wordt nu correct ingesteld na het uploaden door IAssetFile.Upload(filepath).

* IAssetFile.ContentFileSize: Deze eigenschap kan nu worden ingesteld bij het maken van een activum-bestand. Deze was voorheen alleen-lezen.

* IAssetFile.Upload(filepath): Een probleem met waar deze Uploadmethode synchroon is genereren het volgende foutbericht weergegeven wanneer er meerdere bestanden uploaden naar de activa opgelost. De fout is 'Server kan niet worden geverifieerd het verzoek. Zorg ervoor dat de waarde van koptekst verificatie wordt gevormd correct inclusief de handtekening."

* IAssetFile.UploadAsync: Vaste een probleem waar niet meer dan 5 bestanden tegelijk kunnen worden geüpload.

* IAssetFile.UploadProgressChanged: Deze gebeurtenis wordt nu geleverd door de SDK.

* IAssetFile.DownloadAsync (tekenreeks, BlobTransferClient, ILocator, CancellationToken): deze methode overbelasting is nu beschikbaar.

* IAssetFile.DownloadAsync: Vaste een probleem waar niet meer dan 5 bestanden tegelijk kunnen worden gedownload.

* IAssetFile.Delete(): Een probleem met bellen verwijderen kan waar een uitzondering genereren, als er geen bestand is geüpload voor de IAssetFile opgelost.

* Taken: Een het probleem opgelost waarop het koppelen van een 'MP4 aan vloeiende Streams taak"met een"PlayReady beveiliging taak"met taaksjablonen zou niet maken geen taken helemaal.

* EncryptionUtils.GetCertificateFromStore(): Deze methode genereert niet langer een uitzondering null-verwijzing vanwege een fouten het certificaat op basis van configuratieproblemen voor een certificaat gevonden.

##<a id="november_changes_12"></a>November 2012-versie

De wijzigingen die worden genoemd in deze sectie zijn opgenomen in de November 2012 (versie 2.0.0.0) updates SDK. Deze wijzigingen kunnen vragen om een code, die voor de juni 2012 voorbeeldversie SDK los om te worden gewijzigd of herschreven zijn geschreven.

* Activa
    
    IAsset.Create(assetName) is de functie alleen activa maken. IAsset.Create uploadt niet langer bestanden als onderdeel van het gesprek methode. Gebruik IAssetFile voor het uploaden.
    
    De methode IAsset.Publish en de waarde van de inventarisatie AssetState.Publish zijn verwijderd uit de SDK Services. Een code die afhankelijk van deze waarde is moet opnieuw geschreven.

* FileInfo

    Deze klasse is verwijderd en vervangen door IAssetFile.

    IAssetFiles

    IAssetFile Hiermee vervangt u FileInfo en een ander probleem heeft. Als u wilt gebruiken, exemplaar maken van het object IAssetFiles, gevolgd door een bestand is geüpload door de Media Services SDK of de SDK van Azure opslag. De volgende IAssetFile.Upload overbelastingen kunnen worden gebruikt:

    * IAssetFile.Upload(filePath): Een synchroon methode die de thread blokkeert en u wordt aangeraden alleen wanneer een bestand uploaden.
    
    * IAssetFile.UploadAsync (bestandspad, blobTransferClient, locator, cancellationToken): een asynchrone methode. Dit is de voorkeur uploaden om. 

    Bekende bug: Gebruik de cancellationToken, wordt de upload; daadwerkelijk annuleren de status van de annulering van de taken kunt wel een aantal Staten. U moet behoren onderschept en uitzonderingen.

* Locator
    
    De oorsprong / regiospecifieke versies zijn verwijderd. De context SA's / regiospecifieke. Locators.CreateSasLocator (activa, accessPolicy) worden gemarkeerd vervallen of verwijderd door GA. Zie de sectie Locator onder nieuwe functionaliteit voor bijgewerkte gedrag.


##<a id="june_changes_12"></a>De Preview-versie juni 2012

De volgende functionaliteit is nieuw in de November-versie van de SDK.

* Entiteiten verwijderen

    IAsset, IAssetFile, ILocator, IAccessPolicy, worden nu IContentKey objecten op het objectniveau van het, dat wil zeggen IObject.Delete(), in plaats van het vereisen van een verwijderen in de siteverzameling, die is cloudMediaContext.ObjCollection.Delete(objInstance) is verwijderd.

* Locator

    Locator moeten nu worden gemaakt met behulp van de methode CreateLocator en het gebruik van de waarden van de opsommen LocatorType.SAS of LocatorType.OnDemandOrigin als een argument voor de specifieke type locator die u wilt maken.

    Nieuwe eigenschappen zijn toegevoegd aan Locator zodat u eenvoudiger best URI's voor uw inhoud te verkrijgen. Deze nieuw ontwerp van Locator is bedoeld voor meer flexibiliteit bieden voor toekomstige derden uitbreiden en eenvoudig te gebruiken voor media-clienttoepassingen vergroten.

* Asynchrone methode ondersteuning

    Asynchroon ondersteuning is toegevoegd aan alle methoden.


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services MSDN-Forum]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST API verwijzing]: http://msdn.microsoft.com/library/azure/hh973617.aspx 
[Media Services prijsgegevens]: http://azure.microsoft.com/pricing/details/media-services/
[Invoer van metagegevens]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[Uitvoer metagegevens]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Inhoud leveren]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Media-bestanden met Azure Media indexering indexeren]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Werken met Azure mediaservices Live Streaming]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[Dynamische AES-128-versleuteling en belangrijke bezorgingsservice gebruiken]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[Gebruik van PlayReady dynamisch versleuteling en de bezorgingsservice licentie]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Media Services PlayReady licentie sjabloon overzicht]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Opslag streaming versleuteld inhoud]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure Classic Portal]: https://manage.windowsazure.com
[Dynamische verpakking]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Blog van niek Drouin]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Vloeiende Stream PlayReady beveiligen]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[Probeer de logica in het Media-Services SDK voor .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[Gras Valley wordt gemeld EDIUS 7 Streaming tot en met de Cloud]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Beheren Media Service Encoder uitvoer bestandsnamen]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Overlays maken]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Foto van de Video segmenten]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 en 3.0.0.2 versies]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure Active Directory Access Control-Service (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[Verbinding maken met mediaservices met het Media-Services SDK voor .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Azure Media Services-.NET SDK uitbreidingen]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[Azure-sdk-hulpprogramma 's]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Activa Services beheren Media meerdere opslag-accounts]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Verwerking van Media Services taak meldingen]: http://msdn.microsoft.com/library/azure/dn261241.aspx
 
