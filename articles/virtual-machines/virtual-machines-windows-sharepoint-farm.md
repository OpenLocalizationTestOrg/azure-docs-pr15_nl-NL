<properties
    pageTitle="Maken van SharePoint server-farms | Microsoft Azure"
    description="Snel een nieuwe SharePoint 2013 of 2016 van de SharePoint-farm maken in Azure wordt aangegeven."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="josephd"/>

# <a name="create-sharepoint-server-farms"></a>SharePoint server-farms maken

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke model.

## <a name="sharepoint-2013-farms"></a>SharePoint 2013 farms

Met de portal marketplace van Microsoft Azure, kunt u snel vooraf geconfigureerde SharePoint Server 2013 farms maken. Dit kunt u veel tijd wanneer u een eenvoudige of hoge beschikbaarheid SharePoint-farm nodig voor een ontwikkelaar/testomgeving hebt of opslaan als u SharePoint Server 2013 als een oplossing voor teamsamenwerking voor uw organisatie evalueert.

> [AZURE.NOTE] Het item **SharePoint Server-Farm** in de Azure Marketplace van de Azure-portal is verwijderd. Dit is vervangen door de **SharePoint 2013 niet-HA-Farm** en **SharePoint 2013 HA-Farm** items.

De standaard SharePoint-farm bestaat uit drie virtuele machines in deze configuratie.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/Non-HAFarm.png)

U kunt deze serverfarm-configuratie gebruiken voor een eenvoudigere instelling voor het ontwikkelen van Apps voor SharePoint of de eerste keer evaluatie van SharePoint 2013.

De eenvoudige (drie-server) SharePoint-farm maken:

1. Klik op [hier](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Klik op **implementeren**.
3. Klik op **maken**in het deelvenster **SharePoint 2013 niet-HA-Farm** .
4. Instellingen opgeven op de stappen in het deelvenster **Maken SharePoint 2013 niet-HA-Farm** en klik vervolgens op **maken**.

De SharePoint-farm hoge beschikbaarheid bestaat uit negen virtuele machines in deze configuratie.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/HAFarm.png)

U kunt deze serverfarm-configuratie gebruiken om te testen hoger client laadtijd, betere beschikbaarheid van de externe SharePoint-site en SQL Server AlwaysOn beschikbaarheidsgroepen voor een SharePoint-farm. U kunt ook deze configuratie gebruiken voor het ontwikkelen van Apps voor SharePoint in een omgeving met hoge beschikbaarheid.

De SharePoint-farm van hoge beschikbaarheid (9-server) maken:

1. Klik op [hier](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Klik op **implementeren**.
3. Klik op **maken**in het deelvenster **SharePoint 2013 HA-Farm** .
4. Instellingen opgeven op de zeven stappen in het deelvenster **Maken SharePoint 2013 HA-Farm** en klik vervolgens op **maken**.

> [AZURE.NOTE] U kunt de **SharePoint 2013 niet-HA-Farm** of **SharePoint 2013 HA-Farm** maken met een gratis proefversie van Azure.

De portal van Azure Hiermee maakt u beide van deze farms in een virtueel netwerk met een internetgerichte website alleen de cloud. Er is geen verbinding naar website VPN of ExpressRoute terug naar het netwerk van uw organisatie.

> [AZURE.NOTE] Als u de basic maakt of hoge beschikbaarheid SharePoint-farms met behulp van de Azure portal, kunt u een bestaande resourcegroep geen opgeven. U omzeilt deze beperking, door deze farms met Azure PowerShell te maken. Zie [SharePoint 2013 maken ontwikkelaar/testen farms met Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell)voor meer informatie.

## <a name="sharepoint-2016-farms"></a>SharePoint-2016 farms

Zie [dit artikel](https://technet.microsoft.com/library/mt723354.aspx) voor de instructies voor het maken van de volgende één server 2016 van SharePoint Server-farm.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a>De SharePoint-farms beheren

U kunt de servers van deze farms via verbindingen met extern bureaublad beheren. Zie [aanmelden bij de virtuele machine](virtual-machines-windows-hero-tutorial.md#log-on-to-the-virtual-machine)voor meer informatie.

U kunt configureren Mijn sites, SharePoint-toepassingen en andere functionaliteit van de Centraal beheer van de SharePoint-site. Zie [SharePoint configureren](http://technet.microsoft.com/library/ee836142.aspx)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Kennismaken met aanvullende [SharePoint configuraties](https://technet.microsoft.com/library/dn635309.aspx) in Azure infrastructuurservices.
