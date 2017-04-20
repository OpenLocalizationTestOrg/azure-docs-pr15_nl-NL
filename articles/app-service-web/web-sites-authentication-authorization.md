<properties 
    pageTitle="Verificatie met on-premises Active Directory in uw Azure-app | Microsoft Azure" 
    description="Meer informatie over de verschillende opties voor LOB-apps in Azure App-Service om te verifiëren met lokale Active Directory" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="08/31/2016" 
    ms.author="cephalin"/>

# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>Verificatie met on-premises Active Directory in uw Azure-app #

In dit artikel leest u hoe om te verifiëren met zowel on-premises Active Directory (AD) in [Azure App-Service](../app-service/app-service-value-prop-what-is.md). Een Azure-app wordt gehost in de cloud, maar er zijn andere manieren om te verifiëren on-premises implementatie AD gebruikers veilig. 

## <a name="authenticate-through-azure-active-directory"></a>Verifiëren via Azure Active Directory
Een Azure Active Directory-tenant kan zijn directory-gesynchroniseerd met een on-premises AD. Deze methode kan AD gebruikers toegang tot uw App van internet en verifiëren met behulp van hun referenties on-premises implementatie. Bovendien biedt Azure App-Service een [oplossing inschakelen-toets voor deze methode](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Met een paar muisklikken van een knop, kunt u verificatie met een tenant directory gesynchroniseerd inschakelen voor uw Azure-app. Deze methode heeft de volgende voordelen:

-   Hoeft niet alle verificatiecode in uw app. Laat de App-Service doen? de verificatie voor u en uw tijd besteden aan het functies in uw app bieden.
-   [Azure AD Graph-API](http://msdn.microsoft.com/library/azure/hh974476.aspx) kunnen toegang tot directorygegevens uit uw Azure-app.
-   Eenmalige aanmelding biedt voor [alle toepassingen die worden ondersteund door Azure Active Directory](/marketplace/active-directory/), inclusief Office 365, Dynamics CRM Online, Microsoft Intune en duizenden niet-Microsoft cloud-toepassingen. 
-   Azure Active Directory ondersteunt Rolgebaseerd toegangsbeheer. U kunt het patroon [Authorize(Roles="X")] met minimale wijzigingen aan uw code.

Als u wilt zien hoe u schrijft een LOB-Azure app waarmee wordt geverifieerd met Azure Active Directory, raadpleegt u [een Azure LOB--app met Azure Active Directory-verificatie maken](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Verificatie via een on-premises STS
Als u een on-premises implementatie secure token service (STS) zoals Active Directory Federation Services (AD FS) hebt, kunt u die verificatie voor de Azure app een Federatie vormen. Deze methode werkt het beste wanneer bedrijfsbeleid AD-gegevens verboden kunnen worden opgeslagen in Azure wordt aangegeven. Houd echter rekening met het volgende:

-   STS topologie moet geïmplementeerd on-premises met kosten en beheer realiseren.
-   Alleen AD FS-beheerders kunnen configureren [gebruikmakende partij vertrouwensrelaties en claimen regels](http://technet.microsoft.com/library/dd807108.aspx), die mogelijk van de ontwikkelaar opties beperken. Aan de andere kant, is het mogelijk om te beheren en aanpassen van [claims](http://technet.microsoft.com/library/ee913571.aspx) op basis van de per toepassing.
-   Access naar on-premises implementatie AD gegevens is vereist een aparte oplossing tot en met de bedrijfsfirewall.

Om te zien hoe een LOB-Azure app waarmee wordt geverifieerd schrijven met een on-premises implementatie-STS, raadpleegt u [een Azure LOB--app met AD FS-verificatie maken](web-sites-dotnet-lob-application-adfs.md).
 
