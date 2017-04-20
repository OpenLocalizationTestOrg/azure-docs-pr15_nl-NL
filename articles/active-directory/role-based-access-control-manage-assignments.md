<properties
    pageTitle="Toegang van Azure resourcetoewijzingen weergeven | Microsoft Azure"
    description="Weergeven en beheren van alle Rolgebaseerd toegangsbeheer toewijzingen voor een gebruiker of groep in de portal van Azure"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="jeffsta"/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal---public-preview"></a>Toewijzingen van de weergeven toegang voor gebruikers en groepen in de Azure portal - Public preview

> [AZURE.SELECTOR]
- [De toegang van door gebruiker of groep beheren](role-based-access-control-manage-assignments.md)
- [Toegang beheren door resource](role-based-access-control-configure.md)

Met Rolgebaseerd Access besturingselement RBAC () in de voorbeeldversie Azure Active Directory, kunt u toegang tot uw Azure bronnen beheren. [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md)

Access toegewezen met RBAC is fijnmazige omdat er zijn twee manieren kunt u de machtigingen beperken:

- **Bereik:** RBAC roltoewijzingen zijn beperkt tot een bepaald abonnement, resourcegroep of resource. Een gebruiker toegang krijgen tot één resource geen toegang tot andere bronnen in hetzelfde abonnement.
- **Rol:** In het bereik van de toewijzing, access wordt teruggebracht zelfs nog meer door het toewijzen van een rol. Rollen kunnen zijn op hoog niveau, zoals eigenaar of specifieke, zoals VM-lezer.

Rollen kunnen alleen worden toegewezen uit vanuit het abonnement, resourcegroep of resource die het bereik voor de toewijzing. Maar u kunt alle access-toewijzingen voor een bepaalde gebruiker of groep weergeven op één locatie.

Lees meer over het [Gebruik roltoewijzingen voor het beheren van toegang tot uw resources Azure-abonnement](role-based-access-control-configure.md).

##  <a name="view-access-assignments"></a>Access-toewijzingen weergeven

Als u de toewijzingen toegang voor een gebruiker of groep wilt opzoeken, start u in de Azure Active Directory in de [portal van Azure](http://portal.azure.com).

1. Selecteer **Azure Active Directory**. Als deze optie niet zichtbaar is op uw lijst navigatie is, selecteert u **Meer Services** en schuif omlaag naar **Azure Active Directory**.
2. Selecteer **gebruikers en groepen**en klik vervolgens op **alle gebruikers** of **alle groepen**. In dit voorbeeld richten we ons op individuele gebruikers.
    ![Gebruikers en groepen in Azure Active Directory - schermafbeelding beheren](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. De gebruiker zoeken op naam of de gebruikersnaam.
4. Selecteer **Azure resources** op het blad voor de gebruiker. Alle access-toewijzingen voor die gebruiker worden weergegeven.

### <a name="read-permissions-to-view-assignments"></a>Leesmachtigingen voor toewijzingen weergeven

Deze pagina bevat alleen de toewijzingen van access die u gemachtigd bent om te lezen. Zoals u leestoegang hebben tot abonnement A en Ga naar het blad Azure resources om te controleren van een gebruiker toewijzingen. U kunt de toewijzingen van haar access voor abonnement A, maar niet weergegeven dat zij ook toewijzingen van access op abonnement b te drukken heeft.

## <a name="delete-access-assignments"></a>Toewijzingen van access verwijderen

Uit deze blade, kunt u de toewijzingen van access die zijn toegewezen rechtstreeks aan een gebruiker of groep verwijderen. Als de access-toewijzing is overgenomen van een bovenliggende groep, moet u Ga naar de resource of een abonnement en beheren van de toewijzing er.

1. In de lijst met alle access-toewijzingen voor een gebruiker of groep, selecteert u de sectie die u wilt verwijderen.
2. Selecteer **verwijderen** en selecteer **Ja** om te bevestigen.
    ![Access-toewijzing - schermafbeelding verwijderen](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="related-topics"></a>Verwante onderwerpen

- Aan de slag met toegangsbeheer op basis van rollen voor [roltoewijzingen gebruiken voor het beheren van toegang tot uw resources Azure-abonnement](role-based-access-control-configure.md)
- Zie de [RBAC ingebouwde rollen](role-based-access-built-in-roles.md)
