<properties 
    pageTitle="I-SQL-script uitvoeren op Azure gegevens Lake analyses van Azure gegevens Factory" 
    description="Informatie over het verwerken van gegevens door U-SQL-scripts op Azure gegevens Lake Analytics berekeningscluster-service uit te voeren." 
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
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="spelluru"/>

# <a name="run-u-sql-script-on-azure-data-lake-analytics-from-azure-data-factory"></a>I-SQL-script uitvoeren op Azure gegevens Lake analyses van Azure gegevens Factory
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics I-SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)
 
Gegevens in gekoppelde opslagservices verwerkt een pijplijn in een fabriek Azure gegevens door middel van gekoppelde berekeningscluster services. De presentatie bevat een reeks activiteiten waarbij elke activiteit uitvoert voor een specifieke verwerking. In dit artikel worden de **Gegevens Lake Analytics I-SQL activiteit** die een **I-SQL** -script op een **Azure gegevens Lake Analytics** berekeningscluster gekoppeld-service wordt uitgevoerd. 

> [AZURE.NOTE] 
> Maak een Azure gegevens Lake Analytics-account voordat u een pijplijn maakt met een activiteit gegevens Lake Analytics I-SQL. Zie meer informatie over Azure gegevens Lake analyses, [aan de slag met Azure gegevens Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).
>  
> Bekijk het [samenstellen van uw eerste verkooppijplijn zelfstudie](data-factory-build-your-first-pipeline.md) voor gedetailleerde stappen om te maken van een fabriek gegevens, gekoppelde services gegevenssets en een pijplijn. Gebruik JSON fragmenten met gegevens Factory Editor of Visual Studio of Azure PowerShell Data Factory entiteiten maken.

## <a name="azure-data-lake-analytics-linked-service"></a>Azure gegevens Lake Analytics gekoppeld Service
U maken een service van **Azure gegevens Lake Analytics** gekoppeld als u wilt een service voor het berekenen van Azure gegevens Lake Analytics koppelen aan een factory Azure gegevens. De activiteit gegevens Lake Analytics I-SQL in de pijplijn verwijst naar deze gekoppelde service. 

Het volgende voorbeeld bevat JSON definitie voor een service van Azure gegevens Lake Analytics gekoppeld. 

    {
        "name": "AzureDataLakeAnalyticsLinkedService",
        "properties": {
            "type": "AzureDataLakeAnalytics",
            "typeProperties": {
                "accountName": "adftestaccount",
                "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
                "authorization": "<authcode>",
                "sessionId": "<session ID>", 
                "subscriptionId": "<subscription id>",
                "resourceGroupName": "<resource group name>"
            }
        }
    }


De volgende tabel vindt u beschrijvingen voor de eigenschappen die wordt gebruikt in de JSON-definitie. 

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
Type | De eigenschap type moet worden ingesteld: **AzureDataLakeAnalytics**. | Ja
Accountnaam | Azure gegevens Lake Analytics accountnaam. | Ja
dataLakeAnalyticsUri | Azure gegevens Lake Analytics-URI. |  Nee 
autorisatie | Autorisatiecode wordt automatisch opgehaald na **autoriseren** te klikken in de gegevens Factory Editor en voltooien van de OAuth login.  | Ja 
subscriptionId | Azure abonnements-id | Niet (als u niet opgeeft, abonnement van de fabriek gegevens wordt gebruikt). 
resourceGroupName | De naam van de Azure resource-groep |  Niet (als u niet opgeeft, resourcegroep van de fabriek gegevens wordt gebruikt).
sessie-id | sessie-id uit het OAuth autorisatie-sessie. Elke sessie-id is uniek en kan slechts eenmaal worden gebruikt. De sessie-Id is automatisch gegenereerde in de gegevens Factory Editor. | Ja

De autorisatiecode die u hebt gegenereerd met behulp van de knop **autoriseren** verloopt later opnieuw. Zie de volgende tabel voor de verlooptijd tijden voor verschillende soorten gebruikersaccounts. Ziet u mogelijk de volgende fout bericht wanneer de verificatie **token is verlopen**: referentie-bewerking-fout: invalid_grant - AADSTS70002: fout bij valideren van referenties. AADSTS70008: De meegeleverde toegang verlenen is verlopen of ingetrokken. Doelcellen-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 correlatie-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 tijdstempel: 2015-12-15 21:09:31Z

 
| Gebruikerstype | Verloopt na |
| :-------- | :----------- | 
| Gebruikersaccounts die niet worden beheerd door Azure Active Directory (@hotmail.com, @live.com, enz.) | 12 uur |
| Gebruikersaccounts die worden beheerd door Azure Active Directory (AAD) | 14 dagen na het laatste segment uitvoeren. <br/><br/>90 dagen, als een segment op basis van de gekoppelde service OAuth gebaseerde ten minste eenmaal per 14 dagen wordt uitgevoerd. |

Als u wilt deze fout voorkomen/oplossen, met de **autoriseren** autoriseren knop wanneer de **token verloopt** en in dat geval de gekoppelde service. U kunt ook de waarden voor de eigenschappen van **sessie-id** en **autorisatie** programmacode met code in de volgende sectie genereren. 

  
### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>Via een programma sessie-id en de machtiging om waarden te genereren 

    if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
        linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
    {
        AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

        WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
        string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

        AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
        if (azureDataLakeStoreProperties != null)
        {
            azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeStoreProperties.Authorization = authorization;
        }

        AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
        if (azureDataLakeAnalyticsProperties != null)
        {
            azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeAnalyticsProperties.Authorization = authorization;
        }
    }

Zie [AzureDataLakeStoreLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)en [AuthorizationSessionGetResponse klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) -onderwerpen voor meer informatie over de gegevens Factory-klassen gebruikt in de code. Een verwijzing naar toevoegen: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll voor de klas WindowsFormsWebAuthenticationDialog. 
 
 
## <a name="data-lake-analytics-u-sql-activity"></a>Gegevens Lake Analytics I-SQL activiteit 

Het codefragment van de volgende JSON Hiermee definieert u een pijplijn met een activiteit gegevens Lake Analytics I-SQL. De definitie van de activiteit heeft een verwijzing naar de Azure gegevens Lake Analytics gekoppeld-service die u eerder hebt gemaakt.   
  

    {
        "name": "ComputeEventsByRegionPipeline",
        "properties": {
            "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
            "activities": 
            [
                {
                    "type": "DataLakeAnalyticsU-SQL",
                    "typeProperties": {
                        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                        "scriptLinkedService": "StorageLinkedService",
                        "degreeOfParallelism": 3,
                        "priority": 100,
                        "parameters": {
                            "in": "/datalake/input/SearchLog.tsv",
                            "out": "/datalake/output/Result.tsv"
                        }
                    },
                    "inputs": [
                        {
                            "name": "DataLakeTable"
                        }
                    ],
                    "outputs": 
                    [
                        {
                            "name": "EventsByRegionTable"
                        }
                    ],
                    "policy": {
                        "timeout": "06:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "EventsByRegion",
                    "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
                }
            ],
            "start": "2015-08-08T00:00:00Z",
            "end": "2015-08-08T01:00:00Z",
            "isPaused": false
        }
    }


De volgende tabel beschrijft de namen en beschrijvingen van eigenschappen die specifiek voor deze activiteit zijn. 

Eigenschap | Beschrijving | Vereist
:-------- | :----------- | :--------
type | De eigenschap type moet worden ingesteld op **DataLakeAnalyticsU-SQL**. | Ja
scriptPath | Pad naar de map waarin het script I-SQL. Naam van het bestand is hoofdlettergevoelig. | Geen (als u script)
scriptLinkedService | Gekoppelde service die de opslag waarin het script om de gegevens fabriek koppelingen | Geen (als u script)
script | Inline-script in plaats van de precisie scriptPath en scriptLinkedService opgeven. Bijvoorbeeld: "script": "DATABASE maken-test". | Geen (als u scriptPath en scriptLinkedService gebruikt)
degreeOfParallelism | Het maximum aantal knooppunten tegelijk wordt gebruikt voor het uitvoeren van de taak. | Nee
prioriteit | Bepaalt welke taken afmelden bij alle in de wachtrij eerst moeten zijn geselecteerd. De laagste het getal, hoe hoger de prioriteit. | Nee 
parameters | Parameters voor het I-SQL-script | Nee 

Zie [SearchLogProcessing.txt Script definitie](#script-definition) voor de script-definitie. 

## <a name="sample-input-and-output-datasets"></a>Voorbeeld van de invoer en uitvoer gegevenssets

### <a name="input-dataset"></a>Invoer gegevensset
In dit voorbeeld is de invoergegevens bevindt zich in het hulpprogramma voor het Lake van een Azure-gegevensopslag (SearchLog.tsv-bestand in de map datalake/invoer). 

    {
        "name": "DataLakeTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/input/",
                "fileName": "SearchLog.tsv",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }   

### <a name="output-dataset"></a>Uitvoer gegevensset
In dit voorbeeld wordt de uitvoergegevens geproduceerd door het I-SQL-script opgeslagen in het hulpprogramma voor het Lake van een Azure-gegevensopslag (datalake/uitvoer map). 

    {
        "name": "EventsByRegionTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/output/"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="sample-data-lake-store-linked-service"></a>Voorbeeld Lake gegevensopslag gekoppeld Service
Hier ziet u de definitie van de steekproef die Azure Lake gegevensopslag gekoppeld service wordt gebruikt door de gegevenssets invoer/uitvoer. 

    {
        "name": "AzureDataLakeStoreLinkedService",
        "properties": {
            "type": "AzureDataLakeStore",
            "typeProperties": {
                "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
                "sessionId": "<session ID>",
                "authorization": "<authorization URL>"
            }
        }
    }

Zie [gegevens van en naar Azure Lake gegevensopslag verplaatsen](data-factory-azure-datalake-connector.md) artikel voor een beschrijving van JSON-eigenschappen. 

## <a name="sample-u-sql-script"></a>Voorbeeldscript I-SQL 

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv(nullEscape:"#NULL#");
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start <= DateTime.Parse("2012/02/19");
    
    OUTPUT @rs1   
        TO @out
          USING Outputters.Tsv(quoting:false, dateTimeFormat:null);

De waarden voor **@in** en **@out** parameters in het I-SQL-script worden doorgegeven dynamisch door ADF met behulp van de sectie 'parameters'. Zie het gedeelte 'parameters' in de verkooppijplijn definitie.

U kunt andere eigenschappen zoals degreeOfParallelism en prioriteit als u ook opgeven in de definitie van uw pijplijn voor de taken die worden uitgevoerd op de Azure gegevens Lake Analytics-service.

## <a name="dynamic-parameters"></a>Dynamische parameters
In de definitie van de verkooppijplijn voorbeeld in-en uitfaden parameters toegewezen met de hard gecodeerde waarden. 

    "parameters": {
        "in": "/datalake/input/SearchLog.tsv",
        "out": "/datalake/output/Result.tsv"
    }

Het is mogelijk in plaats daarvan dynamische parameters gebruiken. Bijvoorbeeld: 

    "parameters": {
        "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
        "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
    }

In dit geval invoer bestanden zijn nog steeds opgehaald uit de map /datalake/input en uitvoerbestanden in de map /datalake/output worden gegenereerd. De bestandsnamen zijn dynamisch op basis van de begintijd segment.  