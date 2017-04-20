<properties
    pageTitle="Uitvoer naar een Azure bestand Vgx. Cache, met Azure-functies, in Azure Stream Analytics | Microsoft Azure"
    description="Informatie over het gebruik van een functie Azure verbonden een Service Bus wachtrij, in te vullen een Azure bestand Vgx. Cache uit de resultaten van een taak Stream Analytics."
    keywords="gegevensstroom, bestand Vgx. cache, service bus wachtrij"
    services="stream-analytics"
    authors="ryancrawcour"
    manager="jhubbard"
    documentationCenter=""
    />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/26/2016"
    ms.author="ryancraw"/>

# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Gegevens van Azure Stream analyseprogramma opslaan in een Azure bestand Vgx. in Cache met Azure-functies

Azure Stream Analytics kunt u heel snel ontwikkelen en lage kosten oplossingen realtime inzicht van apparaten, sensoren, infrastructuur en toepassingen of een gegevensstroom krijgen. U kunt verschillende gebruik zaken zoals realtime management en de opdracht en controle, detectie van fraude, verbonden auto's en nog veel meer. Bij veel scenario's wilt u mogelijk opslag van gegevens die zijn gegenereerd door Azure Stream Analytics in een winkel gedistribueerde gegevens zoals een cache Azure bestand Vgx..

Stel dat deel uitmaakt van een bedrijf telecommunicatie. U probeert te detecteren SIM fraude waar meerdere oproepen die afkomstig zijn uit dezelfde identiteit, tegelijk tijd, maar in verschillende geografisch locaties. U zijn met name alle mogelijke frauduleuze telefoongesprekken opslaan in een cache Azure bestand Vgx.. In dit blog geven we richtlijnen voor hoe kunt u eenvoudig uw taak voltooien. 

## <a name="prerequisites"></a>Vereisten voor
Voltooi de [Real-time fraude detectie] [ fraud-detection] overzicht voor ASA

## <a name="architecture-overview"></a>Overzicht van de architectuur
![Schermafbeelding van de architectuur](./media/stream-analytics-functions-redis/architecture-overview.png)

Zoals u in de bovenstaande afbeelding, kunnen Stream Analytics streaming invoergegevens worden opgevraagd en verzonden naar een uitvoer. Op basis van de uitvoer, kan Azure-functies vervolgens resulteren in een type gebeurtenis. 

In deze blog, richten we ons op het gedeelte Azure functies van deze verkooppijplijn of specifieker de activeert van een gebeurtenis die frauduleus gegevens worden opgeslagen in de cache.
Na het voltooien van de [Detectie van fraude Real-time] [ fraud-detection] zelfstudie hebt u een invoer (een gebeurtenis hub), een query en een uitvoer (blobopslag) al geconfigureerd en uit te voeren. In dit blog wijzigen we de uitvoer als u wilt een Service Bus wachtrij in plaats daarvan gebruiken. Daarna verbinden we met een functie Azure deze wachtrij. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Maken en verbindt u de uitvoer van een Service Bus wachtrij
Maak een Service Bus-wachtrij, volgt u stap 1 en 2 van de sectie .NET in [Aan de slag met Service Bus wachtrijen][servicebus-getstarted].
Nu we de wachtrij verbinden met de Stream Analytics-taak die is gemaakt in de eerdere fraude detectie overzicht.



1. Klik in de portal Azure Ga naar het blad **uitvoer** van uw werk en selecteert u **toevoegen** aan de bovenkant van de pagina.

    ![Uitvoer van toe te voegen](./media/stream-analytics-functions-redis/adding-outputs.png)

2. Kies **Service Bus wachtrij** als de **vangen** en volg de instructies op het scherm. Kies de naamruimte van de Service Bus wachtrij die u hebt gemaakt in [Aan de slag met Service Bus wachtrijen][servicebus-getstarted]. Klik op de knop 'BAS' wanneer u klaar bent.
3. Geef de volgende waarden:
    - **Gebeurtenis serialisatiefunctie opmaken**: JSON
    - **Codering**: UTF8
    - **Opmaak**: regel gescheiden

4. Klik op de knop **maken** en deze bron en om te bevestigen dat Stream Analytics succes verbinding met het account opslag maken kunt.

5. Vervang in het tabblad **Query** op de huidige query met de volgende items. *[Uw BUS SERVICENAAM]* vervangen door de uitvoernaam van de die u hebt gemaakt in stap 3. 

    ```    

        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2

        INTO [YOUR SERVICE BUS NAME]
    
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
    
        WHERE CS1.SwitchNum != CS2.SwitchNum
    
    ```

## <a name="create-an-azure-redis-cache"></a>Maken van een Cache Azure bestand Vgx.
Maken van een cache Azure bestand Vgx. volgens de sectie .NET in [hoe u gebruik Azure bestand Vgx. Cache] [ use-rediscache] totdat de sectie ***de cache-clients configureren aangeroepen***.
Als voltooid, hebt u een nieuw bestand Vgx. Cache. Schakel onder **alle instellingen** **toegangstoetsen** en noteer de ***primaire verbindingsreeks***.

![Schermafbeelding van de architectuur](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Maken van een Azure-functie
Ga als volgt [maken uw eerste Azure-functie] [ functions-getstarted] zelfstudie aan de slag met Azure-functies. Als u al een Azure functie die u wilt gebruiken, klikt u vervolgens gaat u verder met [het schrijven naar Cache bestand Vgx.](#Writing-to-Redis-Cache)

1. Klik in de portal App Services Selecteer in het linkernavigatievenster en klik op de naam van uw Azure functie app naar van de functie app website te gaan.
    ![Schermafbeelding van de lijst met de functie van de Apps services](./media/stream-analytics-functions-redis/app-services-function-list.png)

2. Klik op **nieuwe functie > ServiceBusQueueTrigger – C#**. Volg deze instructies voor de volgende velden:
    - **De naam van de wachtrij**: dezelfde naam als de naam die u hebt ingevoerd wanneer u de wachtrij in [Aan de slag met Service Bus wachtrijen gemaakt] [ servicebus-getstarted] (niet de naam van de service-bus). Controleer of dat u de wachtrij die is gekoppeld aan de Stream Analytics-uitvoer gebruikt.
    - **Verbinding Service Bus**: Selecteer **een verbindingsreeks toevoegen**. Als u de verbindingsreeks zoekt, gaat u naar de klassieke-portal, selecteert u **Service Bus**, de service-bus die u hebt gemaakt en **Informatie over de DATABASEVERBINDING** onderaan in het scherm. Controleer of dat u bent in het hoofdscherm op deze pagina. Kopieer en plak de verbindingsreeks. Je mag rustig Voer de verbindingsnaam van een.
    
        ![Schermafbeelding van de service bus verbinding](./media/stream-analytics-functions-redis/servicebus-connection.png)
    - **AccessRights**: kies **beheren**


3. Klik op **maken**

## <a name="writing-to-redis-cache"></a>Schrijven naar het bestand Vgx Cache.
We hebben nu een Azure-functie die van een Service Bus wachtrij wordt gemaakt. Alle die er nog moet gebeuren is onze functie gebruiken bij het wegschrijven van deze gegevens aan de Cache bestand Vgx.. 

1. Selecteer de zojuist gemaakte **ServiceBusQueueTrigger**en klikt u op **instellingen van de functie-app** in de rechterbovenhoek. Selecteer **gaat u naar de App-Service-instellingen > Instellingen > Toepassingsinstellingen**

2. Klik in de sectie verbinding tekenreeksen een naam in de sectie **naam** te maken. Plak de primaire verbindingsreeks die u in de stap **maken een Cache bestand Vgx.** in de sectie **waarde gevonden** . Selecteer **aangepaste** waar **SQL-Database**staat.

3. Klik op **Opslaan** aan de bovenkant.

    ![Schermafbeelding van de service bus verbinding](./media/stream-analytics-functions-redis/function-connection-string.png)

4. Nu gaat u terug naar de App-Service-instellingen en selecteer **Hulpmiddelen > App Service Editor (Preview) > op > Ga**.

    ![Schermafbeelding van de service bus verbinding](./media/stream-analytics-functions-redis/app-service-editor.png)

5. Klik in een editor van uw keuze een JSON-bestand met de naam **project.json** met de volgende items maken en opslaan op uw lokale schijf.

        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }

6. Dit bestand niet uploaden naar de hoofdmap van de functie (niet WWWROOT). U ziet een bestand met **project.lock.json** automatisch de naam wordt weergegeven, de acceptatie van dat de Nuget pakketten "StackExchange.Redis" en "Newtonsoft.Json" zijn geïmporteerd.

7. Klik in het bestand **run.csx** , kunt u de vooraf gegenereerde code vervangen door de volgende code. Vervang "CONN naam" met de naam die u hebt gemaakt in stap 2 van de **gegevens op te slaan in de cache bestand Vgx.**in de functie lazyConnection.

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;
    
    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");
    
        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-the-stream-analytics-job"></a>Start de Stream Analytics-taak

1. Start de toepassing telcodatagen.exe. Het gebruik is als volgt:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````

2. Klik op het blad Stream Analytics taak in de portal, op **starten** bij het begin van de pagina.

    ![Schermafbeelding van de starttaak](./media/stream-analytics-functions-redis/starting-job.png)

3. Selecteer **nu** in het blad voor **taak starten** dat wordt weergegeven, en klik vervolgens op de knop **Start** onderaan in het scherm. De taakstatuswijzigingen in de status om te beginnen en na enkele wijzigingen aan de slag.
 
    ![Schermafbeelding van start taak tijd selectie](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Oplossing uitvoeren en resultaten controleren
Teruggaan naar de pagina **ServiceBusQueueTrigger** , moet er nu log-instructies. Deze logboeken weergeven die u hebt u een ander nummer uit de Service Bus wachtrij en zet deze in de database, en deze via de tijd als de sleutel opgehaald!

Om te bevestigen dat uw gegevens zich in de cache bestand Vgx., gaat u naar uw cache bestand Vgx. pagina in de nieuwe portal (zoals u in stap [maken een Cache Azure bestand Vgx.](#Create-an-Azure-Redis-Cache) ) en selecteer Console.

Nu kunt u het bestand Vgx. opdrachten om te bevestigen dat zich in feite gegevens in de cache schrijven.

![Schermafbeelding van Console bestand Vgx.](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Volgende stappen
We enthousiast over de nieuwe zaken aan bod Azure-functies en we hopen dat u ontgrendelt Hiermee nieuwe mogelijkheden voor u Stream analytics samen kunt doen. Hebt u eventuele feedback over wat u vervolgens wilt, altijd de [site van Azure UserVoice](https://feedback.azure.com/forums/270577-stream-analytics)gebruiken.

Als u nieuwe Microsoft Azure, uitnodigen we u kunt uitproberen door zich registreert voor een [gratis proefabonnement Azure-account](https://azure.microsoft.com/pricing/free-trial/). Als u eerder met Stream analyses, klikt u vervolgens nodigen we u [uw eerste Stream Analytics-taak](stream-analytics-create-a-job.md)maken.

Als u meer nodig hebt help of hebt u vragen, posten op [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) - of [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) forums. 

U ziet ook de volgende bronnen:

- [Azure naslaginformatie voor ontwikkelaars van functies](../azure-functions/functions-reference.md)
- [Azure functies C# naslaginformatie voor ontwikkelaars](../azure-functions/functions-reference-csharp.md)
- [Azure functies F # naslaginformatie voor ontwikkelaars](../azure-functions/functions-reference-fsharp.md)
- [Azure functies NodeJS-naslaginformatie voor ontwikkelaars](../azure-functions/functions-reference.md)
- [Azure functies triggers en bindingen](../azure-functions/functions-triggers-bindings.md)
- [Het controleren van de Cache van Azure bestand Vgx.](../redis-cache/cache-how-to-monitor.md)

Als u wilt bijhouden op het laatste nieuws en functies, volgt u [@AzureStreaming](https://twitter.com/AzureStreaming) op Twitter.


[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
