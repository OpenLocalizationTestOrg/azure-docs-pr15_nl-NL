<properties
    pageTitle="Zelfstudie: aan de slag met de bibliotheek Azure Batch .NET | Microsoft Azure"
    description="Informatie over de basisbeginselen van Azure Batch en het ontwikkelen van voor de Batch-service met een voorbeeldscenario."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="08/15/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-library-for-net"></a>Aan de slag met de bibliotheek Azure Batch voor .NET

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Informatie over de basisbeginselen van [Azure Batch] [ azure_batch] en de [Batch.NET] [ net_api] bibliotheek in dit artikel als wordt een C# steekproef toepassing stap voor stap besproken. Besproken hoe deze toepassing van de steekproef worden gebruikt in de Batch-service te verwerken van een parallelle werkbelasting in de cloud, en hoe deze met [Azure-opslag](../storage/storage-introduction.md) voor bestand tijdelijke en voor het ophalen samenwerkt. Hier leert u veelvoorkomende Batch toepassing werkstroom technieken. U hebt ook een grondtal begrip van de belangrijkste onderdelen van de Batch, zoals taken, taken, pools, krijgen en knooppunten berekenen.

![Batch oplossing werkstroom (basic)][11]<br/>

## <a name="prerequisites"></a>Vereisten voor

In dit artikel wordt ervan uitgegaan dat u eerder gewerkt van C# en Visual Studio hebt. Ook wordt ervan uitgegaan dat het lukt om te voldoen aan de vereisten voor het maken van account die zijn opgegeven onder voor Azure en de Batch- en -services.

### <a name="accounts"></a>Accounts

- **Azure-account**: als u nog geen een Azure-abonnement, [een gratis Azure-account maken][azure_free_account].
- **Batch account**: wanneer u een Azure-abonnement, [Maak een Batch Azure-account](batch-account-create-portal.md)hebt.
- **Opslag-account**: Zie [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) in [over Azure opslag-accounts](../storage/storage-create-storage-account.md).

> [AZURE.IMPORTANT] Batch momenteel ondersteunt *alleen* het **algemene** opslag-accounttype, zoals beschreven in stap #5 [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) in [over Azure opslag-accounts](../storage/storage-create-storage-account.md).

### <a name="visual-studio"></a>Visual Studio

U moet **Visual Studio-2015** maken van het voorbeeldproject hebben. U kunt gratis en proefabonnement versies van Visual Studio vinden in het [overzicht van producten voor Visual Studio-2015][visual_studio].

### <a name="dotnettutorial-code-sample"></a>Voorbeeld van de code *DotNetTutorial*

De [DotNetTutorial] [ github_dotnettutorial] voorbeeld is een van de vele codevoorbeelden zijn gevonden in de [azure-batch-voorbeelden] [ github_samples] bibliotheek op GitHub. U kunt de steekproef downloaden door te klikken op de knop **Downloaden ZIP** op de startpagina van de bibliotheek of door te klikken op de [azure-batch-voorbeelden-master.zip] [ github_samples_zip] directe downloadkoppeling. Nadat u de inhoud van het ZIP-bestand hebt uitgepakt, vindt u de oplossing in de volgende map:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure Batch Explorer (optioneel)

De [Azure Batch Explorer] [ github_batchexplorer] is een gratis hulpprogramma die deel van de [azure-batch-voorbeelden uitmaakt] [ github_samples] bibliotheek op GitHub. Terwijl u niet hoeft te voltooien van deze zelfstudie, kan het handig zijn bij het ontwikkelen en voor foutopsporing in uw Batch-oplossingen.

## <a name="dotnettutorial-sample-project-overview"></a>DotNetTutorial steekproef projectoverzicht

In het voorbeeld *DotNetTutorial* is een Visual Studio-2015-oplossing die uit twee projecten bestaat: **DotNetTutorial** en **TaskApplication**.

- **DotNetTutorial** is de clienttoepassing waarin interactie met de Batch en opslagruimte services een parallelle werkbelasting uitvoeren op berekenen knooppunten (virtuele machines). DotNetTutorial op uw lokale computer wordt uitgevoerd.

- **TaskApplication** is het programma die wordt uitgevoerd op berekeningscluster knooppunten in Azure om uit te voeren van de werkelijke hoeveelheid werk. In de steekproef, `TaskApplication.exe` parseert u de tekst in een bestand is gedownload van Azure Storage (de invoer-bestand). Het resultaat vervolgens een tekstbestand (het uitvoerbestand) waarin een lijst met de bovenste drie woorden die worden weergegeven in het bestand dat invoer. Nadat het uitvoerbestand is gemaakt, uploadt TaskApplication het bestand met Azure Storage. Hiermee wordt het toegankelijk is met de clienttoepassing voor downloaden. TaskApplication parallel op meerdere berekeningscluster knooppunten in de Batch-service wordt uitgevoerd.

In het volgende diagram ziet u de primaire bewerkingen die worden uitgevoerd door de clienttoepassing, *DotNetTutorial*en de toepassing die wordt uitgevoerd door de taken, *TaskApplication*. Deze eenvoudige werkstroom wordt gebruikt voor veel berekeningscluster-oplossingen die zijn gemaakt met de Batch. Terwijl deze niet alle functies beschikbaar zijn in de service Batch tonen wordt, bevat bijna elke Batch scenario vergelijkbare processen.

![Voorbeeldwerkstroom batch][8]<br/>

[**Stap 1.**](#step-1-create-storage-containers) **Containers** in Azure-blobopslag maken.<br/>
[**Stap 2.**](#step-2-upload-task-application-and-data-files) Taak toepassingsbestanden en invoer bestanden uploaden naar containers.<br/>
[**Stap 3.**](#step-3-create-batch-pool) Een Batch- **groep**maken.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** De groep **starttaak** downloads de binaire bestanden van taak (TaskApplication) naar knooppunten, terwijl ze deelnemen aan de groep.<br/>
[**Stap 4.**](#step-4-create-batch-job) Maak een Batch- **taak**.<br/>
[**Stap 5.**](#step-5-add-tasks-to-job) **Taken** toevoegen aan de taak.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** De taken worden gepland op knooppunten uitvoeren.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5 ter.** Elke taak de invoergegevens gedownload van Azure Storage en vervolgens wordt uitgevoerd.<br/>
[**Stap 6.**](#step-6-monitor-tasks) Taken controleren.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Als taken zijn voltooid, moet ze hun uitvoergegevens met Azure Storage uploaden.<br/>
[**Stap 7.**](#step-7-download-task-output) Download taak uitvoer van opslag.

Zoals vermeld, niet elke Batch-oplossing op de deze exacte stappen uitvoert, en nog veel meer kan bevatten, maar de toepassing van de steekproef *DotNetTutorial* ziet u veelvoorkomende processen zijn gevonden in een oplossing Batch.

## <a name="build-the-dotnettutorial-sample-project"></a>Het project van de steekproef *DotNetTutorial* maken

Voordat u de steekproef uitvoeren kunt, moet u zowel opslag als Batch accountreferenties in van het project *DotNetTutorial* `Program.cs` bestand. Als u dit nog niet hebt gedaan, opent u de oplossing in Visual Studio door te dubbelklikken op de `DotNetTutorial.sln` oplossingsbestand. Of dit uit in Visual Studio openen met de **Bestand > openen > Project/oplossing** menu.

Open `Program.cs` binnen het project *DotNetTutorial* . Als opgegeven boven aan het bestand, klikt u vervolgens uw referenties toevoegen:

```
// Update the Batch and Storage account credential strings below with the values
// unique to your accounts. These are used when constructing connection strings
// for the Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [AZURE.IMPORTANT] Zoals eerder vermeld, moet u momenteel de referenties voor een **algemene** opslag-account in Azure Storage opgeven. Uw toepassingen Batch gebruik blobopslag binnen het **algemene** opslag-account. Geef de referenties voor een opslag-account dat is gemaakt door te selecteren van het accounttype *blobopslag* geen.

U kunt uw accountreferenties Batch en opslagruimte binnen het blad account van elke service vinden in de [portal van Azure][azure_portal]:

![Referenties in de portal batch][9]
![opslag van referenties in de portal][10]<br/>

Nu u het project hebt bijgewerkt met uw referenties, met de rechtermuisknop op de oplossing in Solution Explorer en klik op **Oplossing maken**. Bevestig het herstel van alle NuGet-pakketten, als u wordt gevraagd.

> [AZURE.TIP] Als de NuGet-pakketten niet automatisch worden hersteld, of als u fouten over de pakketten herstellen niet ziet, zorgen dat u de [NuGet Package Manager] [ nuget_packagemgr] is geïnstalleerd. Schakel het downloaden van ontbrekende pakketten. Zie [Inschakelen pakket herstellen tijdens bouwen] [ nuget_restore] om pakket downloaden te maken.

In de volgende secties we Splits op de voorbeeldtoepassing in de stappen die worden uitgevoerd te verwerken van een werkbelasting in de Batch-service en worden deze stappen in detail beschreven. Is het raadzaam om te verwijzen naar de geopende oplossing in Visual Studio, terwijl u uw weg de rest van dit artikel, werkt omdat u niet elke regel met code in de steekproef wordt besproken.

Ga naar het begin van de `MainAsync` methode in van het project *DotNetTutorial* `Program.cs` bestand naar het begin met stap 1. Elke stap hieronder vervolgens ongeveer volgt u de voortgang van de methode oproepen in `MainAsync`.

## <a name="step-1-create-storage-containers"></a>Stap 1: Opslagcontainers maken

![Containers maken in Azure-opslag][1]
<br/>

Batch biedt ingebouwde ondersteuning voor de communicatie met Azure Storage. Containers in uw account opslag, vindt u de bestanden die nodig zijn voor de taken die worden uitgevoerd in uw account Batch. De containers bieden ook een plaats om de opslag van de uitvoergegevens die de taken produceren. Het eerste wat dat de clienttoepassing *DotNetTutorial* heeft is drie containers in [Azure-blobopslag](../storage/storage-introduction.md)maken:

- **toepassing**: deze container de toepassing macro's starten met de taken, evenals alle bijbehorende afhankelijkheden, zoals dll-bestanden worden opgeslagen.
- **Invoerbereik**: taken de gegevensbestanden verwerkingstijd van de *invoer* container wordt gedownload.
- **uitvoer**: wanneer u taken hebt voltooid invoer bestandsverwerking, worden ze de resultaten uploaden naar de container *uitvoer* .

Om te communiceren met een account voor de opslag en containers maken, gebruiken we de [Azure opslag clientbibliotheek voor .NET][net_api_storage]. We een verwijzing naar het account maken met [CloudStorageAccount][net_cloudstorageaccount], en maak een [CloudBlobClient]uit die[net_cloudblobclient]:

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create the blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

We gebruiken de `blobClient` verwijzen naar in de toepassing en als een parameter doorgegeven aan verschillende methoden. Een voorbeeld hiervan is in de codeblok direct na de bovenstaande, waar verwijst naar de `CreateContainerIfNotExistAsync` daadwerkelijk de containers maken.

```csharp
// Use the blob client to create the containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

Zodra de containers zijn gemaakt, kan de toepassing nu de bestanden die worden gebruikt door de taken uploaden.

> [AZURE.TIP] [Het gebruik van .NET-blobopslag](../storage/storage-dotnet-how-to-use-blobs.md) bevat een handig overzicht van het werken met Azure Storage containers en BLOB's. Dit komt boven aan de lijst lezen terwijl u aan de slag met Batch.

## <a name="step-2-upload-task-application-and-data-files"></a>Stap 2: Taak toepassings- en bestanden uploaden

![Toepassing voor de taak en invoer (gegevens) bestanden uploaden naar containers][2]
<br/>

In de bewerking voor het uploaden van bestanden definieert *DotNetTutorial* eerst verzamelingen van **toepassing** en **invoer** bestandspaden die zijn op de lokale computer. En vervolgens deze deze bestanden naar de containers die u in de vorige stap hebt gemaakt uploadt.

```
// Paths to the executable and its dependencies that will be executed by the tasks
List<string> applicationFilePaths = new List<string>
{
    // The DotNetTutorial project includes a project reference to TaskApplication,
    // allowing us to determine the path of the task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// The collection of data files that are to be processed by the tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload the application and its dependencies to Azure Storage. This is the
// application that will process the data files, and will be executed by each
// of the tasks on the compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload the data files. This is the data that will be processed by each of
// the tasks that are executed on the compute nodes within the pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

Er zijn twee manieren in `Program.cs` die betrokken zijn bij het uploadproces:

- `UploadFilesToContainerAsync`: Deze methode geeft als resultaat een verzameling [ResourceFile] [ net_resourcefile] objecten (Zie hieronder) en intern oproepen `UploadFileToContainerAsync` om elk bestand dat is doorgegeven in de parameter *filePaths* te uploaden.
- `UploadFileToContainerAsync`: Dit is de methode die daadwerkelijk uitvoert van het bestand te uploaden en Hiermee maakt u de [ResourceFile] [ net_resourcefile] objecten. Na het uploaden van het bestand, er wordt opgehaald van de handtekening van een gedeelde access (SA's) voor het bestand en geeft als resultaat een ResourceFile-object met deze. Access handtekeningen worden ook besproken onder gedeeld.

```
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} to container [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath, FileMode.Open);

        // Set the expiry time and permissions for the blob shared access signature.
        // In this case, no start time is specified, so the shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct the SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles

Een [ResourceFile] [ net_resourcefile] vindt u taken in de Batch met de URL naar een bestand in Azure-opslag die wordt gedownload naar een berekeningscluster knooppunt voordat deze taak wordt uitgevoerd. De [ResourceFile.BlobSource] [ net_resourcefile_blobsource] eigenschap Hiermee geeft u de volledige URL van het bestand zoals deze in Azure opslag zijn. De URL mogelijk ook een gedeelde access-handtekening (SA's) die beveiligde toegang tot het bestand geeft. De meeste taken typen binnen Batch .NET bevatten een eigenschap *ResourceFiles* , waaronder:

- [CloudTask][net_task]
- [Starttaak][net_pool_starttask]
- [JobPreparationTask][net_jobpreptask]
- [JobReleaseTask][net_jobreltask]

De toepassing van de steekproef DotNetTutorial geen gebruikmaakt van de taaktypen JobPreparationTask of JobReleaseTask, maar u kunt meer informatie over deze in [uitvoeren voorbereiden en voltooiing projecttaken op Azure Batch knooppunten berekenen](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Gedeelde toegang handtekening (SA's)

Gedeelde toegang handtekeningen zijn tekenreeksen die, wanneer opgenomen als onderdeel van een URL, beveiligde toegang bieden tot containers en BLOB's in Azure opslag. De toepassing wordt gebruikt voor zowel blob als container gedeeld DotNetTutorial handtekening URL's openen en te laat zien hoe deze tekenreeksen van de handtekening gedeelde toegang verkrijgen met de Storage-service.

- **BLOB gedeeld access handtekeningen**: van de groep starttaak in DotNetTutorial blob gedeeld access handtekeningen wordt gebruikt wanneer het downloaden van de toepassing binaire bestanden en de invoergegevens bestanden uit de opslag (Zie stap #3). De `UploadFileToContainerAsync` methode in de DotNetTutorial `Program.cs` bevat de code die wordt opgehaald van elke blob gedeelde toegang handtekening. Dit gebeurt door te bellen [CloudBlob.GetSharedAccessSignature][net_sas_blob].

- **Container gedeeld access handtekeningen**: als het werk op het knooppunt berekeningscluster van elke taak heeft voltooid, deze de uitvoerbestand uploadt naar de container *uitvoer* in Azure Storage. Hiervoor gebruik TaskApplication een container gedeelde access-handtekening waarmee schrijftoegang aan de container als onderdeel van het pad wanneer deze het bestand hebt geüpload. Het verkrijgen van de container gedeelde toegang handtekening is voltooid op een vergelijkbare manier als wanneer het verkrijgen van de blob access handtekening gedeeld. In DotNetTutorial, zult u zien dat de `GetContainerSasUrl` helpmethode roept [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] kunt doen. U moet lees meer informatie over het gebruik van de container met TaskApplication access handtekening in gedeeld "stap 6: monitortaken."

> [AZURE.TIP] Bekijk de reeks uit twee delen op gedeelde toegang handtekeningen, [deel 1: informatie over het model van de handtekening (SA's) gedeelde toegang](../storage/storage-dotnet-shared-access-signature-part-1.md) en [deel 2: maken en gebruiken van een gedeelde access-handtekening (SA's) met Blob storage](../storage/storage-dotnet-shared-access-signature-part-2.md), voor meer informatie over een beveiligde toegang tot gegevens in uw account opslag.

## <a name="step-3-create-batch-pool"></a>Stap 3: Batch groep maken

![Een resourcegroep die Batch maken][3]
<br/>

Een Batch die **groep** is een verzameling berekeningscluster knooppunten (virtuele machines) waarop Batch van een taak taken uitvoeren.

Nadat deze de toepassing en gegevens-bestanden naar de opslag-account uploadt, wordt de interactie met de Batch-service in *DotNetTutorial* gestart met behulp van de Batch .NET-bibliotheek. Hiervoor een [BatchClient] [ net_batchclient] eerst wordt gemaakt:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

Vervolgens wordt een reeks berekeningscluster knooppunten gemaakt in de Batch-account via een oproep naar `CreatePoolAsync`. `CreatePoolAsync`Gebruik de [BatchClient.PoolOperations.CreatePool] [ net_pool_create] methode daadwerkelijk een resourcegroep maken in de Batch-service.

```csharp
private static async Task CreatePoolAsync(
    BatchClient batchClient,
    string poolId,
    IList<ResourceFile> resourceFiles)
{
    Console.WriteLine("Creating pool [{0}]...", poolId);

    // Create the unbound pool. Until we call CloudPool.Commit() or CommitAsync(),
    // no pool is actually created in the Batch service. This CloudPool instance is
    // therefore considered "unbound," and we can modify its properties.
    CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicated: 3,           // 3 compute nodes
            virtualMachineSize: "small",  // single-core, 1.75 GB memory, 224 GB disk
            cloudServiceConfiguration:
                new CloudServiceConfiguration(osFamily: "4")); // Win Server 2012 R2

    // Create and assign the StartTask that will be executed when compute nodes join
    // the pool. In this case, we copy the StartTask's resource files (that will be
    // automatically downloaded to the node by the StartTask) into the shared
    // directory that all tasks will have access to.
    pool.StartTask = new StartTask
    {
        // Specify a command line for the StartTask that copies the task application
        // files to the node's shared directory. Every compute node in a Batch pool
        // is configured with several pre-defined environment variables that you can
        // reference by using commands or applications run by tasks.

        // Since a successful execution of robocopy can return a non-zero exit code
        // (e.g. 1 when one or more files were successfully copied) we need to
        // manually exit with a 0 for Batch to recognize StartTask execution success.
        CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
        ResourceFiles = resourceFiles,
        WaitForSuccess = true
    };

    await pool.CommitAsync();
}
```

Als u een groep maakt met [CreatePool][net_pool_create], u verschillende parameters, zoals het aantal berekeningscluster knooppunten, de [grootte van de knooppunten](../cloud-services/cloud-services-sizes-specs.md)en besturingssysteem van de knooppunten opgeeft. In *DotNetTutorial*, gebruiken we [CloudServiceConfiguration] [ net_cloudserviceconfiguration] om op te geven van Windows Server 2012 R2 van [Cloudservices](../cloud-services/cloud-services-guestos-update-matrix.md). Echter door het opgeven van een [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] in plaats daarvan kunt u toepassingen van knooppunten gemaakt op basis van Marketplace afbeeldingen, waaronder zowel Windows en Linux afbeeldingen, Zie [inrichten Linux berekenen knooppunten in Azure Batch groepen](batch-linux-nodes.md) voor meer informatie.

> [AZURE.IMPORTANT] U wordt geheven voor berekeningscluster resources in de Batch. Als u wilt minimaliseren kosten, kunt u verlagen `targetDedicated` 1 voordat u de steekproef uitvoert.

Samen met de eigenschappen van deze fysiek knooppunt, kunt u ook een [starttaak] opgeven[ net_pool_starttask] voor de toepassingen. De starttaak wordt uitgevoerd op elk knooppunt zoals u dat knooppunt deelneemt aan de groep en telkens wanneer een knooppunt opnieuw is opgestart. De starttaak is vooral handig voor het installeren van toepassingen op berekeningscluster knooppunten voordat het uitvoeren van taken. Bijvoorbeeld: als uw taken proces gegevens met behulp van Python scripts, u kunt een starttaak Python installeren op de knooppunten berekeningscluster.

In dit voorbeeldtoepassing, de starttaak kopieert de bestanden die het downloaden van opslagruimte (die zijn opgegeven met behulp van de [starttaak][net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] eigenschap) van de starttaak-werkmap met de gedeelde map die voor *alle* taken op het knooppunt toegankelijk. In feite Hierdoor wordt gekopieerd `TaskApplication.exe` en bijbehorende afhankelijkheden met de gedeelde map op elk knooppunt terwijl het knooppunt deelneemt aan de groep, zodat alle taken die worden uitgevoerd op het knooppunt er toegang toe.

> [AZURE.TIP] De functie **toepassingspakketten** van Azure Batch biedt een andere manier om uw toepassing op de berekeningscluster knooppunten in een groep. Zie de [toepassing-implementatie waarbij Azure Batch toepassing-pakketten](batch-application-packages.md) voor meer informatie.

Let op in de bovenstaande codefragment is ook het gebruik van twee omgevingsvariabelen in de eigenschap *opdrachtregel* van de starttaak: `%AZ_BATCH_TASK_WORKING_DIR%` en `%AZ_BATCH_NODE_SHARED_DIR%`. Elk berekeningscluster knooppunt in een groep Batch wordt automatisch geconfigureerd met meerdere variabelen bevatten omgeving die specifiek voor de Batch zijn. Een proces dat wordt uitgevoerd door een taak heeft toegang tot deze omgevingsvariabelen.

> [AZURE.TIP] Meer informatie over de omgeving raadpleegt variabelen die beschikbaar op berekeningscluster knooppunten in een Batch van toepassingen en informatie over mappen van taak werken zijn, u de secties [omgevingsinstellingen voor taken](batch-api-basics.md#environment-settings-for-tasks) en [bestanden en mappen](batch-api-basics.md#files-and-directories) in het [overzicht van de functie Batch voor ontwikkelaars](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Stap 4: Batchtaak maken

![Batchtaak maken][4]<br/>

Een Batch **taak** is een verzameling taken en is gekoppeld aan een groep berekeningscluster knooppunten. De taken in een taak uitvoeren op de bijbehorende toepassingen berekeningscluster knooppunten.

U kunt een taak niet alleen voor het organiseren en bijhouden van taken in gerelateerde werkbelasting, maar ook voor de toepassing van bepaalde beperkingen, zoals de maximale runtime voor de taak (en dus ook zijn taken) en de prioriteit van de taak ten opzichte van andere taken in de Batch-account. In dit voorbeeld is de taak echter alleen gekoppeld aan de toepassingen die is gemaakt in stap #3. Geen extra eigenschappen zijn geconfigureerd.

Alle taken zijn gekoppeld aan een specifieke groep. Deze koppeling wordt aangegeven welke taken van de taak wordt uitgevoerd op knooppunten. U dit opgeven met behulp van de [CloudJob.PoolInformation] [ net_job_poolinfo] eigenschap, zoals in het onderstaande codefragment.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

Nu dat een taak is gemaakt, worden taken worden toegevoegd aan het werk uitvoeren.

## <a name="step-5-add-tasks-to-job"></a>Stap 5: Taken toevoegen aan de taak

![Taken toevoegen aan een taak][5]<br/>
*(1) taken worden toegevoegd aan de taak, (2) de taken worden gepland om uit te voeren op knooppunten en (3) de taken de gegevensbestanden verwerkingstijd downloaden*

Batch **taken** zijn de afzonderlijke eenheden van werk die op de knooppunten berekeningscluster uitvoeren. Een taak heeft een opdrachtregel en wordt de scripts of uitvoerbare bestanden die u in die opdrachtregel opgeeft uitgevoerd.

Als u wilt dat is wel inzetten, moeten de taken worden toegevoegd aan een taak. Elke [CloudTask] [ net_task] is geconfigureerd met behulp van een eigenschap voor de opdrachtregel en [ResourceFiles] [ net_task_resourcefiles] (zoals met van de groep starttaak) die de taak die is gedownload naar het knooppunt voordat de opdrachtregel automatisch wordt uitgevoerd. Elke taak verwerkt in het project van de steekproef *DotNetTutorial* slechts één bestand. De collectie ResourceFiles bevat dus één element.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks to job [{1}]...", inputFiles.Count, jobId);

    // Create a collection to hold the tasks that we'll be adding to the job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of the tasks. Because we copied the task application to the
    // node's shared directory with the pool's StartTask, we can access it via
    // the shared directory on the node that the task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add the tasks as a collection, as opposed to issuing a separate AddTask call
    // for each. Bulk task submission helps to ensure efficient underlying API calls
    // to the Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [AZURE.IMPORTANT] Wanneer ze toegang tot omgevingsvariabelen, zoals `%AZ_BATCH_NODE_SHARED_DIR%` of een toepassing niet gevonden in van het knooppunt uitvoeren `PATH`, taak opdracht lijnen moeten worden voorafgegaan door `cmd /c`. Hiermee wordt expliciet het opdrachtverwerkingsprogramma uitvoeren en geef de instructie om deze te beëindigen na het uitvoeren van de opdracht. Deze vereiste geldt onnodige als uw taken een toepassing in van het knooppunt uitvoert `PATH` (zoals *robocopy.exe* of *powershell.exe*) en er geen omgevingsvariabelen worden gebruikt.

Binnen de `foreach` herhalen in het bovenstaande codefragment, kunt u zien dat de opdrachtregel voor de taak wordt opgesteld zodat drie argumenten voor de opdrachtregel worden doorgegeven aan *TaskApplication.exe*:

1. Het **eerste argument** is het pad van het bestand te verwerken. Dit is het lokale pad naar het bestand op het knooppunt. Wanneer de ResourceFile object in `UploadFileToContainerAsync` is gemaakt, de bestandsnaam is gebruikt voor deze eigenschap (als een parameter voor de constructor ResourceFile). Hiermee wordt aangeduid dat het bestand in dezelfde map als *TaskApplication.exe*kan worden gevonden.

2. Het **tweede argument** wordt aangegeven dat de bovenste *N* woorden naar het uitvoerbestand moeten worden opgenomen. In de steekproef, is dit hard gecodeerde zodat de bovenste drie woorden worden geschreven naar het uitvoerbestand.

3. Het **derde argument** is de gedeelde access-handtekening (SA's) die schrijftoegang aan de container **uitvoer** in Azure opslagruimte biedt. *TaskApplication.exe* wordt deze URL van de handtekening gedeelde access gebruikt wanneer deze het uitvoerbestand naar de opslag van Azure uploadt. U vindt de code voor deze in de `UploadFileToContainer` methode in van het project TaskApplication `Program.cs` bestand:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload the file (as a new blob) to the container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath, FileMode.Open);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when the Batch service
                // sets the CloudTask.ExecutionInformation.ExitCode for the task that
                // executed this application, it properly indicates that there was a
                // problem with the task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>Stap 6: Monitortaken

![Monitortaken][6]<br/>
*De clienttoepassing (1) de taken voor voltooiingsgraad kent en success status bewaakt en (2) de taken upload resultaatgegevens met Azure Storage*

Als taken worden toegevoegd aan een taak, worden ze automatisch in de wachtrij en gepland voor uitvoering op berekeningscluster knooppunten binnen de toepassingen die is gekoppeld aan de taak. Batch op basis van de instellingen die u opgeeft, worden alle taak-queuing, planning, opnieuw en andere taak beheer van de taken voor u. Zijn er veel benaderingen monitoring uitvoering van taken. DotNetTutorial ziet u een eenvoudig voorbeeld Staten-alleen op de voltooiingsgraad kent en taak fout of voltooid rapporten.

Binnen de `MonitorTasks` methode in de DotNetTutorial `Program.cs`, er zijn drie Batch .NET-concepten die discussie garandeert. Ze worden vermeld onder in welke volgorde:

1. **ODATADetailLevel**: precisie [ODATADetailLevel] [ net_odatadetaillevel] in de lijst met bewerkingen (zoals het verkrijgen van een lijst met taken van een taak) essentiële om ervoor te zorgen Batch toepassingsprestaties is. [De Batch Azure-service efficiënt Query](batch-efficient-list-queries.md) aan uw lezen lijst toevoegen als u van plan bent een sorteren van status monitoring binnen uw toepassingen Batch doen.

2. **TaskStateMonitor**: [TaskStateMonitor] [ net_taskstatemonitor] biedt u Batch .NET-toepassingen met de helper hulpprogramma's voor het controleren van lidstaten van de taak. In `MonitorTasks`, *DotNetTutorial* wacht voor alle taken bereiken [TaskState.Completed] [ net_taskstate] binnen een termijn. Deze, beëindigt de taak.

3. **TerminateJobAsync**: een project met [JobOperations.TerminateJobAsync] beëindigen[ net_joboperations_terminatejob] (of de blokkering JobOperations.TerminateJob) die taak wordt gemarkeerd als voltooid. Moet u doen als uw oplossing Batch gebruikmaakt van een [JobReleaseTask][net_jobreltask]. Dit is een speciaal type taak, waarin wordt beschreven in [voorbereiding en de voltooiing van projecttaken](batch-job-prep-release.md).

De `MonitorTasks` methode uit *DotNetTutorial*van `Program.cs` wordt weergegeven onder:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed to reach the Completed state within the timeout period.";

    // Obtain the collection of tasks currently managed by the job. Note that we use
    // a detail level to  specify that only the "id" property of each task should be
    // populated. Using a detail level for all list operations helps to lower
    // response time from the Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor to monitor the state of our tasks. In this case, we
    // will wait for all tasks to reach the Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached the "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property to ensure that it did not encounter a scheduling error
    // or return a non-zero exit code.

    // Update the detail level to populate only the task id and executionInfo
    // properties. We refresh the tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate the task's properties with the latest info from the
        // Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.SchedulingError != null)
        {
            // A scheduling error indicates a problem starting the task on the node.
            // It is important to note that the task's state can be "Completed," yet
            // still have encountered a scheduling error.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a scheduling error: {1}",
                task.Id,
                task.ExecutionInformation.SchedulingError.Message);
        }
        else if (task.ExecutionInformation.ExitCode != 0)
        {
            // A non-zero exit code may indicate that the application executed by
            // the task encountered an error during execution. As not every
            // application returns non-zero on failure by default (e.g. robocopy),
            // your implementation of error checking may differ from this example.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within the specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>Stap 7: Download uitvoer van de taak

![Uitvoer van de taak downloaden uit de opslag][7]<br/>

Nu dat de taak is voltooid, kan de uitvoer van de taken van Azure Storage worden gedownload. Dit gebeurt via een oproep naar `DownloadBlobsFromContainerAsync` in *DotNetTutorial*van `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference to a previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all the block blobs in the specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference to the current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents to a file in the specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded to {0}", directoryPath);
}
```

> [AZURE.NOTE] De oproep door naar `DownloadBlobsFromContainerAsync` in de *DotNetTutorial* toepassing is aangegeven dat de bestanden moeten worden gedownload naar uw `%TEMP%` map. Je mag rustig deze uitvoerlocatie wijzigen.

## <a name="step-8-delete-containers"></a>Stap 8: Verwijderen containers

Omdat wordt geheven voor gegevens die zich in Azure opslag bevindt, maar het is altijd een goed idee om een BLOB's die niet meer nodig zijn voor uw taken Batch verwijderen. In de DotNetTutorial `Program.cs`, dit klaar is met drie aanroepen van de helpmethode `DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

De methode zelf alleen worden opgehaald een verwijzing naar de container, en wordt vervolgens [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Stap 9: De taak en de groep verwijderen

In de laatste stap, wordt de gebruiker gevraagd om de taak en de toepassingen die zijn gemaakt door de DotNetTutorial-toepassing te verwijderen. Hoewel u geen btw voor projecten en taken zelf, u *bent geheven* betalen voor berekeningscluster knooppunten. Dus is het raadzaam dat u knooppunten toewijst alleen naar wens. Niet-gebruikte toepassingen verwijderen kan deel uitmaken van het proces onderhoud.

Van de BatchClient [JobOperations] [ net_joboperations] en [PoolOperations] [ net_pooloperations] hebben beide overeenkomstige verwijdering methoden, die als de gebruiker verwijdering bevestigt worden genoemd:

```csharp
// Clean up the resources we've created in the Batch account if the user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [AZURE.IMPORTANT] Houd er rekening mee dat wordt geheven voor berekeningscluster resources, kosten ongebruikte toepassingen verwijderen te minimaliseren. Let op dat een resourcegroep die verwijdert alle berekeningscluster knooppunten in die groep en dat alle gegevens op de knooppunten kunnen niet worden teruggezet nadat de groep wordt verwijderd.

## <a name="run-the-dotnettutorial-sample"></a>De steekproef *DotNetTutorial* uitvoeren

Wanneer u de toepassing van de steekproef uitvoert, zijn de console-uitvoer ongeveer als volgt uit. Tijdens de uitvoering, ervaart u op een pauze `Awaiting task completion, timeout in 00:30:00...` terwijl van de groep berekeningscluster knooppunten worden gestart. Gebruik van de [Azure portal] [ azure_portal] om te controleren van uw groep, berekenen knooppunten, functie en taken tijdens en na worden uitgevoerd. Gebruik van de [Azure portal] [ azure_portal] of de [Azure opslag Explorer] [ storage_explorers] om weer te geven van de opslag-resources (containers en BLOB's) die zijn gemaakt door de toepassing.

Wanneer u de toepassing in de standaardconfiguratie uitvoert, is typische execution tijdstip **ongeveer 5 minuten** .

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe to container [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll to container [application]...
Uploading file ..\..\taskdata1.txt to container [input]...
Uploading file ..\..\taskdata2.txt to container [input]...
Uploading file ..\..\taskdata3.txt to container [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks to job [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Downloading all files from container [output]...
All files downloaded to C:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Volgende stappen

Je mag rustig wijzigt *DotNetTutorial* en *TaskApplication* om te experimenteren met verschillende berekeningscluster scenario's. Bijvoorbeeld, kunt u het toevoegen van een vertraging van de uitvoering naar *TaskApplication*, zoals met [Thread.Sleep][net_thread_sleep], om te simuleren langdurige taken en ze controleren in de portal. Probeer meer taken toe te voegen of het aantal knooppunten berekeningscluster aan te passen. Logica om te controleren en het gebruik van een bestaande toepassingen naar snelheid van tijd toestaan toevoegen (*Tip*: Bekijk `ArticleHelpers.cs` in de [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] project in de [voorbeelden van azure batch][github_samples]).

Nu u bekend bent met de eenvoudige werkstroom van een oplossing Batch, is het tijd om het verloop de extra functies van de Batch-service.

- Lees het [overzicht van de functie Batch voor ontwikkelaars](batch-api-basics.md), die voor alle nieuwe Batch gebruikers wordt aangeraden.
- Beginnen op de andere Batch ontwikkeling artikelen onder **ontwikkeling uitgebreidere** in de [Batch leerpad][batch_learning_path].
- Bekijk een andere implementatie van het verwerken van de werklast "bovenste N woorden" met behulp van de Batch in de [TopNWords] [ github_topnwords] voorbeeld.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Containers maken in Azure-opslag"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Toepassing voor de taak en invoer (gegevens) bestanden uploaden naar containers"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Batch toepassingen maken"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Batchtaak maken"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Taken toevoegen aan een taak"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Monitortaken"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Uitvoer van de taak downloaden uit de opslag"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Batch oplossing werkstroom (volledige diagram)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Batch referenties in de Portal"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Opslag van referenties in de Portal"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Batch oplossing werkstroom (minimale diagram)"
