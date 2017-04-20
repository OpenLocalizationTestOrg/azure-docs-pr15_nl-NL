<properties
    pageTitle="Efficiënt lijst query's in Azure Batch | Microsoft Azure"
    description="Prestaties verbeteren door te filteren van uw query's bij het aanvragen van informatie over de Batch resources zoals groepen, taken, taken en knooppunten berekenen."
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

# <a name="query-the-azure-batch-service-efficiently"></a>De Batch Azure-service efficiënt query

Hier leert u hoe u de prestaties van de toepassing van de Batch Azure doordat de hoeveelheid gegevens die worden geretourneerd door de service wanneer u query van projecten, taken, en berekenen van knooppunten met de [Batch.NET] vergroten[ api_net] bibliotheek.

Vrijwel alle Batch toepassingen moeten een type controleren of een andere bewerking die de Batch-service, vaak met regelmatige tussenpozen vraagt uitvoeren. Om te bepalen of er zijn taken in de wachtrij resterende in een project, moet u bijvoorbeeld gegevens ophalen van elke taak in de taak. Om te bepalen de status van knooppunten in uw groep, moet u gegevens ophalen op elk knooppunt in de groep. In dit artikel wordt uitgelegd hoe u deze query's uitvoeren in de efficiëntste manier.

## <a name="meet-the-detaillevel"></a>Voldoen aan de DetailLevel

In een productie Batch toepassing kunnen entiteiten zoals projecten, taken en berekeningscluster knooppunten in het duizendtal nummeren. Wanneer u informatie over deze bronnen aanvraagt, moet een potentieel grote hoeveelheden gegevens "cross de kabel" van de Batch-service in uw toepassing op beide query's. Door het aantal items en type informatie dat wordt geretourneerd door een query beperken, kunt u de snelheid van uw query's en kunnen daarom de prestaties van uw toepassing vergroten.

Deze [Batch.NET] [ api_net] API-codefragment bevat *elke* taak die is gekoppeld aan een taak, samen met *alle* van de eigenschappen van elke taak:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

U kunt een lijstquery veel efficiënter echter uitvoeren met een "detailniveau" toepassen op uw query. Dit doet u door het opgeven van een [ODATADetailLevel] [ odata] object naar de [JobOperations.ListTasks] [ net_list_tasks] methode. Het codefragment van deze retourneert alleen de ID, opdrachtregel en berekeningscluster knooppunt informatie eigenschappen van voltooide taken:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

In dit voorbeeldscenario als er duizenden taken in de taak zijn, de resultaten van de tweede query meestal teruggeleid veel sneller dan de eerste. Meer informatie over het gebruik van ODATADetailLevel wanneer u een lijst met items met de Batch .NET-API is opgenomen [onder](#efficient-querying-in-batch-net).

> [AZURE.IMPORTANT]
> Het is raadzaam dat u *altijd* Geef een object ODATADetailLevel aan uw gesprekken van de lijst .NET-API om ervoor te zorgen maximale efficiëntie en prestaties van uw toepassing. U kunt een detailniveau opgeeft, verlaag Batch service antwoord tijden, netwerkgebruik verbeteren en geheugengebruik minimaliseren door clienttoepassingen.

## <a name="filter-select-and-expand"></a>Filteren, selecteert u en uitvouwen

De [Batch.NET] [ api_net] en de [REST van de Batch] [ api_rest] API's bieden de mogelijkheid om te verminderen beide het aantal items die in een lijst worden geretourneerd, maar ook de hoeveelheid informatie die wordt geretourneerd voor elke. U doen dat door op te geven **filter**en **Selecteer** **tekenreeksen uitvouwen** bij het uitvoeren van query's lijst.

### <a name="filter"></a>Filter
De filtertekenreeks is een expressie die het aantal items die worden geretourneerd vermindert. Bijvoorbeeld een lijst met alleen de actieve taken voor een taak of lijst alleen berekeningscluster knooppunten die u wilt uitvoeren van taken.

- De filtertekenreeks bestaat uit een of meer expressies, met een expressie die uit een naam van eigenschap, operator en waarde bestaat. De eigenschappen die kunnen worden opgegeven zijn specifiek voor elk entiteitstype die u query uitvoert, zoals de operatoren die worden ondersteund voor elke eigenschap zijn.
- Meerdere expressies kunnen worden gecombineerd met behulp van de logische operatoren `and` en `or`.
- In dit voorbeeld tekenreekslijsten filteren alleen de actieve 'weergegeven' taken: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Selecteer
De optie tekenreeks de geldige eigenschapwaarden beperkt die worden geretourneerd voor elk item. Geeft u een lijst met eigenschapnamen, en alleen de eigenschapswaarden van deze worden geretourneerd voor de items in de queryresultaten.

- De optie tekenreeks bestaat uit een door komma's gescheiden lijst met eigenschapnamen. U kunt opgeven dat een van de eigenschappen voor het entiteitstype die u wilt zoeken.
- In dit voorbeeld select tekenreeks dat slechts drie eigenschapswaarden moeten worden geretourneerd voor elke taak: `id, state, stateTransitionTime`.

### <a name="expand"></a>Uitvouwen
De tekenreeks uitvouwen minder API-oproepen die nodig zijn om bepaalde informatie te verkrijgen. Wanneer u een tekenreeks uitvouwen, meer informatie over elk item kan worden verkregen met een enkele API-oproep. In plaats van eerste het verkrijgen van de lijst met entiteiten, klikt u vervolgens het aanvragen van informatie voor elk item in de lijst met een tekenreeks uitvouwen kunt u dezelfde gegevens in één API aanroep aanvragen. Betere prestaties betekent dat er minder API-oproepen.

- Net als in de select tekenreeks wordt de tekenreeks uitvouwen besturingselementen of bepaalde gegevens worden opgenomen in de lijst met queryresultaten.
- De tekenreeks uitvouwen wordt alleen ondersteund wanneer deze wordt gebruikt in de lijst taken, taakplanningen, taken en groepen. Op dit moment alleen ondersteund statistische gegevens.
- Wanneer alle eigenschappen vereist zijn en geen select tekenreeks wordt opgegeven, worden de uitvouwen tekenreeks *moet* gebruikt om statistieken informatie te verkrijgen. Als een select tekenreeks wordt gebruikt voor een subset van eigenschappen, klikt u vervolgens `stats` kan worden opgegeven in de select tekenreeks en de tekenreeks uitvouwen niet hoeft te worden opgegeven.
- In dit voorbeeld uitvouwen tekenreeks geeft aan dat statistische gegevens moet worden geretourneerd voor elk item in de lijst: `stats`.

> [AZURE.NOTE] Wanneer het bouwen van een van de drie querytypen tekenreeks (filteren en selecteer uitvouwen), moet u ervoor zorgen dat de eigenschapnamen en hoofdletters/kleine letters overeenkomen met die van hun tegenhangers REST API-element. Bijvoorbeeld tijdens het werken met de klas .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) , moet u de **status** in plaats van **de staat**, zelfs als de eigenschap .NET [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). Zie de tabellen onder voor eigenschaptoewijzingen tussen de REST API's en .NET.

### <a name="rules-for-filter-select-and-expand-strings"></a>Regels voor het filter, selecteer en vouwen van tekenreeksen

- Namen van eigenschappen in filter, selecteer en uitvouwen tekenreeksen moet worden weergegeven als in de [REST van de Batch] [ api_rest] API--zelfs wanneer u [Batch.NET] [ api_net] of op een van de andere Batch SDK's.
- De namen van alle eigenschappen zijn hoofdlettergevoelig, maar eigenschapswaarden zijn hoofdlettergevoelig.
- Datum/tijd tekenreeksen kunnen twee verschillende indelingen, en moet worden voorafgegaan door `DateTime`.

  - W3C-DTF-indeling voorbeeld:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  - Voorbeeld van de indeling RFC 1123:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
- Booleaanse tekenreeksen zijn `true` of `false`.
- Als een ongeldige eigenschappen of de operator is opgegeven, een `400 (Bad Request)` fout veroorzaakt.

## <a name="efficient-querying-in-batch-net"></a>Efficiënt query's uitvoeren in de Batch .NET

In de [Batch.NET] [ api_net] API, de [ODATADetailLevel] [ odata] klasse wordt gebruikt voor het opgeven van filteren, en selecteer tekenreeksen aan lijst met bewerkingen uitbreiden. De klasse ODataDetailLevel heeft drie openbare tekenreekseigenschappen die kunnen worden opgegeven in de constructor of ingesteld rechtstreeks op het object. U vervolgens het object ODataDetailLevel als parameter doorgeven naar de verschillende bewerkingen in de lijst zoals [ListPools][net_list_pools], [ListJobs][net_list_jobs], en [ListTasks][net_list_tasks].

- [ODATADetailLevel][odata]. [FilterClause] [ odata_filter]: Beperk het aantal items die worden geretourneerd.
- [ODATADetailLevel][odata]. [SelectClause] [ odata_select]: Opgeven welke eigenschapswaarden worden geretourneerd met elk item.
- [ODATADetailLevel][odata]. [ExpandClause] [ odata_expand]: Gegevens ophalen voor alle items in één API bellen in plaats van afzonderlijke oproepen voor elk item.

Het volgende codefragment gebruikt de Batch .NET-API om efficiënt query's in de Batch-service voor de statistiek van een specifieke reeks toepassingen. In dit scenario heeft de gebruiker Batch zowel testomgeving als operationele toepassingen. De groep test ID's worden voorafgegaan door 'test', en de productiepool id's worden voorafgegaan door "productcatalogus". In het fragment is *myBatchClient* het een goed geïnitialiseerd exemplaar van de klasse [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) .

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [AZURE.TIP] Een exemplaar van [ODATADetailLevel] [ odata] die is geconfigureerd met selecteren en uitvouwen componenten kunnen ook worden doorgegeven om het juiste Get-methoden, zoals [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), om de hoeveelheid gegevens die worden geretourneerd te beperken.

## <a name="batch-rest-to-net-api-mappings"></a>Batch REST aan .NET-API toewijzingen

Eigenschapsnamen in filter, selecteer en uitvouwen tekenreeksen *moet* overeenkomen met de tegenhangers van hun REST API, zowel in de naam en de hoofdletters/kleine letters. De onderstaande tabellen bevatten toewijzingen tussen de .NET en REST API tegenhangers van Excel.

### <a name="mappings-for-filter-strings"></a>Toewijzingen voor tekenreeksen

- **.NET lijst methoden**: elk van de methoden .NET-API in deze kolom kan een [ODATADetailLevel] [ odata] object als een parameter.
- **REST-lijst aanvragen**: elke REST API-pagina die zijn gekoppeld aan in deze kolom bevat een tabel die Hiermee geeft u de eigenschappen en de bewerkingen die zijn toegestaan in *tekenreeksen* . U gebruikt deze eigenschapnamen en bewerkingen wanneer u een [ODATADetailLevel.FilterClause] samenstellen[ odata_filter] tekenreeks.

| Methoden voor .NET-lijst | REST-lijst aanvragen |
|---|---|
| [CertificateOperations.ListCertificates][net_list_certs] | [De certificaten in een account weergeven][rest_list_certs]
| [CloudTask.ListNodeFiles][net_list_task_files] | [Een lijst met de bestanden die zijn gekoppeld aan een taak][rest_list_task_files]
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] | [Een lijst met de status van de voorbereiding van de taak en release projecttaken voor een taak][rest_list_jobprep_status]
| [JobOperations.ListJobs][net_list_jobs] | [Lijst van de taken in een account][rest_list_jobs]
| [JobOperations.ListNodeFiles][net_list_nodefiles] | [Een lijst met de bestanden op een knooppunt][rest_list_nodefiles]
| [JobOperations.ListTasks][net_list_tasks] | [Een lijst met de taken die zijn gekoppeld aan een taak][rest_list_tasks]
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] | [Een lijst met de Project-planningen in een account][rest_list_job_schedules]
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] | [Lijst van de taken die zijn gekoppeld aan een projectplanning][rest_list_schedule_jobs]
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] | [Lijst van de berekeningscluster knooppunten in een groep][rest_list_compute_nodes]
| [PoolOperations.ListPools][net_list_pools] | [Lijst van de groepen in een account][rest_list_pools]

### <a name="mappings-for-select-strings"></a>Toewijzingen voor select tekenreeksen

- **Batch .NET-gegevenstypen**: Batch .NET API typen.
- **REST API entiteiten**: elke pagina in deze kolom bevat een of meer tabellen die de namen van de REST API-eigenschappen voor het type vermelden. De eigenschapnamen van deze worden gebruikt wanneer u bij het samenstellen van tekenreeksen *selecteren* . U kunt deze dezelfde eigenschapnamen worden gebruikt wanneer u een [ODATADetailLevel.SelectClause] samenstellen[ odata_select] tekenreeks.

| Batch .NET-gegevenstypen | REST API entiteiten |
|---|---|
| [Certificaat][net_cert] | [Informatie over een certificaat][rest_get_cert] |
| [CloudJob][net_job] | [Informatie over een taak][rest_get_job] |
| [CloudJobSchedule][net_schedule] | [Informatie over een projectplanning][rest_get_schedule] |
| [ComputeNode][net_node] | [Informatie over een knooppunt][rest_get_node] |
| [CloudPool][net_pool] | [Informatie over een resourcegroep][rest_get_pool] |
| [CloudTask][net_task] | [Informatie over een taak][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Voorbeeld: een filtertekenreeks maken

Wanneer u een filtertekenreeks maken voor [ODATADetailLevel.FilterClause][odata_filter], raadpleegt u de bovenstaande tabel onder 'Toewijzingen voor tekenreeksen' om te zoeken naar de REST API documentatiepagina die overeenkomt met de lijstbewerking die u wilt uitvoeren. U vindt de filterbare eigenschappen en hun ondersteunde operatoren in de eerste multirow tabel op die pagina. Als u wilt ophalen van alle taken waarvan u de afsluitcode niet nul is, bijvoorbeeld deze rij op [lijst met de taken die zijn gekoppeld aan een taak] [ rest_list_tasks] Hiermee geeft u de toepassing eigenschap tekenreeks en de toegestane operatoren:

| Eigenschap | Bewerkingen die zijn toegestaan | Type |
| :--- | :--- | :--- |
| `executionInfo/exitCode` | `eq, ge, gt, le , lt` | `Int` |

De filtertekenreeks voor het aanbieden van alle taken met een andere waarde dan nul afsluitcode zou dus:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Voorbeeld: een select tekenreeks maken

Opbouwen [ODATADetailLevel.SelectClause][odata_select], raadpleegt u de bovenstaande tabel onder "Toewijzingen voor select tekenreeksen" en navigeer naar de REST API-pagina die overeenkomt met het type entiteit die u biedt. U vindt de selecteerbaar eigenschappen en hun ondersteunde operatoren in de eerste multirow tabel op die pagina. Als u ophalen alleen de-ID en de opdrachtregel voor elke taak in een lijst wilt, bijvoorbeeld, vindt u deze rijen in de tabel van toepassing op [informatie over een taak][rest_get_task]:

| Eigenschap | Type | Notities |
| :--- | :--- | :--- |
| `id` | `String` | `The ID of the task.` |
| `commandLine` | `String` | `The command line of the task.` |

De optie tekenreeks voor het opnemen van alleen de-ID en de opdrachtregel met elke lijst taak zou vervolgens:

`id, commandLine`

## <a name="code-samples"></a>Voorbeelden van programmacode

### <a name="efficient-list-queries-code-sample"></a>Voorbeeld van de code efficiënt lijst query 's

Bekijk de [EfficientListQueries] [ efficient_query_sample] steekproef project op GitHub om te zien hoe efficiënt lijst doorzoeken kan invloed hebben op prestaties in een toepassing. Deze C#-console-toepassing wordt gemaakt en voegt een groot aantal taken aan een taak. Vervolgens dit zorgt ervoor dat meerdere oproepen naar de [JobOperations.ListTasks] [ net_list_tasks] methode en stadia [ODATADetailLevel] [ odata] objecten die zijn geconfigureerd met waarden van verschillende eigenschappen variëren van de hoeveelheid gegevens moeten worden geretourneerd. Levert deze uitvoer ongeveer als volgt uit:

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

Zoals u in de tijden verstreken, kunt u query antwoord tijden kunt enorm verlagen door te beperken de eigenschappen en het aantal items die worden geretourneerd. U kunt dit en andere projecten steekproef vinden in de [azure-batch-voorbeelden] [ github_samples] bibliotheek op GitHub.

### <a name="batchmetrics-library-and-code-sample"></a>BatchMetrics bibliotheek en code-voorbeeld

Naast de EfficientListQueries code het voorbeeld hierboven, vindt u de [BatchMetrics] [ batch_metrics] project in de [azure-batch-voorbeelden] [ github_samples] GitHub opslagplaats. Het project van de steekproef BatchMetrics laat zien hoe efficiënt voortgang Azure Batch taak de Batch-API gebruiken.

De [BatchMetrics] [ batch_metrics] voorbeeld bevat een .NET class bibliotheek project die u kunt opnemen in uw eigen projecten en een eenvoudige opdrachtregel programma om te oefenen en het gebruik van de bibliotheek demonstreren.

De toepassing van de steekproef binnen het project ziet u de volgende bewerkingen:

1. Specifieke kenmerken selecteren om te downloaden van alleen de eigenschappen die u nodig hebt
2. Filteren op staat overgangstijden om te kunnen alleen wijzigingen sinds de laatste query downloaden

De volgende methode wordt bijvoorbeeld weergegeven in de bibliotheek BatchMetrics. Dit geeft als resultaat een ODATADetailLevel die dat alleen aangeeft de `id` en `state` eigenschappen moeten worden verkregen voor de eenheden die worden doorzocht. Deze ook geeft aan dat alleen entiteiten waarvan de status is gewijzigd sinds het opgegeven `DateTime` parameter moet worden geretourneerd.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Volgende stappen

### <a name="parallel-node-tasks"></a>Parallelle knooppunt taken

[Azure Batch maximaliseren berekenen Resourcegebruik met gelijktijdige knooppunt taken](batch-parallel-node-tasks.md) is een ander artikel die betrekking hebben op prestaties van de Batch-toepassingen. Sommige soorten werkbelasting kunnen profiteren van parallelle taken uitvoeren op grotere-- maar minder--knooppunten berekenen. Bekijk het [voorbeeldscenario](batch-parallel-node-tasks.md#example-scenario) in het artikel voor meer informatie over dit scenario.

### <a name="batch-forum"></a>Batch-Forum

Het [Forum van Azure Batch] [ forum] op MSDN is een prima plek om te bespreken Batch en vragen over de service. Hoofd op via voor handig "Plaktoetsen" berichten, vragen en posten voor uw dat ze zich voordoen terwijl u uw Batch oplossingen bouwen.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

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

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
