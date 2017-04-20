<properties 
    pageTitle="Het gebruik van de Service Bus wachtrijen met PHP | Microsoft Azure" 
    description="Informatie over het gebruik van de Service Bus wachtrijen in Azure wordt aangegeven. Voorbeelden van de code is geschreven in PHP." 
    services="service-bus" 
    documentationCenter="php" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Het gebruik van de Service Bus wachtrijen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Deze handleiding ziet u hoe u met de Service Bus wachtrijen. In de voorbeelden in PHP zijn geschreven en gebruikt u de [SDK van Azure voor PHP](../php-download-sdk.md). De scenario's waarvoor zijn **maken van wachtrijen** **verzenden en ontvangen van berichten**en **wachtrijen verwijderen**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>Een PHP-toepassing maken

De enige vereiste voor het maken van een PHP-toepassing die toegang heeft tot de service Azure Blob is de verwijzing naar klassen in de [SDK van Azure voor PHP](../php-download-sdk.md) uit uw code. U kunt eventuele ontwikkeling hulpmiddelen voor uw toepassing of Kladblok maken.

> [AZURE.NOTE] Uw installatie PHP moet ook de [OpenSSL extensie](http://php.net/openssl) geïnstalleerd en ingeschakeld.

In deze handleiding gebruikt u de servicefuncties van de die kunnen worden aangeroepen vanuit binnen een PHP-toepassing lokaal of in code die wordt uitgevoerd vanuit een Azure Webrol, werknemer rol of website.

## <a name="get-the-azure-client-libraries"></a>Bibliotheken van de Azure-client krijgen

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing Service Bus configureren

Ga als volgt te werk als u wilt gebruiken in de wachtrij Bus Service API's:

1. Een verwijzing naar de autoloader-bestand met de [require_once] [ require_once] instructie.
2. Overzicht van alle klassen die u kunt gebruiken.

Het volgende voorbeeld ziet u hoe u het bestand autoloader nemen en om te verwijzen naar de klasse **ServicesBuilder** .

> [AZURE.NOTE] In dit voorbeeld (en andere voorbeelden in dit artikel) wordt ervan uitgegaan dat u de bibliotheken van de Client PHP voor Azure via Composer hebt geïnstalleerd. Als u de bibliotheken handmatig of als een pakket PERENBOMEN hebt geïnstalleerd, moet u verwijzen naar het **WindowsAzure.php** autoloader-bestand.

```
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

In de onderstaande voorbeelden de `require_once` instructie altijd worden weergegeven, maar alleen de klassen die nodig zijn voor het voorbeeld uitvoeren wordt verwezen.

## <a name="set-up-a-service-bus-connection"></a>Een Service Bus-verbinding instellen

Als u wilt een Service Bus-client wordt gestart, moet u eerst een geldige verbindingsreeks in deze indeling hebben:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Waar **eindpunt** is meestal van de indeling `[yourNamespace].servicebus.windows.net`.

U moet de klas **ServicesBuilder** gebruiken om te maken van een serviceclient Azure. U kunt:

* Geeft de verbindingsreeks rechtstreeks aan deze.
* Gebruik de **CloudConfigurationManager (CCM)** om te controleren van meerdere externe bronnen voor de verbindingsreeks:
    * Standaard wordt geleverd met ondersteuning voor één externe bron - milieu variabelen
    * U kunt nieuwe bronnen toevoegen door de klasse **ConnectionStringSource** uitbreiden

Voor de voorbeelden hier beschreven, wordt de verbindingsreeks rechtstreeks worden doorgegeven.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="how-to-create-a-queue"></a>Hoe u: een wachtrij maken

U kunt management bewerkingen voor Service Bus wachtrijen via de klasse **ServiceBusRestProxy** uitvoeren. Een object **ServiceBusRestProxy** wordt opgesteld via de methode **ServicesBuilder::createServiceBusService** factory met een geschikte verbindingsreeks die de token machtigingen voor het beheren van dit omvat.

Het volgende voorbeeld ziet u hoe u een **ServiceBusRestProxy** wordt gestart en bellen **ServiceBusRestProxy -> createQueue** -Maak een wachtrij met de naam `myqueue` binnen een `MySBNamespace` Servicenaamruimte:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    $queueInfo = new QueueInfo("myqueue");
        
    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [AZURE.NOTE] U kunt de `listQueues` methode op `ServiceBusRestProxy` objecten om te controleren of een wachtrij met een opgegeven naam al in een naamruimte bestaat.

## <a name="how-to-send-messages-to-a-queue"></a>Hoe u: berichten verzenden naar een wachtrij

Als u wilt een bericht verzenden naar een Service Bus wachtrij, belt uw toepassing de methode **ServiceBusRestProxy -> sendQueueMessage** . De volgende code ziet u hoe u een bericht verzenden naar de `myqueue` wachtrij eerder hebt gemaakt in de `MySBNamespace` Servicenaamruimte.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");
    
    // Send message.
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Berichten die zijn verzonden naar (en ontvangen van) Service Bus wachtrijen zijn exemplaren van de klasse **BrokeredMessage** . **BrokeredMessage** objecten bevatten een set standaard methoden (zoals **getLabel**, **getTimeToLive**, **setLabel**en **setTimeToLive**) en eigenschappen die worden gebruikt in wachtstand toepassingsspecifieke gegevensvelden en een hoofdtekst van willekeurige toepassingsgegevens.

Service Bus wachtrijen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de standaardkleuren of aangepaste toepassingseigenschappen bevat, kan een maximale grootte van 64 KB hebben. Er is geen limiet voor het aantal berichten in een wachtrij gezet, maar er is een initiaal van de totale grootte van de berichten die door een wachtrij gehouden. Deze bovengrens van de wachtrijgrootte van de is 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Hoe u berichten ontvangt van een wachtrij

De beste manier om berichten ontvangt van een wachtrij is met een methode **ServiceBusRestProxy -> receiveQueueMessage** . Berichten kunnen worden ontvangen in twee verschillende modi: **ReceiveAndDelete** (de standaardinstelling) en **PeekLock**.

Wanneer de **ReceiveAndDelete** -modus met ontvangt is een bewerking foto - dat wil zeggen wanneer Service Bus een leesbevestiging voor een bericht in een wachtrij ontvangt, wordt Hiermee markeert u het bericht als wordt verbruikt en naar de toepassing wordt geretourneerd. **ReceiveAndDelete** -modus is de eenvoudigste model en werkt het best voor scenario's waarin een toepassing mogelijk zonder niet verwerken van een bericht in het geval van een fout. Als u wilt weten over dit, kunt u een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze wordt verwerkt. Omdat Service Bus wordt het bericht als wordt verbruikt hebt gemarkeerd, wordt klikt u vervolgens wanneer de toepassing opnieuw is opgestart en begint door nogmaals, andere berichten deze heb gemist het bericht verbruikte vóór het vastlopen.

In de modus **PeekLock** verandert in een bewerking twee fase, die het mogelijk maakt naar ondersteuningstoepassingen die niet kunnen zonder ontbrekende berichten ontvangen van een bericht. Als Service Bus een aanvraag ontvangt, deze vindt u het volgende bericht worden verbruikt, vergrendelen om te voorkomen dat andere consumenten ontvangen deze en klikt u vervolgens naar de toepassing wordt geretourneerd. Nadat de toepassing verwerking van het bericht (of betrouwbaar voor toekomstige verwerking opgeslagen), is de tweede fase van het proces ontvangen door de ontvangen bericht doorgeven aan het **ServiceBusRestProxy -> deleteMessage**voltooid. Wanneer Service Bus de **deleteMessage** bellen, wordt deze markeert u het bericht worden gebruikt en deze te verwijderen uit de wachtrij.

Het volgende voorbeeld ziet u hoe een bericht kan worden ontvangen en verwerkt met behulp van **PeekLock** -modus (niet de standaardmodus).

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
        
    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/windowsazure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Hoe u: toepassing loopt en gelezen berichten verwerken

Service Bus biedt een functie waarmee u fouten in uw toepassing of problemen verwerken van een bericht zonder problemen herstellen. Als een toepassing voor de ontvanger kan niet worden verwerkt van het bericht om een bepaalde reden is, kan deze de methode **unlockMessage** aanroepen voor het ontvangen bericht (in plaats van de methode **deleteMessage** ). Hierdoor wordt de Service Bus u kunt het bericht in de wachtrij ontgrendelen en deze beschikbaar zijn voor het opnieuw worden ontvangen door de dezelfde in beslag nemen toepassing of door een andere in beslag nemen toepassing.

Er is ook een time-out die is gekoppeld aan een bericht in de wachtrij vergrendeld en als de toepassing mislukt verwerkingstijd van het bericht voordat u de vergrendelen time-out is verstreken (bijvoorbeeld als de toepassing loopt vast), en vervolgens Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen.

In het geval dat de toepassing loopt vast na het verwerken van het bericht, maar voordat het verzoek **deleteMessage** is uitgegeven, wordt klikt u vervolgens het bericht worden geleverd met de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **Tenminste één keer Processing**; dat wil zeggen elk bericht ten minste eenmaal worden verwerkt, maar in sommige gevallen hetzelfde bericht kan worden geleverd. Als u het scenario kan geen dubbele verwerking zonder, wordt klikt u vervolgens extra logica toevoegen aan toepassingen u omgaat met dubbele berichtbezorging aanbevolen. Dit is vaak bereikt met de methode **getMessageId** van het bericht ongewijzigd over bezorging pogingen.

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Service Bus wachtrijen hebt geleerd, raadpleegt u [wachtrijen, onderwerpen en abonnementen][] voor meer informatie.

Zie ook [PHP Developer Center](/develop/php/)voor meer informatie.

[Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once

 
