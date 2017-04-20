<properties
    pageTitle="Bewaak uw API's met Azure API Management, gebeurtenis Hubs en Runscope"
    description="Voorbeeld-toepassing wilt zien waarin het beleid log-naar-eventhub door verbindende Azure API Management, Azure gebeurtenis Hubs en Runscope voor HTTP logboekregistratie en controle"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Bewaak uw API's met Azure API Management, gebeurtenis Hubs en Runscope

De [API Management-service](api-management-key-concepts.md) biedt vele mogelijkheden voor het verbeteren van de verwerking van HTTP-aanvragen verzonden naar uw HTTP-API. Er zijn echter de aanwezigheid van het vergaderverzoeken en antwoorden tijdelijke. De wordt aangevraagd en loopt tot en met de API Management-service aan uw backend API. Uw API verwerkt de aanvraag en een antwoord doorloopt naar de API-consument. De API Management-service blijft enkele belangrijke statistieken over de API voor weergave in het dashboard van de Publisher-portal, maar ook buiten dat de details verdwenen zijn.

U kunt de gegevens uit de vergaderverzoeken en antwoorden op een [Azure gebeurtenis Hub](../event-hubs/event-hubs-what-is-event-hubs.md)verzenden met behulp van het [logboek-naar-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [beleid](api-management-howto-policies.md) in de API Management-service. Er zijn tal van redenen waarom u wilt mogelijk gebeurtenissen genereren van HTTP-berichten naar uw API's is verzonden. Enkele voorbeelden zijn audittrail van updates, gebruiksanalyses, uitzondering waarschuwingen en 3e Nieuwjaarsfeest integraties.   

Dit artikel wordt beschreven hoe u het hele HTTP vergaderverzoeken en antwoorden bericht vastleggen, naar een Hub voor de gebeurtenis te verzenden en vervolgens doorgeven dat bericht aan een derde partij service waarmee met een HTTP logboekregistratie en controle services.

## <a name="why-send-from-api-management-service"></a>Waarom van API Management-Service verzenden?
Het is mogelijk om te schrijven HTTP middleware dat kunt aansluiten op HTTP-API kaders om te leggen HTTP vergaderverzoeken en antwoorden en ze ontstaan van logboekregistratie en controle systemen. Het nadeel deze methode is de HTTP-middleware moet worden geïntegreerd in de backend-API en moet overeenkomen met het platform van de API. Als er meerdere API's moet de middleware implementeren op elkaar. Er zijn vaak redenen waarom backend API's kan niet worden bijgewerkt.

Gebruik van de Azure API Management-service integreren met logboekregistratie infrastructuur, biedt een gecentraliseerde en platform-onafhankelijke-oplossing. Het is ook scalable, gedeeltelijk vanwege de [geografische herhaling](api-management-howto-deploy-multi-region.md) mogelijkheden van Azure API Management.

## <a name="why-send-to-an-azure-event-hub"></a>Waarom verzenden naar een Azure gebeurtenis Hub?
Het is een redelijk te vragen, waarom beleid die specifiek is voor Azure gebeurtenis Hubs maken? Er zijn verschillende plaatsen waar ik wil mogelijk Meld u aan mijn aanvragen. Alleen versturen waarom niet aanvragen rechtstreeks naar de uiteindelijke bestemming?  Dit is een optie. Echter bij het logboekregistratieaanvragen van een service API management, moet u rekening moet houden wat de prestaties van de API wordt invloed op logboekregistratie berichten. Geleidelijke stijging laden kunnen worden verwerkt door te verhogen beschikbaar exemplaren van onderdelen of door te profiteren van geografische-herhaling. Korte pieken in het verkeer kunnen echter leiden tot aanvragen aanzienlijk wordt vertraagd als aanvragen voor logboekregistratie infrastructuur beginnen met het vertragen onder laden.

De Azure gebeurtenis Hubs is bedoeld om grote hoeveelheden gegevens, ingress met capaciteit voor omgaan met een aantal gebeurtenissen die veel hoger dan het aantal HTTP-meeste API's proces aanvragen. De gebeurtenis Hub fungeert als een soort geavanceerde buffer tussen uw API management-service en de infrastructuur die worden opgeslagen en de berichten verwerken. Dit zorgt ervoor dat uw prestaties API worden niet beïnvloed vanwege de infrastructuur van logboekregistratie.  

Nadat de gegevens is doorgegeven aan de Hub van een gebeurtenis dit behouden is gebleven en wacht op consumenten gebeurtenis Hub verwerkt. De gebeurtenis Hub niets uit hoe deze worden verwerkt, deze net hecht ervoor te zorgen dat het bericht is afgeleverd.     

Gebeurtenis Hubs hebben de mogelijkheid om te stream gebeurtenissen aan meerdere consumenten groepen. Hiermee kunt gebeurtenissen kan worden verwerkt door volledig andere systemen voor. Hiermee worden veel integratie-scenario's ondersteunende toevoeging vertragingen zonder op te plaatsen de verwerking van de aanvraag API binnen de API Management-service zoals slechts één gebeurtenis moet worden gegenereerd.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Een beleid toepassing/http-berichten te verzenden
Een gebeurtenis Hub accepteert gebeurtenisgegevens als een eenvoudige tekenreeks. De inhoud van de reeks zijn volledig tot aan u. Als u wilt inpakken van een HTTP-aanvraag en stuur dit uit naar gebeurtenis Hubs mogen moeten we de reeks waaruit de aanvraag of antwoord gegevens opmaken. We opnieuw kunt gebruiken en wat u mogelijk niet hoeft te schrijven onze eigen parseren van code in situaties als volgt, als er een bestaande indeling. In eerste instantie als ik beschouwd de [HAR](http://www.softwareishard.com/blog/har-12-spec/) met voor het verzenden van HTTP vergaderverzoeken en antwoorden. Deze indeling is echter geoptimaliseerd voor het opslaan van een reeks HTTP-aanvragen in een indeling van JSON gebaseerd. Bevat een aantal verplicht elementen die toegevoegd onnodige complexiteit voor de scenario voor het doorgeven van het HTTP-bericht over het netwerk.  

Een alternatief is gebruik van de `application/http` mediatype zoals is beschreven in de HTTP-specificatie [RFC 7230](http://tools.ietf.org/html/rfc7230). Dit mediatype exacte dezelfde indeling die wordt gebruikt dat is wel HTTP om berichten te verzenden via het netwerk gebruikt, maar het volledige bericht kan zijn in de hoofdtekst van een andere HTTP-aanvraag hebt opgeslagen. In ons geval gaan we zojuist de hoofdtekst gebruiken als ons bericht te verzenden naar gebeurtenis Hubs. Er is gemakkelijk een parser die zich in [Microsoft ASP.NET-API 2.2 webclient](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) -bibliotheken die u kunnen deze indeling parseert en deze converteren naar de native `HttpRequestMessage` en `HttpResponseMessage` objecten.

Als u wilt kunnen maken van dit bericht die we nodig om te profiteren van C# gebaseerd [beleid expressies](https://msdn.microsoft.com/library/azure/dn910913.aspx) in Azure API Management. Hier ziet u het beleid dat een bericht van de aanvraag HTTP naar Azure gebeurtenis Hubs stuurt.

       <log-to-eventhub logger-id="conferencelogger" partition-id="0">
       @{
           var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                       context.Request.Method,
                                                       context.Request.Url.Path + context.Request.Url.QueryString);

           var body = context.Request.Body?.As<string>(true);
           if (body != null && body.Length > 1024)
           {
               body = body.Substring(0, 1024);
           }

           var headers = context.Request.Headers
                                  .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                  .ToArray<string>();

           var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

           return "request:"   + context.Variables["message-id"] + "\n"
                               + requestLine + headerString + "\r\n" + body;
       }
       </log-to-eventhub>

### <a name="policy-declaration"></a>Beleid declaratie
Er een paar bepaalde dingen vermelden we over deze beleidsexpressie. Het beleid log-naar-eventhub heeft een kenmerk met de naam van logboek-id die verwijst naar de naam van het logboek die binnen de API Management-service is gemaakt. De details van een gebeurtenis Hub-logboek in de API Management-service instellen vindt u in het document [het gebeurtenissen in Azure gebeurtenis Hubs in Azure API Management](api-management-howto-log-event-hubs.md). Het tweede kenmerk is een optionele parameter die gebeurtenis Hubs waarin partitioneren voor de opslag van het bericht in de opdracht geeft. Partities gebeurtenis Hubs gebruiken om te schakelen schaalbaarheid en ten minste twee vereisen. De geordende bezorging van berichten is alleen binnen een partition gegarandeerd. Als we de opdracht gebeurtenis Hub in welke partition te plaatsen van het bericht niet geven, worden gebruikt een algoritme round robin distributie van het selectievakje laden. Dat kan echter enkele van onze berichten om te worden verwerkt volgorde veroorzaken.  

### <a name="partitions"></a>Partities
Ik heb gekozen om ervoor te zorgen onze berichten zijn afgeleverd op consumenten in volgorde en profiteren van de mogelijkheden van de verdeling laden van partities, berichten met HTTP één partitioneren en HTTP-antwoordberichten naar een tweede partition verzenden. Dit zorgt ervoor dat de verdeling van een even laden en wij kunnen garanderen dat alle aanvragen wordt verbruikt in volgorde en alle antwoorden wordt verbruikt in volgorde. Is het mogelijk dat voor een antwoord op voordat u de bijbehorende aanvraag worden gebruikt, maar die probleem wordt veroorzaakt door een zoals we hebben een andere methode voor het correleren vraagt voor antwoorden en we weten dat aanvragen altijd voorafgaan aan antwoorden.

### <a name="http-payloads"></a>HTTP-nettoladingen
Na building de `requestLine` we controleren om te zien als het hoofdgedeelte van de aanvraag moet worden afgekapt. Het hoofdgedeelte van de aanvraag wordt afgekapt tot alleen 1024. Dit kan worden verhoogd, maar afzonderlijke gebeurtenis Hub berichten beperkt tot 256KB zijn, zodat er waarschijnlijk dat sommige HTTP-bericht kan niet in een enkel bericht passen instanties. Bij het uitvoeren van logboekregistratie en analyses kan een aanzienlijk hoeveelheid gegevens worden afgeleid van alleen de HTTP-verzoek lijn en de kop. Ook veel API aanvragen retourneert alleen kleine instanties en dus het verlies van gegevens waarde door te breken grote organisaties vrij minimale ten opzichte van de korting op doorverbinden, verwerking en opslagkosten wilt behouden van alle hoofdtekst-inhoud. Een definitief opmerking over het verwerken van de hoofdtekst is moeten we doorgeven `true` in de As<string>() methode omdat we bij het lezen van de inhoud, maar is ook wilt dat de backend-API kunnen lezen van de hoofdtekst. Door doorgeven True in deze methode hebben we de hoofdtekst naar buffer zodat deze kan worden gelezen een tweede maal worden opgeslagen. Dit is belangrijk dat u rekening moet houden als er een API uploaden van zeer grote bestanden of lange polling gebruikt. In dat geval zou worden aanbevolen om te voorkomen dat de hoofdtekst helemaal lezen.   

### <a name="http-headers"></a>HTTP-headers
HTTP-Headers kunnen worden gewoon overgedragen naar de berichtindeling in een indeling met eenvoudige sleutelwaarde paar gegevenspunten. We hebben gekozen het verwijderen van bepaalde beveiliging gevoelige velden, zodat u kunt voorkomen onnodig lekkende referentiegegevens. Het is waarschijnlijk dat API toetsen en andere referenties voor analytics doeleinden zou worden gebruikt. Als we willen analyse van de gebruiker en het product uitvoeren ze gebruikt en we kan ophalen die uit de `context` object en in te voegen aan het bericht.     
### <a name="message-metadata"></a>Metagegevens van bericht
Als u het volledige bericht te verzenden naar de hub gebeurtenis maakt, de eerste regel is niet daadwerkelijk een deel van de `application/http` bericht. De eerste regel is extra metagegevens die bestaat uit of het bericht is een aanvraag of antwoordbericht en een bericht-id die wordt gebruikt om te relateren aanvragen voor antwoorden. De bericht-id wordt gemaakt met een ander beleid die er zo uitziet:

    <set-variable name="message-id" value="@(Guid.NewGuid())" />

We kan het verzoekbericht hebt gemaakt, opgeslagen die in een variabele totdat het antwoord is geretourneerd en klik vervolgens gewoon de vergaderverzoeken en antwoorden als één bericht verzonden. Echter de vergaderverzoeken en antwoorden onafhankelijk verzenden en een bericht-id om te relateren van de twee, onmiddellijk wordt iets meer flexibiliteit de grootte van berichten, de mogelijkheid om te profiteren van meerdere partities terwijl onderhouden van de volgorde van berichten en het verzoek wordt weergegeven in het dashboard van onze logboekregistratie sneller. Er kan ook zijn bepaalde scenario's waarin een geldig antwoord is nooit hebt verzonden naar de hub gebeurtenis mogelijk vanwege een fout fatale aanvraag in de API Management-service, maar we wordt wel een record van de aanvraag kunt invullen.

Het beleid om het antwoord HTTP-bericht te verzenden Hiermee wordt gezocht lijkt veel op het verzoek en zodat de beleidsconfiguratie voltooid ziet er zo uit:

      <policies>
        <inbound>
            <set-variable name="message-id" value="@(Guid.NewGuid())" />
            <log-to-eventhub logger-id="conferencelogger" partition-id="0">
              @{
                  var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                              context.Request.Method,
                                                              context.Request.Url.Path + context.Request.Url.QueryString);

                  var body = context.Request.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Request.Headers
                                       .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                       .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                       .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "request:"   + context.Variables["message-id"] + "\n"
                                      + requestLine + headerString + "\r\n" + body;
              }
          </log-to-eventhub>
        </inbound>
        <backend>
            <forward-request follow-redirects="true" />
        </backend>
        <outbound>
            <log-to-eventhub logger-id="conferencelogger" partition-id="1">
              @{
                  var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                                      context.Response.StatusCode,
                                                      context.Response.StatusReason);

                  var body = context.Response.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Response.Headers
                                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                                  .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "response:"  + context.Variables["message-id"] + "\n"
                                      + statusLine + headerString + "\r\n" + body;
             }
          </log-to-eventhub>
        </outbound>
      </policies>

De `set-variable` beleid Hiermee maakt u een waarde die toegankelijk is voor beide de `log-to-eventhub` beleid in de `<inbound>` sectie en de `<outbound>` sectie.  

## <a name="receiving-events-from-event-hubs"></a>Gebeurtenissen van gebeurtenis Hubs ontvangt
Gebeurtenissen van Azure gebeurtenis Hub zijn ontvangen via het [AMQP-protocol](http://www.amqp.org/). Het team van Microsoft-Service Bus hebt aangebracht client bibliotheken die beschikbaar zijn om de in beslag nemen gebeurtenissen te vereenvoudigen. Er zijn twee manieren worden ondersteund, wordt een een *Directe consumenten* en de andere gebruikmaakt van de `EventProcessorHost` class. Voorbeelden van de volgende twee manieren vindt u in de [Gebeurtenis Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md). De korte versie van de verschillen is, `Direct Consumer` hebt u volledige controle en de `EventProcessorHost` deel van het werk sanitair heeft voor u maar zorgt ervoor dat bepaalde veronderstellingen over hoe u deze gebeurtenissen worden verwerkt.  

### <a name="eventprocessorhost"></a>EventProcessorHost
In dit voorbeeld gebruiken we de `EventProcessorHost` voor eenvoudig, maar deze mogelijk niet de beste keuze voor dit scenario. `EventProcessorHost`het werk van ervoor te zorgen dat u geen zorgen over problemen binnen een bepaalde gebeurtenis Processorklasse threading te biedt. In ons scenario zijn we echter gewoon converteren van het bericht naar een andere indeling en deze langs naar een andere service met een methode asynchrone doorgegeven. Er is niet nodig voor het bijwerken van gedeelde staat en daarom geen kans op pesterijen threading problemen. Voor de meeste scenario's `EventProcessorHost` is waarschijnlijk de beste keuze en zeker de gemakkelijker optie.     

### <a name="ieventprocessor"></a>IEventProcessor
Het centrale concept bij gebruik van `EventProcessorHost` is het opzetten een een implementatie van de `IEventProcessor` interface waarin de methode `ProcessEventAsync`. Hier ziet u de essentie van deze methode:

  asynchrone taak IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages) {

           foreach (EventData eventData in messages)
           {
               _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

               try
               {
                   var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
                   await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
               }
               catch (Exception ex)
               {
                   _Logger.LogError(ex.Message);
               }
           }
            ... checkpointing code snipped ...
        }

Een lijst met EventData objecten zijn doorgegeven aan de methode en we die lijst doorlopen. Het aantal bytes van elke methode worden geparseerd in een object HttpMessage en dat object wordt doorgegeven aan een exemplaar van IHttpMessageProcessor.

### <a name="httpmessage"></a>HttpMessage
De `HttpMessage` exemplaar bevat drie typen gegevens:

      public class HttpMessage
       {
           public Guid MessageId { get; set; }
           public bool IsRequest { get; set; }
           public HttpRequestMessage HttpRequestMessage { get; set; }
           public HttpResponseMessage HttpResponseMessage { get; set; }

        ... parsing code snipped ...

      }

De `HttpMessage` exemplaar bevat een `MessageId` GUID waarmee wij de HTTP-aanvraag verbinden met de bijbehorende HTTP-antwoord en een Booleaanse waarde die aangeeft of het object een exemplaar van een HttpRequestMessage en HttpResponseMessage bevat. Met behulp van de ingebouwde HTTP lessen uit `System.Net.Http`, ik heb om te profiteren van de `application/http` parseren van code die is opgenomen in `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
De `HttpMessage` exemplaar wordt vervolgens doorgestuurd naar de implementatie van `IHttpMessageProcessor` welke een interface die ik heb gemaakt als u wilt loskoppelen van de ontvangen en de interpretatie van de gebeurtenis van Azure gebeurtenis Hub en de werkelijke verwerking van deze is.


## <a name="forwarding-the-http-message"></a>De HTTP-bericht doorsturen
Dit voorbeeld besloten ik zou interessant voor de HTTP-aanvraag via push naar [Runscope](http://www.runscope.com). Runscope is een gebaseerd cloudservice die gespecialiseerd in HTTP foutopsporing is, logboekregistratie en controle. Ze beschikken over een gratis laag, zodat het is eenvoudig om te proberen en wij ons om de HTTP-aanvragen kunnen in realtime die doorloopt tot en met onze API Management-service weer te geven.

De `IHttpMessageProcessor` implementatie ziet er zo uit,

      public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
       {
           private HttpClient _HttpClient;
           private ILogger _Logger;
           private string _BucketKey;
           public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
           {
               _HttpClient = httpClient;
               var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
               _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
               _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
               _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
               _Logger = logger;
           }

           public async Task ProcessHttpMessage(HttpMessage message)
           {
               var runscopeMessage = new RunscopeMessage()
               {
                   UniqueIdentifier = message.MessageId
               };

               if (message.IsRequest)
               {
                   _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
                   runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
               }
               else
               {
                   _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
                   runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
               }

               var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
               messagesLink.BucketKey = _BucketKey;
               messagesLink.RunscopeMessage = runscopeMessage;
               var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
               _Logger.LogDebug("Request sent to Runscope");
           }
       }

Ik heb om te profiteren van een [bestaande clientbibliotheek voor Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) die kunt u heel gemakkelijk push `HttpRequestMessage` en `HttpResponseMessage` exemplaren van hun brengen. Toegang tot de API Runscope moet u een account en een API-sleutel. Instructies voor het verkrijgen van een API-sleutel vindt u in de screencast [Access Runscope API-toepassingen maken](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) .

## <a name="complete-sample"></a>Voltooid voorbeeld
De [broncode](https://github.com/darrelmiller/ApimEventProcessor) en tests voor de steekproef zijn op Github. Moet u een [API Management-Service](api-management-get-started.md), [Een verbonden Hub voor de gebeurtenis](api-management-howto-log-event-hubs.md)en een [Opslag-Account](../storage/storage-create-storage-account.md) om uit te voeren in het voorbeeld voor uzelf.   

De steekproef is slechts een eenvoudige consoletoepassing die luistert voor gebeurtenissen die afkomstig zijn uit gebeurtenis Hub converteert naar een `HttpRequestMessage` en `HttpResponseMessage` objecten en ze kunnen de API Runscope doorstuurt.

In de volgende animatie ziet u een verzoek om te worden aangebracht in een API in de Portal voor ontwikkelaars, de met het bericht wordt ontvangen, verwerkt en doorgestuurd Console-toepassing en klikt u vervolgens de vergaderverzoeken en antwoorden niet weergegeven in de controle Runscope-verkeer is toegestaan.

![Demonstratie van aanvraag doorgestuurd naar Runscope](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Overzicht
Azure API Management-service biedt een ideale plaats het HTTP-verkeer naar en vanuit uw API's travelling vastleggen. Azure gebeurtenis Hubs is een zeer scalable, lage kosten oplossing voor het vastleggen van dat verkeer en die deze naar de secundaire verwerkingssystemen voor logboekregistratie, bewaken en andere geavanceerde analyses. Verbinding maken met 3e partijen verkeer bewaken zoals Runscope een eenvoudige als een paar tientallen regels met code is.

## <a name="next-steps"></a>Volgende stappen
-   Meer informatie over Azure gebeurtenis Hubs
    -   [Aan de slag met Azure gebeurtenis Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Berichten met EventProcessorHost ontvangen](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Gebeurtenis-Hubs handleiding voor het programmeren](../event-hubs/event-hubs-programming-guide.md)
-   Meer informatie over API Management en gebeurtenis Hubs-integratie
    -   [Hoe u aan te melden gebeurtenissen Azure gebeurtenis Hubs in Azure API Management](api-management-howto-log-event-hubs.md)
    -   [Logboek entiteitsverwijzing](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [log-naar-eventhub beleidsverwijzing](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    