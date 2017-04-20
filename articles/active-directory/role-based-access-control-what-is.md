<properties
    pageTitle="Rolgebaseerd toegangsbeheer | Microsoft Azure"
    description="In access management aan de slag met Azure Rolgebaseerd toegangsbeheer, in de Portal Azure. Gebruik roltoewijzingen machtigingen in uw adreslijst toewijzen."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="get-started-with-access-management-in-the-azure-portal"></a>Aan de slag met toegangsbeheer in de portal van Azure

Beveiliging-georiënteerd bedrijven moeten richten op uw werknemers de exacte machtigingen die ze nodig hebben. Te veel machtigingen beschrijft een account aan hackers. Te weinig machtigingen betekent dat hun werk efficiënt is werknemers niet gevonden. Azure Rolgebaseerd Access besturingselement RBAC () kunt dit probleem oplossen door te bieden fijnmazige toegangsbeheer voor Azure.

RBAC gebruikt, kunt u scheiden van taken in uw team en alleen de hoeveelheid toegang verlenen aan gebruikers die ze nodig hebben om uit te voeren hun taken. Onbeperkte machtigingen in plaats van zodat iedereen in uw Azure-abonnement of resources, kunt u alleen bepaalde acties toestaan. Gebruik bijvoorbeeld RBAC als één werknemer beheren virtuele machines in een abonnement, terwijl een andere SQL-databases binnen hetzelfde abonnement kunt beheren.

## <a name="basics-of-access-management-in-azure"></a>Basisprincipes van toegangsbeheer in Azure wordt aangegeven
Elk Azure abonnement is gekoppeld aan één map van de Azure Active Directory (AD). Gebruikers, groepen en toepassingen van die directory kunnen bronnen in de Azure abonnement beheren. De volgende toegangsrechten met de Azure-portal, Azure opdrachtregel hulpmiddelen en API's voor Azure toewijzen.

Toegang verlenen door de juiste RBAC rol toewijzen aan gebruikers, groepen en toepassingen op een bepaald bereik. Het bereik van een roltoewijzing is een abonnement, een resourcegroep of één resource. Een rol toegewezen aan een bovenliggende bereik verleent ook toegang tot de onderliggende items daarin. Een gebruiker met toegang tot een resourcegroep kunt bijvoorbeeld alle resources die deze, zoals websites, virtuele machines en subnetten bevat beheren.

![Relatie tussen de elementen van de Azure Active Directory - diagram](./media/role-based-access-control-what-is/rbac_aad.png)

De rol RBAC die u toewijst, bepaalt welke resources die de gebruiker, groep of toepassing in dat bereik beheren kan.

## <a name="built-in-roles"></a>Ingebouwde rollen
Azure RBAC heeft drie eenvoudige rollen die van toepassing op alle typen:

- **Eigenaar** heeft volledige toegang tot alle bronnen, waaronder het recht te Gemachtigdentoegang aan anderen.
- **Inzender** kunt maken en beheren van alle typen Azure resources, maar kan geen toegang aan anderen verlenen.
- **Lezer** kunt bestaande Azure resources weergeven.

De rest van de rollen RBAC in Azure beheer van specifieke Azure resources toestaan. De rol Inzender VM kan bijvoorbeeld de gebruiker maken en beheren van virtuele machines. Deze heeft ze geen toegang geven tot het virtuele netwerk of het subnet waarmee de virtuele machine verbinding maakt.

[Ingebouwde rollen RBAC](role-based-access-built-in-roles.md) bevat de rollen die beschikbaar zijn in Azure wordt aangegeven. Dit geeft de bewerkingen en het bereik dat elke ingebouwde rol worden toegewezen aan gebruikers. Als u op zoek bent als u wilt uw eigen rollen voor nog meer controle definiëren, raadpleegt u het maken van [aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md).

## <a name="resource-hierarchy-and-access-inheritance"></a>De overname van de hiërarchie en de toegang van de resource
- Elk **abonnement** in Azure behoort slechts één map.
- Elke **resourcegroep** behoort slechts één abonnement.
- Elke **resource** behoort slechts één resourcegroep.

Access die u bij bovenliggende bereiken verlenen wordt aan onderliggende bereiken overgenomen. Bijvoorbeeld:

- U toewijzen de rol van de lezer aan een Azure AD-groep op het bereik van abonnement. De leden van deze groep kunnen elke resourcegroep en resource weergeven in het abonnement.
- U kunt de rol Inzender toewijzen aan een toepassing voor het bereik van de groep resource. Deze kunt resources van alle typen in die resourcegroep, maar geen andere resourcegroepen in het abonnement beheren.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC versus klassieke abonnement beheerders
Klassieke abonnement beheerders en co-beheerders hebben volledige toegang tot het Azure abonnement. Ze kunnen resources met behulp van de [Azure-portal](https://portal.azure.com) met Azure resourcemanager-API's of het klassieke implementatie [Azure klassieke portal](https://manage.windowsazure.com) en Azure model beheren. In het model RBAC klassieke beheerders de rol van eigenaar op het bereik abonnement toegewezen.

Ondersteuning van Azure RBAC alleen de Azure-portal en de nieuwe Azure resourcemanager-API's. Gebruikers en toepassingen die zijn toegewezen RBAC rollen de klassieke beheerportal en het model Azure klassieke-implementatie niet gebruiken.

## <a name="authorization-for-management-vs-data-operations"></a>Autorisatie voor management versus gegevensbewerkingen
Azure RBAC biedt alleen ondersteuning voor beheertaken uit te voeren van de Azure bronnen in de portal van Azure en Azure resourcemanager-API's. Dit kan niet toestaan dat alle gegevens niveau bewerkingen voor Azure resources. Bijvoorbeeld, kunt u machtigen iemand voor het beheren van opslag-Accounts, maar niet aan de BLOB's of de tabellen in een opslag-Account niet. Daarnaast kan een SQL-database worden beheerd, maar niet de tabellen erin.

## <a name="next-steps"></a>Volgende stappen
- Aan de slag met [toegangsbeheer op basis van rollen in de portal van Azure](role-based-access-control-configure.md).
- Zie de [RBAC ingebouwde rollen](role-based-access-built-in-roles.md)
- Uw eigen [aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md) definiëren
