<properties
   pageTitle="Het toevoegen of verwijderen van een gebruikersrol | Microsoft Azure"
   description="Informatie over het toevoegen van rollen aan bevoegdheden identiteiten Zorg dat de Azure Active Directory bevoegdheden identiteitsbeheer-toepassing."
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
   ms.date="10/24/2016"
   ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure AD bevoegdheden identiteitsbeheer: Hoe toevoegen of verwijderen van een gebruikersrol

Met Azure Active Directory (AD), kunt een globale beheerder (of bedrijfsbeheerder) bijwerken waarmee gebruikers **permanent** zijn toegewezen aan rollen in Azure AD zijn. Dit is gedaan met PowerShell-cmdlets zoals `Add-MsolRoleMember` en `Remove-MsolRoleMember`. Of ze kunnen de Azure klassieke portal gebruiken zoals beschreven in [het toewijzen van beheerdersrollen in Azure Active Directory](active-directory-assign-admin-roles.md).

De toepassing Azure AD bevoegdheden identiteitsbeheer beheerders bevoegdheden rol kunnen ook permanente roltoewijzingen maken. Bovendien kunnen bevoegdheden rol beheerders gebruikers **in aanmerking komen** voor beheerdersrollen aanbrengen. Een in aanmerking komend beheerder kan de rol activeren wanneer ze deze nodig hebt en vervolgens de machtigingen verlopen zodra ze klaar bent.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Rollen met PIM in de Azure-portal beheren

In uw organisatie, kunt u gebruikers toewijzen aan verschillende beheerderrollen in Azure AD, Office 365 en andere Microsoft-services en toepassingen.  Meer informatie over de beschikbare rollen kunnen worden gevonden op [rollen in Azure AD PIM](active-directory-privileged-identity-management-roles.md).

Geef het dashboard PIM, wilt toevoegen of verwijderen van een gebruiker in een rol met bevoegdheden identiteitsbeheer. Vervolgens klikt u op de knop **gebruikers in beheerdersrollen** of Selecteer een specifieke rol (zoals globale beheerder) in de tabel rollen.

> [AZURE.NOTE] Als u nog PIM in de portal van Azure nog niet hebt ingeschakeld, gaat u naar het [aan de slag met Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) voor meer informatie.

Als u een andere gebruikerstoegang geven tot PIM zelf wilt, zijn de rollen die PIM is vereist voor de gebruiker heeft beschreven verder in [hoe u toegang geven tot PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="add-a-user-to-a-role"></a>Een gebruiker toevoegen aan een rol

1. Selecteer de tegel **Azure AD bevoegdheden identiteitsbeheer** op het dashboard in de [portal van Azure](https://portal.azure.com/).
2. Selecteer **bevoegdheden rollen beheren**.
3. Selecteer de rol die u wilt beheren in de tabel **rol samenvatting** .
4. Selecteer in het blad rol **toevoegen**.
5. Klik op **Selecteer gebruikers** en zoeken voor de gebruiker op het blad **gebruikers selecteren** .  
6. Selecteer de gebruiker in de lijst met zoekresultaten en klik op **Gereed**.
4. Klik op **OK** als uw selectie wilt opslaan. De gebruiker die u hebt geselecteerd, wordt weergegeven in de lijst als in aanmerking komen voor de rol.

> [AZURE.NOTE]
>Nieuwe gebruikers met een rol zijn alleen in aanmerking komen voor de rol al dan niet standaard. Als u de rol permanent maken wilt, klikt u op de gebruiker in de lijst. Gegevens van de gebruiker wordt weergegeven in een nieuwe blade. Selecteer **macht maken** in het menu gebruiker informatie.  
>Als een gebruiker kan niet registreren voor Azure meervoudige verificatie MFA () of een Microsoft-account gebruikt (meestal @outlook.com), moet u permanent in alle hun rollen maken. In aanmerking komend beheerders gevraagd te registreren voor MFA tijdens de activering.

Nu dat de gebruiker bevindt zich in aanmerking voor een rol, laten weten dat ze om deze volgens de instructies in [het activeren of deactiveren van een rol activeren te](active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="remove-a-user-from-a-role"></a>Een gebruiker verwijderen uit een rol

U kunt gebruikers verwijderen van in aanmerking komend roltoewijzingen, maar zorg ervoor dat er is altijd ten minste één gebruiker die een permanente globale beheerder.

Volg deze stappen om een specifieke gebruiker van een rol verwijderen:

1. Navigeer naar de rol in de lijst functie door te selecteren van een rol in het dashboard Azure AD PIM of door te klikken op de knop **gebruikers in beheerdersrollen** .
2. Klik op de gebruiker in de lijst met gebruikers.
3. Klik op **verwijderen**. Een bericht wordt u gevraagd om te bevestigen.
4. Klik op **Ja** als u wilt verwijderen van de rol van de gebruiker.

Als u niet zeker weet welke gebruikers nog steeds hun roltoewijzingen nodig, klikt u vervolgens kunt u [een access-revisie voor de rol starten](active-directory-privileged-identity-management-how-to-start-security-review.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
