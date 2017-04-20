<properties 
   pageTitle="Een Azure Active Directory toevoegen met behulp van verbonden Services in Visual Studio | Microsoft Azure"
   description="Een Azure Active Directory toevoegen met behulp van het dialoogvenster Visual Studio verbonden Services toevoegen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="active-directory"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Een Azure Active Directory toevoegen met behulp van verbonden Services in Visual Studio 

##<a name="overview"></a>Overzicht
Met behulp van Azure Active Directory (Azure AD), kunt u eenmalige aanmelding (SSO) voor webtoepassingen ASP.NET MVC of AD-verificatie in Web API-services ondersteunen. Uw gebruikers kunnen hun accounts van Azure AD gebruiken verbinding maken met uw webtoepassingen met Azure AD-verificatie. Wanneer een API van een webtoepassing weergeeft, ook de voordelen van Azure AD-verificatie met Web API verbeterde gegevensbeveiliging. Met Azure AD hoeft u niet te beheren een afzonderlijke verificatie-systeem met een eigen management account en gebruiker.

## <a name="supported-project-types"></a>Ondersteunde projecttypen

Verbinding maken met Azure AD in de volgende projecttypen kunt u het dialoogvenster verbonden Services.

- ASP.NET-MVC projecten

- ASP.NET-webpagina API projecten


### <a name="connect-to-azure-ad-using-the-connected-services-dialog"></a>Verbinding maken met Azure AD met het dialoogvenster verbonden Services

1. Zorg ervoor dat u een Azure-account hebt. Als u niet een Azure-account hebt, kunt u zich registreren voor een [gratis proefversie](http://go.microsoft.com/fwlink/?LinkId=518146).

1. In Visual Studio, opent u het snelmenu van het knooppunt **verwijzingen** in uw project en kies **Verbonden Services toevoegen**.
1. Selecteer **Azure AD-verificatie** en kies vervolgens **configureren**.

    ![Kies toevoegen Azure AD-verificatie](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. Schakel op de eerste pagina van de **configureren Azure AD-verificatie** **configureren eenmalige aanmelding met Azure AD**.

    Als uw project is geconfigureerd met een andere verificatie-configuratie, de wizard wordt u gewaarschuwd dat de vorige configuratie wordt uitgeschakeld als u een doorlopende.

    ![Azure AD configureren in de wizard](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1.  Selecteer een domein uit de vervolgkeuzelijst van het **domein** op de tweede pagina. De lijst met domeinen bevat alle domeinen toegankelijk met de rekeningen in het dialoogvenster Accountinstellingen. Als alternatief, kunt u een domeinnaam invoeren als u de sectie die u, zoals mydomain.onmicrosoft.com zoekt niet vinden. U kunt de optie voor het maken van een nieuwe Azure AD-app of gebruik de instellingen van een bestaande Azure AD-app. 

    ![Azure AD configureren in de wizard](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)


1. Klik op de derde pagina van de wizard, controleert u of **directorygegevens lezen** is ingeschakeld. De wizard wordt ingevuld met het **geheim Client**. 

    ![Azure AD configureren in de wizard](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Kies de knop **Voltooien** . De benodigde configuratiecode en verwijzingen naar uw project voor Azure AD-verificatie inschakelen, wordt het dialoogvenster toegevoegd. Op de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), kunt u het domein AD zien.

1. Bekijk de pagina aan de slag dat wordt weergegeven in uw browser voor ideeën over Vervolgstappen en de pagina Wat is er gebeurd om te zien hoe uw project is gewijzigd. Als u wilt controleren of alles heeft gewerkt, opent u een van de gewijzigde configuratiebestanden en controleer staan de instellingen die worden genoemd in wat is er gebeurd automatisch weergegeven. De belangrijkste web.config in een project ASP.NET MVC hebben bijvoorbeeld deze instellingen toegevoegd:

        <appSettings> 
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:AADInstance" value="https://login.windows.net/" />
            <add key="ida:Domain" value="Your selected domain" />
            <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
            <add key="ida:PostLogoutRedirectUri" value="The default redirect URI from the project" />
        </appSettings>

## <a name="how-your-project-is-modified"></a>Hoe uw project is gewijzigd

Wanneer u de wizard uitvoert, wordt er in Visual Studio wordt toegevoegd Azure AD en dat is gekoppeld verwijzingen aan uw project. Van configuratiebestanden en codebestanden in uw project zijn ook als u wilt toevoegen van ondersteuning voor Azure AD gewijzigd. De specifieke wijzigingen dat Visual Studio maakt, hangt af van het projecttype. Zie voor gedetailleerde informatie over hoe ASP.NET MVC projecten zijn gewijzigd, [Wat is er gebeurd – MVC projecten](http://go.microsoft.com/fwlink/p/?LinkID=513809). Zie [Wat is er gebeurd: Web API projecten](http://go.microsoft.com/fwlink/p/?LinkId=513810)voor Web API projecten.

##<a name="next-steps"></a>Volgende stappen

Vragen kunt stellen en u kunt hulp krijgen.

 - [MSDN-Forum: Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)

 - [Azure AD-documentatie](https://azure.microsoft.com/documentation/services/active-directory/)

 - [Blogbericht: Inleiding tot Azure AD](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

