<properties
    pageTitle="Het inschakelen van eenmalige aanmelding van meerdere-app op iOS met ADAL | Microsoft Azure"
    description="Hoe u de functies van de ADAL SDK gebruiken om eenmalige aanmelding inschakelen via uw toepassingen. "
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>Het inschakelen van eenmalige aanmelding van cross-app op ADAL met iOS


Voorwaarde eenmalige aanmelding (SSO) zodat gebruikers alleen moeten hun referenties eenmaal invoeren en de referenties automatisch laten werken op toepassingen wordt nu naar verwachting klanten. Het probleem bij het invoeren van hun gebruikersnaam en wachtwoord op een klein scherm, vaak tijden gecombineerd met een extra factor (2FA), zoals een telefoongesprek of een texted code, resulteert in de snelle ontevredenheid als een gebruiker heeft meer dan één keer voor uw product hiervoor.

Bovendien als u gebruikmaken van een identiteit-platform die andere toepassingen, zoals Microsoft-Accounts of een werkaccount van Office 365 kunnen gebruiken, verwachten klanten dat de referenties die moeten worden beschikbaar voor gebruik alle webtoepassingen ongeacht de leverancier.

Het platform Microsoft Identity, samen met onze Microsoft identiteit SDK's, wordt dit werk voor u en geeft u de mogelijkheid om te overleggen van uw klanten met SSO hetzij in uw eigen suite van toepassingen of, als met onze makelaar mogelijkheid en verificator-toepassingen, over het hele apparaat.

In dit scenario wordt uitgelegd hoe u voor het configureren van onze SDK binnen uw toepassing tegen deze korting naar uw klanten.

In dit scenario is van toepassing op:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory B2B
* Voorwaardelijke Azure Active Directory-toegang


Houd er rekening mee dat het document hieronder wordt ervan uitgegaan dat u hebt kennis beschikken om te [richten toepassingen in de oudere portal voor Azure Active Directory](./develop/active-directory-how-to-integrate.md) , evenals uw toepassing hebt geïntegreerd met het [Microsoft-identiteit iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Eenmalige aanmelding concepten in de identiteit van de Microsoft-Platform

### <a name="microsoft-identity-brokers"></a>Microsoft-identiteit makelaars

Microsoft biedt toepassingen voor elke mobiele platform waarmee voor de bruggen referenties webtoepassingen van andere leveranciers, evenals kan voor speciale verbeterde functies die één veilige locatie voor het valideren van referenties waaruit vereisen. Verwijst naar deze **makelaars**. Klik op iOS en Android zijn deze opgenomen in downloadbare toepassingen dat klanten afzonderlijk installeren of kunnen verplaatst naar het apparaat dat door een bedrijf die sommige of alle van het apparaat voor hun werknemer beheert. Deze makelaars ondersteuning beheren beveiliging alleen betrekking heeft op sommige toepassingen of het hele apparaat op basis van wat IT-beheerders nodig hebben. In Windows krijgt u deze functionaliteit van een account kiezen ingebouwd in het besturingssysteem, bekende technisch als de makelaar Web-verificatie.

Voor meer informatie over hoe we gebruiken deze makelaars en hoe uw klanten mogelijk deze zien in hun stroom aanmelding voor de Microsoft Identity platform Lees verder voor meer informatie.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Patronen voor logboekregistratie in op mobiele apparaten

Toegang tot de referenties op apparaten twee eenvoudige patronen voor het platform Microsoft Identity als volgt:

* Niet-makelaar hulp bij aanmeldingen
* Hulp bij aanmeldingen Broker

#### <a name="non-broker-assisted-logins"></a>Niet-makelaar hulp bij aanmeldingen

Niet-makelaar hulp bij aanmeldingen zijn login ervaringen die optreden inline Zorg dat de toepassing en het gebruik van de lokale opslag op het apparaat voor de toepassing. Deze opslag in toepassingen kan worden gedeeld, maar de referenties zijn hecht afhankelijk van de app of een reeks apps met behulp van deze referentie. Dit is de ervaring die hebt u waarschijnlijk ervaring in vele mobiele toepassingen waar u een gebruikersnaam en wachtwoord vanuit de toepassing zelf invoeren.

Deze aanmeldingen bestaan uit de volgende voordelen:

-  Gebruikerservaring bestaat volledig in de toepassing.
-  Referenties kunnen worden gedeeld met toepassingen die zijn ondertekend door het certificaat, een functionaliteit voor eenmalige aanmelding op uw pakket toepassingen leveren.
-  Besturingselement rond de ervaring van logboekregistratie in is opgegeven met de toepassing voor en na het aanmelden.

Deze aanmeldingen bestaan uit de volgende nadelen:

- Gebruiker kan geen ervaart u eenmalige aanmelding via alle apps die gebruikmaken van een Microsoft-Identity, alleen op deze Microsoft-Identities die zijn uw toepassing eigenaar is van en hebt geconfigureerd.
- Uw toepassing kan niet worden gebruikt met meer geavanceerde voorzieningen zoals voorwaardelijke toegang of gebruik de suite InTune met producten.
- Uw toepassing ondersteunt geen verificatie via clientcertificaat gebaseerd voor zakelijke gebruikers.

Hier volgt een weergave van de werking van de Microsoft identiteit SDK's met de gedeelde opslag van uw toepassingen SSO inschakelen:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Hulp bij aanmeldingen Broker

Makelaar ondersteunde aanmeldingen zijn login ervaringen die optreden in de toepassing makelaar en de opslag en de beveiliging van de makelaar gebruiken om te delen van referenties voor alle toepassingen op het apparaat die gebruikmaken van het Microsoft Identity-platform. Dit betekent dat uw toepassingen afhankelijk is van de makelaar om te kunnen gebruikers aanmelden. Klik op iOS en Android zijn deze opgenomen in downloadbare toepassingen dat klanten afzonderlijk installeren of kunnen verplaatst naar het apparaat dat door een bedrijf die worden beheerd door het apparaat voor de gebruiker. Een voorbeeld van dit type van toepassing is de toepassing Azure verificator op iOS. In Windows krijgt u deze functionaliteit van een account kiezen ingebouwd in het besturingssysteem, bekende technisch als de makelaar Web-verificatie.
De ervaring kan varieert per platform en soms storend voor gebruikers als niet correct worden beheerd. U bent waarschijnlijk meest bekend zijn met dit patroon als u de Facebook-toepassing geïnstalleerd en Facebook Login-functionaliteit in een andere toepassing gebruiken. Het platform Microsoft Identity maakt gebruik van hetzelfde patroon.

Animatie waarin uw toepassing wordt verzonden naar de achtergrond tijdens de toepassingen Azure verificator wordt voor iOS die dit tot een 'overgang leidt' geleverd naar de voorgrond voor de gebruiker om te selecteren welk account ze willen zich aanmeldt.  

Voor Android en Windows wordt de kiezer account weergegeven boven aan uw toepassing dat minder storend voor de gebruiker is.

#### <a name="how-the-broker-gets-invoked"></a>Hoe de makelaar wordt aangeroepen

Als een compatibele makelaar is geïnstalleerd op het apparaat, zoals de verificator Azure-toepassing en volgt de Microsoft identiteit SDK's automatisch de werklast van de makelaar voor u aanroepen wanneer een gebruiker aangeeft dat zij wensen aan te melden met een account van het Microsoft Identity-platform. Dit kan een persoonlijk Microsoft-Account, een werk of schoolaccount, of een account dat u opgeeft en host in Azure wordt aangegeven met onze producten B2C en B2B zijn. Met behulp van zeer algoritmen en versleuteling die we ervoor zorgen dat de referenties wordt gevraagd om en bezorgd terug naar de toepassing op een veilige manier te beveiligen. De exacte technische details van deze regelingen niet wordt gepubliceerd, maar met samenwerking zijn ontwikkeld door appel- en Google.

**De ontwikkelaar heeft de keuze van als de Microsoft identiteit SDK de makelaar-oproepen of de niet-makelaar hulp bij stroom gebruikt.** Echter als de ontwikkelaar niet wil gebruiken de stroom makelaar ondersteunde verloren ze het voordeel van gebruikmaken van eenmalige aanmeldingsreferenties dat de gebruiker kan het al hebt toegevoegd op het apparaat, evenals wordt voorkomen dat de toepassing die wordt gebruikt met Microsoft haar klanten zoals voorwaardelijke toegang beheermogelijkheden van Intune biedt, voorzieningen en op certificaat verificatie gebaseerde.

Deze aanmeldingen bestaan uit de volgende voordelen:

-  Gebruiker ervaringen SSO alle webtoepassingen ongeacht de leverancier.
-  Uw toepassing kunt gebruikmaken van geavanceerde voorzieningen zoals voorwaardelijke toegang of gebruik de suite InTune met producten.
-  Uw toepassing kunt verificatie via clientcertificaat op basis van de ondersteuning voor zakelijke gebruikers.
- Veel veiliger aanmeldervaring varieert als de identiteit van de toepassing en de gebruiker zijn gecontroleerd door de toepassing makelaar met algoritmen voor extra beveiliging en codering.

Deze aanmeldingen bestaan uit de volgende nadelen:

- De gebruiker is in iOS overgebracht uit van uw toepassing ervaring terwijl referenties zijn geselecteerd.
- Verlies van de mogelijkheid voor het beheren van de gebruikerservaring login voor uw klanten binnen de toepassing.



Hier volgt een weergave van de werking van de Microsoft identiteit SDK's met de toepassingen makelaar SSO inschakelen:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+
```

Uitgerust met deze achtergrondinformatie die u moet mogelijk beter te begrijpen en implementeren van eenmalige aanmelding binnen uw-toepassing via de Microsoft Identity platform en SDK's.


## <a name="enabling-cross-app-sso-using-adal"></a>Cross-app SSO met ADAL inschakelen

Hier gebruiken we de ADAL iOS SDK naar:

- Niet-makelaar hulp bij eenmalige aanmelding voor de reeks apps inschakelen
- Ondersteuning voor eenmalige aanmelding makelaar ondersteunde inschakelen

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Eenmalige aanmelding inschakelen voor niet-makelaar bijstaan eenmalige aanmelding

Voor hulp bij SSO niet makelaar webtoepassingen beheren de Microsoft identiteit SDK's veel van de complexiteit van eenmalige aanmelding voor u. Het gaat hierbij om de juiste gebruiker zoeken in de cache en onderhouden van een lijst met aangemelde gebruikers u query.

Als u wilt inschakelen SSO webtoepassingen u de eigenaar bent moet u als volgt te werk:

1. Zorgen dat alle gebruikers van uw toepassingen dezelfde cliënt-ID of id
* Zorg ervoor dat al uw toepassingen het dezelfde handtekeningcertificaat van Apple delen, zodat u keychains kunt delen
* Vraag het dezelfde sleutelhanger recht voor elk van uw toepassingen.
* Vertel de Microsoft identiteit SDK's over de gedeelde sleutelhanger u dat wij gebruiken.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Gebruik van dezelfde Client-ID / toepassing-ID van alle toepassingen in de reeks apps

In de volgorde voor het platform Microsoft Identity om te weten dat het is toegestaan tokens delen via uw toepassingen, moet elke toepassing delen dezelfde cliënt-ID of id Dit is de unieke id die aan u is verstrekt wanneer u uw eerste toepassing geregistreerd in de portal.

U mag worden wilt u weten hoe u verschillende apps naar de Microsoft Identity-service wordt bepalen of de site gebruikmaakt van de dezelfde toepassing-ID. Het antwoord is met de **Omleiden URI's**. Elke toepassing kan meerdere omleiden URI's geregistreerd in de portal onboarding hebben. Elke app in uw-suite heeft een andere omleiding URI. Een voorbeeld van hoe dit eruitziet is hieronder:

1 Redirect URI:`x-msauth-mytestiosapp://com.myapp.mytestapp`

2-Redirect URI:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 omleiden URI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Deze worden genest onder dezelfde client-ID / toepassings-ID en opgezocht op basis van de omleiding URI die u met ons in uw configuratie SDK te retourneren.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Houd er rekening mee dat de opmaak van deze omleiden-URI's hieronder worden uitgelegd. U kunt een URI omleiden tenzij u wilt ondersteuning voor de makelaar, zodat ze moeten er ongeveer zo de bovenstaande*



#### <a name="create-keychain-sharing-between-applications"></a>Sleutelhanger delen tussen toepassingen maken

Inschakelen van het delen van sleutelhangers valt buiten het bereik van dit document en Apple bestrijkt in een document [Mogelijkheden toe te voegen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Wat is het belangrijk is dat u hebt besloten wat uw sleutelhanger moet worden aangeroepen en die mogelijkheid toevoegen in al uw toepassingen.

Wanneer u hebt rechten instellen correct u ziet een bestand in het telefoonboek van uw project recht `entitlements.plist` waarin iets dat ziet er als volgt uit:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

Nadat u de sleutelhanger recht zijn ingeschakeld in elk van uw toepassingen hebt en u klaar bent voor gebruik van eenmalige aanmelding, vertellen u de Microsoft identiteit SDK over uw sleutelhanger met behulp van de volgende instelling in uw `ADAuthenticationSettings` met de volgende instellingen:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [AZURE.WARNING]
Wanneer u een sleutelhanger tussen alle toepassingen deelt kunt een toepassing gebruikers verwijderen of alle tokens slechter verwijderen in uw toepassing. Dit is vooral desastreuze als er toepassingen die afhankelijk zijn van de tokens werk op de achtergrond. Delen van een sleutelhanger verwijderen betekent dat u voorzichtig in alle moet bewerkingen via de Microsoft identiteit SDK's.

Dat is alles. De identiteit Microsoft SDK wordt nu met het delen van referenties in al uw toepassingen. De lijst met gebruikers worden ook alle toepassing werkstroomexemplaren gedeeld.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Eenmalige aanmelding bijstaan inschakelen van eenmalige aanmelding voor de makelaar

De mogelijkheid om een toepassing voor het gebruik van een makelaar dat op het apparaat is geïnstalleerd, is **standaard uitgeschakeld**. Pas uw toepassing gebruiken met de makelaar moet u enkele aanvullende instellingen en code toevoegen aan uw toepassing.

De stappen te volgen zijn:

1. Makelaar modus inschakelen in uw toepassingscode doorschakelen naar de MS-SDK.
2. Stand brengen van een nieuwe omleiding URI en bieden die zowel de app en de registratie van uw app.
3. Een URL-schema registreren.
4. Ondersteuning voor iOS9: een machtigingsniveau toevoegen aan uw info.plist-bestand.


#### <a name="step-1-enable-broker-mode-in-your-application"></a>Stap 1: Makelaar-modus inschakelen in uw toepassing
De mogelijkheid voor uw toepassing gebruik van de makelaar is ingeschakeld wanneer u de 'context' of de eerste configuratie van uw authenticatie-object maken. U doen dit door in te stellen van het type van uw referenties in code:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
De `AD_CREDENTIALS_AUTO` instelling kunnen de Microsoft identiteit SDK om te proberen te benadrukken met de makelaar, `AD_CREDENTIALS_EMBEDDED` wordt voorkomen dat de identiteit Microsoft SDK bellen met de makelaar.

#### <a name="step-2-registering-a-url-scheme"></a>Stap 2: Een URL-schema registreren
Het platform Microsoft Identity URL's gebruikt om te roepen de makelaar en vervolgens terugkeren terug naar de toepassing. Om te voltooien die retour moet u een URL-schema geregistreerd voor uw toepassing die het platform Microsoft Identity wordt kent. Dit is naast eventuele andere app kunt herkennen die mogelijk u eerder hebt geregistreerd bij uw toepassing.

> [AZURE.WARNING]
Is het raadzaam om de URL-schema vrij uniek zijn voor de kans op een andere app met de dezelfde URL-schema minimaliseren. Apple worden niet afgedwongen door de uniekheid van URL-schema's die zijn geregistreerd in de app store.

Hieronder volgt een voorbeeld van hoe dit wordt weergegeven in de projectconfiguratie van uw. U kunt ook dit doen in XCode ook:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Stap 3: Een nieuwe URI met uw schema URL-omleiding instellen

Om ervoor te zorgen dat we de tokens referentie altijd naar de juiste toepassing terugkeren, moeten we Zorg ervoor dat we terugbellen in uw toepassing op een wijze die in het besturingssysteem van iOS kunt controleren. Het besturingssysteem van iOS rapporten naar de Microsoft makelaar toepassingen de bundel-ID van de bellen van deze toepassing. Dit kan niet worden vervalst door een rogue-toepassing. Daarom gebruikmaken we dit samen met de URI van onze toepassing makelaar om ervoor te zorgen dat de tokens keert terug naar de juiste toepassing. We moet u deze unieke redirect URI beide in uw toepassing vast te stellen en stelt u als een URI omleiden in onze developer-portal.

Uw redirect URI moet op de juiste manier van:

`<app-scheme>://<your.bundle.id>`

ex: *x-msauth-mytestiosapp://com.myapp.mytestapp*

Deze URI omleiden moet worden opgegeven in de registratie van uw app met behulp van de [Azure klassieke portal](https://manage.windowsazure.com/). Zie voor meer informatie over de registratie van Azure AD-app, [integratie met Azure Active Directory](./develop/active-directory-how-to-integrate.md).


##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Stap 3a: een omleiding URI toevoegen in de portal van uw app en ontwikkelaar ter ondersteuning van verificatie via clientcertificaat gebaseerd

Om te ondersteunen certificaat gebaseerd verificatie die een tweede "msauth" worden geregistreerd in uw toepassing en de [Azure klassieke portal moet](https://manage.windowsazure.com/) voor verificatie via clientcertificaat afhandelen, indien u wilt toevoegen die ondersteuning in uw toepassing.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*


#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a>Stap 4: iOS9: een configuratieparameter toevoegen aan uw app

ADAL – canOpenURL wordt gebruikt: om te controleren of de makelaar op het apparaat is geïnstalleerd. In de iOS vergrendeld 9 Apple wat schema toepassingen kunnen zoeken. U moet 'msauth' toevoegen aan de sectie LSApplicationQueriesSchemes van uw `info.plist file`.

<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauth</string>
</array>

### <a name="youve-configured-sso"></a>U kunt eenmalige aanmelding hebt geconfigureerd!

Nu de identiteit Microsoft SDK automatisch zowel referenties delen in uw toepassingen en de makelaar roepen als dit aanwezig is op het apparaat is.
