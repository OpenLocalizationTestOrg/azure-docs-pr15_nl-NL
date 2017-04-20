<properties
    pageTitle="De groepen die uw groep een lid van in de proefversie van Azure Active Directory is beheren | Microsoft Azure"
    description="Groepen kunnen andere groepen in Azure Active Directory bevatten. Hier wordt uitgelegd hoe deze lidmaatschappen beheren."
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


# <a name="manage-the-groups-your-group-is-a-member-of-in-azure-active-directory-preview"></a>De groepen die uw groep een lid van in de proefversie van Azure Active Directory is beheren

Groepen kunnen andere ongewenste groepen in de proefversie van Azure Active Directory bevatten. [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md) Hier wordt uitgelegd hoe deze lidmaatschappen beheren.

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a>Hoe vind ik mijn groep een lid van is groepen?

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com) met een account dat is een globale beheerder voor de adreslijst.

2.  **Meer services**selecteren, voert u **gebruikers en groepen** in het tekstvak en druk vervolgens op **Enter**.

  ![Gebruikersbeheer openen](./media/active-directory-groups-membership-azure-portal/search-user-management.png)

3.  Selecteer **alle groepen**op het blad **gebruikers en groepen** .

  ![Het blad groepen openen](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)

4. Klik op het blad **gebruikers en groepen: alle groepen** , selecteer een groep.

5. Klik op de * *Group - *groepsnaam* ** blade, selecteer **groep lidmaatschappen **.

  ![Het groep lidmaatschappen blad openen](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Als u wilt toevoegen van uw groep als lid van een andere groep op het blad **groep - groepslidmaatschappen** , selecteert u de opdracht **toevoegen** .

7. Selecteer een groep in het blad **Groep selecteren** en selecteer vervolgens de knop **selecteert u** onder aan het blad. U kunt uw groep toevoegen aan slechts één groep tegelijk. Het vak **gebruiker** filters de weergave op basis van overeenkomen met uw invoer naar een deel van de naam van een gebruiker of apparaat. Geen jokertekens worden geaccepteerd in het dialoogvenster.

  ![Lid worden van een groep toevoegen](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)

8. Als u wilt verwijderen van uw groep als lid van een andere groep op het blad **groep - groepslidmaatschappen** , selecteer een groep.

9. Selecteer de opdracht **verwijderen** op het blad ***groepsnaam*** en Bevestig dat bij de prompt.

  ![lidmaatschap van de opdracht verwijderen](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)

9. Wanneer u klaar bent met het wijzigen van de groepslidmaatschap voor uw groep, selecteert u **Opslaan**.


## <a name="additional-information"></a>Aanvullende informatie

Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Zie bestaande groepen](active-directory-groups-view-azure-portal.md)
* [Een nieuwe groep maken en toevoegen van leden](active-directory-groups-create-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](active-directory-groups-members-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](active-directory-groups-dynamic-membership-azure-portal.md)
