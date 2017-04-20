<properties
    pageTitle="Nieuwe gebruikers toevoegen aan Azure Active Directory preview | Microsoft Azure"
    description="Dit artikel wordt uitgelegd hoe u nieuwe gebruikers toevoegen of wijzigen van gebruikersgegevens in Azure Active Directory."
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


# <a name="add-new-users-to-azure-active-directory-preview"></a>Nieuwe gebruikers toevoegen aan Azure Active Directory-voorbeeld

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-users-create-azure-portal.md)
- [Azure klassieke portal](active-directory-create-users.md)

In dit artikel wordt uitgelegd hoe u nieuwe gebruikers in uw organisatie in de voorbeeldversie Azure Active Direstory (Azure AD) toevoegen. [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md)

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com) met een account dat is een globale beheerder voor de adreslijst.

2.  **Meer services**selecteren, voert u **gebruikers en groepen** in het tekstvak en druk vervolgens op **Enter**.

    ![Gebruikersbeheer openen](./media/active-directory-users-create-azure-portal/create-users-user-management.png)

3.  Klik op het blad **gebruikers en groepen** , selecteer **alle gebruikers**en selecteer vervolgens **toevoegen**.

    ![De opdracht toevoegen selecteren](./media/active-directory-users-create-azure-portal/create-users-add-command.png)

4.  Voer gegevens voor de gebruiker, zoals **naam** en **gebruikersnaam in te voeren**. Het domein-gedeelte van de naam van de gebruiker moet zijn de eerste standaarddomeinnaam domein naam 'foo.onmicrosoft.com' of een domeinnaam geverifieerd, niet-federatief, zoals "contoso.com."

5. Kopieer of de gegenereerde gebruikerswachtwoord anders te noteren zodat u deze informatie aan de gebruiker verstrekt kunt nadat deze voltooid is.

6. U kunt desgewenst openen en vul de gegevens in het **profiel** blad, het blad **groepen** of het blad **Directory-rol** voor de gebruiker. Zie [beheerdersrollen toewijzen in Azure AD](active-directory-assign-admin-roles.md)voor meer informatie over rollen voor gebruiker en de beheerder.

7.  Selecteer op het blad **gebruiker** **maken**.

8. Het gegenereerde wachtwoord voor de nieuwe gebruiker veilig distribueren zodat de gebruiker zich kan aanmelden.

## <a name="whats-next"></a>Volgende stappen

- [Een externe gebruiker toevoegen](active-directory-users-create-external-azure-portal.md)
- [Een gebruikerswachtwoord in de nieuwe portal van Azure opnieuw](active-directory-users-reset-password-azure-portal.md)
- [Werkgegevens van een gebruiker wijzigen](active-directory-users-work-info-azure-portal.md)
- [Gebruikersprofielen beheren](active-directory-users-profile-azure-portal.md)
- [Een gebruiker in uw Azure AD verwijderen](active-directory-users-delete-user-azure-portal.md)
- [Een gebruiker toewijzen aan een rol in uw Azure AD](active-directory-users-assign-role-azure-portal.md)
