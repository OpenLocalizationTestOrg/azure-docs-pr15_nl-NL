<properties
    pageTitle="Visual Studio-sjablonen voor Azure Batch | Microsoft Azure"
    description="Lees meer over hoe deze sjablonen voor Visual Studio-project kunt implementeren en uw werkbelasting computerintensieve worden uitgevoerd op Azure Batch"
    services="batch"
    documentationCenter=".net"
    authors="fayora"
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

# <a name="visual-studio-project-templates-for-azure-batch"></a>Visual Studio project-sjablonen voor Azure Batch

De **Manager van de taak** en de **taak Processor Visual Studio-sjablonen** voor Batch bieden code waarmee u kunt implementeren en uitvoeren van uw werkbelasting computerintensieve op Batch met minder inspanning. Dit document een beschrijving van deze sjablonen en bevat richtlijnen voor het gebruik ervan.

>[AZURE.IMPORTANT] In dit artikel wordt beschreven hoe alleen informatie met betrekking tot deze twee sjablonen en wordt ervan uitgegaan dat u bekend met de Batch-service en belangrijke begrippen met betrekking tot deze bent: pools, knooppunten, taken en taken, projecttaken manager, omgevingsvariabelen en andere relevante informatie berekenen. U vindt meer informatie in de [Basisbeginselen van Azure Batch](batch-technical-overview.md), [Functieoverzicht Batch voor ontwikkelaars](batch-api-basics.md)en [aan de slag met de bibliotheek Azure Batch voor .NET](batch-dotnet-get-started.md).

## <a name="high-level-overview"></a>Globaal overzicht

De Manager van de taak en taak Processor sjablonen kunnen worden gebruikt om het maken van twee handige onderdelen:

* Een taak van de taak manager waarmee de splitser van een taak die u kunt een taak Splits op in meerdere taken die belooft parallel kunnen uitvoeren.

* Een taak-processor die kan worden gebruikt om uit te voeren voorverwerking en achteraf rond de opdrachtregel van een toepassing worden verwerkt.

Bijvoorbeeld in een scenario voor film weergave zou de taak splitser veranderen een enkele film-taak in honderden of duizenden verschillende taken die afzonderlijke frames afzonderlijk wilt verwerken. : De taak processor zou de weergave-toepassing niet starten en alle afhankelijke processen die nodig zijn voor weergave van elk frame, evenals extra bewerkingen (bijvoorbeeld het weergegeven frame kopiëren naar een opslaglocatie) uitvoeren.

>[AZURE.NOTE] De Manager van de taak en taak Processor sjablonen zijn onafhankelijk van elkaar, zodat u kiezen kunt voor het gebruik van beide of slechts één van beide, afhankelijk van de vereisten van uw taak berekenen en klik op uw voorkeuren.

Zoals u in het onderstaande diagram, gaat u door drie diverse stadia een berekeningscluster taak waarin deze sjablonen:

1. Een taak met de service Batch op Azure, geven als het programma van de manager taak van de manager van de taak taak ingediend die de clientcode (bijvoorbeeld-toepassing, webservice, enzovoort).

2. De projecttaak manager op een berekeningscluster knooppunt wordt uitgevoerd door de Batch-service en de splitser taak gestart op van het opgegeven aantal taak processor taken, zoals veel knooppunten zoals vereist, op basis van de parameters en specificaties in de code van de splitser taak berekenen.

3. De taken van de processor taak uitgevoerd belooft parallel, om te verwerken van de invoergegevens en genereren van de uitvoergegevens.

![Diagram waarin wordt getoond hoe clientcode moet samenwerken met de Batch-service][diagram01]

## <a name="prerequisites"></a>Vereisten voor

De Batch als sjablonen wilt gebruiken, moet u het volgende:

* Een computer met Visual Studio-2015 verlengt, of hoger, al is geïnstalleerd.

* De Batch-sjablonen beschikbaar in de [Visual Studio-galerie zijn] [ vs_gallery] als Visual Studio uitbreidingen. Er zijn twee manieren om toegang te krijgen van de sjablonen:

  * Installeren van de sjablonen die via het dialoogvenster **Extensions en Updates** in Visual Studio (Zie voor meer informatie, [Zoeken en het gebruik van Visual Studio Extensions][vs_find_use_ext]). Zoek in het dialoogvenster **Extensions en Updates** en downloaden van de volgende twee extensies:

    * Azure Batch Taakbeheer met taak splitsen
    * Azure Batch taak Processor

  * De sjablonen downloaden van de onlinegalerie voor Visual Studio: [Microsoft Azure Batch Project-sjablonen][vs_gallery_templates]

* Als u van plan bent de [Toepassing-pakketten](batch-application-packages.md) gebruiken om te implementeren van de manager van de taak en taak processor naar de Batch berekenen knooppunten, moet u een opslag-account koppelen aan uw account Batch.

## <a name="preparation"></a>Voorbereiding van

Het is raadzaam om het maken van een oplossing die uw manager taak, evenals uw processor taak kan bevatten omdat Hiermee kunt u deze eenvoudiger code tussen uw Taakbeheer en de taak processor-programma's delen. Als u wilt deze oplossing hebt gemaakt, de volgende stappen uit:

1. Open Visual Studio-2015 en selecteer **bestand** > **Nieuw** > **Project**.

2. Klik onder **sjablonen** **Andere projecttypen**uitvouwen, klikt u op **Visual Studio-oplossingen**en selecteer vervolgens **Blanco-oplossing**.

3. Typ een naam waarmee uw toepassing en het doel van deze oplossing (bijvoorbeeld "LitwareBatchTaskPrograms") wordt beschreven.

4. Klik op **OK**om de nieuwe oplossing.

## <a name="job-manager-template"></a>Taakbeheer-sjabloon

De sjabloon Taakbeheer helpt u bij het implementeren van een manager projecttaak die u kunt de volgende acties uitvoeren:

* Een taak splitsen in meerdere taken.
* Deze taken uit te voeren op Batch verzenden.

>[AZURE.NOTE] Zie [overzicht van de Batch-functie voor ontwikkelaars](batch-api-basics.md#job-manager-task)voor meer informatie over het beheren van projecttaken.

### <a name="create-a-job-manager-using-the-template"></a>De Manager van een taak met de sjabloon maken

Als u wilt de manager van een taak toevoegen aan de oplossing aan die u eerder hebt gemaakt, de volgende stappen uit:

1. Open uw bestaande oplossing in Visual Studio-2015.

2. Met de rechtermuisknop op de oplossing in Solution Explorer, klikt u op **toevoegen** > **Nieuw Project**.

3. Klik onder **Visual C#**, op **Cloud**, en klik vervolgens op **Taakbeheer van Azure Batch met taak splitsen**.

4. Typ een naam die wordt beschreven van uw toepassing en dit project geïdentificeerd als de beheerder van de taak (bijvoorbeeld "LitwareJobManager").

5. Klik op **OK**om het project.

6. Tot slot maken op het project om Visual Studio alle waarnaar wordt verwezen NuGet-pakketten laden en om te bevestigen dat het project geldig is voordat u begint met het wijzigen van deze afdwingen.

### <a name="job-manager-template-files-and-their-purpose"></a>Taakbeheer sjabloonbestanden en hun doel

Wanneer u een project met de sjabloon Taakbeheer maakt, genereert deze drie groepen codebestanden:

* De belangrijkste programmabestand (Program.cs). Dit bevat de ingangspunt programma en de afhandeling van op het hoogste niveau uitzonderingen. Niet mag moet u normaal gesproken dit wijzigen.

* De map Framework. Dit bevat de bestanden die verantwoordelijk is voor het 'standaardtekst' werk uitgevoerd door het programma taak manager – uitpakken parameters, taken toevoegen aan de batchtaak, enzovoort. Niet mag moet u normaal gesproken deze bestanden wijzigt.

* De taak splitser-bestand (JobSplitter.cs). Dit is waar u uw toepassingsspecifieke logica voor een taak splitsen in taken worden opgeslagen.

Uiteraard kunt u aanvullende bestanden zoals vereist ter ondersteuning van uw taak splitser-code, afhankelijk van de complexiteit van de taak splitsen logica toevoegen.

De sjabloon genereert ook standaard .NET-projectbestanden, zoals een .csproj-bestand, app.config, packages.config, enzovoort.

De rest van dit gedeelte worden de verschillende bestanden en hun codestructuur en wordt uitgelegd wat elke klasse doet.

![Visual Studio Solution Explorer met het Taakbeheer sjabloonoplossing][solution_explorer01]

**Framework bestanden**

* `Configuration.cs`: Omvat het laden van taak configuratiegegevens zoals Batch-accountgegevens, accountreferenties gekoppelde opslag, functie en taakgegevens en taakparameters. Het biedt ook toegang tot Batch gedefinieerde omgevingsvariabelen (Zie omgevingsinstellingen voor taken, in de documentatie Batch) via de klasse Configuration.EnvironmentVariable.

* `IConfiguration.cs`: Abstracts de uitvoering van de klasse configuratie, zodat u kunt testen van eenheden uw taak splitser met behulp van een configuratieobject valse of model.

* `JobManager.cs`: Orchestrates de onderdelen van het programma van de manager taak. Dit is verantwoordelijk voor de initialisatie van de taak splitser, aanroepen van de taak splitser, en het verzenden van de taken die zijn geretourneerd door de splitser taak naar de inzender taak.

* `JobManagerException.cs`: Hiermee geeft u een fout is van de manager van de taak te beëindigen. JobManagerException wordt gebruikt om te laten teruglopen 'verwachte' fouten waar specifieke diagnostische informatie kan worden verstrekt als onderdeel van opzegging.

* `TaskSubmitter.cs`: Deze klasse is verantwoordelijk voor het toevoegen van taken die zijn geretourneerd door de splitser taak naar de Batch-taak. De Taakbeheer heeft class aggregaties de volgorde van taken in batches voor efficiënte maar tijdig aanvulling op de taak, klikt u vervolgens oproepen TaskSubmitter.SubmitTasks op een achtergrondthread voor elke batch.

**Taak splitsen**

`JobSplitter.cs`: Deze klasse bevat toepassingsspecifieke logica voor de taak splitsen in taken. Het kader Hiermee wordt de methode JobSplitter.Split voor het verkrijgen van een reeks taken, zoals deze worden toegevoegd aan de taak wanneer ze geeft als de methode resultaat. Dit is de klasse waar u de logica van uw werk worden gevoegd. Implementeer de methode splitsen om terug te keren een reeks CloudTask exemplaren die de taken waar u naartoe wilt partitioneren van de taak vertegenwoordigt.

**Standaard projectbestanden voor .NET-opdrachtregel**

* `App.config`: Standaard .NET-toepassing-configuratiebestand.

* `Packages.config`: Standaard NuGet afhankelijkheid pakketbestand.

* `Program.cs`: Het programma ingangspunt en afhandeling van op het hoogste niveau uitzonderingen bevat.

### <a name="implementing-the-job-splitter"></a>De taak splitser implementeren

Wanneer u het sjabloonproject Taakbeheer opent, heeft het project het JobSplitter.cs bestand standaard geopend. U kunt de logica splitsen voor de taken in uw werkzaamheden implementeren met behulp van de onderstaande Split() methode weergeven:

```csharp
/// <summary>
/// Gets the tasks into which to split the job. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The job manager framework invokes the Split method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and the "yield return" statement; this allows tasks to be added
/// and to start running while splitting is still in progress.
/// </summary>
/// <returns>The tasks to be added to the job. Tasks are added automatically
/// by the job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for the split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

>[AZURE.NOTE] De geannoteerde sectie in de `Split()` methode is het alleen-lezen gedeelte van de taak Manager sjablooncode die is bedoeld voor u wijzigen door de logica toevoegen aan uw taken splitsen in verschillende taken. Als u wijzigen van een andere sectie van de sjabloon wilt, Controleer of u hoe de Batch werkt, zijn familiarized en probeer enkele van de [voorbeelden van de code Batch][github_samples].

Uw implementatie Split() heeft toegang tot:

* De taakparameters, via de `_parameters` veld.
* Het CloudJob-object dat staat voor de taak, via de `_job` veld.
* Het CloudTask-object dat staat voor de manager projecttaak, via de `_jobManagerTask` veld.

Uw `Split()` implementatie hoeft niet te taken rechtstreeks toevoegen aan de taak. In plaats daarvan een reeks CloudTask objecten in uw code moet worden geretourneerd en worden deze toegevoegd aan het project automatisch door de framework klassen die de splitser taak roepen. Dit geldt voor gebruik van C# herhaler (`yield return`) functie willen implementeren taak splitsbalken zoals in deze sectie de taken geeft voor het starten van zo snel mogelijk wordt uitgevoerd in plaats van wachten op alle taken moet worden berekend.

**Taak splitser is mislukt**

Als uw taak splitser een fout optreedt, moet deze op:

* De volgorde met de C# beëindigen `yield break` instructie, waarin hoofdletters/kleine letters de manager van de taak wordt verwerkt als geslaagd; of

* Genereert van een uitzondering, waarin de manager van de taak wordt verwerkt als hoofdletters/kleine letters is mislukt en mogelijk een nieuwe poging gedaan afhankelijk van hoe de client deze heeft geconfigureerd).

In beide gevallen alle taken die al die het resultaat van de taak splitser en toegevoegd aan de taak Batch komt in aanmerking om uit te voeren. Als u niet wilt dat dit gebeurt, klikt u kunt doen:

* De taak voordat u terugkeert uit de splitser taak beëindigen

* Het verzamelen van de gehele taak voordat deze wordt geretourneerd opstellen (dat wil zeggen retourneren een `ICollection<CloudTask>` of `IList<CloudTask>` in plaats van uw taak splitser met een C#-herhaler implementeren)

* Taakafhankelijkheden gebruiken om alle taken, hangt af van het afronden van de taak manager te starten

**Taak manager pogingen**

Als de manager taak mislukt, deze mogelijk opnieuw worden uitgevoerd door de service Batch afhankelijk van de client opnieuw-instellingen. In het algemeen, namelijk veilige, wanneer het kader taken aan de taak toevoegt, worden alle taken die al aanwezig genegeerd. Echter, als het berekenen van taken dure is, niet kunt u er de kosten van het opnieuw berekenen van taken die al zijn toegevoegd aan de taak. daarentegen als het opnieuw uitvoeren is niet gegarandeerd genereren van de dezelfde taak-id's wordt Ga het gedrag 'negeren duplicaten' niet lekker. In deze gevallen moet u uw taak splitsen om te bepalen van het werk dat al is uitgevoerd en niet wilt herhalen, bijvoorbeeld door een CloudJob.ListTasks in te voeren voordat u begint met het rendement van taken ontwerpen.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>Codes en uitzonderingen in de sjabloon Taakbeheer afsluiten

Afsluitcodes en uitzonderingen bieden een om te bepalen de uitkomst van de uitvoering van een programma en ze kunnen helpen problemen met de uitvoering van het programma. De sjabloon Taakbeheer implementeert de afsluitcodes en de uitzonderingen in deze sectie beschreven.

Een taak van de manager die wordt geïmplementeerd met de sjabloon Taakbeheer kan drie mogelijke afsluitcodes retourneren:

| Code | Beschrijving |
|------|-------------|
| 0    | De manager taak is voltooid. Uw taak splitser-code hebt uitgevoerd tot voltooiing en alle taken zijn toegevoegd aan de taak. |
| 1    | De manager projecttaak is mislukt met een uitzondering in een 'verwachte' deel van het programma. De uitzondering is omgezet in een JobManagerException met diagnostische informatie en, indien mogelijk, suggesties voor het oplossen van de fout. |
| 2    | De manager projecttaak is mislukt met een 'onverwachte' uitzondering. De uitzondering resultaat is vastgelegd, maar de taak manager kon eventuele aanvullende diagnostic of remediation gegevens toe te voegen. |

In het geval van een taak manager taak mislukt, sommige taken mogelijk nog steeds zijn toegevoegd aan de service voordat de fout is opgetreden. Deze taken uitgevoerd normaal. Zie "Taak splitser is mislukt" hierboven voor discussie van dit codepad.

Alle gegevens die het resultaat van uitzonderingen is geschreven in stdout.txt en stderr.txt-bestanden. Zie [Foutverwerking](batch-api-basics.md#error-handling)voor meer informatie.

### <a name="client-considerations"></a>Clientoverwegingen

Dit onderwerp vindt enkele vereisten voor de uitvoering van de client bij het aanroepen van de manager van een taak op basis van deze sjabloon. Lees [hoe u het doorgeven van parameters en variabelen van de clientcode](#pass-environment-settings) voor meer informatie over het doorgeven van parameters en omgevingsinstellingen.

**Verplicht referenties**

Om te kunnen taken toevoegen aan de taak Azure Batch, is de manager projecttaak vereist uw Azure Batch account URL en van toetsen. U moet deze in een omgevingsvariabelen met de naam YOUR_BATCH_URL en YOUR_BATCH_KEY doorgeven. U kunt deze in de taak Manager instellen omgevingsinstellingen taak. Bijvoorbeeld in een C#-client:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Opslag referenties**

De client hoeft meestal niet op te geven van de gekoppelde opslag-accountreferenties naar de manager projecttaak omdat (a) meest taak managers niet hoeft te expliciet toegang tot het account gekoppelde opslag en (b) het account gekoppelde opslag vaak aan alle taken is opgegeven als een algemene omgevingsinstelling voor de taak. Als u niet het account gekoppelde opslag via de algemene omgevingsinstellingen, en de taak manager is vereist voor toegang tot gekoppelde opslag, moet u opgeven de referenties gekoppelde opslag als volgt:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Taakinstellingen manager taak**

De client moet de markering taak manager *killJobOnCompletion* ingesteld op **Onwaar**.

Voor de client *runExclusive* ingesteld op **Onwaar**meestal veilig is.

De client moet de verzameling *resourceFiles* of *applicationPackageReferences* gebruiken om de taak manager uitvoerbare (en de vereiste DLL-bestanden) naar het knooppunt berekeningscluster geïmplementeerd.

Standaard de manager van de taak wordt niet opnieuw geprobeerd is mislukt. Afhankelijk van uw taak manager logica, de client wilt inschakelen pogingen via *beperkingen*/*maxTaskRetryCount*.

**Taakinstellingen**

Als de taak splitser taken met afhankelijkheden genereert, moet de client van de taak usesTaskDependencies ingesteld op waar.

In het model van taak splitsen is het ongebruikelijke clients wilt taken toevoegen aan taken naast wat de splitser taak wordt gemaakt. De client moet van de taak *onAllTasksComplete* daarom normaal gesproken ingesteld op **terminatejob**.

## <a name="task-processor-template"></a>Taak Processor sjabloon

Een sjabloon taak Processor helpt u bij het implementeren van de processor van een taak die u kunt de volgende acties uitvoeren:

* Stel de informatie die is vereist door elke batchtaak om uit te voeren.
* Alle acties die zijn vereist voor elke batchtaak uitvoeren.
* Sla de uitvoer van de taak op permanente opslag.

Hoewel de processor van een taak niet uitvoeren van taken op de Batch is vereist, is het belangrijk voordeel van het gebruik van een taak processor dat u een Wikkel implementatie van alle Taakacties worden uitgevoerd op één locatie. Bijvoorbeeld, taak als voor het uitvoeren van verschillende toepassingen in de context van elke taak of als u wilt kopiëren gegevens permanent opslaan na het voltooien van elke.

De acties uitgevoerd door de processor taak kunnen worden als eenvoudige of complexe en zoveel of weinig, zoals vereist door uw werkzaamheden. Daarnaast kunnen door alle Taakacties in één taak processor implementeren, kunt u gemakkelijk bijwerken of acties op basis van wijzigingen in toepassingen of werkbelasting vereisten toevoegen. Echter in sommige gevallen de processor van een taak mogelijk niet de optimale oplossing voor uw implementatie zoals deze onnodige complexiteit, bijvoorbeeld bij het uitvoeren van taken die snel kunnen worden gestart vanaf een eenvoudige opdrachtregel kunt toevoegen.

### <a name="create-a-task-processor-using-the-template"></a>Een taak Processor met de sjabloon maken

Als u wilt de processor van een taak toevoegen aan de oplossing aan die u eerder hebt gemaakt, de volgende stappen uit:

1. Open uw bestaande oplossing in Visual Studio-2015.

2. Met de rechtermuisknop op de oplossing in Solution Explorer, klikt u op **toevoegen**en klik vervolgens op **Nieuw Project**.

3. Klik onder **Visual C#**, op **Cloud**, en klik vervolgens op **Azure Batch taak Processor**.

4. Typ een naam die wordt beschreven van uw toepassing en dit project geïdentificeerd als de taak-processor (bijvoorbeeld "LitwareTaskProcessor").

5. Klik op **OK**om het project.

6. Tot slot maken op het project om Visual Studio alle waarnaar wordt verwezen NuGet-pakketten laden en om te bevestigen dat het project geldig is voordat u begint met het wijzigen van deze afdwingen.

### <a name="task-processor-template-files-and-their-purpose"></a>Taak Processor sjabloonbestanden en hun doel

Wanneer u een project met de sjabloon van de processor taak maakt, genereert deze drie groepen codebestanden:

* De belangrijkste programmabestand (Program.cs). Dit bevat de ingangspunt programma en de afhandeling van op het hoogste niveau uitzonderingen. Niet mag moet u normaal gesproken dit wijzigen.

* De map Framework. Dit bevat de bestanden die verantwoordelijk is voor het 'standaardtekst' werk uitgevoerd door het programma taak manager – uitpakken parameters, taken toevoegen aan de batchtaak, enzovoort. Niet mag moet u normaal gesproken deze bestanden wijzigt.

* Het bestand taak processor (TaskProcessor.cs). Dit is waar u uw toepassingsspecifieke logica voor het uitvoeren van een taak (meestal door te bellen naar een bestaande uitvoerbaar bestand) wordt opgeslagen. Vóór en na verwerking code in, zoals aanvullende gegevens downloaden of uploaden van bestanden met resultaten, ook hier.

Uiteraard kunt u aanvullende bestanden zoals vereist ter ondersteuning van uw taak processor-code, afhankelijk van de complexiteit van de taak splitsen logica toevoegen.

De sjabloon genereert ook standaard .NET-projectbestanden, zoals een .csproj-bestand, app.config, packages.config, enzovoort.

De rest van dit gedeelte worden de verschillende bestanden en hun codestructuur en wordt uitgelegd wat elke klasse doet.

![Visual Studio Solution Explorer met de oplossing taak Processor-sjabloon][solution_explorer02]

**Framework bestanden**

* `Configuration.cs`: Omvat het laden van taak configuratiegegevens zoals Batch-accountgegevens, accountreferenties gekoppelde opslag, functie en taakgegevens en taakparameters. Het biedt ook toegang tot Batch gedefinieerde omgevingsvariabelen (Zie omgevingsinstellingen voor taken, in de documentatie Batch) via de klasse Configuration.EnvironmentVariable.

* `IConfiguration.cs`: Abstracts de uitvoering van de klasse configuratie, zodat u kunt testen van eenheden uw taak splitser met behulp van een configuratieobject valse of model.

* `TaskProcessorException.cs`: Hiermee geeft u een fout is van de manager van de taak te beëindigen. TaskProcessorException wordt gebruikt om te laten teruglopen 'verwachte' fouten waar specifieke diagnostische informatie kan worden verstrekt als onderdeel van opzegging.

**Taak Processor**

* `TaskProcessor.cs`: De taak wordt uitgevoerd. Het kader roept de methode TaskProcessor.Run. Dit is de klasse waar u de logica toepassingsspecifieke van uw taak worden gevoegd. De methode uitvoeren om te implementeren:
  * Parseren en valideren van de taakparameters van een
  * De opdrachtregel voor alle externe programma dat u wilt aanroepen opstellen
  * Meld u alle gewenste voor foutopsporing mogelijk diagnostische gegevens
  * Een proces met die opdrachtregel starten
  * Wacht totdat het proces voor het afsluiten
  * De afsluitcode van het proces om te bepalen of deze is geslaagd of mislukt vastleggen
  * Sla alle uitvoerbestanden die u wilt behouden permanent opslaan

**Standaard projectbestanden voor .NET-opdrachtregel**

* `App.config`: Standaard .NET-toepassing-configuratiebestand.
* `Packages.config`: Standaard NuGet afhankelijkheid pakketbestand.
* `Program.cs`: Het programma ingangspunt en afhandeling van op het hoogste niveau uitzonderingen bevat.

## <a name="implementing-the-task-processor"></a>Uitvoering van de taak-processor

Wanneer u de taak Processor sjabloonproject opent, wordt het TaskProcessor.cs bestand standaard geopend in het project hebben. U kunt de logica uitvoeren voor de taken in uw werkzaamheden implementeren met behulp van de methode Run() hieronder wordt weergegeven:

```csharp
/// <summary>
/// Runs the task processing logic. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The task processor framework invokes the Run method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check the exit code of that program and
/// save output files to persistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for the task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
>[AZURE.NOTE] De geannoteerde sectie in de methode Run() is het alleen-lezen gedeelte van de taak Processor sjabloon-code die is bedoeld om te wijzigen door het toevoegen van de logica uitvoeren voor de taken in uw werkzaamheden. Als u wijzigen van een andere sectie van de sjabloon wilt, Neem eerst vertrouwd raken met de werking van Batch door te controleren van de Batch documentatie en een aantal voorbeelden van de Batch-code uitprobeert.

De methode Run() is verantwoordelijk voor het starten van de opdrachtregel starten van een of meer processen, wacht alle proces hebt voltooid, de resultaten op te slaan en ten slotte met een code te retourneren. De methode Run() is waar u de logica verwerking implementeren voor uw taken. Het kader van de processor taak roept de methode Run() voor u; u hoeft niet uzelf bellen.

Uw implementatie Run() heeft toegang tot:

* De taakparameters, via de `_parameters` veld.
* De taak en taak-id's via de `_jobId` en `_taskId` velden.
* De configuratie van de taak moeten via de `_configuration` veld.

**Taak is mislukt**

Voor het geval is mislukt, kunt u de methode Run() afsluiten door uitzondering opgetreden, maar dit het bovenste niveau uitzonderingshandler controle over de taak afsluitcode verlaat. Als u nodig hebt om te bepalen de afsluitcode, zodat u onderscheid maken verschillende soorten mislukt, bijvoorbeeld voor diagnose tussen kunt of omdat sommige modi mislukt de taak moeten worden beëindigd en anderen niet moeten, moet u de methode Run() afsluiten door te retourneren van een afsluitcode van niet-nul. Dit wordt de taak afsluitcode.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>Codes en uitzonderingen in de sjabloon taak Processor afsluiten

Afsluitcodes en uitzonderingen bieden een om te bepalen de uitkomst van de uitvoering van een programma en ze kunnen helpen problemen met de uitvoering van het programma. De sjabloon taak Processor implementeert de afsluitcodes en de uitzonderingen in deze sectie beschreven.

Een taak van de processor die wordt geïmplementeerd met de sjabloon taak Processor kan drie mogelijke afsluitcodes retourneren:

| Code | Beschrijving |
|------|-------------|
|  [Process.ExitCode][process_exitcode] | De taak processor tot voltooiing hebt uitgevoerd. Opmerking: dit niet betekent dat het programma dat u aangeroepen, alleen geslaagd is – dat de taak processor deze is geactiveerd en eventuele na verwerking zonder uitzonderingen uitgevoerd. De betekenis van de afsluitcode, is afhankelijk van het geactiveerde programma: meestal afsluitcode 0 betekent dat het programma is voltooid en eventuele andere afsluitcode betekent dat het programma is mislukt. |
| 1    | De taak processor is mislukt met een uitzondering in een 'verwachte' deel van het programma. De uitzondering is omgezet in een `TaskProcessorException` met diagnostische informatie en, indien mogelijk, suggesties voor het oplossen van de fout. |
| 2    | De taak processor is mislukt met een 'onverwachte' uitzondering. De uitzondering resultaat is vastgelegd, maar de processor taak kan niet eventuele aanvullende diagnostic of remediation gegevens toe te voegen. |

>[AZURE.NOTE] Als het programma aanroepen afsluitcodes 1 en 2 gebruikt om aan te geven van specifieke modi, is klikt u vervolgens met behulp van afsluitcodes 1 en 2 voor de taak processor fouten niet-eenduidige. U kunt deze taak processor-foutcodes opvallend afsluiten codes wijzigen door de uitzondering aanvragen in het bestand Program.cs bewerken.

Alle gegevens die het resultaat van uitzonderingen is geschreven in stdout.txt en stderr.txt-bestanden. Zie foutverwerking, in de Batch-documentatie voor meer informatie.

### <a name="client-considerations"></a>Clientoverwegingen

**Opslag referenties**

Als de taak processor gebruikmaakt van Azure-blobopslag persistent uitvoer, bijvoorbeeld met behulp van het bestand conventies helper bibliotheek, klikt u vervolgens moet deze toegang tot *beide* de cloud opslag account referenties *of* de URL van een blob container met een gedeelde access-handtekening (SA's). De sjabloon biedt ondersteuning voor het leveren van referenties via algemene omgevingsvariabelen. De klant kunt doorgeven de referenties voor de opslag als volgt:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

Het account opslag is beschikbaar in de klas TaskProcessor via de `_configuration.StorageAccount` eigenschap.

Als u liever het gebruik van de URL van een container met SA's, u kunt dit ook doorgeven via een taak algemene omgeving-instelling, maar de taak processor sjabloon bevat momenteel geen ingebouwde ondersteuning voor deze.

**Setup opslag**

Het wordt aanbevolen dat de taak van de manager-client of taak een containers die is vereist door taken voordat u de taken toevoegt aan de taak maken. Dit is verplicht dat als u de URL van een container met SA's gebruikt, als zodanig een URL bevat geen machtigingen voor het maken van de container. Het verdient zelfs als u opslag-accountreferenties, zoals elke taak dat u moet bellen CloudBlobContainer.CreateIfNotExistsAsync op de container worden opgeslagen.

## <a name="pass-parameters-and-environment-variables"></a>Parameters en variabelen doorgeven

### <a name="pass-environment-settings"></a>Omgevingsinstellingen doorgeven

Een client kunt informatie doorgeven aan de taak van de manager in de vorm van omgevingsinstellingen. Deze informatie kan vervolgens worden gebruikt door de manager projecttaak bij het genereren van de taak processor taken die wordt uitgevoerd als onderdeel van de taak berekeningscluster. Voorbeelden van de gegevens die u als omgevingsinstellingen doorgeven kunt zijn:

* Opslag account naam- en -account te gebruiken
* URL van de batch-account
* Batch accountsleutel

De Batch-service heeft een eenvoudige om omgevingsinstellingen doorgeven aan een taak van de manager definiëren met behulp van de `EnvironmentSettings` eigenschap in [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Als u bijvoorbeeld om de `BatchClient` exemplaar voor een Batch-account, kunt u doorgeven zoals omgevingsvariabelen vanuit de client de URL en gedeelde belangrijke referenties voor de Batch-account code. Ook toegang tot het opslag-account dat is gekoppeld aan de Batch-account, kunt u de naam van het opslag-account en doorgeven de accountsleutel opslag als omgevingsvariabelen.

### <a name="pass-parameters-to-the-job-manager-template"></a>Parameters doorgeven aan de taak Manager-sjabloon

In veel gevallen is het handig per taak parameters doorgeven aan de manager projecttaak, om te bepalen de taak splitsen proces of voor het configureren van de taken voor de taak. U kunt dit doen door het uploaden van een JSON-bestand met de naam parameters.json als bronbestand voor de manager projecttaak. De parameters kunnen dan beschikbaar in de `JobSplitter._parameters` veld in de sjabloon Taakbeheer.

>[AZURE.NOTE] De ingebouwde parameter-handler ondersteunt alleen tekenreeks-naar-tekenreeks woordenlijsten. Als u complexe JSON waarden doorgeven als parameterwaarden wilt, moet u deze als tekenreeksen doorgeven en deze in de splitser taak parseert of het wijzigen van het kader `Configuration.GetJobParameters` methode.

### <a name="pass-parameters-to-the-task-processor-template"></a>Parameters doorgeven aan de taak Processor-sjabloon

U kunt ook parameters doorgeven aan individuele taken die zijn geïmplementeerd met behulp van de taak Processor-sjabloon. Net als met de manager taaksjablonen, de taak processor sjabloon Hiermee wordt gezocht naar een resource-bestand met de naam

parameters.JSON, en als deze gevonden laadt als de woordenlijst parameters. Er zijn een paar van opties voor de realisatie parameters doorgeven aan de taak processor taken:

* De taakparameters JSON opnieuw. Dit werkt ook als u de enige parameters taak hele zijn (voor bijvoorbeeld een render hoogte en breedte). Dit doet, bij het maken van een CloudTask in de splitser taak toevoegen een verwijzing naar het object parameters.json resource bestand uit van de projecttaak manager ResourceFiles (`JobSplitter._jobManagerTask.ResourceFiles`) aan van de CloudTask ResourceFiles verzameling.

* Verwijzen naar die blob in van de taak resource bestanden verzameling genereren en een taak / regiospecifieke parameters.json-document uploaden als onderdeel van de taak splitser worden uitgevoerd. Dit is nodig hebt van verschillende taken op verschillende parameters. Een voorbeeld mogelijk een 3D-weergave scenario waar het frame-index wordt doorgegeven aan de taak als een parameter.

>[AZURE.NOTE] De ingebouwde parameter-handler ondersteunt alleen tekenreeks-naar-tekenreeks woordenlijsten. Als u complexe JSON waarden doorgeven als parameterwaarden wilt, moet u deze als tekenreeksen doorgeven en deze in de taak processor parseert of het wijzigen van het kader `Configuration.GetTaskParameters` methode.

## <a name="next-steps"></a>Volgende stappen

### <a name="persist-job-and-task-output-to-azure-storage"></a>Persistent project en uitvoer met Azure Storage

Een ander handig hulpmiddel in de ontwikkelingsfase bevindt Batch-oplossing is [Azure Batch bestand conventies][nuget_package]. Gebruik deze klassenbibliotheek .NET (die momenteel in preview) in de Batch .NET-toepassingen eenvoudig opslaan en uitvoer van de taak en naar Azure Storage ophalen. [Persistent Azure Batch project en uitvoer](batch-task-output.md) bevat een gedetailleerde beschrijving van de bibliotheek en het gebruik ervan.

### <a name="batch-forum"></a>Batch-Forum

Het [Forum van Azure Batch] [ forum] op MSDN is een prima plek om te bespreken Batch en vragen over de service. Hoofd op via voor handig "Plaktoetsen" berichten, vragen en posten voor uw dat ze zich voordoen terwijl u uw Batch oplossingen bouwen.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
