<properties
    pageTitle="Gebruik van de Batch knooppunt met parallelle taken maximaliseren | Microsoft Azure"
    description="Efficiëntie en lagere kosten via minder berekeningscluster knooppunten en lopende gelijktijdige taken op elk knooppunt in een toepassingen Azure Batch vergroten"
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
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="maximize-azure-batch-compute-resource-usage-with-concurrent-node-tasks"></a>Azure Batch berekeningscluster Resourcegebruik met gelijktijdige knooppunt taken maximaliseren

Door meerdere taken tegelijk op elk knooppunt berekeningscluster in uw groep Azure Batch uitgevoerd, kunt u Resourcegebruik op een kleinere aantal knooppunten in de groep maximaliseren. Voor sommige werkbelastingen, kan dit resulteren in korter taaktijden en lagere kosten.

Terwijl u bepaalde scenario's profiteren van alle resources van het knooppunt uitsluitend aan één taak, profiteert verschillende situaties toestaan meerdere taken om deze hulpbronnen te delen:

 - **Minimaliseren overdracht van gegevens** als taken kunt om gegevens te delen. In dit scenario kunt u doorverbinden datakosten aanzienlijk verkleinen door gedeelde gegevens kopiëren naar een kleiner aantal knooppunten en uitvoeren van taken op elk knooppunt parallel. Dit geldt met name als de gegevens die moeten worden gekopieerd naar elke knooppunt moeten worden overgebracht tussen geografische regio's.

 - **Geheugengebruik van Maximizing** wanneer de taken een grote hoeveelheid geheugen, maar alleen tijdens korte perioden van tijd en variabele tijde tijdens het uitvoeren van vereisen. U kunt gebruikmaken van minder, maar groter weergeven, berekeningscluster knooppunten met meer geheugen efficiënte afhandeling van dergelijke pieken. Deze knooppunten meerdere taken uitgevoerd parallel op elk knooppunt zou hebben, maar elke taak wilt profiteren van de knooppunten overvloed geheugen op verschillende momenten.

 - **Knooppunt getal limieten beperkende** als de communicatie tussen knooppunt moet ondernomen binnen een groep worden. Op dit moment zijn geconfigureerd voor de communicatie tussen knooppunt van toepassingen beperkt tot 50 berekeningscluster knooppunten. Als elke knooppunt in een dergelijke groep kunnen uitvoeren van taken parallel, kan een groter aantal taken tegelijkertijd worden uitgevoerd.

 - **Een on-premises repliceren cluster berekenen**, zoals wanneer u eerst een berekeningscluster-omgeving naar Azure verplaatsen. Als uw huidige on-premises implementatie-oplossing meerdere taken per berekeningscluster knooppunt uitvoert, kunt u het maximum aantal knooppunt taken beter gespiegelde die configuratie vergroten.

## <a name="example-scenario"></a>Voorbeeldscenario

Als voorbeeld om te laten zien van de voordelen van de uitvoering van parallelle taken, Stel dat uw taak-toepassing van processor en geheugen vereisten heeft dat [standaard\_D1](../cloud-services/cloud-services-sizes-specs.md#general-purpose-d) knooppunten voldoende zijn. Maar om te voltooien de taak in de vereiste tijd, 1.000 van deze knooppunten nodig zijn.

In plaats van met behulp van standaard\_D1 knooppunten met 1 CPU core, kunt u [standaard\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) knooppunten die 16 cores hebben, en wel parallelle taak kan worden uitgevoerd. Daarom *16 tijden minder knooppunten* kan worden gebruikt--in plaats van 1000 knooppunten, alleen 63 is vereist. Bovendien als grote toepassingsbestanden of referentiegegevens vereist voor elk knooppunt zijn, zijn duur van taak en efficiëntie opnieuw verbeterd aangezien de gegevens wordt gekopieerd naar alleen 16 knooppunten.

## <a name="enable-parallel-task-execution"></a>Uitvoering van parallelle taken inschakelen

U configureren berekeningscluster knooppunten voor de uitvoering van parallelle taken op het niveau van toepassingen. Instellen met de Batch .NET-bibliotheek, de [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] eigenschap wanneer u een groep maken. Als u de Batch REST API gebruikt, stelt u de [maxTasksPerNode] [ rest_addpool] element in de hoofdtekst van het verzoek om tijdens het maken van de groep.

Azure Batch kunt u maximale taken per knooppunt maximaal viermaal drukken instellen (x 4) het aantal knooppunt cores. Bijvoorbeeld als de toepassingen is geconfigureerd met knooppunten van grootte "Groot" (vier cores), klikt u vervolgens `maxTasksPerNode` 16 kan worden ingesteld. Zie voor meer informatie over het aantal cores voor elk van de grootte van het knooppunt, [grootten voor Cloud Services](../cloud-services/cloud-services-sizes-specs.md). Zie voor meer informatie over limieten voor de service, [quota en -limieten voor de Batch Azure-service](batch-quota-limit.md).

> [AZURE.TIP] Zorg ervoor dat u rekening houden met de `maxTasksPerNode` waarde wanneer u een [automatisch schalen formule] samenstellen[ enable_autoscaling] voor uw groep. Bijvoorbeeld een formule die resulteert `$RunningTasks` aanzienlijk kan worden beïnvloed door een stijging van taken per knooppunt. Zie [automatisch de knooppunten in een Azure Batch van toepassingen voor het berekenen van schaal](batch-automatic-scaling.md) voor meer informatie.

## <a name="distribution-of-tasks"></a>Verdeling van taken

Als de berekeningscluster knooppunten in een groep taken tegelijkertijd uitvoert kunnen, is het is belangrijk om op te geven hoe de taken die u moet worden verdeeld over de knooppunten in de groep.

Met behulp van de [CloudPool.TaskSchedulingPolicy] [ task_schedule] eigenschap, kunt u opgeven dat taken gelijkmatig op alle knooppunten in de groep ("verspreiding") moeten worden toegewezen. Of u kunt opgeven dat zo zo veel mogelijk taken moeten worden toegewezen aan elke knooppunt voordat taken zijn toegewezen aan een ander knooppunt in de groep ("verpakking").

Een voorbeeld van hoe deze functie waardevol is, kunt u de groep [standaard\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) knooppunten (in het voorbeeld hierboven) die is geconfigureerd met een [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] waarde van 16. Als de [CloudPool.TaskSchedulingPolicy] [ task_schedule] is geconfigureerd met een [ComputeNodeFillType] [ fill_type] van *Pack*, zou het gebruik van alle 16 cores van elk knooppunt maximaliseren en toestaan van een [groep autoscaling](batch-automatic-scaling.md) weghalen van niet-gebruikte knooppunten uit de groep (knooppunten zonder alle toegewezen taken). Hiermee wordt geminimaliseerd Resourcegebruik en slaat u geld.

## <a name="batch-net-example"></a>Batch .NET-voorbeeld

Deze [Batch.NET] [ api_net] API-codefragment toont een aanvraag om deel te maken van een groep met vier grote knooppunten met maximaal vier taken per knooppunt. Hiermee geeft u een taakplanning beleid dat elk knooppunt worden gevuld met taken vóór taken toewijzen aan een ander knooppunt in de groep. Zie voor meer informatie over het toevoegen van toepassingen met behulp van de Batch .NET-API [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicated: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Voorbeeld van de REST batch

Deze [Batch REST] [ api_rest] API fragment ziet u een verzoek om het maken van een resourcegroep die met twee grote knooppunten met maximaal vier taken per knooppunt. Zie [toevoegen een resourcegroep die bij een account]voor meer informatie over het toevoegen van toepassingen met behulp van de REST API[rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicated":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [AZURE.NOTE] U kunt instellen dat de `maxTasksPerNode` element en [MaxTasksPerComputeNode] [ maxtasks_net] eigenschap alleen op de aanmaaktijd van een groep. Ze kunnen niet worden gewijzigd nadat een resourcegroep die al is gemaakt.

## <a name="code-sample"></a>Codevoorbeeld

De [ParallelNodeTasks] [ parallel_tasks_sample] project op GitHub ziet u het gebruik van de [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] eigenschap.

Deze toepassing C#-console gebruikt de [Batch.NET] [ api_net] -bibliotheek met het maken van een resourcegroep die met een of meer berekeningscluster knooppunten. Een configureerbare aantal taken wordt uitgevoerd op die knooppunten simuleren variabele laden. Uitvoer van de toepassing geeft aan welke knooppunten uitgevoerd elke taak. De toepassing bevat ook een overzicht van de taakparameters- en eindtijd. Het overzicht deel van de uitvoer van twee verschillende wordt uitgevoerd van de steekproef-toepassing wordt weergegeven onder.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

De eerste uitvoering van de steekproef-toepassing geeft die met één knooppunt in de groep en de standaardinstelling van één taak per knooppunt, de duur van de taak is meer dan 30 minuten.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

De tweede reeks van de steekproef ziet nadelig duur van de taak. Dit komt omdat de toepassingen is geconfigureerd met vier taken per knooppunt, waarmee voor de uitvoering van parallelle taken aan de taak voltooien in bijna een kwartaal van de tijd.

> [AZURE.NOTE] De duur van de taak in de bovenstaande samenvattingen Neem niet Aanmaaktijd van een groep. Elk van de bovenstaande taken is verzonden naar eerder gemaakte toepassingen waarvan knooppunten berekeningscluster in de stand van *niet-actief* indiening tegelijk zijn.

## <a name="next-steps"></a>Volgende stappen

### <a name="batch-explorer-heat-map"></a>Batch Explorer Heatmap

De [Azure Batch Explorer][batch_explorer], een van de Azure Batch [steekproef toepassingen][github_samples], bevat een *Heatmap* -functie waarmee visualisatie van de uitvoering van taken. Wanneer u bent uitvoeren van de [ParallelTasks] [ parallel_tasks_sample] steekproef-toepassing, kunt u de functie Heatmap de uitvoering van parallelle taken op elk knooppunt eenvoudig te visualiseren.

![Batch Explorer Heatmap][1]

*Explorer-Heatmap voor de batch met een groep van vier knooppunten, met elk knooppunt vier taken dat momenteel wordt uitgevoerd*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png
