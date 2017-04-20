<properties
   pageTitle="Werken met claim gebaseerde identiteiten in multitenant toepassingen | Microsoft Azure"
   description="Hoe een gebruik vorderingen voor uitgever validatie en autorisatie"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/23/2016"
   ms.author="mwasson"/>

# <a name="working-with-claims-based-identities-in-multitenant-applications"></a>Werken met op claims gebaseerde identiteiten in multitenant toepassingen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

In dit artikel maakt [deel uit van een reeks]. Er is ook een volledige [voorbeeldtoepassing] waarop deze reeks.

## <a name="claims-in-azure-ad"></a>Op claims in Azure AD

Wanneer een gebruiker zich aanmeldt, verzendt Azure AD een ID-token met een verzameling claims over de gebruiker. Een claim is een stukje informatie, uitgedrukt in een paar sleutelwaarde /. Bijvoorbeeld `email` = `bob@contoso.com`.  Op claims hebben een uitgever &mdash; in dit geval Azure AD &mdash; die is de eenheid die wordt geverifieerd door de gebruiker en Hiermee maakt u de claims. U kunt de claims vertrouwt omdat u de uitgever vertrouwen. (Daarentegen als u niet de uitgever vertrouwen, niet vertrouwen de claims!)

Op hoog niveau:

1.  De gebruiker geverifieerd.
2.  De IDP stuurt een set van claims.
3.  De app normaliseert of wordt het claims (optioneel).
4.  De app wordt de claims autorisaties gebruikt.

In OpenID verbinding kunt maken, wordt de set claims die u bepaald door de [parameter scope] van de verificatieaanvraag. Echter problemen Azure AD voor een beperkt aantal claims tot en met OpenID verbinding maken; Zie [ondersteunde Token en claimtypen]. Als u meer informatie over de gebruiker wilt, moet u de grafiek van Azure AD-API gebruiken.

Hier zijn enkele van de claims van AAD die een app mogelijk meestal belangrijk vindt:

Typen in ID token claimen |    Beschrijving
-----------------------|--------------
AUD | Wie het token is uitgegeven voor. Dit is van de toepassing client-ID. In het algemeen wordt niet mag moet u zorgen maken over dit argument, omdat deze automatisch in de middleware worden gevalideerd. Voorbeeld:`"91464657-d17a-4327-91f3-2ed99386406f"`
groepen   | Een lijst met AAD groepen waarvan de gebruiker lid is. Voorbeeld:`["93e8f556-8661-4955-87b6-890bc043c30f", "fc781505-18ef-4a31-a7d5-7d931d7b857e"]`
ISS  | De [uitgever] van het token OIDC. Voorbeeld:`https://sts.windows.net/b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4/`
naam    | De weergavenaam van de gebruiker. Voorbeeld:`"Alice A."`
OID | De object-id voor de gebruiker in AAD. Deze waarde is het onveranderlijke en herbruikbare-id van de gebruiker. Deze waarde, e-mail, gebruiken als een unieke id voor gebruikers. e-mailadressen kunnen wijzigen. Als u de API Azure AD-grafiek in uw app gebruikt, is de object-ID die waarde kon u de profielgegevens van de query. Voorbeeld:`"59f9d2dc-995a-4ddf-915e-b3bb314a7fa4"`
rollen   | Een lijst met app-rollen voor de gebruiker. Voorbeeld:`["SurveyCreator"]`
TID | Tenant-ID. Deze waarde is een unieke identificatiecode voor de tenant Azure AD. Voorbeeld:`"b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4"`
unique_name | Een menselijke leesbare weergavenaam van de gebruiker. Voorbeeld:`"alice@contoso.com"`
UPN | UPN. Voorbeeld:`"alice@contoso.com"`

In deze tabel staan de gegevenstypen claimen zoals deze worden weergegeven in het ID-token. In ASP.NET Core 1.0 converteert de middleware OpenID verbinding enkele van de typen wanneer deze wordt gevuld met de verzameling Claims voor de gebruiker hoofdsom:

-   OID >`http://schemas.microsoft.com/identity/claims/objectidentifier`
-   TID >`http://schemas.microsoft.com/identity/claims/tenantid`
-   unique_name >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
-   UPN >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`

## <a name="claims-transformations"></a>Op claims transformaties

Tijdens de stroom verificatie wilt u mogelijk de claims die u uit de IDP wijzigen. In ASP.NET Core 1.0, kunt u claims transformatie binnen de gebeurtenis **AuthenticationValidated** uitvoeren vanuit het middleware OpenID verbinding maken. (Zie [verificatiegebeurtenissen].)

Een op claims die u tijdens **AuthenticationValidated toevoegt** worden opgeslagen in de sessie verificatiecookie. Deze niet terug gedrukt naar Azure AD.

Hier volgen enkele voorbeelden van claims-transformatie:

-   **Op claims normaliseren**of aanbrengen claims consistent zijn tussen gebruikers. Dit is het meest relevant zijn als u claims krijgt van meerdere IDPs, die verschillende typen voor vergelijkbare informatie kunnen gebruiken.
Azure AD stuurt, bijvoorbeeld een 'upn' claim met e-mail van de gebruiker. Andere IDPs mogelijk een claim "e" verzenden. De volgende code zet het claimen 'upn' in een claim "e":

    ```csharp
    var email = principal.FindFirst(ClaimTypes.Upn)?.Value;
    if (!string.IsNullOrWhiteSpace(email))
    {
      identity.AddClaim(new Claim(ClaimTypes.Email, email));
    }
    ```

- **Standaard claimen waarden** voor claims die niet aanwezig toevoegen &mdash; bijvoorbeeld een gebruiker toewijzen aan een standaardrol. In sommige gevallen kunt u dit autorisatie logica vereenvoudigen.
- **Aangepaste claimtypen** met toepassingsspecifieke informatie over de gebruiker toevoegen. U kunt bijvoorbeeld vindt u informatie over de gebruiker opslaan in een database. U kunt een aangepaste claim met deze gegevens toevoegen aan de tickets verificatie. De claim wordt opgeslagen in een cookie, zodat u hoeft slechts eenmaal kunt u deze ophalen uit de database eenmaal per login sessie. Aan de andere kant, wilt u ook voorkomen dat te groot cookies maken zodat moet u rekening houden de verhouding tussen de grootte van de cookie versus database zoekacties.   

Wanneer de stroom verificatie voltooid is, de claims zijn beschikbaar in `HttpContext.User`. Op dat moment, u moet behandeld als een alleen-lezen collectie &mdash; ze verplaatsen, bijvoorbeeld autorisaties te gebruiken.

## <a name="issuer-validation"></a>Uitgever gegevensvalidatie
In het OpenID verbinding kunt maken, de uitgever claim ("iss") geeft aan wat het IDP dat het ID-token uitgegeven. Deel van de stroom van OIDC verificatie is om te controleren of de uitgever claimen overeenkomt met de werkelijke uitgever. De middleware OIDC verwerkt dit voor u.

Azure AD, de uitgever waarde is unieke per AD-tenant (`https://sts.windows.net/<tenantID>`). Een toepassing Doe daarom een aanvullende controle, om ervoor te zorgen dat de uitgever vertegenwoordigt een tenant die aan te melden bij de app is toegestaan.

Voor een enkel-tenant-toepassing, kunt u alleen controleren of de uitgever uw eigen tenant is. In feite de middleware OIDC gebeurt dit automatisch al dan niet standaard. In een meerdere tenant-app moet u toestaan voor meerdere uitgevers, overeenkomt met de verschillende tenants. Dit is een algemene methode die u wilt gebruiken:

-   Stel in de opties van de middleware OIDC **ValidateIssuer** ONWAAR. Hiermee schakelt u de automatische controle uit.
-   Wanneer een tenant zich registreert, bewaren van de tenant is ingeschakeld en de uitgever in uw gebruikers-DB.
-   Wanneer een gebruiker zich aanmeldt, zoekt u de uitgever in de database. Als de uitgever niet kan worden gevonden, betekent dit dat die tenant is niet geregistreerd. U kunt ze omleiden naar een (+) pagina omhoog.
-  U kunt ook blacklist bepaalde tenants; bijvoorbeeld: voor klanten die niet zijn abonnement betalen.

Zie voor meer informatie over, [aanmelding en onboarding in een toepassing voor multitenant tenant][signup].

## <a name="using-claims-for-authorization"></a>Vorderingen die voortvloeien uit autorisatie gebruiken

Met claims is de identiteit van een gebruiker niet langer een monolithisch entiteit. Een gebruiker kan bijvoorbeeld een e-mailadres, telefoonnummer, verjaardag, geslacht, enzovoort. Van de gebruiker IDP opgeslagen wellicht al deze informatie. Maar wanneer u de gebruiker wordt geverifieerd, meestal krijgt u een subset van deze artikelen als claims. In dit model is de identiteit van de gebruiker gewoon een bundel van claims. Wanneer u autorisatie over een gebruiker beslissingen, zoekt u bepaalde sets van claims. Met andere woorden, de vraag uiteindelijk 'Kan gebruiker X actie uitvoeren Y' wordt "Heeft gebruiker X hebben claimen Z".

Hier volgen enkele eenvoudige patronen voor het controleren van claims.

-  Controleren dat de gebruiker een bepaalde claim hebt met een bepaalde waarde heeft:

    ```csharp
    if (User.HasClaim(ClaimTypes.Role, "Admin")) { ... }
    ```
    Deze code wordt gecontroleerd of de gebruiker een claim rol hebt met de waarde "Beheerder heeft". Het verwerkt de hoofdletters/kleine letters waar de gebruiker geen claim rol of meerdere rol claims heeft correct.

    De klasse **ClaimTypes** definieert constanten voor veelgebruikte claimtypen. U kunt echter een tekenreekswaarde gebruiken voor het claimtype.

-   Zodat u één waarde voor een claimtype, wanneer u er worden maximaal één waarde verwachten:
    ```csharp
     string email = User.FindFirst(ClaimTypes.Email)?.Value;
    ```
-   Alle waarden voor een claimtype opvragen:

    ```csharp
     IEnumerable<Claim> groups = User.FindAll("groups");
    ```

Zie voor meer informatie, [op basis van rollen en op basis van een resource autorisatie in multitenant toepassingen][authorization].

## <a name="next-steps"></a>Volgende stappen

- Lees het volgende artikel in deze reeks: [aanmelding en onboarding in een toepassing voor multitenant tenant][signup]
- Meer informatie over [op Claims gebaseerde verificatie] in de documentatie ASP.NET Core 1.0

<!-- Links -->
[een reeks hoort]: guidance-multitenant-identity.md
[parameter scope]: http://nat.sakimura.org/2012/01/26/scopes-and-claims-in-openid-connect/
[Ondersteunde Token en claimtypen]: ../active-directory/active-directory-token-and-claims.md
[uitgever]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
[Verificatiegebeurtenissen]: guidance-multitenant-identity-authenticate.md#authentication-events
[signup]: guidance-multitenant-identity-signup.md
[Op claims gebaseerde verificatie]: https://docs.asp.net/en/latest/security/authorization/claims.html
[van voorbeeldtoepassing]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[authorization]: guidance-multitenant-identity-authorize.md
