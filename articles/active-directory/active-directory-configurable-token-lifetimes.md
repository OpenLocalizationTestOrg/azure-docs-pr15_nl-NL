<properties
   pageTitle="Configureerbare Token levensduur in Azure Active Directory | Microsoft Azure"
   description="Deze functie wordt gebruikt door beheerders en ontwikkelaars om op te geven van de levensduur van tokens uitgegeven door Azure AD."
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/06/2016"
   ms.author="billmath"/>


# <a name="configurable-token-lifetimes-in-azure-active-directory-public-preview"></a>Configureerbare Token levensduur in Azure Active Directory (openbare Preview)

>[AZURE.NOTE]
>Deze functie is momenteel in openbare preview.  U moet ervoor dat herstellen of verwijderen van wijzigingen.  Deze functie voor iedereen in de openbare voorbeeldweergave proberen we opent, echter een [Azure AD Premium-abonnement](active-directory-get-started-premium.md) wanneer deze in het algemeen beschikbaar op bepaalde aspecten kunnen vragen.


## <a name="introduction"></a>Inleiding
Deze functie wordt gebruikt door beheerders en ontwikkelaars om op te geven van de levensduur van tokens uitgegeven door Azure AD. Token levensduur kunnen worden geconfigureerd voor alle apps in een tenant, voor een toepassing meerdere tenant of voor een specifieke Service Principal in een tenant.

Een beleidsobject vertegenwoordigt in Azure AD, een set regels afgedwongen op afzonderlijke toepassingen of alle toepassingen in een tenant.  Elk type beleid heeft een unieke structuur met een set met eigenschappen die vervolgens worden toegepast op objecten waaraan ze zijn toegewezen.

Een beleid kan worden aangewezen als de standaardweergave voor een tenant. Dit beleid kracht klikt u vervolgens op een willekeurige-toepassing die zich bevindt in tenant zo lang maken als deze niet wordt opgeheven door een beleid met een hogere prioriteit. Beleid kan ook worden toegewezen aan specifieke toepassingen. De volgorde van prioriteit verschilt per beleidstype.

Levensduur van tokens beleid kunnen worden geconfigureerd voor het vernieuwen tokens, access tokens sessie tokens en ID tokens.


### <a name="access-tokens"></a>Access tokens
Een toegangstoken wordt gebruikt door een client voor toegang tot een beveiligde bron. Een toegangstoken kan alleen worden gebruikt voor een specifieke combinatie van de gebruiker, client en resource. Access tokens kunnen niet worden ingetrokken en totdat ze aflopen geldig zijn. Een schadelijke acteur die een toegangstoken verkregen kunt gebruiken voor zover van de levensduur.  Aanpassen levensduur van de access-tokens is een verhouding tussen de systeemprestaties verbeteren en verbetering van de hoeveelheid tijd dat de client access behoudt nadat het account van de gebruiker is uitgeschakeld.  Verbeterde systeemprestaties wilt verbeteren wordt door te verminderen van het aantal keren die een client moet in het bezit van een vers toegangstoken bereikt. 

### <a name="refresh-tokens"></a>Tokens vernieuwen
Wanneer een client een toegangstoken verkrijgt voor toegang tot een beveiligde bron, wordt deze ontvangt zowel een token vernieuwen als een toegangstoken. Het vernieuwen token wordt gebruikt voor nieuwe access/vernieuwen token paren als het huidige toegangstoken verloopt. Vernieuwen tokens zijn afhankelijk van de combinaties van client en gebruiker. Ze kunnen worden ingetrokken en hun geldigheid is ingeschakeld telkens wanneer ze worden gebruikt.

Het is belangrijk onderscheid tussen vertrouwelijke en openbare clients maken. Vertrouwelijke clients zijn toepassingen die kunnen veilig worden opgeslagen het wachtwoord van een client, zodat ze om te bewijzen dat aanvragen afkomstig zijn van de clienttoepassing en niet een schadelijke acteur. Als deze stromen veiliger, de standaardlevensduur van vernieuwen tokens uitgegeven aan deze stromen hoger zijn en niet kunnen worden gewijzigd met beleid worden.

Vanwege de beperkingen van de omgeving waarin de toepassingen worden uitgevoerd in, openbare clients kunnen het wachtwoord van een client veilig worden opgeslagen. Beleid kunnen worden ingesteld op resources om te voorkomen dat vernieuwen tokens afgeleid van openbare clients die ouder zijn dan een bepaalde termijn het verkrijgen van een nieuwe access/vernieuwen token paar (vernieuwen Token Max niet-actieve tijd).  Beleid kunnen ook worden gebruikt voor het instellen van een bepaalde periode waarboven het vernieuwen tokens niet langer zijn geaccepteerd (Token Max Age vernieuwen).  Levensduur van aanpassen vernieuwen tokens kunt u aangeven wanneer en hoe vaak de gebruiker verplicht is voor het Voer de referenties in plaats van stilte opnieuw geverifieerde wordt bij gebruik van een openbare clienttoepassing.


### <a name="id-tokens"></a>ID tokens
ID tokens worden doorgegeven aan websites en systeemeigen clients en profielgegevens over een gebruiker bevatten. Een token ID is gekoppeld aan een specifieke combinatie van gebruikers en -client. ID tokens worden beschouwd als geldige tot datum.  Normaal gesproken vergt een webtoepassing overeenkomt met een gebruiker bevindt zich de levensduur van de sessie in de toepassing op de levensduur van het ID-token uitgegeven voor de gebruiker.  Levensduur van aanpassen ID-tokens kunt u aangeven hoe vaak de webtoepassing wordt de toepassingssessie verloopt en de gebruiker moet opnieuw worden geverifieerd met Azure AD (stilte of interactief).

### <a name="single-sign-on-session-token"></a>Eenmalige aanmelding sessietoken
Wanneer een gebruiker wordt geverifieerd met Azure AD, een sessie voor eenmalige aanmelding tot stand is gebracht met de browser en Azure AD van de gebruiker.  De eenmalige aanmelding sessie toegangstoken, in de vorm van een cookie, staat voor deze sessie. Het is belangrijk te weten dat het SSO sessietoken niet is gekoppeld aan een specifieke resource-/ clienttoepassing. Eenmalige aanmelding sessie tokens kunnen worden ingetrokken en hun geldigheid is ingeschakeld telkens wanneer ze worden gebruikt.

Er zijn twee soorten SSO sessie tokens. Permanente sessie tokens worden opgeslagen als permanente cookies door de browser en niet-permanente sessie tokens als sessiecookies (deze worden verwijderd wanneer de browser is gesloten) zijn opgeslagen.

Tijdelijke sessie tokens hebben een levensduur van 24 uur dat permanente tokens 180 dagen de levensduur hebben. Elk gewenst moment die de SSO sessietoken wordt gebruikt in de geldigheidsperiode is de geldigheidsperiode uitgebreid een andere 24 uur of 180 dagen. Als de SSO-sessie token niet binnen de geldigheidsperiode wordt gebruikt deze wordt beschouwd als verlopen en niet meer worden geaccepteerd. 

Beleid kunnen worden gebruikt voor het instellen van een periode na de eerste sessie token is uitgegeven waarboven de sessie tokens niet langer zijn geaccepteerd (sessie Token Max Age).  Levensduur van aanpassen sessie tokens kunt u aangeven wanneer en hoe vaak de gebruiker is vereist voor referenties in plaats van stilte wordt geverifieerd bij gebruik van een webtoepassing opnieuw in te voeren.

### <a name="token-lifetime-policy-properties"></a>Levensduur van tokens beleidseigenschappen
Een beleid levensduur van tokens is een type beleid-object dat de levensduur van tokens regels bevat.  De eigenschappen van het beleid worden gebruikt om te bepalen van de opgegeven token levensduur.  Als er geen beleid is ingesteld, wordt de waarde voor de standaard-levensduur afgedwongen in het systeem.


### <a name="configurable-token-lifetime-properties"></a>Levensduur van configureerbare tokens eigenschappen
Eigenschap|Tekenreeks van de eigenschap beleid|Van invloed is op|Standaard|Minimum|Maximum|
----- | ----- | ----- |----- | ----- | ----- |
Levensduur van tokens van Access|  AccessTokenLifetime|Toegang tot tokens, ID tokens, SAML2 tokens|1 uur|10 minuten|1 dag|
Niet-actieve tijd Token Max vernieuwen|    MaxInactiveTime |Tokens vernieuwen |14 dagen|10 minuten|    90 dagen|
Enkel-Factor vernieuwen Token Max leeftijd|    MaxAgeSingleFactor  |Vernieuwen tokens (voor alle gebruikers) |90 dagen|10 minuten |Tot ingetrokken *|
Meervoudige vernieuwen Token Max leeftijd| MaxAgeMultiFactor|  Vernieuwen tokens (voor alle gebruikers)| 90 dagen|10 minuten|Tot ingetrokken *|
Enkel-Factor sessie Token Max leeftijd |MaxAgeSessionSingleFactor **    |Sessie tokens (permanente en niet-permanente)| Tot ingetrokken   |10 minuten |Tot ingetrokken *|
Meervoudige sessie Token Max leeftijd| MaxAgeSessionMultiFactor ***|    Sessie tokens (permanente en niet-permanente)| Tot ingetrokken|  10 minuten| Tot ingetrokken *



- * 365 dagen is de maximale expliciete lengte die kan worden ingesteld voor deze kenmerken.
- ** Deze waarde heeft de waarde de waarde MaxAgeSingleFactor als MaxAgeSessionSingleFactor niet is ingesteld. Als geen van beide parameter is ingesteld, gaat de eigenschap op de standaardwaarde (tot ingetrokken).
- Deze waarde heeft de waarde de waarde MaxAgeMultiFactor als MaxAgeSessionMultiFactor niet is ingesteld. Als geen van beide parameter is ingesteld, gaat de eigenschap op de standaardwaarde (tot ingetrokken).

### <a name="exceptions"></a>Uitzonderingen
Eigenschap|Van invloed is op|Standaard|
----- | ----- | ----- |
Vernieuwen Token Max niet-actieve tijd (federatieve gebruikers met onvoldoende intrekkingsgegevens)|Vernieuwen tokens (verleend voor federatieve gebruikers met onvoldoende intrekkingsgegevens)|12 uur|
Vernieuwen Token Max niet-actieve tijd (vertrouwelijke Clients)| Vernieuwen tokens (verleend voor vertrouwelijke Clients)|90 dagen|
Vernieuwen token Max Age (verleend voor vertrouwelijke Clients) |   Vernieuwen tokens (verleend voor vertrouwelijke Clients) |Tot ingetrokken

### <a name="priority-and-evaluation-of-policies"></a>Prioriteit en evaluatie van beleid

Beleidsregels voor token levensduur worden gemaakt en toegewezen aan specifieke toepassingen, tenants en service principes. Dit betekent dat het is mogelijk voor meerdere beleidsregels wilt toepassen op een specifieke toepassing. Het beleid levensduur van tokens die wordt pas van kracht, voert u deze regels:


- Als u een beleid expliciet is toegewezen aan de service-principal, worden deze afgedwongen. 
- Als er geen beleid expliciet is toegewezen aan de service-principal, worden een beleid expliciet die zijn toegewezen aan de tenant van de bovenliggende van de service principal afgedwongen. 
- Als er geen beleid expliciet aan de hoofdsom service of de tenant is toegewezen, worden het beleid die zijn toegewezen aan de toepassing afgedwongen. 
- Als er geen beleid is toegewezen aan de service-hoofdsom, de tenant of het toepassingsobject, de standaardwaarden worden afgedwongen (Zie de tabel hierboven).

Zie voor meer informatie over de relatie tussen toepassingen en service principal objecten in Azure AD, [-toepassing en service principal objecten in Azure Active Directory](active-directory-application-objects.md).

De geldigheid van een token geëvalueerd op het moment dat deze worden gebruikt. Het beleid met de hoogste prioriteit van de toepassing die wordt geopend van kracht wordt.


>[AZURE.NOTE]
>Voorbeeld
>
>Een gebruiker wil toegang tot 2 webtoepassingen, A en b te drukken. 
>
>
>- Beide toepassingen zijn in dezelfde bovenliggende tenant. 
>- Levensduur van tokens beleid 1 met een sessie Token Max Age van 8 uur is ingesteld als de tenant van de bovenliggende standaard.
>- Webtoepassing A is een webtoepassing normaal gebruik en niet is gekoppeld aan een beleid. 
>- Webtoepassing B wordt gebruikt voor zeer gevoelige processen en de hoofdsom van de service is gekoppeld aan de levensduur van tokens beleid 2 met een sessie Token Max Age van 30 minuten.
>
>AT 12:00 PM de gebruiker wordt geopend een nieuwe browsersessie en probeert te krijgen tot webtoepassing A. de gebruiker wordt omgeleid naar Azure AD en wordt gevraagd om aan te melden. Dit een cookie met een sessietoken worden in de browser. De gebruiker wordt omgeleid terugkeren naar de toepassing A met een ID-token waarmee ze toegang tot de toepassing met webonderdelen.
>
>Op 12:15 PM, klikt u vervolgens probeert de gebruiker te openen webtoepassing b te drukken. De browser wordt omgeleid naar Azure AD opsporen van cookie van de sessie. Web application B service principal is gekoppeld aan een beleid 1, maar ook deel uitmaakt van de bovenliggende-tenant met standaardbeleid 2. Beleid 2 kracht Aangezien beleidsregels die zijn gekoppeld aan de service principes een hogere prioriteit dan de standaardbeleidsregels tenant hebben. De sessietoken is oorspronkelijk uitgegeven binnen de laatste 30 minuten, zodat deze wordt beschouwd als geldig. De gebruiker wordt omgeleid naar webtoepassing B met een ID-token verlenen van toegang.
>
>De gebruiker probeert at 1:00 PM navigeren naar de web-toepassing A. De gebruiker omgeleid naar Azure AD. Webtoepassing A is niet gekoppeld aan een beleid, maar omdat deze zich in een tenant met standaardbeleid 1, dit beleid van kracht. De sessie cookie wordt gedetecteerd dat oorspronkelijk is uitgegeven binnen de laatste 8 uur en de gebruiker is stilte teruggeleid naar webtoepassing A met een nieuwe ID-token zonder dat u nodig hebt om te verifiëren.
>
>De gebruiker probeert direct voor toegang tot webtoepassing b te drukken. De gebruiker omgeleid naar Azure AD. Als kracht voordat, beleid 2. Het token is langer dan 30 minuten geleden uitgegeven, klikt u vervolgens de gebruiker gevraagd naar hun referenties opnieuw in te voeren en wordt een geheel nieuwe sessie en ID token worden uitgegeven. De gebruiker toegang vervolgens webtoepassing b te drukken.

## <a name="configurable-policy-properties-in-depth"></a>Eigenschappen van configureerbare beleid: uitgebreide

### <a name="access-token-lifetime"></a>Levensduur van tokens van Access

**Tekenreeks:** AccessTokenLifetime

**Van invloed is op:** Access tokens, ID tokens

**Overzicht:** Dit beleid bepaalt hoe lang access en ID tokens van deze resource worden beschouwd als geldig. De levensduur van de access-tokens wordt afgetrokken verkleint u het risico van een access- of ID token wordt gebruikt door een schadelijke acteur voor een langere periode (zoals ze kunnen niet worden ingetrokken), maar ook negatieve invloed op prestaties aangezien de tokens vaker vervangen.

### <a name="refresh-token-max-inactive-time"></a>Token max niet-actieve tijd vernieuwen

**Tekenreeks:** MaxInactiveTime

**Van invloed is op:** Tokens vernieuwen

**Overzicht:** Dit beleid bepaalt hoe oude een token vernieuwen kan zijn voordat een client deze niet meer gebruiken kunt om op te halen nieuwe access/vernieuwen token twee tijdens een poging om toegang tot deze resource. Aangezien een nieuwe vernieuwen token wordt meestal geretourneerd voor dat een token vernieuwen wordt gebruikt, moet de client niet hebt bereikt uit alle bronnen die met behulp van de token van de huidige vernieuwen voor de opgegeven periode voordat u dit beleid wordt voorkomen dat toegang. 

Dit beleid worden gebruikers die niet zijn actief op de client opnieuw worden geverifieerd om op te halen van een nieuwe vernieuwen token afdwingen. 

Het is belangrijk te weten dat de vernieuwen Token Max niet-actieve tijd moet worden ingesteld op een lagere waarde dan de één-Factor Token Max Age en de meervoudige vernieuwen Token Max Age.

### <a name="single-factor-refresh-token-max-age"></a>Enkel-factor vernieuwen token max leeftijd

**Tekenreeks:** MaxAgeSingleFactor

**Van invloed is op:** Tokens vernieuwen

**Overzicht:** Deze beleid bepaalt hoe lang een gebruiker kunt blijven vernieuwen tokens gebruiken om nieuwe access/vernieuwen token paren na de laatste keer dat deze geverifieerd succes met alleen een factor van één. Wanneer een gebruiker wordt geverifieerd en ontvangt een nieuwe vernieuwen token, ze kunnen gebruiken de token stroom vernieuwen (zo lang maken als het huidige vernieuwen token niet is ingetrokken en er nog niet langer dan de niet-actieve tijd niet-gebruikte) voor de opgegeven periode. Op dat moment worden gebruikers gedwongen opnieuw als u wilt ontvangen van een nieuwe vernieuwen token worden geverifieerd. 

De maximale leeftijd verkleinen, worden gebruikers om te verifiëren vaker gedwongen. Aangezien de één-factor verificatie wordt beschouwd als minder veilig dan een meervoudige verificatie, wordt het wordt aanbevolen dat dit beleid is ingesteld op een dezelfde of een lagere waarde dan de meervoudige vernieuwen Token Max leeftijd beleid.

### <a name="multi-factor-refresh-token-max-age"></a>Meervoudige vernieuwen token max leeftijd

**Tekenreeks:** MaxAgeMultiFactor

**Van invloed is op:** Tokens vernieuwen

**Overzicht:** Deze beleid bepaalt hoe lang een gebruiker kunt blijven vernieuwen tokens gebruiken om nieuwe access/vernieuwen token paren na de laatste keer dat deze geverifieerd succes met meerdere factoren. Wanneer een gebruiker wordt geverifieerd en ontvangt een nieuwe vernieuwen token, ze kunnen gebruiken de token stroom vernieuwen (zo lang maken als het huidige vernieuwen token niet is ingetrokken en er nog niet langer dan de niet-actieve tijd niet-gebruikte) voor de opgegeven periode. Op dat moment worden gebruikers gedwongen opnieuw als u wilt ontvangen van een nieuwe vernieuwen token worden geverifieerd. 

De maximale leeftijd verkleinen, worden gebruikers om te verifiëren vaker gedwongen. Aangezien de één-factor verificatie wordt beschouwd als minder veilig dan een meervoudige verificatie, wordt het wordt aanbevolen dat dit beleid is ingesteld op een waarde gelijk of groter is dan de één-Factor vernieuwen Token Max leeftijd beleid.

### <a name="single-factor-session-token-max-age"></a>Enkel-factor sessie token max leeftijd

**Tekenreeks:** MaxAgeSessionSingleFactor

**Van invloed is op:** Sessie tokens (persistent en niet-persistent)

**Overzicht:** Deze beleid bepaalt hoe lang een gebruiker kunt blijven sessie tokens gebruiken om nieuwe-ID en sessie tokens na de laatste keer dat ze geverifieerd met alleen een factor van één. Wanneer een gebruiker wordt geverifieerd en ontvangt een nieuwe sessietoken, kunnen ze gebruiken van de sessie token stroom (zo lang maken als de huidige sessietoken wordt niet ingetrokken of verlopen) voor de opgegeven periode. Op dat moment worden gebruikers gedwongen opnieuw als u wilt ontvangen van een nieuwe sessietoken worden geverifieerd. 

De maximale leeftijd verkleinen, worden gebruikers om te verifiëren vaker gedwongen. Aangezien de één-factor verificatie wordt beschouwd als minder veilig dan een meervoudige verificatie, wordt het wordt aanbevolen dat dit beleid is ingesteld op een dezelfde of een lagere waarde dan het meervoudige sessie Token Max leeftijd beleid.

### <a name="multi-factor-session-token-max-age"></a>Meervoudige sessie token max leeftijd

**Tekenreeks:** MaxAgeSessionMultiFactor

**Van invloed is op:** Sessie tokens (permanente en niet-permanente)

**Overzicht:** Deze beleid bepaalt hoe lang een gebruiker kunt blijven sessie tokens gebruiken om nieuwe-ID en sessie tokens na de laatste keer dat ze geverifieerd met meerdere factoren. Wanneer een gebruiker wordt geverifieerd en ontvangt een nieuwe sessietoken, kunnen ze gebruiken van de sessie token stroom (zo lang maken als de huidige sessietoken wordt niet ingetrokken of verlopen) voor de opgegeven periode. Op dat moment worden gebruikers gedwongen opnieuw als u wilt ontvangen van een nieuwe sessietoken worden geverifieerd. 

De maximale leeftijd verkleinen, worden gebruikers om te verifiëren vaker gedwongen. Aangezien de één-factor verificatie wordt beschouwd als minder veilig dan een meervoudige verificatie, wordt het wordt aanbevolen dat dit beleid is ingesteld op een waarde gelijk of groter is dan de één-Factor sessie Token Max leeftijd beleid.

## <a name="sample-token-lifetime-policies"></a>Voorbeeld levensduur van tokens beleidsregels

Kunnen maken en beheren van token levensduur voor apps, service principes en uw algemene tenant beschrijft allerlei nieuwe scenario's mogelijk in Azure AD.  Gaan we een paar veelvoorkomende beleid scenario's waarmee u kunt nieuwe regels voor het opleggen doorloop:


- Token levensduur
- Inactieve tijden token Max
- Token Max leeftijd

We gaat een paar scenario's, inclusief doorlopen:


- Beheer van het standaardbeleid van een Tenant
- Een beleid maken voor aanmelding Web
- Maken van een beleid voor systeemeigen Apps bellen van een Web-API
- Een geavanceerde beleid beheren 

### <a name="prerequisites"></a>Vereisten voor
In de steekproef scenario's worden we maken, bijwerken, koppelen, en beleid op apps, service principes en uw algemene tenant verwijderen.  Nieuw voor u zijn Azure AD, uitchecken, [in dit artikel](active-directory-howto-tenant.md) om u te helpen aan de slag als voordat u verder gaat met deze voorbeelden.  


1. Download de meest recente [Azure AD PowerShell-Cmdlet voorbeeld](https://www.powershellgallery.com/packages/AzureADPreview)om te beginnen. 
2.  Nadat u de Azure AD PowerShell-Cmdlets hebt, moet u verbinding maken met opdracht u zich aanmeldt bij uw Azure AD-beheerdersaccount uitvoeren. U moet dit doen wanneer u een nieuwe sessie starten.
        
        Connect-AzureAD -Confirm

3.  Voer de volgende opdracht om te zien van alle beleidsregels die zijn gemaakt in uw tenant is ingeschakeld.  Deze opdracht moet worden gebruikt na de meeste bewerkingen in de volgende scenario's.  Bovendien kunt u de **Object-ID** van uw beleid ophalen. 
        
        Get-AzureADPolicy

### <a name="sample-managing-a-tenants-default-policy"></a>Voorbeeld: Het standaardbeleid van een tenant beheren

In dit voorbeeld maakt wordt een beleid waarin uw gebruikers kunnen minder vaak aanmelden via uw hele tenant. 

Klik hiertoe maken we levensduur van tokens-beleid voor één-Factor vernieuwen Tokens die wordt toegepast op uw tenant. Dit beleid wordt worden toegepast op elke toepassing in uw tenant en elke service principal die een beleid is ingesteld op deze nog niet heeft. 

1.  **Levensduur van tokens-beleid maken.** 

Stel de één-Factor vernieuwen Token op "tot ingetrokken" zin die deze verloopt Won't totdat access is ingetrokken.  De onderstaande definitie van beleid is wat we maakt:
        
        @("{
          `"TokenLifetimePolicy`":
              {
                 `"Version`":1, 
                 `"MaxAgeSingleFactor`":`"until-revoked`"
              }
        }")

Voer de volgende opdracht uit om dit beleid te maken. 

        
    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1, `"MaxAgeSingleFactor`":`"until-revoked`"}}") -DisplayName TenantDefaultPolicyScenario -IsTenantDefault $true -Type TokenLifetimePolicy
        
Voer de volgende opdracht om te zien van uw nieuwe beleid en krijgen de object-id.

    Get-AzureADPolicy
&nbsp;&nbsp;2. **het beleid bijwerken**

U hebt besloten dat het eerste beleid niet als strikte terwijl uw service vereist en u wilt dat uw enkel-Factor vernieuwen Tokens verloopt in twee dagen hebt besloten is. Voer de volgende opdracht. 
        
    Set-AzureADPolicy -ObjectId <ObjectID FROM GET COMMAND> -DisplayName TenantDefaultPolicyUpdatedScenario -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"2.00:00:00`"}}")
        
&nbsp;&nbsp;3. **u bent klaar!** 

### <a name="sample-creating-a-policy-for-web-sign-in"></a>Voorbeeld: Een beleid voor aanmelding web maken

In dit voorbeeld maakt wordt een beleid waarin wordt gevraagd uw gebruikers om te verifiëren vaker in uw Web-App. Dit beleid wordt de levensduur van de Access-Id-Tokens en de Max Age van een Token meervoudige-sessie ingesteld op de hoofdsom service van uw web-app.

1.  **Levensduur van tokens-beleid maken.**

Dit beleid voor aanmelding Web stelt u de levensduur van tokens Access-Id en de leeftijd sessie Token van Max enkel-Factor op 2 uur.

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"AccessTokenLifetime`":`"02:00:00`",`"MaxAgeSessionSingleFactor`":`"02:00:00`"}}") -DisplayName WebPolicyScenario -IsTenantDefault $false -Type TokenLifetimePolicy

Voer de volgende opdracht om te zien van uw nieuwe beleid en krijgen de object-id.

    Get-AzureADPolicy
&nbsp;&nbsp;2. **het beleid toewijzen aan uw service principal.**

Gaan we dit nieuw beleid met een service principal koppelen.  U hebt ook toegang krijgen tot de **object-id** van uw service principal nodig. U kunt query's [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) of Ga naar onze [Graph Explorer hulpmiddel](https://graphexplorer.cloudapp.net/) en meld u aan bij uw account Azure AD om alle aan uw tenant service principes weer te geven. 

Nadat u de **object-id**hebt, kunt u de volgende opdracht uitvoeren.
        
    Add-AzureADServicePrincipalPolicy -ObjectId <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
&nbsp;&nbsp;3. **u bent klaar!** 

 

### <a name="sample-creating-a-policy-for-native-apps-calling-a-web-api"></a>Voorbeeld: Een beleid voor het bellen van een Web API-systeemeigen apps maken

>[AZURE.NOTE]
>Beleidsregels voor koppelingen naar toepassingen is uitgeschakeld.  We werken over het inschakelen van dit binnenkort.  Deze pagina wordt bijgewerkt zodra de functie beschikbaar is.

In dit voorbeeld wordt er een beleid dat, moeten gebruikers om te verifiëren kleiner maakt en wordt de hoeveelheid tijd die ze inactief worden kunnen zonder opnieuw te identificeren verlengen. Het beleid wordt toegepast op de Web-API op die manier wanneer de systeemeigen App hierom vraagt een resource die dit beleid wordt toegepast.

1.  **Levensduur van tokens-beleid maken.** 

Deze opdracht maakt een strikte beleid voor een Web-API. 
        
    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"30.00:00:00`",`"MaxAgeMultiFactor`":`"until-revoked`",`"MaxAgeSingleFactor`":`"180.00:00:00`"}}") -DisplayName WebApiDefaultPolicyScenario -IsTenantDefault $false -Type TokenLifetimePolicy
         
Voer de volgende opdracht om te zien van uw nieuwe beleid en krijgen de object-id.

    Get-AzureADPolicy

&nbsp;&nbsp;2. **het beleid aan uw Web-API toewijzen**.

Gaan we dit nieuw beleid met een toepassing koppelen.  U hebt ook toegang krijgen tot de **object-id** van de toepassing nodig. De beste manier om te zoeken van uw app- **object-id** is het gebruik van de [Azure-Portal](https://portal.azure.com/). 

Nadat u de **object-id**hebt, kunt u de volgende opdracht uitvoeren.

    Add-AzureADApplicationPolicy -ObjectId <ObjectID of the App> -RefObjectId <ObjectId of the Policy>

&nbsp;&nbsp;3. **u bent klaar!** 

### <a name="sample-managing-an-advanced-policy"></a>Voorbeeld: Een geavanceerde beleid beheren 

In dit voorbeeld maakt we een paar beleidsregels om te laten zien hoe het systeem prioriteit werkt en hoe u meerdere beleidsregels die zijn toegepast op verschillende objecten kunt beheren. Hiermee geeft sommige inzicht in de prioriteit van beleidsregels voor uitleg over boven, en kunt u beheren ingewikkelder scenario's ook. 

1.  **Levensduur van tokens-beleid maken**

Dusverre heel eenvoudig. We hebt het standaardbeleid van een tenant die Hiermee stelt u de levensduur van één-Factor vernieuwen tokens op 30 dagen gemaakt. 

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"30.00:00:00`"}}") -DisplayName ComplexPolicyScenario -IsTenantDefault $true -Type TokenLifetimePolicy
Is de object-id, voer de volgende opdracht om te zien van uw nieuwe beleid en u deze ophalen.
 
    Get-AzureADPolicy

&nbsp;&nbsp;2. **het beleid naar een Service Principal toewijzen**

Nu hebben we een beleid op de volledige tenant.  Stel dat we wilt dit beleid 30 dagen voor een specifieke service principal behouden, maar het standaardbeleid voor de tenant als u de bovengrens van 'tot ingetrokken' wilt wijzigen. 

Eerst gaan We dit nieuw beleid met onze service principal koppelen.  U hebt ook toegang krijgen tot de **object-id** van uw service principal nodig. U kunt query's [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) of Ga naar onze [Graph Explorer hulpmiddel](https://graphexplorer.cloudapp.net/) en meld u aan bij uw account Azure AD om alle aan uw tenant service principes weer te geven. 

Nadat u de **object-id**hebt, kunt u de volgende opdracht uitvoeren.

    Add-AzureADServicePrincipalPolicy -ObjectId <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>

&nbsp;&nbsp;3. **de vlag IsTenantDefault naar onwaar met de volgende opdracht instellen**. 

    Set-AzureADPolicy -ObjectId <ObjectId of Policy> -DisplayName ComplexPolicyScenario -IsTenantDefault $false
&nbsp;&nbsp;4. **maken een nieuwe Tenant standaardbeleid**

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"until-revoked`"}}") -DisplayName ComplexPolicyScenarioTwo -IsTenantDefault $true -Type TokenLifetimePolicy

&nbsp;&nbsp;5. **u bent klaar!** 

U hebt nu het oorspronkelijke beleid dat is gekoppeld aan de hoofdsom van uw service en het nieuwe beleid is ingesteld als het standaardbeleid voor uw tenant.  Het is belangrijk te onthouden dat beleidsregels die zijn toegepast op de service principes prioriteit over standaardbeleidsregels tenant hebben. 


## <a name="cmdlet-reference"></a>Verwijzing naar cmdlets

### <a name="manage-policies"></a>Beleid beheren
De volgende cmdlets kan worden gebruikt voor het beheren van beleid.</br></br>



#### <a name="new-azureadpolicy"></a>Nieuwe AzureADPolicy
Hiermee maakt u een nieuw beleid.

    New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsTenantDefault <boolean> -Type <Policy Type> 

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-Definition| De matrix van stringified JSON waarin de regels van het beleid.|-Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"20:00:00`"}}") 
-Weergavenaam|Tekenreeks van de naam van het beleid|MyTokenPolicy - weergavenaam
-IsTenantDefault|Als waar Hiermee stelt u het beleid als tenant-standaardbeleid, indien onwaar heeft geen effect|-IsTenantDefault $true
-Type|Het type van het beleid, voor token levensduur altijd 'TokenLifetimePolicy' gebruiken|-Type TokenLifetimePolicy
-AlternativeIdentifier [optionele]|Hiermee stelt een alternatieve id op het beleid.|-AlternativeIdentifier myAltId
</br></br>

#### <a name="get-azureadpolicy"></a>Get-AzureADPolicy         
Alle AzureAD beleidsregels of opgegeven beleid krijgt 
        
    Get-AzureADPolicy 

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-Object-id [optionele]|De object-Id van het beleid dat u wilt ophalen. |-Object-id &lt;object-id van beleid&gt; 
</br></br>

#### <a name="get-azureadpolicyappliedobject"></a>Get-AzureADPolicyAppliedObject         
Alle apps en service principes die zijn gekoppeld aan een beleid krijgt
        
    Get-AzureADPolicyAppliedObject -ObjectId <object id of policy> 

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-Object-id|De object-Id van het beleid dat u wilt ophalen.|-Object-id &lt;object-id van beleid&gt;
</br></br>

#### <a name="set-azureadpolicy"></a>Set-AzureADPolicy
Een bestaand beleid wordt bijgewerkt
        
    Set-AzureADPolicy -ObjectId <object id of policy> -DisplayName <string> 
 
Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-Object-id|De object-Id van het beleid dat u wilt ophalen.|-Object-id &lt;object-id van beleid&gt;
-Weergavenaam|Tekenreeks van de naam van het beleid|MyTokenPolicy - weergavenaam
-Definition [optionele]|De matrix van stringified JSON waarin de regels van het beleid.|-Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"20:00:00`"}}") 
-IsTenantDefault [optionele]|Als waar Hiermee stelt u het beleid als tenant-standaardbeleid, indien onwaar heeft geen effect|-IsTenantDefault $true
-Type [optionele]|Het type van het beleid, voor token levensduur altijd 'TokenLifetimePolicy' gebruiken|-Type TokenLifetimePolicy
-AlternativeIdentifier [optionele]|Hiermee stelt een alternatieve id op het beleid.|-AlternativeIdentifier myAltId
</br></br>

#### <a name="remove-azureadpolicy"></a>Verwijderen AzureADPolicy         
Hiermee verwijdert u het opgegeven beleid

     Remove-AzureADPolicy -ObjectId <object id of policy>

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-Object-id|De object-Id van het beleid dat u wilt ophalen.|-Object-id &lt;object-id van beleid&gt;
</br></br>

### <a name="application-policies"></a>Beleidsregels voor toepassingen
De volgende cmdlets kan worden gebruikt voor beleidsregels voor toepassingen.</br></br>

#### <a name="add-azureadapplicationpolicy"></a>Toevoegen AzureADApplicationPolicy         
Het opgegeven beleid gekoppeld aan een toepassing

    Add-AzureADApplicationPolicy -ObjectId <object id of application> -RefObjectId <object id of policy>

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-Object-id|De object-Id van de toepassing.|-Object-id &lt;object-id van toepassing&gt; 
-RefObjectId|De object-Id van het beleid. |-RefObjectId &lt;object-id van beleid&gt;
</br></br>

#### <a name="get-azureadapplicationpolicy"></a>Get-AzureADApplicationPolicy        
Het beleid die zijn toegewezen aan een toepassing krijgt

    Get-AzureADApplicationPolicy -ObjectId <object id of application>

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-Object-id|De object-Id van de toepassing.|-Object-id &lt;object-id van toepassing&gt; 
</br></br>

#### <a name="remove-azureadapplicationpolicy"></a>Verwijderen AzureADApplicationPolicy        
Hiermee verwijdert u een beleid van een toepassing

    Remove-AzureADApplicationPolicy -ObjectId <object id of application> -PolicyId <object id of policy>

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-Object-id|De object-Id van de toepassing.|-Object-id &lt;object-id van toepassing&gt; 
-PolicyId| Id van beleid.|-PolicyId &lt;object-id van beleid&gt;
</br></br>

### <a name="service-principal-policies"></a>Service principal beleidsregels
De volgende cmdlets kan worden gebruikt voor service principal beleid.</br></br>

#### <a name="add-azureadserviceprincipalpolicy"></a>Toevoegen AzureADServicePrincipalPolicy         
Het opgegeven beleid gekoppeld aan een service principal

    Add-AzureADServicePrincipalPolicy -ObjectId <object id of service principal> -RefObjectId <object id of policy>

Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-Object-id|De object-Id van de toepassing.|-Object-id &lt;object-id van toepassing&gt; 
-RefObjectId|De object-Id van het beleid. |-RefObjectId &lt;object-id van beleid&gt;
</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a>Get-AzureADServicePrincipalPolicy        
Krijgt een beleid dat is gekoppeld aan de opgegeven service hoofdsom

    Get-AzureADServicePrincipalPolicy -ObjectId <object id of service principal>
 
Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-Object-id|De object-Id van de toepassing.|-Object-id &lt;object-id van toepassing&gt; 
</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a>Verwijderen AzureADServicePrincipalPolicy         
Hiermee verwijdert u het beleid uit de opgegeven service principal

    Remove-AzureADServicePrincipalPolicy -ObjectId <object id of service principal>  -PolicyId <object id of policy>
 
Parameters|Beschrijving|Voorbeeld|
-----| ----- |-----|
-Object-id|De object-Id van de toepassing.|-Object-id &lt;object-id van toepassing&gt; 
-PolicyId| Id van beleid.|-PolicyId &lt;object-id van beleid&gt;
