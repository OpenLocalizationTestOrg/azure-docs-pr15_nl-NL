<properties
    pageTitle="Gebruiker aanmeldingen voor een enterprise-app in de proefversie van Azure Active Directory uitschakelen | Microsoft Azure"
    description="Het uitschakelen van een bedrijfstoepassing zodat niemand bij deze in Azure Active Directory aanmelden kunnen"
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
    ms.date="10/17/2016"
    ms.author="curtand"/>


# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory-preview"></a>Gebruiker aanmeldingen voor een enterprise-app in de proefversie van Azure Active Directory uitschakelen

Het is eenvoudig een bedrijfstoepassing uitschakelen zodat niemand bij deze in de proefversie van Azure Active Directory (Azure AD aanmelden kunnen). [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md) U moet de juiste machtigingen voor het beheren van de enterprise-app. Klik in het huidige voorbeeld moet u de globale beheerder voor de map.

## <a name="how-do-i-disable-user-sign-ins"></a>Hoe kan ik gebruiker aanmeldingen uitschakelen?

1. Meld u aan bij de [portal van Azure](https://portal.azure.com) met een account dat is een globale beheerder voor de adreslijst.

2. Selecteer **meer services**, **Azure Active Directory** invoeren in het tekstvak en druk vervolgens op **Enter**.

3. Klik op de * *Azure Active Directory - *directoryname* ** blade (dat wil zeggen het Azure AD blad voor de map die u beheert), selecteer **Enterprise toepassingen **.

    ![Bedrijfs-apps openen](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)

4. Selecteer op het blad **bedrijfstoepassingen** **alle toepassingen**. U ziet een lijst van de apps die u kunt beheren.

5. Selecteer een app op het blad **bedrijfstoepassingen - alle toepassingen** .

6. Klik op het blad ***toepassingsnaam*** (dat wil zeggen het blad met de naam van de geselecteerde-app in de titel), selecteert u **Eigenschappen**.

    ![De opdracht Alles toepassingen selecteren](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)

7. Klik op de ***toepassingsnaam*** **-Eigenschappen** blade, selecteer **niet** voor **ingeschakeld voor gebruikers moeten aanmelden?**.

8. Selecteer de opdracht **Opslaan** .

## <a name="next-steps"></a>Volgende stappen

- [Mijn groepen bekijken](active-directory-groups-view-azure-portal.md)
- [Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)
- [De toewijzing van een gebruiker of groep verwijdert uit een enterprise-app](active-directory-coreapps-remove-assignment-azure-portal.md)
- [De naam of het logo van een enterprise-app wijzigen](active-directory-coreapps-change-app-logo-user-azure-portal.md)
