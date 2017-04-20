<properties
    pageTitle="Project en uitvoer behouden blijven Azure Batch | Microsoft Azure"
    description="Informatie over het gebruiken van Azure-opslag, zoals een duurzame store voor uw batchtaak en de taak uitvoeren en schakel deze permanente uitvoer weergeven in de portal van Azure."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="persist-azure-batch-job-and-task-output"></a>Persistent Azure Batch project en uitvoer

De taken die u in de Batch meestal uitvoert produceren uitvoer die moet worden opgeslagen en vervolgens later worden opgehaald door andere taken in de taak, de clienttoepassing die de taak of beide uitgevoerd. Deze uitvoer mogelijk bestanden die zijn gemaakt door het verwerken van invoergegevens of de logboekbestanden die is gekoppeld aan een taak kan worden uitgevoerd. In dit artikel maakt u kennis met een .NET-klassenbibliotheek die gebruikmaakt van een methode conventies gebaseerde aanhouden van dergelijke uitvoer van de taak met Azure-blobopslag, waardoor het beschikbaar is zelfs nadat u uw groepen, taken, verwijderen en knooppunten berekenen.

Met behulp van de methode in dit artikel, kunt u ook de uitvoer van uw taak bekijken in **Saved uitvoerbestanden** en **Logboeken opgeslagen** in de [portal van Azure][portal].

![Saved uitvoerbestanden en opgeslagen logboeken selectors in de portal][1]

>[AZURE.NOTE] De [Azure Batch bestand conventies] [ nuget_package] .NET-klassenbibliotheek in dit artikel beschreven is momenteel in de proefversie. Enkele functies die hier worden beschreven, mogelijk wijzigen voordat u algemene beschikbaarheid.

## <a name="task-output-considerations"></a>Aandachtspunten voor de taak uitvoer

Wanneer u uw oplossing Batch ontwerpt, moet u rekening houden met verschillende factoren die zijn gerelateerd aan de uitvoer van taak en taak.

* **Knooppunt levensduur berekenen**: berekenen knooppunten zijn vaak tijdelijke, met name in toepassingen automatisch schalen is ingeschakeld. De uitvoer van de taken die worden uitgevoerd op een knooppunt zijn beschikbaar alleen terwijl het knooppunt bestaat, en alleen binnen het bestand bewaarbeleid tijd die u hebt ingesteld voor de taak. Om ervoor te zorgen dat de uitvoer van de taak blijft behouden, moeten uw taken daarom haar uitvoerbestanden uploaden naar een duurzame store, bijvoorbeeld Azure opslag.

* **Opslag van de uitvoer**: als u wilt blijven optreden uitvoergegevens van de taak naar duurzame opslag, kunt u de [SDK van Azure-opslag](../storage/storage-dotnet-how-to-use-blobs.md) in de taakcode van uw om de uitvoer van de taak te uploaden naar een Blob storage container. Als u een container en bestand naamgevingsconventie implementeert, kunnen de clienttoepassing of andere taken in de taak Zoek en vervolgens deze uitvoer op basis van de overeenkomst downloaden.

* **Uitvoer ophalen**: U kunt uitvoer van de taak ophalen rechtstreeks vanuit de knooppunten berekenen in uw groep of Azure opslag als uw taken zich blijft hun uitvoer voordoen. Als u wilt ophalen van een taak uitvoer rechtstreeks vanuit een berekeningscluster knooppunt, moet u de bestandsnaam en de uitvoerlocatie op het knooppunt. Als u uitvoer met Azure Storage zich blijft voordoen, moet volgende taken of de clienttoepassing het volledige pad naar het bestand in Azure-opslag om deze te downloaden via de SDK van Azure opslag.

* **Weergeven van de uitvoer**: wanneer u navigeren naar een batchtaak in de Azure-portal en selecteer **de bestanden op knooppunt**, alle bestanden die zijn gekoppeld aan de taak wordt weergegeven, niet alleen de uitvoerbestanden die waarin u bent geïnteresseerd. Klik nogmaals serverbestanden op berekeningscluster knooppunten beschikbaar alleen terwijl het knooppunt bestaat en alleen binnen het bestand bewaarbeleid tijd die u hebt ingesteld voor de taak. De uitvoer van de taak die u met Azure Storage hebt doorgevoerd in de portal of een toepassing zoals de [Azure opslag Explorer]weergeven[storage_explorer], moet u de locatie kent en Ga rechtstreeks naar het bestand.

## <a name="help-for-persisted-output"></a>Help voor permanente uitvoer

Als u meer eenvoudig persistent project en uitvoer, het team Batch heeft gedefinieerd en een reeks naamgevingsconventies, evenals een .NET-klassenbibliotheek, de [Azure Batch bestand conventies] geïmplementeerd[ nuget_package] bibliotheek, die u in uw Batch-toepassingen gebruiken kunt. Bovendien is de Azure-portal op de hoogte van deze naamgevingsconventies zodat u de bestanden die u hebt opgeslagen met behulp van de bibliotheek gemakkelijk kunt terugvinden.

## <a name="using-the-file-conventions-library"></a>Gebruik van de bibliotheek met bestand conventies

[Azure Batch bestand conventies] [ nuget_package] is een .NET-klassenbibliotheek waarmee uw Batch .NET-toepassingen kunnen eenvoudig opslaan en uitvoer van de taak en naar Azure opslagruimte op te halen. Dit is bedoeld voor gebruik in zowel de taak en de client-code--in taakcode voor het persistent maken van bestanden en in clientcode wilt weergeven en deze op te halen. Uw taken kunnen ook de bibliotheek gebruiken voor het ophalen van de uitvoer van boven taken, zoals in een scenario voor [taakafhankelijkheden](batch-task-dependencies.md) .

De bibliotheek conventies zorgt om ervoor te zorgen dat Opslagcontainers en taak uitvoer bestanden zijn benoemde op basis van de overeenkomst en zijn geüpload naar de juiste plaats verzonden wanneer behouden is gebleven met Azure Storage. Wanneer u de uitvoer van ophaalt, kunt u de uitvoer van voor een bepaalde taak of een taak eenvoudig vinden door de vermelding of ophalen van de uitvoer van op ID en doel, in plaats van dat hoeft te weten bestandsnamen of waar deze zich bevindt in opslag.

Bijvoorbeeld, kunt u de bibliotheek aan de "een lijst met alle tussenliggende bestanden voor de taak 7" of "Haal mij de miniatuur voor de taak *MyMovie voor linkageName*," zonder dat de bestandsnamen of locatie in uw account opslag informatie nodig hebt.

### <a name="get-the-library"></a>De bibliotheek ophalen

Kunt u de bibliotheek, die bevat nieuwe klassen en breidt de [CloudJob] [ net_cloudjob] en [CloudTask] [ net_cloudtask] klassen met nieuwe methoden, uit [NuGet][nuget_package]. Kunt u deze toevoegen aan uw Visual Studio-project met de [NuGet bibliotheek Package Manager][nuget_manager].

>[AZURE.TIP] U vindt de [broncode] [ github_file_conventions] voor de bibliotheek Azure Batch bestand conventies op GitHub in Microsoft Azure SDK voor .NET-bibliotheek.

## <a name="requirement-linked-storage-account"></a>Vereiste: gekoppelde opslag-account

Uitvoer van duurzame opslagruimte met behulp van het bestand conventies bibliotheek opslaan en deze weergeven in de portal van Azure, moet u [koppeling een Azure Storage-account](batch-application-packages.md#link-a-storage-account) bij uw account Batch. Als u nog niet is gedaan, een opslagruimte account koppelen aan uw account Batch met behulp van de Azure-portal:

Blade **batch account** > **Instellingen** > **Opslag Account** > **Opslag-Account** (geen) > een opslag selecteren in uw abonnement

Zie voor een gedetailleerde overzicht over het koppelen van een account opslag, [toepassing-implementatie waarbij Azure Batch toepassing-pakketten](batch-application-packages.md).

## <a name="persist-output"></a>Persistent uitvoer

Er zijn twee primaire acties uitvoeren bij opslaan van project en uitvoer met de bibliotheek met bestand conventies: de container opslag maken en opslaan van uitvoer aan de container.

>[AZURE.WARNING] Omdat alle uitvoer van project en worden opgeslagen in dezelfde container, kunnen [opslag beperken limieten](../storage/storage-performance-checklist.md#blobs) worden toegepast als een groot aantal taken probeert om bestanden op hetzelfde moment.

### <a name="create-storage-container"></a>Opslag container maken

Uw taken voordat persistent maken uitvoer naar opslag, moet u een blob storage container waaraan ze hun uitvoer uploadt. Dit doen door de ondersteuning voor [CloudJob][net_cloudjob]. [PrepareOutputStorageAsync] [net_prepareoutputasync]. Deze methode extensie heeft een [CloudStorageAccount] [ net_cloudstorageaccount] object als een parameter en maakt u een container met de naam zo dat de inhoud ervan kunnen gevonden worden door de Azure-portal en de methoden voor het ophalen later in het artikel.

Meestal plaatst u deze code in de clienttoepassing--de toepassing die wordt gemaakt van uw groepen, taken en taken.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Uitvoer van een taak opslaan

Nu dat u hebt een container in blobopslag voorbereid, taken uitvoer aan de container kunnen opslaan met behulp van de [TaskOutputStorage] [ net_taskoutputstorage] klasse gevonden in de bibliotheek met bestand conventies.

In de taakcode van uw, moet u eerst een [TaskOutputStorage] maken[ net_taskoutputstorage] object en klik wanneer de taak is voltooid zijn werk, belt u het [TaskOutputStorage][net_taskoutputstorage]. [SaveAsync] [net_saveasync] methode om op te slaan de uitvoer met Azure Storage.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

De parameter 'uitvoer type' de permanente bestanden wordt gecategoriseerd. Er zijn vier vooraf gedefinieerde [TaskOutputKind] [ net_taskoutputkind] typen: "TaskOutput", "TaskPreview", "TaskLog" en "TaskIntermediate." U kunt ook aangepaste typen definiëren als ze nuttig zijn in uw werkstroom kunnen.

Dit soort uitvoer kunnen u opgeven welk type uitvoer van voor een overzicht van wanneer u later Batch de permanente uitvoer van een bepaalde taak zoeken. Wanneer u de uitvoer van voor een taak, kunt u met andere woorden, de lijst op een van de typen uitvoer filteren. Bijvoorbeeld: "geven mij de *preview* -uitvoer voor taak *109*." Meer op de vermelding en het terughalen van uitvoer wordt weergegeven in [uitvoer ophalen](#retrieve-output) later in dit artikel.

>[AZURE.TIP] Geeft het type uitvoer ook waar in de portal van Azure een bepaald bestand wordt weergegeven: *TaskOutput*-gecategoriseerde bestanden worden weergegeven in "Taak uitvoerbestanden" en *TaskLog* bestanden worden weergegeven in "Logboeken aan de taak."

### <a name="store-job-outputs"></a>Uitvoer van een taak opslaan

Naast de uitvoer van een taak op te slaan, kunt u de uitvoer van die is gekoppeld aan een volledig project opslaan. In de taak samenvoegen van een taak van de weergave film kan u bijvoorbeeld de volledig weergegeven film behouden als de uitvoer van een taak. Wanneer uw project is voltooid, de clienttoepassing gewoon kunt lijst en de uitvoer van voor de taak, ophalen en hoeft niet om query's in de individuele taken.

Resultaat van de taak opslaan door de ondersteuning voor de [JobOutputStorage][net_joboutputstorage]. [SaveAsync] [net_joboutputstorage_saveasync] methode, en geef de [JobOutputKind] [ net_joboutputkind] en de bestandsnaam:

```
CloudJob job = await batchClient.JobOperations.GetJobAsync(jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Terwijl met TaskOutputKind voor uitvoer van een taak, u de [JobOutputKind gebruikt] [ net_joboutputkind] -parameter voor het categoriseren van een taak bevindt zich behouden gebleven bestanden. Deze parameter kunt u later query voor (lijst) een bepaalde soort uitvoer. De JobOutputKind bevat zowel uitvoer als preview typen en ondersteunt het maken van aangepaste typen.

### <a name="store-task-logs"></a>Taak Logboeken opslaan

Naast het persistent maken een bestand met duurzame storage wanneer een taak of een taak is voltooid, vindt u het mogelijk nodig om bestanden die tijdens de uitvoering van een taak--logboekbestanden worden bijgewerkt of `stdout.txt` en `stderr.txt`, bijvoorbeeld. Daartoe de bibliotheek Azure Batch bestand conventies biedt de [TaskOutputStorage][net_taskoutputstorage]. [SaveTrackedAsync] [net_savetrackedasync] methode. Met [SaveTrackedAsync][net_savetrackedasync], kunt u updates naar een bestand op het knooppunt (met een tijdsinterval die u opgeeft) bijhouden en updates met Azure Storage behouden.

In het volgende codefragment, gebruiken we [SaveTrackedAsync] [ net_savetrackedasync] bijwerken `stdout.txt` in Azure opslag elke 15 seconden tijdens de uitvoering van de taak:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
    await Task.Delay(stdoutFlushDelay);
}
```

`Code to process data and produce output file(s)`is gewoon een tijdelijke aanduiding voor de code die uw taak normaal kunt uitvoeren. Bijvoorbeeld wellicht code die de gegevens van Azure Storage-downloads en -transformatie of berekening uitgevoerd op is geïnstalleerd. Het belangrijkste gedeelte van dit fragment laat zien hoe hoe u kunt laten teruglopen zoals-code in een `using` blokkeren als u wilt een bestand regelmatig bijwerken met [SaveTrackedAsync][net_savetrackedasync].

De `Task.Delay` is vereist aan het einde van dit `using` blokkeren om ervoor te zorgen dat de knooppunt-agent tijd heeft legen van de inhoud van de standaard-out naar het bestand stdout.txt op het knooppunt (het knooppunt-agent is een programma dat wordt uitgevoerd op elk knooppunt in de groep en de opdracht-en-besturingselementen-interface tussen het knooppunt en de Batch-service). Zonder deze vertraging is het mogelijk te mist het laatste enkele seconden van uitvoer. Deze vertraging is mogelijk niet vereist voor alle bestanden.

>[AZURE.NOTE] Wanneer u een bestand bijhouden met SaveTrackedAsync inschakelt, worden alleen worden *toegevoegd* aan het bestand dat bijgehouden met Azure Storage doorgevoerd. Gebruik deze methode alleen voor het bijhouden van de logboekbestanden niet draaien of andere bestanden die worden toegevoegd aan, dat wil zeggen, gegevens wordt alleen toegevoegd aan het einde van het bestand wordt bijgewerkt.

## <a name="retrieve-output"></a>Uitvoer ophalen

Wanneer u uw vastgelegde uitvoer op basis van de bibliotheek Azure Batch bestand conventies ophaalt, kunt u dat doen in een taak en taak-centraal wijze. U kunt de uitvoer aanvragen voor gegeven taak of project zonder hoeft te weten het pad in blob Storage of zelfs de naam van het bestand. U kunt eenvoudig zegt "Geven mij de uitvoerbestanden voor de taak *109*."

Het volgende codefragment doorlopen alle taken van een taak, wordt afgedrukt vindt u informatie over de uitvoerbestanden voor de taak en klikt u vervolgens de bestanden uit de opslag-downloads.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (TaskOutputStorage output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="task-outputs-and-the-azure-portal"></a>Uitvoer van de taak en de Azure-portal

De Azure portal logboeken die worden doorgevoerd en uitvoer van de taak wordt weergegeven met een gekoppelde opslag van Azure-account met de naamgevingsconventies zijn gevonden in de [Azure Batch bestand conventies Leesmij][github_file_conventions_readme]. U kunt ook zelf deze conventies implementeren in een taal van uw keuze of u kunt de bibliotheek met bestand conventies gebruiken in uw .NET-toepassingen.

### <a name="enable-portal-display"></a>Inschakelen van de portal weergeven

Als u wilt de weergave van uw uitvoer van in de portal inschakelen, moet u de volgende vereisten voldoen:

 1. [Koppeling een Azure Storage-account](#requirement-linked-storage-account) bij uw account Batch.
 2. Voldoen aan de vooraf gedefinieerde naamgeving van Opslagcontainers en bestanden wanneer persistent uitvoer te maken. U kunt de definitie van deze conventies vinden in de bibliotheek in bestand conventies [Leesmij][github_file_conventions_readme]. Als u de [Azure Batch bestand conventies] [ nuget_package] bibliotheek aanhouden van uw uitvoer, deze voorwaarde is voldaan.

### <a name="view-outputs-in-the-portal"></a>De uitvoer van de weergave in de portal

Navigeer naar de taak waarvan de uitvoer u geïnteresseerd bent in en klik op **Saved uitvoerbestanden** of **opgeslagen logboeken**om weer te geven logboeken en uitvoer van de taak in de portal van Azure. In deze afbeelding ziet u de **opgeslagen uitvoerbestanden** voor de taak met ID '007':

![Taak uitvoer blade in de portal van Azure][2]

## <a name="code-sample"></a>Codevoorbeeld

De [PersistOutputs] [ github_persistoutputs] steekproef project is een van de [voorbeelden van Azure Batch code] [ github_samples] op GitHub. Deze oplossing Visual Studio-2015 laat zien hoe de bibliotheek Azure Batch bestand conventies gebruiken om de uitvoer van de taak naar duurzame opslag. Als u wilt uitvoeren in het voorbeeld, als volgt te werk:

1. Open het project in **Visual Studio-2015**.
2. Uw Batch en opslagruimte **accountreferenties** toevoegen aan **AccountSettings.settings** in het project Microsoft.Azure.Batch.Samples.Common.
3. **Maken** (maar worden niet uitgevoerd) de oplossing. Alle NuGet-pakketten herstellen wanneer hierom wordt gevraagd.
4. Gebruik de Azure-portal voor het uploaden van een [toepassingspakket](batch-application-packages.md) voor **PersistOutputsTask**. Zijn de `PersistOutputsTask.exe` en de afhankelijke stroombaan in het ZIP-pakket, de toepassings-ID aan 'PersistOutputsTask' en de versie van de toepassing-pakket naar "1.0" instellen.
5. **Starten** project (klaar) de **PersistOutputs** .

## <a name="next-steps"></a>Volgende stappen

### <a name="application-deployment"></a>Implementatie van toepassingen

De functie [toepassingspakketten](batch-application-packages.md) van Batch biedt een eenvoudige manier om beide te implementeren en de toepassingen die uw taken uitvoeren op berekenen knooppunten-versie.

### <a name="installing-applications-and-staging-data"></a>Installeren van toepassingen en tijdelijke gegevens

Bekijk de [installeren toepassingen en tijdelijk opslaan van de gegevens op Batch berekenen knooppunten] [ forum_post] posten in het forum Azure Batch voor een overzicht van de verschillende methoden voor het voorbereiden van uw knooppunten voor het uitvoeren van taken. Dit bericht is geschreven door een van de Azure Batch teamleden, een goede handleiding op de verschillende manieren om bestanden (inclusief zowel toepassingen als taak invoergegevens) naar uw knooppunten berekenen en enkele aandachtspunten voor elke methode.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Saved uitvoerbestanden en opgeslagen logboeken selectors in de portal"
[2]: ./media/batch-task-output/task-output-02.png "Taak uitvoer blade in de portal van Azure"