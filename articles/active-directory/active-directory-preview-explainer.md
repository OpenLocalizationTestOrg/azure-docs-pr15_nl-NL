<properties
    pageTitle="Azure Active Directory preview explainer | Microsoft Azure"
    description="Een onderwerp waarin wordt uitgelegd van de verschillen tussen Azure Active Directory in de klassieke portal en het voorbeeld van de Azure Active Directory in de portal van Azure."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="preview-of-the-azure-active-directory-management-experience-in-the-azure-portal"></a>Voorbeeld van de Azure Active Directory management-ervaring in de portal van Azure

De Azure Active Directory (Azure AD) management-ervaring is in de proefversie van in de portal van Azure. U kunt proberen deze uitfaden aanmeldt bij [de portal van Azure](https://portal.azure.com) als een globale beheerder van de map. Vervolgens Azure Active Directory in de servicelijst Selecteer, als dit zichtbaar is of **meer services** om weer te geven van de lijst met alle services selecteren. U hoeft niet een Azure-abonnement voor het gebruik van de Azure AD-beheer in de portal van Azure-ervaring.


## <a name="capabilities-of-the-preview-experience"></a>Mogelijkheden van de preview-ervaring

De preview-ervaring kunt u veel directory-bronnen, zoals gebruikers, groepen en -toepassingen, evenals hoofdsitemap configureren, in de portal van Azure beheren. Verbeteren deze ervaring om het opnemen van de mogelijkheden die bestaat in de Azure AD management optreden in de [klassieke Azure-portal](https://manage.windowsazure.com). Tot die tijd zijn er enkele directory management taken die u moet zich nog steeds voltooid in de klassieke portal.

## <a name="manage-the-same-azure-ad-tenants"></a>De dezelfde Azure AD-tenants beheren

De preview-ervaring gelezen en geschreven naar dezelfde Azure Active Directory tenant als de klassieke portal en het Office 365-beheercentrum. Wijzigingen in een van deze portals worden doorgevoerd in alle van de andere weergaven.

## <a name="use-the-same-authorization-logic"></a>Gebruik dezelfde autorisatie logica

De preview-ervaring gebruikt dezelfde autorisatie logica als bestaande Active Directory-clients. Gebruikers zijn geautoriseerd om directory resources op basis van hun rol directory, zoals globale beheerder, Gebruikersbeheerder, wachtwoordbeheerder te wijzigen. Een gebruiker voor het beheren van bronnen voor directory staat niet toe als u een rol op Azure resources of een Azure-abonnement hebt. Azure AD-beheerrollen, Zie [beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles.md)voor meer informatie. 

De preview-ervaring is geoptimaliseerd voor globale beheerders. Als u de preview-ervaring terwijl aangemeld als een gebruiker die is niet een globale beheerder gebruikt, hebt u mogelijk a-ervaring, anders werken. Zoals u mogelijk op een knop waarmee u begint een taak die u op de map niet voltooien. We verbeteren dit probleem snel.
 
## <a name="tell-us-what-you-think"></a>Vertel ons wat u ervan vindt

U kunt feedback geven over de preview-ervaring in de portal sectie beheer van de [Azure AD feedback-forum](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD&filter=alltypes&sort=lastpostdesc).
