<properties 
    pageTitle="Inhoud belangrijke autorisatiebeleid met behulp van Media Services .NET SDK configureren | Microsoft Azure" 
    description="Leer hoe u een autorisatiebeleid inhoud sleutels met behulp van Media Services .NET SDK configureren." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;mingfeiy"/>



# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a>Dynamische versleuteling: inhoud belangrijke autorisatiebeleid configureren

[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

##<a name="overview"></a>Overzicht

Microsoft Azure Media Services kunt u voor het leveren van de MPEG-streepje, vloeiende Streaming en HTTP-Live-Streaming (HLS) streams die zijn beveiligd met geavanceerde versleuteling Standard (AES) (via 128-bit versleuteling toetsen) of [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS kunt u ook voor het leveren van streepje streams versleuteld met Widevine DRM. PlayReady zowel Widevine zijn versleuteld per specificatie van de algemene versleuteling (CENC ISO/IEC 23001 tot en met 7).

Media Services bevat ook een **Toets/licentie bezorgingsservice** waaruit clients AES-sleutels of PlayReady/Widevine licenties voor het afspelen van de versleutelde inhoud kunnen ophalen.

Als u voor Media Services wilt voor het coderen van activa, moet u een versleutelingssleutel (**CommonEncryption** of **EnvelopeEncryption**) koppelen aan het activum (zoals beschreven [hier](media-services-dotnet-create-contentkey.md)) en autorisatie beleidsregels voor de sleutel ook configureren (zoals beschreven in dit artikel).

Een stroom door een speler is aangevraagd, gebruikt Media Services de opgegeven sleutel dynamisch versleutelen uw inhoud AES of DRM-versleuteling gebruikt. Als u wilt versleutelen de stream, wordt de speler de toets vraag aan de belangrijkste bezorgingsservice. Om te bepalen of de gebruiker is gemachtigd om de toets, evalueert de service de autorisatie beleidsregels die u hebt opgegeven voor de sleutel.

Media Services ondersteunt meerdere manieren gebruikers die belangrijke aanvragen te verifiëren. Het autorisatiebeleid belangrijke voor inhoud kan zijn een of meer autorisatie-beperkingen: **openen** of **token** beperking. Het beleid voor token beperkte vergezeld van een token dat is uitgegeven door een Secure Token Service (STS). Media Services ondersteunt tokens in de **Eenvoudige Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) en **JSON Web Token** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3))-indeling.

Media Services biedt geen Secure Token Services. U kunt een aangepaste STS maken of gebruikmaken van Microsoft Azure ACS naar probleem tokens. De STS moeten worden geconfigureerd voor het maken van een token ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie token beperking (zoals beschreven in dit artikel). De belangrijkste bezorgingsservice Media Services wordt de sleutel terug naar de client als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de inhoud sleutel.

Zie voor meer informatie

[JWT token authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integreren Azure Media Services OWIN MVC op basis van de app met Azure Active Directory en belangrijke contentlevering op basis van claims JWT beperken](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Gebruik Azure ACS naar probleem tokens](http://mingfeiy.com/acs-with-key-services).

###<a name="some-considerations-apply"></a>Enkele punten van toepassing:

- Als u wilt kunnen dynamische verpakking en dynamische versleuteling gebruiken, moet u ervoor zorgen dat ten minste één streaming gereserveerde per eenheid. Zie [hoe u de schaal van een Media-Service](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.
- Uw activa moet een reeks geavanceerde bitsnelheid MP4s of geavanceerde bitsnelheid vloeiende Streaming-bestanden bevatten. Zie [coderen activa](media-services-encode-asset.md)voor meer informatie.
- Uploaden en uw activa met de optie **AssetCreationOptions.StorageEncrypted** coderen.
- Als u van plan bent om meerdere inhoud toetsen die opgevolgd dezelfde beleidsconfiguratie moeten, moet het is raadzaam een enkele vergunning-beleid maken en deze opnieuw gebruiken met meerdere inhoud toetsen.
- De toets bezorgingsservice slaat ContentKeyAuthorizationPolicy en de bijbehorende gerelateerde objecten (beleidsopties en beperkingen) gedurende 15 minuten.  Als u een ContentKeyAuthorizationPolicy maken en geef een "Token" beperking worden gebruikt, test deze, en klikt u vervolgens het beleid bijwerken naar 'Openen' beperking, duurt het ongeveer 15 minuten voordat het beleid schakelt u naar de "Geopende" versie van het beleid.
- Als u toevoegen of van uw actief bezorging beleid bijwerken, moet u een bestaande locator verwijderen (indien aanwezig) en een nieuwe locator maken.
- Op dit moment niet kunt u schijven streaming format versleutelen of progressief downloads.


##<a name="aes-128-dynamic-encryption"></a>Dynamische AES-128-versleuteling

###<a name="open-restriction"></a>Open beperking

Open beperking betekent dat het systeem de sleutel voor iedereen die een belangrijke aanvraag gaat geven. Deze beperking kan handig zijn voor testdoeleinden.

In het volgende voorbeeld wordt gemaakt van een open vergunning beleid en toegevoegd aan de inhoud-toets.

statische openbare void AddOpenAuthorizationPolicy (IContentKey contentKey) {/ / ContentKeyAuthorizationPolicy maken met Open beperkingen worden ingeschakeld / / en autorisatiebeleid IContentKeyAuthorizationPolicy-beleid maken = _context.
ContentKeyAuthorizationPolicies.
CreateAsync ("Open vergunning beleid"). Resultaat;

Lijst<ContentKeyAuthorizationPolicyRestriction> beperkingen nieuwe lijst =<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };
    
        restrictions.Add(restriction);
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }


###<a name="token-restriction"></a>Token beperking

In deze sectie wordt beschreven hoe inhoud belangrijke autorisatiebeleid maken en koppelen aan de inhoud-toets. Het beleid wordt beschreven welke vereisten autorisatie moeten worden voldaan om te bepalen of de gebruiker is gemachtigd voor het ontvangen van de sleutel (bijvoorbeeld doet de lijst "verificatie toets" bevatten de sleutel die het token is ondertekend met).

Als u wilt de beperkingsoptie token configureren, moet u een XML gebruiken om te beschrijven van het token autorisatie vereisten. De configuratie token beperking XML-moet voldoen aan de volgende XML-schema.

####<a id="schema"></a>Token beperking schema
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Bij het configureren van de **token** beperkt beleid, moet u de primaire** sleutel voor verificatie**, **uitgever** en **publiek** parameters. De **verificatie van primaire sleutel **bevat de sleutel die het token is ondertekend met, **uitgever** is de beveiligde token service die het token problemen. Het **publiek** (ook wel **bereik**genoemd) worden de bedoeling het token of de resource het token gemachtigd voor toegang tot. De belangrijkste bezorgingsservice Media Services is gevalideerd dat deze waarden in het token overeenkomen met de waarden in de sjabloon. 

Wanneer u een **Media Services SDK voor .NET**gebruikt, kunt u de klasse **TokenRestrictionTemplate** om de beperking token te genereren.
Het volgende voorbeeld wordt een beleid gemaakt met een token beperking. In dit voorbeeld de client zou moeten een token met presenteren: ondertekening sleutel (VerificationKey), een token uitgever en vereiste claims.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };
    
        restrictions.Add(restriction);
    
        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    
        return tokenTemplateString;
    }
    
    static private string GenerateTokenRequirements()
    {
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
    
        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
    
        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
    
        return TokenRestrictionTemplateSerializer.Serialize(template);
    }

####<a id="test"></a>Test token

Ga als volgt te werk als u een test token op basis van een beperking voor de token dat is gebruikt voor het autorisatiebeleid belangrijke.
    
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
    
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


##<a name="playready-dynamic-encryption"></a>PlayReady dynamisch versleuteling 

Media-Services kunt u voor het configureren van de rechten en beperkingen die u voor PlayReady DRM runtime afdwingen wilt wanneer een gebruiker probeert te laten voorlezen beveiligde inhoud. 

Wanneer de inhoud met PlayReady beveiligt, is een van de dingen die u wilt opgeven in het autorisatiebeleid voor een XML-tekenreeks die de [PlayReady licentie sjabloon](media-services-playready-license-template-overview.md)definieert. In Media Services SDK voor .NET kunt de klassen **PlayReadyLicenseResponseTemplate** en **PlayReadyLicenseTemplate** u de sjabloon van de licentie PlayReady definiëren.

[Dit onderwerp](media-services-protect-with-drm.md) wordt uitgelegd hoe u uw inhoud met **PlayReady** en **Widevine**versleutelen.

###<a name="open-restriction"></a>Open beperking
    
Open beperking betekent dat het systeem de sleutel voor iedereen die een belangrijke aanvraag gaat geven. Deze beperking kan handig zijn voor testdoeleinden.

In het volgende voorbeeld wordt gemaakt van een open vergunning beleid en toegevoegd aan de inhoud-toets.

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {
    
        // Create ContentKeyAuthorizationPolicy with Open restrictions 
        // and create authorization policy          
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Open", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                Requirements = null
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
    
    
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

###<a name="token-restriction"></a>Token beperking

Als u wilt de beperkingsoptie token configureren, moet u een XML gebruiken om te beschrijven van het token autorisatie vereisten. De configuratie token beperking XML-moet voldoen aan de XML-schema wordt weergegeven in [deze](#schema) sectie.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
                
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    
        return tokenTemplateString;
    }
    
    static private string GenerateTokenRequirements()
    {
    
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
    
        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
    
    
        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
    
        return TokenRestrictionTemplateSerializer.Serialize(template);
    } 
    
    static private string ConfigurePlayReadyLicenseTemplate()
    {
        // The following code configures PlayReady License Template using .NET classes
        // and returns the XML string.

        //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
        //It contains a field for a custom data string between the license server and the application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // to be returned to the end users. 
        //It contains the data on the content key in the license and any rights or restrictions to be 
        //enforced by the PlayReady DRM runtime when using the content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether the license is persistent (saved in persistent storage on the client) 
        //or non-persistent (only held in memory while the player is using the license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
       
        // AllowTestDevices controls whether test devices can use the license or not.  
        // If true, the MinimumSecurityLevel property of the license
        // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
        // It grants the user the ability to playback the content subject to the zero or more restrictions 
        // configured in the license and on the PlayRight itself (for playback specific policy). 
        // Much of the policy on the PlayRight has to do with output restrictions 
        // which control the types of outputs that the content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
        //then the DRM runtime will only allow the video to be displayed over digital outputs 
        //(analog video outputs won’t be allowed to pass the content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
        // If the output protections are configured too restrictive, 
        // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }


Als u een test token op basis van een beperking voor de token dat is gebruikt voor de belangrijkste autorisatie Zie beleid [in deze](#test) sectie. 

##<a id="types"></a>Typen die worden gebruikt bij het definiëren van ContentKeyAuthorizationPolicy

###<a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType

    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

###<a id="TokenType"></a>TokenType

    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Volgende stap
Nu dat u beleid van inhoud sleutel hebt geconfigureerd, gaat u naar het onderwerp van [het configureren van activa bezorging beleid](media-services-dotnet-configure-asset-delivery-policy.md) .
 
