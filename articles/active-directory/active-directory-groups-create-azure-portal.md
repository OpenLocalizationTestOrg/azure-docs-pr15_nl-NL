<properties
    pageTitle="Een nieuwe groep maken in de proefversie van Azure Active Directory | Microsoft Azure"
    description="Hoe u een groep maken in Azure Active Directory en gebruikers (leden) toevoegen aan de groep"
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


# <a name="create-a-new-group-in-azure-active-directory-preview"></a>Een nieuwe groep maken in de proefversie van Azure Active Directory

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-groups-create-azure-portal.md)
- [Azure klassieke portal](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

In dit artikel wordt uitgelegd hoe maken en een nieuwe groep in de voorbeeldversie Azure Active Directory (Azure AD) te vullen. [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md) Gebruik een groep beheertaken zoals het toewijzen van licenties of machtigingen voor een aantal gebruikers of apparaten in één keer uitvoeren.

## <a name="how-do-i-create-a-group"></a>Hoe maak ik een groep?

1. Meld u aan bij de [portal van Azure](https://portal.azure.com) met een account dat is een globale beheerder voor de adreslijst.

2. **Meer services**selecteren, voert u **gebruikers en groepen** in het tekstvak en druk vervolgens op **Enter**.

  ![Gebruikersbeheer openen](./media/active-directory-groups-create-azure-portal/search-user-management.png)

3. Selecteer **alle groepen**op het blad **gebruikers en groepen** .

  ![Het blad groepen openen](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)

4. Klik op het blad **gebruikers en groepen: alle groepen** , selecteer de opdracht **toevoegen** .

  ![De opdracht toevoegen selecteren](./media/active-directory-groups-create-azure-portal/add-group-command.png)

5. Klik op het blad **groep** toevoegen een naam en beschrijving voor de groep.

6. Als u wilt selecteren leden die u wilt toevoegen aan de groep, **toegewezen** Selecteer in het vak **lidmaatschap type** en selecteer **leden**. Zie [werken met-kenmerken naar geavanceerde regels voor groepslidmaatschap maken](active-directory-groups-dynamic-membership-azure-portal.md)voor meer informatie over het beheren van het lidmaatschap van een groep dynamisch.

  ![Leden die u wilt toevoegen selecteren](./media/active-directory-groups-create-azure-portal/select-members.png)

5. Klik op het blad **leden** , selecteer een of meer gebruikers of apparaten toevoegen aan de groep en selecteer de knop **selecteert u** onder aan het blad aan de groep wilt toevoegen. Het vak **gebruiker** filters de weergave op basis van overeenkomen met uw invoer naar een deel van de naam van een gebruiker of apparaat. Geen jokertekens worden geaccepteerd in het dialoogvenster.

6. Wanneer u klaar bent met het toevoegen van leden aan de groep, selecteert u **maken** op het blad **groep** .    

  ![Groep bevestiging maken](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)




## <a name="additional-information"></a>Aanvullende informatie

Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Zie bestaande groepen](active-directory-groups-view-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](active-directory-groups-members-azure-portal.md)
* [Lidmaatschappen van een groep beheren](active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](active-directory-groups-dynamic-membership-azure-portal.md)
