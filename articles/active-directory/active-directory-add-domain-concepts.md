<properties
    pageTitle="Overzicht van aangepaste domeinnamen in Azure Active Directory | Microsoft Azure"
    description="Dit artikel wordt uitgelegd de conceptueel kader voor het gebruik van aangepaste domeinnamen in Azure Active directory, inclusief Federatie voor eenmalige aanmelding"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Overzicht van aangepaste domeinnamen in Azure Active Directory

Een domeinnaam is een belangrijk onderdeel van de identificatie voor veel directory-resources: dit gedeelte van een gebruiker naam of het e-mailadres voor een gebruiker, onderdeel van het adres van een groep is en kan deel uitmaken van de app-ID-URI voor een toepassing. Een resource in Azure Active Directory (Azure AD) kunt opnemen van een domeinnaam die al is geverifieerd voor eigenaar zijn van de map waarin u de resource. Alleen een hoofdbeheerder kunt domain management taken uitvoeren in Azure AD.

Domeinnamen in Azure AD zijn globaal uniek. Een domeinnaam kan worden gebruikt door een enkel Azure AD. Als één Azure AD-map heeft een domeinnaam gecontroleerd, kan klikt u vervolgens geen andere Azure AD-map verifiëren of die dezelfde domeinnaam.

## <a name="initial-and-custom-domain-names"></a>Eerste en aangepaste domeinnamen

Elke domeinnaam in Azure AD is de naam van een eerste domein, of een aangepaste domeinnaam.

Elke Azure AD wordt geleverd met de naam van een eerste domein in het formulier contoso.onmicrosoft.com. Deze derde niveau domeinnaam, in dit voorbeeld "contoso.onmicrosoft.com," is ingesteld wanneer de map is gemaakt, meestal door de beheerder van wie de map hebt gemaakt. De eerste domeinnaam voor een map kan niet worden gewijzigd of verwijderd. De eerste domeinnaam, is terwijl volledig functioneel, bedoeld hoofdzakelijk moet worden gebruikt als bootstrapping om totdat de naam van een aangepast domein is geverifieerd.

In de meeste productieomgevingen, een map heeft ten minste één gecontroleerde aangepaste domein, zoals "contoso.com," en dat aangepaste domein die zichtbaar is voor eindgebruikers is. Een aangepaste domeinnaam is een domeinnaam die is eigendom en gebruikt door de organisatie, zoals "contoso.com," voor toepassingen zoals het beheren van de website. Deze domeinnaam is voor werknemers, omdat het deel van de naam van de gebruiker waarmee ze te melden bij het bedrijfsnetwerk bevinden, of verzenden en e-mail op te halen.

Voordat deze kan worden gebruikt door Azure AD, moet de naam van het aangepaste domein worden toegevoegd aan uw adreslijst en geverifieerd.

## <a name="verified-and-unverified-domain-names"></a>Gecontroleerde en niet-geverifieerde domeinnamen

De eerste domeinnaam voor een map impliciet geëvalueerd als gecontroleerd door Azure AD. Wanneer een aangepaste domeinnaam beheerder aan een Azure AD toevoegt, is in eerste instantie in een niet-geverifieerde staat. Azure AD, kan geen directory-bronnen een niet-geverifieerde domeinnaam wordt gebruikt. Dit zorgt ervoor dat slechts één directory de beschikking over een bepaalde domeinnaam en dat de domeinnaam wordt gebruikt door de organisatie daadwerkelijk eigenaar is van die domeinnaam.

Azure AD controleert eigendom van een domeinnaam door te zoeken naar een bepaald item in het zonebestand domein naam service (DNS) voor de domeinnaam. Om te controleren of de eigenaar bent van een domeinnaam, krijgt een beheerder van de DNS-vermelding van Azure AD Azure AD zoekt, en die gegevens toegevoegd aan de DNS-zone-bestand voor de domeinnaam. De DNS-zonebestand wordt beheerd door de domeinnaamregistrar voor dat domein. De stappen om te controleren of een domein worden weergegeven in het artikel voor het [toevoegen van een aangepast domein met uw Azure AD-directory](active-directory-add-domain.md).

Een DNS-vermelding toevoegen aan het zonebestand voor de domeinnaam, heeft dit geen invloed op andere domeinservices zoals e-mail of webhosting.

## <a name="federated-and-managed-domain-names"></a>Federatieve en beheerde domeinnamen

Een aangepaste domeinnaam in Azure AD kan worden geconfigureerd voor gebruikers van een federatieve (+) in ervaring tussen uw lokale Active Directory en Azure AD geven. Een domein voor federatie configureren, moet updates aan bevoegdheden resources in Azure AD en ook in uw Windows Server Active Directory. Het configureren van dat een gefedereerd domein moet worden uitgevoerd vanuit Azure AD Connect of via PowerShell. Een aangepast domein samenbrengen kan niet worden geïnitieerd vanaf de portal van Azure klassieke. [Bekijk deze video voor meer informatie over het configureren van AD FS voor gebruiker meld u aan met Azure AD Connect](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Domeinen die niet zijn gekoppeld, worden wel beheerde domeinen. Het oorspronkelijke domein voor een Azure AD-map wordt impliciet geëvalueerd als een beheerde domein.

## <a name="primary-domain-names"></a>Primaire domeinnamen

De primaire domeinnaam voor een map is de domeinnaam die vooraf is geselecteerd als de standaardwaarde voor het gedeelte 'domein' van de gebruikersnaam in te voeren, wanneer een beheerder een nieuwe gebruiker in de [portal van Azure klassieke](https://manage.windowsazure.com/) of een ander portal zoals de Office 365-beheerportal maakt. Een map kan slechts één primaire domeinnaam hebben. Een beheerder kan de primaire domeinnaam moet een aangepast domein dat niet is gekoppeld, geverifieerd wijzigen of op het oorspronkelijke domein.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Domeinnamen in Azure AD en andere Microsoft Online Services

Een domeinnaam moet worden geverifieerd in Azure AD voordat deze kan worden gebruikt door een andere Microsoft-onlineservice zoals Exchange Online, SharePoint Online en Intune. De volgende services vereisen meestal een beheerder een of meer DNS entries die specifiek voor de service zijn toevoegen.

Een Azure WebApp wordt gebruikt dat een eigen om te verifiëren van een domein. Een domein moet worden geverifieerd voor gebruik met Azure AD, zelfs als deze is eerder geverifieerd voor gebruik door een Azure web-app in een abonnement dat is gebaseerd op die Azure AD. Een Azure web-app de beschikking over een domeinnaam die is vastgesteld in een andere map uit de map die de web-app beveiligt.

## <a name="managing-domain-names"></a>Domeinnamen beheren

Beheertaken voor het domein kunnen worden voltooid vanaf de portal van Azure klassieke en vanaf PowerShell. Veel taken kunnen met de API Azure AD-grafiek (in openbare preview) worden ingevuld.

-   [Toevoegen en verifiëren van een aangepaste domeinnaam](active-directory-add-domain.md)

-   [Beheren van domeinen in de klassieke Azure-portal](active-directory-add-manage-domain-names.md)

-   [PowerShell gebruiken voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [De grafiek van Azure AD-API gebruiken voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)
