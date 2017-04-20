<properties 
    pageTitle="CENC met Multi-DRM en toegangsbeheer: een verwijzing ontwerp en de implementatie van Azure en Azure mediaservices | Microsoft Azure" 
    description="Informatie over hoe u naar de Microsoft® vloeiende Streaming Client overbrengen Kit-licenties." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan"  
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="willzhan;kilroyh;yanmf;juliako"/>

#<a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>CENC met Multi-DRM en toegangsbeheer: een verwijzing ontwerp en de implementatie van Azure en Azure mediaservices

##<a name="key-words"></a>Kerstkaartsjabloon
 
Azure Active Directory, Azure Media Services, Azure Media Player, dynamische versleuteling licentie bezorging, PlayReady, Widevine, FairPlay, algemene Encryption(CENC), Multi-DRM, Axinom, streepje, EME, MSE, JSON Web Token (JWT), Claims, moderne Browsers, sleutel Rollover, Symmetric toets, asymmetrische sleutel, OpenID verbinding te maken, X509 certificaat. 

##<a name="in-this-article"></a>In dit artikel

De volgende onderwerpen worden beschreven in dit artikel:

- [Inleiding](media-services-cenc-with-multidrm-access-control.md#introduction)
    - [Overzicht van dit artikel](media-services-cenc-with-multidrm-access-control.md#overview-of-this-article)
- [Het ontwerp van een verwijzing](media-services-cenc-with-multidrm-access-control.md#a-reference-design)
- [Ontwerp toewijzen aan technologie voor implementatie](media-services-cenc-with-multidrm-access-control.md#mapping-design-to-technology-for-implementation)
- [Implementatie](media-services-cenc-with-multidrm-access-control.md#implementation)
    - [Implementatie procedures](media-services-cenc-with-multidrm-access-control.md#implementation-procedures)
    - [Sommige weetjes in uitvoering](media-services-cenc-with-multidrm-access-control.md#some-gotchas-in-implementation)
- [Aanvullende onderwerpen voor implementatie](media-services-cenc-with-multidrm-access-control.md#additional-topics-for-implementation)
    - [HTTP- of HTTPS](media-services-cenc-with-multidrm-access-control.md#http-or-https)
    - [Azure Active Directory sleutelrollover ondertekenen](media-services-cenc-with-multidrm-access-control.md#azure-active-directory-signing-key-rollover)
    - [Waar is de Access-Token?](media-services-cenc-with-multidrm-access-control.md#where-is-the-access-token)
    - [Hoe zit Live Streaming?](media-services-cenc-with-multidrm-access-control.md#what-about-live-streaming)
    - [Hoe zit het met licentieservers buiten Azure Media Services?](media-services-cenc-with-multidrm-access-control.md#what-about-license-servers-outside-of-azure-media-services)
    - [Wat gebeurt er als ik wil een aangepaste STS gebruiken?](media-services-cenc-with-multidrm-access-control.md#what-if-i-want-to-use-a-custom-sts)
- [De voltooide systeem en testen](media-services-cenc-with-multidrm-access-control.md#the-completed-system-and-test)
    - [Gebruikersaanmelding](media-services-cenc-with-multidrm-access-control.md#user-login)
    - [Met behulp van versleutelde Media uitbreidingen voor PlayReady](media-services-cenc-with-multidrm-access-control.md#using-encrypted-media-extensipons-for-playready)
    - [Met behulp van EME voor Widevine](media-services-cenc-with-multidrm-access-control.md#using-eme-for-widevine)
    - [Niet in aanmerking komen gebruikers](media-services-cenc-with-multidrm-access-control.md#not-entitled-users)
    - [Aangepaste Secure Token Service wordt uitgevoerd](media-services-cenc-with-multidrm-access-control.md#running-custom-secure-token-service)
- [Overzicht](media-services-cenc-with-multidrm-access-control.md#summary)

##<a name="introduction"></a>Inleiding

Is het bekende dat dit is een complexe taak ontwerpen en bouwen van een subsysteem DRM voor een OTT of online streaming oplossing. En is een algemene gewoonte voor operatoren/online video providers voor het uitbesteden van dit onderdeel als u wilt gespecialiseerde DRM serviceproviders. Het doel van dit document is om aan te bieden een verwijzing-ontwerpen en implementeren van end-to-end DRM subsysteem in OTT of online streaming oplossing.

De gerichte lezers van dit document zijn engineers werken in DRM subsysteem van OTT of online streaming/meerdere-screen oplossingen, of een DRM subsysteem geïnteresseerd lezers. Aangenomen is dat lezers bekend met ten minste één van de technologieën DRM op de markt, zoals PlayReady, Widevine, FairPlay of Adobe Access bent.

Door DRM ook we CENC (algemene codering) met multi-DRM. Een primaire trend in online streaming en OTT industriële is via CENC met meerdere-native-DRM client platform, namelijk een verschuiving van de vorige trend van het gebruik van een enkel DRM en de client SDK voor verschillende clientplatforms. Wanneer u CENC met meerdere native DRM, zijn PlayReady zowel Widevine versleuteld per specificatie van de [Algemene versleuteling (CENC ISO/IEC 23001 tot en met 7)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) .

De voordelen van CENC met multi-DRM zijn als volgt:

1. Hiermee reduceert u versleuteling kosten omdat er een verwerking één versleuteling wordt gebruikt doelgroepen verschillende platforms met de systeemeigen DRMs;
1. Hiermee reduceert u de kosten van het beheer van versleutelde activa, want alleen een enkel exemplaar van versleutelde activa is nodig;
1. Niet meer licenties kosten sinds de systeemeigen DRM-client meestal gratis op de prompt voor systeemeigen platform is DRM-client.

Microsoft is een actieve stof van streepje en CENC samen met enkele belangrijke zakelijke spelers. Microsoft Azure Media Services heeft zijn ondersteuning van streepje en CENC. Zie voor recente aankondigingen van Mingfei blogs: [services voor de bezorging van Google-Widevine aankondigen licentie in Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)en [Azure mediaservices toevoegt Google Widevine verpakking om multi-DRM stream te bieden](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Overzicht van dit artikel

Het doel van dit artikel bevat de volgende:

1. Vindt u het ontwerp van een verwijzing van DRM subsysteem CENC gebruikt met multi-DRM;
1. Biedt een verwijzing-implementatie op Microsoft Azure/Azure Media Services-platform;
1. Wordt beschreven hoe sommige onderwerpen ontwerpen en implementeren.

In het artikel behandelt "multi-DRM" het volgende:

1. Microsoft PlayReady
1. Google-Widevine
1. Apple FairPlay (nog niet ondersteund door Azure Media Services)

De volgende tabel worden de systeemeigen platform/native-app, en browsers die worden ondersteund door elke DRM.

**Client-Platform**|**Systeemeigen DRM ondersteuning**|**Browser-App**|**Streaming-indelingen**
----|------|----|----
**Slim tv's, operator STB's, OTT STB 's**|PlayReady hoofdzakelijk, en/of Widevine en/of andere|Linux, Opera, WebKit, andere|Verschillende indelingen
**Windows 10-apparaten (PC met Windows, Windows-Tablets, Windows Phone, Xbox)**|PlayReady|MS rand/IE11/EME<br/><br/><br/>UWP|STREEPJE (voor HLS, PlayReady wordt niet ondersteund)<br/><br/>STREEPJE, vloeiende Streaming (voor HLS, PlayReady wordt niet ondersteund) 
**Android-apparaten (telefoon, Tablet TV)**|Widevine|Chrome/EME|STREEPJE
**iOS (iPhone, iPad), OS X-clients en Apple TV**|FairPlay|Safari 8 + slash EME|HLS
**Invoegtoepassing: Adobe WIP**|WIP Access|Browser-invoegtoepassing|SCHIJVEN, HLS

Gezien de huidige status van de implementatie van elke DRM, een service, meestal wilt implementeren van 2 of 3 DRMs om ervoor te zorgen dat u alle typen eindpunten beantwoorden in de beste manier.

Er is een verhouding tussen de complexiteit van de service-logica en de complexiteit op de client naar een bepaald niveau van de gebruikerservaring op de verschillende clients hebt bereikt.

Als u de selectie, houd rekening met deze feiten:

- PlayReady is native geïmplementeerd in elke Windows-apparaat op sommige Android-apparaten, en beschikbaar via de software SDK's op vrijwel elk platform
- Widevine is standaard geïmplementeerd in elke Android-apparaat, in Chrome en in sommige andere apparaten
- FairPlay is beschikbaar alleen op iOS- en Mac OS-clients of via iTunes.

Zodat een typisch multi-DRM zich:

- Optie 1: PlayReady en Widevine
- Optie 2: PlayReady, Widevine en FairPlay


## <a name="a-reference-design"></a>Het ontwerp van een verwijzing

In dit gedeelte worden we het ontwerp van een verwijzing naar technologieën die worden gebruikt om dit te implementeren agnostische namelijk presenteren.

Een subsysteem DRM kan de volgende onderdelen bevatten:

1. Key management
1. DRM verpakking
1. DRM licentie bezorging
1. Recht controleren
1. Verificatie
1. Speler
1. Origin/CDN

In het volgende diagram ziet u het hoogste niveau interactie tussen de onderdelen in een subsysteem DRM.

![DRM subsysteem met CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)
 
Er zijn drie eenvoudige 'lagen' in het ontwerp:

1. Een back-office laag (in zwart) die niet beschikbaar zijn extern;
1. "DMZ" laag (blauw), met alle de eindpunten die tegenover elkaar liggen openbare;
1. Openbare internetlaag (Lichtblauw) met CDN en spelers met verkeer via openbare Internet.
 
Moet er een beheer van webinhoud-programma voor het beheer van DRM-beveiliging, ongeacht het statische of dynamische versleuteling is. De ingangen voor DRM versleuteling moeten zijn:

1. MBR videomateriaal;
1. Inhoud sleutel;
1. Licentie acquisition URL's.

Tijdens de periode afspelen is de stroom van hoog niveau:

1. Gebruiker is geverifieerd;
1. Voor Autorisatietoken wordt gemaakt voor de gebruiker.
1. DRM beveiligde inhoud (manifest) wordt gedownload naar speler;
1. Speler dient licentie acquisition aanvraag met licentieservers samen met belangrijke-ID en autorisatie token.

Voordat u verdergaat naar het volgende onderwerp, een paar woorden over het ontwerp van key management.

**ContentKey-naar-activa**|**Scenario**
------|---------------------------
1-op-1|De eenvoudigste hoofdletters/kleine letters. Dit bevat het beste besturingselement. Maar dit gewoonlijk resulteert in de hoogst licentie bezorging kosten. Bij minimaal één licentie is aanvraag vereist voor elke beveiligde activa.
1-op-veel|U kunt dezelfde inhoud sleutel voor meerdere activa. Bijvoorbeeld voor alle activa in een logische groep zoals een nieuwe of een subset van de shape (of een film gen) kunt u een enkele inhoud sleutel.
Veel-op-1|Meerdere inhoud sleutels nodig zijn voor elke activa. <br/><br/>Als u nodig hebt om toe te passen dynamische CENC beveiliging met multi-DRM voor MPEG-streepje en dynamische AES-128-versleuteling voor HLS, moet u bijvoorbeeld twee afzonderlijke inhoud toetsen, elk voorzien van een eigen ContentKeyType. (Voor de inhoud sleutel voor dynamische CENC beveiliging, ContentKeyType.CommonEncryption moeten worden gebruikt, terwijl voor de inhoud sleutel voor dynamische AES-128-versleuteling, ContentKeyType.EnvelopeEncryption moet worden gebruikt.)<br/><br/>Een ander voorbeeld in CENC beveiliging streepje inhoud, in theorie een inhoud sleutel kan worden gebruikt om video-stream en een andere inhoud sleutel voor het beveiligen van audiostroom te beschermen. 
Veel-op - veel|Combinatie van de bovenstaande twee scenario's: één set met inhoud toetsen worden gebruikt voor elk van de meerdere bedrijfsmiddelen in dezelfde activa "groep".


Een andere belangrijke factor u rekening moet houden is het gebruik van licenties permanente en niet-permanente.

Waarom zijn de volgende punten belangrijk? 

Als u openbare cloud voor de bezorging van de licentie gebruikt hebben directe invloed bij licentie bezorging kosten. Laten we eens de volgende twee verschillende Ontwerpeffecten gevallen om te laten zien:



1. Maandabonnement: permanente licentie en 1-op-veel inhoud toets-naar-activa toewijzing gebruiken. Bijvoorbeeld voor alle kinderen films kunnen we een enkele inhoud sleutel voor codering gebruiken. In dit geval: 

    Total # licenties aangevraagd voor alle kinderen films/apparaat = 1

1. Maandabonnement: niet-permanente licentie en 1-1-toewijzing tussen inhoud sleutel en activa gebruiken. In dit geval:

    Total # licenties aangevraagd voor alle kinderen films/apparaat [# films die u wilt controleren] = x [# sessies]

Als u gemakkelijk zien kunt, resultaat de twee verschillende ontwerpen sterk afwijkt licentie verzoek patronen daarom licentie bezorging kosten als bezorgingsservice licentie wordt geleverd door een openbare cloud zoals Azure Media Services.

## <a name="mapping-design-to-technology-for-implementation"></a>Ontwerp toewijzen aan technologie voor implementatie

Vervolgens toewijzen we onze algemene ontwerp aan technologieën op Microsoft Azure/Azure Media Services-platform, door op te geven welke technologie voor elke bouwsteen wilt gebruiken.

De volgende tabel ziet u de toewijzing:

**Bouwstenen**|**Technologie**
------|-------
**Speler**|[Azure MediaPlayer](https://azure.microsoft.com/services/media-services/media-player/)
**Identiteitsprovider (IDP)**|Azure Active Directory
**Secure Token-Service (STS)**|Azure Active Directory
**DRM beveiliging werkstroom**|Azure Media Services dynamische beveiliging
**DRM licentie bezorging**|1. azure Media Services-licentie bezorging (PlayReady, Widevine, FairPlay), of <br/>2. licentieserver Axinom <br/>3. aangepaste PlayReady licentieserver
**Origin**|Azure Media Services Streaming eindpunt
**Key Management**|Niet nodig voor de uitvoering van de verwijzing
**Beheer van webinhoud**|Een C#-console-toepassing

Met andere woorden, zowel identiteit Provider (IDP) en Secure Token-Service (STS) worden Azure AD. Voor speler, we [Azure Media Player-API](http://amp.azure.net/libs/amp/latest/docs/)gebruiken. Ondersteuning streepje en CENC met multi-DRM zowel Azure Media Services en Azure Media Player.

In het volgende diagram ziet u de algehele structuur en stroom met de bovenstaande technologie-toewijzing.

![CENC op AMS](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Om te kunnen dynamische CENC versleuteling hebt ingesteld, maakt het beheer van webinhoud hulpmiddel gebruik van de volgende invoer:

1. Open inhoud;
1. Inhoud sleutel uit toets generatie/management;
1. Licentie acquisition URL's;
1. Een lijst met gegevens van Azure AD.

De uitvoer van het hulpprogramma voor het beheer van webinhoud is:

1. ContentKeyAuthorizationPolicy met de specificatie op hoe de bezorging van de licentie een token JWT en DRM licentie specificaties controleert;
1. AssetDeliveryPolicy die specificaties op streaming bestandsindeling, DRM-beveiliging en licentie acquisition URL's bevat.

Tijdens runtime, de stroom is als hieronder:

1. Na het gebruikersverificatie, wordt een token JWT gegenereerd;
1. Een van de claims opgenomen in het token JWT is 'groepen' claimen met de object-ID van "EntitledUserGroup". Deze claim worden gebruikt voor het doorgeven van 'recht selectievakje'.
1. Speler downloads client manifest van een CENC beveiligde inhoud en "" hij het volgende ziet:
    1. ID, sleutel 
    1. de inhoud is beveiligd, CENC
    1. Licentie acquisition URL's.

1. Speler kunt u een licentie acquisition aanvraag op basis van de browser/DRM ondersteund. In het verzoek van de overname licentie ID sleutel en het token JWT ook worden verzonden. Bezorgingsservice licentie wordt Controleer of het token JWT en de claims opgenomen alvorens de benodigde licentie.

##<a name="implementation"></a>Implementatie

###<a name="implementation-procedures"></a>Implementatie procedures

De uitvoering bevat de volgende stappen uit:

1. Test activa voorbereiden: een video test voor multi-bitrate coderen/pakket gefragmenteerd MP4 in Azure Media Services. In dit actief is niet DRM beveiligd. DRM-beveiliging wordt uitgevoerd door later dynamische beveiliging.
1. Maken van belangrijke-ID en inhoud key (desgewenst van belangrijke zaad). Voor ons doel, key managementsysteem niet nodig omdat we zijn omgaan met slechts één set van belangrijke-ID en inhoud sleutel voor een aantal test activa;
1. Gebruik AMS API voor het configureren van services voor de bezorging van multi-DRM licentie voor de activa test. Als u aangepaste licentieservers door uw bedrijf of leveranciers van uw bedrijf in plaats van de licentie services in Azure Media Services gebruikt, kunt u deze stap overslaan en licentie acquisition-URL's in de stap voor het configureren van de licentie bezorging opgeven. AMS API nodig is om op te geven gedetailleerde configuraties, zoals autorisatie beleidsbeperking, licentie antwoord sjablonen voor verschillende DRM licentie services, enzovoort. Op dit moment kan biedt de Azure-portal niet nog de benodigde gebruikersinterface voor deze configuratie. U kunt API niveau informatie en probeer een code in Julia Kornich van document: [PlayReady gebruiken en/of Widevine dynamische algemene versleuteling](media-services-protect-with-drm.md). 
1. Gebruik AMS API activa bezorging beleid voor het testen van activa configureren. U kunt API niveau informatie en probeer een code in Julia Kornich van document: [PlayReady gebruiken en/of Widevine dynamische algemene versleuteling](media-services-protect-with-drm.md).
1. Maken en configureren van een Azure Active Directory-tenant in Azure;
1. Een paar gebruikersaccounts en groepen maken in uw Azure Active Directory-tenant: u moet ten minste maken 'EntitledUser' groeperen en een gebruiker toevoegen aan deze groep. Gebruikers in deze groep recht selectievakje licentie aanschafkosten geslaagd, waarna gebruikers niet in deze groep kunnen niet worden doorgegeven verificatie controleren en is niet mogelijk om een licentie te verkrijgen. Dat ze lid zijn van deze groep "EntitledUser" is een claim vereist 'groepen' in het JWT token uitgegeven door Azure AD. Deze vereiste claimen moet worden opgegeven in stap voor multi-DRM licentie bezorging services configureren.
1. Een ASP.NET-MVC-app waarin de videospeler host maken. Deze app ASP.NET beschermd met gebruikersverificatie ten opzichte van de Azure Active Directory-tenant. BEGINLETTERS claims worden opgenomen in de access-tokens na gebruikersverificatie verkregen. OpenID verbinding API geschikt is voor deze stap. U moet de volgende NuGet-pakketten installeren:
    - Installatie-pakket Microsoft.Azure.ActiveDirectory.GraphClient
    - Installatie-pakket Microsoft.Owin.Security.OpenIdConnect
    - Installatie-pakket Microsoft.Owin.Security.Cookies
    - Installatie-pakket Microsoft.Owin.Host.SystemWeb
    - Installatie-pakket Microsoft.IdentityModel.Clients.ActiveDirectory
1. Maak een speler [Azure Media Player-API](http://amp.azure.net/libs/amp/latest/docs/)gebruiken. [Azure Media Player ProtectionInfo API](http://amp.azure.net/libs/amp/latest/docs/) kunt u opgeven welke DRM-technologie gebruiken op ander DRM platform.
1. Test-matrix:

**DRM**|**Browser**|**Resultaat voor recht gebruiker**|**Resultaat voor niet in aanmerking komen gebruiker**
---|---|-----|---------
**PlayReady**|MS rand of IE11 op Windows 10|Mislukt|Mislukt
**Widevine**|Chrome op Windows 10|Mislukt|Mislukt
**FairPlay** |TBD||

George Trifonov van Azure Media Services Team heeft geschreven een blog voor het leveren van gedetailleerde stappen bij het instellen van Azure Active Directory voor een ASP.NET-MVC speler-app: [integreren Azure Media Services OWIN MVC op basis van de app met Azure Active Directory en belangrijke contentlevering op basis van claims JWT beperken](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

George heeft ook een blog [JWT token](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)-verificatie in Azure Media Services en dynamische versleuteling geschreven. En hier is zijn [voorbeeld op Azure AD-integratie met belangrijke bezorging Azure Media Services](https://github.com/AzureMediaServicesSamples/Key-delivery-with-AAD-integration/).

Voor meer informatie over Azure Active Directory:

- U vindt de informatie voor ontwikkelaars in [Azure Active Directory-handleiding voor ontwikkelaars](../active-directory/active-directory-developers-guide.md).
- U vindt de informatie voor beheerders in [Beheren Your Azure AD Directory](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>Sommige weetjes in uitvoering

Er zijn enkele 'weetjes' in de implementatie. Hopelijk kunt de volgende lijst met "weetjes" u problemen oplossen in het geval u problemen ondervindt.

1. **Uitgever** URL moet eindigen met **"/"**.  

    **Publiek** moet de speler toepassings-client-ID en moet u **"/"** ook toevoegen aan het einde van de URL van de uitgever.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" /> 

    In [JWT Mediadecoder](http://jwt.calebb.net/), moet u **aud** en **iss** als onder in het token JWT zien:
    
    ![1e gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)


2. Machtigingen voor de toepassing in AAD (op het tabblad configureren van de toepassing) toevoegen. Dit is vereist voor elke toepassing (lokale en geïmplementeerd versies).

    ![2e gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)


3. Gebruik de juiste uitgever bij het instellen van dynamische CENC beveiliging:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>
    
    De volgende, werkt niet:
    
        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />
    
    De GUID is de id AAD-tenant. De GUID vindt u in eindpunten die wordt weergegeven in Azure-portal.

4. Groepslidmaatschap verlenen vorderingen bevoegdheden. Zorg ervoor dat in AAD-toepassing manifest-bestand, we hebben de volgende

    "groupMembershipClaims": "All", (de standaardwaarde is null)

5. Instelling BEGINLETTERS TokenType bij het maken van beperking vereisten.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Sinds de ondersteuning van JWT (AAD) naast het SWT (ACS) toe te voegen, is de standaardwaarde TokenType TokenType.JWT. Als u SWT/ACS gebruikt, moet u naar TokenType.SWT instellen.

## <a name="additional-topics-for-implementation"></a>Aanvullende onderwerpen voor implementatie
Volgende wordt we disconto uss enkele extra onderwerpen in onze ontwerpen en implementeren.

###<a name="http-or-https"></a>HTTP- of HTTPS?

De toepassing van ASP.NET MVC player die we ingebouwd moet ondersteuning voor het volgende:

1. Gebruiker-authenticatie via Azure AD die moet worden onder HTTPS;
1. JWT token exchange tussen client en Azure AD die moet worden onder HTTPS;
1. DRM licentie overname door de client die worden onder HTTPS moet als licentie bezorging is verstrekt door Azure Media Services. Natuurlijk schrijft PlayReady productsuite niet HTTPS voor de bezorging van de licentie. Als de licentieserver PlayReady buiten Azure Media Services, kunnen HTTP of HTTPS kan worden gebruikt.

De ASP.NET-speler-toepassing, daarom HTTPS gebruikt als een goede gewoonte. Dit houdt in de mediaspeler Azure op een pagina onder HTTPS. Voor streaming we HTTP liever, dus we op moet letten gemengde inhoud probleem.

1. Browser kan geen gemengde inhoud. Maar Plug-ins zoals Silverlight en OSMF-invoegtoepassing voor vloeiende en -streepje toestaan. Gemengde inhoud is een bedreiging: dit is vanwege de bedreiging van de mogelijkheid om toe te voegen schadelijke JS die ertoe dat de gegevens van de klant leiden kunnen kunnen lopen.  Browsers dit al dan niet standaard blokkeren en dusverre de enige manier om deze te omzeilen is op de server (oorsprong), toe te staan dat alle domeinen (ongeacht https of http). Dit is waarschijnlijk niet een goed idee op.
1. We gemengde inhoud moet voorkomen: beide HTTP gebruiken of beide HTTPS gebruiken. Als gemengde inhoud wordt afgespeeld, is silverlightSS tech vereist een waarschuwing voor gemengde inhoud wissen. flashSS tech omgaat met gemengde inhoud zonder gemengde inhoud waarschuwing.
1. Als uw streaming eindpunt is gemaakt voor augustus 2014, ondersteuning er geen voor HTTPS. In dit geval Maak en gebruik een nieuw streaming eindpunt voor HTTPS.

In de uitvoering van de verwijzing, voor DRM beveiligde inhoud, worden zowel een toepassing als streaming onder HTTTPS. Voor inhoud, openen hoeft de speler niet verificatie of welke licentie, zodat u hoeft de liberty HTTP of HTTPS gebruiken.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory sleutelrollover ondertekenen

Dit is een belangrijk punt van uw implementatie rekening te houden. Als u geen rekening met dit in uw implementatie houden, wordt het voltooide systeem uiteindelijk vastlopen volledig binnen maximaal 6 weken.

Azure AD worden standaard tot stand vertrouwensrelatie tussen zelf en het gebruik van Azure AD-toepassingen. Azure AD gebruikt met name een ondertekend sleutel die uit een combinatie van openbare en persoonlijke sleutel bestaat. Wanneer Azure AD een beveiligingstoken dat informatie over de gebruiker maakt bevat, wordt deze token ondertekend door Azure AD met bijbehorende persoonlijke sleutel voordat deze wordt verzonden terug naar de toepassing. Om te bevestigen dat de token geldig en dat is wel is van Azure AD is, moet de toepassing van het token handtekening met de openbare sleutel die worden aangeboden door Azure AD die deel van de tenant Federatie metagegevensdocument uitmaakt valideren. Deze openbare sleutel – en de bijbehorende sleutel waaruit deze is afgeleid – is tevens gebruikt voor alle tenants in Azure AD.

Gedetailleerde informatie over Azure AD-sleutelrollover vindt u in het document: [Belangrijke informatie over ondertekening toets Rollover in Azure AD](../active-directory/active-directory-signing-key-rollover.md).

Tussen de [combinatie openbare persoonlijke sleutel](https://login.windows.net/common/discovery/keys/) 

- De persoonlijke sleutel wordt gebruikt door Azure Active Directory om een token JWT; te genereren
- De openbare sleutel wordt gebruikt door een toepassing zoals DRM licentie bezorging Services in AMS om te controleren of de token JWT;
 
Beveiliging hiertoe draait Azure Active Directory regelmatig dit certificaat (elke 6 weken). Voor het geval inbreuk op beveiliging, kan de sleutelrollover elk gewenst moment optreden. De licentie bezorging services in AMS moeten er daarom bijwerken van de openbare sleutel gebruikt zoals Azure AD Hiermee draait u de combinatie van de sleutel, anders token verificatie in AMS mislukt, en geen licentie wordt uitgevoerd. 

Dit is bereikt door in te stellen TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument tijdens het configureren van services voor de bezorging van DRM licentie.

De JWT token stroom is als hieronder:

1.  Azure AD verleent het token JWT met de huidige persoonlijke sleutel voor een geverifieerde gebruiker;
2.  Wanneer een speler kan een CENC met multi-DRM beveiligde inhoud zien, wordt deze eerst het JWT token uitgegeven door Azure AD vinden.
3.  Windows media player wordt licentie acquisition aanvraag met het token JWT verzonden naar licentie bezorging services in AMS;
4.  De licentie bezorging services in AMS wordt de huidige/geldige openbare sleutel uit Azure AD gebruikt om te controleren of de token JWT alvorens licenties.

Services voor de bezorging van DRM licentie wordt altijd worden controleren op de huidige/geldige openbare sleutel uit Azure AD. De openbare sleutel voorstellingstypen met Azure AD worden de sleutel voor een JWT sleutel uitgegeven door Azure AD is geverifieerd.

Wat gebeurt er als de sleutelrollover gebeurt nadat AAD een token JWT genereert maar voordat u de JWT token door spelers met DRM licentie bezorging services in AMS voor verificatie is verzonden? 

Omdat een sleutel kan worden geïmplementeerd op elk moment, is er altijd meer dan één geldige openbare sleutel beschikbaar in het document Federatie-metagegevens. Azure Media Services licentie bezorging de beschikking over een van de toetsen die zijn opgegeven in het document, aangezien één toets spoedig mogelijk worden geïmplementeerd, een andere mogelijk worden de vervangende, enzovoort.

### <a name="where-is-the-access-token"></a>Waar is de Access-Token?

Als u hoe u een web-app een API-app onder [Toepassings-id met OAuth 2.0 Client referenties verlenen](active-directory-authentication-scenarios.md#web-application-to-web-api)belt bekijken, wordt de stroom verificatie is als hieronder:

1.  Een gebruiker is aangemeld bij Azure AD in de webtoepassing (Zie de [Webbrowser naar de webtoepassing](active-directory-authentication-scenarios.md#web-browser-to-web-application).
2.  Het eindpunt van de autorisatie Azure AD leidt de gebruikersagent terug naar de clienttoepassing met een autorisatiecode. De gebruikersagent berekent autorisatiecode met de clienttoepassing redirect URI.
3.  De webtoepassing moet in het bezit van een toegangstoken zodat deze kan op het web API verifiëren en de gewenste resource op te halen. Deze aanbrengt een aanvraag in Azure AD token eindpunt, de referentie, cliënt-ID en-ID-URI API's webtoepassing. Daaraan de autorisatiecode om te bewijzen dat de gebruiker heeft ingestemd.
4.  Azure AD verifieert de toepassing en geeft als resultaat een JWT-toegangstoken die wordt gebruikt om te bellen van het web API.
5.  De webtoepassing wordt het resulterende JWT-toegangstoken via HTTPS gebruikt om toe te voegen van de tekenreeks JWT met een aanduiding 'Vruchtdragende' in de koptekst van de verificatie van de aanvraag op het web API. Het web API is gevalideerd met het token JWT, en als validatie geslaagd is, geeft de gewenste resource.

In deze stroom "toepassings-id" vertrouwt het web API dat de webtoepassing de gebruiker geverifieerd. Om die reden is dit patroon een vertrouwde subsysteem genoemd. Het [diagram op deze pagina](http://msdn.microsoft.com/library/azure/dn645542.aspx/) wordt beschreven hoe de stroom werkt voor het verlenen van autorisatiecode.

Aanschafkosten van de licentie met token beperking volgen we hetzelfde vertrouwde subsysteem patroon. En de bezorgingsservice licentie in Azure Media Services is het web API resource, de 'backend resource"een webtoepassing toegang moet. Waar is dus het toegangstoken?

We zijn daadwerkelijk, toegangstoken ophaalt van Azure AD. Na een geslaagde verificatie, wordt autorisatiecode geretourneerd. De autorisatiecode wordt vervolgens gebruikt, samen met client-ID en app toets, uitwisselen voor toegangstoken. En het toegangstoken is bedoeld voor toegang tot een "aanwijzer" toepassing wijzen of dat staat voor bezorgingsservice Azure Media Services-licentie.

We nodig hebt geregistreerd en configureren van de app "aanwijzer" in Azure AD door de onderstaande stappen:

1.  In de Azure AD-tenant

    - een toepassing (resource) met eenmalige aanmelding URL toevoegen: 

    https://[resource_name].azurewebsites.NET/ en 

    - App-ID-URL: 
    
    https://[aad_tenant_name].onmicrosoft.com/[resource_name]; 
2.  Een nieuwe sleutel voor de resource-app; toevoegen
3.  De app manifest-bestand bijwerken zodat de eigenschap groupMembershipClaims de volgende waarde heeft: "groupMembershipClaims": "All",  
4.  In de Azure AD-app die wijst naar de speler web-app in de sectie 'de machtigingen voor andere toepassingen,' toegevoegd aan de resource dat is toegevoegd in stap 1 hierboven. Controleer onder "gedelegeerde machtigingen" "Access [resource_name]" vinkje. Het resultaat is de web-app-machtigingen om access tokens voor toegang tot de resource-app te maken. U moet deze stap herhalen voor geïmplementeerd zowel lokale versie van de web-app als u met Visual Studio en Azure WebApp ontwikkelt.
    
Het JWT token uitgegeven door Azure AD dus daadwerkelijk het toegangstoken voor toegang tot deze bron "aanwijzer".

### <a name="what-about-live-streaming"></a>Hoe zit Live Streaming?

In het bovenstaande, is onze discussie is gericht op op aanvraag activa. Hoe zit live streaming?

Het goede nieuws is dat u precies hetzelfde ontwerp en implementatie gebruiken kunt voor het beschermen van persoonlijke streaming in Azure Media Services, van de activa die is gekoppeld aan een programma als "VOD activa" behandelen.

Specifiek, is het algemeen bekend dat hiervoor live streaming in Azure Media Services, moet u een kanaal en vervolgens een programma onder het kanaal maken. Als u wilt het programma hebt gemaakt, moet u activa daarin het persoonlijke archief voor het programma te maken. Kan alleen worden aangeboden CENC met multi-DRM-beveiliging van de actieve inhoud, u moet doen, is toe te passen de dezelfde instelling/verwerking aan het activum alsof deze een activum"VOD" was voordat u het programma start.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Hoe zit het met licentieservers buiten Azure Media Services?

Klanten mogelijk vaak hebben geïnvesteerd in licentie van serverfarm in hun eigen gegevens gecentreerd of die worden gehost door DRM-serviceproviders. Gelukkig Azure Media Services en inhoudsbeveiliging, kunt u hybride modus: inhoud die worden gehost en dynamisch beveiligde in Azure Media Services, terwijl DRM licenties worden geleverd door servers buiten Azure Media Services. In dit geval zijn de volgende wijzigingen:

1. De Secure Token-Service moet actie-tokens die worden geaccepteerd en kunnen worden gecontroleerd door de licentie server-farm. De Widevine licentieservers verstrekt door Axinom is bijvoorbeeld een specifieke JWT token waarin 'recht bericht' vereist. Daarom moet u beschikken over van een STS zoals token JWT uitgeven. De auteurs zoals een implementatie hebt voltooid en u vindt de details in het volgende document in [Azure documentatie Center](https://azure.microsoft.com/documentation/): [Axinom gebruiken voor het leveren van Widevine licenties Azure Media Services](media-services-axinom-integration.md). 
1. U is niet meer nodig hebt voor het configureren van bezorgingsservice licentie (ContentKeyAuthorizationPolicy) in Azure Media Services. Wat u moet doen om te leveren van de licentie acquisition URL's (voor PlayReady, Widevine en FairPlay) is wanneer u AssetDeliveryPolicy bij het instellen van CENC met multi-DRM configureert.
 
### <a name="what-if-i-want-to-use-a-custom-sts"></a>Wat gebeurt er als ik wil een aangepaste STS gebruiken?

Er zijn enkele redenen dat een klant u een aangepaste STS (Secure Token Service) gebruiken wilt mogelijk voor het leveren van JWT tokens. Sommige van deze zijn:

1.  De identiteit Provider (IDP) die wordt gebruikt door de klant biedt geen ondersteuning voor STS. Een aangepaste STS mogelijk in dit geval een optie.
2.  Meer flexibele of betere besturingselement in STS integreren met van klant abonnee facturering systeem mogelijk moet u de klant. Een operator MVPD bieden bijvoorbeeld meerdere OTT abonnee-pakketten zoals premium, eenvoudige, sport, enzovoort. De operator wilt mogelijk overeenkomen met de claims in een token met pakket van een abonnee zodat alleen de inhoud in het juiste pakket beschikbaar is gemaakt. In dit geval een aangepaste STS vindt u de benodigde flexibiliteit en controle.

Twee moet worden gewijzigd wanneer u een aangepaste STS gebruikt:

1.  Tijdens het configureren van de bezorgingsservice licentie voor activa, moet u de beveiligingssleutel voor verificatie wordt gebruikt door de aangepaste STS (meer details hieronder) in plaats van de huidige sleutel uit Azure Active Directory opgeven.
2.  Wanneer een token JTW wordt gegenereerd, een beveiligingssleutel is opgegeven in plaats van de persoonlijke sleutel van de huidige X509 certificaat in Azure Active Directory.

Er zijn twee soorten sleutels:

1.  Symmetric sleutel: dezelfde sleutel wordt gebruikt voor zowel genereren en bevestigt een token JWT;
2.  Asymmetrische-toets: twee openbare en persoonlijke sleutels in een X509 certificaat wordt gebruikt met persoonlijke sleutel voor het coderen/genereren van een token JWT en de openbare sleutel voor de sleutel is geverifieerd.

####<a name="tech-note"></a>Opmerking van technische

Als u .NET Framework / C# als uw ontwikkelingsplatform, de X509 certificaat dat wordt gebruikt voor asymmetrische beveiligingssleutel moet ten minste 2048 lengte hebben. Dit is een vereiste van de klasse System.IdentityModel.Tokens.X509AsymmetricSecurityKey in .NET Framework. Anders wordt de volgende uitzondering worden gegenereerd:

IDX10630: De System.IdentityModel.Tokens.X509AsymmetricSecurityKey voor het ondertekenen van mogen niet groter zijn dan '2048' bits zijn. 

## <a name="the-completed-system-and-test"></a>De voltooide systeem en testen

We doorlopen die een paar scenario's in de voltooide end-to-end-systeem zodat de lezers een basic ": een interactief experiment van het gedrag voordat u een aanmeldingsaccount kunnen hebben.

De webtoepassing speler en bijbehorende login vindt u [hier](https://openidconnectweb.azurewebsites.net/).

Als wat u nodig hebt, "niet-geïntegreerde" scenario is: video activa die worden gehost in Azure Media Services die zijn opgeheven hetzij of DRM beveiligde maar zonder token verificatie (uitgifte een licentie aan iedereen aangevraagd), kunt u deze testen zonder login (door over te schakelen naar HTTP als uw video streaming via HTTP is).

Als wat u nodig hebt, de geïntegreerde scenario end-to-end is: video activa is onder dynamische DRM-beveiliging in Azure Media Services, met token verificatie en JWT token wordt gegenereerd door Azure AD, moet u zich aanmeldt.

### <a name="user-login"></a>Gebruikersaanmelding

Om te testen het end-to-end geïntegreerde DRM-systeem, moet u beschikken over een 'account' gemaakt of toegevoegd. 

Welk account?

Hoewel Azure toegang oorspronkelijk alleen door gebruikers van Microsoft-account, kunt deze nu toegang door gebruikers van beide systems. Dit is gebeurd doordat alle de eigenschappen van Azure vertrouwen Azure AD voor verificatie, met Azure AD organisatie-gebruikers, en door te maken van een relatie Federatie waarbij Azure AD vertrouwt het Microsoft-account consumenten identiteit systeem om consumenten gebruikers te verifiëren. Azure AD is hierdoor kunnen worden geverifieerd 'gast' Microsoft-accounts, evenals "systeemeigen" Azure AD-accounts.

Aangezien Azure AD Microsoft-Account (MSA) domein vertrouwt, kunt u uw accounts uit een van de volgende domeinen naar de aangepaste Azure AD tenant en gebruikt u het account aan aanmelding kunt toevoegen:

**Domeinnaam**|**Domein**
-----------|----------
**Aangepaste Azure AD-tenant domein**|somename.onmicrosoft.com
**Bedrijfsdomein**|Microsoft.com
**Microsoft-Account (MSA) domein**|Outlook.com, live.com, hotmail.com

U kunt contact met een van de auteurs te hebben een account hebt gemaakt of toegevoegd voor u. 

Hieronder vindt u de schermafbeeldingen van verschillende aanmeldingspagina's die worden gebruikt door een ander domeinaccounts.

**Aangepaste Azure AD-tenantaccount domein**: In dit geval u ziet dat de aangepaste aanmeldingspagina van de aangepaste Azure AD-tenant domein.

![Aangepaste Azure AD-tenantaccount domein](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Microsoft-domeinaccount met smartcard**: In dit geval u ziet dat de aanmeldingspagina aangepast door Microsoft zakelijke IT met tweeledige verificatie.

![Aangepaste Azure AD-tenantaccount domein](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Microsoft-Account (MSA)**: In dit geval ziet u de aanmeldingspagina van Microsoft-Account voor consumenten.

![Aangepaste Azure AD-tenantaccount domein](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Met behulp van versleutelde Media uitbreidingen voor PlayReady

Klik op een modern browser met versleuteld Media extensies (EME) voor PlayReady-ondersteuning, zoals Internet Explorer 11 in Windows 8.1 en omhoog en de browser Microsoft Edge op Windows 10 is PlayReady de onderliggende DRM voor EME.

![Met behulp van EME voor PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

Het gebied donker speler is vanwege het feit dat PlayReady beveiliging voorkomt dat een schermopname van beveiligde video aanbrengen. 

Het volgende scherm ziet u de speler-Plug-ins en MSE/EME ondersteuning.

![Met behulp van EME voor PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

EME in Microsoft Edge en IE-11 op Windows 10 kunt aanroepen van [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) op Windows 10-apparaten die dit ondersteunen. U ontgrendelt PlayReady SL3000 de stroom van uitgebreide premium-inhoud (4K, Kopregel, enzovoort) en nieuwe inhoud leveren modellen (vroege venster voor uitgebreide inhoud).

Richten op de Windows-apparaten: PlayReady is de enige DRM in de HW beschikbaar op apparaten met Windows (PlayReady SL3000). Een streaming service kan PlayReady via EME of via een toepassing UWP gebruiken en een hogere videokwaliteit met PlayReady SL3000 dan een ander DRM bieden. 2K inhoud wordt meestal flow tot en met de Chrome of Firefox en 4K inhoud via Microsoft Edge/IE11 of een toepassing UWP op hetzelfde apparaat (afhankelijk van de service-instellingen en -implementatie).


#### <a name="using-eme-for-widevine"></a>Met behulp van EME voor Widevine

Klik op een modern browser met EME/Widevine support, zoals Chrome 41 + op Windows 10, Windows 8.1, Mac OSX Yosemite en Chrome op Android 4.4.4, is Google Widevine de DRM achter EME.

![Met behulp van EME voor Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

U ziet dat Widevine voorkomt niet dat een schermopname maken van aanbrengen beveiligd video.

![Met behulp van EME voor Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Niet in aanmerking komen gebruikers

Als een gebruiker is niet een lid van groep "Gebruikers recht", de gebruiker worden niet kan worden "recht" en de service van de licentie multi-DRM weigert te uitgifte van de gevraagde licentie, zoals hieronder wordt weergegeven. De gedetailleerde beschrijving is ' in het bezit van licentie is mislukt ", welke zoals bedoeld is.

![Niet in aanmerking komen gebruikers](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Aangepaste Secure Token Service wordt uitgevoerd

Voor het scenario van het uitvoeren van aangepaste Secure Token Service (STS), wordt het token JWT uitgevoerd door de aangepaste STS met op symmetric of asymmetrische toets. 

De hoofdletters/kleine letters van het gebruik van de symmetric sleutel (met Chrome):

![Aangepaste STS uitgevoerd](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

De hoofdletters/kleine letters van het gebruik van asymmetrische sleutel via een X509-certificaat (via Microsoft moderne browser).

![Aangepaste STS uitgevoerd](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

In beide gevallen blijft gebruikersverificatie hetzelfde – via Azure AD. De enige verschil is dat JWT tokens worden uitgegeven door de aangepaste STS in plaats van Azure AD. Natuurlijk tijdens het configureren van dynamische CENC beveiliging, de beperking van licentie bezorgingsservice geeft het type JWT token, op symmetric of asymmetrische sleutel.

## <a name="summary"></a>Overzicht

In dit document, we CENC met meerdere native DRM en toegangsbeheer via token verificatie besproken: het ontwerp en de uitvoering met Azure, Azure Media Services en Azure Media Player.

- Het ontwerp van een verwijzing wordt gepresenteerd waarin alle benodigde onderdelen in een subsysteem DRM/CENC;
- De implementatie van een verwijzing op Azure, Azure Media Services en Azure Media Player.
- Er worden ook rechtstreeks bij de opzet en implementatie de volgende onderwerpen besproken.


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Bevestigingen 

William Zhang, Mingfei Yan, Roland bestand Frank, Kilroy Hughes, Julia Kornich
