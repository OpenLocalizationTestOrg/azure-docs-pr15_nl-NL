<properties
    pageTitle="Toegang tot bronnen beheren met Azure Active Directory-groepen | Microsoft Azure"
    description="Het gebruik van groepen in Azure Active Directory voor het beheren van de gebruikerstoegang tot on-premises en cloud-toepassingen en resources."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="managing-access-to-resources-with-azure-active-directory-groups"></a>Toegang tot resources met Azure Active Directory-groepen beheren

Azure Active Directory (Azure AD) is een uitgebreide identiteits- en toegangsbeheer management oplossing vindt u een reeks robuuste mogelijkheden voor het beheren van toegang tot on-premises en cloud-toepassingen en bronnen, waaronder Microsoft online services, zoals Office 365 en de wereld van Microsoft SaaS-toepassingen. In dit artikel krijgt u een overzicht, maar als u wilt aan de slag met Azure AD groepen nu, volgt u de instructies in [de beveiligingsgroepen beheren in Azure AD](active-directory-accessmanagement-manage-groups.md). Als u zien hoe u PowerShell kunt gebruiken wilt voor het beheren van groepen in Azure Active directory vindt u meer in [de preview-cmdlets Azure Active Directory voor groepsbeheer](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).


> [AZURE.NOTE] Als u wilt gebruiken Azure Active Directory, moet u een Azure-account. Als u geen account hebt, kunt u [zich registreren voor een gratis Azure-account](https://azure.microsoft.com/pricing/free-trial/).


Azure AD is een van de belangrijkste functies de mogelijkheid voor het beheren van toegang tot bronnen. Deze resources kunnen deel uitmaken van de adreslijst, zoals in het geval van machtigingen voor het beheren van objecten door middel van rollen in de map of resources die buiten de adreslijst, zoals SaaS-toepassingen, Azure services en SharePoint-sites of op premises resources. Er zijn vier manieren die een gebruiker toegangsrechten voor een resource kan worden toegewezen:


1. Directe toewijzing

    Gebruikers kunnen door de eigenaar van deze resource rechtstreeks naar een resource worden toegewezen.

2. Groepslidmaatschap

    Een groep kan worden toegewezen aan een resource door de eigenaar van de resource en door dit te doen, het toekennen van de leden van die groepstoegang tot de resource. Lidmaatschap van de groep kan vervolgens worden beheerd door de eigenaar van de groep. De eigenaar van de resource gedelegeerd effectief, de machtiging gebruikers toewijzen aan de resource aan de eigenaar van de groep.

3. Op basis van een regel

    De eigenaar van de resource kunt een regel Express welke gebruikers moeten toegang worden toegewezen aan een resource. Het resultaat van de regel is afhankelijk van de kenmerken die wordt gebruikt in die regel en de bijbehorende waarden voor specifieke gebruikers en doet, gedelegeerd de eigenaar van de resource effectief rechts voor het beheren van toegang tot hun resource kan de gemachtigde bron voor de kenmerken die worden gebruikt in de regel. De eigenaar van de resource wordt nog steeds de regel zelf beheert en bepaalt welke kenmerken en waarden toegang tot hun resource bieden.

4. Externe authority

    De toegang tot een resource wordt afgeleid uit een externe bron; bijvoorbeeld: een groep die wordt gesynchroniseerd vanuit een gezaghebbende bron, zoals een on-premises adreslijst of een SaaS-app, zoals werkdag. De eigenaar van de resource wordt toegewezen voor de groep voor toegang tot de resource, en de externe bron worden beheerd door de leden van de groep.

  ![Overzicht van het diagram voor het beheren van access](./media/active-directory-access-management-groups/access-management-overview.png)


## <a name="watch-a-video-that-explains-access-management"></a>Bekijk een video waarin wordt uitgelegd toegangsbeheer

U kunt een korte video waarin wordt uitgelegd meer informatie over dit bekijken:

**Azure AD: Inleiding tot dynamische lidmaatschap voor groepen**

> [AZURE.VIDEO azure-ad--introduction-to-dynamic-memberships-for-groups]

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Hoe heeft toegang tot management in Azure Active Directory werken?
In het midden van de Azure AD is beheeroplossing voor toegang tot de beveiligingsgroep. Toegang tot bronnen beheren met een beveiligingsgroep is een bekende model, waardoor een flexibele en gemakkelijk te begrijpen manier voor toegang tot een resource voor het beoogde groep gebruikers. De eigenaar van de resource (of de beheerder van de map) kan toewijzen aan een groep naar een bepaalde toegang krijgen tot de resources die ze eigenaar bent van rechts. De leden van de groep krijgt het openen en de eigenaar van de resource kunt delegeren rechts voor het beheren van de lijst met leden van een groep aan iemand anders, zoals een afdelingsmanager of een beheerder van de helpdesk.

![Azure Active Directory access management-diagram](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

De eigenaar van een groep kunt ook die groep beschikbaar maken voor selfservice aanvragen. Daarbij kunt een eindgebruiker zoeken en zoek de groep en kunnen een aanvraag om deel te nemen aan, effectief die gemachtigd voor toegang tot de resources die worden beheerd via de groep. De eigenaar van de groep kunt u de groep instellen zodat automatisch worden goedgekeurd of moeten worden goedgekeurd door de eigenaar van de groep. Wanneer een gebruiker een aanvraag om deel te nemen aan een groep maakt, wordt de join-aanvraag wordt doorgestuurd naar de eigenaren van de groep. Als een van de eigenaren van de aanvraag goedkeurt, wordt de gebruiker is een melding en wordt de gebruiker die is gekoppeld aan de groep. Als een van de eigenaren de aanvraag weigert, wordt de gebruiker is een melding ontvangen, maar geen deel uitmaakt van de groep.


## <a name="getting-started-with-access-management"></a>Aan de slag met toegangsbeheer
Klaar om aan de slag? U moet Probeer enkele van de basistaken die u met Azure AD-groepen doen kunt. Gebruik deze mogelijkheden gespecialiseerde toegang geven tot verschillende groepen mensen voor verschillende resources in uw organisatie. Hieronder ziet u een lijst met algemene eerste stappen.

* [Een eenvoudige regel om te configureren dynamische lidmaatschappen voor een groep maken](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)

* [Een groep gebruiken voor het beheren van toegang tot SaaS-toepassingen](active-directory-accessmanagement-group-saasapps.md)

* [Een groep maken beschikbaar voor eindgebruikers Self-service](active-directory-accessmanagement-self-service-group-management.md)

* [Synchronisatie van een on-premises implementatie-groep Azure met Azure AD Connect](active-directory-aadconnect.md)

* [Eigenaren voor een groep beheren](active-directory-accessmanagement-managing-group-owners.md)


## <a name="next-steps-for-access-management"></a>Volgende stappen om toegang te beheren
Nu u de basisbeginselen van toegangsbeheer hebt begrepen, hier zijn enkele extra geavanceerde mogelijkheden beschikbaar in Azure Active Directory voor toegang tot uw toepassingen en bronnen beheren.

* [Geavanceerde regels maken met behulp van kenmerken](active-directory-accessmanagement-groups-with-advanced-rules.md)

* [Beveiligingsgroepen in Azure AD beheren](active-directory-accessmanagement-manage-groups.md)

* [Speciale groepen in Azure AD instellen](active-directory-accessmanagement-dedicated-groups.md)

* [Overzicht van de grafiek API van groepen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)

* [Azure Active Directory-cmdlets voor het configureren van de groepsinstellingen](active-directory-accessmanagement-groups-settings-cmdlets.md)
