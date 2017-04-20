<properties 
    pageTitle="Coderen van de inhoud met opslag versleuteling AMS REST API gebruiken" 
    description="Leer hoe u uw inhoud versleutelen met opslag versleuteling met AMS REST API's." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="encrypting-your-content-with-storage-encryption-using-ams-rest-api"></a>Coderen van de inhoud met opslag versleuteling AMS REST API gebruiken

Het wordt ten zeerste aanbevolen voor het coderen van uw inhoud lokaal met AES-256 bitsversleuteling en uploadt dit naar Azure Storage waar deze worden opgeslagen in rust versleuteld.

In dit artikel geeft een overzicht van AMS opslag versleuteling en ziet u hoe u de inhoud opslag versleuteld uploaden:

- Een inhoud sleutel maakt.
- Maak een actief. Stel de AssetCreationOption op StorageEncryption bij het maken van de activa.

     Versleutelde activa moeten zijn gekoppeld aan inhoud toetsen.
- De inhoud sleutel aan de activum koppelen.  
- Stel de versleuteling parameters op de entiteiten AssetFile gerelateerd.
 
>[AZURE.NOTE]Als u een versleutelde activum opslag leveren wilt, moet u van het actief bezorging beleid configureren. Voordat uw activa kan worden streamen, wordt de streaming server Hiermee verwijdert u de opslag-versleuteling en uw met behoud van het beleid opgegeven bezorging streamt. Zie [Beleid voor het configureren van activa bezorging](media-services-rest-configure-asset-delivery-policy.md)voor meer informatie.


>[AZURE.NOTE] Tijdens het werken met de Media Services REST API, zijn de volgende punten van toepassing:
>
>Bij het openen van entiteiten in Media Services, moet u specifieke koptekstvelden en waarden instellen in uw HTTP-aanvragen. Zie [configuratie van voor de ontwikkeling van Media Services REST API](media-services-rest-how-to-use.md)voor meer informatie.

>Nadat de verbinding tot stand naar https://media.windows.net, ontvangt u een 301 omleiding precisie van een andere Media-URI voor Services. Mislukken volgende oproepen naar de nieuwe URI zoals is beschreven in [verbinding maken met Media-Services REST API gebruiken](media-services-rest-connect-programmatically.md), moet u ervoor. 

##<a name="storage-encryption-overview"></a>Opslag versleuteling-overzicht 

De AMS opslag-versleuteling geldt **AES-CTR** modus versleuteling voor het hele bestand.  AES-CTR-modus is een blokcodering die kan worden versleuteld willekeurige lengtegegevens zonder dat u een opvulling. Werkt door te coderen van een item blok met het AES algoritme en vervolgens op ex.of na de uitvoer van AES met de gegevens om te versleutelen of ontsleutelen.  Het item dat wordt gebruikt door de waarde van de InitializationVector kopiëren naar bytes 0 tot en met 7 van de itemwaarde wordt opgesteld en bytes 8 tot 15 van de teller-waarde zijn ingesteld op 0. Van de blokkering van de teller 16 bytes bytes 8 tot 15 (dat wil zeggen de minste aanzienlijk bytes) gebruikt een eenvoudige 64-bits geheel getal zonder voorteken die met 1 voor elke volgende blok met gegevens verhoogd verwerkt en wordt bewaard in volgorde van bytes. Houd er rekening mee dat als deze geheel getal de maximumwaarde (0xFFFFFFFFFFFFFFFF) oplopende vervolgens opnieuw de teller blok nul (bytes 8 tot 15) bereikt zonder de andere 64 bits van het item (dat wil zeggen bytes 0 t/m 7).   Om te behouden de beveiliging van de AES-CTR modus versleuteling, de waarde InitializationVector voor een bepaalde toets-id voor elke inhoud sleutel moet uniek zijn voor elk bestand en bestanden worden minder dan 2 ^ 64 blokken in lengte.  Dit is om ervoor te zorgen dat de itemwaarde van een nooit met een bepaalde toets wordt opnieuw gebruikt. Zie voor meer informatie over de modus CTR [deze wikipagina](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (het wiki-artikel wordt de term "Nonce" in plaats van "InitializationVector").

**Opslag** -versleuteling wissen inhoud lokaal met AES-256 bits codering versleutelen en uploadt dit naar Azure Storage waar deze zijn opgeslagen in rust versleuteld. Activa die zijn beveiligd met opslag versleuteling zijn automatisch niet versleuteld en geplaatst in een versleutelde bestandssysteem vóór codering en desgewenst opnieuw worden versleuteld voordat u het uploadt weer als een nieuwe uitvoer actief. De primaire use-case voor opslag-versleuteling is wanneer u wilt beveiligen van uw hoge kwaliteit voor de invoer mediabestanden met codering in rust op schijf.

Voor het leveren van een versleutelde activum opslag, moet u van het actief bezorging beleid configureren zodat Media Services weet hoe u wilt uw inhoud te geven. Voordat uw activa kan worden streamen, wordt de streaming server Hiermee verwijdert u de opslag-versleuteling en streamt uw met behoud van het opgegeven bezorging beleid (bijvoorbeeld AES, algemene versleuteling of geen codering).

##<a name="create-contentkeys-used-for-encryption"></a>ContentKeys gebruikt voor codering maken

Versleutelde activa moeten zijn gekoppeld aan opslag versleuteling-toets. U moet de inhoud sleutel moet worden gebruikt voor versleuteling voor het maken van de activa-bestanden maken. In deze sectie wordt beschreven hoe een inhoud sleutel maakt.

Hier volgen algemene stappen voor het genereren van inhoud toetsen dat u wilt koppelen aan activa dat moet worden gecodeerd. 

1. Voor opslag-codering, een 32-byte AES-sleutel willekeurig te genereren. 

    Dit is de inhoud sleutel voor uw activa, wat betekent dat alle bestanden die zijn gekoppeld aan dat actief moet dezelfde inhoud sleutel gebruiken tijdens het ontsleutelen. 
2.  Bel de methoden [GetProtectionKeyId](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkeyid) en [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) als u de juiste X.509-certificaat dat moet worden gebruikt voor het coderen van uw inhoud sleutel.
3.  Uw inhoud sleutel met de openbare sleutel van de X.509-certificaat versleutelen. 

    Media Services .NET SDK gebruikt RSA OAEP bij het uitvoeren van de versleuteling.  Hier ziet u een voorbeeld van de .NET van de [functie EncryptSymmetricKeyData](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4.  Maak een controlesomwaarde berekend met behulp van de sleutel-id en de inhoud sleutel. De volgende .NET-functie berekent de controlesom met het gedeelte GUID van de sleutel-id en de Schakel het selectievakje inhoud sleutel.
    

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting the KID
            // with the content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }


5. De inhoud sleutel maken met de **EncryptedContentKey** (geconverteerd naar tekenreeks base64-codering), **ProtectionKeyId** **ProtectionKeyType**, **ContentKeyType**en **controlesom** waarden die u in de vorige stappen hebt ontvangen.

    
    Voor opslag-codering, moeten de volgende eigenschappen worden opgenomen in de hoofdtekst van het verzoek.
     
    Hoofdteksteigenschap aanvragen   | Beschrijving
    ---|---
    ID | De ContentKey-Id die we onszelf genereren met behulp van de volgende indeling "nb:kid:UUID:<NEW GUID>".
    ContentKeyType | Dit is het belangrijkste inhoudstype als een geheel getal voor deze inhoud sleutel. We doorgeven de waarde 1 voor opslag-versleuteling.
    EncryptedContentKey | Maak een nieuwe inhoud sleutelwaarde die een 256-bits (32-bytetekens waarde). De sleutel is versleuteld met behulp van de opslagruimte versleuteling x.509-certificaat die we uit Microsoft Azure Media Services ophalen door een HTTP GET-aanvraag voor de GetProtectionKeyId en GetProtectionKey methoden uit te voeren. Zie de volgende .NET-code als u bijvoorbeeld: de **EncryptSymmetricKeyData** methode gedefinieerd [hier](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | Dit is de belangrijkste beveiliging-id voor de opslag versleuteling X.509-certificaat dat is gebruikt voor het coderen van onze inhoud sleutel.
    ProtectionKeyType | Dit is het versleutelingstype voor de beveiliging-sleutel die is gebruikt voor het coderen van de inhoud sleutel. Deze waarde is StorageEncryption(1) in ons voorbeeld.
    Controlesom |De MD5 berekende controlesom voor de inhoud sleutel. Deze wordt berekend door het coderen van de inhoud Id met de inhoud-toets. De voorbeeldcode ziet u het berekenen van de controlesom.
    

###<a name="retrieve-the-protectionkeyid"></a>De ProtectionKeyId ophalen 
 

Het volgende voorbeeld ziet hoe u kunt de ProtectionKeyId, een certificaatvingerafdruk, voor het certificaat dat moet u bij het coderen van uw inhoud sleutel ophalen. Voer voor deze stap om ervoor te zorgen dat u al het gewenste certificaat op uw computer.


Vraag:
    
    
    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    

Antwoord:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

###<a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>De ProtectionKey voor de ProtectionKeyId terughalen

Het volgende voorbeeld ziet u het ophalen van de X.509-certificaat met de ProtectionKeyId dat u in de vorige stap hebt ontvangen.

Vraag:
        
    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net
    


Antwoord:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-the-content-key"></a>De inhoud sleutel maken 

Nadat u hebt opgehaald van de X.509-certificaat en gebruikt de openbare sleutel voor het coderen van uw inhoud sleutel, maken een entity **ContentKey** en stel de eigenschap waarden wilt opzoeken hand hiervan.

Een van de waarden die u moet in te stellen wanneer de inhoud maken sleutel is het type. Voor het geval de opslag-codering is de waarde '1'. 

Het volgende voorbeeld wordt getoond hoe een **ContentKey** maken met een **ContentKeyType** voor opslag-versleuteling ('1') en de **ProtectionKeyType** ingesteld op '0' om aan te geven dat de beveiliging toets Id vingerafdruk van het X.509-certificaat.  


Aanvragen

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }


Antwoord:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a>Activa maken

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
    
    {"Name":"BigBuckBunny" "Options":1}


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
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
##<a name="associate-the-contentkey-with-an-asset"></a>De ContentKey koppelen aan een activum

Na het maken van de ContentKey koppelen aan uw activa de bewerking $links te gebruiken, zoals wordt weergegeven in het volgende voorbeeld:
    
Vraag:
    
    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Antwoord:

    HTTP/1.1 204 No Content 

##<a name="create-an-assetfile"></a>Een AssetFile maken

De entiteit [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) vertegenwoordigt een video of audio-bestand dat is opgeslagen in een container blob. Een activum-bestand is altijd gekoppeld aan activa en activa kan een of meer activa-bestanden bevatten. De taak Media Services Encoder mislukt als een activum bestand-object niet gekoppeld aan een digitale bestand in een container blob is.

Houd er rekening mee dat het exemplaar **AssetFile** en het werkelijke mediabestand twee afzonderlijke objecten zijn. Het exemplaar AssetFile bevat metagegevens over het mediabestand, terwijl het mediabestand de werkelijke media-inhoud bevat.

Nadat u uw digitale media-bestand naar een container blob uploaden, kunt u het verzoek HTTP **samenvoegen** wilt gebruiken voor het bijwerken van de AssetFile met informatie over het mediabestand (niet weergegeven in dit onderwerp). 

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
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
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
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
