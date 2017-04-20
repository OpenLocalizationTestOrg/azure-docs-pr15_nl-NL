<properties
   pageTitle="Een sjabloon maken op logica app implementatie | Microsoft Azure"
   description="Leer hoe u een logica app implementatie-sjabloon maken en deze gebruiken voor het beheer van de release"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="create-a-logic-app-deployment-template"></a>Een logica app implementatie-sjabloon maken

Nadat een logica-app is gemaakt, wilt u mogelijk deze als een sjabloon Azure resourcemanager hebt gemaakt. Op deze manier kunt u eenvoudig implementeren de logica app aan elke omgeving of resourcegroep wanneer u deze mogelijk nodig. Voor een inleiding tot resourcemanager sjablonen, zorg ervoor dat u de artikelen over [Azure resourcemanager sjablonen ontwerpen](../resource-group-authoring-templates.md) en [implementeren van resources met behulp van Azure resourcemanager sjablonen](../resource-group-template-deploy.md)uitchecken.

## <a name="logic-app-deployment-template"></a>Sjabloon voor implementatie logica-app

Een app logica heeft drie basisonderdelen:

* **Logica app resource**. Deze resource bevat informatie over items, zoals prijzen van abonnement, locatie en de werkstroomdefinitie.
* **De definitie van de werkstroom**. Dit is wat wordt weergegeven in de codeweergave. Het bevat de definitie van de stappen van de stroom en hoe de engine moet worden uitgevoerd. Dit is de `definition` eigenschap van de resource van de app logica.
* **Verbindingen**. Dit zijn de afzonderlijke resources die metagegevens over elke verbindingslijn verbindingen, zoals een verbindingsreeks en een toegangstoken veilig worden opgeslagen. U deze in een app logica in verwijzingen maken naar de `parameters` gedeelte van de resource van de app logica.

U kunt alle onderdelen voor bestaande logica-apps met behulp van een hulpmiddel zoals [Azure Resource Explorer](http://resources.azure.com)weergeven.

Als u een sjabloon voor een app logica voor gebruik met resource groep implementaties, moet u de resources analyseren en zo nodig voorzien. Bijvoorbeeld als u een ontwikkeling, testen en productieomgeving, wilt u waarschijnlijk gebruiken tekenreeksen met de andere verbinding met een SQL-database in elke omgeving. Of u mogelijk wilt implementeren vanuit andere abonnementen of resourcegroepen.  

## <a name="create-a-logic-app-deployment-template"></a>Een logica app implementatie-sjabloon maken

De eenvoudigste manier om een sjabloon voor een geldige logica app implementatie is het gebruik van de [Visual Studio Tools voor logica-Apps](./app-service-logic-deploy-from-vs.md).  De Visual Studio tools Genereer een geldige implementatiesjabloon die kan worden gebruikt voor een abonnement of locatie.

Een paar andere hulpprogramma's voor kunnen u helpen als u een sjabloon maken op logica app-implementatie. U kunt creëert potlood, dat wil zeggen, met behulp van de resources die al die in dit artikel als u wilt maken van parameters naar wens. Een andere optie is via een [logica app sjabloon creator](https://github.com/jeffhollan/LogicAppTemplateCreator) PowerShell-module. Deze open source-module evalueert eerst de logica-app en alle verbindingen dat deze wordt gebruikt en vervolgens sjabloon resources met de benodigde parameters voor implementatie genereert. Bijvoorbeeld, hebt u een logica-app die u ontvangt van een bericht uit een wachtrij Azure Service Bus en voegt gegevens met een Azure SQL-database, wordt het hulpmiddel behouden alle de logica integratie en de verbinding SQL versus Service Bus tekenreeksen voorzien, zodat ze kunnen worden ingesteld op implementatie.

>[AZURE.NOTE] Verbindingen moeten in dezelfde resourcegroep als de logica-app.

### <a name="install-the-logic-app-template-powershell-module"></a>Installeer de logica app sjabloon PowerShell-module

De eenvoudigste manier voor het installeren van de module is via de [PowerShell-galerie](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1), met de opdracht `Install-Module -Name LogicAppTemplate`.  

Ook kunt u installeren de PowerShell-module handmatig:

1. Download de meest recente versie van de [logica app sjabloon creator](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
1. Extraheren van de map in uw map PowerShell-module (meestal `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Voor de module om te werken met een access-tenant en abonnementen toegangstoken, het is raadzaam dat u deze met het hulpmiddel van de opdrachtregel [ARMClient](https://github.com/projectkudu/ARMClient) gebruiken.  In dit [blog publiceren](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) , wordt ARMClient uitgebreider beschreven.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Een sjabloon voor een app logica genereren via PowerShell

Nadat u PowerShell is geïnstalleerd, kunt u een sjabloon genereren met behulp van de volgende opdracht uit:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`is de id van de Azure-abonnement. Deze regel eerst wordt een access token via ARMClient, en vervolgens deze door naar de PowerShell-script pipes en maakt vervolgens de sjabloon in een JSON-bestand.

## <a name="add-parameters-to-a-logic-app-template"></a>Parameters toevoegen aan een sjabloon voor een app logica

Nadat u uw sjabloon voor een app logica hebt gemaakt, kunt u doorgaan toevoegen of wijzigen van parameters die u mogelijk nodig hebt. Bijvoorbeeld als de definitie van uw een resource-ID aan een Azure-functie of geneste werkstroom die u van plan bent om te implementeren in een enkel implementatie bevat, kunt u meer resources toevoegen aan uw sjabloon en id's voorzien naar wens. Hetzelfde geldt voor alle verwijzingen naar aangepaste API's of Swagger eindpunten die u verwacht te implementeren met per resourcegroep.

## <a name="deploy-a-logic-app-template"></a>Een sjabloon voor een app logica implementeren

Met behulp van een willekeurig aantal hulpprogramma's, zoals PowerShell, REST API Visual Studio Release Management en de implementatie van Azure-Portal sjabloon kunt u uw sjabloon implementeren. Zie dit artikel over het [implementeren van resources met behulp van Azure resourcemanager sjablonen](../resource-group-template-deploy.md) voor meer informatie. Bovendien is het raadzaam dat u een [parameterbestand](../resource-group-template-deploy.md#parameter-file) om op te slaan waarden voor de parameter maken.

### <a name="authorize-oauth-connections"></a>Autoriseer OAuth-verbindingen

Na implementatie werkt de app logica end-to-end met ongeldige parameters. Echter moet OAuth verbindingen nog steeds worden gemachtigd om een geldige toegangstoken te genereren. U kunt dit doen door de logica-app in de ontwerpfunctie voor openen en klik vervolgens machtiging verbindingen. Of, als u automatiseren wilt, kunt u een script toe te staan elke OAuth-verbinding. Er is een voorbeeldscript op GitHub onder het project [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) .

## <a name="visual-studio-release-management"></a>Visual Studio Release Management

Een algemene scenario voor het implementeren en beheren van een omgeving is een hulpmiddel zoals Visual Studio Release beheer gebruiken met een sjabloon logica app-implementatie. Visual Studio Team Services bevat een [Azure resourcegroep implementeren](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) taak die u kunt aan een opbouwen toevoegen of verkooppijplijn loslaat. Moet u beschikken over een [service hoofdsom](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) voor een vergunning implementeren, en klikt u vervolgens kunt u de definitie release genereren.

1. Selecteer in Release Management, als u wilt een nieuwe definitie maken, **lege** om te beginnen met een lege definitie.

    ![Een nieuwe, lege definitie maken][1]   

1. Kies alle bronnen die u nodig voor dit hebt. Hiermee wordt waarschijnlijk de logica app-sjabloon handmatig of als onderdeel van het proces opbouwen gegenereerd.
1. Een taak **Implementatie van Azure Resource groep** toevoegen.
1. Met een [principal service](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)configureren en verwijzen naar de sjabloon en de sjabloonparameters-bestanden.
1. Gaat u verder met het bouwen van stappen in het proces van het release-programma voor een andere omgeving, geautomatiseerde test of goedkeurders naar wens.

<!-- Image References -->
[1]: ./media/app-service-logic-create-deploy-template/emptyReleaseDefinition.PNG
