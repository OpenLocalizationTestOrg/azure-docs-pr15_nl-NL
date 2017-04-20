<properties 
    pageTitle="Het gebruik van de Service Bus onderwerpen met Node.js | Microsoft Azure" 
    description="Leer hoe u de Service Bus onderwerpen en abonnementen gebruiken in Azure wordt aangegeven via een Node.js-app." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Hoe gebruik Service Bus onderwerpen en abonnementen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Deze handleiding wordt beschreven hoe Service Bus onderwerpen en abonnementen vanuit Node.js toepassingen gebruiken. De scenario's waarvoor zijn **maken van onderwerpen en abonnementen**, **abonnement filters maken**, **verzenden van berichten** naar een onderwerp, **ontvangen van berichten van een abonnement**en **verwijderen van onderwerpen en abonnementen**. Zie voor meer informatie over onderwerpen en abonnementen, de sectie van de [volgende stappen](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Een Node.js-servicetoepassing maken

Een lege Node.js-toepassing maken. Zie voor instructies over het maken van een toepassing voor de Node.js [maken en implementeren van een toepassing voor de Node.js naar een website van Azure], [Node.js Cloudservice][] met behulp van Windows PowerShell of website met WebMatrix.

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing Service Bus configureren

Als u wilt gebruiken Service Bus, het Node.js Azure-pakket te downloaden. Dit pakket bevat een bibliotheken die met de Service Bus REST-services communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Gebruik knooppunt pakket Manager (NPM) het pakket verkrijgen

1.  Een opdrachtregel-interface gebruiken zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix), navigeer naar de map waar u uw voorbeeldtoepassing hebt gemaakt.

2.  Typ **npm azure installeren** in het opdrachtvenster, waardoor het moet de volgende uitvoer:

    ```
        azure@0.7.5 node_modules\azure
    ├── dateformat@1.0.2-1.2.3
    ├── xmlbuilder@0.4.2
    ├── node-uuid@1.2.0
    ├── mime@1.2.9
    ├── underscore@1.4.4
    ├── validator@1.1.1
    ├── tunnel@0.0.2
    ├── wns@0.5.3
    ├── xml2js@0.2.7 (sax@0.5.2)
    └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```

3.  U kunt handmatig uitvoeren met de opdracht **ls** om te controleren of die een **knooppunt\_modules** map is gemaakt. Zoek het **azure** -pakket waarin de bibliotheken die u nodig hebt voor toegang tot de Service Bus onderwerpen in deze map.

### <a name="import-the-module"></a>De module importeren

Voeg Kladblok of een andere teksteditor gebruikt, het volgende naar het begin van het bestand **server.js** van de toepassing:

```
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Een Service Bus-verbinding instellen

De Azure module leest de omgevingsvariabelen AZURE\_SERVICEBUS\_NAAMRUIMTE en AZURE\_SERVICEBUS\_ACCESS\_KEY voor informatie die nodig is om te verbinden met Service Bus. Als deze omgevingsvariabelen niet zijn ingesteld, moet u de accountgegevens bij het aanroepen van **createServiceBusService**is ingesteld.

Zie voor een voorbeeld van de omgevingsvariabelen in een configuratiebestand instellen voor een Azure-Cloudservice, [Node.js Cloudservice met opslagmedia][].

Zie voor een voorbeeld van het instellen van de omgevingsvariabelen in de [portal van Azure klassieke][] voor een Website Azure [Node.js webtoepassing met opslagmedia][].

## <a name="create-a-topic"></a>Een onderwerp maken

Het object **ServiceBusService** kunt u werken met onderwerpen. De volgende code maakt een object **ServiceBusService** . Toevoegen aan de bovenkant van het bestand **server.js** na de instructie om te importeren van de azure-module.

```
var serviceBusService = azure.createServiceBusService();
```

Door te bellen **createTopicIfNotExists** op het object **ServiceBusService** , wordt het opgegeven onderwerp geretourneerd (indien aanwezig) of een nieuw onderwerp met de opgegeven naam wordt gemaakt. De volgende code wordt **createTopicIfNotExists** maken of verbinding maken met het onderwerp met de naam 'MyTopic':

```
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

**createServiceBusService** ondersteunt ook extra opties waarmee u kunt de standaardinstellingen overschrijven onderwerp zoals bericht tijd naar live of onderwerp maximale grootte. Het volgende voorbeeld wordt de grootte van de maximale onderwerp ingesteld op 5GB met een tijd aan live van 1 minuut:

```
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filters

Optionele filterbewerkingen kunnen worden toegepast op bewerkingen die zijn uitgevoerd met **ServiceBusService**. Bewerkingen filteren kunt opnemen logboekregistratie, automatisch opnieuw proberen, enzovoort. Filters zijn objecten die implementeren van een methode bij de handtekening:

```
function handle (requestOptions, next)
```

Na het uitvoeren van voorverwerking op de opties van de aanvraag, de methode roept `next` doorgeven van een terugbellen met de volgende handtekening:

```
function (returnObject, finalCallback, next)
```

In dit terugbellen en na het verwerken van de **returnObject** (de reactie van de aanvraag op de server), wordt de behoeften terugbellen naar een volgende aanroepen als deze als u wilt doorgaan met het verwerken van andere filters of roepen **finalCallback** gewoon anders als u wilt de service-aanroep dit uiteindelijk bestaat.

Twee filters die opnieuw logica implementeren zijn opgenomen in de SDK Azure voor Node.js, **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende maakt een **ServiceBusService** -object dat de **ExponentialRetryPolicyFilter**gebruikt:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);

## <a name="create-subscriptions"></a>Abonnementen maken

Onderwerp abonnementen worden ook gemaakt met het object **ServiceBusService** . Abonnementen zijn met de naam en een optionele filter waarmee de verzameling berichten die in de virtuele wachtrij van het abonnement wordt beperkt kunnen hebben.

> [AZURE.NOTE] Abonnementen zijn permanente en blijft tot beide ze bestaan, of het onderwerp ze zijn gekoppeld, worden verwijderd. Als uw toepassing logica bevat voor het maken van een abonnement, moet deze eerst controleren of het abonnement dat al aanwezig is met de methode **getSubscription** .

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement maken met het standaardfilter (MatchAll)

Het filter **MatchAll** is het standaardfilter die wordt gebruikt als er geen filter is opgegeven als een nieuw abonnement wordt gemaakt. Wanneer het **MatchAll** -filter wordt gebruikt, worden alle berichten die zijn gepubliceerd naar het onderwerp van het abonnement virtuele wachtrij geplaatst. In het volgende voorbeeld wordt gemaakt van een abonnement met de naam 'AllMessages' en het standaardfilter voor **MatchAll** gebruikt.

```
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken

U kunt ook filters maken waarmee u naar een bereik die naar een onderwerp berichten moet worden weergegeven binnen een bepaald onderwerp-abonnement.

Het meest flexibele type filter die worden ondersteund in abonnementen is de **SqlFilter**, waarmee een subset van SQL92-standaard worden geïmplementeerd. SQL-filters worden uitgevoerd op de eigenschappen van de berichten die zijn gepubliceerd naar het onderwerp. Voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter, Controleer de [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntaxis.

Filters kunnen worden toegevoegd aan een abonnement met de methode **createRule** van het object **ServiceBusService** . Deze methode kunt u nieuwe filters toevoegen aan een bestaand abonnement.

> [AZURE.NOTE] Omdat het standaardfilter automatisch op alle nieuwe abonnementen toegepast wordt, moet u eerst het standaardfilter verwijderen of de **MatchAll** overschrijft eventuele andere filters die u kunt opgeven. U kunt de standaardregel verwijderen door met de methode **deleteRule** van het object **ServiceBusService** .

Het volgende voorbeeld wordt een abonnement met de naam `HighMessages` met een **SqlFilter** die selecteert alleen berichten met een aangepaste **messagenumber** eigenschap groter is dan 3:

```
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'HighMessages', 
            'HighMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Ook het volgende voorbeeld wordt een abonnement met de naam `LowMessages` met een **SqlFilter** die selecteert alleen berichten met een **messagenumber** eigenschap kleiner is dan of gelijk aan 3:

```
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'LowMessages', 
            'LowMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Wanneer een bericht nu wordt verzonden naar `MyTopic`, deze altijd worden bezorgd bij ontvangers geabonneerd op de `AllMessages` onderwerp-abonnement en selectief geleverde met ontvangers geabonneerd op de `HighMessages` en `LowMessages` onderwerp abonnementen (afhankelijk van de inhoud van het bericht).

## <a name="how-to-send-messages-to-a-topic"></a>Het verzenden van berichten naar een onderwerp

Als u wilt een bericht verzenden naar een onderwerp Service Bus, moet uw toepassing de methode **sendTopicMessage** van het object **ServiceBusService** gebruiken.
Berichten die zijn verzonden naar Service Bus onderwerpen zijn **BrokeredMessage** objecten.
**BrokeredMessage** objecten hebben een reeks standaardeigenschappen (zoals **Label** en **TimeToLive**), een woordenlijst die wordt gebruikt om specifieke eigenschappen maatwerktoepassing, en de hoofdtekst van de tekenreeksgegevens. Een toepassing kunt instellen in de hoofdtekst van het bericht een tekenreekswaarde doorgeven aan de **sendTopicMessage** en alle vereiste eigenschappen van de standaard wordt gevuld met standaardwaarden.

Het volgende voorbeeld ziet u hoe u verzendt vijf berichten naar 'MyTopic' testen. Opmerking die de waarde van de eigenschap **messagenumber** van elk bericht op de herhaling van de lus varieert (dit veld bepaalt welke abonnementen ontvangen):

```
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Service Bus onderwerpen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de standaardkleuren of aangepaste toepassingseigenschappen bevat, kan een maximale grootte van 64 KB hebben. Er is geen limiet voor het aantal berichten in een onderwerp gehouden, maar er is een initiaal van de totale grootte van de berichten die door een onderwerp. De grootte van dit onderwerp wordt opgegeven bij het maken, met een maximum van 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangt van een abonnement

Berichten worden ontvangen van een abonnement met de methode **receiveSubscriptionMessage** op het object **ServiceBusService** . Standaard worden berichten verwijderd uit het abonnement als ze worden opgelezen; u kunt echter (korte weergave) lezen en het bericht vergrendelen zonder deze te verwijderen van het abonnement door in te stellen van de optionele parameter **isPeekLock** op **true**.

Het standaardgedrag van lezen en verwijderen van het bericht als onderdeel van de ontvangstbewerking is de eenvoudigste model en werkt het best voor scenario's waarin een toepassing mogelijk zonder niet verwerken van een bericht in het geval van een fout. Als u wilt weten over dit, kunt u een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze wordt verwerkt. Omdat Service Bus wordt het bericht als wordt verbruikt hebt gemarkeerd, wordt klikt u vervolgens wanneer de toepassing opnieuw is opgestart en begint door nogmaals, andere berichten deze heb gemist het bericht verbruikte vóór het vastlopen.

Als de parameter **isPeekLock** is ingesteld op **waar**, wordt de ontvangen een bewerking twee fase, die het mogelijk maakt naar ondersteuningstoepassingen die niet kunnen zonder ontbrekende berichten. Als Service Bus een aanvraag ontvangt, deze vindt u het volgende bericht worden verbruikt, vergrendelen om te voorkomen dat andere consumenten ontvangen en vervolgens naar de toepassing wordt geretourneerd.
Nadat de toepassing verwerking van het bericht (of betrouwbaar voor toekomstige verwerking opgeslagen), is de tweede fase van het proces ontvangen door te bellen **deleteMessage** methode en het bericht moet worden verwijderd als een parameter voltooid. De methode **deleteMessage** wordt markeert u het bericht worden gebruikt en van het abonnement verwijderen.

Het volgende voorbeeld wordt gedemonstreerd hoe berichten kunnen worden ontvangen en verwerkt met behulp van **receiveSubscriptionMessage**. Het voorbeeld eerst ontvangt Hiermee verwijdert u een bericht van het abonnement 'LowMessages' en klikt u vervolgens een bericht ontvangt van het 'HighMessages'-abonnement met **isPeekLock** ingesteld op waar. Deze verwijdert het bericht met **deleteMessage**:

```
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Hoe u omgaat met toepassing loopt en gelezen berichten

Service Bus biedt een functie waarmee u fouten in uw toepassing of problemen verwerken van een bericht zonder problemen herstellen. Als een toepassing voor de ontvanger kan niet worden verwerkt van het bericht om een bepaalde reden is, kan deze de methode **unlockMessage** aanroepen op het object **ServiceBusService** . Hierdoor wordt de Service Bus u kunt het bericht binnen het abonnement te ontgrendelen en deze beschikbaar zijn voor het opnieuw worden ontvangen door de dezelfde in beslag nemen toepassing of door een andere in beslag nemen toepassing.

Er is ook een time-out die is gekoppeld aan een bericht vergrendeld binnen het abonnement en als de toepassing mislukt verwerkingstijd van het bericht voordat u de vergrendelen time-out is verstreken (bijvoorbeeld als de toepassing loopt vast), en vervolgens op de Service wordt het bericht automatisch ontgrendeld en voor het opnieuw worden ontvangen beschikbaar stelt.

In het geval dat de toepassing loopt vast na het verwerken van het bericht, maar voordat de methode **deleteMessage** wordt aangeroepen, wordt klikt u vervolgens het bericht worden geleverd met de toepassing opnieuw wordt gestart. Heet dit vaak **Tenminste eenmaal Processing**, dat wil zeggen elk bericht wordt ten minste eenmaal worden verwerkt maar in sommige gevallen hetzelfde bericht kan worden geleverd. Als u het scenario kan geen dubbele verwerking zonder, kunnen softwareontwikkelaars moeten extra logica toevoegen aan de toepassing u omgaat met dubbele bericht is bezorgd. Dit is vaak bereikt met de eigenschap **MessageId** van het bericht, constant over bezorging pogingen blijft.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen

Onderwerpen en abonnementen zijn permanente en expliciet moeten worden verwijderd via de [portal van Azure klassieke][] of via een programma.
Het volgende voorbeeld geeft aan hoe u het onderwerp met de naam `MyTopic`:

    serviceBusService.deleteTopic('MyTopic', function (error) {
        if (error) {
            console.log(error);
        }
    });

Een onderwerp worden ook verwijdert, alle abonnementen die zijn geregistreerd met het onderwerp. Abonnementen kunnen ook afzonderlijk worden verwijderd. Het volgende voorbeeld ziet u het verwijderen van een abonnement met de naam `HighMessages` uit de `MyTopic` onderwerp:

    serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
        if(error) {
            console.log(error);
        }
    });

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Service Bus onderwerpen hebt geleerd, volgt u deze koppelingen voor meer informatie.

-   Zie [wachtrijen, onderwerpen en abonnementen][].
-   API-Naslaggids voor [SqlFilter][].
-   Ga naar de bibliotheek [Azure SDK voor knooppunt][] op GitHub.

  [Azure SDK voor knooppunt]: https://github.com/Azure/azure-sdk-for-node
  [Azure klassieke portal]: https://manage.windowsazure.com
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Node.js Cloudservice]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Maak en implementeer een toepassing voor de Node.js naar een website van Azure]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloudservice met opslag]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Node.js webtoepassing met opslag]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
 
