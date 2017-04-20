<properties
    pageTitle="Een gebruiker of groep toewijzen aan een enterprise-app in de proefversie van Azure Active Directory | Microsoft Azure"
    description="Het selecteren van een enterprise-app aan een gebruiker of groep aan toewijzen in Azure Active Directory"
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
    ms.date="10/03/2016"
    ms.author="curtand"/>

# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory-preview"></a>Een gebruiker of groep toewijzen aan een enterprise-app in de proefversie van Azure Active Directory

Het is eenvoudig een gebruiker of een groep toewijzen aan uw zakelijke toepassingen in de proefversie van Azure Active Directory (Azure AD). [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md) U moet de juiste machtigingen voor het beheren van de enterprise-app. Klik in het huidige voorbeeld moet u de globale beheerder voor de map.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Hoe ik de toegang van gebruikers toewijzen aan een enterprise-app?

1. Meld u aan bij de [portal van Azure](https://portal.azure.com) met een account dat is een globale beheerder voor de adreslijst.

2. Selecteer **meer services**, Azure Active Directory invoeren in het tekstvak en druk vervolgens op **Enter**.

3. Klik op de * *Azure Active Directory - *directoryname* ** blade (dat wil zeggen het Azure AD blad voor de map die u beheert), selecteer **Enterprise toepassingen **.

    ![Bedrijfs-apps openen](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)

4. Selecteer op het blad **bedrijfstoepassingen** **alle toepassingen**. Hier ziet u een overzicht van de apps die u kunt beheren.

5. Selecteer een app op het blad **bedrijfstoepassingen - alle toepassingen** .

6. Selecteer **gebruikers en groepen**op het blad ***toepassingsnaam*** (dat wil zeggen het blad met de naam van de geselecteerde-app in de titel).

    ![De opdracht Alles toepassingen selecteren](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)

7. Klik op de ***toepassingsnaam*** **-gebruiker & toewijzing aan een groep** blade, selecteert u de opdracht **toevoegen** .

8. Selecteer **gebruikers en groepen**op het blad **Toewijzing toevoegen** .

    ![Een gebruiker of groep toewijzen aan de app](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)

9. Klik op het blad **gebruikers en groepen** , selecteer een of meer gebruikers of groepen in de lijst en selecteer vervolgens de knop **selecteert u** onder aan het blad.

10. Selecteer de **rol**op het blad **Toewijzing toevoegen** . Selecteer een rol toe te passen op de geselecteerde gebruikers of groepen, klik op het blad **Rol selecteren** en selecteer vervolgens de knop **OK** onderaan in het blad.

11. Selecteer de knop **toewijzen** aan de onderkant van het blad op het blad **Toewijzing toevoegen** . De toegewezen gebruikers of groepen heeft de machtigingen die zijn gedefinieerd door de geselecteerde rol voor deze app enterprise.

## <a name="next-steps"></a>Volgende stappen

- [Zie al mijn groepen](active-directory-groups-view-azure-portal.md)
- [De toewijzing van een gebruiker of groep verwijdert uit een enterprise-app](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Gebruiker aanmeldingen voor een enterprise-app uitschakelen](active-directory-coreapps-disable-app-azure-portal.md)
- [De naam of het logo van een enterprise-app wijzigen](active-directory-coreapps-change-app-logo-user-azure-portal.md)
