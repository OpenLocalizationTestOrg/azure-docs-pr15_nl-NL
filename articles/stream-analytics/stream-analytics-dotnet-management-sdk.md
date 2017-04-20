<properties
    pageTitle="Beheer .NET SDK voor Stream Analytics | Microsoft Azure"
    description="Aan de slag met Stream Analytics Management .NET SDK. Meer informatie over het instellen en analyses taken uitvoeren: een project, invoer, uitvoer en transformaties maken."
    keywords=".NET SDK, analytics API"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>Beheer .NET SDK: Instellen en uitvoeren van analytics-taken met behulp van de Azure Stream Analytics-API voor .NET

Leer hoe u de taken van een analyses uitvoeren met behulp van de Stream Analytics-API voor .NET met de Management .NET SDK instellen. Een project instellen, maak invoer- en uitvoerbereik bronnen, transformaties en starten en stoppen taken. U kunt gegevens van Blob storage of van een gebeurtenis hub streamen voor uw projecten analytics.

Zie de [documentatie management voor de Stream Analytics-API voor .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure Stream Analytics is een volledig beheerde service lage latentie, ten zeerste beschikbaar, scalable, complexe gebeurtenis verwerking leveren via streaming van gegevens in de cloud. Stream Analytics kan klanten instellen de optie streaming van taken te analyseren gegevensstromen en kan ze station in de buurt van realtime analytics.  


## <a name="prerequisites"></a>Vereisten voor
Voordat u in dit artikel begint, hebt u het volgende:

- Visual Studio 2012 of 2013 installeren.
- Download en installeer [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Maak een resourcegroep Azure in uw abonnement. Hierna volgt een voorbeeld Azure PowerShell-script. Zie voor informatie Azure PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md);  


        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


-   Stel een invoerbron en uitvoerdoel in gebruik. Instructies Zie voor meer [Invoeritems toevoegen](stream-analytics-add-inputs.md) voor het instellen van een steekproef invoer en [Uitvoer van toevoegen](stream-analytics-add-outputs.md) om de uitvoer van een steekproef in te stellen.


## <a name="set-up-a-project"></a>Een project instellen

U maakt met een taak analytics de Stream Analytics-API voor .NET, stelt u eerst uw project.

1. Een Visual Studio C# .NET-console-toepassing maken.
2. In de Console Package Manager, voer de volgende opdrachten voor het installeren van de NuGet-pakketten. De eerste fase is de Azure Stream Analytics Management .NET SDK. Het tweede is de Azure Active Directory-client die wordt gebruikt voor verificatie.

        Install-Package Microsoft.Azure.Management.StreamAnalytics
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

4. De volgende sectie **appSettings** toevoegen aan het bestand App.config:

        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>


    Waarden voor **SubscriptionId** en **ActiveDirectoryTenantId** vervangen door uw Azure-abonnement en id's tenant. U kunt deze waarden krijgen door de volgende Azure PowerShell-cmdlet uit te voeren:

        Get-AzureAccount

5. De volgende instructies voor het **gebruik van** toevoegen aan het bronbestand (Program.cs) in het project:

        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

6.  Een helper verificatiemethode toevoegen:

        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(
                        ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                        ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    result = context.AcquireToken(
                        resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                        clientId: ConfigurationManager.AppSettings["AsaClientId"],
                        redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                        promptBehavior: PromptBehavior.Always);
                }
                catch (Exception threadEx)
                {
                    Console.WriteLine(threadEx.Message);
                }
            });

            thread.SetApartmentState(ApartmentState.STA);
            thread.Name = "AcquireTokenThread";
            thread.Start();
            thread.Join();

            if (result != null)
            {
                return result.AccessToken;
            }

            throw new InvalidOperationException("Failed to acquire token");
        }  


## <a name="create-a-stream-analytics-management-client"></a>Maak een Stream Analytics management-client

Een object **StreamAnalyticsManagementClient** kunt u voor het beheren van de taak en de onderdelen van de taak, zoals invoer, uitvoer en transformatie.

De volgende code toevoegen aan het begin van de methode **Main** :

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

Waarde van de variabele **resourceGroupName** moet hetzelfde als de naam van de resourcegroep die u hebt gemaakt of gekozen in de vereiste stappen.

Als u wilt de aspecten van de presentatie referentie van het maken van taken automatiseren, raadpleegt u [een service principal met Azure resourcemanager verifiëren](../resource-group-authenticate-service-principal.md).

De overige secties van dit artikel wordt ervan uitgegaan dat deze code aan het begin van de methode **Main** .

## <a name="create-a-stream-analytics-job"></a>Een taak Stream analyses maken

De volgende code maakt een taak Stream Analytics onder de resourcegroep die u hebt gedefinieerd. Aan de taak wordt u een invoer, uitvoer en transformatie later toegevoegd.

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a>Een invoerbron Stream analyses maken

De volgende code maakt een Stream Analytics-invoerbron met de blob invoerbron type en het CSV-serialisatie. Als u wilt maken van een gebeurtenis hub-invoerbron, gebruikt u **EventHubStreamInputDataSource** in plaats van **BlobStreamInputDataSource**. U kunt ook het serialisatietype de invoerbron aanpassen.

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

Invoerbronnen, zijn uit Blob storage of een gebeurtenis-hub, gekoppeld aan een specifieke taak. Als u wilt gebruiken de dezelfde invoerbron voor verschillende taken, moet u de methode opnieuw bellen en geef de naam van een ander project.


## <a name="test-a-stream-analytics-input-source"></a>Een Stream Analytics-invoerbron testen

De methode **TestConnection** gecontroleerd of de taak Stream Analytics verbinding maken met de invoerbron, evenals andere aspecten die specifiek zijn voor het type invoerbron is. Bijvoorbeeld, in de blob invoerbron die u in een eerdere stap hebt gemaakt, de methode moeten worden gecontroleerd dat de opslagaccountnaam en een combinatie van sleutel kan worden gebruikt om te verbinden met de opslag-account en controleer of de opgegeven container bestaat.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>Een doel van de uitvoer Stream analyses maken

Maken van een uitvoerdoel is vergelijkbaar met het maken van een invoerbron Stream Analytics. Uitvoer doelen zijn zoals invoerbronnen gekoppeld aan een specifieke taak. Als u hetzelfde uitvoerdoel voor verschillende projecten, moet u de methode opnieuw bellen en geef de taaknaam van een andere.

De volgende code maakt een uitvoerdoel (Azure SQL-database). U kunt het gegevenstype van het uitvoerdoel en/of serialisatietype aanpassen.

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a>Een doel van de uitvoer Stream Analytics testen

Een doel Stream Analytics-uitvoer, heeft ook de **TestConnection** methode voor het testen van verbindingen.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Een Stream Analytics-transformatie maken

De volgende code maakt een Stream Analytics-transformatie met de query "Selecteer * uit invoer" opgegeven als u één streaming eenheid voor de Stream Analytics-taak toewijzen. Zie voor meer informatie over het aanpassen van streaming eenheden [schaal Azure Stream Analytics-taken](stream-analytics-scale-jobs.md).


    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

Net als invoer en uitvoer, een transformaties ook is gebonden aan de specifieke Stream Analytics-taak deze onder is gemaakt.

## <a name="start-a-stream-analytics-job"></a>Een taak Stream Analytics starten
Na het maken van een taak Stream Analytics en de input(s), output(s) en transformatie, kunt u de taak starten door te bellen met de methode **Start** .

Het volgende voorbeeld van de code wordt gestart een Stream Analytics-project met de begintijd van een aangepaste uitvoer ingesteld op 12 December 2012 12:12:12 UTC:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);



## <a name="stop-a-stream-analytics-job"></a>Een taak Stream Analytics stoppen
U kunt een actieve Stream Analytics-taak stoppen door aan te roepen met de methode **Stop** .

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>Een taak Stream analyses verwijderen
De methode **verwijderen** verwijdert de taak, evenals de onderliggende submappen bronnen, inclusief input(s), output(s) en transformatie van de taak.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);


## <a name="get-support"></a>Ondersteuning
Probeer onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)voor meer hulp.


## <a name="next-steps"></a>Volgende stappen

U hebt zal het leren van de basisbeginselen van het gebruik van een .NET-SDK maken en analyses taken uitvoeren. Zie de volgende onderwerpen voor meer informatie:

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
