<properties
   pageTitle="Gebruik van de verbindingslijn Azure Resource in logica apps | Microsoft Azure-App-Service"
   description="Het maken en configureren van de Azure Resource verbindingslijn of API-app en deze gebruiken in een app logica in Azure App-Service"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="stepsic"/>

# <a name="get-started-with-the-azure-resource-connector-and-add-it-to-your-logic-app"></a>Aan de slag met de verbindingslijn van de Resource Azure en deze toevoegen aan uw logica-app
>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2014-12-01-schema voorbeeldversie.

Gebruik de verbindingslijn van de Resource Azure eenvoudig Azure bronnen beheren binnen uw logica-app.

## <a name="create-the-azure-resource-connector"></a>De verbindingslijn Azure Resource maken
Gebruik Azure Resource verbindingslijn API app, moet u eerst een exemplaar van dit te maken. Dit is mogelijk een van beide inline tijdens het maken van een app logica of door te selecteren van de Azure Resource Manager verbindingslijn API-app van Azure Marketplace.

Als u wilt configureren, hebt u nodig hebt voor het instellen van een Service Principal met machtigingen om te doen wat het is dat u wilt doen in Azure wordt aangegeven. Alle oproepen komen op-namens-of deze Service Principal die u hebt ingesteld. Hiermee kunt u om te bepalen de verbindingslijn gebruiken alleen precies wat u wilt doen, en niets meer.

David Ebbo heeft [een geweldige blogbericht](http://blog.davidebbo.com/2014/12/azure-service-principal.html) geschreven op hoe u dit instelt. Alle de instructies er en uw **Tenant-ID**, **Cliënt-ID** en **geheim**ophalen. Deze drie velden, plus de **Abonnements-ID**, zijn wat zijn vereist voor het configureren van de verbindingslijn.

## <a name="using-the-azure-resource-connector-in-logic-apps-designer"></a>De verbindingslijn van de Resource Azure in logica apps designer gebruiken
### <a name="trigger"></a>Trigger
Er zijn twee triggers die worden ondersteund in de verbindingslijn:

Naam | Beschrijving
---- | -----------
Gebeurtenis plaatsvindt | Trigger bij een gebeurtenis aan een resourceweergave in uw abonnement.
Metrisch kruist drempelwaarde |  Trigger wanneer een meting voldoet aan een bepaalde drempel.

### <a name="action"></a>Actie

U kunt ook een groot aantal acties bevatten binnen uw Azure-abonnement:

Voor **resourcegroepen** kunt u het volgende doen:

Naam | Beschrijving
---- | -----------
Lijst-resourcegroepen | Een lijst met alle resourcegroepen in het abonnement.
Resourcegroep ophalen | Een resourcegroep met een id krijgen.
Resourcegroep maken | Maken of een resourcegroep bijwerken.
Resourcegroep verwijderen | Een resourcegroep verwijderen.

Voor **Resources** kunt u het volgende doen:

Naam | Beschrijving
---- | -----------
Lijst met resources | Lijst met bronnen in uw abonnement met verschillende soorten filters.
Bron | Één resource door de resource-id ophalen
Maken of bijwerken van resource | Een resource maken of bijwerken van een bestaande bron. U moet alle eigenschappen opgeven voor de desbetreffende resource.
Resource-actie |  Andere acties uitvoeren op een resource. U moet weten de actienaam van de en het pakket dat deze actie duurt (indien aanwezig).
Resource verwijderen | Een resource verwijderen.

Voor **Resource Providers** kunt u het volgende doen:

Naam | Beschrijving
---- | -----------
Lijst resource providers | Een lijst met alle beschikbare resource providers in het abonnement.

Voor **Resource groep implementaties** kunt u het volgende doen:

Naam | Beschrijving
---- | -----------
Lijst-implementaties | Overzicht van de implementaties in een resourcegroep.
Implementatie ophalen | De sjabloonimplementatie van een met een id krijgen.
Implementatie maken | Maak een nieuwe resource groep implementatie door middel van een sjabloon.

Voor **gebeurtenissen** over resources kunt u het volgende doen:

Naam | Beschrijving
---- | -----------
Gebeurtenissen ophalen | U gebeurtenissen in een abonnement of voor een resource.

Voor **de doelstellingen** over resources kunt u het volgende doen:

Naam | Beschrijving
---- | -----------
Aan de doelstellingen ophalen | Een meting ophalen voor een resource Id.

## <a name="do-more-with-your-connector"></a>Doe meer met de verbindingslijn
Nu dat de verbindingslijn is gemaakt, kunt u deze toevoegen aan een zakelijke stroom met een logica-app. Zie [Wat zijn de logica apps?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Als u aan de slag met Azure logica apps wilt voordat u zich registreert voor een Azure-account, gaat u naar [probeert logica app](https://tryappservice.azure.com/?appservice=logic), waar u direct een tijdelijk starter logica-app in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

Weergeven van de verwijzing Swagger REST API bij [verbindingslijnen en API apps verwijzing](http://go.microsoft.com/fwlink/p/?LinkId=529766).

<!--References -->

<!--Links -->
[Creating a Logic app]: app-service-logic-create-a-logic-app.md
