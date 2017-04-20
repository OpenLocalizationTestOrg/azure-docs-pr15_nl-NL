<properties
    pageTitle="Het gebruik van blobopslag uit Node.js | Microsoft Azure"
    description="Ongestructureerde gegevens opslaan in de cloud met Azure-blobopslag (object opslag)."
    services="storage"
    documentationCenter="nodejs"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>



# <a name="how-to-use-blob-storage-from-nodejs"></a>Het gebruik van Node.js-blobopslag

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

In dit artikel leest u hoe u veelvoorkomende scenario's met blobopslag uitvoert. In de voorbeelden worden geschreven via de API Node.js. De scenario's waarvoor zijn hoe uploaden, lijst, downloaden en BLOB's verwijderen.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Een Node.js-servicetoepassing maken

Voor instructies over het maken van een toepassing voor de Node.js, Zie [maken een Node.js web-app in Azure App-Service], [bouwen en implementeren van een toepassing Node.js naar een Cloudservice Azure] --met behulp van Windows PowerShell of [bouwen en implementeren van een Node.js web-app naar Azure met behulp van Web-Matrix].

## <a name="configure-your-application-to-access-storage"></a>Uw toepassing toegang hebben tot opslag configureren

Als u wilt gebruiken Azure opslag, moet u de SDK van Azure opslag voor Node.js, waaronder een reeks gemak bibliotheken die met de opslagruimte REST-services communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Gebruik knooppunt pakket Manager (NPM) het pakket verkrijgen

1.  Een opdrachtregel-interface zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix), gebruiken om te navigeren naar de map waar u uw voorbeeldtoepassing hebt gemaakt.

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

3.  U kunt handmatig uitvoeren met de opdracht **ls** om te controleren of die een **knooppunt\_modules** map is gemaakt. Zoek het pakket **azure-opslag** , waarin de bibliotheken die u nodig hebt voor toegang tot opslag in deze map.

### <a name="import-the-package"></a>Het pakket importeren

Voeg Kladblok of een andere teksteditor gebruikt, het volgende naar het begin van het bestand **server.js** van de toepassing waarin u van plan bent opslag gebruiken:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Een Azure Storage-verbinding instellen

De Azure module leest de omgevingsvariabelen `AZURE_STORAGE_ACCOUNT` en `AZURE_STORAGE_ACCESS_KEY`, of `AZURE_STORAGE_CONNECTION_STRING`, voor informatie die nodig is om te koppelen aan een account Azure opslag. Als deze omgevingsvariabelen niet zijn ingesteld, moet u de accountgegevens bij het aanroepen van **createBlobService**is ingesteld.

Zie voor een voorbeeld van het instellen van de omgevingsvariabelen in de [Portal van Azure](https://portal.azure.com) voor een Azure web-app, [Node.js WebApp met de Service van de tabel Azure].

## <a name="create-a-container"></a>Een container maken

Het object **BlobService** kunt u werken met containers en BLOB's. De volgende code maakt een object **BlobService** . Voeg de volgende bovenaan **server.js**:

    var blobSvc = azure.createBlobService();

> [AZURE.NOTE] U kunt een blob anoniem openen met **createBlobServiceAnonymous** en adres van de host leveren. Bijvoorbeeld, gebruikt u `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Als u wilt een nieuwe container maken, gebruikt u **createContainerIfNotExists**. Het volgende voorbeeld wordt een nieuwe container met de naam 'mycontainer':

    blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
        if(!error){
          // Container exists and is private
        }
    });

Als de container pas is gemaakt, `result.created` waar is. Als de container al bestaat, `result.created` ONWAAR is. `response`bevat informatie over de bewerking, waaronder de ETag-gegevens voor de container.

### <a name="container-security"></a>Container-beveiliging

Standaard worden nieuwe containers zijn privé en anoniem kunnen niet worden gebruikt. Als u de container openbare, zodat u toegang hebt tot deze anoniem, kunt u het toegangsniveau van de container naar **blob** of **container**instellen.

* **blob** - biedt anonieme alleen toegang tot blob inhoud en metagegevens in deze container, maar niet in de container metagegevens zoals vermelding van alle BLOB's in een container

* **container** - biedt anonieme alleen toegang tot blob inhoud en metagegevens, evenals container metagegevens

Het volgende voorbeeld ziet u het toegangsniveau naar **blob**instellen:

    blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
        if(!error){
          // Container exists and allows
          // anonymous read access to blob
          // content and metadata within this container
        }
    });

U kunt ook het toegangsniveau van een container wijzigen met behulp van **setContainerAcl** om op te geven het toegangsniveau. Het volgende voorbeeld wordt het toegangsniveau aan container gewijzigd:

    blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
      if(!error){
        // Container access level set to 'container'
      }
    });

Het resultaat bevat informatie over de bewerking, waaronder de huidige **ETag** voor de container.

### <a name="filters"></a>Filters

U kunt optioneel filterbewerkingen toepassen op de bewerkingen die zijn uitgevoerd met **BlobService**. Bewerkingen filteren kunt opnemen logboekregistratie, automatisch opnieuw proberen, enzovoort. Filters zijn objecten die implementeren van een methode bij de handtekening:

    function handle (requestOptions, next)

Na de voorverwerking op de opties van de aanvraag, de methode moet bellen 'volgende', doorgeven van een terugbellen met de volgende handtekening:

    function (returnObject, finalCallback, next)

In dit terugbellen en na het verwerken van de returnObject (de reactie van de aanvraag op de server), moet de terugbellen roepen volgende indien aanwezig als u wilt doorgaan met het verwerken van andere filters of gewoon roepen finalCallback als u wilt de service-aanroep beëindigen.

Twee filters die opnieuw logica implementeren zijn opgenomen in de SDK Azure voor Node.js, **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende maakt een **BlobService** -object dat de **ExponentialRetryPolicyFilter**gebruikt:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var blobSvc = azure.createBlobService().withFilter(retryOperations);

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Er zijn drie soorten BLOB's: BLOB's blokkeren, pagina's BLOB's en BLOB's toevoegen. Blok BLOB's kunnen dat u naar meer grote gegevens efficiënt uploaden. Toevoegen BLOB's zijn geoptimaliseerd voor bewerkingen toevoegen. Pagina BLOB's zijn geoptimaliseerd voor alleen-lezen/schrijven bewerkingen. Zie [lidmaatschap blok BLOB's, BLOB's toevoegen en BLOB van pagina's](http://msdn.microsoft.com/library/azure/ee691964.aspx)voor meer informatie.

### <a name="block-blobs"></a>Blok BLOB 's

Als u wilt uploaden gegevens naar een blob blokkeren, gebruikt u de volgende:

* **createBlockBlobFromLocalFile** - Hiermee maakt u een nieuw blok blob en uploadt de inhoud van een bestand

* **createBlockBlobFromStream** - Hiermee maakt u een nieuw blok blob en uploadt de inhoud van een stroom

* **createBlockBlobFromText** - Hiermee maakt u een nieuw blok blob en uploadt de inhoud van een tekenreeks

* **createWriteStreamToBlockBlob** - biedt een stroom schrijven naar een blob blokkeren

Het volgende voorbeeld wordt de inhoud van het bestand **test.txt** uploadt naar **myblob**.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

De `result` die het resultaat van de volgende manieren bevat informatie over de bewerking, zoals de **ETag** van de blob.

### <a name="append-blobs"></a>BLOB's toevoegen

Als u wilt uploaden gegevens naar een nieuwe toevoegen blob, gebruikt u de volgende:

* **createAppendBlobFromLocalFile** - Hiermee maakt u een nieuwe toevoegen blob en uploadt de inhoud van een bestand

* **createAppendBlobFromStream** - Hiermee maakt u een nieuwe toevoegen blob en uploadt de inhoud van een stroom

* **createAppendBlobFromText** - Hiermee maakt u een nieuwe toevoegen blob en uploadt de inhoud van een tekenreeks

* **createWriteStreamToNewAppendBlob** - Hiermee maakt u een nieuwe toevoegen blob en worden vervolgens een stream om ernaar te schrijven

Het volgende voorbeeld wordt de inhoud van het bestand **test.txt** uploadt naar **myappendblob**.

    blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Als u wilt een tekstblok toevoegen aan een bestaande toevoegen blob, gebruikt u de volgende:

* **appendFromLocalFile** - de inhoud van een bestand aan een bestaande toevoegen blob toevoegen

* **appendFromStream** - de inhoud van een stream aan een bestaande toevoegen blob toevoegen

* **appendFromText** - de inhoud van een tekenreeks toevoegen aan een bestaande toevoegen blob

* **appendBlockFromStream** - de inhoud van een stream aan een bestaande toevoegen blob toevoegen

* **appendBlockFromText** - de inhoud van een tekenreeks toevoegen aan een bestaande toevoegen blob

> [AZURE.NOTE] appendFromXXX API's doet validatie aan de clientzijde als u wilt snel om te voorkomen dat unncessary server gesprek is mislukt. appendBlockFromXXX, zijn niet.

Het volgende voorbeeld wordt de inhoud van het bestand **test.txt** uploadt naar **myappendblob**.

    blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
      if(!error){
        // text appended
      }
    });


### <a name="page-blobs"></a>Pagina BLOB 's

Als u wilt uploaden gegevens naar een pagina-blob, gebruikt u de volgende:

* **createPageBlob** - Hiermee maakt u een nieuwe pagina blob van een bepaalde lengte

* **createPageBlobFromLocalFile** - Hiermee maakt u een nieuwe pagina blob en uploadt de inhoud van een bestand

* **createPageBlobFromStream** - Hiermee maakt u een nieuwe pagina blob en uploadt de inhoud van een stroom

* **createWriteStreamToExistingPageBlob** - biedt een stroom schrijven naar een bestaande pagina blob

* **createWriteStreamToNewPageBlob** - Hiermee maakt u een nieuwe pagina blob en worden vervolgens een stream om ernaar te schrijven

Het volgende voorbeeld wordt de inhoud van het bestand **test.txt** uploadt naar **mypageblob**.

    blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

> [AZURE.NOTE] Pagina BLOB's bestaan uit 512 bytes 'pagina's '. Er wordt een foutbericht bij het uploaden van gegevens met een grootte die niet een veelvoud is van 512.

## <a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container

U kunt de BLOB's in een container, gebruikt u de methode **listBlobsSegmented** . Als u retourneren BLOB's met een bepaald voorvoegsel wilt, gebruikt u **listBlobsSegmentedWithPrefix**.

    blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
      if(!error){
          // result.entries contains the entries
          // If not all blobs were returned, result.continuationToken has the continuation token.
      }
    });

De `result` bevat een `entries` siteverzameling, dat wil zeggen een matrix van objecten die elke blob beschrijven. Als alle BLOB's kunnen niet worden geretourneerd, de `result` vindt u ook een `continuationToken`, waarmee u mogelijk als tweede parameter extra waarden worden opgehaald.

## <a name="download-blobs"></a>BLOB's downloaden

Als u wilt gegevens downloaden uit een blob, gebruikt u de volgende:

* **getBlobToLocalFile** - schrijft de inhoud blob tot een bestand

* **getBlobToStream** - schrijft de blob-inhoud naar een stroom

* **getBlobToText** - schrijft de blob-inhoud op een tekenreeks

* **createReadStream** - biedt een stroom van de blob lezen

Het volgende voorbeeld wordt gedemonstreerd **getBlobToStream** de inhoud van de blob **myblob** downloaden en naar het bestand **uitvoer.txt** opslaat met behulp van een stroom gebruiken:

    var fs = require('fs');
    blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
      if(!error){
        // blob retrieved
      }
    });

De `result` bevat informatie over de blob, waaronder **ETag** -informatie.

## <a name="delete-a-blob"></a>Een blob verwijderen

Ten slotte, als u wilt een blob verwijderen, belt u **deleteBlob**. Het volgende voorbeeld wordt de blob met de naam **myblob**verwijderd.

    blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
      if(!error){
        // Blob has been deleted
      }
    });

## <a name="concurrent-access"></a>Gelijktijdige toegang

Om gelijktijdige toegang tot een blob uit meerdere klanten of meerdere procesexemplaren, kunt u **ETags** of **lease**.

* **Etag** - biedt een manier om te bepalen die de blob of container is gewijzigd door een ander proces

* **Lease** - biedt een manier om te verkrijgen exclusief, verlengd, schrijven of toegang tot een blob verwijderen voor een bepaalde periode

### <a name="etag"></a>ETag

Gebruik ETags als u toestaan meerdere klanten of exemplaren wilt schrijven naar de blokkering Blob of pagina Blob tegelijk. De ETag kunt u bepalen als de container of blob is gewijzigd sinds u in eerste instantie lezen of die u hebt gemaakt, kunt u voorkomen dat wijzigingen die zijn doorgevoerd door een andere client of proces overschreven.

U kunt ETag voorwaarden instellen via het optionele `options.accessConditions` parameter. Het volgende voorbeeld uploads alleen het bestand **test.txt** als de blob al bestaat en heeft de waarde ETag opgenomen door `etagToMatch`.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
        if(!error){
        // file uploaded
      }
    });

Wanneer u ETags gebruikt, is de grote lijnen:

1. De ETag aanvragen als het resultaat van een maken, lijst of get-bewerking.

2. Een bewerking uitvoeren, controleren of de waarde ETag niet is gewijzigd.

Als de waarde is gewijzigd, betekent dit dat een andere client of exemplaar gewijzigd het blob of de container nadat u de waarde ETag verkregen.

### <a name="lease"></a>Lease

U kunt een nieuwe lease met behulp van de methode **acquireLease** , geven de blob of de container die u een lease wilt op aanschaffen. De volgende code wordt bijvoorbeeld een lease op **myblob**verkrijgt.

    blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
      if(!error) {
        console.log('leaseId: ' + result.id);
      }
    });

Verdere bewerkingen op **myblob** moeten ondersteuning bieden voor de `options.leaseId` parameter. De ID die wordt geretourneerd als lease `result.id` uit **acquireLease**.

> [AZURE.NOTE] Standaard is de duur van een lease oneindig. U kunt de duur van een niet-oneindig (tussen 15 en 60 seconden) opgeven door op te geven de `options.leaseDuration` parameter.

Als u wilt een lease verwijderen, gebruikt u **releaseLease**. Als u wilt een lease verbreken, maar voorkomen dat anderen een nieuwe lease totdat de oorspronkelijke duur is verlopen, gebruikt u **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Werken met gedeelde toegang handtekeningen

Gedeelde toegang handtekeningen (SA's) zijn een veilige manier voor gedetailleerde toegang tot BLOB's en containers zonder dat uw opslagaccountnaam of toetsen. Gedeelde toegang handtekeningen worden vaak gebruikt voor beperkte toegang tot uw gegevens, zoals een mobiele app voor toegang tot BLOB's toestaan.

> [AZURE.NOTE] Terwijl u kunt ook anonieme toegang tot BLOB's toestaan, wordt gedeeld access handtekeningen kunnen u meer beperkte toegang bieden, zoals moet u de SA's genereren.

Een vertrouwde toepassing, bijvoorbeeld een cloudservice genereert gedeelde toegang handtekeningen met de **generateSharedAccessSignature** van de **BlobService**en wordt dit aangeboden aan een niet-vertrouwde of gedeeltelijk vertrouwde-toepassing zoals een mobiele app. Gedeelde toegang handtekeningen worden gegenereerd met behulp van een beleid, waarin de begindatum en einddatum waarin het gedeelde access-handtekeningen geldig zijn, alsmede het toegangsniveau verleend degene handtekeningen gedeelde toegang.

Het volgende voorbeeld wordt een nieuwe gedeelde-beleid kan de gebruiker gedeelde toegang handtekeningen meer bewerkingen uitvoeren op de blob **myblob** gegenereerd en verloopt 100 minuten na het tijdstip waarop dat deze is gemaakt.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
    var host = blobSvc.host;

Houd er rekening mee dat de host moet worden geïnformeerd ook als dit nodig is als degene handtekeningen gedeelde toegang probeert te krijgen van de container.

Vervolgens kunt u de clienttoepassing wordt gedeelde toegang handtekeningen gebruikt met **BlobServiceWithSAS** bewerkingen ten opzichte van de blob uit te voeren. De volgende krijgt informatie over **myblob**.

    var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
    sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
      if(!error) {
        // retrieved info
      }
    });

Nadat de gedeelde toegang handtekeningen waren gegenereerd met alleen-lezen-toegang als wordt geprobeerd de blob wijzigen, is een fout wordt geretourneerd.

### <a name="access-control-lists"></a>Toegangscontrolelijsten

U kunt ook een toegangsbeheerlijst (ACL) gebruiken voor het instellen van het access-beleid voor SA's. Dit is handig als u wilt toestaan meerdere klanten toegang tot een container maar bieden verschillende clienttoegangsbeleid voor elke klant.

Een ACL wordt geïmplementeerd met van een matrix van clienttoegangsbeleid, met een ID die is gekoppeld aan elke beleid. Het volgende voorbeeld worden twee beleidsregels, één voor 'gebruiker1' en één voor 'gebruiker2' gedefinieerd:

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Het volgende voorbeeld wordt de huidige ACL voor **mycontainer**en telt vervolgens de nieuwe beleidsregels die gebruikmaken van **setBlobAcl**. Deze methode kunt:

    var extend = require('extend');
    blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Wanneer de ACL is ingesteld, kunt u gedeelde toegang handtekeningen op basis van de ID voor een beleid vervolgens maken. Het volgende voorbeeld wordt de nieuwe gedeelde toegang handtekeningen voor 'gebruiker2':

    blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie.

-   [Azure opslag SDK voor knooppunt API verwijzing][]
-   [Azure opslag teamblog][]
-   [Azure opslag SDK voor knooppunt][] bibliotheek op GitHub
-   [Node.js Developer Center](/develop/nodejs/)
-   [Gegevens met het hulpprogramma voor de opdrachtregel AzCopy overbrengen](storage-use-azcopy.md)

[Azure opslag SDK voor knooppunt]: https://github.com/Azure/azure-storage-node

[Een Node.js web-app maakt in Azure App-Service]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
[Node.js WebApp met de Service van de tabel Azure]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[Bouw en implementeer een Node.js web-app in Azure met behulp van Web-Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
[Bouwen en implementeren van een toepassing Node.js naar een Cloudservice van Azure]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure opslag teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure opslag SDK voor knooppunt API verwijzing]: http://dl.windowsazure.com/nodestoragedocs/index.html
