<properties
    pageTitle="Gebruikers toevoegen vanuit andere mappen of bedrijven in de proefversie van Azure Active Directory van partner | Microsoft Azure"
    description="Dit artikel wordt uitgelegd hoe u gebruikers toevoegen of wijzigen van de gebruikersinformatie in Azure Active Directory, met inbegrip van Gast en externe gebruikers."
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

# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory-preview"></a>Gebruikers toevoegen vanuit andere mappen of partnerbedrijven in de proefversie van Azure Active Directory

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-users-create-external-azure-portal.md)
- [Azure klassieke portal](active-directory-create-users-external.md)

In dit artikel wordt uitgelegd hoe u gebruikers toevoegt uit andere mappen in de proefversie van Azure Active Directory (Azure AD) of van partnerbedrijven. [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md) Voor informatie over het toevoegen van nieuwe gebruikers in uw organisatie en het toevoegen van gebruikers die Microsoft-accounts hebt, raadpleegt u [de nieuwe gebruikers toevoegen met Azure Active Directory](active-directory-users-create-azure-portal.md). Toegevoegde gebruikers geen beheerdersmachtigingen al dan niet standaard, maar u kunt rollen toewijzen aan deze op elk gewenst moment.

## <a name="add-a-user"></a>Een gebruiker toevoegen

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com) met een account dat is een globale beheerder voor de adreslijst.

2.  **Meer services**selecteren, voert u **gebruikers en groepen** in het tekstvak en druk vervolgens op **Enter**.

    ![Gebruikersbeheer openen](./media/active-directory-users-create-external-azure-portal/create-users-user-management.png)

3.  Klik op het blad **gebruikers en groepen** , selecteer **gebruikers**en selecteer vervolgens **toevoegen**.

    ![De opdracht toevoegen selecteren](./media/active-directory-users-create-external-azure-portal/create-users-add-command.png)

4. Geef op het blad **gebruiker** een weergavenaam in het vak **naam** en naam van de gebruiker aanmelden in **gebruikersnaam in te voeren**.

5. Kopieer of de gegenereerde gebruikerswachtwoord anders te noteren zodat u deze informatie aan de gebruiker verstrekt kunt nadat deze voltooid is.

6. (Optioneel) Selecteer **profiel** moet worden eerst de gebruikers toevoegen en achternaam, een functie en de naam van een afdeling.
    
    ![Het gebruikersprofiel openen](./media/active-directory-users-create-external-azure-portal/create-users-user-profile.png)

    - Selecteer **groepen** de gebruiker toevoegen aan een of meer groepen.

        ![Een gebruiker toevoegen aan groepen](./media/active-directory-users-create-external-azure-portal/create-users-user-groups.png)

    - Selecteer **organisatorische rol** toewijzen aan de gebruiker aan een rol in de lijst **rollen** . Zie [beheerdersrollen toewijzen in Azure AD](active-directory-assign-admin-roles.md)voor meer informatie over rollen voor gebruiker en de beheerder.

        ![Een gebruiker toewijzen aan een rol](./media/active-directory-users-create-external-azure-portal/create-users-assign-role.png)

7. Selecteer **maken**.

8. Het gegenereerde wachtwoord voor de nieuwe gebruiker veilig distribueren zodat de gebruiker zich kan aanmelden.

> [AZURE.IMPORTANT] Als uw organisatie meer dan één domein gebruikt, moet u weten over de volgende problemen wanneer u een gebruikersaccount toevoegen:
>
> - Als u wilt toevoegen van gebruikersaccounts met de dezelfde UPN (User Principal Name) voor domeinen, **eerste** hebt toegevoegd, bijvoorbeeld geoffgrisso@contoso.onmicrosoft.com, **gevolgd door** geoffgrisso@contoso.com.
> - **Geen** toevoegen geoffgrisso@contoso.com voordat u toevoegt geoffgrisso@contoso.onmicrosoft.com. Deze volgorde is belangrijk en lastig om ongedaan te maken.

Als u gegevens voor een gebruiker wiens identiteit wordt gesynchroniseerd met uw lokale Active Directory-service wijzigt, kunt u de gegevens van de gebruiker in de portal van Azure klassieke niet wijzigen. Als u wilt wijzigen van gegevens van de gebruiker, door uw lokale Active Directory beheerprogramma's te gebruiken.


## <a name="whats-next"></a>Volgende stappen

- [Een gebruiker toevoegen](active-directory-users-create-azure-portal.md)
- [Een gebruikerswachtwoord in de nieuwe portal van Azure opnieuw](active-directory-users-reset-password-azure-portal.md)
- [Een gebruiker toewijzen aan een rol in uw Azure AD](active-directory-users-assign-role-azure-portal.md)
- [Werkgegevens van een gebruiker wijzigen](active-directory-users-work-info-azure-portal.md)
- [Gebruikersprofielen beheren](active-directory-users-profile-azure-portal.md)
- [Een gebruiker in uw Azure AD verwijderen](active-directory-users-delete-user-azure-portal.md)
