<properties
 pageTitle="Scheduler concepten, termen en entiteiten | Microsoft Azure"
 description="Azure Scheduler concepten, terminologie en entiteitenhiërarchie, inclusief taken en verzamelingen van de taak.  Ziet u een volledig voorbeeld van een geplande taak."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-concepts-terminology--entity-hierarchy"></a>Scheduler concepten, terminologie + entiteitenhiërarchie

## <a name="scheduler-entity-hierarchy"></a>Scheduler entiteitenhiërarchie

De volgende tabel worden de belangrijkste bronnen die worden aangeboden of die wordt gebruikt door de API Scheduler beschreven:

|Resource | Beschrijving |
|---|---|
|**Taak-siteverzameling**|Een verzameling taak bevat een aantal taken en onderhoudt instellingen, quota en throttles die worden gedeeld door taken binnen de verzameling. Een verzameling taak is gemaakt door de eigenaar van een abonnement en taken bij elkaar op basis van gebruik of een toepassing grenzen van groepen. Het is beperkt tot één regio. Daarnaast kunt u het afdwingen van quota als u het gebruik van alle taken in die verzameling wilt beperken. De quota opnemen MaxJobs en MaxRecurrence.|
|**Taak**|Een taak definieert een enkele terugkerende actie, met eenvoudige of complexe strategieën voor het uitvoeren van. Acties kunnen onder andere bestaan HTTP, opslag wachtrij, service bus wachtrij of serviceaanvragen bus onderwerp.|
|**Overzicht van functies**|Geschiedenis van een staat voor meer informatie voor de uitvoering van een taak. De presentatie bevat success versus mislukt, evenals de details van een antwoord.|

## <a name="scheduler-entity-management"></a>Scheduler entiteit-beheer

Laten zien de volgende bewerkingen uit op de bronnen op hoog niveau, de planner en de API voor het servicebeheer:

|De mogelijkheid|Beschrijving en URI adres|
|---|---|
|**Taakbeheer van de siteverzameling**|U, plaatsen en ondersteuning voor het maken en wijzigen van de taak verzamelingen en de taken die daarin verwijderd. Een siteverzameling van de taak is een container voor functies en kaarten quota en gedeelde instellingen. Voorbeelden van quota, verderop, zijn maximum aantal taken en kleinste terugkeerpatroon. <p>PLAATSEN en verwijderen:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p><p>Toevoegen:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p>
|**Taakbeheer**|U, plaatsen, zet, PATCH en ondersteuning voor het maken en wijzigen van taken verwijderen. Alle taken moeten behoren tot de siteverzameling van een taak die al bestaat, dus er geen impliciete maken is. <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p>|
|**Geschiedenis van Taakbeheer**|ONDERSTEUNING voor het ophalen van 60 dagen van de uitvoering werkervaring, zoals taak verstreken tijd en taak kan worden uitgevoerd resultaten. Query tekenreeks parameter ondersteuning voor filteren op basis van de staat en status toevoegen. <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p>|

## <a name="job-types"></a>Taaktypen

Er zijn meerdere soorten taken: HTTP-taken (met inbegrip van HTTPS-taken die ondersteuning bieden voor SSL), opslag wachtrijtaken, service bus wachtrijtaken en service bus onderwerp taken. HTTP-taken zijn ideaal als er een eindpunt van een bestaande werklast of service. U kunt opslagruimte wachtrijtaken gebruiken om berichten te posten naar opslag wachtrijen, zodat deze taken zijn ideaal voor werkbelasting die opslag wachtrijen gebruiken. Daarnaast zijn de service bus taken ideaal voor werkbelasting die service bus wachtrijen en onderwerpen gebruiken.

## <a name="the-job-entity-in-detail"></a>De entiteit 'taak' in detail

Een geplande taak bevat op een eenvoudige niveau, de verschillende onderdelen:

- De actie moet worden uitgevoerd wanneer de taak timer wordt uitgevoerd  

- (Optioneel) De tijd aan de taak uitvoeren  

- (Optioneel) Wanneer en hoe vaak de taak herhalen  

- (Optioneel) Een actie moet worden gestart als de primaire actie mislukt  

Intern, bevat een geplande taak ook systeem gegevens zoals de volgende keer dat geplande worden uitgevoerd.

De volgende code biedt een uitgebreide voorbeeld van een geplande taak. Details worden in de volgende secties gegeven.

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often to fire (default to 1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default to recur infinitely)
            "endTime": "2012-11-04",                     // optional (default to recur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

Zoals gezien in de steekproef geplande taak bovenstaande, bevat de verschillende onderdelen van de taakdefinitie van een:

- Begintijd ("starttijd")  

- Actie (""), waaronder fout actie ("errorAction")

- Terugkeerpatroon ("terugkeerpatroon")  

- Status ("status")  

- Status ("status")  

- Probeer beleid ("retryPolicy")  

We bekijken deze uitgebreid beschreven:

## <a name="starttime"></a>Starttijd

De "starttijd" is de begintijd en kan de beller een verschuiving tijdzone op de kabel in [ISO-8601-notatie](http://en.wikipedia.org/wiki/ISO_8601)opgeven.

## <a name="action-and-erroraction"></a>actie en errorAction

De "bewerking" is de actie die wordt gestart op elk exemplaar en een beschrijving van een type aanroep van de service. De actie is wat wordt uitgevoerd op de meegeleverde planning. Scheduler ondersteunt HTTP, opslag wachtrij service bus onderwerp en service bus wachtrij acties.

De actie in het bovenstaande voorbeeld is een HTTP-actie. Hieronder volgt een voorbeeld van een opslag wachtrij actie:

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

Hieronder volgt een voorbeeld van een service bus onderwerp actie.

  "actie": {"type": "serviceBusTopic", "serviceBusTopicMessage": {"topicPath": "t 1",  
      "naamruimte": "mySBNamespace", "transportType": "netMessaging", / / netMessaging of AMQP 'verificatie' kan zijn: {"sasKeyName": "QPolicy", "type": "sharedAccessKey"}, "bericht": "Sommige bericht", "brokeredMessageProperties": {}, "customMessageProperties": {"toepassingsnaam": "FromScheduler"}},}

Hieronder volgt een voorbeeld van een service bus wachtrij actie:


  "actie": {"serviceBusQueueMessage": {"wachtrijnaam": 'k1',  
      "naamruimte": "mySBNamespace", "transportType": "netMessaging", / / netMessaging of AMQP 'verificatie' kan zijn: {  
        "sasKeyName": "QPolicy", "type": "sharedAccessKey"}, "bericht": "Sommige bericht"  
      "brokeredMessageProperties": {}, "customMessageProperties": {"toepassingsnaam": "FromScheduler"}}, "type": "serviceBusQueue"}

De "errorAction" is de foutafhandeling, de actie die wordt gestart wanneer de primaire actie mislukt. U kunt deze variabele een eindpunt foutverwerking belt of het verzenden van een gebruikersmelding. Dit kan worden gebruikt voor het bereiken van een secundaire eindpunt in het geval dat de primaire niet beschikbaar is (bijvoorbeeld in geval van een voordoet op de site van het eindpunt) is of kan worden gebruikt voor een eindpunt voor foutafhandeling de hoogte te stellen. Net als de primaire actie, kan de fout actie enkelvoudige of samengestelde logica op basis van andere acties zijn. Meer informatie over het maken van een token SA's, raadpleegt u [maken en gebruiken van een handtekening Access gedeeld](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="recurrence"></a>terugkeerpatroon

Terugkeerpatroon bevat de verschillende onderdelen:

- Frequentie: Een van de minuut, uur, dag, week, maand, jaar  

- Interval: Interval voor met de opgegeven frequentie voor het terugkeerpatroon  

- Vooraf aangegeven planning: minuten, uren, weekdagen, maanden en monthdays van het terugkeerpatroon opgeven  

- Tellen: Het aantal exemplaren  

- Eindtijd: geen taken na de opgegeven eindtijd wordt uitgevoerd  

Een taak terugkerend als er een terugkerende object dat is opgegeven in de JSON-definitie. Als zowel aantal- en eindtijd zijn opgegeven, wordt de aanvulling regel die eerst voorkomt geaccepteerd.

## <a name="state"></a>de staat

De status van de taak is een van de vier waarden: ingeschakeld, uitgeschakeld, voltooid of mislukt. U kunt plaatsen of PATCH taken kunnen worden bijgewerkt om ze naar de status ingeschakeld of uitgeschakeld. Als een taak is voltooid of mislukt, dat wil zeggen een definitieve staat die niet kan worden bijgewerkt (dat de taak kan nog steeds worden verwijderd). Een voorbeeld van de eigenschap state is als volgt:


        "state": "disabled", // enabled, disabled, completed, or faulted
Voltooide en mislukte taken worden na 60 dagen verwijderd.

## <a name="status"></a>status

Nadat een geplande taak is gestart, wordt informatie over de huidige status van de taak als resultaat gegeven. Dit object kan niet worden ingesteld door de gebruiker, dit ingesteld door het systeem. Dit is echter opgenomen in het project-object (in plaats van een afzonderlijke gekoppelde resource) zodat u kunt een eenvoudig de status van een taak aanvragen.

Taakstatus bevat de tijd van de vorige uitvoering (indien aanwezig), het tijdstip waarop het volgende geplande is uitgevoerd (voor taken in uitvoering) en het aantal uitvoering van de taak.

## <a name="retrypolicy"></a>retryPolicy

Als een geplande taak mislukt, is het mogelijk om op te geven van een beleid opnieuw om te bepalen of en hoe de actie opnieuw wordt gestart. Dit wordt bepaald door het object **retryType** : dit is ingesteld op **geen als er geen beleid opnieuw, zoals hierboven** . Stel deze in op **vaste** als er een beleid opnieuw is.

Kunt u een beleid opnieuw instellen, kunnen u twee aanvullende instellingen opgeven: een interval voor nieuwe pogingen (**retryInterval**) en het aantal pogingen (**retryCount**).

Het interval voor nieuwe pogingen, opgegeven met het object **retryInterval** , is het interval tussen nieuwe pogingen. De standaardwaarde is 30 seconden, de configureerbare minimumwaarde is 15 seconden en de maximumwaarde is 18 maanden. Taken in gratis taak collecties hebben een configureerbare minimumwaarde 1 uur.  Deze is gedefinieerd in het ISO-8601-notatie. Op dezelfde manier is de waarde van het aantal pogingen opgegeven met het object **retryCount** . is het aantal keren die een nieuwe poging wordt gedaan. De standaardwaarde 4 is, en de maximale waarde is 20\. beide **retryInterval** en **retryCount** zijn optioneel. Ze zijn hun standaard als **retryType** is ingesteld op **vast** en geen waarden expliciet zijn opgegeven doorgegeven.

## <a name="see-also"></a>Zie ook

 [Wat is Scheduler?](scheduler-intro.md)

 [Aan de slag met Scheduler in de portal van Azure](scheduler-get-started-portal.md)

 [Abonnementen en facturering in Azure Scheduler](scheduler-plans-billing.md)

 [Het maken van complexe planningen en geavanceerde terugkeerpatroon met Azure Scheduler](scheduler-advanced-complexity.md)

 [Azure Scheduler REST API-verwijzing](https://msdn.microsoft.com/library/mt629143)

 [Overzicht van Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Azure Scheduler hoge beschikbaarheid en betrouwbaarheid](scheduler-high-availability-reliability.md)

 [Azure Scheduler limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler uitgaande verificatie](scheduler-outbound-authentication.md)
