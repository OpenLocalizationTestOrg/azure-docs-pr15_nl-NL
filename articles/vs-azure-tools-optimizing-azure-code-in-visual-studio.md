<properties
   pageTitle="Optimaliseren van uw Azure-code in Visual Studio | Microsoft Azure"
   description="Meer informatie over hoe Azure code optimalisatie hulpmiddelen voor in Visual Studio zorgen uw code meer robuuste en meer."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="optimizing-your-azure-code"></a>Uw Azure-Code optimaliseren

Als u apps die gebruikmaken van Microsoft Azure programmeren, zijn er enkele coding procedures die u volgen moet om te voorkomen van problemen met de app schaalbaarheid, gedrag en prestaties in een cloud-omgeving. Microsoft biedt een analyseprogramma Azure Code die wordt herkend en identificeert verscheidene van deze meestal veroorzaakt problemen en kunt u ze kunt oplossen. U kunt het hulpprogramma in Visual Studio via NuGet downloaden.

## <a name="azure-code-analysis-rules"></a>Azure Code analyse regels

Het hulpmiddel voor Azure Code gebruikt de volgende regels moeten worden automatisch uw Azure-code wordt gemarkeerd als deze bekende problemen met prestaties die invloed hebben op zijn gevonden. Problemen worden weergegeven als een waarschuwingen gedetecteerd of compileerprogramma fouten. Codecorrecties of suggesties voor het oplossen van de waarschuwing of fout zijn vaak opgenomen in een gloeilampje.

## <a name="avoid-using-default-in-process-session-state-mode"></a>(Standaard in-process) gebruikt vermijden

### <a name="id"></a>ID

AP0000

### <a name="description"></a>Beschrijving

Als u (de standaard in-process) gebruikt voor cloud-toepassingen gebruikt, kunt u de status van de sessie gaan verloren.

Deel uw ideeën en feedback [Azure Code analyse feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Standaard is de sessie staat modus is opgegeven in het bestand web.config in het proces. Als geen vermelding in het configuratiebestand is opgegeven, wordt de modus Session State standaard ook als u wilt in-proces. De modus in-process sessie staat in geheugen opgeslagen op de webserver. Wanneer een exemplaar opnieuw is opgestart of een nieuw exemplaar wordt gebruikt voor taakverdeling of failover-ondersteuning, wordt niet de status van de sessie die zijn opgeslagen in het geheugen van de webserver opgeslagen. Deze situatie wordt voorkomen dat de toepassing wordt scalable in de cloud.

ASP.NET-sessie state ondersteunt verschillende verschillende opslagopties voor sessie staat gegevens: InProc, StateServer, SQL Server, aangepast, en uit. Het wordt aanbevolen dat u aangepaste modus voor gegevens op een externe sessie staat store, zoals de [status van de sessie Azure-provider voor het bestand Vgx.](http://go.microsoft.com/fwlink/?LinkId=401521)gebruiken.

### <a name="solution"></a>Oplossing

Een aanbevolen oplossing is om op te slaan sessie staat op een service beheerde cache. Leren werken met [Azure Session State-provider voor het bestand Vgx.](http://go.microsoft.com/fwlink/?LinkId=401521) voor de opslag van uw status van de sessie. U kunt ook de status van de sessie opslaan op andere locaties om ervoor te zorgen uw toepassing scalable in de cloud. Meer informatie over alternatieve Lees oplossingen [Sessie staat modi](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Uitvoeren methode moet asynchrone niet worden

### <a name="id"></a>ID

AP1000

### <a name="description"></a>Beschrijving

Asynchrone methoden (zoals [Wacht tot](https://msdn.microsoft.com/library/hh156528.aspx)) buiten de methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) maken en vervolgens de asynchrone methoden bellen vanuit [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). De methode [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) als asynchrone declareren zorgt ervoor dat de rol werknemer om in te voeren een lus opnieuw starten.

Deel uw ideeën en feedback [Azure Code analyse feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Aanroepen van asynchrone methoden binnen de methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) zorgt ervoor dat de runtime van de cloud-service naar de Prullenbak van de rol werknemer. Wanneer een rol werknemer wordt gestart, plaats alle programma-uitvoering binnen de methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . De methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) afsluiten zorgt ervoor dat de rol werknemer om opnieuw te starten. Wanneer de runtime van de rol werknemer de methode asynchrone raakt, verzendt alle bewerkingen na de methode asynchrone en wordt als resultaat. Hierdoor wordt de rol werknemer van de methode [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) afsluiten en opnieuw starten. In de volgende iteratie uitvoeren van de rol werknemer raakt de methode asynchrone opnieuw en opnieuw is opgestart, de rol werknemer naar de Prullenbak het opnieuw als u ook de oorzaak.

### <a name="solution"></a>Oplossing

Plaats alle asynchrone bewerkingen buiten de methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Klik, belt u de methode geherstructureerd asynchrone uit binnen de methode [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) zoals RunAsync () .wait. Het hulpmiddel voor Azure Code kunt u kunt dit probleem oplossen.

Het volgende codefragment ziet u de code-oplossing voor dit probleem:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Verificatie van de Service Bus gedeeld Access handtekening gebruiken

### <a name="id"></a>ID

AP2000

### <a name="description"></a>Beschrijving

Gebruik gedeeld Access handtekening (SA's) voor verificatie. Access Control Service (ACS) wordt voor service bus verificatie vervangen.

Deel uw ideeën en feedback [Azure Code analyse feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Voor een betere beveiliging is Azure Active Directory ACS verificatie vervangen door SA's verificatie. Zie [Azure Active Directory gebeurt er met ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) voor informatie over het abonnement overgang.

### <a name="solution"></a>Oplossing

SA's verificatie gebruiken op uw apps. Het volgende voorbeeld ziet u hoe u een bestaande SA's token gebruikt voor toegang tot een service bus naamruimte of entiteit.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Zie de volgende onderwerpen voor meer informatie.

- Zie voor een overzicht, [Access handtekening-verificatie gedeeld met Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)

- [Het gebruik van de verificatie van handtekening gedeeld met Service Bus](https://msdn.microsoft.com/library/dn205161.aspx)

- Voor een voorbeeld van project, raadpleegt u [Gebruik gedeeld Access handtekening (SA's) verificatie met een Service Bus-abonnement](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a>U kunt OnMessage methode gebruiken om te voorkomen "ontvangen lus"

### <a name="id"></a>ID

AP2002

### <a name="description"></a>Beschrijving

Om te voorkomen gaan in een 'ontvangen lus', aanroepen van de methode **OnMessage** is een betere oplossing voor het ontvangen van berichten dan het aanroepen van de methode **ontvangen** . Als u de methode **ontvangen** gebruiken moet en u opgeeft dat een niet-standaard-server wachttijd, zorg er echter dat de wachttijd server is meer dan één minuut.

Deel uw ideeën en feedback [Azure Code analyse feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Wanneer u belt **OnMessage**, begint de client een interne bericht pomp welke voortdurend de wachtrij of -abonnement probeert. Dit bericht pomp bevat een lus die een oproep problemen voor het ontvangen van berichten. Als het gesprek treedt er een time-out, oplossen deze een nieuw gesprek. De time-outinterval wordt bepaald door de waarde van de eigenschap [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) van de [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)die wordt gebruikt.

Het voordeel van het gebruik van **OnMessage** vergeleken met **ontvangen** is dat gebruikers geen handmatig controleren op berichten, uitzonderingen, meerdere berichten parallel verwerkt en voert u de berichten.

Als u **ontvangen** telefonisch zonder de standaardwaarde, zorgt u ervoor dat de waarde *ServerWaitTime* meer dan één minuut. Als u *ServerWaitTime* op meer dan één minuut voorkomt dat de server time-out voordat het bericht wordt volledig ontvangen.

### <a name="solution"></a>Oplossing

Raadpleeg de volgende codevoorbeelden voor het gebruik van aanbevolen. Zie [QueueClient.OnMessage methode (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)en [QueueClient.Receive-methode (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx)voor meer informatie.

Ter verbetering van de prestaties van de Azure SMS-infrastructuur, raadpleegt u het ontwerppatroon [Asynchrone Messaging handleiding](https://msdn.microsoft.com/library/dn589781.aspx).

Hier volgt een voorbeeld van het gebruik van **OnMessage** voor het ontvangen van berichten.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

Hier volgt een voorbeeld van het gebruik van **ontvangen** met de wachttijd van de standaard-server.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

Hier volgt een voorbeeld van het gebruik van **ontvangen** met een niet-standaard server wachttijd.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Asynchrone Service Bus methoden kunt u overwegen

### <a name="id"></a>ID

AP2003

### <a name="description"></a>Beschrijving

Asynchrone Service Bus methoden gebruiken om de prestaties met brokered messaging te verbeteren.

Deel uw ideeën en feedback [Azure Code analyse feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Asynchrone methoden kunt toepassing programma gelijktijdigheid omdat het uitvoeren van elk gesprek, niet de belangrijkste thread blokkeren. Bij gebruik van de Service Bus messaging methoden, een bewerking (verzenden, ontvangen, verwijderen, enz.) duurt. Ditmaal bevat de verwerking van de bewerking door de service-Service Bus naast de latentie van de aanvraag en het antwoord. Als u wilt het aantal bewerkingen per keer vergroten, moeten gelijktijdig bewerkingen uitvoeren. Zie [Aanbevolen procedures voor prestaties verbeteringen gebruiken Service Bus Brokered Messaging](https://msdn.microsoft.com/library/azure/hh528527.aspx)voor meer informatie.

### <a name="solution"></a>Oplossing

Zie [QueueClient Class (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) voor informatie over het gebruik van de aanbevolen asynchrone methode.

Ter verbetering van de prestaties van de Azure SMS-infrastructuur, raadpleegt u het ontwerppatroon [Asynchrone Messaging handleiding](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Houd rekening met partities Service Bus wachtrijen en onderwerpen

### <a name="id"></a>ID

AP2004

### <a name="description"></a>Beschrijving

Partition Service Bus wachtrijen en onderwerpen voor betere prestaties met Service Bus messaging.

Deel uw ideeën en feedback [Azure Code analyse feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Partitioneren Service Bus wachtrijen en onderwerpen Hiermee verhoogt u de beschikbaarheid van de doorvoer en service van de prestaties omdat de algehele doorvoer van een gepartitioneerde wachtrij of onderwerp niet meer wordt beperkt door de prestaties van een enkel bericht makelaar of SMS-winkel. Daarnaast zorg een tijdelijke storing van een SMS-archief niet een gepartitioneerde wachtrij of onderwerp niet beschikbaar. Zie [Messaging entiteiten partitioneren](https://msdn.microsoft.com/library/azure/dn520246.aspx)voor meer informatie.

### <a name="solution"></a>Oplossing

Het volgende codefragment ziet hoe u SMS-berichten entiteiten partitioneren.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Zie voor meer informatie [partitioneren Service Bus wachtrijen en onderwerpen | Microsoft Azure-Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) en check de steekproef [Microsoft Azure Service Bus partitioneren wachtrij](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) .

## <a name="do-not-set-sharedaccessstarttime"></a>Stel niet SharedAccessStartTime

### <a name="id"></a>ID

AP3001

### <a name="description"></a>Beschrijving

U moet SharedAccessStartTimeset vermijden naar de huidige tijd het beleid voor toegang tot gedeelde onmiddellijk te starten. Alleen moet u deze eigenschap instellen als u wilt het beleid voor toegang tot gedeelde starten op een later tijdstip.

Deel uw ideeën en feedback [Azure Code analyse feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Synchronisatie van de computerklok zorgt ervoor dat een lichte tijdverschil tussen datacenters. Bijvoorbeeld logisch daar de begintijd van een opslag SA's beleid instellen als de huidige tijd met behulp van DateTime.Now of een vergelijkbare manier wordt ertoe leiden dat het beleid SA's onmiddellijk wilt doorvoeren. Echter kunnen de lichte tijd verschillen tussen datacenters problemen veroorzaken met dit sinds enkele malen datacenter mogelijk iets hoger dan de begintijd, terwijl de anderen ligt deze. Hierdoor het beleid SA's kunt verloopt snel (of zelfs direct) als de levensduur van het beleid is ingesteld te kort.

Zie [Inleiding tot tabel SA's (gedeeld Access handtekening), wachtrij SA's en update van Blob SA's - teamblog van Microsoft Azure opslag - Site Start - MSDN-Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)voor meer informatie vinden over het gebruik van Access-handtekening gedeeld op Azure opslag.

### <a name="solution"></a>Oplossing

Verwijder de instructie die Hiermee stelt u de begintijd van het gedeelde-beleid. Het hulpmiddel voor Azure Code biedt een oplossing voor dit probleem. Voor meer informatie over de beveiligingsbeheer, raadpleegt u het ontwerppatroon [Valet sleutel patroon](https://msdn.microsoft.com/library/dn568102.aspx).

Het volgende codefragment ziet u de code-oplossing voor dit probleem.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Access-beleid verlooptijd meer dan vijf minuten moet worden gedeeld

### <a name="id"></a>ID

AP3002

### <a name="description"></a>Beschrijving

Kunnen er zo groot vijf minuten verschil in klok tussen datacenters op verschillende locaties vanwege een voorwaarde bekend als "klok scheefheid." Om te voorkomen dat de SA's instellen beleid token verloopt eerder dan gepland, de verlooptijd naar meer dan vijf minuten.

Deel uw ideeën en feedback [Azure Code analyse feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Datacenters op verschillende locaties overal ter wereld synchroniseren met een kloksignaal. Omdat het duurt voor kloksignaal naar verschillende locaties, kunnen er een afwijking van tijd tussen datacenters op verschillende geografische locaties maar alles zogenaamd wordt gesynchroniseerd. Dit tijdsverschil kan de toegang tot gedeelde beleid start tijd en de vervaldatum interval beïnvloeden. Daarom om ervoor te zorgen toegang tot gedeelde beleid onmiddellijk van kracht worden, geeft u de begintijd. Zorg bovendien dat de verlooptijd is meer dan 5 minuten om te voorkomen dat vroege time-out.

Zie [Inleiding tot tabel SA's (gedeeld Access handtekening), wachtrij SA's en update van Blob SA's - teamblog van Microsoft Azure opslag - Site Start - MSDN-Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)voor meer informatie over het gebruik van Access-handtekening gedeeld op Azure opslag.

### <a name="solution"></a>Oplossing

Zie voor meer informatie over beveiligingsbeheer het ontwerppatroon [Valet sleutel patroon](https://msdn.microsoft.com/library/dn568102.aspx).

Hier volgt een voorbeeld van een begintijd voor toegang tot gedeelde beleid niet op te geven.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Hier volgt een voorbeeld van het opgeven van de begintijd van een toegang tot gedeelde-beleid met een duur van beleid vervaldatum meer dan vijf minuten.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Zie voor meer informatie [maken en gebruiken van een handtekening Access gedeeld](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>CloudConfigurationManager gebruiken

### <a name="id"></a>ID

AP4000

### <a name="description"></a>Beschrijving

Met behulp van de klas [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) voor projecten zoals Azure Website en Azure mobiele services won't runtime problemen introduceren. Een aanbevolen om is deze echter een goed idee om Cloud[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) als een geïntegreerd manier voor het beheren van configuraties voor alle Azure Cloud-toepassingen gebruiken.

Deel uw ideeën en feedback [Azure Code analyse feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

CloudConfigurationManager leest het configuratiebestand die geschikt zijn voor de toepassingsomgeving.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Oplossing

Refactoring van de code als de [CloudConfigurationManager Class](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)wilt gebruiken. Een code-oplossing voor dit probleem is opgegeven door het hulpprogramma Azure Code analyse.

Het volgende codefragment ziet u de code-oplossing voor dit probleem. Vervangen

`var settings = ConfigurationManager.AppSettings["mySettings"];`

met

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Hier volgt een voorbeeld van de configuratie-instelling opslaan in een App.config of Web.config-bestand. De instellingen naar de sectie appSettings van het configuratiebestand toevoegen. Hierna volgt het bestand Web.config voor het vorige voorbeeld.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Verbindingstekenreeksen met de hard gecodeerde vermijden

### <a name="id"></a>ID

AP4001

### <a name="description"></a>Beschrijving

Als u hard gecodeerde verbindingstekenreeksen gebruikt en moet u deze later bijwerken, hebt u wijzigingen aanbrengen in uw broncode en de toepassing compileren. Echter als u uw verbindingstekenreeksen in een configuratiebestand opslaat, kunt u ze later door gewoon het configuratiebestand bij te werken.

Deel uw ideeën en feedback [Azure Code analyse feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Harde codering verbindingstekenreeksen met de is een ongeldige gewoonte omdat deze maakt u kennis met problemen wanneer de verbindingstekenreeksen moeten snel worden gewijzigd. Bovendien als het project worden ingecheckt op bronbeheer moet, introduceren verbindingstekenreeksen met de hard gecodeerde beveiligingsrisico's sinds de tekenreeksen in de broncode kunnen worden weergegeven.

### <a name="solution"></a>Oplossing

Sla verbindingstekenreeksen met de in de van configuratiebestanden of Azure-omgevingen.

- Voor zelfstandige-toepassingen, gebruikt u app.config om op te slaan verbindingsinstellingen voor een tekenreeks.

- Voor webtoepassingen IIS-die worden gehost, gebruikt u web.config voor de opslag van verbindingstekenreeksen met de.

- Voor ASP.NET vNext-toepassingen, gebruikt u configuration.json voor de opslag van verbindingstekenreeksen met de.

Zie voor informatie over het gebruik van configuraties bestanden zoals web.config of app.config [ASP.NET Web configuratie richtlijnen](https://msdn.microsoft.com/library/vstudio/ff400235(v=vs.100).aspx). Zie voor informatie over hoe Azure omgeving variabelen werken, [Azure-websites: hoe tekenreeksen van toepassingen en verbinding tekenreeksen werken](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Zie [geen vertrouwelijke informatie zoals verbindingstekenreeksen in bestanden die zijn opgeslagen in bron code opslagplaats providers](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)voor informatie over het opslaan van de verbindingsreeks in het besturingselement voor gegevensbronnen.

## <a name="use-diagnostics-configuration-file"></a>Diagnostisch hulpprogramma configuratiebestand gebruiken

### <a name="id"></a>ID

AP5000

### <a name="description"></a>Beschrijving

In plaats van het hulpprogramma's voor diagnose-instellingen configureren in uw code zoals met behulp van de Microsoft.WindowsAzure.Diagnostics programming API, moet u de instellingen diagnostische gegevens in het bestand diagnostics.wadcfg configureren. (Of diagnostics.wadcfgx als u Azure SDK 2,5 gebruikt). Dit doet, kunt u instellingen diagnostische gegevens wijzigen zonder dat u moet uw code compileren.

Deel uw ideeën en feedback [Azure Code analyse feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

Voordat Azure SDK 2,5 (die gebruikmaakt van Azure diagnostisch hulpprogramma 1.3), Azure diagnostische gegevens (af) kan worden geconfigureerd met behulp van verschillende methoden: toevoegen aan de configuratie blob in opslag, met behulp van dwingende code, declaratieve configuratie of de standaardconfiguratie. De beste manier voor het configureren van diagnostische gegevens is echter een XML-configuratiebestand (diagnostics.wadcfg of diagnositcs.wadcfgx voor SDK 2,5 en hoger) in het toepassingsproject gebruiken. In deze benadering kan het bestand diagnostics.wadcfg volledig definieert de configuratie en worden gewijzigd en bij wordt geïmplementeerd. Het gebruik van het configuratiebestand diagnostics.wadcfg mengen met het programma methoden voor het instelling configuraties met behulp van de klassen [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)of [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)kan leiden tot verwarring. Zie [initialisatie of configuratie van Azure diagnostische wijzigen](https://msdn.microsoft.com/library/azure/hh411537.aspx) voor meer informatie.

Beginnen met af 1.3 (inbegrepen in Azure SDK 2,5), is het niet meer mogelijk code gebruiken voor het configureren van diagnostische gegevens. Hierdoor kunt u alleen de configuratie bij het toepassen of bijwerken van de extensie diagnostische hulpprogramma's bieden.

### <a name="solution"></a>Oplossing

Gebruik de ontwerpfunctie van de configuratie diagnostisch hulpprogramma Diagnostische instellingen verplaatsen naar het configuratiebestand diagnostische gegevens (diagnositcs.wadcfg of diagnositcs.wadcfgx voor SDK 2,5 en hoger). Het verdient ook [Azure SDK 2,5](http://go.microsoft.com/fwlink/?LinkId=513188) te installeren en gebruiken van de meest recente hulpprogramma's voor diagnose-functie.

1. Kies Eigenschappen in het snelmenu voor de rol die u wilt configureren, en kies vervolgens het tabblad configuratie.

1. Zorg dat het selectievakje **Diagnostische hulpprogramma's inschakelen** is geselecteerd in de sectie **diagnostische hulpprogramma's** .

1. Kies de knop **configureren** .

  ![Toegang krijgen tot de optie inschakelen diagnostische gegevens](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

  Zie [Diagnostisch hulpprogramma voor Azure-Cloudservices en virtuele Machines configureren](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) voor meer informatie.


## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Voorkomen dat de objecten DbContext als statische declareren

### <a name="id"></a>ID

AP6000

### <a name="description"></a>Beschrijving

Als u wilt opslaan geheugen, geen DBContext objecten als statische declareren.

Deel uw ideeën en feedback [Azure Code analyse feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Reden

DBContext objecten houdt u de queryresultaten van elk gesprek. Statische DBContext objecten zijn niet verwijderd totdat het toepassingsdomein verwijderd wordt. Een statische DBContext-object kan dus grote hoeveelheden geheugen gebruiken.

### <a name="solution"></a>Oplossing

DBContext als een lokale variabele of de niet-statisch exemplaarveld declareren en laat deze worden verwijderd na gebruik deze gebruiken voor een taak.

Het volgende voorbeeld MVC controller class ziet u hoe u het object DBContext gebruikt.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over optimzing en probleemoplossing van Azure-apps, [problemen oplossen met een WebApp in Azure App Service Visual Studio](./app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).
