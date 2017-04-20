<properties
   pageTitle="Hoe u toegang geven tot PIM | Microsoft Azure"
   description="Leer hoe u rollen voor gebruikers met de extensie Azure Active Directory bevoegdheden identiteitsbeheer toevoegt, zodat ze PIM kunnen beheren."
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
   ms.date="09/22/2016"
   ms.author="kgremban"/>

# <a name="how-to-give-access-to-manage-azure-ad-privileged-identity-management"></a>Hoe u toegang geven tot het beheren van Azure AD bevoegdheden identiteitsbeheer

De globale beheerder die automatisch Azure AD bevoegdheden identiteitsbeheer (PIM) voor een organisatie kunt krijgen roltoewijzingen en toegang tot PIM. Niemand anders krijgt schrijftoegang standaard, waaronder andere globale beheerders. Andere globale beheerders, Beveiligingsbeheerder en beveiliging lezers hebben alleen-lezen toegang tot Azure AD PIM. Als u wilt toegang geven tot PIM, kan de eerste gebruiker anderen toewijzen aan de **bevoegdheden rol** beheerdersrol. Deze toewijzing moet worden uitgevoerd in PIM zelf, en via PowerShell of andere portals kan niet worden gewijzigd.

> [AZURE.NOTE] Azure AD PIM beheren vereist Azure MFA. Aangezien Microsoft-accounts kunnen niet hebt geregistreerd voor Azure MFA, kan een gebruiker die zich aan met een Microsoft-account geen toegang tot Azure AD PIM.

Zorg ervoor dat er altijd ten minste twee gebruikers in een beheerdersrol bevoegdheden rol geval één gebruiker is vergrendeld of hun account wordt verwijderd.

## <a name="give-another-user-access-to-manage-pim"></a>Geef een andere gebruikerstoegang tot het beheren van PIM

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) en selecteer de app **Azure AD bevoegdheden identiteitsbeheer** op het dashboard.
2. Selecteer **bevoegdheden rollen beheren** > **bevoegdheden rol beheerder** > **toevoegen**.

    ![Bevoegdheden rol beheerders - schermafbeelding toevoegen][1]

4. Stap 1 is al voltooid op het blad van de beheerde gebruikers toevoegen. Selecteer in stap 2, **Selecteer gebruikers** en zoeken voor de gebruiker die u wilt toevoegen.

    ![Selecteer gebruikers - schermafbeelding][2]

6. Selecteer de gebruiker in de lijst met zoekresultaten en klik op **Gereed**.
7. Klik op **OK** als uw selectie wilt opslaan. De gebruiker die u hebt geselecteerd, wordt weergegeven in de lijst met bevoegdheden rol beheerders.

    - Wanneer u een nieuwe rol aan iemand toewijzen, worden ze automatisch ingesteld als in aanmerking komende te activeren van de rol. Als u ze in de rol permanent maken wilt, klikt u op de gebruiker in de lijst. Selecteer **macht maken** in het menu gebruiker informatie.

8. De gebruiker een koppeling naar [aan de slag met Azure AD bevoegdheden identiteitsbeheer](active-directory-privileged-identity-management-getting-started.md)verzenden.


## <a name="remove-another-users-access-rights-for-managing-pim"></a>Verwijderen van een andere gebruiker toegangsrechten voor het beheren van PIM

Voordat u iemand uit de beheerdersrol bevoegdheden rol verwijdert, altijd ervoor te zorgen er nog steeds worden twee gebruikers die zijn toegewezen.

1. Klik op de rol **bevoegdheden rol beheerder**in het dashboard PIM.  De lijst met gebruikers die momenteel in die rol verschijnt.
2. Klik op de gebruiker in de lijst met gebruikers.
3. Klik op **verwijderen**.  Er worden weergegeven met een bevestigingsbericht weergegeven.
4. Klik op **Ja** om de gebruiker verwijderen uit de rol.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
