<properties
    pageTitle="MPI toepassingen in Azure Batch uitvoeren met meerdere exemplaren taken | Microsoft Azure"
    description="Informatie over het gebruik van het taaktype meerdere exemplaren in Azure Batch bericht Passing Interface (MPI)-toepassingen uitvoeren."
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
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-azure-batch"></a>Gebruik van meerdere exemplaren taken bericht Passing Interface (MPI) toepassingen uitvoeren in Azure Batch

Meerdere exemplaren taken kunt u een taak Azure Batch op meerdere berekeningscluster knooppunten gelijktijdig. Deze taken inschakelen krachtige computing scenario's als bericht Passing Interface (MPI)-toepassingen in de Batch. In dit artikel leert u hoe u het uitvoeren van meerdere exemplaren taken met behulp van de [Batch.NET] [ api_net] bibliotheek.

>[AZURE.NOTE] Terwijl de voorbeelden in dit artikel op Batch .NET, MS-MPI richten, en Windows knooppunten berekenen, zijn de meerdere exemplaren taak concepten die in dit artikel van toepassing op andere platforms en -technologieën (Python en Intel MPI op Linux knooppunten, bijvoorbeeld).

## <a name="multi-instance-task-overview"></a>Meerdere exemplaren taakoverzicht

In Batch, elke taak normaal wordt uitgevoerd op een knooppunt één berekening--foutenrapporten meerdere taken aan een taak en elke taak voor uitvoering op een knooppunt worden gepland in de Batch-service. Echter door het configureren van een taak **meerdere exemplaren instellingen**, zien u Batch in plaats daarvan één primaire taak en meerdere subtaken die vervolgens worden uitgevoerd op meerdere knooppunten te maken.

![Meerdere exemplaren taakoverzicht][1]

Wanneer u een taak met meerdere exemplaren instellingen op een taak indient, voert Batch diverse stappen unieke meerdere exemplaren taken:

1. De Batch-service Hiermee maakt u één **primaire** en meerdere **subtaken** op basis van de instellingen voor meerdere exemplaren. Het totale aantal taken (primaire plus alle subtaken) overeenkomt met het aantal **exemplaren** (berekeningscluster knooppunten) u in de instellingen voor meerdere exemplaren opgeeft.
1. Batch wijst een van de knooppunten berekenen als de **basispagina**en laat u de primaire taak uitvoeren op de basispagina. Deze worden gepland in de subtaken uitvoeren op de rest van de knooppunten berekeningscluster is toegewezen aan de taak meerdere exemplaren, een subtaak per knooppunt.
1. De primaire en alle subtaken download **algemene resource-bestanden** die u in de instellingen voor meerdere exemplaren opgeeft.
1. Na de algemene resource bestanden zijn gedownload, de primaire en subtaken uitvoering van de **opdracht afhankelijk** dat u in de instellingen voor meerdere exemplaren opgeeft. De opdracht afhankelijk wordt meestal gebruikt voor het voorbereiden van knooppunten voor het uitvoeren van de taak. Dit zijn het starten van de Achtergrondservices (zoals [Microsoft MPI][msmpi_msdn]van `smpd.exe`) en bevestigt dat de knooppunten gereed is voor de verwerking tussen knooppunt berichten zijn.
1. De primaire taak voert de **toepassingsopdracht** op de basispagina knooppunt *nadat* die de opdracht afhankelijk is voltooid door de primaire en alle subtaken. De toepassingsopdracht is de opdrachtregel van de taak met meerdere exemplaren zelf, en alleen op basis van de belangrijkste taak wordt uitgevoerd. In een [MS-MPI][msmpi_msdn]-op basis van de oplossing dit is waar u het gebruik van uw MPI-toepassingen uitvoert `mpiexec.exe`.

> [AZURE.NOTE] Hoewel er functioneel distinct, de taak' meerdere exemplaren"is niet een unieke taaktype zoals de [starttaak] [ net_starttask] of [JobPreparationTask][net_jobprep]. De meerdere exemplaren taak daadwerkelijk gewoon een standaard Batch ([CloudTask] [ net_task] in Batch .NET) waarvan meerdere exemplaren-instellingen zijn geconfigureerd. In dit artikel wordt verwezen als volgt als de **taak met meerdere exemplaren**.

## <a name="requirements-for-multi-instance-tasks"></a>Vereisten voor meerdere exemplaren taken

Meerdere exemplaren taken vragen om een groep met **tussen knooppunt communicatie is ingeschakeld**en **uitgeschakeld van de uitvoering van gelijktijdige taken**. Als u probeert uit te voeren van een taak meerdere exemplaren in een groep met de communicatie tussen knooppunten is uitgeschakeld of als de taak is met een *maxTasksPerNode* waarde die groter is dan 1, nooit gepland--blijft voor onbepaalde tijd in de 'actieve' status. In dit codefragment toont het maken van dergelijke een resourcegroep die met de Batch .NET-bibliotheek.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicated: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

Daarnaast kunnen kunnen meerdere exemplaren taken uitvoeren *alleen* knooppunten in **toepassingen die zijn gemaakt nadat 14 December 2015**.

### <a name="use-a-starttask-to-install-mpi"></a>Gebruik een starttaak MPI installeren

Als u wilt uitvoeren MPI toepassingen met een taak meerdere exemplaren, moet u eerst een MPI-implementatie ([MS-MPI of Intel MPI, bijvoorbeeld) op de knooppunten berekenen in de groep installeren. Dit is een goed moment om te gebruiken van een [starttaak][net_starttask], die wordt uitgevoerd wanneer een knooppunt deelneemt aan een groep of opnieuw is opgestart. In dit codefragment Hiermee maakt u een starttaak waarmee het installatiepakket MS-MPI als een [bronbestand][net_resourcefile]. Opdrachtregel voor het van de begintaak wordt uitgevoerd wanneer het bronbestand wordt gedownload naar het knooppunt. In dit geval de opdrachtregel worden uitgevoerd zonder toezicht MS-MPI te installeren.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    RunElevated = true,
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Externe directe geheugen access (RDMA)

Wanneer u een [RDMA geschikte grootte](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) zoals A9 voor de knooppunten berekenen in uw groep Batch kiest, kunt uw MPI-toepassing profiteren van de Azure krachtige, lage latentie externe directe geheugen access (RDMA) netwerk.

Zoekt u de grootte die is opgegeven als 'RDMA kunnen' in de volgende artikelen:

* **CloudServiceConfiguration** van toepassingen

  * [Grootte voor Cloudservices](../cloud-services/cloud-services-sizes-specs.md) (Alleen Windows)

* **VirtualMachineConfiguration** van toepassingen

  * [Grootte voor virtuele machines in Azure wordt aangegeven](../virtual-machines/virtual-machines-linux-sizes.md) (Linux)

  * [Grootte voor virtuele machines in Azure wordt aangegeven](../virtual-machines/virtual-machines-windows-sizes.md) (Windows)

>[AZURE.NOTE] Als u wilt profiteren van RDMA op [Linux berekenen knooppunten](batch-linux-nodes.md), moet u **Intel MPI** op de knooppunten. Zie voor meer informatie over CloudServiceConfiguration en VirtualMachineConfiguration van toepassingen, de sectie [toepassingen](batch-api-basics.md#Pool) van het overzicht van de functie Batch.

## <a name="create-a-multi-instance-task-with-batch-net"></a>Een taak meerdere exemplaren met Batch .NET maken

Nu dat we de vereisten van toepassingen en MPI pakketinstallatie besproken hebben, laten we de taak meerdere exemplaren te maken. In dit fragment, maak een standaard [CloudTask][net_task], configureert u de [MultiInstanceSettings] [ net_multiinstance_prop] eigenschap. Zoals eerder is vermeld, is de taak meerdere exemplaren niet op een afzonderlijke taaktype, maar een standaard batchtaak dat is geconfigureerd met meerdere exemplaren-instellingen.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Primaire taak en subtaken

Als u de instellingen meerdere exemplaren van een taak maakt, geeft u het aantal berekeningscluster knooppunten die de taak uitvoeren. Wanneer u de taak aan een taak indient, geeft de Batch-service wordt gemaakt van één **primaire** taak en voldoende **subtaken** die elkaar overeenkomen met het aantal knooppunten die u hebt opgegeven.

Deze taken aan *numberOfInstances* - 1 een geheel getal-id in het bereik van 0 toegewezen. De taak-id waarmee 0 is de primaire taak en alle andere-id's zijn subtaken. Als u de volgende meerdere exemplaren-instellingen voor een taak maakt, de primaire taak moet een id van 0 en de subtaken moet id's 1 tot en met 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Basispagina knooppunt

Wanneer u een taak meerdere exemplaren indient, de Batch-service wordt een van de knooppunten berekenen als het knooppunt "basispagina" toegewezen en worden gepland in de primaire taak uitvoeren op de basispagina knooppunt. De subtaken zijn gepland uitvoeren op de rest van de knooppunten toegewezen aan de taak meerdere exemplaren.

## <a name="coordination-command"></a>De opdracht afhankelijk

De **opdracht afhankelijk** wordt uitgevoerd door zowel de primaire en subtaken.

De aanroep van de opdracht afhankelijk wordt geblokkeerd door--Batch niet de toepassingsopdracht wordt uitgevoerd totdat de opdracht afhankelijk is voor alle subtaken heeft geretourneerd. De opdracht afhankelijk moet daarom alle Achtergrondservices vereist starten, controleren of ze zijn gereed voor gebruik en sluit. Bijvoorbeeld: deze opdracht afhankelijk voor een oplossing met MS-MPI versie 7 start u de service SMPD op het knooppunt vervolgens afgesloten:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Houd rekening met het gebruik van `start` in deze opdracht afhankelijk. Dit is nodig omdat de `smpd.exe` toepassing niet direct na uitvoering weer. Zonder het gebruik van het [starten] [ cmd_start] opdracht, deze opdracht afhankelijk niet het resultaat en de toepassingsopdracht wordt uitgevoerd daarom wilt blokkeren.

## <a name="application-command"></a>Toepassingsopdracht

Als de primaire taak en alle subtaken hebt uitvoeren van de opdracht afhankelijk, wordt de opdrachtregel van de taak meerdere exemplaren uitgevoerd door de primaire taak *alleen*. Verwijst naar dit de **toepassingsopdracht** te onderscheiden van de opdracht afhankelijk.

Voor MS-MPI-toepassingen, gebruikt u de toepassingsopdracht uitvoeren van uw toepassing MPI ingeschakelde met `mpiexec.exe`. Hier is bijvoorbeeld een toepassingsopdracht voor een oplossing MS-MPI versie 7 met:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

>[AZURE.NOTE] Omdat MS-MPI `mpiexec.exe` gebruikt de `CCP_NODES` variabele door standaard (Zie [omgevingsvariabelen](#environment-variables)) in het voorbeeld toepassing opdrachtregel bovenstaande sluit deze.

## <a name="environment-variables"></a>Omgevingsvariabelen

Batch wordt gemaakt van verschillende [omgevingsvariabelen] [ msdn_env_var] specifiek zijn voor meerdere exemplaren taken op de knooppunten berekeningscluster is toegewezen aan een taak meerdere exemplaren. De lijnen van de opdracht afhankelijk en -toepassing kunt verwijzingen maken naar deze omgevingsvariabelen, zoals u kunnen de scripts en programma's die ze uitvoeren.

De volgende omgevingsvariabelen worden gemaakt door de service Batch voor gebruik door meerdere exemplaren taken:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Volledige Zie voor meer informatie over deze en de andere Batch berekeningscluster knooppunt omgevingsvariabelen, inclusief hun inhoud en de zichtbaarheid [berekenen knooppunt omgevingsvariabelen][msdn_env_var].

>[AZURE.TIP] In het voorbeeld Batch Linux MPI bevat een voorbeeld van hoe verscheidene van deze omgevingsvariabelen kunnen worden gebruikt. De [afhankelijk-cmd] [ coord_cmd_example] we vaker doen script algemene toepassing downloads en invoer bestanden van Azure-opslag, kunt u een share NFS Network File System () op de basispagina knooppunt en configureert u de andere knooppunten toegewezen aan de taak meerdere exemplaren als NFS-clients.

## <a name="resource-files"></a>Bronbestanden

Er zijn twee soorten bronbestanden aandachtspunten voor meerdere exemplaren taken: **algemene bronbestanden** dat *alle* taken downloaden (zowel primaire en subtaken), en de **bronbestanden** opgegeven voor het meerdere exemplaar taak zelf, welke taak *alleen de primaire* -downloads.

U kunt een of meer **algemene resource-bestanden** in de instellingen meerdere exemplaren van een taak opgeven. Deze algemene resource-bestanden zijn gedownload van [Azure opslag](./../storage/storage-introduction.md) van elk knooppunt **taak gedeelde map** op de primaire en alle subtaken. Kunt u de gedeelde map taak openen vanaf regels-toepassing en coördineren met behulp van de `AZ_BATCH_TASK_SHARED_DIR` omgevingsvariabele. De `AZ_BATCH_TASK_SHARED_DIR` pad is identieke op elk knooppunt dat is toegewezen aan de taak meerdere exemplaren, dus kunt u een opdracht afhankelijk van één tussen de primaire en alle subtaken delen. Batch "deelt niet' de map in een zin remote access, maar u kunt deze gebruiken als een koppeling of punt zoals eerder beschreven in de punt op omgevingsvariabelen delen.

Resource-bestanden die u opgeeft voor de taak met meerdere exemplaren zelf worden gedownload naar de werkmap van de taak, `AZ_BATCH_TASK_WORKING_DIR`, al dan niet standaard. Zoals vermeld, in tegenstelling tot algemene bronbestanden, downloads alleen de primaire taak resource-bestanden die zijn opgegeven voor de taak met meerdere exemplaren zelf.

> [AZURE.IMPORTANT] Gebruik altijd de omgevingsvariabelen `AZ_BATCH_TASK_SHARED_DIR` en `AZ_BATCH_TASK_WORKING_DIR` om te verwijzen naar deze mappen in uw regels. Probeer niet om de paden handmatig te maken.

## <a name="task-lifetime"></a>Levensduur van taak

De levensduur van de belangrijkste taak Hiermee stelt u de levensduur van de taak hele meerdere exemplaren. Als de primaire afsluit, worden alle subtaken beëindigd. De afsluitcode van de primaire de afsluitcode van de taak is en daarom wordt gebruikt om te bepalen het slagen of mislukken van de taak voor de toepassing opnieuw.

Als een van de subtaken mislukt, mislukt afsluiten met een niet-nul afzender code, bijvoorbeeld de hele meerdere exemplaren taak. De taak meerdere exemplaren vervolgens wordt beëindigd en opnieuw worden gestart, tot aan de limiet voor het opnieuw.

Wanneer u een taak meerdere exemplaren verwijdert, worden ook de primaire en alle subtaken verwijderd door de Batch-service. Alle mappen van een subtaak en hun bestanden worden verwijderd uit de berekeningscluster knooppunten, net als voor een taak.

[TaskConstraints] [ net_taskconstraints] voor een taak meerdere exemplaren, zoals de [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock], en [RetentionTime] [ net_taskconstraint_retention] eigenschappen, zoals ze zijn voor een taak en toepassen op de primaire en alle subtaken zijn uitgevoerd. Echter als u de [RetentionTime] wijzigt[ net_taskconstraint_retention] eigenschap na het toevoegen van de taak meerdere exemplaren aan de taak, wordt deze wijziging wordt alleen toegepast op de primaire taak. Alle subtaken gaat u verder met het gebruik van de oorspronkelijke [RetentionTime][net_taskconstraint_retention].

Lijst met recente taak van een berekeningscluster knooppunt weerspiegelt de id van een subtaak als de recente taak is een onderdeel van een taak meerdere exemplaren.

## <a name="obtain-information-about-subtasks"></a>Informatie over subtaken verkrijgen

Als u informatie op subtaken met behulp van de Batch .NET-bibliotheek, belt u het [CloudTask.ListSubtasks] [ net_task_listsubtasks] methode. Deze methode geeft als resultaat informatie over alle subtaken en informatie over het knooppunt berekeningscluster die de taken uitgevoerd. Met deze informatie ziet u de hoofdmap van elke subtaak, de id van de toepassingen, zijn huidige staat, afsluitcode en meer. U kunt deze gegevens gebruiken in combinatie met de [PoolOperations.GetNodeFile] [ poolops_getnodefile] methode van de subtaak-bestanden downloaden. Houd er rekening mee dat deze methode geen informatie voor de primaire taak (id 0 retourneert).

> [AZURE.NOTE] Tenzij anders vermeld, Batch .NET methoden die werken op de meerdere exemplaren [CloudTask] [ net_task] zelf *alleen* van toepassing op de primaire taak. Bijvoorbeeld wanneer u de [CloudTask.ListNodeFiles] belt[ net_task_listnodefiles] methode voor een taak meerdere exemplaren, alleen de primaire taak bestanden worden geretourneerd.

Het volgende codefragment ziet hoe u subtaak informatie te verkrijgen, evenals bestandsinhoud aanvraagt bij de knooppunten waarop ze uitgevoerd.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == TaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Codevoorbeeld

De [MultiInstanceTasks] [ github_mpi] code op GitHub ziet u hoe u een [MS-MPI] uitvoeren met een taak meerdere exemplaren[ msmpi_msdn] toepassing op Batch knooppunten berekenen. Volg de stappen in het [voorbereiden](#preparation) en [uitvoeren](#execution) om uit te voeren in het voorbeeld.

### <a name="preparation"></a>Voorbereiding van

1. De eerste twee stappen in [het compileren en uitvoeren van een eenvoudige MS-MPI programma][msmpi_howto]. Dit aansluiten op de prerequesites voor de volgende stap.
1. Maken van *een versie van de [MPIHelloWorld] * [ helloworld_proj] steekproef MPI programma. Dit is het programma dat door de taak meerdere exemplaren op berekeningscluster knooppunten wordt uitgevoerd.
1. Maak een zip-bestand met `MPIHelloWorld.exe` (welke u stap 2 ingebouwd) en `MSMpiSetup.exe` (die u hebt gedownload stap 1). Als een toepassingspakket in de volgende stap uploadt u dit zipbestand.
1. Gebruik van de [Azure portal] [ portal] maken van een Batch [toepassing](batch-application-packages.md) 'MPIHelloWorld' genoemd en geef het zip-bestand u hebt gemaakt in de vorige stap als versie "1.0" van de toepassingspakket. Zie [uploaden en -toepassingen beheren](batch-application-packages.md#upload-and-manage-applications) voor meer informatie.

>[AZURE.TIP] Maken van *een versie van* `MPIHelloWorld.exe` zodat u niet hoeft te eventuele aanvullende afhankelijkheden bevatten (bijvoorbeeld `msvcp140d.dll` of `vcruntime140d.dll`) in uw toepassingspakket.

### <a name="execution"></a>Kan worden uitgevoerd

1. Downloaden van de [azure-batch-voorbeelden] [ github_samples_zip] uit GitHub.
1. Open de MultiInstanceTasks- **oplossing** in Visual Studio-2015. De `MultiInstanceTasks.sln` oplossingsbestand zich bevindt in:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`

1. Voer de referenties voor uw Batch en opslag-account in `AccountSettings.settings` in het project **Microsoft.Azure.Batch.Samples.Common** .
1. **Maken en uitvoeren** de oplossing MultiInstanceTasks uitvoeren van de MPI voorbeeld van toepassing op berekenen knooppunten in een groep Batch.
1. *Optioneel*: de [Azure-portal] gebruiken[ portal] of de [Batch Explorer] [ batch_explorer] te onderzoeken het voorbeeld van toepassingen, functie en taak ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") voordat u de bronnen verwijderen.

>[AZURE.TIP] U kunt downloaden [Visual Studio-Community] [ visual_studio] gratis als u nog geen Visual Studio.

Uitvoer van `MultiInstanceTasks.exe` is vergelijkbaar met de volgende handelingen uit:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Volgende stappen

- De blog Microsoft HPC & Azure Batch Team wordt beschreven hoe [MPI ondersteuning voor Linux op Azure Batch][blog_mpi_linux], en bevat informatie over het gebruik van [OpenFOAM] [ openfoam] met Batch. Vindt u voorbeelden van Python code voor het [voorbeeld van de OpenFOAM op GitHub][github_mpi].

- Leer hoe u [groepen Linux berekeningscluster knooppunten maken](batch-linux-nodes.md) voor gebruik in uw Azure Batch MPI-oplossingen.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Meerdere exemplaren overzicht"
