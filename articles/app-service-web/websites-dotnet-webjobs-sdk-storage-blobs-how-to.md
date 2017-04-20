<properties 
    pageTitle="Hoe u met de WebJobs SDK Azure-blobopslag" 
    description="Informatie over het gebruiken van Azure-blobopslag met de WebJobs SDK. Een proces te activeren wanneer een nieuwe blob wordt weergegeven in een container en afhandelen poison BLOB's." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a>Hoe u met de WebJobs SDK Azure-blobopslag

## <a name="overview"></a>Overzicht

Deze handleiding bevat voorbeelden C#-code zien hoe u een proces te activeren wanneer een Azure blob is gemaakt of bijgewerkt. Voorbeelden van de code gebruik [WebJobs SDK](websites-dotnet-webjobs-sdk.md) versie 1.x.

Raadpleeg [het gebruik van Azure wachtrij opslag met de SDK WebJobs](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)voor codevoorbeelden zien hoe u BLOB's maken. 
        
De handleiding wordt ervan uitgegaan dat u weet [hoe u een project WebJob in Visual Studio met verbindingstekenreeksen die naar uw account opslag verwijzen maakt](websites-dotnet-webjobs-sdk-get-started.md) of [meerdere opslag-accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

## <a id="trigger"></a>Hoe u een functie te activeren wanneer een blob is gemaakt of bijgewerkt

In deze sectie wordt uitgelegd hoe u de `BlobTrigger` kenmerk. 

> [AZURE.NOTE] De WebJobs SDK gescand logboekbestanden wil bekijken voor nieuwe of gewijzigde BLOB's. Dit proces is niet realtime; een functie mogelijk niet worden geactiveerd tot enkele minuten of langer nadat de blob is gemaakt. Daarnaast [opslag logboeken zijn gemaakt op een "beste inspanningen"](https://msdn.microsoft.com/library/azure/hh343262.aspx) soort_jaar; Er is geen garantie dat alle gebeurtenissen worden worden opgenomen. Onder bepaalde omstandigheden de logboeken kunnen worden overgeslagen. Als u de snelheid en betrouwbaarheid beperkingen van blob triggers zijn niet toegestaan voor uw toepassing, wordt u aangeraden een bericht wachtrij maken wanneer u de blob maken en gebruiken van het kenmerk [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) in plaats van de `BlobTrigger` kenmerk van de functie die de blob verwerkt.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Enkele tijdelijke aanduiding voor de naam van de blob met de extensie  

Het volgende voorbeeld wordt gekopieerd tekst BLOB's die worden weergegeven in de container *invoer* aan de container *uitvoer* :

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

De attribuutconstructor kent de tekenreeksparameter van een die Hiermee geeft u de containernaam van de en een tijdelijke aanduiding voor de naam van de blob. Als een benoemde *Blob1.txt* blob is gemaakt in de container *invoer* , maakt de functie in dit voorbeeld een blob met de naam *Blob1.txt* in de container *uitvoer* . 

Zoals in het volgende voorbeeld, kunt u met de naam blob tijdelijke aanduiding voor een naampatroon opgeven:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Deze code wordt alleen BLOB's die namen die beginnen met 'oorspronkelijke--' hebt gekopieerd. *Oorspronkelijke-Blob1.txt* in de container *invoer* wordt bijvoorbeeld gekopieerd naar *Kopie-Blob1.txt* in de container *uitvoer* .

Als u een naampatroon opgeven voor blob namen die accolades in de naam moet, dubbelklikt u de accolades. Stel dat u wilt zoeken BLOB's in de container met *afbeeldingen* met namen als volgt:

        {20140101}-soundfile.mp3

Gebruik deze optie voor het patroon:

        images/{{20140101}}-{name}

In het voorbeeld is *de waarde van tijdelijke aanduiding voor* *soundfile.mp3*. 

### <a name="separate-blob-name-and-extension-placeholders"></a>Tijdelijke aanduidingen de naam en een toestelnummer van de afzonderlijke blob

Het volgende voorbeeld wordt de bestandsextensie zoals BLOB's die worden weergegeven in de container *invoer* aan de container *uitvoer* worden gekopieerd. De code de uitbreiding van de *invoer* blob logboeken en Hiermee stelt u de uitbreiding van de *uitvoer* blob op *.txt*.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a id="types"></a>Typen die u met BLOB's verbinden kunt

U kunt de `BlobTrigger` kenmerk op de volgende typen:

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Andere typen gedeserialiseerd door [ICloudBlobStreamBinder](#icbsb) 

Als u werken rechtstreeks met de opslag van Azure-account wilt, kunt u ook toevoegen een `CloudStorageAccount` -parameter voor de methodehandtekening.

Zie de [blob binding-code in de bibliotheek azure-webjobs-sdk op GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs)voor voorbeelden.

## <a id="string"></a>Aan de inhoud van de tekst blob binding met tekenreeks

Als u tekst BLOB's worden verwacht, `BlobTrigger` kunnen worden toegepast op een `string` parameter. Het volgende voorbeeld wordt een blob tekst naar een `string` parameter met de naam `logMessage`. De functie gebruikt die parameter de inhoud van de blob naar het dashboard WebJobs SDK schrijven. 
 
        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>Blob inhoud aan serienummer met behulp van ICloudBlobStreamBinder

Het volgende voorbeeld wordt gebruikt voor een klasse die implementeert `ICloudBlobStreamBinder` om in te schakelen de `BlobTrigger` kenmerk binding maken van een blob naar de `WebImage` type.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK", 
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

De `WebImage` code binden vindt u in een `WebImageBinder` class waar vandaan `ICloudBlobStreamBinder`.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input, 
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="getting-the-blob-path-for-the-triggering-blob"></a>Het pad blob ophalen voor de die als trigger dient blob

U kunt de containernaam en blob naam van de blob die de functie heeft geactiveerd bevatten een `blobTrigger` parameter in de functiehandtekening tekenreeks.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>Hoe u omgaat met poison BLOB 's

Wanneer een `BlobTrigger` functie mislukt, de SDK belt deze opnieuw, als het probleem wordt veroorzaakt door een tijdelijke fout. Als het probleem wordt veroorzaakt door de inhoud van de blob, wordt de functie mislukt telkens wanneer de blob wordt verwerkt. Standaard roept de SDK een functie maximaal 5 tijden voor een bepaald blob. Als het vijfde uitproberen mislukt, wordt een bericht in de SDK toegevoegd aan een wachtrij *webjobs-blobtrigger-poison*met de naam.

Het maximum aantal pogingen kan worden geconfigureerd. Dezelfde [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) instelling wordt gebruikt voor de afhandeling van poison blob en poison wachtrij bericht afhandeling. 

Het bericht wachtrij voor poison BLOB's is een JSON-object dat de volgende eigenschappen heeft:

* FunctionId (in de notatie *{WebJob naam}*. Functies zijn opgenomen. *{Functienaam}*, bijvoorbeeld: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" of "PageBlob")
* ContainerName
* BlobName
* ETag (een blob-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

Klik in het volgende voorbeeld, de `CopyBlob` functie heeft code die ervoor zorgt dat mislukt telkens wanneer dit wordt genoemd. Nadat de SDK deze voor het maximum aantal pogingen oproepen, een bericht in de wachtrij poison blob wordt gemaakt en dat bericht wordt verwerkt door het `LogPoisonBlob` functie. 

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }
        
        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

De SDK deserializes automatisch de JSON-bericht. Hier ziet u de `PoisonBlobMessage` class: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>BLOB peiling algoritme

De WebJobs SDK gescand alle containers opgegeven door `BlobTrigger` kenmerken aan begin van de toepassing. In een grote opslag-account deze scan kan enige tijd duren, zodat het mogelijk tijd voordat de nieuwe BLOB's zijn gevonden en `BlobTrigger` functies worden uitgevoerd.

Om op te sporen nieuwe of gewijzigde BLOB's na toepassing start, worden de SDK regelmatig leest van de blob storage Logboeken. De logboeken blob zijn buffer en alleen krijgen fysiek geschreven 10 minuten of zo is, zodat er aanzienlijk vertraging optreden nadat een blob wordt gemaakt of voordat u de bijbehorende bijgewerkt `BlobTrigger` functie wordt uitgevoerd. 

Er is een uitzondering voor BLOB's die u met behulp van maakt de `Blob` kenmerk. Als de WebJobs SDK Hiermee maakt u een nieuwe blob, wordt de nieuwe blob onmiddellijk doorgegeven op een overeenkomende `BlobTrigger` functies. Daarom hebt u een reeks blob invoer en uitvoer, kunt de SDK verwerken ze efficiënt. Maar als u wilt dat lage latentie uitvoeren van uw blob verwerkingsfuncties voor BLOB's die zijn gemaakt of op een andere manier bijgewerkt, gebruiken aangeraden `QueueTrigger` plaats `BlobTrigger`.

### <a id="receipts"></a>BLOB bevestigingen

De WebJobs SDK zorgt ervoor dat er geen `BlobTrigger` functie meer dan één keer wordt aangeroepen voor de dezelfde nieuwe of bijgewerkte blob. Dit gebeurt door *blob bevestigingen* onderhouden om te bepalen of een versie van de opgegeven blob is verwerkt.

BLOB ontvangstbevestigingen worden opgeslagen in een container met de naam *azure-webjobs-hosts* in het opgegeven door de verbindingsreeks AzureWebJobsStorage Azure opslag-account. Een leesbevestiging blob heeft de volgende informatie:

* De functie die heet voor de blob ("*{WebJob naam}*. Functies zijn opgenomen. *{Functienaam}*", bijvoorbeeld:"WebJob1.Functions.CopyBlob")
* De containernaam van de
* Het type blob ("BlockBlob" of "PageBlob")
* De naam van de blob
* De ETag (een blob-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

Als u afdwingen opnieuw verwerken van een blob wilt, kunt u de ontvangst blob voor die blob handmatig verwijderen uit de container *azure-webjobs-hosts* .

## <a id="queues"></a>Verwante onderwerpen bestrijkt het artikel wachtrijen

Voor informatie over hoe u omgaat met blob verwerking geactiveerd door een bericht wachtrij of voor WebJobs SDK Zie scenario's die niet specifiek zijn voor blob processing, [Azure wachtrij opslagruimte met de SDK WebJobs gebruiken](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Verwante onderwerpen in dit artikel zijn:

* Asynchrone functies
* Meerdere exemplaren
* Correcte afsluiten
* WebJobs SDK kenmerken in de hoofdtekst van een functie gebruiken
* Stel de SDK verbindingstekenreeksen in code.
* Waarden instellen voor WebJobs SDK constructor parameters in code
* Configureren `MaxDequeueCount` voor het verwerken van poison blob.
* Handmatig een functie activeren
* Logboeken schrijven

## <a id="nextsteps"></a>Volgende stappen

Deze handleiding biedt voorbeelden van de code die wordt aangegeven hoe u omgaat met veelvoorkomende scenario's voor het werken met Azure BLOB's. Zie voor meer informatie over het gebruik van Azure WebJobs en de SDK WebJobs [Azure WebJobs aanbevolen Resources](http://go.microsoft.com/fwlink/?linkid=390226).
 
