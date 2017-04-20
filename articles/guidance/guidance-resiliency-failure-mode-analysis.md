<properties
   pageTitle="Fout bij modus analyse | Microsoft Azure"
   description="Richtlijnen voor het uitvoeren van mislukt modus analyses voor cloud-oplossingen op basis van Azure."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="mwasson"/>

# <a name="azure-resiliency-guidance-failure-mode-analysis"></a>Azure tolerantie richtlijnen: analyse op modus

Fout bij modus analyse (FMA) is een proces voor het samenstellen van tolerantie in een systeem, door te identificeren mogelijke fout wordt verwezen in het systeem. De FMA moeten deel uitmaken van de architectuur en een ontwerp fasen, zodat u foutherstel in het systeem vanaf het begin maken kunt.

Dit is het algemene proces voor het uitvoeren van een FMA: 

1. Geef alle onderdelen in het systeem. Externe afhankelijkheden, zoals opnemen als identiteitsprovider, externe services, enzovoort.   

2. Geef voor elk onderdeel, mogelijke fouten die kunnen optreden. Één onderdeel wellicht meer dan één mislukt-modus. U moet bijvoorbeeld kunt u lezen fouten en fouten afzonderlijk schrijven omdat de impact en mogelijke oplossingen afwijken.

3. Iedere modus mislukt op basis van de algehele risico's beoordelen. Deze factoren overwegen:  

    - Wat is de kans van de fout. Is het relatief algemene? Extrememly niet vaak voorkomen? U hoeft geen exacte getallen. het doel is om te helpen rangschikken van de prioriteit.

    - Wat is de invloed op de toepassing is en wat betreft beschikbaarheid, verlies van gegevens, monetaire kosten en bedrijven verstoringen? 

4. Bepalen hoe de toepassing wordt beantwoorden en herstellen voor iedere modus is mislukt. Houd rekening met compromissen nodig zijn in kosten- en complexiteit.   

Als uitgangspunt voor uw FMA-proces bevat in dit artikel een catalogus van mogelijke fout modi en hun beperkingen. De catalogus is geordend op technologie of Azure-service, plus een categorie Algemeen voor ontwerp van het niveau van de webtoepassing. De catalogus is niet volledig, maar veel van de core Azure behandelt services. 


## <a name="app-service"></a>App-Service

### <a name="app-service-app-shuts-down"></a>App Service app is afgesloten.

**Detectie**. Mogelijke oorzaken:

- Verwachte afsluiting
    
    - Een operator is afgesloten van de toepassing niet omzeilen. bijvoorbeeld, met behulp van de Azure portal.

    - De app is verwijderd omdat deze niet actief is. (Alleen als de `Always On` instelling is uitgeschakeld.)

- Onverwachte afsluiten

    - De app loopt vast.

    - Een exemplaar van de App Service VM niet meer beschikbaar.


Application_End logboekregistratie de app-afsluiting van het domein (vloeiende proces vastlopen) wordt onderschept en is de enige manier om de toepassing domein afsluitingen onderschept.

**Herstel**

- Als de afsluiting verwacht, gebruikt u de gebeurtenis afsluiten van de toepassing afsluiten. Bijvoorbeeld in ASP.NET, gebruikt u de `Application_End` methode.

- Als de toepassing is verwijderd terwijl niet actief is, wordt deze automatisch opnieuw gestart op de volgende aanvraag. U wordt echter in rekening gebracht het 'koudwatersystemen starten"kosten. 

- Als u wilt voorkomen dat de toepassing wordt niet geladen terwijl niet actief geweest, inschakelen de `Always On` instellen in de web-app. Zie [web-apps in Azure App-Service configureren][app-service-configure].

- Als u wilt voorkomen dat een operator als u de app afsluit, stel een resource vergrendelen met `ReadOnly` niveau. Zie [vergrendelingsbronnen met Azure resourcemanager][rm-locks].

- Als de app vastloopt of een App-Service VM niet meer beschikbaar, App Service opgestart de app. 


**Diagnostische hulpprogramma's**. Toepassing Logboeken en web serverlogboeken. Zie [Diagnostische gegevens vastleggen voor web-apps in Azure App-Service inschakelen][app-service-logging].


### <a name="a-particular-user-repeatedly-makes-bad-requests-or-overloads-the-system"></a>Een bepaalde gebruiker herhaaldelijk zorgt ervoor dat ongeldige aanvragen of overloads het systeem. 

**Detectie**. Verificatie van gebruikers en gebruikers-ID in de toepassingslogboeken bevatten.

**Herstel**

- Gebruik van [Azure API Management] [ api-management] naar vertraging aanmeldingsaanvragen van de gebruiker. Zie [Geavanceerde aanvraag beperken met Azure API Management.][api-management-throttling]

- De gebruiker blokkeert.

**Diagnostische hulpprogramma's**. Meld u aan alle verificatieaanvragen.


### <a name="a-bad-update-was-deployed"></a>Een ongeldige update is geïmplementeerd.

**Detectie**. De toepassingsstatus via de Portal Azure controleren (Zie [Monitor Azure web app prestaties][app-insights-web-apps]) of de [systeemstatus eindpunt controleren patroon]implementeert[health-endpoint-monitoring-pattern].

**Herstel**. Gebruik van meerdere [implementatie sleuven] [ app-service-slots] en terugkeren naar de laatste bekende goede-implementatie. Zie voor meer informatie, [eenvoudige web application verwijzing architectuur][ra-web-apps-basic].


## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="openid-connect-oidc-authentication-fails"></a>OpenID verbinding (OIDC)-verificatie mislukt.

**Detectie**. Mogelijke fout modi opnemen:

1. Azure AD is niet beschikbaar of kan niet worden bereikt vanwege een netwerkprobleem. Omleiding naar het eindpunt verificatie mislukt en de middleware OIDC genereert een uitzondering.
2. Azure AD-tenant bestaat niet. Omleiding naar het eindpunt verificatie geeft als resultaat een HTTP-foutcode en de middleware OIDC genereert een uitzondering.
3. Gebruiker verifiëren niet. Geen strategie detectie is noodzakelijk; Azure AD verwerkt login fouten.

**Herstel**

1. Variabel onverwerkte uitzonderingen uit de middleware.
2. Verwerken `AuthenticationFailed` gebeurtenissen.
3. De gebruiker omgeleid naar de foutpagina van een.
4. Nieuwe pogingen gebruiker.


## <a name="azure-search"></a>Azure zoeken

### <a name="writing-data-to-azure-search-fails"></a>Schrijven gegevens naar Azure zoeken is mislukt.

**Detectie**. Variabel `Microsoft.Rest.Azure.CloudException` fouten.

**Herstel**

De [Zoekopdracht .NET SDK] [ search-sdk] automatisch pogingen na tijdelijke fouten. Eventuele uitzonderingen door de client SDK moeten worden behandeld als tijdelijke fouten.

Het standaardbeleid voor opnieuw gebruikt exponentiële back-uitschakelen. Als u wilt gebruiken in een andere opnieuw beleid, bellen `SetRetryPolicy` op de `SearchIndexClient` of `SearchServiceClient` class. Zie voor meer informatie, [Automatische nieuwe pogingen][auto-rest-client-retry].

**Diagnostische hulpprogramma's**. Gebruik [Zoeken verkeer Analytics][search-analytics].


### <a name="reading-data-from-azure-search-fails"></a>Het lezen van gegevens van Azure zoekresultaten mislukt.

**Detectie**. Variabel `Microsoft.Rest.Azure.CloudException` fouten.

**Herstel**

De [Zoekopdracht .NET SDK] [ search-sdk] automatisch pogingen na tijdelijke fouten. Eventuele uitzonderingen door de client SDK moeten worden behandeld als tijdelijke fouten.

Het standaardbeleid voor opnieuw gebruikt exponentiële back-uitschakelen. Als u wilt gebruiken in een andere opnieuw beleid, bellen `SetRetryPolicy` op de `SearchIndexClient` of `SearchServiceClient` class. Zie voor meer informatie, [Automatische nieuwe pogingen][auto-rest-client-retry].

**Diagnostische hulpprogramma's**. Gebruik [Zoeken verkeer Analytics][search-analytics].



## <a name="cassandra"></a>Cassandra


### <a name="reading-or-writing-to-a-node-fails"></a>Lezen of schrijven naar een knooppunt mislukt.

**Detectie**. De uitzondering onderschept. Voor .NET-clients, dit meestal is `System.Web.HttpException`. Andere client mogelijk andere typen uitzondering.  Zie [Cassandra foutafhandeling rechts klaar](http://www.datastax.com/dev/blog/cassandra-error-handling-done-right)voor meer informatie.

**Herstel**

- Elke [Cassandra client](https://wiki.apache.org/cassandra/ClientOptions) heeft een eigen opnieuw beleidsregels en mogelijkheden. Zie voor meer informatie, [Cassandra foutafhandeling rechts klaar][cassandra-error-handling].
- Gebruik een rek-hoogte implementatie, met gegevensknooppunten verdeeld over de domeinen van de fout.
- Dashboard implementeren naar meerdere gebieden met Lokaal quorum consistentie. Als een tijdelijke een fout optreedt, kunt u niet naar een andere regio.

**Diagnostische hulpprogramma's**. Toepassingslogboeken aan de


## <a name="cloud-service"></a>Cloudservice

### <a name="web-or-worker-roles-are-unexpectedlybeing-shut-down"></a>Web of werknemer rollen zijn onverwacht wordt afgesloten.

**Detectie**. De [RoleEnvironment.Stopping] [ RoleEnvironment.Stopping] gebeurtenis wordt gestart.

**Herstel**. De [RoleEntryPoint.OnStop] overschrijven[ RoleEntryPoint.OnStop] methode om op te schonen zonder problemen. Voor meer informatie raadpleegt u [De rechts-manier om te verwerken Azure OnStop gebeurtenissen] [ onstop-events] (blog).


## <a name="documentdb"></a>DocumentDB

### <a name="reading-data-from-documentdb-fails"></a>Het lezen van gegevens uit DocumentDB mislukt.

**Detectie**. Variabel `System.Net.Http.HttpRequestException` of `Microsoft.Azure.Documents.DocumentClientException`. 

**Herstel**

- De SDK pogingen automatisch mislukte pogingen. Het aantal pogingen en de maximale wachttijd stelt configureren `ConnectionPolicy.RetryOptions`. Uitzonderingen dat de client bevinden zich buiten het beleid opnieuw of zijn geen tijdelijke fouten. 

- Als DocumentDB de client bandbreedte, wordt een fout HTTP 429 geretourneerd. De statuscode inchecken de `DocumentClientException`. Als u fout 429 consistente krijgt, kunt u de doorvoercapaciteit van de verzameling DocumentDB vergroten.

- De database DocumentDB repliceren in twee of meer regio's. Alle replica's kunnen worden gelezen. Met de client SDK's, geef de `PreferredLocations` parameter. Dit is een geordende lijst van Azure regio's. Alle lees worden verzonden naar de eerste beschikbare regio in de lijst. Als het verzoek mislukt, probeert de client de andere regio's in de lijst in volgorde. Zie voor meer informatie [ontwikkelen met accounts voor meerdere landen/regio DocumentDB][docdb-multi-region].

**Diagnostische hulpprogramma's**. Meld u aan alle fouten op de client.


### <a name="writing-data-to-documentdb-fails"></a>Het schrijven van gegevens naar DocumentDB mislukt.

**Detectie**. Variabel `System.Net.Http.HttpRequestException` of `Microsoft.Azure.Documents.DocumentClientException`. 

**Herstel**

- De SDK pogingen automatisch mislukte pogingen. Het aantal pogingen en de maximale wachttijd stelt configureren `ConnectionPolicy.RetryOptions`. Uitzonderingen dat de client bevinden zich buiten het beleid opnieuw of zijn niet tijdelijke fouten. 

- Als DocumentDB de client bandbreedte, wordt een fout HTTP 429 geretourneerd. De statuscode inchecken de `DocumentClientException`. Als u fout 429 consistente krijgt, kunt u de doorvoercapaciteit van de verzameling DocumentDB vergroten.

- De database DocumentDB repliceren in twee of meer regio's. Als de primaire regio mislukt, een andere regio zullen worden gepromoveerd tot schrijven. U kunt ook een failover handmatig activeren. De SDK biedt automatische detectie en routering, zodat de toepassingscode nog steeds werkt nadat failover is uitgevoerd. Tijdens de failover-periode (meestal minuten) heeft schrijven bewerkingen hoger latentie, als de SDK vindt u de nieuwe schrijven regio. Zie voor meer informatie [ontwikkelen met accounts voor meerdere landen/regio DocumentDB][docdb-multi-region].

- Het document naar een back-wachtrij persistent terug te vallen, en de wachtrij later verwerken.

**Diagnostische hulpprogramma's**. Meld u aan alle fouten op de client.


## <a name="elasticsearch"></a>Elasticsearch

### <a name="reading-data-from-elasticsearch-fails"></a>Het lezen van gegevens uit Elasticsearch mislukt.

**Detectie**. De juiste uitzondering voor de bepaalde [Elasticsearch client] onderschept[ elasticsearch-client] die wordt gebruikt. 

**Herstel**

- Gebruik een om opnieuw. Elke client heeft een eigen beleid voor het opnieuw. 

- Meerdere Elasticsearch knooppunten implementeren en herhaling gebruiken voor beschikbaarheid.

Zie voor meer informatie [Elasticsearch uitgevoerd op Azure][elasticsearch-azure].

**Diagnostische hulpprogramma's**. U kunt controleren hulpprogramma's voor Elasticsearch of meld u aan alle fouten op de client met de nettolading. Zie het gedeelte 'Monitoring' in [Elasticsearch uitgevoerd op Azure][elasticsearch-azure].


### <a name="writing-data-to-elasticsearch-fails"></a>Het schrijven van gegevens naar Elasticsearch mislukt.

**Detectie**. De juiste uitzondering voor de bepaalde [Elasticsearch client] onderschept[ elasticsearch-client] die wordt gebruikt.  

**Herstel**

- Gebruik een om opnieuw. Elke client heeft een eigen beleid voor het opnieuw. 
 
- Als de toepassing mogelijk zonder een niveau verminderde consistentie, kunt u schrijven met `write_consistency` instellen van `quorum`.

Zie voor meer informatie [Elasticsearch uitgevoerd op Azure][elasticsearch-azure].

**Diagnostische hulpprogramma's**. U kunt controleren hulpprogramma's voor Elasticsearch of meld u aan alle fouten op de client met de nettolading. Zie het gedeelte 'Monitoring' in [Elasticsearch uitgevoerd op Azure][elasticsearch-azure].


## <a name="queue-storage"></a>Wachtrij opslag

### <a name="writing-a-message-to-azure-queue-storage-fails-consistently"></a>Consistente schrijven van een bericht naar Azure wachtrij opslag mislukt.

**Detectie**. Nadat u *N* herhalingspogingen, mislukt de bewerking schrijven nog steeds.

**Herstel**

- Opslag van de gegevens in een lokale cache en doorsturen van het schrijven naar opslag later, wanneer de service beschikbaar.
- Maak een secundaire wachtrij en schrijven voor deze wachtrij als de primaire wachtrij niet beschikbaar is.

**Diagnostische hulpprogramma's**. Gebruik [metrische opslaggegevens][storage-metrics].


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>De toepassing kan een bepaald bericht uit de wachtrij verwerken. 

**Detectie**. Toepassing specifieke. Stel het bericht bevat ongeldige gegevens of de bedrijfslogica mislukt om welke reden. 

**Herstel**

Het bericht verplaatsen naar een afzonderlijke wachtrij. Als u wilt onderzoeken de berichten in die wachtrij als afzonderlijk proces worden uitgevoerd.

Kunt u overwegen Azure Service Bus Messaging wachtrijen, waarmee een [Global] [ sb-dead-letter-queue] functionaliteit voor dit doel.

Opmerking: Als u opslag wachtrijen met WebJobs gebruikt, biedt de WebJobs SDK verwerken van ingebouwde verontreinigde berichten. Lees [hoe u met Azure wachtrij opslag met de SDK WebJobs][sb-poison-message].

**Diagnostische hulpprogramma's**. Logboekregistratie van toepassingen gebruiken.


## <a name="redis-cache"></a>Bestand Vgx Cache.

### <a name="reading-from-the-cache-fails"></a>Lezen uit de cache is mislukt.

**Detectie**. Variabel `StackExchange.Redis.RedisConnectionException`.

**Herstel**

1. Probeer opnieuw op tijdelijke fouten. Azure bestand Vgx. cache ondersteunt ingebouwde opnieuw tot en met Zie [bestand Vgx. Cache opnieuw richtlijnen][redis-retry].
2. Tijdelijke fouten behandelen als een gemiste cache en terugschakelen naar de oorspronkelijke gegevensbron.

**Diagnostische hulpprogramma's**. Gebruik [bestand Vgx. Cache diagnostisch hulpprogramma][redis-monitor].


### <a name="writing-to-the-cache-fails"></a>Schrijven naar de cache is mislukt.

**Detectie**. Variabel `StackExchange.Redis.RedisConnectionException`.

**Herstel**

1. Probeer opnieuw op tijdelijke fouten. Azure bestand Vgx. cache ondersteunt ingebouwde opnieuw tot en met Zie [bestand Vgx. Cache opnieuw richtlijnen][redis-retry].
2. Als de fout tijdelijke is, negeert u deze en andere transacties schrijven aan de cache later laten.

**Diagnostische hulpprogramma's**. Gebruik [bestand Vgx. Cache diagnostisch hulpprogramma][redis-monitor].


## <a name="sql-database"></a>SQL-Database

### <a name="cannot-connect-to-the-database-in-the-primary-region"></a>Kan geen verbinding maken met de database in de primaire regio.

**Detectie**. -Verbinding mislukt.

**Herstel**

Let op: De database moet worden geconfigureerd voor actieve geografische-replicatie. Zie [SQL actieve geografische-databasereplicatie][sql-db-replication].

- Lees voor query's uit een secundaire replica. 
- Wordt ingevoegd en updates, handmatig niet via een secundaire replica. Zie [een failover- of niet gepland voor Azure SQL-Database][sql-db-failover].

De replica gebruikt een andere verbindingsreeks, dus moet u de verbindingsreeks in uw toepassing bijwerken.


### <a name="client-runs-out-of-connections-in-the-connection-pool"></a>Client wordt uitgevoerd uit verbindingen in de groep.

**Detectie**. Variabel `System.InvalidOperationException` fouten. 

**Herstel**

- Probeer het opnieuw.
- Als een plan voor risicobeperking isoleren de verbinding van toepassingen voor elke use-case, zodat een use-case kan niet alle verbindingen domineren.
- Vergroot het maximumaantal verbindingen van toepassingen.

**Diagnostische hulpprogramma's**. Logboeken aan de toepassing.


### <a name="database-connection-limit-is-reached"></a>Database-verbindingslimiet is bereikt. 

**Detectie**. Azure SQL-Database beperkt het aantal gelijktijdige werknemers, aanmeldingen en sessies. De limieten, hangt af van de servicelaag. Voor meer informatie raadpleegt u [limieten van Azure SQL Database-resource][sql-db-limits].

Als u wilt deze fouten kan worden gevonden, onderschept `System.Data.SqlClient.SqlException` en controleert u de waarde van `SqlException.Number` voor de SQL-foutcode. Zie voor een lijst met relevante foutcodes, [SQL-foutcodes voor SQL-Database-clienttoepassingen: Database verbindingsfout en andere problemen][sql-db-errors].

**Herstel**. Deze fouten worden beschouwd als tijdelijke, zodat u kunt het probleem opnieuw mogelijk verhelpen. Als u deze fouten consistente raakt, kunt u de schaal van de database.

**Diagnostische hulpprogramma's**. -De [sys.event_log] [ sys.event_log] query succesvolle databaseverbindingen, verbindingsfouten en deadlocks geeft als resultaat.

- Een [Waarschuwing regel] maken[ azure-alerts] is mislukt verbindingen voor.

- [SQL-Database] inschakelen[ sql-db-audit] en controleren op mislukte aanmeldingen.


## <a name="service-bus-messaging"></a>Service-Bus Messaging

### <a name="reading-a-message-from-a-service-bus-queue-fails"></a>Een bericht lezen van een Service Bus-wachtrij mislukt.

**Detectie**. Variabel uitzonderingen vanuit de client SDK. De klasse base voor Service Bus uitzonderingen is [MessagingException][sb-messagingexception-class]. Als de fout tijdelijke, is de `IsTransient` eigenschap waar is. 

Zie voor meer informatie, [Service Bus messaging uitzonderingen][sb-messaging-exceptions].

**Herstel**

1. Probeer opnieuw op tijdelijke fouten. Zie [Service Bus opnieuw richtlijnen][sb-retry].

2. Berichten die naar een ontvanger kunnen niet worden bezorgd worden in een *Global*geplaatst. Gebruik deze wachtrij om te zien welke berichten niet kunnen worden ontvangen. Er is geen automatische opschoning van de wachtrij. Berichten, blijven er totdat u ze expliciet ophalen. Zie [overzicht van Service Bus wachtrijen][sb-dead-letter-queue].


### <a name="writing-a-message-to-a-service-bus-queue-fails"></a>Een bericht schrijft een Service Bus mislukt wachtrij.

**Detectie**. Variabel uitzonderingen vanuit de client SDK. De klasse base voor Service Bus uitzonderingen is [MessagingException][sb-messagingexception-class]. Als de fout tijdelijke, is de `IsTransient` eigenschap waar is. 

Zie voor meer informatie, [Service Bus messaging uitzonderingen][sb-messaging-exceptions].

**Herstel**

1. De client Service Bus pogingen automatisch na tijdelijke fouten. Standaard wordt deze gebruikt in exponentiële back-uitschakelen. Na het maximale aantal nieuwe pogingen of maximale time-out genereert de client een uitzondering. Zie voor meer informatie, [Service Bus opnieuw richtlijnen][sb-retry].

2. Als het wachtrijquotum voor wordt overschreden, de client [QuotaExceededException]genereert[QuotaExceededException]. Het uitzonderingsbericht biedt meer informatie. Enkele berichten uit de wachtrij corrigeren voordat opnieuw wordt geprobeerd en kunt u overwegen het patroon stroomonderbreker voortdurende pogingen om opnieuw te vermijden, terwijl de limiet is overschreden. Ook voor zorgen dat de eigenschap [BrokeredMessage.TimeToLive] niet te hoog is ingesteld. 

3. In een gebied, tolerantie kan worden verbeterd met behulp van [gepartitioneerde wachtrijen of onderwerpen][sb-partition]. Een niet-partities wachtrij of onderwerp is toegewezen aan één SMS winkel. Als dit SMS archief niet beschikbaar is, worden alle bewerkingen voor de wachtrij of onderwerp, mislukt. Een gepartitioneerde wachtrij of onderwerp is partitioneren over meerdere SMS winkels. 

4. Voor aanvullende tolerantie, twee Service Bus naamruimten maken in verschillende regio's en de berichten repliceren. U kunt op actieve herhaling of passieve replicatie gebruiken.

    - Actieve herhaling: de client verzendt u elk bericht dat naar beide wachtrijen. De ontvanger luistert op beide wachtrijen. Berichten met een unieke identificatie, een label om de client dubbele berichten kunt verwijderen.

    - Passieve herhaling: de client verzendt het bericht naar één wachtrij. Als er een fout, schakelt de client terug naar de andere wachtrij. De ontvanger luistert op beide wachtrijen. Deze methode minder dubbele berichten die worden verzonden. De ontvanger moet echter nog steeds dubbele berichten verwerken.

    Zie voor meer informatie [GeoReplication voorbeeld] [ sb-georeplication-sample] en [Aanbevolen procedures voor isolerende toepassingen tegen Service Bus bijvoorbeeld en systeemfouten][sb-outages].


### <a name="duplicate-message"></a>Dubbele bericht.

**Detectie**. Bekijk de `MessageId` en `DeliveryCount` eigenschappen van het bericht.

**Herstel**

- Indien mogelijk ontwerp uw bericht verwerking moeten idempotency is ingeschakeld. Anders bericht-id's van berichten die al worden verwerkt en controleer de ID voordat het verwerken van een bericht opslaan.

- Dubbele detectie, inschakelen met het maken van de wachtrij met `RequiresDuplicateDetection` ingesteld op waar. Met deze instelling, Service Bus automatisch Hiermee verwijdert u alle berichten die worden verzonden met dezelfde `MessageId` als een vorige bericht.  Houd rekening met het volgende:

    -  Deze instelling kunt u voorkomt dat dubbele berichten worden in de wachtrij plaatsen. Deze voorkomen niet dat een ontvanger hetzelfde bericht meerdere keren verwerken.

    - Dubbele detectie heeft een tijdvenster. Als er een dubbel voorbij dit venster wordt verzonden, won't worden gedetecteerd.

**Diagnostische hulpprogramma's**. Meld u aan individuele berichten.


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>De toepassing kan een bepaald bericht uit de wachtrij verwerken. 

**Detectie**. Toepassing specifieke. Stel het bericht bevat ongeldige gegevens of de bedrijfslogica mislukt om welke reden. 

**Herstel**

Er zijn twee mislukt modi u rekening moet houden. 

- De ontvanger wordt de fout gedetecteerd. In dit geval het bericht verplaatsen naar de wachtrij. Later uitvoeren als u wilt onderzoeken de berichten in de wachtrij als afzonderlijk proces.

- De ontvanger mislukt in het midden van het verwerken van bericht &mdash; bijvoorbeeld vanwege een onverwerkte uitzondering. Als u wilt in dit geval gebruikt `PeekLock` modus. In deze modus als het hangslot is verlopen, beschikbaar het bericht voor andere ontvangers. Als het bericht groter is dan de bezorging van het maximum aantal of de time-to-live, wordt het bericht is automatisch verplaatst naar de wachtrij.

Voor meer informatie raadpleegt u [overzicht van Service Bus wachtrijen][sb-dead-letter-queue].

**Diagnostische hulpprogramma's**. Wanneer een bericht de toepassing naar de wachtrij verplaatst, moet u een gebeurtenis schrijven naar de toepassingslogboeken aan de.


## <a name="service-fabric"></a>Service stof

### <a name="a-request-to-a-service-fails"></a>Een verzoek om een service is mislukt.

**Detectie**. De service retourneert een fout.

**Herstel**

- Zoek opnieuw naar een proxy (`ServiceProxy` of `ActorProxy`) en de methode service/acteur opnieuw. 

- **Statuscontrole-service**. Bewerkingen op betrouwbare verzamelingen in een transactie laten teruglopen. Als er een fout is, wordt ook de transactie hersteld. De aanvraag wordt als opgehaald uit een wachtrij, opnieuw worden verwerkt. 
 
- **Stateless service**. Als de service zich blijft gegevens naar een externe store voordoen, moeten alle bewerkingen idempotency is ingeschakeld.


**Diagnostische hulpprogramma's**. Toepassingslogboek

### <a name="service-fabric-node-is-shut-down"></a>Service stof knooppunt is afgesloten.

**Detectie**. Een token annulering van de service is doorgegeven `RunAsync` methode. De taak annuleert service stof voordat u het knooppunt afsluit.

**Herstel**. Gebruik de annulering token om afsluiten gedetecteerd. Als Service stof annulering aanvraagt, eventuele werk voltooien en afsluiten `RunAsync` zo snel mogelijk.

**Diagnostische hulpprogramma's**. Toepassingslogboeken aan de


## <a name="storage"></a>Opslag

### <a name="writing-data-to-azure-storage-fails"></a>Schrijven gegevens naar Azure Storage mislukt?

**Detectie**. De client ontvangt fouten bij het schrijven.

**Herstel**

1. Probeer het opnieuw, herstellen uit tijdelijke fouten. Het [beleid opnieuw] [ Storage.RetryPolicies] in de client SDK omgaat met dit automatisch.
2. Implementeer het patroon stroomonderbreker om te voorkomen dat verwarrend opslag.
3. Als N herhalingspogingen mislukt, voert u een correcte gebruik. Bijvoorbeeld:

    - Opslag van de gegevens in een lokale cache en doorsturen van het schrijven naar opslag later, wanneer de service beschikbaar.
    - Als de actie schrijven in een bereik transacties was, dit u de transactie.

**Diagnostische hulpprogramma's**. Gebruik [metrische opslaggegevens][storage-metrics].


### <a name="reading-data-from-azure-storage-fails"></a>Het lezen van gegevens uit de opslag van Azure mislukt.

**Detectie**. De client ontvangt fouten tijdens het lezen.

**Herstel**

1. Probeer het opnieuw, herstellen uit tijdelijke fouten. Het [beleid opnieuw] [ Storage.RetryPolicies] in de client SDK omgaat met dit automatisch.
2. Voor AB-GRS opslag, als het lezen van het primaire eindpunt mislukt, kunt u lezen van het secundaire eindpunt. De client SDK kan dit automatisch verwerken. Zie [Azure Storage replicatie][storage-replication].
3. Als *N* herhalingspogingen mislukt, moet u een fallback actie verslechteren zonder problemen. Als de productafbeelding van een kan niet worden opgehaald uit de opslag, bijvoorbeeld een algemene tijdelijke aanduiding voor afbeelding weergeven.

**Diagnostische hulpprogramma's**. Gebruik [metrische opslaggegevens][storage-metrics].


## <a name="virtual-machine"></a>VM

### <a name="connection-to-a-backend-vm-fails"></a>Verbinding met een backend VM is mislukt.

**Detectie**. Fouten.

**Herstel**

- Ten minste twee backend VMs in een set beschikbaarheid, achter een taakverdeling implementeren.

- Als de verbindingsfout tijdelijke is, soms TCP wordt succes opnieuw geprobeerd het bericht verstuurt. 

- Een nieuwe poging-beleid implementeert in de toepassing. 

- Implementeren voor permanente of tijdelijke fouten, de [stroomonderbreker] [ circuit-breaker] patroon.

- Als de bellen VM groter is dan de limiet van de egress netwerk, wordt de uitgaande wachtrij vult u omhoog. Als de uitgaande wachtrij consistente vol is, kunt u horizontaal schalen. 

**Diagnostische hulpprogramma's**. Gebeurtenissen aan de servicegrenzen van de.

### <a name="vm-instance-becomes-unavailable-or-unhealthy"></a>VM exemplaar wordt niet beschikbaar of beschadigd.

**Detectie**. Een taakverdeling- [systeemstatus-test] configureren[ lb-probe] die geeft aan of het exemplaar VM correct is. De test te controleren of de kritieke functies correct zijn reageert. 

**Herstel**. Voor elke toepassingslaag meerdere exemplaren van VM in dezelfde beschikbaarheid set plaatsen en plaats een taakverdeling vóór de VMs. Als de status-test is mislukt, stopt de taakverdeling nieuwe verbindingen naar het beschadigde exemplaar verzendt.

**Diagnostische hulpprogramma's**. -Gebruik van taakverdeling [log analytics][lb-monitor].
- Configureer uw systeem van toezicht om de alle de status eindpunten cmdlets voor controle te houden.


### <a name="operator-accidentally-shuts-down-a-vm"></a>Een VM uitschakelt operator per ongeluk.

**Detectie**. N/B

**Herstel**. Instellen van een resource vergrendelen met `ReadOnly` niveau. Zie [vergrendelingsbronnen met Azure resourcemanager][rm-locks].

**Diagnostische hulpprogramma's**. Gebruik [Azure activiteit logboeken][azure-activity-logs].


## <a name="webjobs"></a>WebJobs

### <a name="continuous-job-stops-running-when-the-scm-host-is-idle"></a>Continue taak stopt worden uitgevoerd wanneer de SCM-host niet actief is.

**Detectie**. Een token annulering doorgeven aan de functie WebJob. Zie voor meer informatie [correcte afsluiten][web-jobs-shutdown]. 

**Herstel**. Schakel de `Always On` instellen in de web-app. Zie voor meer informatie, [achtergrondtaken uitvoeren met WebJobs][web-jobs].


## <a name="application-design"></a>Het ontwerp van toepassing

### <a name="application-cant-handle-a-spike-in-incoming-requests"></a>Toepassing verwerken niet een Prikker in verzoeken voor oproepen.

**Detectie**. Is afhankelijk van de toepassing. Veelvoorkomende symptomen:

- De website wordt gestart HTTP 5xx foutcodes retourneren.
- Afhankelijke services, zoals database of opslag, starten aanvragen beperken. Zoek naar HTTP-fouten zoals HTTP 429 (te veel aanvragen), afhankelijk van de service.
- Lengte van de HTTP-wachtrij in omvang groeit.

**Herstel**

- Schaal naar grotere belasting. 

- Verklein fouten om te voorkomen trapsgewijze fouten verstoort de hele toepassing. Strategieën voor risicobeperking opnemen:

    - Het [Patroon beperken] implementeren[ throttling-pattern] problematisch backend-systemen vermijden.
    - Gebruik van de [wachtrij gebaseerde laden effenen] [ queue-based-load-leveling] buffer aanvragen en ze verwerken met een juiste snelheid.
    - Bepaalde clients prioriteit te bepalen. Als de toepassing gratis en betaalde lagen heeft, beperken bijvoorbeeld klanten op de gratis laag, maar niet betaald klanten. Zie [prioriteit wachtrij patroon][priority-queue-pattern].

**Diagnostische hulpprogramma's**. De functie [App Service diagnostische gegevens vastleggen]gebruiken[app-service-logging]. Het gebruik van een service zoals [Azure Log Analytics][azure-log-analytics], [Toepassing inzichten][app-insights], of [Nieuwe Relic] [ new-relic] om te begrijpen wat de diagnostische logboeken.


### <a name="one-of-the-operations-in-a-workflow-or-distributed-transaction-fails"></a>Een van de bewerkingen in een werkstroom of gedistribueerde transactie mislukt.

**Detectie**. Nadat u *N* herhalingspogingen, het nog steeds niet.

**Herstel**

- Als een plan voor risicobeperking implementeren de [Scheduler Agent toezichthouder] [ scheduler-agent-supervisor] patroon voor het beheren van de hele werkstroom. 
- Klik op time-out niet opnieuw. Er is een tarief lage success voor deze fout. 
- Wachtrij werk, om te kunnen probeer het later opnieuw.

**Diagnostische hulpprogramma's**. Meld u aan alle bewerkingen (geslaagde en mislukte), inclusief acties verwacht. Gebruik correlatie-id's, zodat u alle bewerkingen binnen dezelfde transactie kunt bijhouden.


### <a name="a-call-to-a-remote-service-fails"></a>Een oproep naar een externe service is mislukt.

**Detectie**. HTTP-foutcode.

**Herstel**

1. Probeer opnieuw op tijdelijke fouten. 
2. Als het gesprek mislukt na *N* probeert, een fallback actie uitvoeren. (Toepassing specifieke.)
3. Het [patroon stroomonderbreker] implementeren[ circuit-breaker] trapsgewijze fouten te vermijden. 

**Diagnostische hulpprogramma's**. Meld u aan alle externe oproep fouten.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het proces FMA, [flexibiliteit door ontwerp voor cloudservices] [ resilience-by-design-pdf] (PDF-bestand downloaden).

<!-- links -->

[api-management]: https://azure.microsoft.com/documentation/services/api-management/
[api-management-throttling]: ../api-management/api-management-sample-flexible-throttling.md
[app-insights]: ../application-insights/app-insights-overview.md
[app-insights-web-apps]: ../application-insights/app-insights-azure-web-apps.md
[app-service-configure]: ../app-service-web/web-sites-configure.md
[app-service-logging]: ../app-service-web/web-sites-enable-diagnostic-log.md
[app-service-slots]: ../app-service-web/web-sites-staged-publishing.md
[auto-rest-client-retry]: https://github.com/Azure/autorest/blob/master/Documentation/clients-retry.md
[azure-activity-logs]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[azure-alerts]: ../monitoring-and-diagnostics/insights-alerts-portal.md
[azure-log-analytics]: ../log-analytics/log-analytics-overview.md
[BrokeredMessage.TimeToLive]: https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
[cassandra-error-handling]: http://www.datastax.com/dev/blog/cassandra-error-handling-done-right
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[docdb-multi-region]: ../documentdb/documentdb-developing-with-multiple-regions.md
[elasticsearch-azure]: guidance-elasticsearch-running-on-azure.md
[elasticsearch-client]: https://www.elastic.co/guide/en/elasticsearch/client/index.html
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[onstop-events]: https://azure.microsoft.com/blog/the-right-way-to-handle-azure-onstop-events/
[lb-monitor]: ../load-balancer/load-balancer-monitor-log.md
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md#learn-about-the-types-of-probes
[new-relic]: https://newrelic.com/
[priority-queue-pattern]: https://msdn.microsoft.com/library/dn589794.aspx
[queue-based-load-leveling]: https://msdn.microsoft.com/library/dn589783.aspx
[QuotaExceededException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.quotaexceededexception.aspx
[ra-web-apps-basic]: guidance-web-apps-basic.md
[redis-monitor]: ../redis-cache/cache-how-to-monitor.md
[redis-retry]: ../best-practices-retry-service-specific.md#cache-redis-retry-guidelines
[resilience-by-design-pdf]: http://download.microsoft.com/download/D/8/C/D8C599A4-4E8A-49BF-80EE-FE35F49B914D/Resilience_by_Design_for_Cloud_Services_White_Paper.pdf
[RoleEntryPoint.OnStop]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[RoleEnvironment.Stopping]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx
[rm-locks]: ../resource-group-lock-resources.md
[sb-dead-letter-queue]: ../service-bus-messaging/service-bus-dead-letter-queues.md
[sb-georeplication-sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/GeoReplication
[sb-messagingexception-class]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
[sb-messaging-exceptions]: ../service-bus-messaging/service-bus-messaging-exceptions.md
[sb-outages]: ../service-bus/service-bus-outages-disasters.md#protecting-queues-and-topics-against-datacenter-outages-or-disasters
[sb-partition]: ../service-bus-messaging/service-bus-partitioning.md
[sb-poison-message]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#poison
[sb-retry]: ../best-practices-retry-service-specific.md#service-bus-retry-guidelines
[search-sdk]: https://msdn.microsoft.com/library/dn951165.aspx
[scheduler-agent-supervisor]: https://msdn.microsoft.com/library/dn589780.aspx
[search-analytics]: ../search/search-traffic-analytics.md
[sql-db-audit]: ../sql-database/sql-database-auditing-get-started.md
[sql-db-errors]: ../sql-database/sql-database-develop-error-messages.md#resource-governanance-errors
[sql-db-failover]: ../sql-database/sql-database-geo-replication-failover-portal.md
[sql-db-limits]: ../sql-database/sql-database-resource-limits.md
[sql-db-replication]: ../sql-database/sql-database-geo-replication-overview.md
[storage-metrics]: https://msdn.microsoft.com/library/dn782843.aspx
[storage-replication]: ../storage/storage-redundancy.md
[Storage.RetryPolicies]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.aspx
[sys.event_log]: https://msdn.microsoft.com/library/dn270018.aspx
[throttling-pattern]: https://msdn.microsoft.com/library/dn589798.aspx
[web-jobs]: ../app-service-web/web-sites-create-web-jobs.md
[web-jobs-shutdown]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful

