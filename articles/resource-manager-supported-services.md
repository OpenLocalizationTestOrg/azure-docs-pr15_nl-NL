<properties
   pageTitle="Resourcemanager ondersteund services | Microsoft Azure"
   description="Beschrijving van de resource-providers die ondersteuning bieden voor resourcemanager, hun schema's en beschikbare API-versies en de regio's dat de host van de resources."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="magoedte;tomfitz"/>

# <a name="resource-manager-providers-regions-api-versions-and-schemas"></a>Resourcemanager providers, regio's, API-versies en schema's maken

Azure resourcemanager vindt u een nieuwe manier om te implementeren en beheren van de services waaruit uw toepassingen. De meeste, maar niet alle services ondersteunen resourcemanager en bepaalde services resourcemanager slechts gedeeltelijk ondersteund. In dit onderwerp biedt een lijst met ondersteunde resource providers voor Azure resourcemanager.

Wanneer u uw resources implementeert, moet u ook weten welke regio's die Ondersteuningsbronnen en welke API-versies zijn beschikbaar voor de resources. De sectie [ondersteunde regio's](#supported-regions) ziet u hoe u wilt weten welke regio's werken voor uw abonnement en resources. De sectie [ondersteund API versies](#supported-api-versions) ziet u hoe om te bepalen welke API-versies die u kunt gebruiken.

Zie [Azure beschikbaarheid van de portal-grafiek](https://azure.microsoft.com/features/azure-portal/availability/)voor welke services worden ondersteund in de portal van Azure en klassieke portal. Welke services zwevend Ondersteuningsbronnen, raadpleegt u [resources naar nieuwe resourcegroep abonnement verplaatsen](resource-group-move-resources.md).

De volgende tabellen bevatten welke Microsoft services ondersteuning-implementatie en beheer via resourcemanager en welke niet. De koppeling in de kolom **Quickstart sjablonen** stuurt een query naar de bibliotheek Azure Quickstart sjablonen voor de opgegeven resource-provider. QuickStart sjablonen zijn toegevoegd en regelmatig bijgewerkt. De aanwezigheid van een koppeling voor een bepaalde service houdt niet per se in dat de query geeft sjablonen uit de bibliotheek. Er zijn ook diverse derden resource-providers die ondersteuning bieden voor resourcemanager. Leert u hoe om alle resource-providers in de sectie [Resource providers en typen](#resource-providers-and-types) weer te geven.


## <a name="compute"></a>Berekenen

| Service | Resourcemanager die zijn ingeschakeld | REST API | Schema | QuickStart sjablonen |
| ------- | ------------------------ |-------- | ------ | ------ |
| Batch   | Ja     | [Batch REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) | [2015-12-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-12-01/Microsoft.Batch.json)       |  |
|Container | Ja | [Container Service REST](https://msdn.microsoft.com/library/azure/mt711470.aspx) | [2016-03-30 onderneming](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.ContainerService.json) | [Microsoft.ContainerService](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ContainerService%22&type=Code) |
| Dynamics Levenscyclusservices | Ja  |      |        |  |
| Schaal Sets | Ja | [Schaal REST instellen](https://msdn.microsoft.com/library/azure/mt705635.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachineScaleSets](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=virtualMachineScaleSets&type=Code) | 
| Service stof | Ja  | [Service stof Rest](https://msdn.microsoft.com/library/azure/dn707692.aspx)  |      | [Microsoft.ServiceFabric](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceFabric%22&type=Code) |
| Virtuele Machines | Ja | [VM REST](https://msdn.microsoft.com/library/azure/mt163647.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachines](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Compute%2Fvirtualmachines%22&type=Code) |
| Virtuele Machines (klassieke) | Beperkt | - | - | - |
| Remote-App | Nee      | -  | - | - |
| Cloudservices (klassieke) | Beperkt (Zie hieronder) | - | - | - |

Virtuele Machines (klassieke) verwijst naar de resources die zijn geïmplementeerd via het implementatiemodel klassieke, in plaats van via het objectmodel resourcemanager-implementatie. In het algemeen, deze resources resourcemanager bewerkingen niet ondersteund, maar er zijn enkele bewerkingen die zijn ingeschakeld. Zie voor meer informatie over deze implementatiemodellen, [lidmaatschap resourcemanager implementatie- en klassieke implementatie](resource-manager-deployment-model.md). 

Cloudservices (klassieke) kunnen worden gebruikt met andere klassieke resources; klassieke resources zijn echter doen niet profiteren van alle functies van de Resource Manager en zijn niet geschikt voor toekomstige oplossingen. In plaats daarvan een goed idee om de infrastructuur van uw toepassing als u wilt gebruiken, resources uit de naamruimten Microsoft.Compute, Microsoft.Storage en Microsoft.Network.


## <a name="networking"></a>Netwerken

| Service | Resourcemanager die zijn ingeschakeld | REST API | Schema | QuickStart sjablonen |
| ------- | -------  | -------- | ------ | ------ |
| Toepassingsgateway | Ja | [Toepassingsgateway REST](https://msdn.microsoft.com/library/azure/mt684939.aspx)   |        | [applicationGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FapplicationGateways%22&type=Code) |
| DNS     | Ja | [DNS-REST](https://msdn.microsoft.com/library/azure/mt163862.aspx)         | [04-01-2016](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Network.json) | [dnsZones](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FdnsZones%22&type=Code) |
| ExpressRoute | Ja  | [ExpressRoute REST](https://msdn.microsoft.com/library/azure/mt586720.aspx)  |       | [expressRouteCircuits](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FexpressRouteCircuits%22&type=Code) |
| De belasting voor documentconversies | Ja  | [De belasting voor documentconversies REST](https://msdn.microsoft.com/library/azure/mt163651.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [loadBalancers](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Floadbalancers%22&type=Code) |
| Verkeer Manager | Ja | [Verkeer Manager REST](https://msdn.microsoft.com/library/azure/mt163667.aspx) | [2015-01-11](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-11-01/Microsoft.Network.json)  | [trafficmanagerprofiles](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Ftrafficmanagerprofiles%22&type=Code) |
| Virtuele netwerken | Ja| [Virtual Network REST](https://msdn.microsoft.com/en-us/library/azure/mt163650.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [virtualNetworks](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworks%22&type=Code) |
| VPN Gateway | Ja | [Netwerkgateway REST](https://msdn.microsoft.com/library/azure/mt163859.aspx) |  | [virtualNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworkGateways%22&type=Code) <br /> [localNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FlocalNetworkGateways%22&type=Code) <br />[verbindingen](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Fconnections%22&type=Code) |


## <a name="storage"></a>Opslag

| Service | Resourcemanager die zijn ingeschakeld | REST API | Schema | QuickStart sjablonen |
| ------- | ------- | -------- | ------ | ------- | ------ |
| Opslag | Ja  | [Opslag REST](https://msdn.microsoft.com/library/azure/mt163683.aspx) | [Opslag-account](resource-manager-template-storage.md) | [Microsoft.Storage](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Storage%22&type=Code) |
| StorSimple | Ja  |         |        |  |

## <a name="databases"></a>Databases

| Service | Resourcemanager die zijn ingeschakeld | REST API | Schema | QuickStart sjablonen |
| ------- | ------- | -------- | ------ | ------- | ------ |
| DocumentDB | Ja  | [DocumentDB REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) | [2015-04-08](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-08/Microsoft.DocumentDB.json)  | [Microsoft.DocumentDB](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DocumentDb%22&type=Code) |
| Bestand Vgx Cache. | Ja |   | [04-01-2016](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Cache.json) | [Microsoft.Cache](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cache%22&type=Code) |
| SQL-Database | Ja | [SQL-Database REST](https://msdn.microsoft.com/library/azure/mt163571.aspx) | [2014-04-01-voorbeeld](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) | [Microsoft.Sql](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Sql%22&type=Code) |
| SQL datawarehouse | Ja |   |      |


## <a name="web--mobile"></a>Web en Mobile

| Service | Resourcemanager die zijn ingeschakeld | REST API | Schema | QuickStart sjablonen |
| ------- | ------- | -------- | ------ | ------ |
| API-Apps | Ja |   | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [API-Apps](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22kind%22%3A+%22apiApp%22&type=Code) |
| API-beheer | Ja  | [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) | [2016-07-07](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-07-07/Microsoft.ApiManagement.json)       | [Microsoft.ApiManagement](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ApiManagement%22&type=Code) | 
| Inhoud Moderator | Ja |   |   |   |
| Functie-App | Ja |   |   | [functionApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22functionApp%22&type=Code) |
| Logica Apps | Ja   | [Werkstroom Service REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx) | [2016-01-06](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.Logic.json) | [Microsoft.Logic](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Logic%22&type=Code) |
| Mobiele Apps | Ja |  | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json)  | [mobileApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22mobileApp%22&type=Code)   |
| Mobiele afspraken | Ja  |  [Mobiele betrokkenheid REST](https://msdn.microsoft.com/library/azure/mt683754.aspx)        |        | [Microsoft.MobileEngagements](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.MobileEngagement%22&type=Code) |
| Zoeken | Ja  | [Zoeken REST](https://msdn.microsoft.com/library/azure/dn798935.aspx) |  | [Microsoft.Search](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Search%22&type=Code) |
| WebApps | Ja |          | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [Microsoft.Web](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Web%22&type=Code) |

## <a name="analytics"></a>Analytics

| Service | Resourcemanager die zijn ingeschakeld | REST API | Schema | QuickStart sjablonen |
| ------- | -------  | -------- | ------ | ------ |
| Gegevenscatalogus | Ja  | [Gegevenscatalogus REST](https://msdn.microsoft.com/library/azure/mt267595.aspx)  | [2016-03-30 onderneming](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.DataCatalog.json) |  |
| Gegevens Factory | Ja | [Gegevens Factory REST](https://msdn.microsoft.com/library/azure/dn906738.aspx) |    | [Microsoft.DataFactory](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataFactory%22&type=Code) |
| Gegevens Lake Analytics | Ja |   |   [2015-10-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeAnalytics](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeAnalytics%22&type=Code) |
| Lake gegevensopslag | Ja  | [Gegevensopslag Lake REST](https://msdn.microsoft.com/library/azure/mt693424.aspx) | [2015-10-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeStore](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeStore%22&type=Code) |
| HDInsights | Ja | [HDInsights REST](https://msdn.microsoft.com/library/azure/mt622197.aspx) |        | [Microsoft.HDInsight](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.HDInsight%22&type=Code) |
| Machine Learning | Ja | [Machine Learning-REST](https://msdn.microsoft.com/library/azure/mt767538.aspx)        | [2016-05-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-01-preview/Microsoft.MachineLearning.json) |
| Stream Analytics | Ja  | [Stoom Analytics REST](https://msdn.microsoft.com/library/azure/dn835031.aspx)   |        |  |
| Power BI | Ja | [Power BI ingesloten REST](https://msdn.microsoft.com/library/azure/mt712303.aspx) | [2016-01-29](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-01-29/Microsoft.PowerBI.json) |  |

## <a name="intelligence"></a>Bedrijfsinformatie

| Service | Resourcemanager die zijn ingeschakeld | REST API | Schema | QuickStart sjablonen |
| ------- | ------- | -------- | ------ | ------ |
| Cognitieve Services | Ja |  | [2016-02-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-01-preview/Microsoft.CognitiveServices.json) |   |

## <a name="internet-of-things"></a>Internet dingen

| Service | Resourcemanager die zijn ingeschakeld | REST API | Schema | QuickStart sjablonen |
| ------- | ------- | -------- | ------ | ------ |
| Gebeurtenis-Hub | Ja  | [Gebeurtenis-Hub REST](https://msdn.microsoft.com/library/azure/dn790674.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.EventHub.json) | [Microsoft.EventHub](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.EventHub%22&type=Code)  |
| IoTHubs | Ja | [IoT Hub REST](https://msdn.microsoft.com/library/azure/mt589014.aspx) | [2016-02-03](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-03/Microsoft.Devices.json) | [Microsoft.Devices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Devices%22&type=Code) |
| Melding Hubs | Ja | [Melding Hub REST](https://msdn.microsoft.com/library/azure/dn495827.aspx) | [04-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) | [Microsoft.NotificationHubs](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.NotificationHubs%22&type=Code) |

## <a name="media--cdn"></a>Media & CDN

| Service | Resourcemanager die zijn ingeschakeld | REST API | Schema | QuickStart sjablonen |
| ------- | ------- | -------- | ------ | ------ |
| CDN | Ja | [CDN REST](https://msdn.microsoft.com/library/azure/mt634456.aspx)  | [2016-04-02](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-02/Microsoft.Cdn.json) | [Microsoft.Cdn](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cdn%22&type=Code) |
| Media-Service | Ja | [Media-Services REST](https://msdn.microsoft.com/library/azure/hh973617.aspx)  | [10-01-2015](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01/Microsoft.Media.json) |


## <a name="hybrid-integration"></a>Hybride-integratie

| Service | Resourcemanager die zijn ingeschakeld | REST API | Schema | QuickStart sjablonen |
| ------- | ------- | -------- | ------ | ------ |
| BizTalk-Services | Ja |          | [04-01-2014](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) |  |
| Herstel-Service | Ja | [Site-herstel REST](https://msdn.microsoft.com/library/azure/mt750497.aspx) | [2016-01-06](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.RecoveryServices.json) | [Microsoft.RecoveryServices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.RecoveryServices%22&type=Code) |
| Service Bus | Ja | [Service Bus REST](https://msdn.microsoft.com/library/azure/mt639375.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.ServiceBus.json)       | [Microsoft.ServiceBus](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceBus%22&type=Code) |

## <a name="identity--access-management"></a>Identiteit & toegangsbeheer 

Azure Active Directory werkt met resourcemanager om in te schakelen Rolgebaseerd toegangsbeheer voor uw abonnement. Zie voor meer informatie over het gebruik van Rolgebaseerd toegangsbeheer en Active Directory, [Toegangsbeheer op basis van Azure rol](./active-directory/role-based-access-control-configure.md).

## <a name="developer-services"></a>Services voor ontwikkelaars 

| Service | Resourcemanager die zijn ingeschakeld | REST API | Schema | QuickStart sjablonen |
| ------- | ------- | -------- | ------ | ------ |
| Toepassing inzichten | Ja  | [Inzichten REST](https://msdn.microsoft.com/library/azure/dn931943.aspx)  | [04-01-2014](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.Insights.json) | [Microsoft.Insights](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.insights%22&type=Code) |
| Bing Maps | Ja  |          |        |  |
| DevTest Labs | Ja |   | [2016-05-15](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-15/Microsoft.DevTestLab.json) | [Microsoft.DevTestLab](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DevTestLab%22&type=Code)  |
| Visual Studio-account | Ja   |          | [2014-02-26](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) |  |

## <a name="management-and-security"></a>Beheer en beveiliging

| Service | Resourcemanager die zijn ingeschakeld | REST API | Schema | QuickStart sjablonen |
| ------- | ------- | -------- | ------ | ------ |
| Automatisering | Ja | [Automatisering REST](https://msdn.microsoft.com/library/azure/mt662285.aspx) | [2015-10-31](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-31/Microsoft.Automation.json)       | [Microsoft.Automation](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Automation%22&type=Code) |
| Belangrijke kluis | Ja | [Belangrijke kluis REST](https://msdn.microsoft.com/library/azure/dn903609.aspx) | [Belangrijke kluis](resource-manager-template-keyvault.md)<br />[Belangrijke kluis geheim](resource-manager-template-keyvault-secret.md)   | [Microsoft.KeyVault](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.KeyVault%22&type=Code) |
| Operationele inzichten | Ja |          |        |  |
| Planner | Ja  | [Scheduler REST](https://msdn.microsoft.com/library/azure/mt629143.aspx)     | [03-01-2016](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-01/Microsoft.Scheduler.json) | [Microsoft.Scheduler](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Scheduler%22&type=Code) |
| Beveiliging (preview) | Ja | [Beveiliging REST](https://msdn.microsoft.com/library/azure/mt704034.aspx)  |  | [Microsoft.Security](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Security%22&type=Code)  |

## <a name="resource-manager"></a>Resourcemanager

| Functie | Resourcemanager die zijn ingeschakeld | REST API | Schema | QuickStart sjablonen |
| ------- | ------- | -------------- | -------- | ------ | ------ |
| Autorisatie | Ja | [Management vergrendelingen](https://msdn.microsoft.com/library/azure/mt204563.aspx)<br >[Toegangsbeheer op basis van rollen](https://msdn.microsoft.com/library/azure/dn906885.aspx)  | [Resource-vergrendelen](resource-manager-template-lock.md)<br />[Roltoewijzingen](resource-manager-template-role.md)  | [Microsoft.Authorization](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Authorization%22&type=Code) |
| Resources | Ja | [Gekoppelde resources](https://msdn.microsoft.com/library/azure/mt238499.aspx) | [Resource-koppelingen](resource-manager-template-links.md) | [Microsoft.Resources](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Resources%22&type=Code) |


## <a name="resource-providers-and-types"></a>Resource-providers en typen

Wanneer u resources implementeert, moet u vaak ophalen van informatie over de resource providers en typen. U kunt deze gegevens via REST API, Azure PowerShell of Azure CLI ophalen.

Als u wilt werken met een provider voor de resource, moet deze resource-provider zijn geregistreerd bij uw account. Standaard worden automatisch diverse resource providers geregistreerd; mogelijk moet u handmatig hebt geregistreerd bij sommige providers resource. De onderstaande voorbeelden wordt getoond hoe u de registratiestatus van een resource-provider ophalen en registreren van de resource-provider, indien nodig.

### <a name="portal"></a>Portal

U kunt eenvoudig ziet een lijst met ondersteunde bronnen providers met de volgende stappen uit:

1. Selecteer **Mijn machtigingen**.

    ![Mijn machtigingen](./media/resource-manager-supported-services/my-permissions.png)

2. Selecteer **de status van de Resource-provider**

    ![status van de resource-providers](./media/resource-manager-supported-services/resource-provider-status.png)

3. U ziet de volledige lijst met providers van de resource voor uw abonnement.

    ![lijst resource providers](./media/resource-manager-supported-services/list-providers.png)

### <a name="rest-api"></a>REST API

Als u alle van de beschikbare resource gebruikt providers, inclusief hun typen, locaties, API-versies en registratiestatus, de [lijst van alle resource-providers](https://msdn.microsoft.com/library/azure/dn790524.aspx) -bewerking. Als u nodig hebt om te registreren van een resource-provider, raadpleegt u [een abonnement met een provider voor de resource registreren](https://msdn.microsoft.com/library/azure/dn790548.aspx).

### <a name="powershell"></a>PowerShell

Het volgende voorbeeld ziet u hoe u alle beschikbare resource providers.

    Get-AzureRmResourceProvider -ListAvailable
    

Het volgende voorbeeld ziet hoe u de resourcetypen voor een bepaalde resource-provider.

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes
    
De uitvoer is vergelijkbaar met:

    ResourceTypeName                Locations                                         
    ----------------                ---------                                         
    sites/extensions                {Brazil South, East Asia, East US, Japan East...} 
    sites/slots/extensions          {Brazil South, East Asia, East US, Japan East...} .
    ...
    
Als u wilt een resource-provider hebt geregistreerd, bieden de naamruimte:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement

### <a name="azure-cli"></a>Azure CLI

Het volgende voorbeeld ziet u hoe u alle beschikbare resource providers.

    azure provider list
    
De uitvoer is vergelijkbaar met:

    info:    Executing command provider list
    + Getting registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.ApiManagement          Unregistered
    data:    Microsoft.AppService             Registered
    data:    Microsoft.Authorization          Registered
    ...

U kunt de gegevens voor een bepaalde resource-provider opslaan naar een bestand met de volgende opdracht uit.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Als u wilt een resource-provider hebt geregistreerd, bieden de naamruimte:

    azure provider register -n Microsoft.ServiceBus

## <a name="supported-regions"></a>Ondersteunde regio 's

Wanneer u resources implementeert, moet u meestal een gebied voor de resources aan te geven. Resourcemanager in alle regio's wordt ondersteund, maar de bronnen die u implementeert mogelijk niet wordt ondersteund in alle regio's. Bovendien kunnen er beperkingen voor uw abonnement op die voorkomen dat u gebruikmaakt van bepaalde gebieden die ondersteuning bieden voor de resource. Deze beperkingen mogelijk zijn gerelateerd aan het btw-problemen voor uw land of het resultaat van een beleid door de beheerder van uw abonnement gebruik alleen bepaalde gebieden worden geplaatst. 

Zie voor een volledige lijst van alle ondersteunde regio's voor alle Azure-services, [Services per regio](https://azure.microsoft.com/regions/#services); Deze lijst bevatten echter regio's die uw abonnement biedt geen ondersteuning. U kunt de regio's voor een bepaald brontype die ondersteuning biedt voor uw abonnement via de portal, REST API, PowerShell of Azure CLI vaststellen.

### <a name="portal"></a>Portal

Hier ziet u de ondersteunde regio's voor een resourcetype tot en met de volgende stappen uit:

1. Selecteer **meer services** > **Resource Explorer**.

    ![resource explorer](./media/resource-manager-supported-services/select-resource-explorer.png)

2. Open het knooppunt **Providers** .

    ![providers selecteren](./media/resource-manager-supported-services/select-providers.png)

3. Selecteer een provider van de resource en de ondersteunde regio's en API-versies weergeven.

    ![Weergaveprovider](./media/resource-manager-supported-services/view-provider.png)

### <a name="rest-api"></a>REST API

Als u wilt ontdekken welke regio's zijn beschikbaar voor een bepaald brontype in uw abonnement, gebruikt u de [lijst van alle resource-providers](https://msdn.microsoft.com/library/azure/dn790524.aspx) -bewerking. 

### <a name="powershell"></a>PowerShell

Het volgende voorbeeld ziet u hoe u de ondersteunde regio's voor websites.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
    
De uitvoer is vergelijkbaar met:

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia
    Central US
    East US 2

### <a name="azure-cli"></a>Azure CLI

Het volgende voorbeeld retourneert alle ondersteunde locaties voor elk resourcetype.

    azure location list

U kunt ook de resultaten van de locatie met een hulpprogramma JSON zoals [jq](https://stedolan.github.io/jq/)filteren.

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

Welke geeft als resultaat:

    {
      "name": "Microsoft.Web/sites",
      "location": "Brazil South,East Asia,East US,Japan East,Japan West,North Central US,
            North Europe,South Central US,West Europe,West US,Southeast Asia,Central US,East US 2"
    }

## <a name="supported-api-versions"></a>Ondersteunde API-versies

Wanneer u een sjabloon implementeert, moet u een versie API moet worden gebruikt voor het maken van elke resource. De API-versie overeenkomt met een versie van de REST API-bewerkingen die zijn uitgebracht door de provider van de resource. Als de provider van een resource kunt nieuwe functies, geeft dit een nieuwe versie van de REST API. De versie van de API die u in de sjabloon opgeeft is dus van invloed op welke eigenschappen kunt u in de sjabloon. U wilt in het algemeen, selecteert u de meest recente versie van de API bij het maken van sjablonen. Voor bestaande sjablonen, kunt u bepalen of u wilt doorgaan met een eerdere versie van de API of bijwerken van uw sjabloon voor de meest recente versie om te profiteren van nieuwe functies.

### <a name="portal"></a>Portal

U kunt de ondersteunde API-versies vaststellen op dezelfde manier die u bepaald ondersteunde regio's (eerder weergegeven).

### <a name="rest-api"></a>REST API

Als u wilt ontdekken welke API-versies zijn beschikbaar voor de resourcetypen, gebruikt u de [lijst van alle resource-providers](https://msdn.microsoft.com/library/azure/dn790524.aspx) -bewerking. 

### <a name="powershell"></a>PowerShell

Het volgende voorbeeld ziet u hoe u de beschikbare API-versies voor een bepaald brontype.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
    
De uitvoer is vergelijkbaar met:
    
    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

### <a name="azure-cli"></a>Azure CLI

U kunt de gegevens (inclusief de beschikbare API-versies) opslaan voor een resource-provider naar een bestand met de volgende opdracht uit.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

U kunt het bestand openen en de **apiVersions** -element

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het maken van sjablonen van de Resource Manager, [Azure resourcemanager Authoring sjablonen](resource-group-authoring-templates.md).
- Zie voor meer informatie over het implementeren van resources, [Deploy een toepassing met Azure resourcemanager sjabloon](resource-group-template-deploy.md).
