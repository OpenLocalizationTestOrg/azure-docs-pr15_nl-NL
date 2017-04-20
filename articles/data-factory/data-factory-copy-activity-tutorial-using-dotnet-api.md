<properties 
    pageTitle="Zelfstudie: Een pijplijn maken met kopie activiteit .NET-API gebruiken | Microsoft Azure" 
    description="In deze zelfstudie maakt u een verkooppijplijn Factory van Azure-gegevens met de activiteit in een kopie door .NET-API gebruiken." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="10/27/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Zelfstudie: Een pijplijn maken met kopie activiteit .NET-API gebruiken
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md)
- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure resourcemanager-sjabloon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Deze zelfstudie ziet u hoe u maken en controleren van een Azure gegevens factory de .NET-API gebruiken. De pijplijn in de fabriek gegevens gebruikt de activiteit in een kopie gegevens van Azure-blobopslag kopiëren met Azure SQL-Database.

De activiteit kopie kunt u de verplaatsing van gegevens uitvoeren in fabriek van Azure-gegevens. De activiteit is mogelijk gemaakt door een globaal beschikbare service die gegevens tussen verschillende opgeslagen in de gegevens op een veilige, betrouwbare en scalable manier kunt kopiëren. Zie [Gegevens verkeer activiteiten](data-factory-data-movement-activities.md) artikel voor meer informatie over de activiteit kopiëren.   

> [AZURE.NOTE] 
> In dit artikel besproken niet alle de gegevens Factory .NET API. Zie [Naslag voor Data Factory .NET-API](https://msdn.microsoft.com/library/mt415893.aspx) voor meer informatie over Data Factory .NET SDK. 

## <a name="prerequisites"></a>Vereisten voor
- Ga via [zelfstudie overzicht en minimumvereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor het ophalen van een overzicht van de zelfstudie en voer de **vereiste** stappen uit. 
- Visual Studio 2012 of 2013 of 2015
- Download en installeer [Azure.NET SDK](http://azure.microsoft.com/downloads/)
- Azure PowerShell. Volg de instructies in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) artikel Azure PowerShell installeren op uw computer. Azure PowerShell kunt u een Azure Active Directory-toepassing maken.

### <a name="create-an-application-in-azure-active-directory"></a>Maken van een toepassing in Azure Active Directory
Maken van een toepassing Azure Active Directory, de hoofdsom van een service voor de toepassing maken en toewijzen aan de rol **Inzender Factory** .  

1. **PowerShell**starten. 
1. Voer de volgende opdracht en voer de gebruikersnaam en wachtwoord waarmee u zich aanmelden bij de portal van Azure.
    
        Login-AzureRmAccount   
2. Voer de volgende opdracht om te bekijken van alle abonnementen voor dit account.

        Get-AzureRmSubscription 
3. Voer de volgende opdracht om te selecteren van het abonnement waaraan u wilt werken. Vervang ** &lt;NameOfAzureSubscription** &gt; met de naam van uw Azure-abonnement. 

        Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

    > [AZURE.IMPORTANT] Houd er rekening mee omlaag **SubscriptionId** en **TenantId** uit de resultaten van deze opdracht. 
4. Maak een Azure resourcegroep met de naam **ADFTutorialResourceGroup** door de volgende opdracht uit te voeren in de PowerShell.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Als de resourcegroep al bestaat, opgeven u of u wilt bijwerken (Y) of instellen als (N). 

    Als u een resourcegroep gebruikt, moet u de naam van de resourcegroep in plaats van ADFTutorialResourceGroup in deze zelfstudie gebruiken.
5. Maak een Azure Active Directory-toepassing. 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

    Als u het volgende foutbericht krijgt, een andere URL opgeven en voert u de opdracht opnieuw. 

        Another object with the same value for property identifierUris already exists.

6. Maak de advertentie service principal. 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. Service principal toevoegen aan de rol **Inzender Factory** . 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. Ophalen van de toepassings-ID.

        $azureAdApplication

    Houd er rekening mee omlaag de toepassings-ID (**applicationID** uit de uitvoer).

U nodig hebt met het volgen van vier waarden uit de volgende stappen uit: 

- Tenant-ID
- Abonnements-ID
- Toepassings-ID 
- Wachtwoord (opgegeven in de eerste opdracht)   

## <a name="walkthrough"></a>Stapsgewijze instructies
1. Met Visual Studio 2012-2013/2015, maakt u een C# .NET-console-toepassing.
    1. Start **Visual Studio** 2012-2013-2015.
    2. Klik op **bestand**, wijs **Nieuw**aan en klik op **Project**.
    3. Vouw **sjablonen**en selecteer **Visual C#**. In dit scenario u C#, maar u kunt een .NET-taal.
    4. Selecteer **Console-toepassing** in de lijst met projecttypen aan de rechterkant.
    5. Voer **DataFactoryAPITestApp** voor de naam.
    6. Selecteer **C:\ADFGetStarted** voor de locatie.
    7. Klik op **OK** om het project te maken.
2. Klik op **Extra**, wijst u **Nuget Package Manager**en op **Package Manager-Console**.
3.  Voer de volgende stappen uit in de **Beheerconsole van pakket**: 
    1.  Voer de volgende opdracht Data Factory-pakket installeren:`Install-Package Microsoft.Azure.Management.DataFactories`       
    2.  Voer de volgende opdracht voor het installeren van Azure Active Directory-pakket (Active Directory-API in gebruikt de code):`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
6. De volgende **appSetttings** sectie toevoegen aan het bestand **App.config** . Deze instellingen worden gebruikt door de helpmethode: **GetAuthorizationHeader**. 

    Vervang waarden voor ** &lt;toepassings-ID&gt;**, ** &lt;wachtwoord&gt;**, ** &lt;abonnements-ID&gt;**, en ** &lt;tenant ID&gt; ** met uw eigen waarden. 

        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
            </startup>
            <appSettings>
                <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
                <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
                <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

                <add key="ApplicationId" value="your application ID" />
                <add key="Password" value="Password you used while creating the AAD application" />
                <add key="SubscriptionId" value= "Subscription ID" />
                <add key="ActiveDirectoryTenantId" value="Tenant ID" />
            </appSettings>
        </configuration>
6. De volgende instructies voor het **gebruik van** toevoegen aan het bronbestand (Program.cs) in het project.

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. Voeg de volgende code die een exemplaar van de methode **Main** **DataPipelineManagementClient** klasse maakt. Dit object kunt u een factory gegevens, een gekoppelde service, invoer- en uitvoerbereik gegevenssets en een pijplijn maken. U kunt ook in dit object gebruiken om de segmenten van een gegevensset gedurende runtime te houden.    

            // create data factory management client
            string resourceGroupName = "ADFTutorialResourceGroup";
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.IMPORTANT] 
    > Vervang de waarde van **resourceGroupName** door de naam van uw Azure resourcegroep. 
    > 
    > Naam van de gegevens fabriek (**dataFactoryName**) moet uniek bijwerken. Naam van de gegevens fabriek moet uniek zijn. Zie [Gegevens Factory - regels voor naamgeving van](data-factory-naming-rules.md) onderwerp voor naming regels voor Data Factory-onderdelen. 

7. Voeg de volgende code die wordt gemaakt van een **fabriek gegevens** met de methode **Hoofdgegeven** .

            // create a data factory
            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties() { }
                    }
                }
            );

8. Voeg de volgende code die Hiermee maakt u een **Azure Storage service gekoppeld** aan de methode **Main** . 

    > [AZURE.IMPORTANT] **Storageaccountname** en **accountkey** vervangen door de naam en van toetsen van uw opslagruimte van Azure-account. 

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. Voeg de volgende code die Hiermee maakt u een **SQL Azure service gekoppeld** aan de methode **Main** .
 
    > [AZURE.IMPORTANT] **Servernaam**, **databasenaam**, **gebruikersnaam**en **wachtwoord** vervangen door namen van uw Azure SQL server-database, gebruiker en wachtwoord.  

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. Voeg de volgende code die wordt gemaakt van **invoer- en uitvoerbereik gegevenssets** met de methode **Hoofdgegeven** . 

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. Voeg de volgende code die **wordt gemaakt en wordt een pijplijn geactiveerd** met de methode **Hoofdgegeven** . Deze verkooppijplijn heeft een **CopyActivity** waarmee **BlobSource** als een bron- en **BlobSink** als een sink.

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "BlobToAzureSql",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    TypeProperties = new CopyActivity()
                                    {
                                        Source = new BlobSource(),
                                        Sink = new BlobSink()
                                        {
                                            WriteBatchSize = 10000,
                                            WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                        }
                                    }
                                }
                            },
                        }
                    }
                }); 

12. De volgende code toevoegen aan de methode **Main** om de status van een deel van de gegevens van de gegevensset uitvoer. Er is alleen segment verwacht in dit voorbeeld.   
 
            // Pulling status within a timeout threshold
            DateTime start = DateTime.Now;
            bool done = false;

            while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
            {
                Console.WriteLine("Pulling the slice status");
                // wait before the next status check
                Thread.Sleep(1000 * 12);

                var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                    new DataSliceListParameters()
                    {
                        DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                        DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                    });

                foreach (DataSlice slice in datalistResponse.DataSlices)
                {
                    if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                    {
                        Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                        done = true;
                        break;
                    }
                    else
                    {
                        Console.WriteLine("Slice status is: {0}", slice.State);
                    }
                }
            }

14. Voeg de volgende code als u wilt ophalen details voor een segment gegevens uitvoeren met de methode **Hoofdgegeven** .

            Console.WriteLine("Getting run details of a data slice");

            // give it a few minutes for the output slice to be ready
            Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
            Console.ReadKey();

            var datasliceRunListResponse = client.DataSliceRuns.List(
                    resourceGroupName,
                    dataFactoryName,
                    Dataset_Destination,
                    new DataSliceRunListParameters()
                    {
                        DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                    }
                );

            foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
            {
                Console.WriteLine("Status: \t\t{0}", run.Status);
                Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
                Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
                Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
                Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
                Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
                Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();

13. Voeg de volgende helpmethode gebruikt door de methode **Main** aan de klasse **programma** .  
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
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


15. In de Verkenner oplossing uitvouwen van het project (**DataFactoryAPITestApp**), met de rechtermuisknop op **verwijzingen**en klik op **Add Reference**. Schakel het selectievakje voor "**System.Configuration**" constructie en klik op **OK**. 
16. De consoletoepassing maken. Op de knop **Opbouwen** in het menu en klik op **Oplossing maken**. 
16. Controleer of er minimaal één bestand in de container **adftutorial** in uw Azure-blobopslag. Als dat niet zo is, **Emp.txt** -bestand in Kladblok met de volgende inhoud maken en uploaden naar de container adftutorial.

        John, Doe
        Jane, Doe
     
17. De steekproef worden uitgevoerd door te klikken op **fouten opsporen in** -> **Starten foutopsporing** in het menu. Wanneer u de **details van een segment gegevens ophalen uitvoeren ziet**, wacht enkele minuten en druk op **ENTER**. 
18. Gebruik de Azure-portal om te controleren of de gegevens fabriek **APITutorialFactory** is gemaakt met de volgende onderdelen: 
    - Service gekoppeld: **LinkedService_AzureStorage** 
    - Gegevensset: **DatasetBlobSource** en **DatasetBlobDestination**.
    - Verkooppijplijn: **PipelineBlobSample** 
18. Controleer of de twee werknemerrecords zijn gemaakt in de tabel '**emp**' in de opgegeven Azure SQL-database.

## <a name="next-steps"></a>Volgende stappen

- Lees [Gegevens verkeer activiteiten](data-factory-data-movement-activities.md) artikel, waarin vindt u gedetailleerde informatie over de kopie activiteit die u in deze zelfstudie hebt gebruikt.
- Zie [Naslag voor Data Factory .NET-API](https://msdn.microsoft.com/library/mt415893.aspx) voor meer informatie over Data Factory .NET SDK. In dit artikel besproken niet alle de gegevens Factory .NET API. 

 
