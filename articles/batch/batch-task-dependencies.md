<properties
    pageTitle="Taakafhankelijkheden in Azure Batch | Microsoft Azure"
    description="Taken die afhankelijk van het afronden van andere taken zijn voor het verwerken van MapReduce stijl en soortgelijke groot gegevens maken werkbelasting in Azure Batch."
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
    ms.date="09/28/2016"
    ms.author="marsma" />

# <a name="task-dependencies-in-azure-batch"></a>Taakafhankelijkheden in Azure Batch

De functie afhankelijkheden van Azure Batch is een goede aanpassen als u wilt verwerken:

- De werklast van de stijl van een MapReduce in de cloud.
- Taken waarvan gegevensverwerking taken kunnen worden weergegeven als een Services acyclische grafiek (DAG).
- Elke andere taak waarin volgende taken zijn afhankelijk van de uitvoer van boven taken.

Taakafhankelijkheden batch kunnen u taken die zijn gepland voor uitvoering op berekeningscluster knooppunten alleen na het afronden van een of meer taken maken. U kunt bijvoorbeeld een taak die worden weergegeven door elk frame van een 3D-film aan afzonderlijke, parallelle taken maken. Het uiteindelijke taak--de "samenvoegen taak"--wordt samengevoegd de weergegeven frames in de volledige film alleen nadat alle frames zijn correct weergegeven.

U kunt taken die afhankelijk van andere taken in een een-op- of een-op-veel-relatie zijn maken. U kunt zelfs een bereik afhankelijkheid waarop een taak afhankelijk van het afronden van een groep taken binnen een bepaald datumbereik taak-id's is maken. U kunt deze drie eenvoudige scenario's als u veel-op-veel-relaties wilt combineren.

## <a name="task-dependencies-with-batch-net"></a>Taakafhankelijkheden met Batch .NET

In dit artikel bespreken we taakafhankelijkheden configureren met behulp van de [Batch.NET] [ net_msdn] bibliotheek. Eerst leert u hoe u [taakafhankelijkheid inschakelen](#enable-task-dependencies) voor uw projecten, en wordt vervolgens getoond het [configureren van een taak met afhankelijkheden](#create-dependent-tasks). Ten slotte, wordt de [afhankelijkheid scenario's](#dependency-scenarios) die ondersteuning biedt voor Batch besproken.

## <a name="enable-task-dependencies"></a>Taakafhankelijkheden inschakelen

Als u wilt gebruiken taakafhankelijkheden in uw toepassing Batch, moet u eerst hoogte de Batch-service dat de taak taakafhankelijkheden gebruikt. In de Batch .NET, inschakelen op uw [CloudJob] [ net_cloudjob] door in te stellen van de [UsesTaskDependencies] [ net_usestaskdependencies] eigenschap `true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

In het voorgaande codefragment "batchClient" is een exemplaar van de [BatchClient] [ net_batchclient] class.

## <a name="create-dependent-tasks"></a>Afhankelijke taken maken

Als u een taak maken die afhankelijk is van het afronden van een of meer taken, wilt zien u Batch dat de taak 'afhankelijk van"de andere taken is. Configureer de [CloudTask]in Batch .NET,[net_cloudtask]. [DependsOn] [net_dependson] eigenschap met een exemplaar van de [TaskDependencies] [ net_taskdependencies] class:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

In dit codefragment maakt u een taak met de ID van 'Bloemen' die wordt gepland om alleen worden uitgevoerd op een berekeningscluster knooppunt nadat de taken met id's van "Regen" en "Zo" zijn voltooid.

 > [AZURE.NOTE] Een taak wordt beschouwd als moet zijn voltooid als er in de stand **voltooid** en de **sluiten programmacode** `0`. In de Batch .NET, betekent dit dat een [CloudTask][net_cloudtask]. [De staat] [net_taskstate] eigenschapwaarde van `Completed` en van de CloudTask [TaskExecutionInformation][net_taskexecutioninformation]. [ExitCode] [net_exitcode] eigenschapswaarde is `0`.

## <a name="dependency-scenarios"></a>Afhankelijkheid scenario 's

Er zijn drie eenvoudige taak afhankelijkheid scenario's die u in Azure Batch gebruiken kunt:-op-een, een-op-veel en taak-ID bereik afhankelijkheid. Deze kunnen worden gecombineerd zodat een vierde scenario voor veel-op-veel.

 Scenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Voorbeeld | |
 :-------------------: | ------------------- | -------------------
 [Een-op-](#one-to-one) | *taskB* is afhankelijk van *taskA* <p/> *taskB* wordt niet worden gepland voor uitvoering totdat *taskA* is voltooid | ![Diagram:-op-een taakafhankelijkheid][1]
 [Een-op-veel](#one-to-many) | *taskC* hangt af van *taskA* en *taskB* <p/> *taskC* wordt niet worden gepland voor uitvoering totdat zowel *taskA* als *taskB* zijn voltooid | ![Diagram: een-op-veel-taakafhankelijkheid][2]
 [Taak-ID-bereik](#task-id-range) | *taskD* is afhankelijk van een bereik van taken <p/> *taskD* wordt niet worden gepland voor uitvoering totdat de taken met id's *1* tot en met *10* zijn voltooid | ![Diagram: Id-bereik taakafhankelijkheid][3]

>[AZURE.TIP] Kunt u **veel-op-veel** -relaties, zoals waar taken, C, D, E en F elke hangt af van de taken A en b te drukken. Dit is handig, bijvoorbeeld in parallelized voorverwerking scenario's waarin uw volgende taken zijn afhankelijk van de uitvoer van meerdere boven taken.

### <a name="one-to-one"></a>Een-op-

Als u een taak die afhankelijk van het afronden van een andere taak is maakt, wilt u een één taak-ID aan de [TaskDependencies]opgeeft[net_taskdependencies]. [OnId] [net_onid] statische methode wanneer u de [DependsOn] vullen[ net_dependson] eigenschap van [CloudTask][net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Een-op-veel

Als u een taak maken die afhankelijk van het afronden van meerdere taken is, wilt u een verzameling taak-id's toe aan de [TaskDependencies]opgeeft[net_taskdependencies]. [OnIds] [net_onids] statische methode wanneer u de [DependsOn] vullen[ net_dependson] eigenschap van [CloudTask][net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

### <a name="task-id-range"></a>Taak-ID-bereik

Als u wilt maken van een taak die afhankelijk van het afronden van een groep taken waarvan liggen id's in een bereik is, u leveren van de eerste en laatste id's de taak in het bereik aan de [TaskDependencies][net_taskdependencies]. [OnIdRange] [net_onidrange] statische methode wanneer u de [DependsOn] vullen[ net_dependson] eigenschap van [CloudTask][net_cloudtask].

>[AZURE.IMPORTANT] Wanneer u een taak-ID-bereiken voor uw afhankelijkheden gebruikt, moet de taak-id's in het bereik *moet* tekenreeks in de vorm van gehele getallen. Daarnaast kunnen moet elke taak in het bereik voltooid voor de afhankelijke taak te worden uitgevoerd.

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="code-sample"></a>Codevoorbeeld

De [TaskDependencies] [ github_taskdependencies] steekproef project is een van de [voorbeelden van Azure Batch code] [ github_samples] op GitHub. Deze oplossing Visual Studio-2015 laat zien hoe inschakelen taakafhankelijkheid op een taak, taken die afhankelijk van andere taken zijn maken en uitvoeren van deze taken op een groep berekeningscluster knooppunten.

## <a name="next-steps"></a>Volgende stappen

### <a name="application-deployment"></a>Implementatie van toepassingen

De functie [toepassingspakketten](batch-application-packages.md) van Batch biedt een eenvoudige manier om beide te implementeren en de toepassingen die uw taken uitvoeren op berekenen knooppunten-versie.

### <a name="installing-applications-and-staging-data"></a>Installeren van toepassingen en tijdelijke gegevens

Bekijk de [installeren toepassingen en tijdelijk opslaan van de gegevens op Batch berekenen knooppunten] [ forum_post] posten in het forum Azure Batch voor een overzicht van de verschillende methoden voor het voorbereiden van uw knooppunten taken uitvoeren. Dit bericht is geschreven door een van de Azure Batch teamleden, een goede handleiding op de verschillende manieren om bestanden (inclusief zowel toepassingen als taak invoergegevens) naar uw knooppunten berekeningscluster.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagram: een-op-afhankelijkheid"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagram: een-op-veel-afhankelijkheid"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagram: id-bereik taakafhankelijkheid"
