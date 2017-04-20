<properties
   pageTitle="Scalable webtoepassing | Azure verwijzing architectuur | Microsoft Azure"
   description="Verbeteren van de schaalbaarheid in een webtoepassing met Microsoft Azure."
   services="app-service,app-service\web,sql-database"
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/12/2016"
   ms.author="mwasson"/>


# <a name="improving-scalability-in-a-web-application"></a>Verbeteren van de schaalbaarheid in een webtoepassing 

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In dit artikel leest een aanbevolen architectuur voor het verbeteren van de schaalbaarheid en prestaties in een webtoepassing uitgevoerd op Microsoft Azure. De architectuur die is gebaseerd op [Azure verwijzing architectuur: eenvoudige webtoepassing][basic-web-app]. De aanbevelingen en overwegingen van dit artikel zijn van toepassing op deze architectuur ook.

>[AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: resourcemanager en klassiek. In dit artikel worden de resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties gebruikt.

## <a name="architecture-diagram"></a>Architectuurdiagram

![[0]][0]

De architectuur heeft de volgende onderdelen:

- **Resourceveld groep**. Een [resourcegroep] [ resource-group] is een logische container voor Azure resources. 

- ** [WebApp] [ app-service-web-app] ** en ** [API app][app-service-api-app]**. Een typische moderne toepassing kan zowel een website en een of meer RESTful web API's bevatten. Een web API kan worden gebruikt door browserclients via AJAX, door systeemeigen clienttoepassingen, of door aan de clientzijde toepassingen. Zie [API ontwerp richtlijnen]voor overwegingen op ontwerpen web API's,[api-guidance].    

- **WebJob**. Gebruik van [Azure WebJobs] [ webjobs] langdurige taken uitvoeren in de achtergrond. WebJobs kan worden uitgevoerd op een planning, continu, of in antwoord op een trigger, zoals een bericht in een wachtrij plaatsen. Een WebJob wordt uitgevoerd als een achtergrondproces in de context van een App Service-app. 

- **Wachtrij**. In de architectuur zoals hier wordt weergegeven, de toepassingswachtrijen achtergrondtaken door te plaatsen van een bericht naar een [Azure wachtrij Storage] [ queue-storage] wachtrij. Het bericht wordt een functie in de WebJob geactiveerd. 

    U kunt ook Service Bus wachtrijen gebruiken. Zie voor een vergelijking [Azure wachtrijen en Service Bus wachtrijen - vergeleken en in afgezet tegen][queues-compared].

- **Cache**. Semi-statische gegevens opslaan in de [Cache van Azure bestand Vgx.][azure-redis].  

- **CDN**. [Azure inhoud bezorging Network] gebruiken[ azure-cdn] (CDN) cache openbaar inhoud voor lagere latentie en snellere levering van inhoud.

- **Gegevensopslag.** Gebruik van [Azure SQL-Database] [ sql-db] voor relationele gegevens. Voor niet-relationele gegevens, kunt u overwegen een NoSQL winkel, zoals Azure Table Storage of [DocumentDB][documentdb].

- **Azure zoeken**. Gebruik de [Zoekfunctie van Azure] [ azure-search] om toe te voegen zoekfunctionaliteit, inclusief zoeksuggesties, bij benadering zoeken en taalspecifieke zoeken. Azure zoeken wordt meestal gebruikt in combinatie met een ander gegevensopslag, vooral als de primaire gegevensopslag strikte consistentie is vereist. In deze benadering de gezaghebbende gegevens in de andere gegevensopslag toe en de zoekindex besteedt Azure zoeken. Azure zoeken kan ook worden gebruikt om samen te voegen een enkel zoekindex van meerdere gegevens winkels.  

- **E-mailbericht/SMS**. Als uw toepassing e-mail of SMS-berichten verzenden moet, gebruikt u een derde partij service zoals SendGrid of Twilio, in plaats van deze functionaliteit rechtstreeks in de toepassing bouwen.

## <a name="recommendations"></a>Aanbevelingen

### <a name="app-service-apps"></a>App-Service-apps 

Het is raadzaam om het maken van de webtoepassing en de web-API als afzonderlijke App Service-apps. Deze ontwerpen kunt u ze worden uitgevoerd in afzonderlijke App Service-abonnementen, waarmee u schalen dat ze onafhankelijk beurtelings kunt. Als u niet nodig hebt dat niveau van schaalbaarheid bij eerst, kunt u de apps implementeren in hetzelfde plan, en deze verplaatsen in afzonderlijke plannen later, indien nodig. (Voor de abonnementen Basic, Standard en Premium voor u zijn gefactureerd voor de exemplaren VM in het abonnement, niet per app. Zie [App Service prijzen][app-service-pricing])

Als u van plan bent *Eenvoudig tabellen* of *Eenvoudig API's* functies van App-Service Mobile-Apps te gebruiken, moet u een aparte App Service-app maken voor dit doel.  Deze functies zijn afhankelijk van een specifieke toepassing framework pstn_conferencing wilt inschakelen.

### <a name="webjobs"></a>WebJobs

Als de WebJob veel bronnen is, kunt u deze naar een lege App Service-app in een afzonderlijk App Service-abonnement speciale exemplaren verstrekken voor de WebJob implementeren. Zie [achtergrond taken richtlijnen]voor het[webjobs-guidance].  

### <a name="cache"></a>Cache

U kunt de prestaties en schaalbaarheid verbeteren met behulp van [Azure bestand Vgx. Cache] [ azure-redis] om bepaalde gegevens in cache. Bestand Vgx. Cache voor u overwegen:

- Semi-statische transactiegegevens.

- De status van de sessie.

- HTML-uitvoer. Dit is handig in toepassingen die complexe HTML-uitvoer weergegeven. 

Voor meer richtlijnen voor het ontwerpen van een in de cache strategie, raadpleegt u [Caching van richtlijnen][caching-guidance].

### <a name="cdn"></a>CDN 

Gebruik van [Azure CDN] [ azure-cdn] naar statische inhoud cache. De belangrijkste voordelen van een CDN is verkleinen van latentie voor gebruikers, omdat inhoud wordt opgeslagen in de cache op een *randserver* die geografisch lijkt op de gebruiker. CDN kunt belasting van de toepassing, ook verkleinen, omdat dit verkeer door de toepassing niet wordt verwerkt.

- Als uw app voornamelijk statische pagina's bevat, kunt u overwegen CDN om de hele-app in cache. Zie [Azure CDN in Azure App-Service gebruiken][cdn-app-service].

- Anders bestanden statische inhoud, zoals afbeeldingen, CSS en HTML-, in Azure Storage en gebruik CDN naar deze bestanden in cache. Zie [een Account opslagruimte met CDN integreren][cdn-storage-account].

> [AZURE.NOTE] Azure CDN kan geen inhoud die is verificatie vereist fungeren.

Voor meer informatie vinden, raadpleegt u [inhoud bezorging netwerk (CDN) richtlijnen][cdn-guidance]. 

### <a name="storage"></a>Opslag

Moderne toepassingen verwerken vaak grote hoeveelheden gegevens. Als u wilt schalen voor de cloud, is het belangrijk om de juiste opslagtype te kiezen. Hier volgen enkele aanbevelingen volgens de basislijn.  Voor meer informatie vinden, raadpleegt u [Beoordeling van gegevens Store mogelijkheden voor Polyglot permanente oplossingen][polyglot-storage].

Wat u wilt opslaan | Voorbeeld | Aanbevolen opslag
--- | --- | ---
Bestanden | Afbeeldingen, documenten, PDF-bestanden | Azure-blobopslag
Sleutel/waardeparen | Gebruikersprofielgegevens opgezocht met gebruikers-ID | Azure-tabelopslag
Korte berichten die zijn bedoeld om te activeren nadere verwerking | Serviceaanvragen | Azure wachtrij opslag, Service Bus wachtrij of Service Bus onderwerp
Niet-relationele gegevens met een flexibele schema, vereisen van eenvoudige query's uitvoeren | Productcatalogus | Document-database, zoals Azure DocumentDB, MongoDB of Apache CouchDB
Relationele gegevens, rijkere Queryondersteuning, strikte schema en/of sterke consistentie vereisen | Productvoorraad | Azure SQL-Database

## <a name="scalability-considerations"></a>Aandachtspunten voor de schaalbaarheid

### <a name="app-service-app"></a>App-Service-app

Als uw oplossing verschillende App Service-apps bevat, kunt u ze om te scheiden van App-Service-abonnementen. Deze methode kunt u schalen dat ze onafhankelijk, omdat ze op afzonderlijke exemplaren. Zie voor meer informatie over het schalen, de [schaalbaarheid overwegingen] [ basic-web-app-scalability] sectie in de [Microsoft-toepassingsarchitectuur][basic-web-app].

Op dezelfde manier te plaatsen van een WebJob in een eigen-indeling, zodat achtergrondtaken niet uitvoeren op de dezelfde exemplaren die HTTP-aanvragen verwerken.  

### <a name="sql-database"></a>SQL-Database

Schaalbaarheid van een SQL-database verhogen met *sharding* de database &mdash; dat wil zeggen, horizontaal partitioneren de database. Sharding kunt u horizontaal schaal van de database met behulp [elastische Databasehulpmiddelen][sql-elastic]. Mogelijke voordelen van sharding zijn:

- Betere transactiedoorvoer.

- Query's kunnen sneller via een subset van de gegevens worden uitgevoerd. 

### <a name="azure-search"></a>Azure zoeken

Azure zoeken Hiermee verwijdert u de realiseren van complexe gegevens zoekopdrachten uitvoeren van de primaire gegevensopslag en deze kunt schalen om af te handelen laden. Zie [schaal resource niveaus voor de query en indexing werkbelasting in Azure zoeken][azure-search-scaling].

## <a name="security-considerations"></a>Veiligheidsoverwegingen

### <a name="cross-origin-resource-sharing-cors"></a>Cross-Origin Resource delen (CORS)

Als u een website- en web API als afzonderlijke apps maakt, kan de website aan de clientzijde AJAX aanroepen tot de API kan doen tenzij u CORS inschakelen. 

> [AZURE.NOTE] Browserbeveiliging wordt voorkomen dat een pagina met webonderdelen AJAX-aanvragen naar een ander domein aanbrengen. Deze beperking heet het beleid dezelfde oorsprong, en voorkomen dat een schadelijke site het lezen van upnaam gegevens van een andere site. CORS is een W3C-standaard waarmee een server het beleid dezelfde oorsprong Ontspanning en te zorgen dat bepaalde cross-origin aanvragen terwijl de anderen te negeren.

App-Services heeft ingebouwde ondersteuning voor CORS, zonder toepassingscode te schrijven. Zie [verbruiken een API-app in JavaScript met CORS][cors]. De website toevoegen aan de lijst met toegestane oorspronkelijke bestanden voor de API. 

### <a name="sql-database-encryption"></a>SQL-Database versleutelen

[Transparante gegevens] versleuteling[ sql-encryption] als u nodig hebt om gegevens in rust in de database te versleutelen. Deze functie kunt u realtime coderen en decoderen van een volledige database (waaronder back-ups en transactie-logbestanden), zonder wijzigingen in de toepassing uitvoeren. Versleuteling enkele latentie, toevoegen, zodat het is een goede gewoonte om te scheiden van de gegevens die moeten worden beveiligde in een eigen database en versleuteling alleen voor die database inschakelen.  

## <a name="next-steps"></a>Volgende stappen

- Implementeren van de toepassing in meer dan één regio en [Azure verkeer Manager] gebruiken voor een betere beschikbaarheid[ tm] voor failover. Zie voor meer informatie [Azure verwijzing architectuur: webtoepassing met hoge beschikbaarheid][web-app-multi-region].    

<!-- links -->

[api-guidance]: ../best-practices-api-design.md
[app-service-web-app]: ../app-service-web/app-service-web-overview.md
[app-service-api-app]: ../app-service-api/app-service-api-apps-why-best-platform.md
[app-service-pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[azure-cdn]: https://azure.microsoft.com/en-us/services/cdn/
[azure-redis]: https://azure.microsoft.com/en-us/services/cache/
[azure-search]: https://azure.microsoft.com/en-us/documentation/services/search/
[azure-search-scaling]: ../search/search-capacity-planning.md
[background-jobs]: ../best-practices-background-jobs.md
[basic-web-app]: guidance-web-apps-basic.md
[basic-web-app-scalability]: guidance-web-apps-basic.md#scalability-considerations 
[caching-guidance]: ../best-practices-caching.md
[cdn-app-service]: ../app-service-web/cdn-websites-with-cdn.md
[cdn-storage-account]: ../cdn/cdn-create-a-storage-account-with-cdn.md
[cdn-guidance]: ../best-practices-cdn.md
[cors]: ../app-service-api/app-service-api-cors-consume-javascript.md
[documentdb]: https://azure.microsoft.com/en-us/documentation/services/documentdb/
[polyglot-storage]: https://github.com/mspnp/azure-guidance/blob/master/Polyglot-Solutions.md
[queue-storage]: ../storage/storage-dotnet-how-to-use-queues.md
[queues-compared]: ../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md
[resource-group]: ../resource-group-overview.md
[sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[sql-elastic]: ../sql-database/sql-database-elastic-scale-introduction.md
[sql-encryption]: https://msdn.microsoft.com/en-us/library/dn948096.aspx
[tm]: https://azure.microsoft.com/en-us/services/traffic-manager/
[web-app-multi-region]: ./guidance-web-apps-multi-region.md
[webjobs-guidance]: ../best-practices-background-jobs.md
[webjobs]: ../app-service/app-service-webjobs-readme.md
[0]: ./media/blueprints/paas-web-scalability.png "Webtoepassing in Azure wordt aangegeven met verbeterde schaalbaarheid"