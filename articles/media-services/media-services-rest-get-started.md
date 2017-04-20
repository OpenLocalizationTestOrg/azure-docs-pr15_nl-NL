<properties 
    pageTitle="Aan de slag met het geven van inhoud op aanvraag gebruiken REST | Microsoft Azure" 
    description="Deze zelfstudie leert u de stappen van de uitvoering van een op aanvraag-toepassing die inhoud leveren met Azure Media Services REST API gebruiken." 
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
    ms.date="10/11/2016" 
    ms.author="juliako"/>

#<a name="get-started-with-delivering-content-on-demand-using-rest"></a>Aan de slag met het geven van inhoud op aanvraag REST gebruiken 

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


>[AZURE.NOTE]
> Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. Zie [Azure gratis proefversie](/pricing/free-trial/?WT.mc_id=A261C142F)voor meer informatie. 


Deze snelstartgids begeleidt u bij de stappen van de uitvoering van een Video-on-Demand (VoD)-contentlevering-toepassing via Azure Media Services (AMS) REST API's. 

De zelfstudie maakt u kennis met de eenvoudige werkstroom voor Media Services en de meest voorkomende programming objecten en taken die zijn vereist voor de ontwikkeling van de Media Services. Aan het einde van de zelfstudie, is mogelijk streamen of geleidelijk downloaden van een steekproef media-bestand dat u hebt geüpload, codering en gedownload.  

## <a name="prerequisites"></a>Vereisten voor
De volgende vereisten wordt voldaan om te beginnen met het ontwikkelen van met Media-Services met REST API's.

- Lidmaatschap van het ontwikkelen van met Media Services REST API. Zie voor meer informatie [media-services-rest-overzicht](http://msdn.microsoft.com/library/azure/hh973616.aspx).
- Een toepassing van uw keuze die HTTP vergaderverzoeken en antwoorden kunt verzenden. In deze zelfstudie wordt [Fiddler](http://www.telerik.com/download/fiddler). 

De volgende taken worden weergegeven in deze snelstartgids.

1.  Maak een Media Services-account (via de portal van Azure).
2.  Streaming eindpunt (via de portal van Azure) configureren.
1.  Verbinding maken met het account Media Services met REST API.
1.  Maak een nieuwe activa en upload een videobestand met REST API.
1.  Configureer streaming eenheden met REST API.
2.  Het bronbestand in een reeks geavanceerde bitsnelheid MP4-bestanden met REST API coderen.
1.  De activa en get-streaming en URL's geleidelijke downloaden met REST API publiceren. 
1.  Uw inhoud afspelen. 

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Maak een Azure Media Services-account met behulp van de Azure portal

De stappen in deze sectie laten zien hoe een AMS-account maken.

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2. Klik op **+ nieuwe** > **Media + CDN** > **mediaservices**.

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Voer de vereiste waarden in **MEDIA SERVICES-ACCOUNT maken** .

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Voer de naam van de nieuwe AMS-account in het vak **Naam van het Account**. De naam van een Media Services-account is alle kleine letters cijfers of letters zonder spaties, en is 3 tot en met 24 tekens.
    2. Selecteer in abonnement, tussen de verschillende Azure abonnementen waartoe u toegang hebt.
    
    2. Selecteer de nieuwe of bestaande resource in **Resourcegroep**.  Een resourcegroep is een verzameling resources die levenscyclus, machtigingen en beleid delen. Meer informatie [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Schakel de geografische regio worden op **locatie**gebruikt voor de opslag van de media en metagegevens records voor uw Media Services-account. Dit gebied wordt gebruikt om te verwerken en media streamen. Alleen de beschikbare Media Services regio's weergegeven in het vak van de vervolgkeuzelijst. 
    
    3. Selecteer in de **Opslag-Account**, een opslag-account om u te bieden-blobopslag van het media-inhoud van uw Media Services-account. U kunt een bestaand account voor de opslag in hetzelfde geografische gebied, als uw account Media Services selecteren of kunt u een opslag-account maken. Een nieuwe opslag-account is gemaakt in dezelfde regio. De regels voor opslag-accountnamen zijn hetzelfde als voor Media Services-accounts.

        Meer informatie over de opslag [hier](storage-introduction.md).

    4. Selecteer **vastmaken aan dashboard** om te zien van de voortgang van de account-implementatie.
    
7. Klik op **maken** onder aan het formulier.

    Zodra het account is gemaakt, verandert de status **uitgevoerd**. 

    ![Media Services-instellingen](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Voor het beheren van uw account AMS (bijvoorbeeld, video's uploaden, coderen van activa, taakvoortgang bewaken) Gebruik het venster **Instellingen** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Streaming eindpunten met behulp van de Azure portal configureren

Wanneer u werkt met Azure Media Services een van de meeste gevallen video via geavanceerde bitsnelheid streaming levert aan uw klanten. Media Services ondersteunt de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS), vloeiende Streaming, MPEG-streepje en schijven (voor Adobe WIP/Access houders).

Media Services biedt dynamische verpakking, waarmee u uw geavanceerde bitsnelheid codering MP4-inhoud in streaming door Encoder ondersteunde Media Services (MPEG streepje, HLS, vloeiende Streaming, schijven) just-in-time, zonder dat u voor de opslag van vooraf ingepakte versies van elk van deze indelingen streaming hoeft te geven.

Om te profiteren van dynamische verpakking, moet u als volgt te werk:

- Coderen uw mezzanine (bron)-bestand in een reeks geavanceerde bitsnelheid MP4-bestanden (de codering stappen worden verderop in deze zelfstudie gedemonstreerd).  
- Ten minste één streaming eenheid voor het *eindpunt streaming* waaruit u wilt gaan bezorging van uw inhoud maken. De onderstaande stappen uitgelegd hoe u het aantal streaming eenheden wijzigt.

Met dynamische verpakking, hoeft u alleen te slaan en te betalen voor de bestanden in één opslagindeling en Media Services genereert en het juiste antwoord op basis van aanvragen van een client fungeert.

Ga als volgt te werk als u wilt maken en het aantal streaming gereserveerde eenheden wijzigt:


1. Klik in het venster **Instellingen** op **Streaming eindpunten**. 

2. Klik op de standaard streaming eindpunt. 

    Het venster **Standaard STREAMING EINDPUNTDETAILS** wordt geopend.

3. Als u wilt opgeven van het aantal streaming eenheden, schuift u de schuifregelaar **Streaming eenheden** .

    ![Streaming eenheden](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Klik op de knop **Opslaan** als uw wijzigingen wilt opslaan.

    >[AZURE.NOTE]De toewijzing van een nieuwe eenheden kan maximaal 20 minuten duren.

## <a id="connect"></a>Verbinding maken met het account Media Services met REST API

Twee dingen zijn vereist bij het openen van Azure Media Services: een toegangstoken verstrekt door Azure Access Control Services (ACS) en de URI van Media Services zelf. U kunt een betekent dat die u wilt dat bij het maken van deze aanvragen zo lang maken als u de juiste koptekst waarden opgeven en uw in het toegangstoken correct binnenkomen bij het aanroepen van Media Services gebruiken.

De volgende stappen wordt uitgelegd van de meest voorkomende werkstroom bij gebruik van de Media Services REST API verbinding maken met Media Services:

1. Een toegangstoken ontvangen. 
2. Verbinding maken met de mediaservices URI.  

    Vergeet niet dat nadat de verbinding tot stand naar https://media.windows.net, ontvangt u een 301 omleiding precisie van een andere Media-URI voor Services. U moet mislukken volgende oproepen naar de nieuwe URI. U kunt ook een HTTP/1.1 200 antwoord met de beschrijving van de ODATA-API-metagegevens ontvangen.
3. Uw volgende API-oproepen naar de nieuwe URL posten. 
    
    Als na het evalueren van om verbinding te maken, hebt u bijvoorbeeld het volgende:
        
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    U moet uw volgende API-oproepen naar https://wamsbayclus001rest-hs.cloudapp.net/api/ boeken.

###<a name="getting-an-access-token"></a>Een toegangstoken ophalen

Voor toegang tot Media Services rechtstreeks via de REST API, een toegangstoken opgehaald uit ACS en deze gebruiken tijdens elke HTTP-aanvraag die u in de service aanbrengt. U hoeft niet alle andere vereiste software voordat u rechtstreeks verbinding maakt met Media-Services.

Het volgende voorbeeld ziet u de header HTTP-aanvraag en de hoofdtekst gebruikt voor het ophalen van een token.

**Kop**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json

    
**Hoofdtekst**:

U moet de client_id en client_secret waarden in de hoofdtekst van deze aanvraag; bewezen client_id en client_secret komen overeen met de waarden van de accountnaam en AccountKey, respectievelijk. Deze waarden worden door Media Services voor u beschikbaar wanneer u uw account hebt ingesteld. 

De AccountKey voor uw Media Services-account moet URL-codering wanneer u deze gebruikt als de waarde client_secret in uw token toegangsaanvraag.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Bijvoorbeeld: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


Het volgende voorbeeld ziet het HTTP-antwoord met het openen in de hoofdtekst van het antwoord token.

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670
    
    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Het wordt aanbevolen om de waarden 'access_token' en 'expires_in' naar een externe opslag in cache. De token gegevens kan later worden opgehaald uit de opslag en opnieuw worden gebruikt in uw Media Services REST API-oproepen. Dit is vooral handig voor scenario's waar het token veilig tussen meerdere processen of computers kan worden gedeeld.

Controleer of de waarde "expires_in" van het toegangstoken bewaken en bijwerken van uw oproepen REST API met nieuwe tokens, indien nodig.

###<a name="connecting-to-the-media-services-uri"></a>Verbinding maken met de mediaservices URI

Ga naar de hoofdsite URI voor Media Services is https://media.windows.net/. U moet eerst verbinding maken met deze URI en als u een 301 omleiding terug in antwoord, moet u mislukken volgende oproepen naar de nieuwe URI. Daarnaast gebruik niet alle automatische omleiding/volgen-logica in uw aanvragen. HTTP-woorden en instanties van de aanvraag wordt niet worden doorgestuurd naar de nieuwe URI.

Ga naar de hoofdsite URI voor activa bestanden uploaden en downloaden is https://yourstorageaccount.blob.core.windows.net/ waar is de opslagaccountnaam tevens die u tijdens de configuratie van Media Services-account gebruikt.

Het volgende voorbeeld wordt de HTTP-aanvraag in de hoofdmap Media Services URI (https://media.windows.net/). Het verzoek krijgt een 301 omleiding terug in antwoord. De volgende aanvraag maakt gebruik van de nieuwe URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP-aanvraag**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**HTTP-antwoord**:
    
    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164
    
    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**HTTP-aanvraag** (via de nieuwe URI):
            
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-antwoord**:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}
     


>[AZURE.NOTE] Als u voortaan wordt de nieuwe URI gebruikt in deze zelfstudie.

## <a id="upload"></a>Maak een nieuwe activa en uploadt u een videobestand met REST API

In Media-Services, kunt u uw digitale bestanden uploaden naar activa. De **activa** entity kunt video, audio, afbeeldingen, miniaturen verzamelingen, tekst nummers en bevatten ondertiteling bestanden (en de metagegevens over deze bestanden.)  Nadat de bestanden worden geüpload naar de activa, wordt uw inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming. 

Een van de waarden die u opgeven moet bij het maken van activa is opties voor het maken van actief. De eigenschap **Options** is de inventarisatiewaarde van een waarmee de versleutelingsopties die activa kan worden gemaakt met beschreven. Een ongeldige waarde is een van de waarden uit de lijst hieronder, niet een combinatie van waarden in deze lijst:

 
- **Geen** = **0** - geen versleuteling wordt gebruikt. Wanneer u deze optie gebruikt is de inhoud niet beveiligd tijdens overdracht of op rest in opslag.
    Als u van plan bent om uit te voeren een MP4 geleidelijke downloaden, gebruikt u deze optie gebruikt. 
- **StorageEncrypted** = **1** - worden gecodeerd wissen inhoud lokaal met AES-256 bitsversleuteling en vervolgens geüpload naar Azure Storage waar deze zijn opgeslagen in rust versleuteld. Activa die zijn beveiligd met opslag versleuteling zijn automatisch niet versleuteld en geplaatst in een versleutelde bestandssysteem vóór codering en desgewenst opnieuw worden versleuteld voordat u het uploadt weer als een nieuwe uitvoer actief. De primaire use-case voor opslag-versleuteling is wanneer u wilt beveiligen van uw kwalitatief hoogwaardig invoer mediabestanden met codering in rust op schijf.
- **CommonEncryptionProtected** = **2** : Gebruik deze optie als u bezig bent met het uploaden van inhoud die al is versleuteld en beveiligd met algemene versleuteling of PlayReady DRM (bijvoorbeeld vloeiende Streaming beveiligd met DRM PlayReady).
- **EnvelopeEncryptionProtected** = **4** – Gebruik deze optie als u bezig bent met het uploaden van HLS versleuteld met AES. De bestanden zijn moeten gecodeerd en versleuteld door Manager transformeren.

### <a name="create-an-asset"></a>Activa maken

Een actief is een container voor meerdere typen of sets met objecten in Media Services, met inbegrip van video, audio, afbeeldingen, miniaturen verzamelingen, tekst nummers en ondertiteling bestanden. In de REST API moet voor het maken van activa POST-aanvraag naar Media Services en alle eigenschapsgegevens over uw activa plaatsen in het hoofdgedeelte van de aanvraag.

Het volgende voorbeeld ziet u hoe u een actief maakt.

**HTTP-aanvraag**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45
    
    {"Name":"BigBuckBunny.mp4", "Options":"0"}
    

**HTTP-antwoord**

Als dit lukt, wordt het volgende geretourneerd:
    
    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
### <a name="create-an-assetfile"></a>Een AssetFile maken

De entiteit [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) vertegenwoordigt een video of audio-bestand dat is opgeslagen in een container blob. Een activum-bestand is altijd gekoppeld aan activa en activa kan een of meer AssetFiles bevatten. De taak Media Services Encoder mislukt als een activum bestand-object niet gekoppeld aan een digitale bestand in een container blob is.

Nadat u uw digitale media-bestand naar een container blob uploaden, kunt u het verzoek HTTP **samenvoegen** de AssetFile bijwerken met informatie over het mediabestand (zoals u later in het onderwerp). 

**HTTP-aanvraag**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 164
    
    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-antwoord**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>De AccessPolicy te maken met de machtiging schrijven. 

Voordat u bestanden uploadt naar blobopslag, toegang beleid rechten instellen voor het schrijven van activa. PUBLICEREN om dat te doen, een HTTP-aanvraag met de AccessPolicies entiteit set. Een waarde DurationInMinutes tijdens het maken van definiëren of foutbericht een 500 Interne Server terug in antwoord. Zie voor meer informatie over AccessPolicies, [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Het volgende voorbeeld ziet hoe u een AccessPolicy maakt:
        
**HTTP-aanvraag**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP-antwoord**

    If successful, the following response is returned:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

### <a name="get-the-upload-url"></a>De Upload-URL

Maak een Locator SA's ontvangen de werkelijke upload-URL. Locator definiëren de begintijd en type verbindingseindpunt voor clients die toegang tot bestanden in een actief wilt. U kunt meerdere Locator entiteiten voor een bepaald AccessPolicy en activa paar naar afhandeling van aanvragen van verschillende clients en maken. Elk van deze Locator de waarde voor de starttijd plus de waarde DurationInMinutes van de AccessPolicy gebruikt om te bepalen hoe lang die een URL kan worden gebruikt. Zie [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx)voor meer informatie.


De URL van een SA's heeft de volgende indeling:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Enkele punten van toepassing:

- U kunt meer dan vijf unieke Locator die is gekoppeld aan een bepaald activum in één keer geen. Zie Locator voor meer informatie.
- Als u nodig hebt voor het uploaden van bestanden direct, moet u de waarde voor de starttijd ingesteld op vijf minuten voordat u de huidige tijd. Dit is omdat er mogelijk klok scheefheid tussen de clientcomputer en het Media-Services. De waarde voor de starttijd moet ook zijn in de volgende DateTime-notatie: jjjj-MM-ddTHH (bijvoorbeeld "2014-05-23T17:53:50Z").   
- Er zijn mogelijk een tweede 30-40 uitstellen nadat een Locator is gemaakt naar wanneer deze beschikbaar voor gebruik is. Dit probleem geldt voor SA's URL en Origin Locator.

Het volgende voorbeeld ziet u het maken van een SA's URL Locator, zoals gedefinieerd door de eigenschap Type in het hoofdgedeelte van de aanvraag ('1' voor een locator SA's) en '2' voor een op aanvraag origin locator. De eigenschap **Path** geretourneerd bevat de URL die u gebruiken moet om uw bestand te uploaden.
    
**HTTP-aanvraag**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178
    
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**HTTP-antwoord**

Als dit lukt, wordt het volgende antwoord geretourneerd:
        
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Een bestand uploaden naar een blob storage container
    
Nadat u hebt de AccessPolicy en Locator ingesteld, wordt het bestand wordt geüpload naar een Azure-blobopslag container met Azure opslag REST API's. U kunt uploaden pagina of BLOB's blokkeren. 

>[AZURE.NOTE] De bestandsnaam voor het bestand dat u wilt uploaden naar de Locator **pad** -waarde in de vorige sectie hebt ontvangen, moet u toevoegen. Bijvoorbeeld https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Zie voor meer informatie over het werken met opslag Azure BLOB's [Blob Service REST API](http://msdn.microsoft.com/library/azure/dd135733.aspx).


### <a name="update-the-assetfile"></a>De AssetFile bijwerken 

Nu dat u het bestand hebt geüpload, moet u de informatie FileAsset grootte (en andere) bijwerken. Bijvoorbeeld:
    
    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-antwoord**

Als geslaagd, de volgende handelingen uit als resultaat gegeven: HTTP/1.1 204 geen inhoud

## <a name="delete-the-locator-and-accesspolicy"></a>De Locator en AccessPolicy verwijderen 

**HTTP-aanvraag**


    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    
**HTTP-antwoord**

Als dit lukt, wordt het volgende geretourneerd:

    HTTP/1.1 204 No Content 
    ...

**HTTP-aanvraag**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-antwoord**

Als dit lukt, wordt het volgende geretourneerd:

    HTTP/1.1 204 No Content 
    ...

 
## <a id="configure_streaming_units"></a>Streaming eenheden configureren met REST API

Wanneer u werkt met Azure Media Services een van de meeste gevallen geavanceerde bitsnelheid streaming levert aan uw klanten. De client kunt overschakelen naar een hogere of lagere bitsnelheid met geavanceerde bitsnelheid streaming, zoals de video wordt weergegeven op basis van de huidige netwerkbandbreedte, CPU-gebruik en andere factoren. Media Services ondersteunt de volgende geavanceerde bitsnelheid streaming technologieën: HTTP Live Streaming (HLS), vloeiende Streaming, MPEG-streepje en schijven (voor Adobe WIP/Access houders). 

Media Services biedt dynamische verpakking, waarmee u voor het leveren van de inhoud van het geavanceerde bitsnelheid MP4 of vloeiende Streaming codering in door Services Media (MPEG streepje, HLS, vloeiende Streaming, schijven) Encoder ondersteunde streaming zonder dat u hoeft om te verpakken in deze indelingen streaming. 

Om te profiteren van dynamische verpakking, moet u als volgt te werk:

- ten minste één streaming eenheid voor het **eindpunt streaming **waaruit u van plan bent om uit te voeren van uw inhoud (beschreven in deze sectie) ophalen.
- coderen of transcoderen uw mezzanine (bron) opbergen in een reeks geavanceerde bitsnelheid MP4-bestanden of geavanceerde bitsnelheid vloeiende Streaming-bestanden (de codering stappen worden gedemonstreerd verderop in deze zelfstudie)  

Met dynamische verpakking, hoeft u alleen te slaan en te betalen voor de bestanden in één opslagindeling en Media Services genereert en het juiste antwoord op basis van aanvragen van een client fungeert. 


>[AZURE.NOTE] Zie voor informatie over prijzen details [Media Services prijzen Details](http://go.microsoft.com/fwlink/?LinkId=275107).

Ga als volgt te werk als u wilt wijzigen hoeveel streaming gereserveerde eenheden:
    
### <a name="get-the-streaming-endpoint-you-want-to-update"></a>Krijg het streaming eindpunt dat u wilt bijwerken

Laten we haalt bijvoorbeeld het eerste streaming eindpunt in uw account (u kunt maximaal twee streaming eindpunten in staat uitgevoerd op hetzelfde moment hebben).

**HTTP-aanvraag**:

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints()?$top=1 HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-antwoord**
    
Als dit lukt, wordt het volgende geretourneerd:
    
    HTTP/1.1 200 OK
    . . . 
    
### <a name="scale-the-streaming-endpoint"></a>De schaal van het eindpunt streaming aanpassen
 
**HTTP-aanvraag**:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints('nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486')/Scale HTTP/1.1
    Content-Type: application/json;odata=verbose
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {"scaleUnits":1}

**HTTP-antwoord**

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    x-ms-request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:16:43 GMT
    Content-Length: 0

    
### <a id="long_running_op_status"></a>Controleren of de status van een langdurige bewerking

De toewijzing van een nieuwe eenheden duurt ongeveer 20 minuten om te voltooien. Controleer de status van het gebruik van de bewerking de methode **bewerkingen** en geef de Id van de bewerking. De bewerking Id is geretourneerd als het antwoord op het verzoek **schaal** .

    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
 
**HTTP-aanvraag**:
    
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7') HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
**HTTP-antwoord**
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 515
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 829e1a89-3ec2-4836-a04d-802b5aeff5e8
    request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    x-ms-request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:57:39 GMT
    
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Operation"
          },
          "Id":"nb:opid:UUID:cc339c28-6bba-4f7d-bee5-91ea4a0a907e",
          "State":"Succeeded",
          "TargetEntityId":"nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486",
          "ErrorCode":null,
          "ErrorMessage":null
       }
    }


## <a id="encode"></a>Coderen van het bronbestand in een reeks geavanceerde bitsnelheid MP4-bestanden

Na ingesting die activa in Media Services, media kunnen worden gecodeerd, transmuxed, een watermerk, enzovoort, voordat deze wordt bezorgd in clients. Deze activiteiten zijn gepland en uitvoeren op meerdere exemplaren van achtergrond rol om ervoor te zorgen optimale prestaties en beschikbaarheid. Deze activiteiten taken worden genoemd en elke [taak](http://msdn.microsoft.com/library/azure/hh974289.aspx) bestaat uit atomaire taken die de werkelijke hoeveelheid werk aan het bestand activa uitvoeren. 

Zoals eerder genoemde, wanneer u werkt met Azure Media Services een van de meeste gevallen geavanceerde bitsnelheid streaming levert aan uw klanten. Media-Services kunt dynamisch Inpakken voor een reeks geavanceerde bitsnelheid MP4-bestanden in een van de volgende indelingen: HTTP Live Streaming (HLS), vloeiende Streaming, MPEG-streepje en schijven (voor Adobe WIP/Access houders). 

Om te profiteren van dynamische verpakking, moet u als volgt te werk:

- coderen of transcoderen uw mezzanine (bron) opbergen in een reeks geavanceerde bitsnelheid MP4-bestanden of geavanceerde bitsnelheid vloeiende Streaming-bestanden  
- ten minste één streaming eenheid voor het streaming eindpunt waaruit u van plan bent om uit te voeren van uw inhoud downloaden. 

De volgende sectie ziet u hoe een taak met één codering taak wilt maken. De taak aangegeven te transcoderen in het mezzanine-bestand in een reeks geavanceerde bitsnelheid MP4s met **Media Encoder standaard**. De sectie ziet ook hoe u de taak processing voortgang bewaken. Als de taak voltooid is, kunt u zou kunnen Locator die nodig zijn om toegang te krijgen tot uw activa maken. 

### <a name="get-a-media-processor"></a>Krijg een Mediaprocessor

In een Media-Services is een Mediaprocessor een onderdeel waarmee een specifieke verwerkingstaak, zoals codering, conversie van bestandsindeling, codering of decoderen media-inhoud. Voor de codering taak worden weergegeven in deze zelfstudie gaat we gebruiken de Media Encoder standaard.

De volgende code aanvraagt van de encoder-id. 

**HTTP-aanvraag**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP-antwoord**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Een taak maken

Elke taak kan hebben een of meer taken afhankelijk van het type verwerking die u wilt uitvoeren. Via de REST API, kunt u taken en hun verwante taken op twee manieren: taken kunnen worden gedefinieerd inline via de eigenschap navigatie van taken op taak entiteiten of via OData-batchbestand. De Media Services SDK batchbestand gebruikt. Voor de leesbaarheid van in de codevoorbeelden in dit onderwerp zijn taken echter gedefinieerde inline. Voor informatie over batchbestand, Zie [(OData) Open Data Protocol Batch-verwerking](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

Het volgende voorbeeld ziet u hoe maken en publiceren van een project met een die taak instellen voor het coderen van een video op een bepaalde resolutie en de kwaliteit. De volgende documentatie sectie bevat de lijst met alle [taak vooraf ingestelde](http://msdn.microsoft.com/library/mt269960) ondersteund door de Media Encoder standaard processor.  

**HTTP-aanvraag**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482
    
    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**HTTP-antwoord**

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  
    
             ]
          }
       }
    }


Er zijn enkele belangrijke punten onthouden in elk verzoek taak:

- TaskBody eigenschappen moeten letterlijke XML gebruiken om te definiëren het aantal invoer of uitvoer van activa die worden gebruikt door de taak. Het onderwerp van de taak bevat de XML-Schema-definitie voor de XML.
- In de definitie TaskBody elke binnenste waarde van <inputAsset> en <outputAsset> JobInputAsset(value) of JobOutputAsset(value) moet worden ingesteld.
- Een taak kan meerdere uitvoer activa hebben. Één JobOutputAsset(x) kunnen slechts één keer worden gebruikt als een uitvoer van een taak in een taak.
- U kunt JobInputAsset of JobOutputAsset opgeven als invoer activa van een taak.
- Taken moeten een cyclus geen vormen.
- De parameter value die u aan JobInputAsset of JobOutputAsset doorgeven geeft de indexwaarde voor activa. De werkelijke activa zijn gedefinieerd in de eigenschappen van de navigatie InputMediaAssets en OutputMediaAssets op de taak entiteitsdefinitie. 

>[AZURE.NOTE] Omdat Media Services is gebaseerd op OData v3 en de afzonderlijke activa in InputMediaAssets en OutputMediaAssets navigatie eigenschap verzamelingen wordt verwezen door een ' __metadata: uri "paar naam-waarde. 

- InputMediaAssets koppelt aan een of meer activa die u hebt gemaakt in Media Services. OutputMediaAssets worden gemaakt door het systeem. Ze niet verwijzen naar een bestaand activum.
- OutputMediaAssets kan de naam met het kenmerk assetName. Als dit kenmerk niet aanwezig is, is de naam van de OutputMediaAsset ongeacht de binnenste tekstwaarde van de <outputAsset> element is met een achtervoegsel van de taaknaam waarde of de taak-Id-waarde (in het geval waar de naam van eigenschap wordt niet gedefinieerd). Bijvoorbeeld als u een waarde voor assetName "Steekproef" hebt ingesteld, klikt u vervolgens de OutputMediaAsset naam eigenschap zou zijn ingesteld op "Voorbeeld". Als u een waarde voor assetName niet hebt ingesteld, maar de taaknaam op 'NewJob' ingesteld, zou klikt u vervolgens de naam van de OutputMediaAsset wel "JobOutputAsset (waarde) _NewJob". 

    Het volgende voorbeeld ziet u hoe het kenmerk assetName instellen:
    
        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"


- Inschakelen taak koppelen:

    - Een taak moet ten minste twee taken
    - Er moet ten minste één taak waarvan invoer uitvoer van een andere taak in de taak is.

Zie voor meer informatie [voor het maken van een taak coderen met de Media Services REST API](http://msdn.microsoft.com/library/azure/jj129574.aspx).

### <a name="monitor-processing-progress"></a>Verwerking van de voortgang controleren

U kunt de taakstatus ophalen met behulp van de eigenschap State zoals wordt weergegeven in het volgende voorbeeld. 

**HTTP-aanvraag**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**HTTP-antwoord**

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT
    
    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Een taak annuleren

Media-Services kunt u actieve taken tot en met de functie CancelJob annuleren. In dit gesprek geeft als resultaat een 400 foutcode als u probeert te annuleren van een taak wanneer de status wordt geannuleerd, heffen, fout of voltooid.

Het volgende voorbeeld wordt getoond hoe CancelJob bellen.


**HTTP-aanvraag**


    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


Als dit lukt, wordt een antwoordcode 204 geretourneerd met geen berichttekst.

>[AZURE.NOTE] U moet URL-codering de taak-id (normaal nb:jid:UUID: somevalue) wanneer deze in als u een parameter aan CancelJob wordt doorgegeven.


### <a name="get-the-output-asset"></a>De uitvoer van activa ophalen 

De volgende code toont over het aanvragen van de activa uitvoer Id. 


**HTTP-aanvraag**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP-antwoord**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <a id="publish_get_urls"></a>De activa en get-streaming en URL's geleidelijke downloaden met REST API publiceren

Om te streamen of downloaden van activa, moet u eerst "publiceert' door te maken van een locator. Locator bieden toegang tot bestanden in de activa. Media Services ondersteunt twee soorten Locator: OnDemandOrigin Locator, gebruikt om te stream media (bijvoorbeeld MPEG streepje, HLS of vloeiende Streaming) en Locator Access handtekening (SA's), gebruikt om te downloaden van media-bestanden.

Nadat u de Locator hebt gemaakt, kunt u de URL's die worden gebruikt om te streamen of downloaden van uw bestanden kunt maken. 


De URL van een streaming voor vloeiende Streaming heeft de volgende indeling:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

De URL van een streaming voor HLS heeft de volgende indeling:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

De URL van een streaming voor MPEG streepje heeft de volgende indeling:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


De URL van een SA's gebruikt om te downloaden van bestanden heeft de volgende indeling:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Hier vindt u hoe u de volgende taken uitvoeren nodig uw activa "publiceren '.  

- De AccessPolicy maken met leesmachtiging 
- De URL van een SA's maken voor het downloaden van inhoud 
- Maken van een URL origin voor streaming inhoud 

###<a name="creating-the-accesspolicy-with-read-permission"></a>De AccessPolicy maken met leesmachtiging

Voordat downloaden of alle mediainhoud-streaming, eerst een AccessPolicy met leesmachtigingen definiëren en maak de juiste Locator entiteit waarmee het type van bezorging om die u wilt inschakelen voor uw klanten. Zie voor meer informatie over de eigenschappen die beschikbaar zijn, [AccessPolicy entiteitseigenschappen](https://msdn.microsoft.com/library/azure/hh974297.aspx#accesspolicy_properties).

Het volgende voorbeeld ziet u de AccessPolicy voor leesmachtigingen voor een bepaald activum opgeven.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue
    
    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Als dit lukt, wordt een succescode 201 geretourneerd met een beschrijving van de AccessPolicy entiteit die u hebt gemaakt. U gebruikt u de AccessPolicy Id samen met de activa-Id van de activa die het bestand dat u leveren (zoals activa uitvoer wilt) als u wilt maken van de entiteit Locator bevat.

>[AZURE.NOTE]
Deze eenvoudige werkstroom is hetzelfde als het uploaden van een bestand wanneer ingesting activa (zoals eerder in dit onderwerp is besproken). Ook, zoals het uploaden van bestanden, als u (of uw klanten) moeten direct toegang krijgen tot uw bestanden, de starttijd waarde instellen op vijf minuten voordat u de huidige tijd. Deze actie is nodig omdat er mogelijk klok scheefheid tussen de client en Media Services. De starttijd waarde moet liggen in de volgende DateTime-notatie: jjjj-MM-ddTHH (bijvoorbeeld "2014-05-23T17:53:50Z").


###<a name="creating-a-sas-url-for-downloading-content"></a>De URL van een SA's maken voor het downloaden van inhoud 

De volgende code ziet u hoe u een URL die kan worden gebruikt om een mediabestand hebt gemaakt en die eerder zijn geüpload, te downloaden. De AccessPolicy machtigingenset heeft lees- en het pad Locator verwijst naar de URL van een SA's downloaden.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Als dit lukt, wordt het volgende antwoord geretourneerd:

    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


De resulterende **pad** eigenschap bevat de URL SA's.

>[AZURE.NOTE]
Als u opslag versleuteld inhoud downloaden, moet u handmatig decoderen voordat deze of de opslag decoderen MediaProcessor gebruiken in een taak voor de verwerking van het uitvoeren van verwerkte bestanden in de wissen naar een OutputAsset en vervolgens downloaden via dat actief. Zie maken van een taak coderen met de Media Services REST API voor meer informatie over verwerking. Bovendien kan niet SA's URL Locator worden bijgewerkt nadat ze zijn gemaakt. Bijvoorbeeld niet kunt u de dezelfde Locator met een bijgewerkte waarde voor de starttijd opnieuw gebruiken. Dit is vanwege de manier die SA 's URL's worden gemaakt. Als u toegang wilt tot activa downloaden nadat een Locator is verlopen, maakt u een nieuw bestand met een nieuwe starttijd.

###<a name="download-files"></a>Bestanden downloaden

Nadat u hebt de AccessPolicy en Locator instellen, kunt u bestanden met behulp van de Azure Storage REST API's downloaden.  

>[AZURE.NOTE] De bestandsnaam voor het bestand dat u wilt downloaden naar de Locator **pad** -waarde in de vorige sectie hebt ontvangen, moet u toevoegen. Bijvoorbeeld https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Zie voor meer informatie over het werken met opslag Azure BLOB's [Blob Service REST API](http://msdn.microsoft.com/library/azure/dd135733.aspx).

Als gevolg van de codering taak die u hebt uitgevoerd eerdere (codering in geavanceerde MP4 set), maar u hebt meerdere MP4-bestanden die u kunt geleidelijk downloaden. Bijvoorbeeld:    
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a>De URL van een streaming maken voor streaming inhoud


De volgende code ziet u hoe u een streaming URL Locator maakt:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

Als u wilt een origin vloeiende Streaming URL in een streaming mediaspeler streamen, moet u het pad eigenschap met de naam van de vloeiende Streaming bestandenlijst bestand, gevolgd door "/ bestandenlijst" toevoegen.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

Om te stromen HLS, toevoegen (opmaak = m3u8-aapl) nadat de "/ bestandenlijst".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

Om te stromen MPEG streepje, toevoegen (opmaak = mpd-tijd-csf) nadat de "/ bestandenlijst".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a id="play"></a>Uw inhoud afspelen  

Gebruiken om te stromen u video, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Als u wilt testen geleidelijke downloaden, door een URL in een browser (bijvoorbeeld IE, Chrome, Safari) te plakken.


##<a name="next-steps-media-services-learning-paths"></a>Volgende stappen: Mediaservices leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="looking-for-something-else"></a>Zoekt u iets anders?

Als dit onderwerp niet bevatten wat u verwacht, ontbreekt er iets of in een andere manier niet aan uw wensen voldoet, geef ons uw feedback met de onderstaande Disqus-thread.



