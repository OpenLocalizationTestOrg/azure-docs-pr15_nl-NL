<properties 
    pageTitle="Logica Apps prijzen model | Microsoft Azure" 
    description="Meer informatie over hoe prijzen in logica-Apps werkt" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="logic-apps-pricing-model"></a>Logica Apps prijzen model

Azure logica Apps kunt u schaal en integratie werkstromen niet uitvoeren in de cloud.  Hieronder vindt u meer informatie over de facturering en abonnementen prijzen voor logica-Apps.

## <a name="consumption-pricing"></a>Verbruik prijzen

Nieuw gemaakte logica Apps gebruikt u een abonnement verbruik. Met de logica Apps verbruik prijzen model betaalt u alleen voor wat u gebruikt.  Logica Apps worden niet vertraagd wanneer u een abonnement verbruik gebruikt.
Alle acties uitgevoerd in een uitvoeren van een exemplaar van de app logica worden gemeten.

### <a name="what-are-action-executions"></a>Wat zijn de actie executions?

Elke stap in een definitie van de app logica is een actie.  Dit geldt ook voor triggers, besturingselement stroom stappen dezelfde voorwaarden voldoen, bereiken, voor elke lussen als tot lussen, oproepen aan verbindingslijnen en oproepen naar systeemeigen acties.
Triggers zijn alleen met speciale acties die zijn ontwikkeld voor een nieuwe instantie van een app logica wanneer een bepaalde gebeurtenis plaatsvindt.  Er zijn een aantal verschillende bewerkingen voor triggers kunnen beïnvloeden hoe de app logica is netwerken met een datalimiet.

-   Een eindpunt **polling-trigger** – deze trigger continu worden opgevraagd totdat er een bericht dat voldoet aan de criteria voor het maken van een nieuw exemplaar van een logica-app.  Het polling-interval kan worden geconfigureerd in de trigger in de ontwerpfunctie voor logica Apps.  Elk verzoek om een peiling, zelfs als deze niet in een nieuw exemplaar van een app logica gemaakt wordt, tellen als een actie kan worden uitgevoerd.

-   **Webhook trigger** – deze trigger is wacht van een client naar een nieuw vergaderverzoek verzenden op een bepaald eindpunt.  Elke aanvraag verzonden naar het eindpunt webhook telt als een actie kan worden uitgevoerd. De aanvraag en de inwerkingtreding HTTP Webhook zijn beide triggers webhook.

-   **Terugkeerpatroon trigger** – deze trigger maakt een nieuw exemplaar van de logica-app op basis van het terugkeerpatroon geconfigureerd in de trigger.  Een terugkeerpatroon-trigger kan zo worden geconfigureerd om uit te voeren om de 3 dagen of zelfs elke minuut.

Trigger executions kunnen worden bekeken in het blad logica Apps resource in het gedeelte Geschiedenis Trigger.

Alle bewerkingen die zijn uitgevoerd, of ze zijn geslaagde of mislukte zijn netwerken met een datalimiet als een actie kan worden uitgevoerd.  Acties die zijn overgeslagen vanwege een voorwaarde niet is voldaan of acties die niet worden uitgevoerd omdat de app logica beëindigd voordat deze was voltooid, worden niet geteld als de actie executions.

Acties uitgevoerd binnen lussen worden per herhaling van de lus geteld.  Bijvoorbeeld een enkele actie in een voor elke lus doorlopen een lijst met 10 items worden geteld als het aantal items in de lijst (10) vermenigvuldigd met het aantal acties in de lus (1) plus één telefoonnummer voor de inleiding van de lus die in dit voorbeeld is (10 * 1) + 1 = 11 actie executions.

Logica Apps die worden uitgeschakeld geen nieuwe exemplaren die zijn gemaakt en kunnen daarom op het moment dat deze zijn uitgeschakeld wordt geen krijgen btw geheven.  Zijn toegewezen dat na het uitschakelen van een app logica het even de tijd voor de exemplaren stilleggen duurt voordat u volledig die wordt uitgeschakeld.

## <a name="app-service-plans"></a>App-Service-abonnementen

Abonnementen van App-Service niet meer nodig om een logica-toepassing te maken.  U kunt ook verwijzen naar een App-Service plannen met een bestaande logica-app.  Logica apps eerder hebt gemaakt met een abonnement dat is App blijft gedragen voordat een waar, afhankelijk van het abonnement gekozen, wordt krijgen vertraagd nadat een aantal dagelijkse executions worden overschreden en wordt niet gefactureerd met de actie execution meter.

App-serviceplannen en hun dagelijkse toegestane actie executions:

| |Vrij/gedeeld/Basic|Standaard|Premium|
|---|---|---|---|
|Actie executions per dag| 200|10.000|50.000|

### <a name="convert-from-consumption-to-app-service-plan-pricing"></a>Van verbruik converteren naar de App-Service plannen prijzen

Als u verwijst naar een App-Service plannen voor een verbruik logica App, kunt u eenvoudig [uitvoeren de onder PowerShell-script](https://github.com/logicappsio/ConsumptionToAppServicePlan).  Zorg er eerst de [Hulpmiddelen voor Azure PowerShell](https://github.com/Azure/azure-powershell) is geïnstalleerd.

``` powershell
Param(
    [string] $AppService_RG = '<app-service-resource-group>',
    [string] $AppService_Name = '<app-service-name>',
    [string] $LogicApp_RG = '<logic-app-resource-group>',
    [string] $LogicApp_Name = '<logic-app-name>',
    [string] $subscriptionId = '<azure-subscription-id>'
)

Login-AzureRmAccount 
$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId
$appserviceplan = Get-AzureRmResource -ResourceType "Microsoft.Web/serverFarms" -ResourceGroupName $AppService_RG -ResourceName $AppService_Name
$logicapp = Get-AzureRmResource -ResourceType "Microsoft.Logic/workflows" -ResourceGroupName $LogicApp_RG -ResourceName $LogicApp_Name

$sku = @{
    "name" = $appservicePlan.Sku.tier;
    "plan" = @{
      "id" = $appserviceplan.ResourceId;
      "type" = "Microsoft.Web/ServerFarms";
      "name" = $appserviceplan.Name  
    }
}

$updatedProperties = $logicapp.Properties | Add-Member @{sku = $sku;} -PassThru

$updatedLA = Set-AzureRmResource -ResourceId $logicapp.ResourceId -Properties $updatedProperties -ApiVersion 2015-08-01-preview
```

### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>Van App-Service plannen prijzen verbruik converteren

Een App logica met een App-Service plannen gekoppeld aan een model verbruik van verwijderen van de verwijzing naar de App-Service plannen in de logica App definitie wijzigen.  Dit kunt doen met een oproep naar een PowerShell-cmdlet:

`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`

## <a name="pricing"></a>Prijzen

Zie [Logica Apps prijzen](https://azure.microsoft.com/pricing/details/logic-apps/)voor details prijzen.

## <a name="next-steps"></a>Volgende stappen

- [Een overzicht van de logica Apps][whatis]
- [Uw eerste logica-app maken][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: app-service-logic-what-are-logic-apps.md
[create]: app-service-logic-create-a-logic-app.md

