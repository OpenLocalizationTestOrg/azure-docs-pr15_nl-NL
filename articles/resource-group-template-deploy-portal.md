<properties 
    pageTitle="Azure-portal gebruiken om te implementeren van Azure resources | Microsoft Azure" 
    description="Azure-portal en Azure Resource beheren gebruiken om te implementeren van uw resources." 
    services="azure-resource-manager,azure-portal" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Resources met resourcemanager sjablonen en Azure portal implementeren

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST API](resource-group-template-deploy-rest.md)

Dit onderwerp wordt uitgelegd hoe u de [Azure-portal](https://portal.azure.com) met [Azure resourcemanager](azure-resource-manager/resource-group-overview.md) om te implementeren van uw Azure resources. Zie voor meer informatie over het beheren van uw bronnen, [resources Azure beheren via de portal](./azure-portal/resource-group-portal.md).

Op dit moment ondersteunt niet elke service de portal of resourcemanager. Voor die services moet u de [klassieke portal](https://manage.windowsazure.com)gebruiken. Voor de status van elke service, raadpleegt u de [beschikbaarheid van Azure portal grafiek](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="create-resource-group"></a>Resourcegroep maken

1. Als u wilt een lege resourcegroep maken, selecteert u daarna **Nieuw** > **Management** > **Resourcegroep**.

    ![lege resourcegroep maken](./media/resource-group-template-deploy-portal/create-empty-group.png)

2. Geef deze een naam en locatie en selecteer indien nodig een abonnement. U moet een locatie voor de resourcegroep omdat de resourcegroep metagegevens over de resources worden opgeslagen. Voor naleving redenen wilt u mogelijk opgeven waarin die metagegevens is opgeslagen. In het algemeen, is het raadzaam dat u opgeeft dat een locatie waar de meeste uw resources zich bevinden. Het gebruik van dezelfde locatie, kan uw sjabloon vereenvoudigen.

    ![waarden instellen](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Resources uit Marketplace implementeren

Nadat u een resourcegroep hebt gemaakt, kunt u resources tot het implementeren van Marketplace. De Marketplace biedt vooraf gedefinieerde oplossingen voor veelvoorkomende scenario's.

1. Als u wilt een implementatie, selecteert u **Nieuw** en het type resource die u wilt implementeren. Kijk naar de bepaalde versie van de resource die u wilt implementeren.

    ![resource implementeren](./media/resource-group-template-deploy-portal/deploy-resource.png)

2. Als u de bepaalde oplossing die u wilt implementeren niet ziet, kunt u de Marketplace voor deze zoeken.

    ![zoeken marketplace](./media/resource-group-template-deploy-portal/search-resource.png)

3. Afhankelijk van het type geselecteerde resource hebt u een verzameling relevante eigenschappen instellen voor implementatie. Deze opties worden niet hier weergegeven als ze, af van het resourcetype hangen. Voor alle typen, moet u een resourcegroep bestemming. De volgende afbeelding ziet hoe u een WebApp maken en het dashboard implementeren naar de resourcegroep die u hebt gemaakt.

    ![resourcegroep maken](./media/resource-group-template-deploy-portal/select-existing-group.png)

    U kunt ook een resourcegroep maken wanneer uw resources implementeren. Selecteer **Nieuw** en geef een naam op voor de resourcegroep.

    ![nieuwe resourcegroep maken](./media/resource-group-template-deploy-portal/select-new-group.png)

4. Uw implementatie wordt gestart. De implementatie, kan een paar minuten duren. Als de implementatie is voltooid, ziet u een melding.

    ![weergave-melding](./media/resource-group-template-deploy-portal/view-notification.png)

5. Nadat u hebt uw resources wordt geïmplementeerd, kunt u meer resources toevoegen aan de resourcegroep met behulp van de opdracht **toevoegen** op het blad van de groep resource.

    ![resources toevoegen](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Resources uit de aangepaste sjabloon implementeren

Als u wilt uitvoeren van een distributie maar geen gebruik van de sjablonen in de Marketplace, kunt u een aangepaste sjabloon die u de infrastructuur voor uw oplossing definieert maken. Zie voor meer informatie over het maken van sjablonen, [Azure resourcemanager Authoring sjablonen](resource-group-authoring-templates.md).

1. Als u wilt een aangepaste sjabloon via de portal implementeert, selecteert u daarna **Nieuw**en start **De implementatie van de sjabloon** zoekt totdat u deze in de opties selecteren kunt.

    ![implementatie van de sjabloon zoeken](./media/resource-group-template-deploy-portal/search-template.png)

2. Selecteer **De implementatie van de sjabloon** in de beschikbare resources.

    ![Selecteer de sjabloonimplementatie](./media/resource-group-template-deploy-portal/select-template.png)

3. Na het starten van de sjabloonimplementatie, open de lege sjabloon die beschikbaar zijn om aan te passen.

    ![sjabloon maken](./media/resource-group-template-deploy-portal/show-custom-template.png)

    Klik in de editor toevoegen de syntaxis van de JSON waarin de bronnen die u wilt implementeren. Selecteer **Opslaan** wanneer u klaar bent. Zie voor hulp bij het schrijven van de syntaxis van de JSON, [resourcemanager sjabloon Stapsgewijze instructies](resource-manager-template-walkthrough.md).

    ![sjabloon bewerken](./media/resource-group-template-deploy-portal/edit-template.png)

4. Of u kunt een bestaande sjabloon selecteren in de [Azure quickstart sjablonen](https://azure.microsoft.com/documentation/templates/). Deze sjablonen zijn bijdrage van de community. Hierin zijn veel veelvoorkomende scenario's en iemand een sjabloon die lijkt op wat u wilt implementeren hebt toegevoegd. De sjablonen om iets die overeenkomt met uw scenario te vinden, kunt u zoeken.

    ![quickstart sjabloon selecteren](./media/resource-group-template-deploy-portal/select-quickstart-template.png)

    U kunt de geselecteerde sjabloon weergeven in de editor.

5. Na het opgeven van alle andere waarden, schakelt u **maken** om te implementeren van de sjabloon. 

    ![sjabloon implementeren](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Resources uit een sjabloon die zijn opgeslagen in uw account implementeren

De portal kunt u een sjabloon opslaan in uw Azure-account en later implementeren. Voor meer informatie over het werken met deze opgeslagen sjablonen, [aan de slag met persoonlijke sjablonen op de Azure-portal](./marketplace-consumer/mytemplates-getstarted.md).

1. Als u wilt zoeken naar opgeslagen sjablonen, selecteert u **Bladeren** > **sjablonen**.

    ![Bladeren in sjablonen](./media/resource-group-template-deploy-portal/browse-templates.png)

2. Selecteer in de lijst met sjablonen die zijn opgeslagen in uw account, de sectie die u wilt werken.

    ![opgeslagen sjablonen](./media/resource-group-template-deploy-portal/saved-templates.png)

3. Selecteer **Deploy** te implementeren van deze opgeslagen sjabloon.

    ![opgeslagen sjabloon implementeren](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Volgende stappen

- Controlelogboeken bijhouden, Zie [bewerkingen met resourcemanager controleren](resource-group-audit.md).
- Als u wilt oplossen voor implementatie, raadpleegt u [Probleemoplossing resource groep implementaties met Azure-portal](resource-manager-troubleshoot-deployments-portal.md).
- Een sjabloon worden opgehaald uit een implementatie of resourcegroep, raadpleegt u [de sjabloon Azure resourcemanager exporteren aan de bestaande bronnen](resource-manager-export-template.md).





