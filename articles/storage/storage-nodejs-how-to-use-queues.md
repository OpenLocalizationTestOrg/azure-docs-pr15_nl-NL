<properties
    pageTitle="Het gebruik van de wachtrij opslag van Node.js | Microsoft Azure"
    description="Meer informatie over het gebruik van de wachtrij Azure-service maken en wachtrijen, verwijderen en invoegen, krijgen en berichten verwijderen. Voorbeelden in Node.js geschreven."
    services="storage"
    documentationCenter="nodejs"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-nodejs"></a>Het gebruik van de wachtrij opslag van Node.js

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

Deze handleiding ziet u hoe u veelvoorkomende scenario's met de service Microsoft Azure wachtrij uitvoert. In de voorbeelden worden geschreven met de API Node.js. De scenario's waarvoor opnemen **Invoegen**, **inspecteren**, **ophalen**en **verwijderen van** berichten in wachtrij plaatsen, evenals **maken en verwijderen van wachtrijen**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Een Node.js-servicetoepassing maken

Een lege Node.js-toepassing maken. Zie instructies voor het maken van een toepassing Node.js [maken een Node.js web-app in Azure App-Service], [bouwen en implementeren van een toepassing Node.js naar een Cloudservice Azure] werkt met Windows PowerShell, of [bouwen en implementeren van een Node.js web-app naar Azure met behulp van Web-Matrix].

## <a name="configure-your-application-to-access-storage"></a>Uw toepassing toegang tot opslag configureren

Als u wilt gebruiken Azure opslag, moet u de SDK van Azure opslag voor Node.js, waaronder een reeks gemak bibliotheken die met de opslagruimte REST-services communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Gebruik knooppunt pakket Manager (NPM) het pakket verkrijgen

1.  Een opdrachtregel-interface gebruiken zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix), navigeer naar de map waar u uw voorbeeldtoepassing hebt gemaakt.

2.  Typ **npm Installeer azure-opslag** in het opdrachtvenster. Uitvoer van de opdracht is vergelijkbaar met het volgende voorbeeld.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  U kunt handmatig uitvoeren met de opdracht **ls** om te controleren of die een **knooppunt\_modules** map is gemaakt. In deze map vindt u het pakket **azure-opslag** , waarin de bibliotheken die u nodig hebt voor toegang tot opslag.

### <a name="import-the-package"></a>Het pakket importeren

Voeg Kladblok of een andere teksteditor gebruikt, het volgende naar het begin van het bestand **server.js** van de toepassing waarin u van plan bent opslag gebruiken:

    var azure = require('azure-storage');

## <a name="setup-an-azure-storage-connection"></a>Een verbinding Azure opslag instellen

De azure module leest de omgevingsvariabelen AZURE\_opslag\_ACCOUNT en AZURE\_opslag\_ACCESS\_toets of AZURE\_opslag\_verbinding\_tekenreeks voor informatie die nodig is om te koppelen aan een account Azure opslag. Als deze omgevingsvariabelen niet zijn ingesteld, moet u de accountgegevens bij het aanroepen van **createQueueService**is ingesteld.

Zie voor een voorbeeld van het instellen van de omgevingsvariabelen in de [Portal van Azure](https://portal.azure.com) voor een Website Azure [Node.js WebApp met de Service van de tabel Azure].

## <a name="how-to-create-a-queue"></a>Procedure: Een wachtrij maken

De volgende code maakt een object **QueueService** , waarmee u kunt werken met wachtrijen.

    var queueSvc = azure.createQueueService();

Gebruik de methode **createQueueIfNotExists** , die geeft als de opgegeven wachtrij resultaat als dit al bestaat of die u een nieuwe wachtrij met de opgegeven naam maakt als deze nog niet bestaat.

    queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
      if(!error){
        // Queue created or exists
      }
    });

Als de wachtrij is gemaakt, `result.created` waar is. Als de wachtrij bestaat, `result.created` ONWAAR is.

### <a name="filters"></a>Filters

Optionele filterbewerkingen kunnen worden toegepast op bewerkingen die zijn uitgevoerd met **QueueService**. Bewerkingen filteren kunt opnemen logboekregistratie, automatisch opnieuw proberen, enzovoort. Filters zijn objecten die implementeren van een methode bij de handtekening:

    function handle (requestOptions, next)

Na de voorverwerking op de opties van de aanvraag, moet de methode bellen 'volgende' het doorgeven van een terugbellen met de volgende handtekening:

    function (returnObject, finalCallback, next)

In dit terugbellen en na het verwerken van de returnObject (de reactie van de aanvraag op de server), moet de terugbellen roepen volgende indien aanwezig als u wilt doorgaan met het verwerken van andere filters of roepen finalCallback gewoon anders als u wilt de service-aanroep dit uiteindelijk.

Twee filters die opnieuw logica implementeren zijn opgenomen in de SDK Azure voor Node.js, **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende maakt een **QueueService** -object dat de **ExponentialRetryPolicyFilter**gebruikt:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var queueSvc = azure.createQueueService().withFilter(retryOperations);

## <a name="how-to-insert-a-message-into-a-queue"></a>Procedure: Een bericht invoegen in een wachtrij

U een bericht in een wachtrij invoegt, gebruikt u de methode **createMessage** in een nieuw bericht maken en deze toevoegen aan de wachtrij.

    queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
      if(!error){
        // Message inserted
      }
    });

## <a name="how-to-peek-at-the-next-message"></a>Procedure: Bekijken van het volgende bericht

U kunt het bericht aan de voorzijde van een wachtrij bekijken zonder het te verwijderen uit de wachtrij door te bellen van de methode **peekMessages** . Standaard geeft **peekMessages** een enkel bericht.

    queueSvc.peekMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
      }
    });

De `result` het bericht bevat.

> [AZURE.NOTE] Met **peekMessages** wanneer er zijn dat geen berichten in de wachtrij niet een fout zullen retourneren, maar geen berichten worden geretourneerd.

## <a name="how-to-dequeue-the-next-message"></a>Procedure: Het volgende bericht in wachtrij

Verwerking van een bericht is een proces met twee fasen:

1. Het bericht in wachtrij.

2. Het bericht verwijderen.

Als u wilt een bericht in wachtrij, gebruikt u **getMessages**. Hiermee wordt de berichten onzichtbaar gemaakt in de wachtrij zodat deze kunnen worden verwerkt door geen andere clients. Wanneer uw toepassing een bericht is verwerkt, belt u **deleteMessage** om het te verwijderen uit de wachtrij. Het volgende voorbeeld wordt een bericht en wordt deze verwijderd:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
        var message = result[0];
        queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
          if(!error){
            //message deleted
          }
        });
      }
    });

> [AZURE.NOTE] Standaard wordt een bericht alleen verborgen gedurende 30 seconden, waarna deze zichtbaar voor andere clients is. U kunt een andere waarde opgeven met behulp van `options.visibilityTimeout` met **getMessages**.

> [AZURE.NOTE]
> Met **getMessages** wanneer er zijn dat geen berichten in de wachtrij niet een fout zullen retourneren, maar geen berichten worden geretourneerd.

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedure: De inhoud van een bericht in de wachtrij wijzigen

U kunt de inhoud van een bericht in-place in de wachtrij met **updateMessage**wijzigen. De tekst van een bericht wordt bijgewerkt door het volgende voorbeeld:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Got the message
        var message = result[0];
        queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
          if(!error){
            // Message updated successfully
          }
        });
      }
    });

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Procedure: Extra opties voor waarbij berichten

Er zijn twee manieren kunt u voor het bericht ophalen uit een wachtrij aanpassen:

* `options.numOfMessages`-Ophalen van een reeks berichten (maximaal 32.)
* `options.visibilityTimeout`-Een time-out langer of korter invisibility instellen.

Het volgende voorbeeld wordt de **getMessages** -methode 15 berichten in een oproep ontvangt. Vervolgens wordt verwerkt elk bericht met een lus. Ook wordt de time-out invisibility ingesteld op vijf minuten voor alle berichten die het resultaat van deze methode.

    queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
      if(!error){
        // Messages retreived
        for(var index in result){
          // text is available in result[index].messageText
          var message = result[index];
          queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
            if(!error){
              // Message deleted
            }
          });
        }
      }
    });

## <a name="how-to-get-the-queue-length"></a>Procedure: Lengte van de wachtrij ophalen

De **getQueueMetadata** retourneert metagegevens over de wachtrij, met inbegrip van de niet-geheel exacte aantal berichten staan in de wachtrij.

    queueSvc.getQueueMetadata('myqueue', function(error, result, response){
      if(!error){
        // Queue length is available in result.approximateMessageCount
      }
    });

## <a name="how-to-list-queues"></a>Procedure: Lijst wachtrijen

Als u wilt een lijst met wachtrijen ophalen, gebruikt u **listQueuesSegmented**. Als u wilt ophalen van een lijst gefilterd op een bepaald voorvoegsel, gebruikt u **listQueuesSegmentedWithPrefix**.

    queueSvc.listQueuesSegmented(null, function(error, result, response){
      if(!error){
        // result.entries contains the list of queues
      }
    });

Als alle wachtrijen kunnen niet worden geretourneerd, `result.continuationToken` kunnen worden gebruikt als eerste parameter van **listQueuesSegmented** of de tweede parameter van **listQueuesSegmentedWithPrefix** om meer resultaten te halen.

## <a name="how-to-delete-a-queue"></a>Procedure: Een wachtrij verwijderen

Als u wilt een wachtrij en alle bijbehorende berichten die dit verwijderen, belt u de methode **deleteQueue** op het object in de wachtrij.

    queueSvc.deleteQueue(queueName, function(error, response){
      if(!error){
        // Queue has been deleted
      }
    });

Schakel alle berichten van een wachtrij zonder deze te verwijderen, gebruikt u **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Hoe u: werken met gedeelde toegang handtekeningen

Gedeelde Access handtekeningen (SA's) zijn een veilige manier voor gedetailleerde toegang tot wachtrijen zonder dat uw opslagaccountnaam of toetsen. SA's worden gebruikt voor beperkte toegang tot uw wachtrijen, zoals het toestaan van een mobiele app om berichten te versturen.

Een vertrouwde toepassing, bijvoorbeeld een cloudservice genereert een SA's met de **generateSharedAccessSignature** van de **QueueService**en wordt dit aangeboden aan een niet-vertrouwde of gedeeltelijk vertrouwde-toepassing. Bijvoorbeeld: een mobiele app. De SA's wordt gegenereerd op basis van een beleid, waarin de begin- en einddatums waarin de SA's geldig is, alsmede het toegangsniveau verleend degene die SA's.

Het volgende voorbeeld wordt een nieuwe gedeelde-beleid waarmee degene SA's aan berichten toevoegen aan de wachtrij gegenereerd en verloopt 100 minuten na het tijdstip waarop dat deze is gemaakt.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

    var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
    var host = queueSvc.host;

Houd er rekening mee dat de host moet worden geïnformeerd ook als dit nodig is wanneer u degene die SA's probeert te krijgen tot de wachtrij.

De clienttoepassing vervolgens wordt de SA's gebruikt met **QueueServiceWithSAS** bewerkingen ten opzichte van de wachtrij uit te voeren. Het volgende voorbeeld is verbonden met de wachtrij en maakt een bericht.

    var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
    sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
      if(!error){
        //message added
      }
    });

Aangezien de SA's is gegenereerd met toegang toevoegen als een poging om te lezen, bijwerken of verwijderen van berichten zijn aangebracht, wordt een fout geretourneerd.

### <a name="access-control-lists"></a>Toegangscontrolelijsten

U kunt ook een Access (Toegangsbeheerlijst) gebruiken voor het instellen van het access-beleid voor een SA's. Dit is handig als u toestaan meerdere klanten toegang tot de wachtrij wilt, maar bieden verschillende clienttoegangsbeleid voor elke klant.

Een ACL wordt geïmplementeerd met van een matrix van clienttoegangsbeleid, met een ID die is gekoppeld aan elke beleid. Het volgende voorbeeld wordt gedefinieerd twee beleid; een voor 'gebruiker1' en een voor 'gebruiker2':

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Het volgende voorbeeld wordt de huidige ACL voor **DezeWachtrij**en worden vervolgens de nieuwe beleidsregels die gebruikmaken van **setQueueAcl**opgeteld. Deze methode kunt:

    var extend = require('extend');
    queueSvc.getQueueAcl('myqueue', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Wanneer de ACL is ingesteld, kunt u vervolgens een SA's op basis van de ID voor een beleid maken. Het volgende voorbeeld wordt een nieuwe SA's voor 'gebruiker2':

    queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van wachtrij opslagruimte hebt geleerd, volgt u deze koppelingen voor meer informatie over opslagtaken voor meer complexe.

-   Ga naar de [teamblog Azure opslag][].
-   Ga naar de bibliotheek [Azure opslag SDK voor knooppunt][] op GitHub.

  [Azure opslag SDK voor knooppunt]: https://github.com/Azure/azure-storage-node
  [using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: https://portal.azure.com
  [Een Node.js web-app maakt in Azure App-Service]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js WebApp met de Service van de tabel Azure]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md


  [Queue1]: ./media/storage-nodejs-how-to-use-queues/queue1.png
  [plus-new]: ./media/storage-nodejs-how-to-use-queues/plus-new.png
  [quick-create-storage]: ./media/storage-nodejs-how-to-use-queues/quick-storage.png



  [Bouwen en implementeren van een toepassing Node.js naar een Cloudservice van Azure]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Azure opslag teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Bouw en implementeer een Node.js web-app in Azure met behulp van Web-Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
