<properties 
    pageTitle="Wat is de SDK van Azure WebJobs" 
    description="Een inleiding tot de SDK van Azure WebJobs. Dit artikel wordt uitgelegd wat de SDK, typische scenario's dit is handig voor, en voorbeelden voor opmaakcode." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="what-is-the-azure-webjobs-sdk"></a>Wat is de SDK van Azure WebJobs

## <a id="overview"></a>Overzicht

In dit artikel wordt uitgelegd wat de WebJobs SDK is, reviseert enkele veelvoorkomende scenario's dit is handig voor, en geeft een overzicht van hoe u deze in uw code gebruiken.

[WebJobs](websites-webjobs-resources.md) is een functie van Azure App Service waarmee u een programma of script uitvoeren in dezelfde context als een WebApp, API-app of mobiele app. Het doel van de [WebJobs SDK](websites-webjobs-resources.md) is om te vereenvoudigen de code die u voor algemene taken schrijft dat een WebJob kunt uitvoeren, zoals de verwerking van afbeeldingen, in de wachtrij processing, RSS aggregatie onderhoud van bestanden en e-mailberichten verzenden. De WebJobs SDK heeft ingebouwde functies voor het werken met Azure opslagcapaciteit en de Service Bus, voor het plannen van taken en foutafhandeling en voor groot aantal andere gebruikelijke scenario's. Bovendien heeft deze zo ontworpen dat extensible. De die wordt [WebJobs SDK is bron openen](https://github.com/Azure/azure-webjobs-sdk/), en er is een [bron opslagplaats voor extensies openen](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

De WebJobs SDK bevat de volgende onderdelen:

* **NuGet-pakketten**. NuGet-pakketten die u aan een project Visual Studio-Console-toepassing toevoegt bieden een kader die uw code wordt gebruikt door uw methoden met WebJobs SDK kenmerken versieren.
  
* **Dashboard**. Onderdeel van de WebJobs SDK deel uitmaakt van Azure App-Service en vindt u uitgebreide controle en diagnostische hulpprogramma's voor het programma's die de NuGet-pakketten gebruiken. U hoeft niet te schrijven code voor het gebruik van deze functies voor controle en diagnostische gegevens.

## <a id="scenarios"></a>Scenario 's

Hier volgen enkele typische scenario's die met de SDK van Azure WebJobs kunt u er nog eenvoudiger afhandelen:

* Afbeelding van de verwerking of andere CPU veel werk. Een algemene functie van websites is de mogelijkheid voor het uploaden van afbeeldingen of video. Vaak wilt u de inhoud bewerken nadat deze wordt geüpload, maar u niet wilt dat de gebruiker wacht terwijl u dat doen maken.

* Wachtrij verwerking. Een gebruikelijke manier voor een frontend web om te communiceren met een backend-service is via wachtrijen. Wanneer de website uw werk moet, worden deze een bericht naar een wachtrij. Een back-end-service ophaalt berichten uit de wachtrij en het werk. U kunt wachtrijen voor de verwerking van afbeeldingen: bijvoorbeeld nadat de gebruiker een aantal bestanden uploadt, plaatst u de bestandsnamen op een bericht wachtrij om te worden opgehaald door de backend voor verwerking. Of u kunt wachtrijen om reactiesnelheid van de site. Bijvoorbeeld in plaats van schrijven rechtstreeks naar een SQL-database, schrijven naar een wachtrij, zien de gebruiker die u klaar bent en laat de backend-greep hoge latentie relationele database werken. Zie voor een voorbeeld van wachtrij verwerken met afbeelding proces, de [zelfstudie WebJobs SDK aan de slag](websites-dotnet-webjobs-sdk-get-started.md).

* RSS-aggregatie. Als u een site die u een lijst van RSS-feeds onderhoudt hebt, kunt u in alle artikelen uit de feeds in een achtergrondproces ophalen.

* Onderhoud van bestanden, zoals aggregeren of logboekbestanden opschonen.  Wellicht logboekbestanden wordt gemaakt door verschillende sites of voor afzonderlijke tijdsperioden die u wilt combineren om optimaal te analyse taken wordt uitgevoerd. Of u kunt een taak om uit te voeren om op te schonen oude logboekbestanden wekelijkse plannen.

* Ingress naar Azure tabellen. U mogelijk bestanden die zijn opgeslagen en BLOB's hebt en wilt deze parseren en opslag van de gegevens in tabellen. Een groot aantal rijen (miljoenen in sommige gevallen) kan schrijft u de functie ingress en de WebJobs SDK maakt het mogelijk deze functionaliteit eenvoudig implementeren. De SDK bevat ook realtime-controle van voortgangsindicatoren zoals het aantal rijen in de tabel zijn geschreven.

* Andere langdurige taken die u wilt uitvoeren in een achtergrondthread, zoals het [verzenden van e-mailberichten](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure). 

* Alle taken die u uitvoeren op een schema wilt, zoals een back-up-bewerking nachts.

In veel van deze scenario's wilt u mogelijk de schaal van een web-app uitvoeren op meerdere VMs, die in meerdere WebJobs tegelijk aanpassen. In sommige gevallen kan dit resulteren in dezelfde gegevens meerdere keren verwerkt, maar dit probleem wordt veroorzaakt door een wanneer u de ingebouwde wachtrij, blob en Service Bus triggers van de WebJobs SDK gebruikt. De SDK zorgt ervoor dat uw functies slechts één keer worden verwerkt voor elk bericht of blob.

De WebJobs SDK ook eenvoudig worden afgehandeld veelvoorkomende scenario's voor foutafhandeling. U kunt waarschuwingen instellen voor het verzenden van meldingen wanneer een functie mislukt en u time-out instellen kunt zodat een functie wordt automatisch geannuleerd als deze niet voltooid binnen een bepaalde termijn.

## <a id="code"></a>Voorbeelden van programmacode

De code voor het afhandelen van veelvoorkomende taken die met Azure Storage werken is eenvoudig. In uw Console-toepassing `Main` methode die u maakt een `JobHost` object dat het aanroepen van methoden schrijft u coördinaten. Het kader WebJobs SDK weet wanneer uw methoden aanroepen en wat parameterwaarden gebruiken op basis van de WebJobs SDK kenmerken die u in deze gebruiken. De SDK bevat *triggers* die opgeven wat voorwaarden ertoe leiden dat de functie die wordt aangeroepen en *modellen* die om gegevens in- en afmelden bij methodeparameters opgeeft.

Het kenmerk [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) wordt bijvoorbeeld een functie moet worden aangeroepen wanneer een bericht is ontvangen op een wachtrij en als de berichtindeling JSON voor een matrix van bytes of een aangepast type, het bericht automatisch gedeserialiseerde is. Het kenmerk [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) gebeurtenis een proces wordt als een nieuwe blob wordt gemaakt in een opslag van Azure-account.

Hier volgt een eenvoudig programma waarmee een wachtrij worden opgevraagd en Hiermee maakt u een blob voor elk ontvangen bericht voor wachtrij:

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

De `JobHost` object is een container voor een aantal functies van de achtergrond. De `JobHost` object bewaakt de functies, controles voor gebeurtenissen die door deze wordt geactiveerd en voert u de functies wanneer trigger gebeurtenissen plaatsvinden. U belt een `JobHost` methode om aan te geven of u wilt dat de container proces om uit te voeren op de huidige thread of thread op de achtergrond. In het voorbeeld wordt de `RunAndBlock` methode is het proces continu worden uitgevoerd op de huidige thread.

Omdat de `ProcessQueueMessage` methode in dit voorbeeld heeft een `QueueTrigger` kenmerk, de trigger voor deze functie is de ontvangst van een nieuw bericht in de wachtrij. De `JobHost` object wacht op nieuwe berichten op de opgegeven wachtrij ('webjobsqueue' in dit voorbeeld) en wanneer een wordt gevonden, wordt aangeroepen `ProcessQueueMessage`. 

De `QueueTrigger` kenmerk bindingen de `inputText` -parameter voor de waarde van het bericht wachtrij. En de `Blob` kenmerk bindingen een `TextWriter` object naar een blob met de naam 'blobname' in een container met de naam "containername".  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

Deze parameters vervolgens wordt de waarde van het bericht wachtrij naar de blob schrijven:

        writer.WriteLine(inputText);

De trigger en binder functies van de WebJobs SDK vereenvoudigen de code die u hebt om te schrijven. De laag niveau code vereist om te verwerken wachtrijen, BLOB's of bestanden of om te starten geplande taken, wordt uitgevoerd voor u door het kader WebJobs SDK. Bijvoorbeeld het kader maakt wachtrijen die niet, wordt geopend bestaat de wachtrij, gelezen berichten wachtrij, verwijderen in de wachtrij berichten wanneer verwerking is voltooid, wordt gemaakt van blob containers die niet voorkomen nog, gegevens worden geschreven naar BLOB's, enzovoort.

Het volgende voorbeeld ziet u een aantal triggers in één WebJob: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, en `ErrorTrigger`. 

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

        public static void ErrorMonitor(
        [ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
        [SendGrid(
            To = "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors to the Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a id="schedule"></a>Plannen

De `TimerTrigger` kenmerk kunt u de trigger functies uit te voeren op een planning. U kunt een WebJob plannen als geheel tot en met Azure of planning afzonderlijke functies van een WebJob met de SDK WebJobs `TimerTrigger`. Hier volgt een codevoorbeeld.

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

Zie voor meer voorbeelden van code, [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) in de bibliotheek azure-webjobs-sdk-extensies op GitHub.com.

## <a name="extensibility"></a>Uitbreiden

De WebJobs SDK kunt dat u bent niet beperkt tot ingebouwde functionaliteit--u aangepaste triggers en modellen schrijven.  U kunt bijvoorbeeld triggers voor cachegebeurtenissen en periodiek planningen schrijven. Een [bron-bibliotheek openen](https://github.com/Azure/azure-webjobs-sdk-extensions) , bevat een [gedetailleerde uitleg op WebJobs SDK uitbreiden](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) en steekproef code waarmee u aan de slag met het schrijven van uw eigen triggers en modellen.

## <a id="workerrole"></a>Met behulp van de SDK WebJobs buiten WebJobs

Een programma waarin het de SDK WebJobs is een standaard-Console-toepassing en kan worden uitgevoerd overal--deze geen uitvoeren als een WebJob. U kunt het programma lokaal testen op de computer en in productie u kan worden uitgevoerd in een Cloudservice werknemer rol of een Windows-service als u liever een van deze omgevingen. 

Het dashboard is echter alleen beschikbaar als extensie voor een web-app van Azure App-Service. Als u wilt uitvoeren buiten een WebJob en nog steeds gebruiken het Dashboard, kunt u een web-app als u wilt gebruiken, hetzelfde opslag-account dat de verbindingsreeks WebJobs SDK Dashboard verwijst naar, en dat gegevens over een functie wordt uitgevoerd vanuit het programma dat wordt uitgevoerd ergens anders klikt, WebApp WebJobs Dashboard wordt weergegeven. U gaat naar het Dashboard met behulp van de URL https://*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions. Voor meer informatie [een dashboard voor lokale ontwikkeling met de SDK WebJobs](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx)ziet, maar houd er rekening mee dat het blogbericht ziet u de naam van een oude verbinding tekenreeks. 

## <a id="nostorage"></a>Dashboardfuncties

De WebJobs SDK biedt diverse voordelen, zelfs als u niet WebJobs SDK triggers of modellen gebruikt:

* U kunt functies vanuit het Dashboard aanroepen.
* U kunt functies vanuit het Dashboard opnieuw afspelen.
* U kunt Logboeken weergeven in het Dashboard, gekoppeld aan de bepaalde WebJob (toepassingslogboeken, geschreven met behulp van Console.Out, Console.Error, doelcellen, enzovoort) of gekoppeld aan de specifieke functie-aanroep die ze gegenereerd (Logboeken geschreven met behulp van een `TextWriter` object dat de SDK aan de functie als een parameter doorgegeven). 

Zie voor meer informatie [hoe u handmatig een functie roepen](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) en [hoe u Logboeken schrijft](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>Volgende stappen

Zie voor meer informatie over de WebJobs SDK, [Azure WebJobs aanbevolen Resources](http://go.microsoft.com/fwlink/?linkid=390226).

Zie voor informatie over de meest recente verbeteringen aan de WebJobs SDK, de [Releaseopmerkingen](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).
 
