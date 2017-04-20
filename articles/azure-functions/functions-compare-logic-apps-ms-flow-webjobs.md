<properties
    pageTitle="Kiezen tussen stroom, logica Apps, functies en WebJobs | Microsoft Azure"
    description="Vergelijken en het contrast de cloud integration services van Microsoft en voor bepalen welke service (s) die u moet gebruiken."
    services="functions,app-service\logic"
    documentationCenter="na"
    authors="cephalin"
    manager="wpickett"
    tags=""
    keywords="Microsoft-mailstroom, stroom, logica apps, azure functies, functies, azure webjobs, webjobs, gebeurtenis processing, dynamische berekeningscluster, als u kiest architectuur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="chrande; glenga"/>

# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Kiezen tussen stroom, logica Apps, functies en WebJobs

In dit artikel worden vergeleken en verschilt van de volgende services in de cloud Microsoft, waarin u kunt alle integratie problemen en automatisering van bedrijfsprocessen oplossen:

- [Microsoft-mailstroom](https://flow.microsoft.com/)
- [Azure logica Apps](https://azure.microsoft.com/services/logic-apps/)
- [Azure-functies](https://azure.microsoft.com/services/functions/)
- [Azure App Service WebJobs](../app-service-web/web-sites-create-web-jobs.md)

Al deze services zijn handig als 'lijmen' samen verschillende systemen. Ze kunnen alle definiëren invoer, acties, voorwaarden en uitvoer. U kunt elk van deze uitvoeren op een planning of trigger. Echter elke service een unieke set in een hoofdletter wordt toegevoegd, en deze vergelijken is niet een vraag "welke service is de beste?" maar een van de "welke service aanbevolen is geschikt voor deze situatie?" Een combinatie van de volgende services is vaak de beste manier om snel een oplossing scalable, volledige aanbevolen integratie maken.

<a name="flow"></a>
## <a name="flow-vs-logic-apps"></a>Stroom versus logica Apps

We kunt bespreken Flow Microsoft Azure logica Apps samen omdat ze beide *configuratie-eerste* integratieservices, waardoor u deze gemakkelijk te bouwen van processen en werkstromen en integreren met verschillende SaaS en enterprise-toepassingen. 

- Stroom is gebouwd logica Apps
- Ze hebben de dezelfde werkstroomontwerper
- [Verbindingslijnen](../connectors/apis-list.md) die werken in een kunt ook met de andere werken

Loopt kunnen elke werknemer office naar eenvoudige integraties uitvoeren (bijvoorbeeld SMS krijgen voor belangrijke e-mailberichten) zonder tussenkomst ontwikkelaars of IT. Logica Apps kunt aan de andere kant, geavanceerde of belangrijke integraties (bijvoorbeeld B2B processen) waar de procedures DevOps en beveiliging op gebruikersniveau enterprise vereist zijn. Het is typisch voor een zakelijke werkstroom om te laten groeien in complexiteit overuren. Dienovereenkomstig gewijzigd, kunt u beginnen met een stroom bij de eerste en vervolgens converteren naar een app logica naar wens.

De volgende tabel kunt u bepalen of stroom of logica Apps aanbevolen voor een bepaald integratie is.

|               | Stroom                                                                             | Logica Apps                                                                                          |
|---------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Publiek      | Office werknemers, zakelijke gebruikers                                                   | IT-professionals, ontwikkelaars                                                                                 |
| Scenario 's     | Zelf te kunnen                                                                     | Belangrijke                                                                                    |
| Hulpmiddel voor websiteontwerp   | Browser, alleen UI                                                              | In de browser en [Visual Studio](../app-service/logic/app-service-logic-deploy-from-vs.md), [codeweergave](../app-service-logic/app-service-logic-author-definitions.md) beschikbaar |
| DevOps        | Ad-hoc, ontwikkelen in productie                                                    | besturingselement voor gegevensbronnen, testen, ondersteuning en automatisering en beheerbaarheid in [Azure resourcebeheer](../app-service-logic/app-service-logic-arm-provision.md)|
| Beheerder-ervaring| [https://flow.Microsoft.com](https://flow.microsoft.com)                       | [https://Portal.Azure.com](https://portal.azure.com)                                                |
| Beveiliging      | Standaardprocedures: [gegevens te garanderen](https://wikipedia.org/wiki/Technological_Sovereignty), [versleuteling in rust](https://wikipedia.org/wiki/Data_at_rest#Encryption) voor gevoelige gegevens, enzovoort. | Beveiliging assurance van Azure: [Azure beveiliging](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Beveiligingscentrum](https://azure.microsoft.com/services/security-center/) [controlelogboeken](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/)en meer. |

<a name="function"></a>
## <a name="functions-vs-webjobs"></a>Functies versus WebJobs

We kunnen bespreken Azure-functies en Azure App Service WebJobs samen omdat ze beide *code eerst* integratieservices en ontworpen voor ontwikkelaars. Hiermee kunt u een script of een deel van een code uitvoeren in antwoord op verschillende gebeurtenissen, zoals [Nieuwe opslag BLOB's](functions-bindings-storage.md) of [een WebHook aanvragen](functions-bindings-http-webhook.md). Hier volgen de overeenkomsten: 

- Beide zijn gebaseerd op [Azure App-Service](../app-service/app-service-value-prop-what-is.md) en profiteren van functies zoals [besturingselement voor gegevensbronnen](../app-service-web/app-service-continuous-deployment.md), [verificatie](../app-service/app-service-authentication-overview.md)en [controle](../app-service-web/web-sites-monitor.md).
- Beide zijn services developer gerichte.
- Zowel standaard uitvoeren van scripts en programmeertaal ondersteunen.
- Beide hebben NuGet en NPM ondersteunen.

Functies is de natuurlijke ontwikkeling van WebJobs in dat deze duurt het mooie van WebJobs en hen verbetert. De verbeteringen omvatten: 

- Gestroomlijnde ontwikkelaar, testen en uitvoeren van code, rechtstreeks in de browser.
- Ingebouwde integratie met meer Azure services en 3e derden services zoals [GitHub WebHooks](https://developer.github.com/webhooks/creating/).
- Betalen per gebruik, hoeft te betalen voor een [App-Service plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
- Automatische, [dynamische schaalbaarheid](functions-scale.md).
- Voor bestaande klanten van App-Service, uitgevoerd op de App serviceplan nog steeds mogelijk is (om te profiteren van resources onder gebruikt).
- Integratie met logica Apps.

De volgende tabel bevat een overzicht van de verschillen tussen functies en WebJobs:

|                        | Functies                                                                                                                                                                | WebJobs                            |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| Schaalbaarheid                | Configurationless schaalbaarheid                                                                                                                                                | schalen met App-abonnement        |
| Prijzen                | Betalen per gebruik of een deel van de App-abonnement                                                                                                                                  | Deel van de App-abonnement           |
| Uitvoeren van tekst               | geactiveerd, gepland (door timer trigger)                                                                                                                                  | geactiveerde, continue, geplande   |
| Trigger gebeurtenissen         | [timer](functions-bindings-timer.md), [Azure DocumentDB](functions-bindings-documentdb.md), [Azure gebeurtenis Hubs](functions-bindings-event-hubs), [HTTP/WebHook (GitHub, marge)](functions-bindings-http-webhook.md), [Azure App Service Mobile-Apps](functions-bindings-mobile-apps.md), [Azure melding Hubs](functions-bindings-notification-hubs.md), [Azure Service Bus](functions-bindings-service-bus.md), [Azure Storage](articles/functions-bindings-storage.md) | [Azure opslag](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), [Azure Service Bus](websites-dotnet-webjobs-sdk-service-bus.md)         |
| Ontwikkeling van de browser | x                                                                                                                                                                        |                                    |
| Venster uitvoeren van scripts       | experiment                                                                                                                                                             | x                                  |
| PowerShell             | experiment                                                                                                                                                             | x                                  |
| C#                     | x                                                                                                                                                                        | x                                  |
| F#                     | x                                                                                                                                                                        |                                    |
| We vaker doen                   | experiment                                                                                                                                                             | x                                  |
| PHP                    | experiment                                                                                                                                                             | x                                  |
| Python                 | experiment                                                                                                                                                             | x                                  |
| JavaScript             | x                                                                                                                                                                        | x                                  |
| Java                   | experiment                                                                                                                                                             | x                                  |

Tussen functies of WebJobs uiteindelijk afhankelijk van wat u al met App-Service doet. Als u een App Service-app waaraan u wilt uitvoeren van codefragmenten hebt en u wilt beheren ze samen in dezelfde DevOps omgeving, kunt u WebJobs moet gebruiken. Als u wilt uitvoeren codefragmenten voor andere Azure services of zelfs 3e derden apps, of als u wilt uw codefragmenten integratie afzonderlijk beheren vanuit uw App Service-apps, of als u wilt uw codefragmenten bellen via een logica-app, moet u profiteren van alle verbeteringen in functies.  

<a name="together"></a>
## <a name="flow-logic-apps-and-functions-together"></a>Flow, logica Apps en functies samen

Zoals eerder vermeld, welke service is het meest geschikt is voor u is afhankelijk van uw situatie. 

- Eenvoudige bedrijven, kunnen worden gebruikt stroom.
- Als uw integratiescenario ook voor mailstroom geavanceerde is of hebt u DevOps mogelijkheden en beveiliging conformiteit nodig, klikt u vervolgens logica-Apps te gebruiken.
- Als een stap in uw integratiescenario ten zeerste aangepaste transformatie of speciale code vereist, noteert u een functie-app en klikt u vervolgens een functie activeren als een actie in uw app logica.

U kunt een app logica bellen in een stroom. U kunt ook een functie in een app logica en een logica-app in een functie bellen. De integratie tussen stroom, logica Apps en functies blijven overuren te verbeteren. U kunt iets in één service maken en gebruiken in de andere services. Een investering die u in deze drie technologieën aanbrengt dus handig.

## <a name="next-steps"></a>Volgende stappen

Aan de slag met elk van de services door te maken van uw eerste stroom, logica app, functie app of WebJob. Klik op een van de volgende koppelingen:

- [Aan de slag met Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
- [Een logica-app maken](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Maak uw eerste Azure-functie](../azure-functions/functions-create-first-azure-function.md)
- [Gebruik van Visual Studio WebJobs implementeren](../app-service-web/websites-dotnet-deploy-webjobs.md)

Of lees meer over deze integratieservices met de volgende koppelingen:

- [Gebruikmaken van Azure-functies en Azure App-Service voor integratie van scenario's door Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
- [Eenvoudige aangebracht door Jeroen Lamanna integraties](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
- [Logica Apps Live Webcast](http://aka.ms/logicappslive)
- [Microsoft Flow Veelgestelde vragen](https://flow.microsoft.com/documentation/frequently-asked-questions/)
- [Azure WebJobs documentatie resources](../app-service-web/websites-webjobs-resources.md)
