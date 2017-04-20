<properties 
    pageTitle="Het gebruik van de Service Bus onderwerpen met PHP | Microsoft Azure" 
    description="Informatie over het gebruiken van Service Bus onderwerpen met PHP in Azure wordt aangegeven." 
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
    ms.date="10/14/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Het gebruik van de Service Bus onderwerpen en abonnementen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In dit artikel leest u het gebruik van de Service Bus onderwerpen en abonnementen. In de voorbeelden in PHP zijn geschreven en gebruikt u de [SDK van Azure voor PHP](../php-download-sdk.md). De scenario's waarvoor zijn **maken van onderwerpen en abonnementen** **abonnement filters maken**, **berichten verzenden naar een onderwerp**, **ontvangen van berichten van een abonnement**en **verwijderen van onderwerpen en abonnementen**.

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>Een PHP-toepassing maken

De enige vereiste voor het maken van een PHP-toepassing die toegang heeft tot de service Azure Blob is verwijzen naar klassen in de [SDK van Azure voor PHP](../php-download-sdk.md) uit uw code. U kunt eventuele ontwikkeling hulpmiddelen voor uw toepassing of Kladblok maken.

> [AZURE.NOTE] Uw installatie PHP moet ook de [OpenSSL extensie](http://php.net/openssl) geïnstalleerd en ingeschakeld.

In dit artikel wordt beschreven hoe servicefuncties gebruiken die kunnen worden aangeroepen vanuit een PHP-toepassing lokaal of in code die wordt uitgevoerd vanuit een Azure Webrol, werknemer rol of website.

## <a name="get-the-azure-client-libraries"></a>Bibliotheken van de Azure-client krijgen

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing Service Bus configureren

Gebruik van de Service Bus API's:

1. Een verwijzing naar de autoloader-bestand met de [require_once] [ require-once] instructie.
2. Overzicht van alle klassen die u kunt gebruiken.

Het volgende voorbeeld ziet u hoe u het bestand autoloader nemen en om te verwijzen naar de klasse **ServiceBusService** .

> [AZURE.NOTE] In dit voorbeeld (en andere voorbeelden in dit artikel) wordt ervan uitgegaan dat u de bibliotheken van de Client PHP voor Azure via Composer hebt geïnstalleerd. Als u de bibliotheken handmatig of als een pakket PERENBOMEN hebt geïnstalleerd, moet u verwijzen naar het **WindowsAzure.php** autoloader-bestand.

```
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

In de volgende voorbeelden wordt de `require_once` instructie altijd worden weergegeven, maar alleen de klassen die nodig zijn voor het voorbeeld uitvoeren wordt verwezen.

## <a name="set-up-a-service-bus-connection"></a>Een Service Bus-verbinding instellen

Als u wilt maken van een Service Bus-client moet u eerst een geldige verbindingsreeks in deze indeling hebben:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Waar `Endpoint` is meestal van de indeling `https://[yourNamespace].servicebus.windows.net`.

U moet de klas **ServicesBuilder** gebruiken om te maken van een serviceclient Azure. U kunt:

* Geeft de verbindingsreeks rechtstreeks aan deze.
* Gebruik de **CloudConfigurationManager (CCM)** om te controleren van meerdere externe bronnen voor de verbindingsreeks:
    * Standaard wordt deze geleverd met ondersteuning voor één externe bron - milieu variabelen.
    * U kunt nieuwe bronnen toevoegen door de klasse **ConnectionStringSource** uitbreiden.

Voor de voorbeelden hier beschreven, wordt de verbindingsreeks rechtstreeks doorgegeven.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
    
$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Een onderwerp maken

U kunt management bewerkingen voor Service Bus onderwerpen via de klasse **ServiceBusRestProxy** uitvoeren. Een object **ServiceBusRestProxy** wordt opgesteld via de methode **ServicesBuilder::createServiceBusService** factory met een geschikte verbindingsreeks die de token machtigingen voor het beheren van dit omvat.

Het volgende voorbeeld ziet u hoe u een **ServiceBusRestProxy** wordt gestart en bellen **ServiceBusRestProxy -> createTopic** -als u wilt maken van een onderwerp met de naam `mytopic` binnen een `MySBNamespace` naamruimte:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;
    
// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
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

> [AZURE.NOTE] U kunt de `listTopics` methode op `ServiceBusRestProxy` objecten om te controleren of een onderwerp met een opgegeven naam al in een Servicenaamruimte bestaat.

## <a name="create-a-subscription"></a>Een abonnement maken

Onderwerp abonnementen worden ook gemaakt met de methode **ServiceBusRestProxy -> createSubscription** . Abonnementen zijn met de naam en een optionele filter waarmee de verzameling berichten doorgegeven aan de virtuele wachtrij van het abonnement wordt beperkt kunnen hebben.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement maken met het standaardfilter (MatchAll)

Het filter **MatchAll** is het standaardfilter die wordt gebruikt als er geen filter is opgegeven als een nieuw abonnement wordt gemaakt. Wanneer het **MatchAll** -filter wordt gebruikt, worden alle berichten die zijn gepubliceerd naar het onderwerp van het abonnement virtuele wachtrij geplaatst. In het volgende voorbeeld wordt gemaakt van een abonnement met de naam 'mysubscription' en het standaardfilter voor **MatchAll** gebruikt.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken

U kunt ook filters waarmee u kunt opgeven welke berichten u verzonden naar een onderwerp moeten instellen worden weergegeven binnen een bepaald onderwerp-abonnement. Het meest flexibele type filter die worden ondersteund in abonnementen is de **SqlFilter**, waarmee een subset van SQL92-standaard worden geïmplementeerd. SQL-filters worden uitgevoerd op de eigenschappen van de berichten die zijn gepubliceerd naar het onderwerp. Zie voor meer informatie over SqlFilters, [SqlFilter.SqlExpression eigenschap][sqlfilter].

> [AZURE.NOTE] Elke regel op een abonnement verwerkt binnenkomende berichten belooft hun Resultaatberichten toe te voegen aan het abonnement. Elke nieuwe abonnement heeft daarnaast een standaard- **regel** -object met een filter dat alle berichten van het onderwerp naar het abonnement worden opgeteld. Als u wilt alleen berichten die overeenkomen met uw filter ontvangt, moet u de standaardregel verwijderen. U kunt de standaardregel verwijderen met behulp van de `ServiceBusRestProxy->deleteRule` methode.

Het volgende voorbeeld wordt een abonnement met de naam **HighMessages** met een **SqlFilter** die selecteert alleen berichten met een aangepaste **MessageNumber** eigenschap die groter is dan 3 (Zie [berichten verzenden naar een onderwerp](#send-messages-to-a-topic) voor informatie over aangepaste eigenschappen toevoegen aan berichten):

```
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Opmerking dat deze code is vereist voor het gebruik van een extra naamruimte: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Het volgende voorbeeld wordt ook een abonnement met de naam **LowMessages** met een **SqlFilter** die selecteert alleen berichten met een **MessageNumber** eigenschap kleiner is dan of gelijk aan 3:

```
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Nu, wanneer een bericht wordt verzonden naar de `mytopic` onderwerp wordt altijd bezorgd bij ontvangers geabonneerd op de `mysubscription` -abonnement en selectief geleverde met ontvangers geabonneerd op de `HighMessages` en `LowMessages` abonnementen (afhankelijk van de inhoud van het bericht).

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp

Als u wilt een bericht verzenden naar een onderwerp Service Bus, belt uw toepassing de methode **ServiceBusRestProxy -> sendTopicMessage** . De volgende code ziet u hoe u een bericht verzenden naar de `mytopic` onderwerp eerder hebt gemaakt in de `MySBNamespace` Servicenaamruimte.

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
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Berichten die zijn verzonden naar Service Bus onderwerpen zijn exemplaren van de klasse **BrokeredMessage** . **BrokeredMessage** objecten hebben een set standaardeigenschappen en methoden (zoals **getLabel**, **getTimeToLive** **setLabel**en **setTimeToLive**), evenals eigenschappen die kunnen worden gebruikt om de aangepaste eigenschappen van toepassingsspecifieke houdt. Het volgende voorbeeld wordt getoond hoe 5 testberichten verzenden naar de `mytopic` onderwerp eerder hebt gemaakt. De methode **EigenschapInstellen** wordt gebruikt om toe te voegen een aangepaste eigenschap (`MessageNumber`) aan elk bericht. Houd er rekening mee dat het `MessageNumber` waarde onroerend goed varieert voor elk bericht (u kunt deze waarde om te bepalen welke abonnementen ontvangen, zoals wordt weergegeven in de sectie [maken een abonnement](#create-a-subscription) ):

```
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);
            
    // Set custom property.
    $message->setProperty("MessageNumber", $i);
            
    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Service Bus onderwerpen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de standaardkleuren of aangepaste toepassingseigenschappen bevat, kan een maximale grootte van 64 KB hebben. Er is geen limiet voor het aantal berichten in een onderwerp gehouden, maar er is een initiaal van de totale grootte van de berichten die door een onderwerp. Deze bovengrens van de grootte van het onderwerp is 5 GB. Zie [quota voor Service Bus][]voor meer informatie over quota.

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangt van een abonnement

De beste manier om berichten ontvangt van een abonnement is met een methode **ServiceBusRestProxy -> receiveSubscriptionMessage** . Ontvangen berichten kunnen werken in twee verschillende modi: **ReceiveAndDelete** (de standaardinstelling) en **PeekLock**.

Wanneer de **ReceiveAndDelete** -modus met ontvangt is een foto-bewerking. dat wil zeggen Service Bus ontvangt een leesbevestiging voor een bericht in een abonnement, deze markeert u het bericht worden gebruikt als naar de toepassing wordt geretourneerd. **ReceiveAndDelete** -modus is de eenvoudigste model en werkt het best voor scenario's waarin een toepassing mogelijk zonder niet verwerken van een bericht in het geval van een fout. Als u wilt weten over dit, kunt u een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze wordt verwerkt. Omdat Service Bus wordt het bericht als wordt verbruikt hebt gemarkeerd, wordt klikt u vervolgens wanneer de toepassing opnieuw is opgestart en begint door nogmaals, andere berichten deze heb gemist het bericht verbruikte vóór het vastlopen.

In de modus **PeekLock** verandert in een bewerking twee fase, die het mogelijk maakt naar ondersteuningstoepassingen die niet kunnen zonder ontbrekende berichten ontvangen van een bericht. Als Service Bus een aanvraag ontvangt, deze vindt u het volgende bericht worden verbruikt, vergrendelen om te voorkomen dat andere consumenten ontvangen en vervolgens naar de toepassing wordt geretourneerd. Nadat de toepassing verwerking van het bericht (of betrouwbaar voor toekomstige verwerking opgeslagen), is de tweede fase van het proces ontvangen door de ontvangen bericht doorgeven aan het **ServiceBusRestProxy -> deleteMessage**voltooid. Wanneer Service Bus de **deleteMessage** bellen, wordt deze markeert u het bericht worden gebruikt en deze te verwijderen uit de wachtrij.

Het volgende voorbeeld wordt getoond hoe ontvangen en verwerken van een bericht met **PeekLock** -modus (niet de standaardmodus). 

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
    
    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Hoe u: toepassing loopt en gelezen berichten verwerken

Service Bus biedt een functie waarmee u fouten in uw toepassing of problemen verwerken van een bericht zonder problemen herstellen. Als een toepassing voor de ontvanger kan niet worden verwerkt van het bericht om een bepaalde reden is, kan deze de methode **unlockMessage** aanroepen voor het ontvangen bericht (in plaats van de methode **deleteMessage** ). Hierdoor wordt de Service Bus u kunt het bericht in de wachtrij ontgrendelen en deze beschikbaar zijn voor het opnieuw worden ontvangen door de dezelfde in beslag nemen toepassing of door een andere in beslag nemen toepassing.

Er is ook een time-out die is gekoppeld aan een bericht in de wachtrij vergrendeld en als de toepassing mislukt verwerkingstijd van het bericht voordat u de vergrendelen time-out is verstreken (bijvoorbeeld als de toepassing loopt vast), en vervolgens Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen.

In het geval dat de toepassing loopt vast na het verwerken van het bericht, maar voordat het verzoek **deleteMessage** is uitgegeven, wordt klikt u vervolgens het bericht worden geleverd met de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **Tenminste één keer Processing**; dat wil zeggen elk bericht ten minste eenmaal worden verwerkt, maar in sommige gevallen hetzelfde bericht kan worden geleverd. Als u het scenario kan geen dubbele verwerking zonder, kunnen softwareontwikkelaars moeten extra logica toevoegen aan toepassingen u omgaat met dubbele bericht is bezorgd. Dit is vaak bereikt met de methode **getMessageId** van het bericht ongewijzigd over bezorging pogingen.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen

Als u wilt een onderwerp of een abonnement verwijdert, gebruikt u de- **ServiceBusRestProxy -> deleteTopic** of de methoden **ServiceBusRestProxy -> deleteSubscripton** respectievelijk. Houd er rekening mee dat alle abonnementen die zijn geregistreerd met het onderwerp een onderwerp verwijderen ook verwijderd.

Het volgende voorbeeld ziet u het verwijderen van een onderwerp met de naam `mytopic` en de geregistreerde abonnementen.

```
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Met de methode **deleteSubscription** , kunt u een abonnement onafhankelijk verwijderen:

```
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Service Bus wachtrijen hebt geleerd, raadpleegt u [wachtrijen, onderwerpen en abonnementen][] voor meer informatie.

[Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[require-once]: http://php.net/require_once
[Quota voor Service Bus]: service-bus-quotas.md
