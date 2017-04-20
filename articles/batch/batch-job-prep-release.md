<properties
    pageTitle="Voorbereiding van en opruimen in Batch | Microsoft Azure"
    description="Gebruik voorbereiding van de taak op gebruikersniveau taken te minimaliseren ten gegevens overgebracht naar Azure Batch knooppunten berekenen en laat taken voor knooppunt opruimen bij Taakvoltooiing."
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
    ms.date="09/16/2016"
    ms.author="marsma" />

# <a name="run-job-preparation-and-completion-tasks-on-azure-batch-compute-nodes"></a>Voorbereiding van en de voltooiing van projecttaken worden uitgevoerd op Azure Batch berekeningscluster knooppunten

 Een taak Azure Batch is vaak vereist voor sommige formulier van setup, voordat de taken worden uitgevoerd en onderhoud na taak als de taken zijn voltooid. Mogelijk moet u veelvoorkomende taken invoergegevens downloaden naar uw knooppunten berekenen of taak uitvoergegevens uploaden naar Azure Storage nadat de taak is voltooid. **Voorbereiding van de taak** en **taak vrijgeven** taken kunt u deze bewerkingen uitvoeren.

## <a name="what-are-job-preparation-and-release-tasks"></a>Wat zijn de voorbereiding van de taak en de toets los taken?

Voordat u de taken van een taak uitvoert, wordt de voorbereidende projecttaak uitgevoerd op alle berekeningscluster knooppunten de planning moet ten minste één taak uitvoeren. Wanneer de taak is voltooid, wordt de release projecttaak uitgevoerd op elk knooppunt in de toepassingen die ten minste één taak uitgevoerd. Als u met de normale batchtaken, kunt u een opdrachtregel moet worden aangeroepen wanneer een voorbereidende of release van een taak wordt uitgevoerd.

Projecttaken voorbereiden en release bieden vertrouwde Batch taak-functies zoals bestandsoverdracht ([bronbestanden][net_job_prep_resourcefiles]), verhoogde worden uitgevoerd, aangepaste omgevingsvariabelen, maximum execution duur, aantal nieuwe pogingen en bestand bewaarbeleid tijd.

In de volgende secties leert u hoe u de [JobPreparationTask] [ net_job_prep] en [JobReleaseTask] [ net_job_release] klassen gevonden in de [Batch.NET] [ api_net] bibliotheek.

> [AZURE.TIP] Projecttaken voorbereiden en release zijn met name handig in "gedeelde toepassingen" omgevingen, waarin een groep berekeningscluster knooppunten zich blijft voordoen tussen taak wordt uitgevoerd en wordt gebruikt door te veel taken.

## <a name="when-to-use-job-preparation-and-release-tasks"></a>Wanneer u gebruik voorbereiding van de taak en vrijgeven van taken

Voorbereiding van de taak en projecttaken release zijn een goede geschikte optie voor de volgende situaties:

**Algemene taakgegevens downloaden**

Een gemeenschappelijke set met gegevens vereist batch functies vaak als invoer voor taken van de taak. Bijvoorbeeld in berekeningen voor gegevensanalyse met dagelijkse risico is marktgegevens de taak / regiospecifieke nog beschikbaar is in alle taken in de taak. Deze marktgegevens, vaak verscheidene gigabytes groot, moet worden gedownload voor elk knooppunt berekeningscluster slechts één keer zodat elke taak die wordt uitgevoerd op het knooppunt deze kunt gebruiken. Gebruik een **projecttaak voorbereiding van** deze gegevens downloaden voor elk knooppunt voordat het uitvoeren van de taak bevindt zich andere taken.

**Uitvoer van project en verwijderen**

In een omgeving 'gedeelde toepassingen', waar van een groep berekeningscluster knooppunten niet bedrijf tussen taken zijn, moet u mogelijk verwijderen taakgegevens tussen wordt uitgevoerd. Mogelijk moet u beschikbare schijfruimte op de knooppunten of voldoen aan beveiligingsbeleid voor apparaten van uw organisatie. Gebruik een **projecttaak release** om gegevens die is gedownload door een projecttaak voorbereidende of gegenereerd tijdens de uitvoering van taken te verwijderen.

**Log bewaarbeleid**

Mogelijk wilt u een kopie van de logboekbestanden die uw taken genereren, of misschien vastlopen bestanden die kunnen worden gegenereerd door mislukte toepassingen. Een **taak van taak vrijgeven** in dat geval gebruiken voor het comprimeren en deze gegevens uploaden naar een [Azure Storage] [ azure_storage] account.

>[AZURE.TIP] Een andere manier om te behouden logboeken en andere taak en de taak uitvoer van gegevens is via de bibliotheek [Azure Batch bestand conventies](batch-task-output.md) .

## <a name="job-preparation-task"></a>Voorbereiding van projecttaak

Voordat u de uitvoering van de taken van een taak uitvoeren Batch de voorbereidende projecttaak op elk berekeningscluster knooppunt waarop een taak is gepland. Standaard wacht de Batch-service voor de taak voorbereiding van taak moet zijn voltooid voordat u de taken die zijn uitgevoerd bij het knooppunt. U kunt echter de service niet te wachten configureren. Als het knooppunt opnieuw is opgestart, de voorbereidende projecttaak opnieuw wordt uitgevoerd, maar u kunt dit gedrag ook uitschakelen.

De voorbereiding van projecttaak wordt alleen op knooppunten die zijn gepland voor een taak uitgevoerd. Hierdoor wordt de onnodige uitvoering van een taak voorbereiding van geval knooppunt niet een taak is toegewezen. Dit kan gebeuren wanneer het aantal taken voor een taak kleiner dan het aantal knooppunten in een groep is. Dit geldt ook wanneer [uitvoering van gelijktijdige taken](batch-parallel-node-tasks.md) is ingeschakeld, waarbij enkele knooppunten niet actief is als het aantal taken lager dan het totaal aantal mogelijke gelijktijdige taken is. U kunt door de voorbereidende projecttaak niet op inactief knooppunten uit te voeren, minder geld besteden aan datakosten doorverbinden.

> [AZURE.NOTE] [JobPreparationTask] [ net_job_prep_cloudjob] verschilt van [CloudPool.StartTask] [ pool_starttask] in dat JobPreparationTask wordt uitgevoerd aan het begin van elke taak, terwijl starttaak wordt uitgevoerd alleen wanneer een berekeningscluster knooppunt eerst deelneemt aan een groep of opnieuw opstarten.

## <a name="job-release-task"></a>Release projecttaak

Nadat u een taak is gemarkeerd als voltooid, wordt de release projecttaak wordt uitgevoerd op elk knooppunt in de toepassingen die ten minste één taak uitgevoerd. U kunt een taak markeren als voltooid een beëindigen verzoek verzenden. De Batch-service vervolgens Hiermee stelt u de taakstatus op *beëindigen*, geen actief of actieve taken die zijn gekoppeld aan de taak beëindigd en wordt uitgevoerd de release projecttaak. De taak vervolgens wordt verplaatst naar de status *voltooid* .

> [AZURE.NOTE] Verwijdering van de taak wordt ook uitgevoerd voor de release projecttaak. Echter als een taak al is beëindigd, wordt de taak vrijgeven niet uitgevoerd een tweede maal als de taak later wordt verwijderd.

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Taak prep en de toets los taken met Batch .NET

Als u wilt gebruiken een voorbereidende projecttaak, toewijzen een [JobPreparationTask] [ net_job_prep] object naar van uw taak [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] eigenschap. Op dezelfde manier geïnitialiseerd een [JobReleaseTask] [ net_job_release] en toewijzen aan van uw taak [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] eigenschap voor het instellen van de taak vrijgeven van de taak.

In dit codefragment `myBatchClient` is een exemplaar van [BatchClient][net_batch_client], en `myPool` is van een bestaande toepassingen binnen de Batch-account.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Zoals eerder is vermeld, wordt de taak vrijgeven wordt uitgevoerd wanneer een taak is afgesloten of verwijderd. Een taak met [JobOperations.TerminateJobAsync]beëindigen[net_job_terminate]. Verwijderen van een project met [JobOperations.DeleteJobAsync][net_job_delete]. U meestal beëindigen of een taak verwijderen wanneer de taken worden voltooid, of wanneer een time-out die u hebt gedefinieerd is bereikt.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Voorbeeld van de code op GitHub

Om te zien voorbereiding van de taak en vrijgeven van taken in actie, raadpleegt u de [JobPrepRelease] [ job_prep_release_sample] steekproef project op GitHub. Deze consoletoepassing gebeurt het volgende:

1. Hiermee maakt u een groep met twee "kleine" knooppunten.
2. Hiermee maakt een taak met voorbereiding van de taak, release en standaard taken.
3. De voorbereiding van projecttaak, waarin u schrijft eerst de knooppunt-ID naar een tekstbestand in van het knooppunt 'gedeelde' map is uitgevoerd.
4. Een taak op elk knooppunt die naar het tekstbestand met dezelfde, de taak-ID schrijft wordt uitgevoerd.
5. Zodra alle taken worden voltooid (of de time-out is bereikt), wordt u de inhoud van elk knooppunt van tekstbestand dat u wilt de console afgedrukt.
6. Wanneer de taak is voltooid, moet u de taak release taak om het bestand verwijderen uit het knooppunt wordt uitgevoerd.
6. De afsluitcodes van de projecttaken voor voorbereiden en release voor elk knooppunt waarop ze uitgevoerd afdrukken
7. Pauzes uitvoering toe te staan dat bevestiging van taak en/of de groep verwijderen.

Uitvoer van de toepassing van de steekproef is vergelijkbaar met het volgende:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

>[AZURE.NOTE] Vanwege de variabele kunnen maken en start tijd van knooppunten in een nieuwe groep (enkele knooppunten gereed zijn voor taken voordat u anderen), ziet u mogelijk verschillende uitvoer. Specifiek, omdat de taken, snel voltooien, een van de groep van toepassingen knooppunten mogelijk uitgevoerd alle taken van de taak. Als dit gebeurt, ziet u dat de taak voorbereiden en release taken bestaan niet voor het knooppunt dat geen taken uitgevoerd.

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Voorbereiding van de taak en release taken in de portal van Azure controleren

Als u de toepassing van de steekproef uitvoert, kunt u de [portal van Azure] [ portal] naar de eigenschappen van de taak en de bijbehorende taken weergeven of zelfs downloaden van het gedeelde tekstbestand dat door de taken van de taak is gewijzigd.

De volgende schermafbeelding ziet u de **voorbereidende taken blade** in de portal van Azure na een uitvoeren van de steekproef-toepassing. Navigeer naar de eigenschappen van het *JobPrepReleaseSampleJob* nadat uw taken zijn voltooid (maar voordat uw taak en de groep wordt verwijderd) en klik **voorbereiding van taken** of **Release taken** om hun eigenschappen weer te geven.

![Voorbereiding van taakeigenschappen in Azure-portal][1]

## <a name="next-steps"></a>Volgende stappen

### <a name="application-packages"></a>Toepassingspakketten

U kunt ook de functie [toepassingspakketten](batch-application-packages.md) van Batch berekeningscluster knooppunten voorbereiden voor de uitvoering van taken naast de voorbereidende projecttaak gebruiken. Deze functie is vooral handig voor het implementeren van toepassingen die niet met een installatieprogramma, -toepassingen die veel (100-+) bestanden bevatten of toepassingen die opgevolgd strikte versiebeheer moeten is vereist.

### <a name="installing-applications-and-staging-data"></a>Installeren van toepassingen en tijdelijke gegevens

Dit bericht MSDN-forum biedt een overzicht van verschillende methoden voor het voorbereiden van uw knooppunten voor het uitvoeren van taken:

[Installeren van toepassingen en tijdelijke gegevens op Batch berekenen knooppunten][forum_post]

Geschreven door een van de Azure Batch teamleden, deze verschillende technieken die u gebruiken kunt om te implementeren toepassingen en gegevens als u wilt berekenen van knooppunten worden besproken.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
