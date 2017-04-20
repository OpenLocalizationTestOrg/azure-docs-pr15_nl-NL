<properties 
    pageTitle="Het gebruik van de Service Bus wachtrijen in Node.js | Microsoft Azure" 
    description="Informatie over het gebruik van de Service Bus wachtrijen in Azure wordt aangegeven via een Node.js-app." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Het gebruik van de Service Bus wachtrijen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In dit artikel wordt beschreven hoe Service Bus wachtrijen gebruiken in Node.js. In de voorbeelden in JavaScript zijn geschreven en gebruikt u de module Node.js Azure. De scenario's waarvoor zijn **maken van wachtrijen** **verzenden en ontvangen van berichten**en **wachtrijen verwijderen**. Zie voor meer informatie over wachtrijen, de sectie van de [volgende stappen](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-nodejs-application"></a>Een Node.js-servicetoepassing maken

Een lege Node.js-toepassing maken. Zie voor instructies over het maken van een toepassing voor de Node.js [maken en implementeren van een toepassing Node.js naar een Website Azure][], of [Node.js Cloudservice][] via Windows PowerShell.

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing Service Bus configureren

Als u wilt gebruiken Azure Service Bus, downloaden en gebruiken van het Node.js Azure-pakket. Dit pakket bevat een bibliotheken die met de Service Bus REST-services communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Gebruik knooppunt pakket Manager (NPM) het pakket verkrijgen

1. Het venster van de opdracht **Windows PowerShell voor Node.js** gebruiken om te navigeren naar de **c:\\knooppunt\\sbqueues\\WebRole1** map waarin u uw voorbeeldtoepassing hebt gemaakt.

2. Typ **npm azure installeren** in het opdrachtvenster, waardoor het moet uitvoer ongeveer als volgt uit:

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

3. U kunt handmatig uitvoeren met de opdracht **ls** om te controleren of die een **knooppunt\_modules** map is gemaakt. Zoek het **azure** -pakket waarin de bibliotheken die u nodig hebt voor toegang tot de Service Bus wachtrijen in deze map.

### <a name="import-the-module"></a>De module importeren

Voeg Kladblok of een andere teksteditor gebruikt, het volgende naar het begin van het bestand **server.js** van de toepassing:

```
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Een verbinding Azure Service Bus instellen

De Azure module leest de omgevingsvariabelen AZURE\_SERVICEBUS\_NAAMRUIMTE en AZURE\_SERVICEBUS\_ACCESS\_toets om informatie nodig is om te verbinden met Service Bus te verkrijgen. Als deze omgevingsvariabelen niet zijn ingesteld, moet u de accountgegevens bij het aanroepen van **createServiceBusService**is ingesteld.

Zie voor een voorbeeld van de omgevingsvariabelen in een configuratiebestand instellen voor een Azure-Cloudservice, [Node.js Cloudservice met opslagmedia][].

Zie voor een voorbeeld van het instellen van de omgevingsvariabelen in de [portal van Azure klassieke][] voor een Website Azure [Node.js webtoepassing met opslagmedia][].

## <a name="create-a-queue"></a>Een wachtrij maken

Het object **ServiceBusService** kunt u werken met Service Bus wachtrijen. De volgende code maakt een object **ServiceBusService** . Toevoegen aan de bovenkant van het bestand **server.js** na de instructie om te importeren van de Azure-module.

```
var serviceBusService = azure.createServiceBusService();
```

Door te bellen **createQueueIfNotExists** op het object **ServiceBusService** , de opgegeven wachtrij als resultaat gegeven (indien aanwezig) of een nieuwe wachtrij met de opgegeven naam wordt gemaakt. De volgende code **createQueueIfNotExists** gebruikt om te maken of verbinding maken met de wachtrij met de naam `myqueue`:

```
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

**createServiceBusService** ondersteunt ook extra opties waarmee u kunt de standaardinstellingen overschrijven wachtrij zoals bericht tijd naar live- of maximumlimiet wachtrijgrootte. Het volgende voorbeeld wordt de grootte van de maximale wachtrij aan 5 GB aan en per keer wilt live (TTL)-waarde van 1 minuut:

```
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a>Filters

Optionele filterbewerkingen kunnen worden toegepast op bewerkingen die zijn uitgevoerd met **ServiceBusService**. Bewerkingen filteren kunt opnemen logboekregistratie, automatisch opnieuw proberen, enzovoort. Filters zijn objecten die implementeren van een methode bij de handtekening:

```
function handle (requestOptions, next)
```

Na de oude verwerking op de opties voor de aanvraag, de methode moet bellen `next`, doorgeven van een terugbellen met de volgende handtekening:

```
function (returnObject, finalCallback, next)
```

In dit terugbellen en na het verwerken van de **returnObject** (de reactie van de aanvraag op de server), het terugbellen ofwel moet aanroepen `next` indien aanwezig als u wilt doorgaan met het verwerken van andere filters, of gewoon roepen `finalCallback`, waarop de service-aanroep eindigt.

Twee filters die opnieuw logica implementeren zijn opgenomen in de SDK Azure voor Node.js, **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende maakt een **ServiceBusService** -object dat de **ExponentialRetryPolicyFilter**gebruikt:

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a>Berichten verzenden naar een wachtrij

Als u wilt een bericht verzenden naar een Service Bus wachtrij, uw toepassing aanroept de **sendQueueMessage** op het object **ServiceBusService** . Berichten die zijn verzonden naar (en ontvangen van) Service Bus wachtrijen zijn **BrokeredMessage** objecten en hebt een reeks standaardeigenschappen (zoals **Label** en **TimeToLive**), een woordenlijst die wordt gebruikt om aangepaste toepassingsspecifieke eigenschappen en een willekeurige toepassing gegevens. Een toepassing kan de hoofdtekst van het bericht door het doorgeven van een tekenreeks als het bericht ingesteld. Alle vereiste standaard eigenschappen worden gevuld met standaardwaarden.

Het volgende voorbeeld wordt getoond hoe een testbericht verzenden naar de wachtrij met de naam `myqueue` **sendQueueMessage**gebruiken:

```
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Service Bus wachtrijen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de standaardkleuren of aangepaste toepassingseigenschappen bevat, kan een maximale grootte van 64 KB hebben. Er is geen limiet voor het aantal berichten in een wachtrij gezet, maar er is een initiaal van de totale grootte van de berichten die door een wachtrij gehouden. De wachtrijgrootte van deze is opgegeven bij het maken, met een maximum van 5 GB. Zie [quota voor Service Bus][]voor meer informatie over quota.

## <a name="receive-messages-from-a-queue"></a>Berichten ontvangt van een wachtrij

Berichten worden ontvangen uit een wachtrij met de methode **receiveQueueMessage** op het object **ServiceBusService** . Standaard worden berichten verwijderd uit de wachtrij als ze worden opgelezen; u kunt echter (korte weergave) lezen en het bericht vergrendelen zonder deze te verwijderen uit de wachtrij door in te stellen van de optionele parameter **isPeekLock** op **true**.

Het standaardgedrag van lezen en verwijderen van het bericht als onderdeel van de ontvangstbewerking is de eenvoudigste model en werkt het best voor scenario's waarin een toepassing mogelijk zonder niet verwerken van een bericht in het geval van een fout. Als u wilt weten over dit, kunt u een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze wordt verwerkt. Omdat Service Bus wordt het bericht als wordt verbruikt hebt gemarkeerd, wordt klikt u vervolgens wanneer de toepassing opnieuw is opgestart en begint door nogmaals, andere berichten deze heb gemist het bericht verbruikte vóór het vastlopen.

Als de parameter **isPeekLock** is ingesteld op **waar**, wordt de ontvangen een bewerking van twee fase, die het mogelijk maakt naar ondersteuningstoepassingen die u kunnen geen ontbrekende berichten zonder. Als Service Bus een aanvraag ontvangt, deze vindt u het volgende bericht worden verbruikt, vergrendelen om te voorkomen dat andere consumenten ontvangen en vervolgens naar de toepassing wordt geretourneerd. Nadat de toepassing verwerking van het bericht (of betrouwbaar voor toekomstige verwerking opgeslagen), is de tweede fase van het proces ontvangen door te bellen **deleteMessage** methode en het bericht moet worden verwijderd als een parameter voltooid. De methode **deleteMessage** wordt markeert u het bericht worden gebruikt en deze te verwijderen uit de wachtrij.

Het volgende voorbeeld wordt ontvangen en berichten met **receiveQueueMessage**verwerken. Het voorbeeld eerst ontvangt en Hiermee verwijdert u een bericht, en laat ontvangt van een bericht met **isPeekLock** ingesteld op **waar**en vervolgens het bericht met **deleteMessage**verwijderd:

```
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Hoe u omgaat met toepassing loopt en gelezen berichten

Service Bus biedt een functie waarmee u fouten in uw toepassing of problemen verwerken van een bericht zonder problemen herstellen. Als een toepassing voor de ontvanger kan niet worden verwerkt van het bericht om een bepaalde reden is, kan deze de methode **unlockMessage** aanroepen op het object **ServiceBusService** . Hierdoor wordt de Service Bus u kunt het bericht in de wachtrij ontgrendelen en deze beschikbaar zijn voor het opnieuw worden ontvangen door de dezelfde in beslag nemen toepassing of door een andere in beslag nemen toepassing.

Er is ook een time-out die is gekoppeld aan een bericht in de wachtrij vergrendeld en als de toepassing mislukt verwerkingstijd van het bericht voordat u de vergrendelen time-out is verstreken (bijvoorbeeld als de toepassing loopt vast), en vervolgens Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen.

In het geval dat de toepassing loopt vast na het verwerken van het bericht, maar voordat de methode **deleteMessage** wordt aangeroepen, wordt klikt u vervolgens het bericht worden geleverd met de toepassing opnieuw wordt gestart. Heet dit vaak **Tenminste eenmaal Processing**, dat wil zeggen elk bericht wordt ten minste eenmaal worden verwerkt maar in sommige gevallen hetzelfde bericht kan worden geleverd. Als u het scenario kan geen dubbele verwerking zonder, kunnen softwareontwikkelaars moeten extra logica toevoegen aan de toepassing u omgaat met dubbele bericht is bezorgd. Dit is vaak bereikt met de eigenschap **MessageId** van het bericht, constant over bezorging pogingen blijft.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over wachtrijen.

-   [Wachtrijen, onderwerpen en abonnementen][]
-   [Azure SDK voor knooppunt][] bibliotheek op GitHub
-   [Node.js Developer Center](/develop/nodejs/)

  [Azure SDK voor knooppunt]: https://github.com/Azure/azure-sdk-for-node
  [Azure klassieke portal]: http://manage.windowsazure.com
  
  [Node.js Cloudservice]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
  [Maak en implementeer een toepassing Node.js naar een Azure-Website]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloudservice met opslag]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js webtoepassing met opslag]: ../storage/storage-nodejs-how-to-use-table-storage.md
  [Quota voor Service Bus]: service-bus-quotas.md
 
