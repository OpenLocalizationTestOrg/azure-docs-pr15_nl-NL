<properties
    pageTitle="Inschakelen van opslageenheden in de Portal Azure | Microsoft Azure"
    description="Het inschakelen van opslageenheden voor de services Blob, wachtrij tabel en bestand"
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a>Azure opslageenheden inschakelen en weergeven van gegevens aan de doelstellingen

[AZURE.INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Overzicht

Standaard is de metrische opslaggegevens niet ingeschakeld voor uw opslagservices. U kunt inschakelen via de [Portal van Azure](https://portal.azure.com) of Windows PowerShell of via een programma via de bibliotheek van de client opslag cmdlets voor controle.

Wanneer u metrische opslaggegevens inschakelt, moet u een bewaarperiode voor de gegevens: deze periode bepaalt voor hoe lang de opslag service blijft de aan de doelstellingen en de kosten die u voor de afstand vereist om ze hebt opgeslagen. Meestal, moet u een kortere bewaarperiode minuut doelstellingen dan per uur aan de doelstellingen vanwege de aanzienlijk extra ruimte vereist minuut aan de doelstellingen. U kunt een bewaarperiode moet kiezen, zodat er voldoende tijd om de gegevens analyseren en u wilt behouden voor offline analyse of rapportage parameters downloaden. Vergeet niet dat u wordt ook gefactureerd voor het downloaden van de doelstellingen gegevens uit uw opslag-account.

## <a name="how-to-enable-metrics-using-the-azure-portal"></a>Het inschakelen van de doelstellingen met behulp van de Azure-Portal

Volg deze stappen om de doelstellingen in de [Portal van Azure](https://portal.azure.com)inschakelen:

1. Ga naar uw account opslag.
1. Open het blad **Instellingen** en selecteer van **Diagnostische gegevens**.
1. Zorg ervoor dat **Status** is ingesteld **op**.
1. Selecteer de doelstellingen voor de services die u wilt controleren.
2. Geef een bewaarbeleid om aan te geven hoe lang de doelstellingen behouden en gegevens vastleggen.

Houd er rekening mee dat de [Portal van Azure](https://portal.azure.com) momenteel laten voor het configureren van de doelstellingen van de minuut in uw account opslag; moet u aan de minuut doelstellingen via PowerShell inschakelen of via een programma.

## <a name="how-to-enable-metrics-using-powershell"></a>Het inschakelen van de doelstellingen via PowerShell

U kunt PowerShell gebruiken op uw lokale computer metrische opslaggegevens configureren in uw account opslagruimte met behulp van de Azure PowerShell-cmdlet Get-AzureStorageServiceMetricsProperty om op te halen van de huidige instellingen en de cmdlet Set-AzureStorageServiceMetricsProperty om de huidige instellingen te wijzigen.

De cmdlets waarmee wordt bepaald metrische opslaggegevens wordt de volgende parameters gebruiken:

- MetricsType: mogelijke waarden zijn uren en minuten.

- ServiceType: mogelijke waarden zijn Blob, wachtrij en tabel.

- MetricsLevel: mogelijke waarden zijn geen, Service en ServiceAndApi.

De volgende opdracht wordt bijvoorbeeld op minuut aan de doelstellingen voor de service Blob in uw standaardaccount voor de opslag met het bewaarbeleid periode is ingesteld op vijf dagen:

`Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`

De volgende opdracht haalt de huidige per uur aan de doelstellingen niveau en het bewaarbeleid dagen voor de service blob in uw standaardaccount voor de opslag:

`Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob`

Zie voor informatie over het configureren van de Azure PowerShell-cmdlets voor gebruik met uw Azure abonnement en het selecteren van het standaardaccount voor de opslag wilt gebruiken: [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Het inschakelen van metrische opslaggegevens via programmacode

Het volgende C#-fragment ziet u hoe de doelstellingen en logboekregistratie voor de Blob-service met behulp van de bibliotheek van de client opslag voor .NET inschakelen:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Enable Storage Analytics logging and set retention policy to 10 days.
    ServiceProperties properties = new ServiceProperties();
    properties.Logging.LoggingOperations = LoggingOperations.All;
    properties.Logging.RetentionDays = 10;
    properties.Logging.Version = "1.0";

    // Configure service properties for metrics. Both metrics and logging must be set at the same time.
    properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.HourMetrics.RetentionDays = 10;
    properties.HourMetrics.Version = "1.0";

    properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.MinuteMetrics.RetentionDays = 10;
    properties.MinuteMetrics.Version = "1.0";

    // Set the default service version to be used for anonymous requests.
    properties.DefaultServiceVersion = "2015-04-05";

    // Set the service properties.
    blobClient.SetServiceProperties(properties);


## <a name="viewing-storage-metrics"></a>Metrische opslaggegevens weergeven

Nadat u opslag Analytics maatstaven voor het controleren van uw opslag-account hebt geconfigureerd, records opslag Analytics cijfers over de in een verzameling bekende tabellen in uw account opslag. U kunt grafieken om te per uur statistieken bekijken in de [Portal van Azure](https://portal.azure.com)configureren:

1. Ga naar uw account opslag in de [Portal van Azure](https://portal.azure.com).
2. Klik in de sectie **controle** op **Tegels toevoegen** als u wilt toevoegen van een nieuwe grafiek. Selecteer de meetwaarde die u wilt weergeven in de **Galerie met tegel**en sleep deze naar de sectie **controle** .
3. Als u wilt bewerken op welke parameters in een grafiek worden weergegeven, klikt u op de koppeling **bewerken** . U kunt toevoegen of verwijderen van de doelstellingen van de afzonderlijke door te selecteren of deze selectie.
4. Klik op **Opslaan** wanneer u klaar bent met het bewerken van de doelstellingen.

Als u wilt downloaden van de doelstellingen voor langdurige opslag of ze lokaal analyseren, moet u naar:

- Gebruik een hulpprogramma dat is op de hoogte van deze tabellen en kunt u bekijken en kunt u deze downloaden.
- Schrijf een maatwerktoepassing of script te lezen en opslaan van de tabellen.

Veel-opslag-browsen op het hulpprogramma's van derden zich bewust bent van deze tabellen en kunnen u ze rechtstreeks te bekijken.
Zie [Azure opslag Explorers](storage-explorers.md) voor een lijst met beschikbare hulpmiddelen.

> [AZURE.NOTE] Beginnen met versie 0.8.0 van [Microsoft Azure opslag Explorer] (http://storageexplorer.com/), kunnen u nu bekijken en de tabellen van de doelstellingen analytics downloaden.

Om te kunnen openen via een programma de analytics-tabellen, houd er rekening mee dat de analytics-tabellen niet weergegeven worden als u alle tabellen in uw account opslag. U kunt deze rechtstreeks op naam te openen, of de [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) in de bibliotheek van de client .NET gebruiken om query's in de tabelnamen.

### <a name="hourly-metrics"></a>Ieder uur aan de doelstellingen
- $MetricsHourPrimaryTransactionsBlob
- $MetricsHourPrimaryTransactionsTable
- $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Minuut aan de doelstellingen
- $MetricsMinutePrimaryTransactionsBlob
- $MetricsMinutePrimaryTransactionsTable
- $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Capaciteit
- $MetricsCapacityBlob

U kunt de volledige details van de schema's vinden voor deze tabellen aan [Opslag Analytics aan de doelstellingen tabelschema](https://msdn.microsoft.com/library/azure/hh343264.aspx). De steekproef rijen onder slechts een subset van de beschikbare kolommen weergeven, maar ziet u enkele belangrijke functies van de manier waarop die metrische opslaggegevens wordt deze gegevens opgeslagen:

| PartitionKey  |       RowKey       |                    Tijdstempel | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Beschikbaarheid | AverageE2ELatency | AverageServerLatency | PercentSuccess |
|---------------|:------------------:|-----------------------------:|---------------|-----------------------|--------------|-------------|--------------|-------------------|----------------------|----------------|
| 20140522T1100 |      gebruiker. Alle      | 2014-05-22T11:01:16.7650250Z | 7             | 7                     | 4003         | 46801       | 100          | 104.4286          | 6.857143             | 100            |
| 20140522T1100 | gebruiker. QueryEntities | 2014-05-22T11:01:16.7640250Z | 5             | 5                     | 2694         | 45951       | 100          | 143,8             | 7,8                  | 100            |
| 20140522T1100 |  gebruiker. QueryEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 538          | 633         | 100          | 3                 | 3                    | 100            |
| 20140522T1100 | gebruiker. UpdateEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 771          | 217         | 100          | 9                 | 6                    | 100               |

In dit voorbeeld de doelstellingen van de minuut hebben de toets partition de keer gebruikt met minuut resolutie. De rij-toets geeft aan wat het type gegevens dat is opgeslagen in de rij en dit bestaat uit twee delen van informatie, het toegangstype en het type verzoek:

- Het toegangstype is gebruiker of system, waar gebruikers verwijst naar aanvragen van alle gebruikers met de storage-service en systeem verwijst naar aanvragen die zijn aangebracht door opslag Analytics.

- Het aanvraagtype is in dat geval moeten deze staat een overzicht lijn, of de specifieke API zoals QueryEntity of UpdateEntity worden aangeduid.


De bovenstaande voorbeeldgegevens ziet u alle records voor een enkele minuten (beginnend met 11:00 AM), zodat het aantal QueryEntities aanvragen plus het aantal QueryEntity aanvraagt plus het aantal UpdateEntity aanvraagt toevoegen maximaal zeven, dat wil het totaal zeggen van de gebruiker: alle rij weergegeven. U kunt ook de end-to-end een latentie van gemiddeld 104.4286 op de rij van de gebruiker: alle afleiden door te berekenen ((143.8 * 5) + 3 + 9) / 7.

U overwegen bij het instellen van waarschuwingen in de [Portal van Azure](https://portal.azure.com) op de pagina Monitor zodat metrische opslaggegevens kunt automatisch een melding van belangrijke wijzigingen in het gedrag van uw opslagservices. Als u een opslag explorer hulpmiddel gebruiken om te downloaden van deze gegevens aan de doelstellingen in een indeling met scheidingstekens, kunt u Microsoft Excel gebruiken om de gegevens te analyseren. Zie het blogbericht [Microsoft Azure opslag Explorer-vensters vertegenwoordigen](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) voor een lijst met beschikbare opslagruimte explorer hulpmiddelen.



## <a name="accessing-metrics-data-programmatically"></a>Toegang krijgen tot aan de doelstellingen gegevens via programmacode

De volgende vermelding ziet voorbeeld C#-code die toegang heeft tot de minuut aan de doelstellingen voor een bereik van minuten en de resultaten weergeven in een venster-console. De bibliotheek in Azure opslagcapaciteit versie 4 met de klas CloudAnalyticsClient waardoor het eenvoudiger wordt toegang krijgen tot de tabellen aan de doelstellingen in opslag wordt gebruikt.

    private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
    {
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
    Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
    var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
    var t = analyticsClient.GetMinuteMetricsTable(service);
    var opContext = new OperationContext();
    var query =
    from entity in metricsQuery
    // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
    // because they are calculated fields in the MetricsEntity class.
    // The PartitionKey identifies the DataTime of the metrics.
    where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0
    select entity;

    // Filter on "user" transactions after fetching the metrics from Table Storage.
    // (StartsWith is not supported using LINQ with Azure table storage)
    var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
    var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
    Console.WriteLine(resultString);
    }
    }

    private static string MetricsString(MetricsEntity entity, OperationContext opContext)
    {
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;

    }




## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Welke kosten oplopen u wanneer u metrische opslaggegevens inschakelen?

Schrijf aanvragen voor het maken van de tabel entiteiten aan de doelstellingen bij de toepassing op alle bewerkingen van Azure Storage standaardtarieven in rekening worden gebracht.

Lees- en delete aanvragen door een client naar gegevens aan de doelstellingen zijn ook factureerbare bij standaardtarieven. Als u een bewaarbeleid voor gegevens hebt geconfigureerd, bent u geen btw geheven wanneer Azure Storage worden de oude aan de doelstellingen gegevens verwijderd. Als u analytics-gegevens verwijdert, wordt uw account echter betalen voor de verwijderbewerkingen.

De capaciteit die worden gebruikt door de tabellen aan de doelstellingen is ook factureerbare: u kunt de volgende handelingen uit om een schatting van de hoeveelheid capaciteit gebruikt voor het opslaan van gegevens aan de doelstellingen te gebruiken:

- Als u elk uur elke API in elke service maakt gebruik van een service, wordt ongeveer 148KB aan gegevens per uur in de tabellen van de doelstellingen transactie opgeslagen als u zowel de service en de API niveau samenvatting hebt ingeschakeld.

- Als u elk uur elke API in elke service maakt gebruik van een service, wordt ongeveer 12KB aan gegevens per uur in de tabellen van de doelstellingen transactie opgeslagen als u alleen serviceniveau samenvatting hebt ingeschakeld.

- De tabel capaciteit voor BLOB's heeft twee rijen toegevoegd van elke dag (mits de gebruiker zich heeft aangemeld voor Logboeken): Dit houdt in dat elke dag de grootte van deze tabel wordt verhoogd door maximaal ongeveer 300 bytes.

## <a name="next-steps"></a>Volgende-stappen:
[Opslag en het benaderen van logboekgegevens inschakelen](https://msdn.microsoft.com/library/dn782840.aspx)
