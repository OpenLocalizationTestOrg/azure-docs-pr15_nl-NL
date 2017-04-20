<properties
    pageTitle="Azure Active Directory-B2C: En beperkingen | Microsoft Azure"
    description="Een lijst met beperkingen met Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-limitations-and-restrictions"></a>Azure Active Directory-B2C: Beperkingen en beperkingen

Er zijn verschillende functies en mogelijkheden van Azure Active Directory (Azure AD) B2C die nog niet worden ondersteund. Veel van deze bekende problemen en beperkingen moeten worden verzonden gaan, maar u rekening moet houden van deze als u samenstelt met Azure AD B2C consumenten gerichte-toepassingen.

## <a name="issues-during-the-creation-of-azure-ad-b2c-tenants"></a>Problemen tijdens het maken van Azure AD B2C tenants

Als er problemen tijdens het [maken van een tenant Azure AD B2C optreden](active-directory-b2c-get-started.md), raadpleegt u [een Azure AD-tenant of een B2C van Azure AD-tenant--problemen en oplossingen maken](active-directory-b2c-support-create-directory.md) voor instructies.

Merk op dat er zijn bekende problemen wanneer u een bestaande B2C-tenant verwijderen en opnieuw met dezelfde domeinnaam maken. U moet een tenant B2C maken met een andere domeinnaam.

## <a name="note-about-b2c-tenant-quotas"></a>Houd er rekening mee over B2C tenant quota

Standaard is het aantal gebruikers in een tenant B2C beperkt tot 50.000 gebruikers. Als u nodig hebt die het quotum van uw tenant B2C, moet u contact opnemen met de ondersteuning.

## <a name="branding-issues-on-verification-email"></a>Problemen op verificatie-e-mailbericht met huisstijl

Het standaard verificatie-e-mailbericht bevat de huisstijl van Microsoft. We worden deze in de toekomst verwijderd. Nu kunt u deze verwijderen door met de [bestaande huisstijl functie bedrijf](../active-directory/active-directory-add-company-branding.md).

## <a name="restrictions-on-applications"></a>Beperkingen voor toepassingen

De volgende typen toepassingen worden momenteel niet ondersteund in Azure AD B2C. Raadpleeg voor een beschrijving van de ondersteunde typen toepassingen, [Azure AD B2C: typen toepassingen](active-directory-b2c-apps.md).

### <a name="single-page-applications-javascript"></a>Eén pagina-toepassingen (JavaScript)

Veel moderne toepassingen hebben een één pagina toepassing WACHTWOORDVERIFICATIE front die hoofdzakelijk in JavaScript is geschreven en wordt vaak gebruikt een kader beveiligd-WACHTWOORDVERIFICATIE zoals AngularJS, Ember.js, Durandal, enzovoort. Deze stroom is nog niet beschikbaar in Azure AD B2C.

### <a name="daemons--server-side-applications"></a>Daemons / serverzijde toepassingen

Toepassingen die langdurige processen bevatten of die worden toegepast zonder de aanwezigheid van een gebruiker moeten ook toegang krijgen tot beveiligde resources, zoals Web-API's. Deze toepassingen kunnen verifiëren en tokens verkrijgen met behulp van de identiteit van de toepassing (in plaats van een consument gedelegeerd identiteit) in de [OAuth 2.0-client referenties stroom](active-directory-b2c-reference-protocols.md#oauth2-client-credentials-grant-flow). Deze stroom is nog niet beschikbaar in Azure AD B2C, zodat voorlopig toepassingen tokens krijgen kunnen alleen nadat een interactieve consumenten aanmeldingsproblemen stroom is opgetreden.

### <a name="standalone-web-apis"></a>Zelfstandige Web API 's

In de Azure AD B2C moet u de mogelijkheid om te [maken van een Web-API die is beveiligd met behulp van OAuth 2.0 tokens](active-directory-b2c-apps.md#web-apis). Echter is die Web API alleen mogelijk tokens ontvangen van een client dat de dezelfde toepassing-ID deelt. Samenstellen van een Web-API dat wordt geopend vanuit verschillende verschillende clients wordt niet ondersteund.

### <a name="web-api-chains-on-behalf-of"></a>Web API komen (op-namens-Of)

Veel architecturen bevatten een Web-API die vereist zijn voor het bellen van een andere volgende Web API, beide beveiligd met Azure AD B2C. Dit scenario geldt in systeemeigen clients die een Web API back-end die op zijn beurt een Microsoft-onlineservice zoals de API Azure AD Graph aanroept hebben.

Dit gekoppelde Web API-scenario kan worden ondersteund met behulp van het OAuth 2.0 Jwt vruchtdragende referentie verlenen, ook bekend als de stroom aan-namens-Of. De stroom aan-namens-Of is echter niet momenteel geïmplementeerd in de Azure AD-B2C.

## <a name="restriction-on-libraries-and-sdks"></a>Beperking voor bibliotheken en SDK 's

De set bibliotheken van Microsoft die worden ondersteund die Azure AD B2C werken is op dit moment zeer beperkt. We hebben ondersteuning voor .NET op basis van WebApps en services, evenals NodeJS WebApps en services.  We hebben ook een preview .NET-clientbibliotheek bekend als MSAL die kan worden gebruikt met Azure AD B2C in Windows en andere .NET-apps.

We hebben momenteel geen ondersteuning voor andere talen en platforms, waaronder iOS en Android bibliotheek.  Als u maken op een ander platform dan de wilt hierboven genoemde, wordt u aangeraden met behulp van een open source SDK, verwijzen naar onze [OAuth 2.0 en OpenID verbinding Protocol verwijzing](active-directory-b2c-reference-protocols.md) indien nodig.  Azure AD B2C implementeert OAuth & OpenID verbinding kunt maken, waardoor u deze een algemene OAuth of OpenID verbinding-bibliotheek voor integratie gebruiken.

Onze iOS en Android snel aan de slag-zelfstudies gebruik open source-bibliotheken die we hebben getest voor compatibiliteit met Azure AD B2C.  Al onze zelfstudies snel begin zijn beschikbaar in onze sectie [aan de slag](active-directory-b2c-overview.md#getting-started) .

## <a name="restriction-on-protocols"></a>Beperking van protocollen

Azure AD B2C ondersteunt OpenID verbinding en OAuth 2.0. Echter niet alle functies en mogelijkheden van elke protocol zijn geïmplementeerd. Voor een beter begrip van het bereik van de ondersteunde protocol functionaliteit in Azure AD B2C, Lees onze [OpenID verbinding en OAuth 2.0 protocol verwijzing](active-directory-b2c-reference-protocols.md). Op SAML en WS-Fed protocolondersteuning is niet beschikbaar.

## <a name="restriction-on-tokens"></a>Beperking van tokens

Veel van de tokens uitgegeven door Azure AD B2C worden geïmplementeerd als JSON Web Tokens of JWTs. Niet alle gegevens in JWTs ('claims' genoemd) is echter helemaal wanneer deze moet of ontbreekt. Enkele voorbeelden zijn de 'sub' en 'preferred_username' claims.  Als de waarden, opmaak of zin van claims wijzigen na verloop van tijd, tokens voor uw bestaande beleidsregels, blijven ongewijzigd: u kunt vertrouwen van de waarden in productie-apps.  Als waarden veranderen, krijgt we u de mogelijkheid om deze wijzigingen configureren voor elk van uw beleid.  Lees meer informatie over de tokens dat momenteel door de service Azure AD B2C, onze [token verwijzing](active-directory-b2c-reference-tokens.md).

## <a name="restriction-on-nested-groups"></a>Beperking van geneste groepen

Geneste groepslidmaatschappen worden niet ondersteund in Azure AD B2C tenants. We wilt niet dat deze mogelijkheid toevoegen.

## <a name="restriction-on-differential-query-feature-on-azure-ad-graph-api"></a>Beperking van de functie differentiële query op Azure AD Graph-API

De [functie voor differentiële query op Azure AD Graph API](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-differential-query) wordt niet ondersteund in Azure AD B2C tenants. Dit is op onze langdurige routekaart.

## <a name="issues-with-user-management-on-the-azure-classic-portal"></a>Problemen met Gebruikersbeheer in de klassieke Azure-portal

B2C functies zijn toegankelijk is op de Azure-portal. U kunt echter de Azure klassieke portal gebruiken voor toegang tot andere functies van de tenant, waaronder Gebruikersbeheer. Er zijn momenteel een aantal bekende problemen met Gebruikersbeheer (het tabblad van de **gebruikers** ) op de Azure klassieke portal:

- Het veld **Gebruikersnaam** niet voor de gebruiker van een lokale account (dat wil zeggen een consument die zich registreert met een e-mailadres en wachtwoord, of een gebruikersnaam en wachtwoord), komen overeen met het aanmelden id (e-mailadres of gebruikersnaam) dat is gebruikt tijdens het aanmelden. Dit is omdat het veld wordt weergegeven op de portal van Azure klassieke daadwerkelijk de UPN (User Principal Name), wordt niet gebruikt in B2C scenario's. Als u wilt de id aanmeldingsproblemen van de lokale account weergeven, zoek het gebruikersobject in [Graph Explorer](https://graphexplorer.cloudapp.net/). Vindt u hetzelfde probleem met een gebruiker sociale-account (dat wil zeggen een consument die zich registreert met Facebook, Google +, enz.), maar in dat geval er is geen id aanmeldingsproblemen van spreken.

    ![Lokale account - UPN](./media/active-directory-b2c-limitations/limitations-user-mgmt.png)

- Voor een lokale account gebruiker wordt u niet kunt bewerken op een van de velden en wijzigingen opslaan op het tabblad **profiel** .

## <a name="issues-with-admin-initiated-password-reset-on-the-azure-classic-portal"></a>Problemen met door de beheerder geactiveerde wachtwoord opnieuw in op de Azure klassieke-portal

Als u het wachtwoord opnieuw instellen voor een lokale consumenten op basis van een account op de Azure klassieke-portal ( **Wachtwoord opnieuw instellen** van de opdracht op het tabblad **gebruikers** ), die consumenten is niet mogelijk zijn of haar wachtwoord wijzigen voor de volgende aanmelden, als u een teken omhoog of beleid, aanmelden en afmelden bij uw toepassingen vergrendeld. Als een tijdelijke oplossing gebruiken om de [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) van de consument wachtwoord (zonder bijna verlopen wachtwoorden) of een aanmelden beleid in plaats van een teken omhoog of beleid aanmelden.

## <a name="issues-with-creating-a-custom-attribute"></a>Problemen met het maken van een aangepaste kenmerk

Een [aangepaste kenmerk toegevoegd op de Azure-portal](active-directory-b2c-reference-custom-attr.md) is niet onmiddellijk gemaakt in uw tenant B2C. Moet u het aangepaste kenmerk in ten minste één van uw beleid voor deze gebruiken in uw tenant B2C worden gemaakt en beschikbaar zijn via Graph API.

## <a name="issues-with-verifying-a-domain-on-the-azure-classic-portal"></a>Problemen met het verifiëren van een domein op de Azure klassieke-portal

Momenteel niet kunt u een domein op de [portal van Azure klassieke](https://manage.windowsazure.com/)verifiëren.

## <a name="issues-with-sign-in-with-mfa-policy-on-safari-browsers"></a>Problemen met aanmelden met MFA beleid in Safari browsers

Aanvragen voor aanmelden beleid (met MFA ingeschakeld) fail afwisselend in Safari browsers met HTTP 400 (ongeldige aanvraag) fouten. Dit is de einddatum van Safari lage cookie limieten voor de bestandsgrootte. Er zijn enkele oplossingen voor dit probleem:

- Gebruik het 'inschrijfformulier of aanmeldingsproblemen beleid"in plaats van het 'aanmelden beleid".
- Beperk het aantal **toepassing claims** in het beleid wordt aangevraagd.
