<properties
    pageTitle="De toewijzing van een gebruiker of groep verwijdert uit een enterprise-app in de proefversie van Azure Active Directory | Microsoft Azure"
    description="Hoe u een access-toewijzing van een gebruiker of groep verwijdert uit een enterprise-app in Azure Active Directory"
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
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory-preview"></a>Een gebruiker of een toewijzing aan een groep verwijdert uit een enterprise-app in de proefversie van Azure Active Directory

Het is eenvoudig om te trekken voor een gebruiker of een groep toegang wordt toegewezen aan een van uw zakelijke toepassingen in de proefversie van Azure Active Directory (Azure AD). [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md) U moet de juiste machtigingen voor het beheren van de enterprise-app. Klik in het huidige voorbeeld moet u de globale beheerder voor de map.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Hoe verwijder ik een gebruiker of een toewijzing aan een groep?

1. Meld u aan bij de [portal van Azure](https://portal.azure.com) met een account dat is een globale beheerder voor de adreslijst.

2. Selecteer **meer services**, **Azure Active Directory** invoeren in het tekstvak en druk vervolgens op **Enter**.

3. Klik op de * *Azure Active Directory - *directoryname* ** blade (dat wil zeggen het Azure AD blad voor de map die u beheert), selecteer **Enterprise toepassingen **.

    ![Bedrijfs-apps openen](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)

4. Selecteer op het blad **bedrijfstoepassingen** **alle toepassingen**. Hier ziet u een overzicht van de apps die u kunt beheren.

5. Selecteer een app op het blad **bedrijfstoepassingen - alle toepassingen** .

6. Selecteer **gebruikers en groepen**op het blad ***toepassingsnaam*** (dat wil zeggen het blad met de naam van de geselecteerde-app in de titel).

    ![Gebruikers of groepen selecteren](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)

7. Klik op de ***toepassingsnaam*** **-gebruiker & toewijzing aan een groep** blade, selecteer een van de meer gebruikers of groepen en selecteer vervolgens de opdracht **verwijderen** . Bevestig dat u bij de prompt.

    ![De opdracht verwijderen te kiezen](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Volgende stappen

- [Zie al mijn groepen](active-directory-groups-view-azure-portal.md)
- [Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)
- [Gebruiker aanmeldingen voor een enterprise-app uitschakelen](active-directory-coreapps-disable-app-azure-portal.md)
- [De naam of het logo van een enterprise-app wijzigen](active-directory-coreapps-change-app-logo-user-azure-portal.md)
