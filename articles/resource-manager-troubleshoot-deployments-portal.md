<properties
   pageTitle="Implementatie bewerkingen met de portal weergeven | Microsoft Azure"
   description="Beschrijving van het gebruik van de Azure-portal naar veelvoorkomende fouten in resourcemanager-implementatie."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-portal"></a>Implementatie bewerkingen weergeven met de Portal van Azure

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API](resource-manager-troubleshoot-deployments-rest.md)

U kunt de bewerkingen voor een implementatie via de portal van Azure weergeven. U mogelijk interessant bekijken van de bewerkingen als er een fout optreedt zodat in dit artikel bevat informatie over het weergeven van de bewerkingen die zijn mislukt tijdens de implementatie hebt. De portal biedt een interface waarmee u kunt eenvoudig vinden van de fouten en mogelijke oplossingen bepalen.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="use-deployment-operations-to-troubleshoot"></a>Implementatie bewerkingen gebruiken om op te lossen

Als u wilt zien van de implementatie-bewerkingen, gebruikt u de volgende stappen uit:

1. Voor de resourcegroep die nodig zijn voor het in de implementatie, ziet u de status van de laatste implementatie. U kunt deze status voor meer informatie.

    ![Implementatiestatus](./media/resource-manager-troubleshoot-deployments-portal/deployment-status.png)

2. Hier ziet u de recente implementatie geschiedenis. Selecteer de implementatie die is mislukt.

    ![Implementatiestatus](./media/resource-manager-troubleshoot-deployments-portal/select-deployment.png)

3. Selecteer **is mislukt. Klik hier voor meer informatie** als u een beschrijving van waarom de implementatie is mislukt. De DNS-record is in de onderstaande afbeelding niet uniek.  

    ![mislukte implementatie weergeven](./media/resource-manager-troubleshoot-deployments-portal/view-error.png)

    Dit foutbericht wordt weergegeven moet u op te lossen. Als u meer informatie nodig over welke taken zijn voltooid, kunt u de bewerkingen weergeven, zoals wordt weergegeven in de volgende stappen uit.

4. U kunt alle bewerkingen implementatie weergeven in het blad **implementatie** . Selecteer een bewerking voor meer details kunnen zien.

    ![bewerkingen weergeven](./media/resource-manager-troubleshoot-deployments-portal/view-operations.png)

    In dit geval ziet u dat de opslag-account, virtuele netwerk en beschikbaarheid van de set zijn gemaakt. Het openbare IP-adres is mislukt en andere resources zijn niet geprobeerd.

5. U kunt gebeurtenissen voor de implementatie weergeven door het selecteren van **gebeurtenissen**.

    ![gebeurtenissen weergeven](./media/resource-manager-troubleshoot-deployments-portal/view-events.png)

6. U ziet alle gebeurtenissen voor de implementatie en selecteert u een voor meer informatie.

    ![Zie gebeurtenissen](./media/resource-manager-troubleshoot-deployments-portal/see-all-events.png)

## <a name="use-audit-logs-to-troubleshoot"></a>Controlelogboeken gebruiken om op te lossen

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Als u wilt zien fouten voor een implementatie, gebruik de volgende stappen uit:

1. De controlelogboeken voor een resourcegroep weergeven door in te schakelen **Logboeken controleren**.

    ![Selecteer controlelogboeken](./media/resource-manager-troubleshoot-deployments-portal/select-audit-logs.png)

2. In het blad **Controlelogboeken bijhouden** ziet u een overzicht van recente bewerkingen voor alle resourcegroepen in uw abonnement. Het bevat een grafische weergave van de tijd en de status van de bewerkingen, maar ook een lijst met de bewerkingen.

    ![acties weergeven](./media/resource-manager-troubleshoot-deployments-portal/audit-summary.png)

3. U kunt de weergave van de controlelogboeken kunt richten op bepaalde voorwaarden filteren. Selecteer **Filter** boven aan het blad **controlelogboeken bijhouden** .

    ![filter-Logboeken](./media/resource-manager-troubleshoot-deployments-portal/filter-logs.png)

4. Selecteer in het blad **Filter** voorwaarden voor de weergave van de controlelogboeken beperken tot alleen de bewerkingen die u wilt zien. U kunt bijvoorbeeld bewerkingen zodat alleen weergegeven fouten voor de resourcegroep filteren.

    ![filteropties instellen](./media/resource-manager-troubleshoot-deployments-portal/set-filter.png)

5. Verder kunt u bewerkingen filteren door in te stellen van een tijdspanne. De weergave voor een bepaalde 20 minuten tijdspanne worden gefilterd door de volgende afbeelding.

    ![tijd instellen](./media/resource-manager-troubleshoot-deployments-portal/select-time.png)

6. U kunt geen van de bewerkingen in de lijst selecteren. Kies de bewerking waarmee de fout die u wilt onderzoeken bevat.

    ![Selecteer bewerking](./media/resource-manager-troubleshoot-deployments-portal/select-operation.png)
  
7. U ziet alle gebeurtenissen voor de bewerking. Zoals u ziet de **Correlatie-id's** in het overzicht. Deze ID wordt gebruikt voor het bijhouden van gebeurtenissen. Het kan handig zijn wanneer u werkt met technische ondersteuning van een probleem kunt oplossen. U kunt een van de gebeurtenis voor meer informatie over de gebeurtenis te selecteren.

    ![Selecteer gebeurtenis](./media/resource-manager-troubleshoot-deployments-portal/select-event.png)

8. Hier ziet u details van de gebeurtenis. Met name aandacht besteden aan de **Eigenschappen** voor informatie over de fout.

    ![audit logboekdetails weergeven](./media/resource-manager-troubleshoot-deployments-portal/audit-details.png)

Het filter dat u op het controlelogboek hebt toegepast, wordt de volgende keer dat u deze bekijken, zodat u mogelijk moet wijzigen van deze waarden als u wilt uitbreiden, de weergave van de bewerkingen bewaard.

## <a name="next-steps"></a>Volgende stappen

- Zie [oplossen van veelvoorkomende fouten bij de implementatie van Azure met Azure resourcemanager-informatiebronnen](resource-manager-common-deployment-errors.md)voor hulp bij het oplossen van implementatiefouten bepaalde.
- Voor meer informatie over het gebruik van de controlelogboeken om de andere soorten acties te houden, raadpleegt u [bewerkingen met resourcemanager controleren](resource-group-audit.md).
- Zie [Deploy resourcegroep met Azure resourcemanager sjabloon](resource-group-template-deploy.md)valideren van uw implementatie voordat deze wordt uitgevoerd.
