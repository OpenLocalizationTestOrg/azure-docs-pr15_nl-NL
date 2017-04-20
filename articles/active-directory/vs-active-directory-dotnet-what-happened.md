<properties
    pageTitle="Wat is er gebeurd met mijn MVC-project (Visual Studio Azure Active Directory verbonden service) | Microsoft Azure "
    description="Wordt beschreven wat gebeurt er met uw project MVC wanneer u verbinding met Azure AD maakt met behulp van Visual Studio verbonden services"
    services="active-directory"
    documentationCenter="na"
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

# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Wat is er gebeurd met mijn MVC-project (Visual Studio Azure Active Directory verbonden service)?

> [AZURE.SELECTOR]
> - [Aan de slag](vs-active-directory-dotnet-getting-started.md)
> - [Wat is er gebeurd](vs-active-directory-dotnet-what-happened.md)



## <a name="references-have-been-added"></a>Verwijzingen zijn toegevoegd

### <a name="nuget-package-references"></a>NuGet pakket verwijzingen

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>.NET-verwijzingen

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel**
- **System.IdentityModel.Tokens.Jwt**
- **System.Runtime.Serialization**

## <a name="code-has-been-added"></a>Code is toegevoegd

### <a name="code-files-were-added-to-your-project"></a>Codebestanden zijn toegevoegd aan uw project

Een klasse verificatie opstarten **App_Start/Startup.Auth.cs** is toegevoegd aan uw project met opstarten logica voor Azure AD-verificatie. Een klasse controller, Controllers/AccountController.cs is ook toegevoegd die methoden **SignIn()** en **SignOut()** bevat. Ten slotte een gedeeltelijke weergave, **Views/Shared/_LoginPartial.cshtml** is toegevoegd met een koppeling naar actie voor aanmelding bij/SignOut.

### <a name="startup-code-was-added-to-your-project"></a>Opstartcode is toegevoegd aan uw project

Als u al een Opstartklasse in uw project, is de methode **configuratie** bijgewerkt met een oproep naar **ConfigureAuth(app)**. Een Opstartklasse is anders wordt toegevoegd aan uw project.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>Uw app.config of web.config heeft nieuwe configuratiewaarden

De volgende configuratie-gegevens zijn toegevoegd.


    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Een App Azure Active Directory (AD) is gemaakt
Een Azure AD-toepassing is gemaakt in de map die u hebt geselecteerd in de wizard.

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Als ik ingeschakeld *afzonderlijke gebruikersaccounts verificatie uitschakelt*, worden welke aanvullende wijzigingen zijn aangebracht in mijn project?
NuGet pakket verwijzingen zijn verwijderd en bestanden zijn verwijderd en back-up gemaakt. Afhankelijk van de status van uw project, is het wellicht handmatig verwijderen van extra verwijzingen of bestanden of code desgewenst wijzigen.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet pakket verwijzingen verwijderd (voor die aanwezig)

- **Microsoft.AspNet.Identity.Core**
- **Microsoft.AspNet.Identity.EntityFramework**
- **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Codebestanden back-up gemaakt en verwijderd (voor die aanwezig)

Elk van de volgende bestanden back-up gemaakt en verwijderd uit het project. Back-upbestanden bevinden zich in een map 'Back-up' in de hoofdmap van de adreslijst van het project.

- **App_Start\IdentityConfig.cs**
- **Controllers\ManageController.cs**
- **Models\IdentityModels.cs**
- **Models\ManageViewModels.cs**

### <a name="code-files-backed-up-for-those-present"></a>Codebestanden back-up gemaakt (voor die aanwezig)

Elk van de volgende bestanden back-up is voordat het wordt vervangen. Back-upbestanden bevinden zich in een map 'Back-up' in de hoofdmap van de adreslijst van het project.

- **Startup.cs**
- **App_Start\Startup.auth.cs**
- **Controllers\AccountController.cs**
- **Views\Shared\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Als ik *directory-gegevens lezen*hebt ingeschakeld, worden welke aanvullende wijzigingen zijn aangebracht in mijn project?

Aanvullende verwijzingen zijn toegevoegd.

###<a name="additional-nuget-package-references"></a>Aanvullende NuGet pakket verwijzingen

- **EntityFramework**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **System.Spatial**

###<a name="additional-net-references"></a>Aanvullende .NET-verwijzingen

- **EntityFramework**
- **EntityFramework.SqlServer**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
- **System.Spatial**

###<a name="additional-code-files-were-added-to-your-project"></a>Extra Code-bestanden zijn toegevoegd aan uw project

Twee bestanden zijn toegevoegd aan de ondersteuning voor token caching: **Models\ADALTokenCache.cs** en **Models\ApplicationDbContext.cs**.  Een extra controller en de weergave zijn toegevoegd aan de toegang tot gebruikersprofielinformatie met Azure graph API's illustreren.  Deze bestanden zijn **Controllers\UserProfileController.cs** en **Views\UserProfile\Index.cshtml**.

###<a name="additional-startup-code-was-added-to-your-project"></a>Aanvullende opstarten-code is toegevoegd aan uw project

Een nieuw **OpenIdConnectAuthenticationNotifications** -object is in het bestand **startup.auth.cs** toegevoegd aan het lid **meldingen** van de **OpenIdConnectAuthenticationOptions**.  Dit is om te schakelen ontvangen het OAuth-code en deze exchange voor een toegangstoken.

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Aanvullende wijzigingen zijn aangebracht in uw app.config of web.config

De volgende aanvullende configuratie-gegevens zijn toegevoegd.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

De volgende configuratiesecties en de verbindingsreeks zijn toegevoegd.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


###<a name="your-azure-active-directory-app-was-updated"></a>Uw Azure Active Directory-App is bijgewerkt
Uw Azure Active Directory-App is bijgewerkt met de machtiging *lezen directory-gegevens* en een extra toets is gemaakt die vervolgens als de *ida: ClientSecret* in het bestand **web.config** is gebruikt.

[Meer informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
