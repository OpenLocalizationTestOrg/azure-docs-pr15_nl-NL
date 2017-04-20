<properties
    pageTitle="Rolgebaseerd toegangsbeheer gebruiken in de portal van Azure | Microsoft Azure"
    description="In access management aan de slag met toegangsbeheer op basis van rollen in de Portal Azure. Gebruik roltoewijzingen machtigingen toewijzen aan uw resources."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="use-role-assignments-to-manage-access-to-your-azure-subscription-resources"></a>Roltoewijzingen voor het beheren van toegang tot uw Azure abonnement resources gebruiken

> [AZURE.SELECTOR]
- [De toegang van door gebruiker of groep beheren](role-based-access-control-manage-assignments.md)
- [Toegang beheren door resource](role-based-access-control-configure.md)

Azure Rolgebaseerd Access besturingselement RBAC () kunt fijnmazige toegangsbeheer voor Azure. RBAC gebruikt, kunt u verlenen alleen het bedrag van access die gebruikers nodig hebben om uit te voeren hun taken. In dit artikel vindt u aan de slag met RBAC in de portal van Azure. Als u meer informatie over hoe RBAC helpt u bij het beheren van access wilt, raadpleegt u [Wat Rolgebaseerd toegangsbeheer is](role-based-access-control-what-is.md).

## <a name="view-access"></a>Toegang weergeven
U kunt zien wie toegang heeft tot een resource, resourcegroep of -abonnement van de belangrijkste blade in de [portal van Azure](https://portal.azure.com). We horen, bijvoorbeeld om te zien wie toegang heeft tot een van onze resourcegroepen:

1. Selecteer **resourcegroepen** in de navigatiebalk aan de linkerkant.  
    ![Resourcegroepen - pictogram](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Selecteer de naam van de resourcegroep in het blad **resourcegroepen** .
3. Selecteer **toegangsbeheer (IAM)** in het linkermenu.  
4. Het Access-besturingselement blad bevat alle gebruikers, groepen en toepassingen die toegang hebben gekregen aan de resourcegroep.  

    ![Gebruikers blade - overgenomen tegenover toegewezen access schermafbeelding](./media/role-based-access-control-configure/view-access.png)

Melding dat sommige gebruikers zijn **toegewezen** toegang terwijl de anderen **overgenomen** toe. Access wordt toegewezen specifiek aan de resourcegroep of overgenomen van een toewijzing naar het bovenliggende-abonnement.

> [AZURE.NOTE] Klassieke abonnement beheerders en co-beheerders worden beschouwd als eigenaren van het abonnement in het nieuwe RBAC-model.


## <a name="add-access"></a>Access toevoegen
U verlenen vanuit de resource, de resourcegroep of het abonnement dat het bereik van de toewijzing van rol.

1. Selecteer **toevoegen** op het blad Access-besturingselement.  
2. Selecteer de rol die u wilt toewijzen van het blad **selecteert u een rol** .
3. Selecteer de gebruiker, groep of een toepassing in uw adreslijst die u wilt verlenen van toegang tot. U kunt de map met weergegeven namen, e-mailadressen en object-id's zoeken.  

    ![Toevoegen van gebruikers blade: schermafbeelding zoeken](./media/role-based-access-control-configure/grant-access2.png)

4. Selecteer **OK** om de toewijzing. De **gebruiker toevoegen** die wordt weergegeven kunt u de voortgang bijhouden.  
    ![Gebruiker voortgangsbalk - schermafbeelding toevoegen](./media/role-based-access-control-configure/addinguser_popup.png)

Na het toevoegen van een roltoewijzing is, wordt deze weergegeven op het blad **gebruikers** .

## <a name="remove-access"></a>Verwijder toegang

1. Selecteer de roltoewijzing op het blad Access-besturingselement.
2. Selecteer **verwijderen** in het blad van toewijzing details.  
3. Selecteer **Ja** verwijderen te bevestigen.  
    ![Gebruikers blade - verwijderen uit de rol schermafbeelding](./media/role-based-access-control-configure/remove-access1.png)

Overgenomen toewijzingen kunnen niet worden verwijderd. In de onderstaande afbeelding ziet dat de knop verwijderen niet beschikbaar is. In plaats daarvan, kijkt u naar de details **Die zijn toegewezen aan** . Ga naar de resource die hier staan vermeld als u wilt verwijderen van de toewijzing van rol.

![Gebruikers blade - overgenomen access wordt uitgeschakeld verwijderen knop schermafbeelding](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>Andere hulpmiddelen voor het beheren van access
U kunt rollen toewijzen en beheren van toegang met Azure RBAC opdrachten in hulpmiddelen voor dan de Azure-portal.  Volg de koppelingen voor meer informatie over de vereisten en aan de slag met de opdrachten Azure RBAC.

- [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure opdrachtregel-Interface](role-based-access-control-manage-access-azure-cli.md)
- [REST API](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Volgende stappen
- [Een access-rapport wijzigen geschiedenis maken](role-based-access-control-access-change-history-report.md)
- Zie de [RBAC ingebouwde rollen](role-based-access-built-in-roles.md)
- Uw eigen [aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md) definiëren
