<properties
   pageTitle="Rollen in PIM | Microsoft Azure"
   description="Informatie over welke functies worden gebruikt voor bevoegdheden identiteiten met de extensie bevoegdheden identiteitsbeheer Azure."
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
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="roles-in-azure-ad-privileged-identity-management"></a>Rollen in Azure AD bevoegdheden identiteitsbeheer

<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

U kunt toewijzen aan gebruikers in uw organisatie verschillende beheerderrollen in Azure AD. Deze roltoewijzingen bepalen welke taken, zoals toevoegen of verwijderen van gebruikers of wijzigen van service-instellingen, de gebruikers zijn uitvoeren op Azure AD, Office 365 en andere Microsoft Online Services en verbonden toepassingen.  

Een globale beheerder kan worden bijgewerkt waarmee gebruikers zijn **permanent** zijn toegewezen aan rollen in Azure AD, met PowerShell-cmdlets zoals `Add-MsolRoleMember` en `Remove-MsolRoleMember`, of via de klassieke-portal als beschreven in [het toewijzen van beheerdersrollen in Azure Active Directory](active-directory-assign-admin-roles.md).

Azure AD rechten identiteitsbeheer (PIM) beheert beleidsregels voor bevoegdheden toegang voor gebruikers in Azure AD. PIM wijst gebruikers toe aan een of meer rollen in Azure AD en kunt u iemand permanent in de rol, of in aanmerking komen voor de rol toewijzen. Wanneer een gebruiker definitief wordt toegewezen aan een rol of een roltoewijzing worden in aanmerking komend activeert en ze kunnen Azure Active Directory, Office 365 en andere toepassingen met de machtigingen die zijn toegewezen aan hun rollen beheren.

Er is geen verschil tussen de toegang die aan iemand met een permanente versus een in aanmerking komend roltoewijzing doorgegeven. De enige verschil is dat sommige personen niet nodig die toegang altijd hebt. Deze worden aangebracht in aanmerking komen voor de rol en kunnen inschakelen en uitschakelen wanneer ze moeten uitvoeren.

## <a name="roles-managed-in-pim"></a>Functies die worden beheerd in PIM

Bevoegdheden identiteitsbeheer kunt u gebruikers toewijzen aan algemene beheerdersrollen, waaronder:


- **Globale beheerder** (ook wel bekend als bedrijfsbeheerder) heeft toegang tot alle functies van de administratieve. U kunt meer dan één globale beheerder in uw organisatie hebben. De persoon die zich registreert kunt kopen van Office 365 automatisch verandert in een globale beheerder.
- **Bevoegdheden rol beheerder** Azure AD PIM beheert en roltoewijzingen voor andere gebruikers worden bijgewerkt.  
- **Facturering beheerder** schaft producten, beheert abonnementen, ondersteuningstickets, en bewaakt de servicestatus.
- **Wachtwoordbeheerder** wachtwoorden opnieuw instellen, beheert serviceaanvragen en bewaakt de servicestatus. Wachtwoordbeheerders zijn beperkt tot het opnieuw instellen van wachtwoorden voor gebruikers.
- **Service-beheerder** beheert serviceaanvragen en beeldschermen servicestatus.

  > [AZURE.NOTE] Als u Office 365 gebruikt, klikt u vervolgens voordat u de service-beheerdersrol toewijzen aan een gebruiker eerst de gebruiker administratieve machtigingen toewijzen aan een service, zoals Exchange Online.

- **Gebruikersbeheerder** wachtwoorden opnieuw instellen, bewaakt de servicestatus en beheer van gebruikersaccounts, gebruikersgroepen en serviceaanvragen. De beheerder gebruikerstoegang niet kunt verwijderen van een globale beheerder, andere beheerdersrollen maken of opnieuw instellen van wachtwoorden opnieuw instellen voor financiële medewerkers, hoofdbeheerders en servicebeheerders.
- **Exchange-beheerder** heeft beheerderstoegang tot Exchange Online via het Exchange-beheercentrum (EAC) en kan vrijwel alle taken uitvoeren in Exchange Online.
- **SharePoint-beheerder** heeft beheerderstoegang tot SharePoint Online via het SharePoint Online-beheercentrum en kan vrijwel alle taken in SharePoint Online uitvoeren.
- **Skype voor bedrijven-beheerder** heeft beheerderstoegang tot Skype voor bedrijven via het Skype voor bedrijven-beheercentrum en kan vrijwel alle taken uitvoeren in Skype voor bedrijven Online.

Lees de volgende artikelen voor meer informatie over het [toewijzen in Azure AD-beheerdersrollen](active-directory-assign-admin-roles.md) en [beheerdersrollen toewijzen in Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


Uit PIM kunt u [deze rollen aan een gebruiker toewijzen](active-directory-privileged-identity-management-how-to-add-role-to-user.md) zodat de gebruiker kan [de rol als noodzakelijk activeren](active-directory-privileged-identity-management-how-to-activate-role.md).

Als u gebruikerstoegang verlenen een andere om te beheren in PIM zelf wilt, zijn de rollen die PIM is vereist voor de gebruiker heeft beschreven verder in [hoe u toegang geven tot PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).


<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Functies die niet worden beheerd in PIM

Rollen in Exchange Online of SharePoint Online, behalve voor bovengenoemde, niet worden weergegeven in Azure AD en dus niet zichtbaar zijn in PIM. Voor meer informatie over het wijzigen van fijnmazige roltoewijzingen in deze Office 365-services, raadpleegt u [machtigingen in Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure-abonnementen en resourcegroepen ook niet worden weergegeven in Azure AD. Azure abonnementen beheren, raadpleegt u [het toevoegen of wijzigen van Azure-beheerdersrollen](../billing-add-change-azure-subscription-administrator.md) en Zie [Azure_Role-Based toegangsbeheer](role-based-access-control-configure.md)voor meer informatie over Azure RBAC.

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Gebruikersrollen en aanmelden
Voor sommige Microsoft-services en toepassingen, een gebruiker toewijzen aan een rol mogelijk niet voldoende zijn voor die gebruiker moet een beheerder.

Toegang tot de portal van Azure klassieke moet dat de gebruiker een service-beheerder of beheerder zijn als samen op een Azure-abonnement, zelfs als de gebruiker niet hoeft te beheren van de Azure abonnementen.  Bijvoorbeeld u configuratie-instellingen voor Azure AD in de klassieke-portal beheren door een gebruiker moet een globale beheerder in Azure AD zowel de collega beheerder van een abonnement op een Azure-abonnement.  Als u wilt weten hoe u gebruikers toevoegen aan Azure abonnementen, raadpleegt u [het toevoegen of wijzigen van Azure-beheerdersrollen](../billing-add-change-azure-subscription-administrator.md).

Toegang tot de Microsoft Online Services kan vragen om de gebruiker ook beschikken over een licentie voordat ze deze kunnen openen van de service-portal of beheerderstaken uitvoeren.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Een licentie toewijzen aan een gebruiker in Azure AD

1. Meld u aan bij de [Azure klassieke portal] (http://manage.windowsazure.com) met een account voor globale beheerder of een collega beheerdersaccount.
2. Selecteer **Alle Items** in het hoofdmenu.
3. Selecteer de map die u wilt werken en die is gekoppeld aan licenties heeft.
4. Selecteer **licenties**. De lijst met beschikbare licenties wordt weergegeven.
5. Selecteer het abonnement waarop licentie waarin de licenties die u wilt verspreiden.
6. Selecteer **toewijzen aan gebruikers**.
7. Selecteer de gebruiker die u wilt een licentie toewijzen.
8. Klik op de knop **toewijzen** .  De gebruiker kan nu aanmelden bij Azure.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
