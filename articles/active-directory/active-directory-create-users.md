<properties
    pageTitle="Nieuwe gebruikers toevoegen aan Azure Active Directory | Microsoft Azure"
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
    ms.topic="get-started-article"
    ms.date="09/22/2016"
    ms.author="curtand"/>

# <a name="add-new-users--or-users-with-microsoft-accounts-to-azure-active-directory"></a>Nieuwe gebruikers of gebruikers met een Microsoft-account toevoegen aan Azure Active Directory

Gebruikers in te vullen uw adreslijst toevoegen. In dit artikel wordt uitgelegd hoe u nieuwe gebruikers in uw organisatie toevoegt en hoe u gebruikers met een Microsoft-account toevoegen. Zie voor meer informatie over het toevoegen van gebruikers in andere mappen in Azure Active Directory of toevoegen van gebruikers van partnerbedrijven [gebruikers toevoegen vanuit andere mappen of partnerbedrijven in Azure Active Directory](active-directory-create-users-external.md). Toegevoegde gebruikers geen beheerdersmachtigingen al dan niet standaard, maar u kunt rollen toewijzen aan deze op elk gewenst moment.

## <a name="add-a-user"></a>Een gebruiker toevoegen

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com) met een account dat is een globale beheerder voor de adreslijst.
2. Selecteer **Active Directory**en selecteer vervolgens de naam van de adreslijst van uw organisatie.
3. Selecteer het tabblad **gebruikers** en selecteer vervolgens in de opdrachtenbalk **Gebruiker toevoegen**.
4. Selecteer op de pagina **Vertel ons over deze gebruiker** onder **Type gebruiker**een van de volgende opties:

    - **Nieuwe gebruiker in uw organisatie** – voegt een nieuwe gebruikersaccount in uw adreslijst.
    - **Gebruiker met een bestaande Microsoft-account** : een bestaand account van de Microsoft-consumenten toevoegt aan uw adreslijst (bijvoorbeeld een Outlook-account)

5. Afhankelijk van het **Type gebruiker**, voert u een gebruikersnaam in te voeren (voor de nieuwe gebruiker) of een e-mailadres (voor een gebruiker met een Microsoft-account).
6. Geef een naam en achternaam, een beschrijvende naam en een gebruikersrol in de lijst **rollen** op de pagina **gebruikersprofiel** . Zie [beheerdersrollen toewijzen in Azure AD](active-directory-assign-admin-roles.md)voor meer informatie over rollen voor gebruiker en de beheerder. Opgeven of u wilt **Inschakelen meervoudige verificatie** voor de gebruiker.
7. Selecteer op de pagina **krijgen tijdelijke wachtwoord** **maken**.

> [AZURE.IMPORTANT] Als uw organisatie meer dan één domein gebruikt, moet u weten over de volgende problemen wanneer u een gebruikersaccount toevoegen:
>
> - Als u wilt toevoegen van gebruikersaccounts met de dezelfde UPN (User Principal Name) voor domeinen, **eerste** hebt toegevoegd, bijvoorbeeld geoffgrisso@contoso.onmicrosoft.com, **gevolgd door** geoffgrisso@contoso.com.
> - **Geen** toevoegen geoffgrisso@contoso.com voordat u toevoegt geoffgrisso@contoso.onmicrosoft.com. Deze volgorde is belangrijk en lastig om ongedaan te maken.

## <a name="change-user-information"></a>Gebruikersgegevens wijzigen

Kunt u een gebruikerskenmerk, behalve voor de object-ID.

1. Open de map.
2. Selecteer het tabblad **gebruikers** en selecteer vervolgens de weergavenaam van de gebruiker die u wilt wijzigen.
3. Breng uw wijzigingen aan en klik vervolgens op **Opslaan**.

Als de gebruiker die u wilt wijzigen, wordt gesynchroniseerd met uw lokale Active Directory-service, kunt u de gegevens van de gebruiker die met deze procedure niet wijzigen. Als u wilt wijzigen van de gebruiker, door uw lokale Active Directory beheerprogramma's te gebruiken.

## <a name="guest-user-management-and-limitations"></a>Gast Gebruikersbeheer en beperkingen

Gastaccounts zijn gebruikers uit andere mappen die zijn uitgenodigd voor uw adreslijst voor toegang tot SharePoint-documenten, -toepassingen of andere Azure resources. Een gastaccount in uw adreslijst heeft het onderliggende UserType kenmerk is ingesteld op 'Gast'. Gewone gebruikers (specifiek, de leden van uw adreslijst) hebben het kenmerk UserType "Lid."

Gasten hebben een beperkt aantal rights in de adreslijst. Deze rechten kunt u de mogelijkheid om gasten om informatie over andere gebruikers in de adreslijst beperken. Gastgebruikers kunnen echter nog steeds werken met de gebruikers en groepen die is gekoppeld aan de resources die ze werken. Gastgebruikers kunt doen:

- Zie andere gebruikers en groepen die is gekoppeld aan een Azure-abonnement waaraan ze zijn toegewezen
- Zie de leden van de groepen waartoe ze behoren
- Andere gebruikers in de adreslijst, opzoeken als ze het volledige e-mailadres van de gebruiker kennen
- Zie slechts een beperkt aantal kenmerken van de gebruikers die ze--beperkt opzoeken tot naam, e-mailadres, UPN (User Principal Name) en miniatuurfoto weergeven
- Een lijst met geverifieerde domeinen in de adreslijst
- Toestemming tot toepassingen, toekennen ze dezelfde toegang die leden in uw adreslijst hebben

## <a name="set-guest-user-access-policies"></a>Gebruikersbeleid voor access gast instellen

Het tabblad **configureren** van een map bevat opties voor toegangsbeheer voor gastgebruikers. Deze opties kunnen alleen in Azure klassieke portal worden gewijzigd door de globale beheerder van een map. Er is momenteel geen PowerShell of API-methode.

Als u wilt openen in het tabblad **configureren** in de portal van Azure klassieke, selecteer **Active Directory**en selecteer vervolgens de naam van de map.

![Tabblad configureren in Azure Active Directory][1]

Vervolgens kunt u de opties voor toegangsbeheer voor gastgebruikers bewerken.

![Opties voor toegangsbeheer voor gastgebruikers][2]


## <a name="whats-next"></a>Volgende stappen

- [Gebruikers toevoegen vanuit andere mappen of partnerbedrijven in Azure Active Directory](active-directory-create-users-external.md)
- [Azure AD beheren](active-directory-administer.md)
- [Wachtwoorden beheren in Azure AD](active-directory-manage-passwords.md)
- [Beheer van groepen in Azure AD](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
