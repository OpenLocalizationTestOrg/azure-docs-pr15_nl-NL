<properties
    pageTitle="Wat is er gebeurd met mijn WebApi-project (Visual Studio Azure Active Directory verbonden service) | Microsoft Azure "
    description="Beschrijving van wat er gebeurt met uw project MVC WebApi u verbinding met Azure AD maakt met behulp van Visual Studio"
  services="active-directory"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Wat is er gebeurd met mijn WebApi-project (Visual Studio Azure Active Directory verbonden service)

> [AZURE.SELECTOR]
> - [Aan de slag](vs-active-directory-webapi-getting-started.md)
> - [Wat is er gebeurd](vs-active-directory-webapi-what-happened.md)

##<a name="references-have-been-added"></a>Verwijzingen zijn toegevoegd

###<a name="nuget-package-references"></a>NuGet pakket verwijzingen

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###<a name="net-references"></a>.NET-verwijzingen

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

##<a name="code-changes"></a>Codewijzigingen

###<a name="code-files-were-added-to-your-project"></a>Codebestanden zijn toegevoegd aan uw project

Een klasse verificatie opstarten **App_Start/Startup.Auth.cs** is toegevoegd aan uw project met opstarten logica voor Azure AD-verificatie.

###<a name="startup-code-was-added-to-your-project"></a>Opstartcode is toegevoegd aan uw project

Als u al een Opstartklasse in uw project, de methode **configuratie** is bijgewerkt met een oproep naar `ConfigureAuth(app)`. Een Opstartklasse is anders wordt toegevoegd aan uw project.


###<a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Uw bestand app.config of web.config heeft nieuwe configuratiewaarden.

De volgende configuratie-gegevens zijn toegevoegd.
```
    `<appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

###<a name="an-azure-ad-app-was-created"></a>Een Azure AD-App is gemaakt

Een Azure AD-toepassing is gemaakt in de map die u hebt geselecteerd in de wizard.

[Meer informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Als ik ingeschakeld *afzonderlijke gebruikersaccounts verificatie uitschakelt*, worden welke aanvullende wijzigingen zijn aangebracht in mijn project?
NuGet pakket verwijzingen zijn verwijderd en bestanden zijn verwijderd en back-up gemaakt. Afhankelijk van de status van uw project, is het wellicht handmatig verwijderen van extra verwijzingen of bestanden of code desgewenst wijzigen.

###<a name="nuget-package-references-removed-for-those-present"></a>NuGet pakket verwijzingen verwijderd (voor die aanwezig)

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###<a name="code-files-backed-up-and-removed-for-those-present"></a>Codebestanden back-up gemaakt en verwijderd (voor die aanwezig)

Elk van de volgende bestanden back-up gemaakt en verwijderd uit het project. Back-upbestanden bevinden zich in een map 'Back-up' in de hoofdmap van de adreslijst van het project.

- `App_Start\IdentityConfig.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Providers\ApplicationOAuthProvider.cs`

###<a name="code-files-backed-up-for-those-present"></a>Codebestanden back-up gemaakt (voor die aanwezig)

Elk van de volgende bestanden back-up is voordat het wordt vervangen. Back-upbestanden bevinden zich in een map 'Back-up' in de hoofdmap van de adreslijst van het project.

- `Startup.cs`
- `App_Start\Startup.Auth.cs`

##<a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Als ik *directory-gegevens lezen*hebt ingeschakeld, worden welke aanvullende wijzigingen zijn aangebracht in mijn project?

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Aanvullende wijzigingen zijn aangebracht in uw app.config of web.config

De volgende aanvullende configuratie-gegevens zijn toegevoegd.

```
    `<appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

###<a name="your-azure-active-directory-app-was-updated"></a>Uw Azure Active Directory-App is bijgewerkt
Uw Azure Active Directory-App is bijgewerkt met de machtiging *lezen directory-gegevens* en een extra toets is gemaakt die is vervolgens gebruikt als het *Ida: wachtwoord* in het `web.config` bestand.

[Meer informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
