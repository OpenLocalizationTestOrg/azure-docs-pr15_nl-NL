<properties
   pageTitle="Autorisatie in multitenant toepassingen | Microsoft Azure"
   description="Hoe u autorisatie uitvoert in een toepassing voor multitenant"
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
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="role-based-and-resource-based-authorization-in-multitenant-applications"></a>Op basis van rollen en op basis van een resource autorisatie in multitenant toepassingen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

In dit artikel maakt [deel uit van een reeks]. Er is ook een volledige [voorbeeldtoepassing] waarop deze reeks.

Onze [verwijzing-implementatie] is een ASP.NET Core 1.0-toepassing. In dit artikel bespreken we bij twee algemene benaderingen naar autorisatie, met de vergunning API's die beschikbaar zijn in ASP.NET Core 1.0.

-   **Rollen gebaseerde verificatie**. Een actie op basis van de rollen toegewezen aan een gebruiker toestemming. Sommige bewerkingen moet bijvoorbeeld een beheerdersrol.
-   **Autorisatie op basis van een resource**. Een actie op basis van een bepaalde resource toestemming. Elke resource heeft bijvoorbeeld een eigenaar. De eigenaar van kunt de resource; verwijderen andere gebruikers kunnen niet.

Een typische app maakt gebruik van een combinatie van beide. Bijvoorbeeld als u wilt verwijderen van een resource, moet de gebruiker de resource eigenaar _of_ beheerder.


## <a name="role-based-authorization"></a>Rollen gebaseerde verificatie

De [De enquêtes] [ Tailspin] toepassing definieert de volgende rollen:

- Beheerder. Alle CRUD-bewerkingen in een enquête maken die bij die tenant hoort kunnen worden uitvoeren.
- Maker. Nieuwe enquêtes kunt maken
- Reader. Een enquêtes die deel uitmaken van die tenant kunnen worden gelezen

Rollen gelden voor _gebruikers_ van de toepassing. Klik in de toepassing enquêtes is een gebruiker een administrator, creator, of lezer.

Zie de [Toepassingsrollen]voor een discussie van hoe definieert en rollen beheren.

Ongeacht hoe u de rollen beheren, uw autorisatiecode ziet er ongeveer uit. ASP.NET-Core 1.0 maakt u kennis met een abstractie [autorisatie beleidsregels]genoemd[policies]. Met deze functie kunt u autorisatie beleidsregels definiëren in code, en klikt u vervolgens beleid van toepassing op de controller acties. Het beleid is van de controller ontkoppelde.

### <a name="create-policies"></a>Beleid maken

Als u wilt een beleid definieert, moet u eerst een klasse die implementeert maakt `IAuthorizationRequirement`. Het gemakkelijkst afgeleid van `AuthorizationHandler`. In de `Handle` methode, de relevante claim(s) onderzoeken.

Hier volgt een voorbeeld van de configuratietoepassing de enquêtes:

```csharp
public class SurveyCreatorRequirement : AuthorizationHandler<SurveyCreatorRequirement>, IAuthorizationRequirement
{
    protected override void Handle(AuthorizationContext context, SurveyCreatorRequirement requirement)
    {
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin) ||
            context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            context.Succeed(requirement);
        }
    }
}
```

> [AZURE.NOTE] Zie [SurveyCreatorRequirement.cs]

Deze klasse definieert de vereiste voor een gebruiker maakt u een nieuwe enquête. De gebruiker moet zich in de rol SurveyAdmin of SurveyCreator.

Definieer een benoemde beleid met een of meer vereisten in uw klas opstarten. Als er meerdere vereisten, kan de gebruiker moet voldoen aan _elke_ willen worden toegestaan. De volgende code definieert twee beleidsregels:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy(PolicyNames.RequireSurveyCreator,
        policy =>
        {
            policy.AddRequirements(new SurveyCreatorRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });

    options.AddPolicy(PolicyNames.RequireSurveyAdmin,
        policy =>
        {
            policy.AddRequirements(new SurveyAdminRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });
});
```

> [AZURE.NOTE] Zie [Startup.cs]

Deze code wordt ook het schema voor verificatie, waarin wordt uitgelegd welke middleware verificatie moet worden uitgevoerd als autorisatie mislukt ASP.NET. We opgeven in dit geval de cookie verificatie middleware, omdat de cookie verificatie middleware de gebruiker naar een pagina 'omleiden kan'. De locatie van de pagina is ingesteld in de optie AccessDeniedPath voor de middleware cookie; Zie [de middleware verificatie configureren].

### <a name="authorize-controller-actions"></a>Autoriseer controller acties

Ten slotte, als u akkoord gaat een actie in een MVC controller, het beleid instellen in de `Authorize` kenmerk:

```csharp
[Authorize(Policy = "SurveyCreatorRequirement")]
public IActionResult Create()
{
    // ...
}
```

In eerdere versies van ASP.NET stelt u de eigenschap **rollen** op het kenmerk:

```csharp
// old way
[Authorize(Roles = "SurveyCreator")]

```

Dit nog steeds wordt ondersteund in ASP.NET Core 1.0, maar er enkele nadelen vergeleken met autorisatie beleidsregels:

-   Er wordt een bepaalde claimtype aangenomen. Beleidsregels kunnen voor elk gewenst claimtype controleren. Rollen zijn een type claimen.
-   Naam van de rol is vastgelegde in het kenmerk. Met beleidsregels is de logica autorisatie allemaal op één plaats beschikbaar, zodat u deze eenvoudiger naar bijwerken of zelfs laden uit configuratie-instellingen.
-   Beleidsregels inschakelen complexere autorisatiebeslissingen (bijvoorbeeld ouderdom > = 21) die op lidmaatschap van een eenvoudige rol kan niet worden aangegeven.

## <a name="resource-based-authorization"></a>Autorisatie voor resources die zijn gebaseerd

_Resources op basis van autorisatie_ treedt op wanneer de vergunning afhankelijk is van een specifieke bron die worden beïnvloed door een bewerking. Klik in de toepassing de enquêtes heeft elke enquête een eigenaar en nul-op-veel inzenders.

-   De eigenaar kunt lezen, bijwerken, verwijderen, publiceren en publicatie ongedaan maken van de enquête.
-   De eigenaar kunt inzenders toewijzen aan de enquête.
-   Inzenders kunnen lezen en bijwerken van de enquête.

Houd er rekening mee dat "eigenaar" en "Inzender" niet rollen van toepassing zijn; ze zijn opgeslagen per enquête, in de databasetoepassing. Als u wilt controleren of een gebruiker een enquête kunt verwijderen, bijvoorbeeld de app wordt gecontroleerd of de gebruiker de eigenaar van deze enquête is.

In ASP.NET Core 1.0, implementeert autorisatie op basis van een resource die voortvloeien uit **AuthorizationHandler** en de methode **verwerken** overschrijven.

```csharp
public class SurveyAuthorizationHandler : AuthorizationHandler<OperationAuthorizationRequirement, Survey>
{
     protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
    {
    }
}
```

Zoals u ziet dat deze klasse voor enquête objecten wordt ingevoerd.  Klasse voor DI registreren bij het opstarten:

```csharp
services.AddSingleton<IAuthorizationHandler>(factory =>
{
    return new SurveyAuthorizationHandler();
});
```

Als u wilt uitvoeren autorisatie controles, gebruikt u de **IAuthorizationService** -interface, die u in de controllers invoeren kunt. De volgende code wordt gecontroleerd of een gebruiker een enquête kunt lezen:

```csharp
if (await _authorizationService.AuthorizeAsync(User, survey, Operations.Read) == false)
{
    return new HttpStatusCodeResult(403);
}
```

Omdat geven we in een `Survey` object aan, in dit gesprek wordt aangeroepen de `SurveyAuthorizationHandler`.

Een goede manier is in uw autorisatiecode statistische alle van de machtigingen op basis van rollen en resources op basis van de gebruiker en klik vervolgens selectievakje de aggregatie instellen ten opzichte van de gewenste bewerking.
Hier volgt een voorbeeld van de app enquêtes. De toepassing definieert verschillende typen van de machtiging:

- Beheerder
- Inzender
- Creator
- Eigenaar
- Lezer

De toepassing definieert ook een aantal mogelijke bewerkingen op enquêtes:

- Maken
- Lezen
- Update
- Verwijderen
- Publiceren
- Unpublsh

De volgende code maakt een lijst met machtigingen voor een bepaalde gebruiker en de enquête. U ziet dat deze code analyseert de rollen van de app van de gebruiker en de eigenaar/Inzender velden in de enquête.

```csharp
protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
{
    var permissions = new List<UserPermissionType>();
    string userTenantId = context.User.GetTenantIdValue();
    int userId = ClaimsPrincipalExtensions.GetUserKey(context.User);
    string user = context.User.GetUserName();

    if (resource.TenantId == userTenantId)
    {
        // Admin can do anything, as long as the resource belongs to the admin's tenant.
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin))
        {
            context.Succeed(operation);
            return;
        }

        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            permissions.Add(UserPermissionType.Creator);
        }
        else
        {
            permissions.Add(UserPermissionType.Reader);
        }

        if (resource.OwnerId == userId)
        {
            permissions.Add(UserPermissionType.Owner);
        }
    }
    if (resource.Contributors != null && resource.Contributors.Any(x => x.UserId == userId))
    {
        permissions.Add(UserPermissionType.Contributor);
    }
    if (ValidateUserPermissions[operation](permissions))
    {
        context.Succeed(operation);
    }
}
```

> [AZURE.NOTE] Zie [SurveyAuthorizationHandler.cs].

In een toepassing voor meerdere tenant, moet u ervoor zorgen dat machtigingen niet "" met gegevens van de andere tenant lekken. In de app enquêtes de machtiging Inzender is toegestaan in tenants &mdash; kunt u iemand toewijzen van een andere tenant als een contriubutor. Andere machtigingstypen zijn beperkt tot resources die deel uitmaakt van de tenant van deze gebruiker. Als u deze vereiste, controleert de code de ID van de tenant voordat u de toestemming. (De `TenantId` veld toegewezen wanneer de enquête wordt gemaakt.)

De volgende stap is om te controleren van de bewerking ten opzichte van de machtigingen (lezen, bijwerken, verwijderen, enzovoort). De app enquêtes implementeert deze stap met behulp van een opzoektabel van functies:

```csharp
static readonly Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>> ValidateUserPermissions
    = new Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>>

    {
        { Operations.Create, x => x.Contains(UserPermissionType.Creator) },

        { Operations.Read, x => x.Contains(UserPermissionType.Creator) ||
                                x.Contains(UserPermissionType.Reader) ||
                                x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Update, x => x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Delete, x => x.Contains(UserPermissionType.Owner) },

        { Operations.Publish, x => x.Contains(UserPermissionType.Owner) },

        { Operations.UnPublish, x => x.Contains(UserPermissionType.Owner) }
    };
```


## <a name="next-steps"></a>Volgende stappen

- Lees het volgende artikel in deze reeks: [een backend beveiligen web API in een toepassing voor multitenant][web-api]
- Meer informatie over de resource op basis van autorisatie in ASP.NET 1.0 Core, Zie [Autorisatie voor resources op basis van][rbac].

<!-- Links -->
[Tailspin]: guidance-multitenant-identity-tailspin.md
[een reeks hoort]: guidance-multitenant-identity.md
[Toepassingsrollen]: guidance-multitenant-identity-app-roles.md
[policies]: https://docs.asp.net/en/latest/security/authorization/policies.html
[rbac]: https://docs.asp.net/en/latest/security/authorization/resourcebased.html
[verwijzing-implementatie]: guidance-multitenant-identity-tailspin.md
[SurveyCreatorRequirement.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyCreatorRequirement.cs
[Startup.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Web/Startup.cs
[De verificatie middleware configureren]: guidance-multitenant-identity-authenticate.md#configuring-the-authentication-middleware
[SurveyAuthorizationHandler.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyAuthorizationHandler.cs
[van voorbeeldtoepassing]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[web-api]: guidance-multitenant-identity-web-api.md
