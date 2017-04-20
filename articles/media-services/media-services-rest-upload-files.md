<properties 
    pageTitle="Bestanden uploaden naar een Media Services-account met REST | Microsoft Azure" 
    description="Leer hoe u media-inhoud van Media Services door te maken en uploaden van activa." 
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


# <a name="upload-files-into-a-media-services-account-using-rest"></a>Bestanden uploaden naar een REST met Media Services-account

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [REST](media-services-rest-upload-files.md)
 - [Portal](media-services-portal-upload-files.md)

In Media-Services, kunt u uw digitale bestanden uploaden naar activa. De [activa](https://msdn.microsoft.com/library/azure/hh974277.aspx) entity kunt video, audio, afbeeldingen, miniaturen verzamelingen, tekst nummers en bevatten ondertiteling bestanden (en de metagegevens over deze bestanden.)  Nadat de bestanden worden geüpload naar de activa, wordt uw inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming. 

>[AZURE.NOTE]De volgende overwegingen bij het kiezen van een bestandsnaam van de activa van toepassing:
>
>- Media Services gebruikt de waarde van de eigenschap IAssetFile.Name bij het maken van URL's voor de streaming inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Daarom is percentagecodering niet toegestaan. De waarde van de eigenschap **Name** geen van de volgende [tekens percentage-codering-gereserveerd](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Bovendien kunnen alleen er een '.' voor de bestandsnaamextensie.
>
>- De lengte van de naam mag niet langer zijn dan 260 tekens.

De eenvoudige werkstroom voor het uploaden van activa is in de volgende secties verdeeld:

- Activa maken
- Versleutelen activa (optioneel)
- Een bestand uploaden naar-blobopslag

AMS kunt u ook activa bulksgewijs uploaden. Zie voor meer informatie [in deze](media-services-rest-upload-files.md#upload_in_bulk) sectie.

##<a name="upload-assets"></a>Activa uploaden

###<a name="create-an-asset"></a>Activa maken

>[AZURE.NOTE] Tijdens het werken met de Media Services REST API, zijn de volgende punten van toepassing:
>
>Bij het openen van entiteiten in Media Services, moet u specifieke koptekstvelden en waarden instellen in uw HTTP-aanvragen. Zie [configuratie van voor de ontwikkeling van Media Services REST API](media-services-rest-how-to-use.md)voor meer informatie.

>Nadat de verbinding tot stand naar https://media.windows.net, ontvangt u een 301 omleiding precisie van een andere Media-URI voor Services. Mislukken volgende oproepen naar de nieuwe URI zoals is beschreven in [verbinding maken met Media-Services REST API gebruiken](media-services-rest-connect-programmatically.md), moet u ervoor. 
 
Een actief is een container voor meerdere typen of sets met objecten in Media Services, met inbegrip van video, audio, afbeeldingen, miniaturen verzamelingen, tekst nummers en ondertiteling bestanden. In de REST API moet voor het maken van activa POST-aanvraag naar Media Services en alle eigenschapsgegevens over uw activa plaatsen in het hoofdgedeelte van de aanvraag.

Een van de eigenschappen die u opgeven kunt bij het maken van een actief is **Opties**. **Opties** is de inventarisatiewaarde van een waarmee de versleutelingsopties die activa kan worden gemaakt met beschreven. Een ongeldige waarde is een van de waarden uit de lijst hieronder, niet een combinatie van waarden. 

- **Geen** = **0**: geen versleuteling wordt gebruikt. Dit is de standaardwaarde. Houd er rekening mee dat wanneer u deze optie gebruikt de inhoud niet is beveiligd tijdens overdracht of op rest in opslag.
    Als u van plan bent om uit te voeren een MP4 geleidelijke downloaden, gebruikt u deze optie gebruikt. 

- **StorageEncrypted** = **1**: Geef aan of voor uw bestanden worden versleuteld met AES-256 bits codering voor uploaden en opslag.

    Als uw activa versleuteld opslag is, moet u activa bezorging beleid configureren. Zie [configureren activa bezorging beleid](media-services-rest-configure-asset-delivery-policy.md)voor meer informatie.

- **CommonEncryptionProtected** = **2**: opgeven als u bestanden die zijn beveiligd met een gemeenschappelijke coderingsmethode (zoals PlayReady) wilt uploaden. 

- **EnvelopeEncryptionProtected** = **4**: opgeven als u HLS versleuteld met AES-bestanden wilt uploaden. Houd er rekening mee dat de bestanden moeten codering en versleuteld door Manager transformeren.

>[AZURE.NOTE]Als uw activa versleuteling gebruikt, moet u een **ContentKey** maken en dit koppelen aan uw activa, zoals wordt beschreven in het volgende onderwerp:[het maken van een ContentKey](media-services-rest-create-contentkey.md). Houd er rekening mee dat nadat u de bestanden naar de activa uploaden, moet u de eigenschappen versleuteling op de entiteit **AssetFile** bijwerken met de waarden die u hebt ontvangen tijdens de **activa** -codering. Werk met behulp van de aanvraag HTTP **samenvoegen** . 


Het volgende voorbeeld ziet u hoe u een actief maakt.

**HTTP-aanvraag**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"BigBuckBunny.mp4"}
    

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
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
###<a name="create-an-assetfile"></a>Een AssetFile maken

De entiteit [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) vertegenwoordigt een video of audio-bestand dat is opgeslagen in een container blob. Een activum-bestand is altijd gekoppeld aan activa en activa kan een of meer activa-bestanden bevatten. De taak Media Services Encoder mislukt als een activum bestand-object niet gekoppeld aan een digitale bestand in een container blob is.

Houd er rekening mee dat het exemplaar **AssetFile** en het werkelijke mediabestand twee afzonderlijke objecten zijn. Het exemplaar AssetFile bevat metagegevens over het mediabestand, terwijl het mediabestand de werkelijke media-inhoud bevat.

Nadat u uw digitale media-bestand naar een container blob uploaden, kunt u het verzoek HTTP **samenvoegen** wilt gebruiken voor het bijwerken van de AssetFile met informatie over het mediabestand (zoals u later in het onderwerp). 

**HTTP-aanvraag**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
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

Voordat u bestanden uploadt naar blobopslag, toegang beleid rechten instellen voor het schrijven van activa. PUBLICEREN om dat te doen, een HTTP-aanvraag met de AccessPolicies entiteit set. Een waarde DurationInMinutes tijdens het maken van definiëren of, ontvangt u een 500 Interne Server-foutbericht weer in de reactie. Zie voor meer informatie over AccessPolicies, [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Het volgende voorbeeld ziet hoe u een AccessPolicy maakt:
        
**HTTP-aanvraag**

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP-aanvraag**

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

###<a name="get-the-upload-url"></a>De Upload-URL

Maak een Locator SA's ontvangen de werkelijke upload-URL. Locator definiëren de begintijd en type verbindingseindpunt voor clients die toegang tot bestanden in een actief wilt. U kunt meerdere Locator entiteiten voor een bepaald AccessPolicy en activa paar naar afhandeling van aanvragen van verschillende clients en maken. Elk van deze Locator de waarde voor de starttijd plus de waarde DurationInMinutes van de AccessPolicy gebruiken om te bepalen hoe lang een URL kan worden gebruikt. Zie [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx)voor meer informatie.


De URL van een SA's heeft de volgende indeling:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Enkele punten van toepassing:

- U kunt meer dan vijf unieke Locator die is gekoppeld aan een bepaald activum in één keer geen. Zie Locator voor meer informatie.
- Als u nodig hebt voor het uploaden van bestanden direct, moet u de waarde voor de starttijd ingesteld op vijf minuten voordat u de huidige tijd. Dit is omdat er mogelijk klok scheefheid tussen de clientcomputer en het Media-Services. De waarde voor de starttijd moet ook zijn in de volgende DateTime-notatie: jjjj-MM-ddTHH (bijvoorbeeld "2014-05-23T17:53:50Z").   
- Er zijn mogelijk een tweede 30-40 uitstellen nadat een Locator is gemaakt naar wanneer deze beschikbaar voor gebruik is. Dit probleem geldt voor SA's URL en Origin Locator.

Het volgende voorbeeld ziet u het maken van een SA's URL Locator, zoals gedefinieerd door de eigenschap Type in het hoofdgedeelte van de aanvraag ('1' voor een locator SA's) en '2' voor een op aanvraag origin locator. De eigenschap **Path** geretourneerd bevat de URL die u gebruiken moet om uw bestand te uploaden.
    
**HTTP-aanvraag**
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
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
    
    MERGE https://media.windows.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-antwoord**

Als geslaagd, de volgende handelingen uit als resultaat gegeven: HTTP/1.1 204 geen inhoud

### <a name="delete-the-locator-and-accesspolicy"></a>De Locator en AccessPolicy verwijderen 

**HTTP-aanvraag**


    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
**HTTP-antwoord**

Als dit lukt, wordt het volgende geretourneerd:

    HTTP/1.1 204 No Content 
    ...

**HTTP-aanvraag**

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

**HTTP-antwoord**

Als dit lukt, wordt het volgende geretourneerd:

    HTTP/1.1 204 No Content 
    ...

##<a id="upload_in_bulk"></a>Activa bulksgewijs uploaden

###<a name="create-the-ingestmanifest"></a>De IngestManifest maken

De IngestManifest is een container voor een reeks activa, activa bestanden en statistische gegevens die kan worden gebruikt om te bepalen de voortgang van bulksgewijs ingesting voor de set.


**HTTP-aanvraag**

    POST https:// media.windows.net/API/IngestManifests HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 36
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST" }

###<a name="create-assets"></a>Activa maken

Voordat u de IngestManifestAsset maakt, moet u de activa die wordt voltooid met bulksgewijs ingesting maken. Een actief is een container voor meerdere typen of sets met objecten in Media Services, met inbegrip van video, audio, afbeeldingen, miniaturen verzamelingen, tekst nummers en ondertiteling bestanden. In de REST API moet voor het maken van activa een HTTP POST-aanvraag wordt verzonden naar Microsoft Azure Media Services en alle eigenschapsgegevens over uw activa plaatsen in het hoofdgedeelte van de aanvraag. In dit voorbeeld wordt de activum gemaakt met de optie StorageEncrption(1) is inbegrepen in de hoofdtekst van het verzoek.

**HTTP-antwoord**

    POST https://media.windows.net/API/Assets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 55
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST_Asset", "Options" : 1 }

###<a name="create-the-ingestmanifestassets"></a>De IngestManifestAssets maken

IngestManifestAssets vertegenwoordigen activa binnen een IngestManifest die worden gebruikt met bulksgewijs ingesting. Het in feite het activum koppelen aan het manifest. Azure Media Services toekijkt intern voor het bestand te uploaden op basis van IngestManifestFiles siteverzameling die is gekoppeld aan de IngestManifestAsset. Nadat u deze bestanden worden geüpload, wordt de activa is voltooid. U kunt een nieuwe IngestManifestAsset maken met een HTTP POST-aanvraag. In het hoofdgedeelte van de aanvraag, bevat de IngestManifest-Id en de activa-Id die de IngestManifestAsset voor het bulksgewijs ingesting koppelen moet.

**HTTP-antwoord**

    POST https://media.windows.net/API/IngestManifestAssets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 152
    Expect: 100-continue
    { "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "Asset" : { "Id" : "nb:cid:UUID:b757929a-5a57-430b-b33e-c05c6cbef02e"}}


###<a name="create-the-ingestmanifestfiles-for-each-asset"></a>De IngestManifestFiles maken voor alle activa

Een IngestManifestFile vertegenwoordigt een werkelijke video of audio blob-object dat als onderdeel van het bulksgewijs ingesting voor activa worden geüpload. Versleuteling gerelateerde eigenschappen zijn niet verplicht, tenzij de activa een versleutelingsoptie wordt gebruikt. Het voorbeeld die wordt gebruikt in deze sectie bevat een IngestManifestFile maken die wordt gebruikt met StorageEncryption voor de activa eerder hebt gemaakt.


**HTTP-antwoord**

    POST https://media.windows.net/API/IngestManifestFiles HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 367
    Expect: 100-continue
    
    { "Name" : "REST_Example_File.wmv", "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "ParentIngestManifestAssetId" : "nb:maid:UUID:beed8531-9a03-9043-b1d8-6a6d1044cdda", "IsEncrypted" : "true", "EncryptionScheme" : "StorageEncryption", "EncryptionVersion" : "1.0", "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510" }
    
###<a name="upload-the-files-to-blob-storage"></a>Upload de bestanden met Blob Storage

U kunt een hoge snelheid client-toepassing kan de activa-bestanden uploaden naar de blob storage container Uri door de eigenschap BlobStorageUriForUpload van de IngestManifest verstrekt gebruiken. Een aantal aanzienlijke hoge snelheid upload-service is [Aspera op aanvraag voor Azure-toepassing](http://go.microsoft.com/fwlink/?LinkId=272001).

###<a name="monitor-bulk-ingest-progress"></a>Monitor met een groot aantal nemen voortgang

U kunt de voortgang van bulksgewijs bewerkingen voor een IngestManifest ingesting door de eigenschap statistieken van de IngestManifest polling controleren. Deze eigenschap is een complexe type, [IngestManifestStatistics](https://msdn.microsoft.com/library/azure/jj853027.aspx). Als u wilt controleren van de eigenschap statistieken, verzendt u een HTTP GET-aanvraag voor het doorgeven van de IngestManifest Id.
 

##<a name="create-contentkeys-used-for-encryption"></a>ContentKeys gebruikt voor codering maken

Als uw activa versleuteling gebruikt, moet u de ContentKey moet worden gebruikt voor versleuteling voor het maken van de activa-bestanden maken. Voor opslag-codering, moeten de volgende eigenschappen worden opgenomen in de hoofdtekst van het verzoek.
 
Hoofdteksteigenschap aanvragen   | Beschrijving
---|---
ID | De ContentKey-Id die we onszelf genereren met behulp van de volgende indeling "nb:kid:UUID:<NEW GUID>".
ContentKeyType | Dit is het belangrijkste inhoudstype als een geheel getal voor deze inhoud sleutel. We doorgeven de waarde 1 voor opslag-versleuteling.
EncryptedContentKey | Maak een nieuwe inhoud sleutelwaarde die een 256-bits (32-bytetekens waarde). De sleutel is versleuteld met behulp van de opslagruimte versleuteling x.509-certificaat die we uit Microsoft Azure Media Services ophalen door een HTTP GET-aanvraag voor de GetProtectionKeyId en GetProtectionKey methoden uit te voeren.
ProtectionKeyId | Dit is de belangrijkste beveiliging-id voor de opslag versleuteling X.509-certificaat dat is gebruikt voor het coderen van onze inhoud sleutel.
ProtectionKeyType | Dit is het versleutelingstype voor de beveiliging-sleutel die is gebruikt voor het coderen van de inhoud sleutel. Deze waarde is StorageEncryption(1) in ons voorbeeld.
Controlesom |De MD5 berekende controlesom voor de inhoud sleutel. Deze wordt berekend door het coderen van de inhoud Id met de inhoud-toets. De voorbeeldcode ziet u het berekenen van de controlesom.


**HTTP-antwoord**
    
    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 572
    Expect: 100-continue
    
    {"Id" : "nb:kid:UUID:316d14d4-b603-4d90-b8db-0fede8aa48f8", "ContentKeyType" : 1, "EncryptedContentKey" : "Y4NPej7heOFa2vsd8ZEOcjjpu/qOq3RJ6GRfxa8CCwtAM83d6J2mKOeQFUmMyVXUSsBCCOdufmieTKi+hOUtNAbyNM4lY4AXI537b9GaY8oSeje0NGU8+QCOuf7jGdRac5B9uIk7WwD76RAJnqyep6U/OdvQV4RLvvZ9w7nO4bY8RHaUaLxC2u4aIRRaZtLu5rm8GKBPy87OzQVXNgnLM01I8s3Z4wJ3i7jXqkknDy4VkIyLBSQvIvUzxYHeNdMVWDmS+jPN9ScVmolUwGzH1A23td8UWFHOjTjXHLjNm5Yq+7MIOoaxeMlKPYXRFKofRY8Qh5o5tqvycSAJ9KUqfg==", "ProtectionKeyId" : "7D9BB04D9D0A4A24800CADBFEF232689E048F69C", "ProtectionKeyType" : 1, "Checksum" : "TfXtjCIlq1Y=" }

### <a name="link-the-contentkey-to-the-asset"></a>De ContentKey koppelen aan de activa

De ContentKey is gekoppeld aan een of meer activa door een HTTP POST-aanvraag te verzenden. De volgende aanvraag is een voorbeeld in het voorbeeld ContentKey koppelen aan het voorbeeld actief door Id.

**HTTP-antwoord**
    
    POST https://media.windows.net/API/Assets('nb:cid:UUID:b3023475-09b4-4647-9d6d-6fc242822e68')/$links/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 113
    Expect: 100-continue
    
    { "uri": "https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A32e6efaf-5fba-4538-b115-9d1cefe43510')"}

**HTTP-antwoord**

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net



##<a name="next-step"></a>Volgende stap

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



 
[How to Get a Media Processor]: media-services-get-media-processor.md
 
