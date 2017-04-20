<properties
   pageTitle="Azure Resource Explorer | Microsoft Azure"
   description="Beschrijving van Azure Resource Explorer en hoe deze kan worden gebruikt om te bekijken en bijwerken van implementaties via Azure resourcemanager"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="stuartleeks"
   manager="ankodu"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/01/2016"
   ms.author="stuartle;tomfitz"/>

# <a name="use-azure-resource-explorer-to-view-and-modify-resources"></a>Azure Resource Explorer weergeven of wijzigen van resources gebruiken
De [Azure Resource Explorer](https://resources.azure.com) is een prima hulpmiddel voor het bekijken via de resources die u al hebt gemaakt in uw abonnement. Met dit hulpmiddel kunt u lezen hoe de resources zijn gestructureerd en raadpleegt u de eigenschappen die aan elke resource is toegewezen. U kunt meer informatie over de REST API bewerkingen en de PowerShell-cmdlets die beschikbaar voor een resourcetype zijn en kunt u opdrachten via de interface uitgeven. Resource Explorer kan zijn met name handig bij het maken van sjablonen resourcemanager omdat u de eigenschappen voor bestaande resources weergeven.

De bron voor de Resource Explorer tool is beschikbaar op [github](https://github.com/projectkudu/ARMExplorer), waarmee een waardevol verwijzing als u moet hetzelfde probleem zich voordoet implementeren in uw eigen toepassingen.

## <a name="view-resources"></a>Bronnen weergeven
Ga naar [https://resources.azure.com](https://resources.azure.com) en meld u aan met de dezelfde referenties die u voor de [Azure-Portal gebruiken wilt](https://portal.azure.com).

Wanneer geladen, wordt de structuurweergave aan de linkerkant kunt u inzoomen op uw abonnementen en resourcegroepen:

![structuurweergave](./media/resource-manager-resource-explorer/are-01-treeview.png)

Als u op een resourcegroep inzoomen ziet u de providers waarvoor er resources in die groep:

![providers](./media/resource-manager-resource-explorer/are-02-treeview-providers.png)

Hier kunt u beginnen lagere niveaus van de exemplaren van de resource. In de onderstaande schermafbeelding ziet u de `sltest` SQL Server-instantie in de structuurweergave. Aan de rechterkant, kunt u informatie over de REST API aanvragen die u met de desbetreffende resource gebruiken kunt bekijken. Door te schuiven naar het knooppunt voor een resource, worden Resource Explorer automatisch de GET-aanvraag om op te halen informatie over de resource gemaakt. In het grote tekstgebied onder de URL ziet u het antwoord van de API. 

Als u vertrouwd raken met resourcemanager sjablonen te begint de hoofdinhoud om te zoeken vertrouwde! De sectie **Eigenschappen** van het antwoord komt overeen met de waarden die u in de sectie **Eigenschappen** van de sjabloon opgeeft.

![SQL server](./media/resource-manager-resource-explorer/are-03-sqlserver-with-response.png)

Resource Explorer kunt u behouden inzoomen om te verkennen onderliggende resources, in het geval van de SQL-databaseserver, zijn er onderliggende bronnen voor dingen zoals databases en firewallregels.

Een database verkennen ziet u ons de eigenschappen van de database. In de onderstaande schermafbeelding kunt zien we dat de database `edition` is `Standard` en de `serviceLevelObjective` (of databaselaag) is `S1`.

![SQL-database](./media/resource-manager-resource-explorer/are-04-database-get.png)

## <a name="change-resources"></a>Resources wijzigen

Nadat u naar een resource bent gegaan, kunt u de knop bewerken om de JSON-inhoud worden bewerkt. U kunt vervolgens Resource Explorer gebruiken om te bewerken van de JSON en verzend een opslag-verzoek om te wijzigen van de resource. De onderstaande afbeelding ziet u bijvoorbeeld de databaselaag gewijzigd in `S0`:

![database - opslag-aanvraag](./media/resource-manager-resource-explorer/are-05-database-put.png)

Door te selecteren **zet** indienen u de aanvraag. 

Zodra de aanvraag is verzonden problemen Resource Explorer opnieuw de GET-aanvraag de status vernieuwen. In dit geval we kunt zien dat de `requestedServiceObjectiveId` is bijgewerkt en verschilt van de `currentServiceObjectiveId` die aangeeft dat een schaal bewerking uitgevoerd wordt. U kunt de GET klikken om de status handmatig vernieuwen.

![database - GET-request2](./media/resource-manager-resource-explorer/are-06-database-get2.png)

## <a name="performing-actions-on-resources"></a>Acties op resources uit te voeren

Het tabblad **Acties** kunt u zien en aanvullende REST bewerkingen uitvoeren. Bijvoorbeeld wanneer u een resource website hebt geselecteerd, vindt het tabblad acties u een lang overzicht van beschikbare bewerkingen, sommige hieronder worden weergegeven.

![Web - POST-aanvraag](./media/resource-manager-resource-explorer/are-web-post.png)

## <a name="invoking-the-api-via-powershell"></a>Aanroepen van de API via PowerShell
Het tabblad PowerShell in Resource Explorer ziet u de cmdlets gebruiken om te communiceren met de bron die u momenteel verkennen bent. Afhankelijk van het type resource die u hebt geselecteerd, het weergegeven PowerShell-script kan variëren van eenvoudige cmdlets (zoals `Get-AzureRmResource` en `Set-AzureRmResource`) naar ingewikkelder cmdlets (zoals verwisselen sleuven op een website). 

![PowerShell](./media/resource-manager-resource-explorer/are-07-powershell.png)

Zie voor meer informatie over het Azure PowerShell-cmdlets, [Azure PowerShell gebruiken met Azure Resource Manager](powershell-azure-resource-manager.md)

## <a name="summary"></a>Overzicht
Als u werkt met Resource Manager, kan de Resource Explorer zeer nuttig gereedschap zijn. Dit is een uitstekende manier om te vinden van manieren om PowerShell query en wijzigingen te gebruiken. Als u met de REST API werkt is een uitstekende manier om aan de slag en API oproepen snel te testen voordat u begint met het schrijven van code. En als u sjablonen dat het kan zijn een uitstekende manier schrijft om meer informatie over de hiërarchie van de resource en zoeken waar moeten worden geplaatst configuratie - kunt u er een wijziging aanbrengt in de Portal en zoek de overeenkomende vermeldingen in Resource Explorer!

Bekijk het [kanaal 9 video met Scott Hanselman en David Ebbo](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Resource-Manager-Explorer-with-David-Ebbo) voor meer informatie.


