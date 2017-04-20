<properties
   pageTitle="Gebruik van prestatiemeteritems in Azure diagnostische hulpprogramma's | Microsoft Azure"
   description="Gebruik prestatie-items in Azure cloudservices of VM knelpunten zoeken en het afstemmen van prestaties."
   services="cloud-services"
   documentationCenter=".net"
   authors="rboucher"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/29/2016"
   ms.author="robb" />

# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Maken en gebruiken van items in een Azure-toepassing

In dit artikel worden de voordelen van het en hoe u items in uw Azure toepassing plaatsen. U kunt deze gegevens verzamelen, knelpunten zoeken en systeemprestaties en de toepassingen af te stellen.

Van prestatiemeteritems die beschikbaar zijn voor Windows Server, IIS en ASP.NET kunnen ook worden verzameld en gebruikt om te bepalen de status van uw Azure web rollen, werknemer rollen en virtuele Machines. U kunt ook maken en gebruiken van aangepaste prestatie-items.  

U kunt de prestatiemeteritemgegevens controleren
1. Rechtstreeks op de toepassinghost met de prestaties Monitor hulpmiddel geopend met extern bureaublad
2. Gebruik van de Azure Management Pack met System Center Operations Manager
3. Andere controleren hulpmiddelen die u toegang de diagnostische gegevens tot worden overgedragen naar Azure opslag. Zie [Store en diagnostische gegevens van de weergave in Azure-opslag](https://msdn.microsoft.com/library/azure/hh411534.aspx) voor meer informatie.  

Zie [Monitor Cloud Services](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/)voor meer informatie over het controleren van de prestaties van uw toepassing in de [klassieke Azure-portal](http://manage.azure.com/).

Zie voor meer gedetailleerde informatie over het maken van een logboekregistratie en strategie aanwijzen en het gebruik van diagnostische hulpprogramma's en andere technieken problemen oplost en Azure toepassingen optimaliseren, [Aanbevolen procedures voor het ontwikkelen van Azure toepassingen probleemoplossing](https://msdn.microsoft.com/library/azure/hh771389.aspx).


## <a name="enable-performance-counter-monitoring"></a>Prestatiecontroles uit item inschakelen

Prestatie-items worden niet al dan niet standaard ingeschakeld. Uw toepassing of een taak opstarten moet de standaard diagnostisch hulpprogramma agentconfiguratie als u wilt opnemen van de specifieke items die u wilt controleren voor elk exemplaar van de rol wijzigen.

### <a name="performance-counters-available-for-microsoft-azure"></a>Van prestatiemeteritems die beschikbaar zijn voor Microsoft Azure

Azure biedt een subset van de van prestatiemeteritems die beschikbaar zijn voor Windows Server, IIS en de stapel ASP.NET. De volgende tabel worden enkele van de items van bepaalde rente voor Azure-toepassingen.

|Item categorie: Object (exemplaar)|De Tellernaam van de      |Verwijzing|
|---|---|---|
|.NET CLR uitzonderingen (_globale_)|# Uitzonderingen gegenereerd / sec   |Uitzondering prestatie-items|
|.NET CLR geheugen (_globale_)    |Percentage tijd in ° c            |Geheugen prestatie-items|
|ASP.NET                      |Start van toepassing    |Prestatie-items voor ASP.NET|
|ASP.NET                      |Verwerkingstijd aanvragen  |Prestatie-items voor ASP.NET|
|ASP.NET                      |Verbroken aanvragen   |Prestatie-items voor ASP.NET|
|ASP.NET                      |Werknemer proces opnieuw is opgestart |Prestatie-items voor ASP.NET|
|ASP.NET-toepassingen (__totale__)|Totaal aantal aanvragen        |Prestatie-items voor ASP.NET|
|ASP.NET-toepassingen (__totale__)|Aanvragen per seconde          |Prestatie-items voor ASP.NET|
|ASP.NET-v4.0.30319           |Verwerkingstijd aanvragen  |Prestatie-items voor ASP.NET|
|ASP.NET-v4.0.30319           |Wachttijd aanvragen       |Prestatie-items voor ASP.NET|
|ASP.NET-v4.0.30319           |Huidige aanvragen        |Prestatie-items voor ASP.NET|
|ASP.NET-v4.0.30319           |Aanvragen in wachtrij         |Prestatie-items voor ASP.NET|
|ASP.NET-v4.0.30319           |Geweigerde aanvragen       |Prestatie-items voor ASP.NET|
|Geheugen                       |Beschikbare megabytes (MB)        |Geheugen prestatie-items|
|Geheugen                       |Toegewezen Bytes         |Geheugen prestatie-items|
|Processor(_Totaal)            |% Processortijd        |Prestatie-items voor ASP.NET|
|TCPv4                        |Verbindingsfouten     |TCP-Object|
|TCPv4                        |Verbindingen tot stand gebracht |TCP-Object|
|TCPv4                        |Verbindingen opnieuw instellen       |TCP-Object|
|TCPv4                        |Segmenten verzonden per seconde       |TCP-Object|
|Netwerk Interface(*)         |Ontvangen/per seconde      |Object in de gebruikersinterface|
|Netwerk Interface(*)         |Verzonden bytes per seconde          |Object in de gebruikersinterface|
|Netwerk Interface (Microsoft VM Bus netwerk Adapter _2)|Ontvangen/per seconde|Object in de gebruikersinterface|
|Netwerk Interface (Microsoft VM Bus netwerk Adapter _2)|Verzonden bytes per seconde|Object in de gebruikersinterface|
|Netwerk Interface (Microsoft VM Bus netwerk Adapter _2)|Totaal aantal bytes/seconde|Object in de gebruikersinterface|

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Maken en aangepaste prestatiemeteritems toevoegen aan uw toepassing

Azure heeft ondersteuning maken en aangepaste prestatie-items voor web rollen en werknemer rollen wijzigen. De items kunnen worden gebruikt voor het bijhouden en toepassingsspecifieke gedrag controleren. U kunt maken en aangepaste prestatiemeteritem categorieën en aanduidingen verwijderen van een taak voor opstarten, Webrol of werknemer rol met verhoogde machtigingen.

>[AZURE.NOTE] Code die wijzigingen aanbrengt in aangepaste prestatiemeteritems moet veel rechten om uit te voeren. Als de code zich in een Webrol of de rol van de werknemer, moet de rol de tag bevatten <Runtime executionContext="elevated" /> in het bestand ServiceDefinition.csdef voor de rol juist geïnitialiseerd.

U kunt aangepaste prestatiemeteritemgegevens verzenden met Azure storage de hulpprogramma's voor diagnose-agent gebruiken.

De prestaties van de standaard-itemgegevens wordt gegenereerd door de Azure processen. Aangepaste prestatiemeteritemgegevens moet worden gemaakt door uw rol of werknemer rol webtoepassing. Zie [Prestaties itemtypen](https://msdn.microsoft.com/library/z573042h.aspx) voor informatie over de soorten gegevens die kunnen worden opgeslagen in de aangepaste prestatie-items. Zie [PerformanceCounters voorbeeld](http://code.msdn.microsoft.com/azure/) voor een voorbeeld dat wordt gemaakt en Hiermee stelt u aangepaste prestatiemeteritemgegevens in een Webrol.

## <a name="store-and-view-performance-counter-data"></a>Opslaan en weergave prestaties-itemgegevens

Azure slaat prestatiemeteritemgegevens met andere diagnostische informatie. Deze gegevens zijn beschikbaar voor externe controle terwijl het exemplaar van de rol actief is toegang tot extern bureaublad met hulpprogramma's zoals Performance Monitor weergeven. Als u wilt de gegevens buiten het exemplaar van de rol persistent, moet de hulpprogramma's voor diagnose-agent de gegevens overbrengen naar Azure opslag. De maximale grootte van de gegevens in de cache prestaties kan worden geconfigureerd in de diagnostische hulpprogramma's-agent of het is mogelijk geconfigureerd als onderdeel van een gedeelde limiet voor alle diagnostische gegevens. Zie voor meer informatie over het instellen van de grootte van de [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) en [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Zie [Store en diagnostische gegevens van de weergave in Azure-opslag](https://msdn.microsoft.com/library/azure/hh411534.aspx) voor een overzicht van de hulpprogramma's voor diagnose-agent instellen voor het overbrengen van gegevens naar een opslag-account.

Elk exemplaar van de teller geconfigureerde prestaties is vastgelegd met een opgegeven steekproeven snelheid en de steekproef gegevens overgebracht naar de opslag-account door een verzoek om geplande bestandsoverdracht of een aanvraag op aanvraag. Automatische overdrachten mogelijk gepland zo vaak als eenmaal per minuut. Prestatiemeteritemgegevens worden overgedragen door de diagnostische hulpprogramma's-agent is opgeslagen in een tabel, WADPerformanceCountersTable, in de opslagruimte-account. In deze tabel kan worden geopend en een query wordt uitgevoerd met standaard Azure API opslagmethoden. Zie [Microsoft Azure PerformanceCounters voorbeeld](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) op een voorbeeld van query's uitvoeren en het weergeven van prestatiemeteritemgegevens uit de tabel WADPerformanceCountersTable.

>[AZURE.NOTE] Afhankelijk van het hulpprogramma's voor diagnose agent doorverbinden frequentie en wachtrij latentie zijn de meest recente prestatiemeteritemgegevens in het account opslag mogelijk enkele minuten verouderd.

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Prestatiemeteritems met hulpprogramma's voor diagnose configuratiebestand inschakelen

Gebruik de volgende procedure prestatiemeteritems in uw Azure toepassing inschakelen.

## <a name="prerequisites"></a>Vereisten voor

In deze sectie wordt ervan uitgegaan dat u hebt geïmporteerd van het beeldscherm diagnostische gegevens in uw toepassing en het configuratiebestand diagnostische hulpprogramma's toegevoegd aan uw Visual Studio-oplossing (diagnostics.wadcfg in SDK 2,4 en hieronder of diagnostics.wadcfgx in SDK 2,5 en hoger). Zie stap 1 en 2 in [Diagnostische hulpprogramma's inschakelen in Azure Cloud Services en virtuele Machines](./cloud-services-dotnet-diagnostics.md)) voor meer informatie.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Stap 1: Verzamelen en bewaren van gegevens uit de prestatie-items

Nadat u het bestand diagnostische hulpprogramma's hebt toegevoegd aan uw Visual Studio-oplossing kunt u de siteverzameling en de opslag van prestatiemeteritemgegevens in een Azure-toepassing. Hiermee wordt uitgevoerd door de prestatiemeteritems toevoegen aan het bestand diagnostische gegevens. Diagnostische hulpprogramma's worden, met inbegrip van prestatiemeteritems, eerst gegevens op het exemplaar. De gegevens permanent wordt vervolgens opgeslagen in de tabel WADPerformanceCountersTable in de tabel Azure-service, zodat u ook moet opgeven van het account opslagruimte in uw toepassing. Als u uw toepassing lokaal in de Emulator berekenen testen bent, kunt u diagnostische gegevens ook lokaal opslaan in de Emulator opslag. Voordat u diagnostische gegevens opslaan moet u eerst gaat u naar de [klassieke Azure-portal](http://manage.windowsazure.com/) en een opslag-account maken. Er is een goede gewoonte om te zoeken van uw account opslagruimte in dezelfde geografische-locatie als uw Azure-toepassing om te voorkomen betaalt kosten voor externe bandbreedte en latentie verkleinen.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Items toevoegen aan het bestand diagnostische gegevens

Zijn er veel items die u kunt gebruiken. Het volgende voorbeeld ziet u diverse prestatiemeteritems die worden aanbevolen voor het web en werknemer rol bewaken.

Open het bestand diagnostische gegevens (diagnostics.wadcfg in SDK 2,4 en hieronder of diagnostics.wadcfgx in SDK 2,5 en hoger) en de volgende manieren te werk toevoegen aan het element DiagnosticMonitorConfiguration:

```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
       <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
    </PerformanceCounters>
```

Het kenmerk bufferQuotaInMB Hiermee geeft u de maximale hoeveelheid systeem bestandsopslag die beschikbaar is voor het gegevenstype voor siteverzameling (Azure Logboeken, IIS-logboeken, enzovoort). De standaardinstelling is aan 0. Wanneer het quotum is bereikt, wordt de oudste gegevens verwijderd als nieuwe gegevens worden toegevoegd. De som van alle bufferQuotaInMB eigenschappen moet groter zijn dan de waarde van het kenmerk OverallQuotaInMB. Zie voor meer informatie over om te bepalen hoeveel opslagruimte moeten worden uitgevoerd voor het verzamelen van gegevens voor diagnostische hulpprogramma's, de sectie Setup af van de [Aanbevolen procedures voor het ontwikkelen van Azure toepassingen probleemoplossing](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).

Het kenmerk scheduledTransferPeriod, dat het interval tussen geplande overdrachten van gegevens, naar boven afgerond op het dichtstbijzijnde minuten. In de volgende voorbeelden is deze ingesteld op PT30M (30 minuten). Als u de periode doorverbinden op een kleine waarde, zoals 1 minuut, wordt negatieve invloed hebben op prestaties van de toepassing in productie, maar zijn handig voor ziet diagnostische gegevens snel werken wanneer u wilt testen. De periode geplande doorverbinden moet klein is om ervoor te zorgen dat diagnostische gegevens niet overschreven van het exemplaar, maar groot genoeg is worden dat de prestaties van uw toepassing niet van invloed zijn.

Het kenmerk counterSpecifier Hiermee geeft u de prestatie-item om te verzamelen. Het kenmerk sampleRate geeft het tarief weer waarop de prestatie-item moet worden partijen, in dit geval 30 seconden.

Nadat u de items die u wilt verzamelen hebt toegevoegd, kunt u uw wijzigingen opslaan op het bestand diagnostische gegevens. Vervolgens moet u het opslag-account dat de gegevens diagnostisch hulpprogramma gehandhaafd voor opgeven.

### <a name="specify-the-storage-account"></a>Geef de opslag-account

Als u wilt uw informatie diagnostische gegevens bij uw account Azure Storage behouden, moet u een verbindingsreeks in het configuratiebestand van service (ServiceConfiguration.cscfg).

Voor Azure SDK 2,5 kan het opslag-Account in het bestand diagnostics.wadcfgx worden opgegeven.

>[AZURE.NOTE] Deze instructies zijn alleen van toepassing op Azure SDK 2,4 en onder. Voor Azure SDK 2,5 kan het opslag-Account in het bestand diagnostics.wadcfgx worden opgegeven.

De verbindingstekenreeksen instellen:

1. Open het ServiceConfiguration.Cloud.cscfg-bestand met uw favoriete teksteditor en stel de verbindingsreeks voor uw opslag. De waarden *accountnaam* en *AccountKey* vindt u in de portal voor Azure klassieke in het dashboard van het account opslag, onder sleutels beheren.

    ```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Sla het bestand ServiceConfiguration.Cloud.cscfg.

3. Open het bestand ServiceConfiguration.Local.cscfg en controleer of UseDevelopmentStorage is ingesteld op waar.

    ```
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
Nu dat de verbindingstekenreeksen zijn ingesteld, ook uw toepassing naar diagnostische gegevens in uw opslag-account gebruikt wanneer u uw toepassing wordt geïmplementeerd.
4. Opslaan en het project bouwen en implementeren van uw toepassing.

## <a name="step-2-optional-create-custom-performance-counters"></a>Stap 2: (Optioneel) maken aangepaste prestatie-items

Naast de vooraf gedefinieerde items, kunt u uw eigen aangepaste prestatiemeteritems om te controleren van Internet of een werknemer rollen toevoegen. Aangepaste prestatiemeteritems kunnen worden gebruikt voor het bijhouden en toepassingsspecifieke gedrag bewaken en kan worden gemaakt of verwijderd in een taak voor opstarten, Webrol of werknemer rol met verhoogde machtigingen.

De diagnostische gegevens van Azure-agent Hiermee vernieuwt u de prestatiemeteritem configuratie uit het bestand .wadcfg een minuut nadat u hebt gestart.  Als u aangepaste prestatie-items in de methode OnStart maken en uw opstarttaken langer duren dan een minuut uit te voeren, worden uw aangepaste prestatie-items niet hebt gemaakt wanneer de diagnostisch hulpprogramma Azure-agent probeert te laden.  In dit scenario ziet u dat diagnostisch hulpprogramma Azure juist wordt vastgelegd alle gegevens worden diagnostische hulpprogramma's, behalve van uw aangepaste prestatie-items.  U lost dit probleem op door het maken van de items in een taak opstarten of verplaats enkele van de taak opstarten werken met de methode OnStart na het maken van de items.

De volgende stappen als u wilt maken van een eenvoudige aangepaste prestaties teller met de naam "\MyCustomCounterCategory\MyButton1Counter" uitvoeren:

1. Open de service-definitiebestand (CSDEF) voor uw toepassing.
2. Hiermee voegt u de Runtime element dat moet worden de WebRole of WorkerRole element dat moet worden uitgevoerd met verhoogde bevoegdheden:

    ```
    <runtime executioncontext="elevated"/>
    ```
3. Sla het bestand.
4. Open het bestand diagnostische gegevens (diagnostics.wadcfg in SDK 2,4 en hieronder of diagnostics.wadcfgx in SDK 2,5 en hoger) en voeg de volgende toe aan de DiagnosticMonitorConfiguration 

    ```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
     <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Sla het bestand.
6. Maak de categorie van de teller aangepaste prestaties in de methode OnStart van uw rol, voordat base. OnStart. Het volgende C#-voorbeeld wordt een aangepaste categorie, als deze nog niet bestaat:

    ```
    public override bool OnStart()
    {
    if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
    {
       CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

       // add a counter tracking user button1 clicks
       CounterCreationData operationTotal1 = new CounterCreationData();
       operationTotal1.CounterName = "MyButton1Counter";
       operationTotal1.CounterHelp = "My Custom Counter for Button1";
       operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
       counterCollection.Add(operationTotal1);

       PerformanceCounterCategory.Create(
         "MyCustomCounterCategory",
         "My Custom Counter Category",
         PerformanceCounterCategoryType.SingleInstance, counterCollection);

       Trace.WriteLine("Custom counter category created.");
    }
    else{
       Trace.WriteLine("Custom counter category already exists.");
    }

    return base.OnStart();
    }
    ```
7. Werk de items binnen de toepassing. Het volgende voorbeeld bijgewerkt een aangepaste prestatie-item op Button1_Click gebeurtenissen:

    ```
    protected void Button1_Click(object sender, EventArgs e)
        {
         PerformanceCounter button1Counter = new PerformanceCounter(
           "MyCustomCounterCategory",
           "MyButton1Counter",
           string.Empty,
           false);
         button1Counter.Increment();
        this.Button1.Text = "Button 1 count: " +
           button1Counter.RawValue.ToString();
        }
    ```
8. Sla het bestand.  

Aangepaste prestatiemeteritemgegevens worden nu verzameld door de monitor Azure diagnostische gegevens.

## <a name="step-3-query-performance-counter-data"></a>Stap 3: Query prestaties-itemgegevens

Nadat uw toepassing wordt geïmplementeerd en uitgevoerd het beeldscherm diagnostisch hulpprogramma begint prestatiemeteritems verzamelen en persistent maken die gegevens met Azure storage. U kunt hulpprogramma's zoals Server Explorer in Visual Studio, [Azure opslag Explorer](http://azurestorageexplorer.codeplex.com/)of [Azure diagnostisch hulpprogramma Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) door Cerebrata de prestatiegegevens items weergeven in de tabel WADPerformanceCountersTable. U kunt ook via programmacode de service van de tabel met [C#](../storage/storage-dotnet-how-to-use-tables.d) [Java](../storage/storage-java-how-to-use-table-storage.md), [Node.js](../storage/storage-nodejs-how-to-use-table-storage.md), [Python](../storage/storage-python-how-to-use-table-storage.md), [Ruby](../storage/storage-ruby-how-to-use-table-storage.md)of [PHP](../storage/storage-php-how-to-use-table-storage.md)zoeken.

Het volgende C#-voorbeeld ziet u een eenvoudige query ten opzichte van de tabel WADPerformanceCountersTable en slaat de gegevens diagnostische gegevens naar een CSV-bestand. Zodra de items worden opgeslagen in een CSV-bestand kunt u de grafische functies in Microsoft Excel of enkele andere hulpmiddel de gegevens kunt visualiseren. Zorg ervoor dat u een verwijzing naar Microsoft.WindowsAzure.Storage.dll, toevoegen die is opgenomen in de SDK Azure voor .NET oktober 2012 en hoger. De vergadering is naar de map % programma Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ geïnstalleerd.

```
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ...

    // Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
    // you store your connection string using the Microsoft Azure service configuration
    // system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
    // to retrieve your storage connection string.  If you're not using Cloud Services, it's
    // recommended that you store the connection string in your web.config or app.config file.
    // Use the ConfigurationManager type to retrieve your storage connection string.

    string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
    //string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

    // Get a reference to the storage account using the connection string.  You can also use the development
    // storage account (Storage Emulator) for local debugging.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    //CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
    CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

    // Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
    TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
       .Where(
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
          TableOperators.And,
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
          TableOperators.And,
          TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
          )
       )
    );

    // Execute the table query.
    IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

    // Process the query results and build a CSV file.
    StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

    foreach (PerformanceCountersEntity entity in result)
    {
       sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
          + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
    }

    StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
    sw.Write(sb.ToString());
    sw.Close();
```

Entiteiten toewijzen aan C#-objecten met behulp van een aangepaste klasse afgeleid van **TableEntity**. De volgende code definieert een entity-klasse die een prestatie-item in de tabel **WADPerformanceCountersTable** vertegenwoordigt.


    public class PerformanceCountersEntity : TableEntity
    {
       public long EventTickCount { get; set; }
       public string DeploymentId { get; set; }
       public string Role { get; set; }
       public string RoleInstance { get; set; }
       public string CounterName { get; set; }
       public double CounterValue { get; set; }
    }


## <a name="next-steps"></a>Volgende stappen
[Extra artikelen voor de weergave op diagnostisch hulpprogramma Azure] (.. / azure-diagnostics.md)
