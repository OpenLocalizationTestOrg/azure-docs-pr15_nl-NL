## <a name="send-messages-to-event-hubs"></a>Berichten verzenden naar gebeurtenis Hubs

De bibliotheek Java-client voor gebeurtenis Hubs is beschikbaar voor gebruik in Maven projecten uit het [Centrale opslagplaats Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)en het gebruik van de volgende afhankelijkheid declaratie in het projectbestand Maven kan worden verwezen:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Voor verschillende soorten opbouwen omgevingen, kunt u de meest recente oppervlak bestanden expliciet uit het [Centrale opslagplaats Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) of vanaf [het moment release verdeling GitHub](https://github.com/Azure/azure-event-hubs/releases)aanvragen.  

Importeer het pakket *com.microsoft.azure.eventhubs* voor de gebeurtenis Hubs client klassen en de *com.microsoft.azure.servicebus* voor hulpprogramma klassen zoals algemene uitzonderingen die worden gedeeld met de Azure Service Bus SMS-client voor een uitgever eenvoudige gebeurtenis. 

Voor het onderstaande voorbeeld, moet u eerst een nieuw project Maven voor een console/shell-toepassing maken in uw favoriete Java-ontwikkelomgeving. De klas wordt aangeroepen ```Send```.     

``` Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Vervang de naamruimte en de namen van de Hub van de gebeurtenis met de waarden die worden gebruikt wanneer u de gebeurtenis-Hub hebt gemaakt.

``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Kies vervolgens een enkelvoud gebeurtenis maken door het omzetten van een tekenreeks in de UTF-8-bytecodering. Er vervolgens een nieuw exemplaar van de gebeurtenis Hubs-client maken uit de verbindingsreeks en verzend het bericht.   

``` Java 
                
    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);
    
    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 
