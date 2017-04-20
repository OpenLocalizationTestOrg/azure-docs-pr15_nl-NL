<properties
    pageTitle="Aangepaste domeinnamen in uw Azure Active Directory beheren | Microsoft Azure"
    description="Basisbeginselen over het beheer en uitleg over het beheren van een aangepast domein in Azure Active Directory"
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

# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Aangepaste domeinnamen in uw Azure Active Directory beheren

Een domeinnaam is een belangrijk onderdeel van de identificatie voor veel directory-resources: dit gedeelte van een gebruiker naam of het e-mailadres voor een gebruiker, onderdeel van het adres van een groep is en kan deel uitmaken van de app-ID-URI voor een toepassing. Een resource in Azure Active Directory (Azure AD) kunt opnemen van een domeinnaam die al is geverifieerd voor eigenaar zijn van de map waarin u de resource. Alleen een hoofdbeheerder kunt domain management taken uitvoeren in Azure AD.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>De primaire domeinnaam voor uw Azure AD-map instellen

Wanneer uw adreslijst is gemaakt, is de eerste domeinnaam, zoals 'contoso.onmicrosoft.com', ook de primaire domeinnaam voor uw adreslijst. De primaire sleutel domein is de standaarddomeinnaam voor een nieuwe gebruiker wanneer u een nieuwe gebruiker in de [portal van Azure klassieke](https://manage.windowsazure.com/)of andere portals zoals de Office 365-beheerportal maken. Dit stroomlijnt het proces voor een beheerder om te maken van nieuwe gebruikers in de portal.

De primaire domeinnaam voor uw map wijzigen:

1.  Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com/) met een gebruikersaccount waarmee een globale beheerder van de Azure AD-map.

2.  Selecteer **Active Directory** in de linkernavigatiebalk.

3.  Open de map.

4.  Selecteer het tabblad **domeinen** .

5.  Selecteer de knop **primaire wijzigen** op de opdrachtbalk.

6.  Selecteer het domein dat u wilt de nieuwe hoofddomein voor uw adreslijst.

U kunt de primaire domeinnaam voor uw adreslijst moeten alle geverifieerde aangepast domein dat niet is gekoppeld. Wijzigen van het hoofddomein voor uw adreslijst, verandert niet de gebruikersnamen voor bestaande gebruikers.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Aangepaste domeinnamen toevoegen aan uw Azure AD

U kunt maximaal 900 aangepaste domeinnamen toevoegen aan elke Azure AD-map. Het proces voor het [toevoegen van een andere aangepaste domeinnaam](active-directory-add-domain.md) is dezelfde voor de eerste aangepaste domeinnaam.

## <a name="add-subdomains-of-a-custom-domain"></a>Subdomeinen van een aangepast domein toevoegen

Als u een derde niveau domeinnaam, zoals 'europe.contoso.com' toevoegen aan uw adreslijst wilt, moet u eerst toevoegen en controleer of het tweede niveau-domein, zoals contoso.com. Het subdomein wordt automatisch door Azure AD worden gecontroleerd. Als u wilt zien dat het subdomein dat u zojuist hebt toegevoegd is geverifieerd, vernieuwt u de pagina in de browser die de domeinen in uw adreslijst.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Wat u moet doen als u de DNS-domeinregistrar voor uw aangepaste domeinnaam wijzigt

Als u de DNS-domeinregistrar voor uw aangepaste domeinnaam wijzigt, kunt u blijven uw aangepaste domeinnaam gebruiken met Azure AD zelf zonder onderbreking en zonder extra configuratietaken uit te voeren. Als u uw aangepaste domeinnaam met Office 365, Intune of andere services die afhankelijk van aangepaste domeinnamen in Azure AD gebruiken zijn, raadpleegt u de documentatie van deze services.

## <a name="delete-a-custom-domain-name"></a>De naam van een aangepast domein verwijderen

Als uw organisatie niet meer die domeinnaam gebruikt, of als u wilt gebruiken die domeinnaam met een ander Azure AD, kunt u een aangepaste domeinnaam verwijderen uit uw Azure AD.

Als u wilt verwijderen van een aangepaste domeinnaam, moet u eerst ervoor zorgen dat geen resources in uw adreslijst, afhankelijk van de domeinnaam. U kunt de naam van een domein verwijderen uit uw adreslijst als:

-   Elke gebruiker heeft een gebruikersnaam in te voeren, e-mailadres of proxy-adres dat bevat de domeinnaam.

-   Een groep heeft een e-mailadres of proxyadres waarin de domeinnaam.

-   Een toepassing in uw Azure AD heeft een app-ID-URI waarin de domeinnaam.

U moet wijzigen of verwijderen van een dergelijke resource in uw adreslijst Azure AD voordat u de aangepaste domeinnaam kunt verwijderen.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>PowerShell gebruiken of Graph API voor het beheren van domeinnamen

De meeste beheertaken voor domeinnamen in Azure Active Directory kunnen ook worden uitgevoerd via Microsoft PowerShell of via een programma Azure AD Graph-API gebruiken (in openbare preview).

-   [PowerShell gebruiken voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Graph-API gebruiken voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Volgende stappen

-   [Meer informatie over domeinnamen in Azure AD](active-directory-add-domain-concepts.md)

-   [Aangepaste domeinnamen beheren](active-directory-add-manage-domain-names.md)
