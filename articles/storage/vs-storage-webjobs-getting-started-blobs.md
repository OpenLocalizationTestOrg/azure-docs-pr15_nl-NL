<properties
    pageTitle="Aan de slag met blob storage en Visual Studio verbonden services (WebJob projecten) | Microsoft Azure"
    description="Hoe u aan de slag met blobopslag in een project WebJob na het verbinding maken met een Azure opslag gebruik van Visual Studio verbonden services."
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Aan de slag met Azure Blob storage en Visual Studio verbonden services (WebJob projecten)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

In dit artikel vindt u voorbeelden C#-code zien hoe u een proces te activeren wanneer een Azure blob is gemaakt of bijgewerkt. Voorbeelden van de code gebruiken de versie [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) 1.x. Wanneer u een opslag-account aan een project WebJob toevoegt met behulp van het dialoogvenster Visual Studio, **Verbonden Services toevoegen** , het juiste Azure opslag NuGet-pakket is geïnstalleerd, de juiste .NET-verwijzingen worden toegevoegd aan het project en verbindingstekenreeksen met de voor de opslag-account in het bestand App.config worden bijgewerkt.



## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Hoe u een functie te activeren wanneer een blob is gemaakt of bijgewerkt

Hier vindt u het gebruik van het kenmerk **BlobTrigger** .

 **Notitie:** De WebJobs SDK gescand logboekbestanden wil bekijken voor nieuwe of gewijzigde BLOB's. Dit proces is inherent traag; een functie mogelijk niet worden geactiveerd tot enkele minuten of langer nadat de blob is gemaakt.  Als uw toepassing BLOB's direct proces moet, wordt de aanbevolen methode is een bericht wachtrij maken wanneer u de blob maken en gebruiken van het kenmerk [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) in plaats van het kenmerk **BlobTrigger** van de functie die de blob verwerkt.

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

## <a name="types-that-you-can-bind-to-blobs"></a>Typen die u met BLOB's verbinden kunt

Klik op de volgende typen kunt u het kenmerk **BlobTrigger** :

* **tekenreeks**
* **TextReader**
* **Stream**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Andere typen gedeserialiseerd door [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)

Als u werken rechtstreeks met de opslag van Azure-account wilt, kunt u ook een parameter **CloudStorageAccount** toevoegen aan de methodehandtekening.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Aan de inhoud van de tekst blob binding met tekenreeks

Als u tekst BLOB's worden verwacht, kunnen **BlobTrigger** worden toegepast op een parameter **tekenreeks** . Het volgende voorbeeld wordt een blob tekst aan de parameter van een **tekenreeks** met de naam **logMessage**. De functie gebruikt die parameter de inhoud van de blob naar het dashboard WebJobs SDK schrijven.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Blob inhoud aan serienummer met behulp van ICloudBlobStreamBinder

Het volgende voorbeeld wordt een klasse die **ICloudBlobStreamBinder implementeert** zodat het kenmerk **BlobTrigger** binding maken van een blob met het type **WebImage** gebruikt.

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

De **WebImage** binding-code is opgegeven in een **WebImageBinder** klasse dat is afgeleid van **ICloudBlobStreamBinder**.

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

## <a name="how-to-handle-poison-blobs"></a>Hoe u omgaat met poison BLOB 's

Wanneer een functie **BlobTrigger** mislukt, oproepen de SDK deze opnieuw, als het probleem wordt veroorzaakt door een tijdelijke fout. Als het probleem wordt veroorzaakt door de inhoud van de blob, wordt de functie mislukt telkens wanneer de blob wordt verwerkt. Standaard roept de SDK een functie maximaal 5 tijden voor een bepaald blob. Als het vijfde uitproberen mislukt, wordt een bericht in de SDK toegevoegd aan een wachtrij *webjobs-blobtrigger-poison*met de naam.

Het maximum aantal pogingen kan worden geconfigureerd. Dezelfde [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) instelling wordt gebruikt voor de afhandeling van poison blob en poison wachtrij bericht afhandeling.

Het bericht wachtrij voor poison BLOB's is een JSON-object dat de volgende eigenschappen heeft:

* FunctionId (in de notatie *{WebJob naam}*. Functies zijn opgenomen. *{Functienaam}*, bijvoorbeeld: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" of "PageBlob")
* ContainerName
* BlobName
* ETag (een blob-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

In het volgende voorbeeld bevat de functie **CopyBlob** code die ervoor zorgt dat mislukt telkens wanneer dit wordt genoemd. Nadat de SDK deze voor het maximum aantal pogingen oproepen, een bericht in de wachtrij poison blob wordt gemaakt en dat bericht door de functie **LogPoisonBlob** wordt verwerkt.

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

De SDK deserializes automatisch de JSON-bericht. Hier ziet u de klas **PoisonBlobMessage** :

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>BLOB peiling algoritme

De WebJobs SDK gescand alle containers opgegeven door **BlobTrigger** kenmerken aan begin van de toepassing. In een grote opslag-account deze scan kan enige tijd duren, zodat het mogelijk een tijdje voordat nieuwe BLOB's zijn gevonden en **BlobTrigger** -functies worden uitgevoerd.

Om op te sporen nieuwe of gewijzigde BLOB's na toepassing start, worden de SDK regelmatig leest van de blob storage Logboeken. De logboeken blob zijn buffer en alleen krijgen fysiek geschreven 10 minuten of zo is, zodat er aanzienlijk vertraging optreden nadat een blob wordt gemaakt of voordat u de bijbehorende bijgewerkt **BlobTrigger** -functie wordt uitgevoerd.

Er is een uitzondering voor BLOB's die u maakt met het kenmerk **Blob** . Wanneer de WebJobs SDK een nieuwe blob maakt, doorgegeven deze de nieuwe blob onmiddellijk aan een overeenkomende **BlobTrigger** -functies. Daarom hebt u een reeks blob invoer en uitvoer, kunt de SDK verwerken ze efficiënt. Maar indien lage latentie uitgevoerd van uw blob processing-functies voor BLOB's die zijn gemaakt of op een andere manier bijgewerkt gewenst, wordt aangeraden gebruikt **QueueTrigger** in plaats van **BlobTrigger**.

### <a name="blob-receipts"></a>BLOB bevestigingen

De WebJobs SDK zorgt ervoor dat geen **BlobTrigger** -functie wordt aangeroepen meer dan eens de dezelfde nieuwe of bijgewerkte blob. Dit gebeurt door *blob bevestigingen* onderhouden om te bepalen of een versie van de opgegeven blob is verwerkt.

BLOB ontvangstbevestigingen worden opgeslagen in een container met de naam *azure-webjobs-hosts* in het opgegeven door de verbindingsreeks AzureWebJobsStorage Azure opslag-account. Een leesbevestiging blob heeft de volgende informatie:

* De functie die heet voor de blob ("*{WebJob naam}*. Functies zijn opgenomen. *{Functienaam}*", bijvoorbeeld:"WebJob1.Functions.CopyBlob")
* De containernaam van de
* Het type blob ("BlockBlob" of "PageBlob")
* De naam van de blob
* De ETag (een blob-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

Als u afdwingen opnieuw verwerken van een blob wilt, kunt u de ontvangst blob voor die blob handmatig verwijderen uit de container *azure-webjobs-hosts* .

## <a name="related-topics-covered-by-the-queues-article"></a>Verwante onderwerpen bestrijkt het artikel wachtrijen

Voor informatie over hoe u omgaat met blob verwerking geactiveerd door een bericht wachtrij of voor WebJobs SDK Zie scenario's die niet specifiek zijn voor blob processing, [Azure wachtrij opslagruimte met de SDK WebJobs gebruiken](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).

Verwante onderwerpen in dit artikel zijn:

* Asynchrone functies
* Meerdere exemplaren
* Correcte afsluiten
* WebJobs SDK kenmerken in de hoofdtekst van een functie gebruiken
* Stel de SDK verbindingstekenreeksen in code.
* Waarden instellen voor WebJobs SDK constructor parameters in code
* **MaxDequeueCount** configureren voor de afhandeling poison blob.
* Handmatig een functie activeren
* Logboeken schrijven

## <a name="next-steps"></a>Volgende stappen

In dit artikel biedt voorbeelden van de code die wordt aangegeven hoe u omgaat met veelvoorkomende scenario's voor het werken met Azure BLOB's. Zie voor meer informatie over het gebruik van Azure WebJobs en de SDK WebJobs [Azure WebJobs documentatie resources](http://go.microsoft.com/fwlink/?linkid=390226).
