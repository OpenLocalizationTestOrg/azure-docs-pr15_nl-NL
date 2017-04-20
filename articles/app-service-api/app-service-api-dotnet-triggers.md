<properties 
    pageTitle="App-Service API app triggers | Microsoft Azure" 
    description="Triggers implementeren in een API-App in Azure App-Service" 
    services="logic-apps" 
    documentationCenter=".net" 
    authors="guangyang"
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="na" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="rachelap"/>

# <a name="azure-app-service-api-app-triggers"></a>Azure App Service API app-triggers

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op API apps 2014-12-01-schema voorbeeldversie.


## <a name="overview"></a>Overzicht

In dit artikel wordt uitgelegd hoe API app triggers implementeren en te gebruiken deze via een logica-app.

Alle van de codefragmenten in dit onderwerp worden gekopieerd van het [voorbeeld van de code FileWatcher API-App](http://go.microsoft.com/fwlink/?LinkId=534802). 

Opmerking die u de volgende nuget downloaden voor de code in dit artikel moet voor het maken en uitvoeren: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Wat zijn API app triggers?

Dit is een algemene scenario voor een API-app naar een gebeurtenis wordt gestart zodat clients van de API-app de juiste actie in antwoord op de gebeurtenis ondernemen kunnen. De REST API gebaseerd om die ondersteuning biedt voor dit scenario wordt een API-app-trigger genoemd. 

Stel dat bijvoorbeeld de [verbindingslijn API Twitter-app](../app-service-logic/app-service-logic-connector-twitter.md) wordt gebruikt door uw clientcode en uw code moet uitvoeren van een actie op basis van nieuwe tweets die bepaalde woorden bevatten. U kunt in dit geval een peiling of push-trigger instellen om deze behoefte te vereenvoudigen.

## <a name="poll-trigger-versus-push-trigger"></a>Peiling trigger versus push-trigger

Op dit moment worden twee soorten triggers ondersteund:

- Peiling trigger - Client controleert de API-app voor melding van een gebeurtenis die is gestart 
- Push-signaal - Client is een melding ontvangen door de app API wanneer een gebeurtenis wordt geactiveerd 

### <a name="poll-trigger"></a>Peiling-trigger

Een peiling-trigger als een gewone REST API is geïmplementeerd en de clients (zoals een app logica) verwacht poll om te krijgen van de melding. Terwijl de client staat behouden mogelijk, is de peiling-trigger zelf stateless. 

De volgende informatie over de vergaderverzoeken en antwoorden-pakketten illustreren enkele belangrijke aspecten van de peiling trigger-opdracht:

- Aanvragen
    - HTTP-methode: ophalen
    - Parameters
        - triggerState - deze optionele parameter kan clients om op te geven dat hun status zodat de peiling-trigger correct beslissen of u kunt wilt retourneren melding of niet op basis van de opgegeven staat.
        - API-specifieke parameters
- Antwoord
    - Status code **200** - aanvraag geldig is en er wordt een melding van de trigger. De inhoud van de melding is de hoofdtekst van het antwoord. Een koptekst 'Opnieuw-na' in het antwoord geeft aan dat aanvullende kennisgeving gegevens moeten worden opgehaald met een volgende aanvraag-oproep.
    - Status code **202** - aanvraag geldig is, maar er is geen nieuwe melding van de trigger.
    - Status code **4xx** - aanvraag is niet geldig. De client moet niet herhalen de aanvraag.
    - Status code **5xx** - verzoek heeft geleid tot een interne serverfout en/of tijdelijk probleem. De client moet herhalen de aanvraag.

Het volgende codefragment is een voorbeeld van het implementeren van een peiling-trigger.

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

Als u wilt testen deze peiling-trigger, als volgt te werk:

1. Implementeer de App API met een instelling voor de verificatie van **openbare anonieme**.
2. Bel de bewerking **Raak** zodat een bestand. De volgende afbeelding ziet u een voorbeeld van een aanvraag via Postman.
   ![Gesprek Touch bewerking via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. Bel de peiling-trigger met de parameter **triggerState** is ingesteld op een tijdstempel vóór stap #2. De volgende afbeelding ziet u het verzoek van de steekproef via Postman.
   ![Peiling Trigger via Postman bellen](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Push-trigger

Een push-signaal wordt geïmplementeerd als een gewone REST API waardoor meldingen aan clients die zich hebben geregistreerd wilt worden gewaarschuwd als er bepaalde gebeurtenissen gestart.

De volgende informatie over de vergaderverzoeken en antwoorden-pakketten illustreren enkele belangrijke aspecten van het contract push-trigger.

- Aanvragen
    - HTTP-methode: plaatsen
    - Parameters
        - triggerId: vereist - ondoorzichtig tekenreeks (bijvoorbeeld een GUID) die de registratie van een push-signaal vertegenwoordigt.
        - callbackUrl: vereist - URL van de terugbellen om aan te roepen wanneer de gebeurtenis plaatsvindt. De aanroep is een eenvoudige bericht HTTP-oproep.
        - API-specifieke parameters
- Antwoord
    - Status code **200** - aanvraag om deel te registreren van de client is geslaagd.
    - Status code **4xx** - aanvraag is niet geldig. De client moet niet herhalen de aanvraag.
    - Status code **5xx** - verzoek heeft geleid tot een interne serverfout en/of tijdelijk probleem. De client moet herhalen de aanvraag.
- Terugbellen
    - HTTP-methode: posten
    - Hoofdtekst aanvragen: inhoud van de melding.

Het volgende codefragment is een voorbeeld van hoe u een push-signaal implementeert:

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

Als u wilt testen deze peiling-trigger, als volgt te werk:

1. Implementeer de App API met een instelling voor de verificatie van **openbare anonieme**.
2. Blader naar [http://requestb.in/](http://requestb.in/) een RequestBin die als de URL van uw terugbellen fungeert maken.
3. Bel de push-trigger met een GUID als **triggerId** en de URL RequestBin als **callbackUrl**.
   ![Push-signaal via Postman bellen](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Bel de bewerking **Raak** zodat een bestand. De volgende afbeelding ziet u een voorbeeld van een aanvraag via Postman.
   ![Gesprek Touch bewerking via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Controleer de RequestBin om te bevestigen dat de push trigger terugbellen wordt aangeroepen met de eigenschap uitvoer.
   ![Peiling Trigger via Postman bellen](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Triggers in API definitie beschrijven

Nadat de triggers implementeren en implementeren van uw app API naar Azure, Ga naar het blad **API definitie** in de portal Azure preview en ziet u dat triggers automatisch worden herkend in de gebruikersinterface, die wordt bepaald door de definitie Swagger 2.0-API van de API-app.

![API definitie Blade](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Als u op de knop **Downloaden Swagger** en het bestand JSON Open, ziet u resultaten ongeveer als volgt uit:

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

De extensie eigenschap **x-ms-schedular-trigger** is hoe triggers worden beschreven in API definitie en worden automatisch toegevoegd door de API app gateway wanneer u de definitie API via de gateway aanvragen als het verzoek om een van de volgende criteria voldoet. (U kunt ook deze eigenschap handmatig toevoegen.)

- Peiling-trigger
    - Als de HTTP-methode **ophalen**.
    - Als de eigenschap **bewerkingsnummer** de tekenreeks- **trigger bevat**.
    - Als de eigenschap **parameters** een parameter met een eigenschap **naam** is ingesteld op **triggerState bevat**.
- Push-trigger
    - Als de HTTP-methode **plaatsen**.
    - Als de eigenschap **bewerkingsnummer** de tekenreeks- **trigger bevat**.
    - Als de eigenschap **parameters** een parameter met een eigenschap **naam** is ingesteld op **triggerId bevat**.

## <a name="use-api-app-triggers-in-logic-apps"></a>API-app triggers gebruiken in logica-apps

### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a>Een lijst met en API app triggers configureren in de ontwerpfunctie voor logica-apps

Als u een app logica in dezelfde resourcegroep als de API-app maakt, is mogelijk toe te voegen aan de ontwerpfunctie canvas gewoon door erop te klikken. De volgende afbeeldingen illustreren dit:

![Triggers in logica App Designer](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Peiling Trigger in logica App Designer configureren](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Push-signaal configureren in logica App Designer](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>API-app triggers voor logica apps optimaliseren

Nadat u triggers aan een API-app toevoegt, zijn er enkele dingen die u doen kunt om de verbetering van de gebruikerservaring bij gebruik van de API-app in een app logica.

Bijvoorbeeld, moet de parameter **triggerState** voor peiling triggers worden ingesteld op de volgende expressie in de logica-app. Deze expressie moet de laatste aanroep van de trigger vanuit de app logica evalueren en die waarde te retourneren.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

Opmerking: Voor een uitleg van de functies die worden gebruikt in de bovenstaande expressie, raadpleegt u de documentatie over [Logica App werkstroom Definition Language](https://msdn.microsoft.com/library/azure/dn948512.aspx).

Logica app-gebruikers moeten de expressie hierboven voor de parameter **triggerState** bij het gebruik van de trigger. Het is mogelijk dat deze waarde vooraf ingestelde door de ontwerper van de app logica tot en met de extensie eigenschap **x-ms-scheduler-aanbeveling**.  De eigenschap van de extensie **x-ms-zichtbaarheid** kan worden ingesteld op een waarde van *interne* zodat de parameter zelf niet in de ontwerpfunctie weergegeven wordt.  Het codefragment van de volgende ziet u dat.

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

Voor push-triggers, moet de parameter **triggerId** uniek zijn de logica-app. Een aanbevolen beste manier is om het instellen van deze eigenschap op de naam van de werkstroom met behulp van de volgende expressie:

    @workflow().name

De eigenschappen van de extensie **x-ms-scheduler-aanbeveling** en **x-ms-zichtbaarheid** in de definitie van de API gebruikt, kunt de API-app overbrengen naar de ontwerpfunctie van de app logica voor het automatisch instellen van deze expressie voor de gebruiker.

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a>Eigenschappen van de extensie in API-definition toevoegen

Extra metagegevens - gegevens, zoals extensie eigenschappen van **x-ms-scheduler-aanbeveling** en **x-ms-zichtbaarheid** - kan worden toegevoegd in de API-definition op twee manieren: statische of dynamische.

Voor statische metagegevens, kunt u rechtstreeks bewerken van het bestand */metadata/apiDefinition.swagger.json* in uw project en de eigenschappen handmatig toevoegen.

Voor API-apps met dynamische metagegevens, kunt u het bestand SwaggerConfig.cs om toe te voegen een bewerking filter die deze extensies kunt toevoegen.

    GlobalConfiguration.Configuration 
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


Hier volgt een voorbeeld van hoe deze klasse om te vergemakkelijken het dynamische metagegevens scenario kan worden geïmplementeerd.

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
 
