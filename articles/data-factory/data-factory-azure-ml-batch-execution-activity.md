<properties 
    pageTitle="Gebruik van Machine Learning-activiteiten | Microsoft Azure" 
    description="Wordt beschreven hoe u blog pijpleidingen met Azure gegevens Factory en Azure Machine Learning maken" 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="create-predictive-pipelines-using-azure-machine-learning-activities"></a>Bekijk pijpleidingen met Azure Machine Learning-activiteiten maken   
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics I-SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Inleiding

[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) kunt u bouwen, testen en bekijk analyses oplossingen implementeren. Vanuit een hoog niveau oogpunt, wordt deze uitgevoerd in drie stappen: 

1. **Een experiment training maken**. U doen deze stap met behulp van de Azure ML Studio. De ML studio is een gezamenlijke visuele ontwikkelomgeving waarmee u zich trainen en testen van een blog analytics-model met trainingsgegevens.
2. **Converteren naar een blog experiment**. Als uw model met bestaande gegevens is getraind en u wilt gebruiken voor het verkrijgen van nieuwe gegevens zijn, kunt u voorbereiden en uw experiment voor het scoren stroomlijnen.
3. **Het als een webservice dashboard implementeren**. U kunt uw score experiment publiceren als een Azure webservice. U kunt gegevens naar uw model via dit eindpunt van de web-service verzenden en ontvangen resultaat voorspellingen van het model.  

Azure gegevens Factory kunt u eenvoudig maken pijpleidingen met een gepubliceerde [Azure Machine Learning] [ azure-machine-learning] webservice van blog analysegegevens. Zie [Inleiding tot Factory van Azure-gegevens](data-factory-introduction.md) en het [bouwen van uw eerste verkooppijplijn](data-factory-build-your-first-pipeline.md) artikelen voor snel aan de slag met de Azure gegevens Factory-service. 

De **Batch Execution activiteit** in een Azure gegevens Factory-verkooppijplijn gebruikt, kunt u een webservice Azure ML zodat voorspellingen van de gegevens in de batch oproepen. Zie de [aanroepen van een ML Azure webservice met de Batch Execution activiteit](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) sectie voor meer informatie.

Over een periode moeten de blog modellen in de Azure ML scoren experimenten worden retrained nieuwe invoer gegevenssets opnieuw gebruiken. U kunt een model Azure ML uit een pijplijn Data Factory retrain door te doen van de volgende stappen uit: 

1. Het experiment training (niet blog experiment) publiceren als een webservice. U doen deze stap in het Azure ML Studio als hiervoor toen u blog experiment weergegeven als een webservice in het vorige scenario.
2. Gebruik de activiteit Azure ML Batch worden uitgevoerd om aan te roepen de webservice voor het experiment training. U kunt de activiteit Azure ML Batch Execution in principe gebruiken om aan te roepen zowel training-webservice als score webservice. 
  
Nadat u klaar bent met nodig, die u wilt bijwerken van de score webservice (blog experiment weergegeven als een webservice) met het zojuist ervaren model. Hier volgen de stappen: 

1. Een eindpunt niet-standaard toevoegen aan de score webservice. De standaardeindpunt van de webservice kan niet worden bijgewerkt, dus u hoeft te maken van een niet-standaard-eindpunt met behulp van de Azure portal. Zie het artikel [Eindpunten maken](../machine-learning/machine-learning-create-endpoint.md) voor algemene informatie en procedurestappen.
2. Bestaande Azure ML gekoppeld services voor het gebruik van het eindpunt van de niet-standaard scoren bijwerken. Beginnen met het nieuwe endpoint gebruik van de webservice die automatisch wordt bijgewerkt.
3. Gebruik de **Azure ML bijwerken Resource activiteit** om de webservice bijwerken met het zojuist ervaren model.  

Zie de sectie [modellen van Azure ML bijwerken met de activiteit van de Resource bijwerken](#updating-azure-ml-models-using-the-update-resource-activity) voor meer informatie. 

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Aanroepen van Batch Execution activiteit met een webservice

U Azure gegevens Factory goedkeuringen verplaatsing van gegevens en verwerking gebruiken en voer batch kan worden uitgevoerd Azure Machine Learning uit. Hier volgen de stappen op het hoogste niveau:

1. Maak een service Azure Machine Learning gekoppeld. U hebt het volgende nodig:
    1. **URI van de aanvraag** voor de Batchuitvoering API. U kunt de URI aanvragen vinden door te klikken op de koppeling **BATCH EXECUTION** in de pagina met webonderdelen services.
    1. **API-sleutel** voor de gepubliceerde Azure Machine Learning-webservice. U kunt de API-sleutel vinden door te klikken op de webservice die u hebt gepubliceerd. 
 2. Gebruik de activiteit **AzureMLBatchExecution** .

    ![Machine Learning-Dashboard](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

    ![Batch URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)


### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Scenario: Proeven met Web service invoer/uitvoer van die naar gegevens in Azure-blobopslag verwijzen
In dit scenario de Azure Machine Learning-webservice maakt gebruik van gegevens uit een bestand in een Azure-blobopslag voorspellingen worden opgeslagen en de resultaten tekstvoorspelling in de blobopslag. De volgende JSON Hiermee definieert u een Data Factory-pijplijn met een activiteit AzureMLBatchExecution. De activiteit heeft de gegevensset **DecisionTreeInputBlob** als invoer en **DecisionTreeResultBlob** als de uitvoer. De **DecisionTreeInputBlob** wordt doorgegeven als invoer voor de webservice met behulp van de **webServiceInput** JSON-eigenschap. De **DecisionTreeResultBlob** wordt als een uitvoer doorgegeven aan de webservice met behulp van de **webServiceOutputs** JSON-eigenschap.  

> [AZURE.IMPORTANT] 
> Als de webservice meerdere ingangen duurt, gebruikt u de eigenschap **webServiceInputs** in plaats van **webServiceInput**. Zie het gedeelte [webservice waarmee meerdere ingangen is vereist](#web-service-requires-multiple-inputs) voor een voorbeeld van het gebruik van de eigenschap webServiceInputs.
>  
> Gegevenssets waarnaar wordt verwezen door de **webServiceInput**/eigenschappen**webServiceInputs** en **webServiceOutputs** (in **typeProperties**) moeten ook worden opgenomen in de activiteit **invoer** en **uitvoer**.
> 
> In uw experiment Azure ML hebben web service invoer en uitvoerpoorten en globale parameters standaardnamen ("input1", "input2") die u kunt aanpassen. De namen die u voor webServiceInputs, webServiceOutputs en globalParameters instellingen gebruikt moeten precies overeenkomen met de namen in de experimenten. U kunt de steekproef verzoek nettolading bekijken op de pagina Batch Execution Help voor uw eindpunt Azure ML om te controleren of de verwachte toewijzing. 


    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "DecisionTreeInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "DecisionTreeResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties":
            {
                "webServiceInput": "DecisionTreeInputBlob",
                "webServiceOutputs": {
                    "output1": "DecisionTreeResultBlob"
                }                
            },
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

> [AZURE.NOTE] Alleen invoer en uitvoer van de activiteit AzureMLBatchExecution kunnen als parameters worden doorgegeven aan de webservice. In het bovenstaande JSON-fragment is DecisionTreeInputBlob bijvoorbeeld een invoer voor de activiteit AzureMLBatchExecution, dat wordt doorgegeven als invoer voor de webservice via webServiceInput parameter.   

### <a name="example"></a>Voorbeeld

In dit voorbeeld gebruikt Azure-opslag voor de invoer- en uitvoerbereik gegevens. 

Het is raadzaam dat u leest u over het [maken van uw eerste verkooppijplijn met gegevens Factory] [ adf-build-1st-pipeline] Zelfstudievideo voordat u verdergaat met dit voorbeeld. De gegevens Factory-Editor gebruiken om te maken van gegevens Factory-onderdelen (gekoppelde services, gegevenssets, verkooppijplijn) in dit voorbeeld.   
 

1. Maak een **gekoppelde service** voor uw **Azure-opslag**. Als de invoer- en uitvoerbereik-bestanden in verschillende opslag-accounts zijn, moet u twee gekoppelde services. Hier volgt een voorbeeld JSON:

        {
          "name": "StorageLinkedService",
          "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
            }
          }
        }

2. Maak de **invoer** Azure gegevens Factory **gegevensset**. In tegenstelling tot enkele andere gegevens Factory-gegevenssets, moet deze gegevenssets **mappad** en de **bestandsnaam** waarden bevatten. U kunt partitioneren elke batch worden uitgevoerd (elk segment van de gegevens) om te verwerken of produceren unieke invoer en -bestanden. Mogelijk moet u enkele boven activiteit te transformeren van de invoer in de CSV-indeling in de opslagruimte-account voor elk segment plaatsen opnemen. In dat geval u de instellingen voor **externe** en **externalData** weergegeven in het volgende voorbeeld niet wilt opnemen, en uw DecisionTreeInputBlob zou de gegevensset uitvoer van een andere activiteit.

        {
          "name": "DecisionTreeInputBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/input",
              "fileName": "in.csv",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Day",
              "interval": 1
            },
            "policy": {
              "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
              }
            }
          }
        }
    
    Uw invoer CSV-bestand moet de kolom veldnamenrij. Als u de **Kopie activiteit** naar maken/naar te verplaatsen naar het csv-blobopslag de gebruikt, moet u de sink eigenschap **blobWriterAddHeader** ingesteld op **waar**. Bijvoorbeeld:
    
         sink: 
         {
             "type": "BlobSink",     
             "blobWriterAddHeader": true 
         }
     
    Als het CSV-bestand geen de veldnamenrij, ziet u mogelijk de volgende fout: **fout in activiteit: fout bij het lezen van tekenreeks. Onverwacht token: StartObject. Pad '', regel 1, positie 1**.
3. Maak de **uitvoer** Azure gegevens Factory **gegevensset**. In dit voorbeeld wordt partitioneren gebruikt om een uitvoerpad unieke voor de uitvoering van elke segment te maken. Zonder de partities, zou de activiteit het bestand overschrijven.

        {
          "name": "DecisionTreeResultBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/scored/{folderpart}/",
              "fileName": "{filepart}result.csv",
              "partitionedBy": [
                {
                  "name": "folderpart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "yyyyMMdd"
                  }
                },
                {
                  "name": "filepart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HHmmss"
                  }
                }
              ],
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Day",
              "interval": 15
            }
          }
        }

4. Maken van een **gekoppelde service** van het type: **AzureMLLinkedService**, de API leveren belangrijke en modelleren van batch execution URL.
        
        {
          "name": "MyAzureMLLinkedService",
          "properties": {
            "type": "AzureML",
            "typeProperties": {
              "mlEndpoint": "https://[batch execution endpoint]/jobs",
              "apiKey": "[apikey]"
            }
          }
        }
5. Tot slot creëert een pijplijn met een activiteit **AzureMLBatchExecution** . Gedurende runtime, wordt in verkooppijplijn de volgende stappen uitvoert:
    1. De locatie van de invoer bestand krijgt uit uw invoer gegevenssets.
    2. Hiermee wordt de uitvoering van Azure Machine Learning batch API
    3. Hiermee kopieert de uitvoer van de uitvoering batch naar de blob uitgedrukt in uw gegevensset uitvoer. 

    > [AZURE.NOTE] AzureMLBatchExecution activiteit kan hebben nul of meer invoer en uitvoer van een of meer.

        {
          "name": "PredictivePipeline",
          "properties": {
            "description": "use AzureML model",
            "activities": [
              {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                  {
                    "name": "DecisionTreeInputBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "DecisionTreeResultBlob"
                  }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                  "concurrency": 3,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 1,
                  "timeout": "02:00:00"
                }
              }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
          }
        }

    **Begin** en **einde** datum/tijd moet [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601). Bijvoorbeeld: 2014-10-14T16:32:41Z. **De eindtijd** is optioneel. Als u geen waarde voor de eigenschap **end** opgeeft, wordt berekend als "**start + 48 uur.**" Als u wilt de pijplijn voor onbepaalde tijd uitvoeren, geeft u **9999-09-09** als de waarde voor de eigenschap **end** . Zie [JSON uitvoeren van scripts naslaginformatie](https://msdn.microsoft.com/library/dn835050.aspx) voor meer informatie over de eigenschappen van JSON.

    > [AZURE.NOTE] Als u invoer voor de AzureMLBatchExecution is activiteit optioneel. 

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Scenario: Proeven Reader/Writer Modules gebruiken om te verwijzen naar gegevens in verschillende gewoonlijk om de naam

Een andere gebruikelijk bij het maken van Azure ML experimenten is via de lezer en schrijver modules. De module reader wordt gebruikt om gegevens te laden in een experiment en de module schrijver gegevens van uw experimenten worden opgeslagen. Zie voor meer informatie over de lezer en schrijver modules, onderwerpen over de [lezer](https://msdn.microsoft.com/library/azure/dn905997.aspx) en [schrijver](https://msdn.microsoft.com/library/azure/dn905984.aspx) op MSDN-bibliotheek.     

Wanneer u met de lezer en schrijver modules, is het raadzaam om te gebruiken van een Web-service-parameter voor elke eigenschap van deze modules lezer/schrijver. Deze parameters web kunnen u de waarden tijdens runtime configureren. U kunt bijvoorbeeld een experiment maken met een reader-module die gebruikmaakt van een Azure SQL-Database: XXX.database.windows.net. Nadat de webservice is geïmplementeerd, die u wilt schakelen van de consumenten van de webservice om op te geven van een andere Azure SQL Server YYY.database.windows.net genoemd. U kunt een parameter van de service Web toe te staan dat deze waarde moet worden geconfigureerd.

> [AZURE.NOTE] Web service en uitvoer verschillen van de parameters van de Web-service. U hebt gezien hoe een invoer- en uitvoer voor een webservice Azure ML kunnen worden opgegeven in het eerste scenario. In dit scenario kunt u parameters voor een webservice die overeenkomen met eigenschappen van de lezer/schrijver modules doorgeven. 

Bekijk een scenario voor het gebruik van de parameters van de Web-service. U hebt een geïmplementeerd Azure Machine Learning-webservice die een module reader gebruikt om te lezen van gegevens uit een van de gegevensbronnen worden ondersteund door Azure Machine Learning (bijvoorbeeld: Azure SQL-Database). Nadat de batchuitvoering is uitgevoerd, worden de resultaten geschreven met een schrijver-module (Azure SQL-Database).  Geen web service invoer en uitvoer worden gedefinieerd in de experimenten. In dit geval is het raadzaam dat u relevante web serviceparameters voor de lezer en schrijver modules configureren. Deze configuratie kan de reader/writer modules bij gebruik van de activiteit AzureMLBatchExecution worden geconfigureerd. U opgeven Web serviceparameters in de sectie **globalParameters** in de activiteit JSON als volgt. 


    "typeProperties": {
        "globalParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }

U kunt ook [Gegevens Factory-functies](https://msdn.microsoft.com/library/dn835056.aspx) gebruiken in het doorgeven van waarden voor parameters van de webservice zoals weergegeven in het volgende voorbeeld:

    "typeProperties": {
        "globalParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Parameters van de webservice zijn hoofdlettergevoelig, dus zorg ervoor dat de namen die u opgeeft in de activiteit JSON overeenkomen met de gegevenstypen die worden aangeboden door de webservice. 

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Een module Reader gebruikt om te lezen van gegevens uit meerdere bestanden in Azure Blob
Grote gegevens pijpleidingen met activiteiten zoals varken en component kan leiden tot een of meer bestanden uitvoer zonder extensie. Wanneer u een externe component tabel opgeeft, kan er bijvoorbeeld de gegevens voor de externe component tabel worden opgeslagen in Azure-blobopslag met de volgende naam 000000_0. U kunt de module reader gebruiken in een experiment om meerdere bestanden te lezen en deze gebruiken voor voorspellingen. 

Wanneer u de module lezer in een experiment Azure Machine Learning, kunt u Azure Blob opgeven als invoer. De bestanden in de Azure-blobopslag kunnen zijn de uitvoerbestanden (voorbeeld: 000000_0) die worden geproduceerd door een varken en component script uitvoeren op HDInsight. De module reader kunt u bestanden (met zonder extensie) lezen door het configureren van de **pad naar de container, directory/blob**. Het **pad naar de container** -verwijst naar de container en **directory/blob** map die de bestanden bevat, zoals wordt weergegeven in de volgende afbeelding wordt verwezen. Het sterretje dat wil zeggen \*) **geeft aan dat alle bestanden in de container/tijdelijke map (dat wil zeggen gegevens-aggregateddata-jaar = 2014/maand-6 /\*)** als onderdeel van het experiment worden opgelezen.

![Azure Blob-eigenschappen](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)


### <a name="example"></a>Voorbeeld 
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Pijplijn met AzureMLBatchExecution activiteit met Web Service Parameters

    {
      "name": "MLWithSqlReaderSqlWriter",
      "properties": {
        "description": "Azure ML model with sql azure reader/writer",
        "activities": [
          {
            "name": "MLSqlReaderSqlWriterActivity",
            "type": "AzureMLBatchExecution",
            "description": "test",
            "inputs": [
              {
                "name": "MLSqlInput"
              }
            ],
            "outputs": [
              {
                "name": "MLSqlOutput"
              }
            ],
            "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
            "typeProperties":
            {
                "webServiceInput": "MLSqlInput",
                "webServiceOutputs": {
                    "output1": "MLSqlOutput"
                }
                "globalParameters": {
                    "Database server name": "<myserver>.database.windows.net",
                    "Database name": "<database>",
                    "Server user account name": "<user name>",
                    "Server user account password": "<password>"
                }              
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            },
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }
 
Klik in het voorbeeld hierboven JSON:

- De geïmplementeerd Azure Machine Learning webservice gebruikt een lezer en een module schrijver voor alleen-lezen/schrijven gegevens van/naar een Azure SQL-Database. Deze webservice heeft de volgende vier parameters: servernaam, de naam van de Database Server-account gebruikersnaam en wachtwoord voor een gebruikersaccount Server-Database.  
- **Begin** en **einde** datum/tijd moet [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601). Bijvoorbeeld: 2014-10-14T16:32:41Z. **De eindtijd** is optioneel. Als u geen waarde voor de eigenschap **end** opgeeft, wordt berekend als "**start + 48 uur.**" Als u wilt de pijplijn voor onbepaalde tijd uitvoeren, geeft u **9999-09-09** als de waarde voor de eigenschap **end** . Zie [JSON uitvoeren van scripts naslaginformatie](https://msdn.microsoft.com/library/dn835050.aspx) voor meer informatie over de eigenschappen van JSON.

### <a name="other-scenarios"></a>Andere scenario 's

#### <a name="web-service-requires-multiple-inputs"></a>Meerdere ingangen is vereist
Als de webservice meerdere ingangen duurt, gebruikt u de eigenschap **webServiceInputs** in plaats van **webServiceInput**. Gegevenssets waarnaar wordt verwezen door de **webServiceInputs** moeten ook worden opgenomen in de activiteit **invoeritems**.
 
In uw experiment Azure ML hebben web service invoer en uitvoerpoorten en globale parameters standaardnamen ("input1", "input2") die u kunt aanpassen. De namen die u voor webServiceInputs, webServiceOutputs en globalParameters instellingen gebruikt moeten precies overeenkomen met de namen in de experimenten. U kunt de steekproef verzoek nettolading bekijken op de pagina Batch Execution Help voor uw eindpunt Azure ML om te controleren of de verwachte toewijzing.


    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [{
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [{
                    "name": "inputDataset1"
                }, {
                    "name": "inputDataset2"
                }],
                "outputs": [{
                    "name": "outputDataset"
                }],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties": {
                    "webServiceInputs": {
                        "input1": "inputDataset1",
                        "input2": "inputDataset2"
                    },
                    "webServiceOutputs": {
                        "output1": "outputDataset"
                    }
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }

#### <a name="web-service-does-not-require-an-input"></a>Webservice is niet vereist voor een invoer

Azure ML batch execution-webservices kunnen worden gebruikt om uit te voeren, werkstromen, voor voorbeeld-R of Python scripts, die mogelijk niet nodig voor elke invoer. Of het experiment zijn geconfigureerd met een Reader-module die niet alle GlobalParameters worden. De activiteit AzureMLBatchExecution zou in dat geval als volgt worden geconfigureerd:

    {
        "name": "scoring service",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "myBlob"
            }
        ],
        "typeProperties": {
            "webServiceOutputs": {
                "output1": "myBlob"
            }              
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },
   

#### <a name="web-service-does-not-require-an-inputoutput"></a>Webservice is niet vereist voor een invoer/uitvoer
De webservice voor de uitvoering van Azure ML mogelijk geen webservice uitvoer geconfigureerd. In dit voorbeeld is er een webservice waarmee invoer of uitvoer, noch eventuele GlobalParameters zijn geconfigureerd. Er is nog steeds een uitvoer voor de activiteit zelf hebt geconfigureerd, maar deze niet wordt weergegeven als een webServiceOutput.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "placeholderOutputDataset"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Lezers van Web Service wordt gebruikt en schrijvers en de activiteit wordt uitgevoerd alleen wanneer andere activiteiten zijn geslaagd

De Azure ML web service reader schrijver modules en is mogelijk geconfigureerd om uit te voeren met of zonder een GlobalParameters. U kunt echter serviceaanvragen insluiten in een pijplijn waarbij gegevensset afhankelijkheden wordt gebruikt om aan te roepen van de service alleen wanneer sommige boven verwerking is voltooid. U kunt ook een andere actie activeren nadat de batchuitvoering is voltooid met deze methode. In dat geval kunt u de afhankelijkheden met activiteit invoer en uitvoer, zonder een van deze als webservice invoer of uitvoer van uitdrukken.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "inputs": [
            {
                "name": "upstreamData1"
            },
            {
                "name": "upstreamData2"
            }
        ],
        "outputs": [
            {
                "name": "downstreamData"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

De **takeaways** zijn:

-   Als uw eindpunt experiment gebruikmaakt van een webServiceInput: dit wordt aangeduid met een gegevensset blob en deel uitmaakt van de invoer van de activiteit en de eigenschap webServiceInput. Anders wordt de eigenschap webServiceInput weggelaten. 
-   Als uw eindpunt experiment gebruikmaakt van webServiceOutput(s): deze worden aangegeven met blob gegevenssets en zijn opgenomen in de uitvoer van de activiteit en in de eigenschap webServiceOutputs. Hiermee kunt u de activiteit en webServiceOutputs door de naam van elke uitvoer in het experiment zijn toegewezen. Anders wordt de eigenschap webServiceOutputs weggelaten.
-   Als uw eindpunt experiment globalParameter(s) biedt, zijn zijn ze opgegeven in de activiteit globalParameters eigenschap key, waardeparen. Anders wordt de eigenschap globalParameters weggelaten. De toetsen zijn hoofdlettergevoelig. [Azure gegevens Factory-functies](data-factory-scheduling-and-execution.md#data-factory-functions-reference) kunnen worden gebruikt in de waarden. 
- Extra gegevenssets kunnen worden opgenomen in de eigenschappen van de invoer en uitvoer voor activiteit, zonder waarnaar wordt verwezen in de typeProperties activiteit. Deze gegevenssets kan worden uitgevoerd segment afhankelijkheden bepalen maar anders door de activiteit AzureMLBatchExecution worden genegeerd. 


## <a name="updating-models-using-update-resource-activity"></a>Modellen, Update Resource activiteit met bijwerken
Over een periode moeten de blog modellen in de Azure ML scoren experimenten worden retrained nieuwe invoer gegevenssets opnieuw gebruiken. Nadat u klaar bent met nodig, die u wilt bijwerken van de score webservice met het retrained ML-model. De normale stappen voor het inschakelen van Azure ML modellen van opnieuw opleiden en bij te werken via webservices zijn: 

1. Maak een experiment in [Azure ML Studio](https://studio.azureml.net). 
2. Als u tevreden met het model bent, gebruikt u Azure ML Studio webservices voor beide de **training experimenteren** publiceren en scoren /**blog experiment**.

De volgende tabel beschrijft de webservices in dit voorbeeld gebruikt.  Zie [Retrain Machine Learning model via een programma](../machine-learning/machine-learning-retrain-models-programmatically.md) voor meer informatie.

| Type webservice | Beschrijving 
| :------------------ | :---------- 
| **Training voor webservice** | Trainingsgegevens worden ontvangen en ervaren modellen oplevert. De uitvoer van de bijscholing is een .ilearner-bestand in een Azure-blobopslag.  Het **standaardeindpunt** wordt automatisch voor u gemaakt wanneer u het experiment training als een webservice publiceert. U kunt meer eindpunten maken, maar in het voorbeeld wordt alleen het standaardeindpunt |
| **Score webservice** | Voorbeelden van zonder label gegevens ontvangt en ervoor zorgt dat voorspellingen. De uitvoer van tekstvoorspelling kan verschillende vormen, zoals een CSV-bestand of rijen in een Azure SQL-database, afhankelijk van de configuratie van het experiment hebben. De standaardeindpunt wordt automatisch voor u gemaakt wanneer u het blog experiment als een webservice publiceert. Maak het tweede **eindpunt voor niet-standaard en bij te werken** met behulp van de [Azure-portal](https://manage.windowsazure.com). U kunt meer eindpunten maken, maar in dit voorbeeld wordt slechts één niet-standaard die kunnen worden bijgewerkt eindpunt. Zie het artikel [Eindpunten maken](../machine-learning/machine-learning-create-endpoint.md) voor stapsgewijze instructies.       
 
De volgende afbeelding ziet u de relatie tussen training en eindpunten in Azure ML scoren. 

![Webservices](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)


U kunt de **webservice training** aanroepen met behulp van de **Azure ML Batch Execution activiteit**. Aanroepen van een webservice training komt overeen met het aanroepen van een Azure ML-webservice (scoren webservice) voor score gegevens. De voorgaande gedeelten duidelijk hoe u het aanroepen van een webservice Azure ML uit een verkooppijplijn Azure gegevens Factory uitgebreid beschreven. 
  
U kunt het **scoren webservice** aanroepen met behulp van de **Azure ML bijwerken Resource activiteit** bij de webservice met het zojuist ervaren model. Zoals in de bovenstaande tabel is vermeld, moet u maken en gebruiken van het eindpunt van niet-standaard-die kunnen worden bijgewerkt. Een bestaande gekoppelde services in uw fabriek gegevens naar het eindpunt van de niet-standaard gebruiken, zodat ze altijd de meest recente retrained model gebruiken ook bijwerken. 

De volgende scenario vindt u meer informatie. Een voorbeeld voor bijscholing en bijwerken van Azure ML modellen van een Azure gegevens Factory-verkooppijplijn heeft. 
 
### <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Scenario: bijscholing en een model Azure ML bijwerken
Deze sectie bevat een voorbeeld pijplijn waarin de **activiteit van Azure ML Batch Execution** voor een model retrain wordt gebruikt. De **activiteit van Azure ML bijwerken Resource** de pijplijn ook gebruikt voor het bijwerken van het model in de score webservice. De sectie bevat ook JSON fragmenten voor alle gekoppelde services, gegevenssets en verkooppijplijn in het voorbeeld. 

Hier ziet u de diagramweergave van de steekproef pijplijn. Zoals u ziet, wordt de activiteit Azure ML Batch Execution haalt de invoer van training en genereert een training-uitvoer (iLearner-bestand). De activiteit Azure ML Update Resource duurt de uitvoer van deze training en updates van het model in het score web service-eindpunt. De activiteit van de Resource Update levert geen uitvoer. De placeholderBlob is een pop-uitvoer gegevensset is vereist door de Azure gegevens Factory-service voor het uitvoeren van de pijplijn. 

![pijplijn-diagram](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


#### <a name="azure-blob-storage-linked-service"></a>Azure-blobopslag gekoppeld service:
De opslag van Azure bevat de volgende gegevens:

- trainingsgegevens. De ingevoerde gegevens voor de webservice van Azure ML training.  
- iLearner-bestand. De uitvoer van de Azure ML training-webservice. Dit bestand is ook de invoer voor de activiteit Update Resource.  
   
Hier ziet u de definitie van de JSON voorbeeld van de gekoppelde service: 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
            }
        }
    }


#### <a name="training-input-dataset"></a>Training voor het invoeren van de gegevensset:
De volgende gegevensset staat voor de invoer trainingsgegevens voor de webservice van Azure ML training. De activiteit Azure ML Batch Execution gaat dit gegevensset als invoer. 

    {
        "name": "trainingData",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "labeledexamples",
                "fileName": "labeledexamples.arff",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "policy": {          
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

#### <a name="training-output-dataset"></a>Training uitvoer gegevensset:
De volgende gegevensset staat voor het uitvoerbestand iLearner van de Azure ML training-webservice. De activiteit Azure ML Batch Execution levert dit gegevensset. Deze dataset is ook de invoer voor de activiteit Azure ML Update Resource.

    {
        "name": "trainedModelBlob",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "trainingoutput",
                "fileName": "model.ilearner",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            }
        }
    }

#### <a name="linked-service-for-azure-ml-training-endpoint"></a>Gekoppelde service voor Azure ML training eindpunt 
Het codefragment van de volgende JSON Hiermee definieert u een service van Azure Machine Learning gekoppeld die naar de standaardeindpunt van de webservice training verwijst. 

    {   
        "name": "trainingEndpoint",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
                "apiKey": "myKey"
            }
        }
    }

**Azure ML Studio**, moet u het volgende als u waarden voor **mlEndpoint** en **apiKey**wilt doen:

1. Klik op **WEB SERVICES** in het linkermenu.
2. Klik op de **webservice training** in de lijst met webservices. 
3. Klik op kopiëren naast het tekstvak **API-sleutel** . Plak de toets op het Klembord in de gegevens Factory JSON-editor.
4. Klik op **BATCH EXECUTION** koppeling in de **Azure ML studio**.
5. Kopieer de **URI aanvragen** van de sectie **aanvragen** en plak deze in de gegevens Factory JSON-editor.   


#### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Gekoppelde Service voor Azure ML die kunnen worden bijgewerkt score eindpunt:
Het codefragment van de volgende JSON Hiermee definieert u een service van Azure Machine Learning gekoppeld die naar de niet-standaard die kunnen worden bijgewerkt eindpunt van de score webservice verwijst.  

    {
        "name": "updatableScoringEndpoint2",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
                "apiKey": "endpoint2Key",
                "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
            }
        }
    }


Voordat maken en implementeren van een ML Azure gekoppeld service, volgt u de stappen in de artikel [Eindpunten maken](../machine-learning/machine-learning-create-endpoint.md) om een tweede (niet-standaard en bij te werken) eindpunt voor de score webservice te maken.

Nadat u het niet-standaard die kunnen worden bijgewerkt eindpunt hebt gemaakt, het volgende doen:

- Klik op **BATCH EXECUTION** om de waarde URI voor de **mlEndpoint** JSON-eigenschap.
- Klik op de koppeling **Bijwerken RESOURCE** om de waarde URI voor de **updateResourceEndpoint** JSON-eigenschap. De API-sleutel is op het eindpunt pagina zelf (in de rechterbenedenhoek). 

![eindpunt die kunnen worden bijgewerkt](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

 
#### <a name="placeholder-output-dataset"></a>Tijdelijke aanduiding voor uitvoer gegevensset:
De activiteit Azure ML Update Resource genereert geen uitvoer. Azure gegevens Factory is echter een gegevensset uitvoer aan de planning van een pijplijn wordt aangestuurd vereist. Daarom gebruiken we een gegevensset pop/tijdelijke aanduiding in dit voorbeeld.  

    {
        "name": "placeholderBlob",
        "properties": {
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "any",
                "format": {
                    "type": "TextFormat"
                }
            }
        }
    }


#### <a name="pipeline"></a>Verkooppijplijn
De pijplijn heeft twee activiteiten: **AzureMLBatchExecution** en **AzureMLUpdateResource**. De activiteit Azure ML Batch Execution trainingsgegevens als invoer haalt en genereert een iLearner-bestand als een uitvoer. De activiteit Hiermee wordt de webservice training (training experiment weergegeven als een webservice) met de trainingsgegevens van de invoer en het bestand ilearner ontvangt van de webservice. De placeholderBlob is een pop-uitvoer gegevensset is vereist door de Azure gegevens Factory-service voor het uitvoeren van de pijplijn. 

![pijplijn-diagram](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


    {
        "name": "pipeline",
        "properties": {
            "activities": [
                {
                    "name": "retraining",
                    "type": "AzureMLBatchExecution",
                    "inputs": [
                        {
                            "name": "trainingData"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "typeProperties": {
                        "webServiceInput": "trainingData",
                        "webServiceOutputs": {
                            "output1": "trainedModelBlob"
                        }              
                     },
                    "linkedServiceName": "trainingEndpoint",
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "02:00:00"
                    }
                },
                {
                    "type": "AzureMLUpdateResource",
                    "typeProperties": {
                        "trainedModelName": "Training Exp for ADF ML [trained model]",
                        "trainedModelDatasetName" :  "trainedModelBlob"
                    },
                    "inputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "placeholderBlob"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "name": "AzureML Update Resource",
                    "linkedServiceName": "updatableScoringEndpoint2"
                }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }


### <a name="reader-and-writer-modules"></a>Lezer en schrijver Modules

Een algemene scenario voor het gebruik van de parameters van de Web-service is het gebruik van Azure SQL-lezers en schrijvers. De module reader wordt gebruikt voor het laden van gegevens in een experiment van data management-services buiten Azure Machine Learning Studio. De module schrijver is gegevens uit uw experimenten in data management-services buiten Azure Machine Learning Studio opslaan.  

Zie voor meer informatie over Azure Blob/Azure SQL-lezer/schrijver, [lezer](https://msdn.microsoft.com/library/azure/dn905997.aspx) en [schrijver](https://msdn.microsoft.com/library/azure/dn905984.aspx) onderwerpen op MSDN-bibliotheek. Het voorbeeld in de vorige sectie gebruikt de lezer Azure Blob en Azure Blob schrijver. In deze sectie wordt beschreven hoe met Azure SQL-lezer en Azure SQL-schrijver.


## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Q:** Ik heb meerdere bestanden die zijn gegenereerd met mijn pijpleidingen groot gegevens. Kan ik de activiteit AzureMLBatchExecution gebruiken om te werken aan alle bestanden?

**A:** Ja. Zie de sectie **met een module Reader als u wilt lezen van gegevens uit meerdere bestanden in Azure Blob** voor meer informatie. 

## <a name="azure-ml-batch-scoring-activity"></a>Azure ML Batch scoren activiteit
Als u de activiteit **AzureMLBatchScoring** integreren met Azure Machine Learning gebruikt, is het raadzaam dat u de meest recente **AzureMLBatchExecution** activiteit gebruikt. 

De activiteit AzureMLBatchExecution is geïntroduceerd in de augustus 2015-versie van Azure SDK en Azure PowerShell.

Als u doorgaan met het gebruik van de activiteit AzureMLBatchScoring wilt, gaat u verder lezen door deze sectie.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Azure ML Batch scoren activiteit met behulp van Azure-opslag voor invoer/uitvoer 

    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchScoring",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "ScoringInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "ScoringResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

### <a name="web-service-parameters"></a>Parameters van Web-Service
Waarden voor Web serviceparameters wilt opgeven, een sectie **typeProperties** gedeelte toevoegen aan de **AzureMLBatchScoringActivty** in de pijplijn JSON zoals wordt weergegeven in het volgende voorbeeld: 

    "typeProperties": {
        "webServiceParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }


U kunt ook [Gegevens Factory-functies](https://msdn.microsoft.com/library/dn835056.aspx) gebruiken in het doorgeven van waarden voor parameters van de webservice zoals weergegeven in het volgende voorbeeld:

    "typeProperties": {
        "webServiceParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Parameters van de webservice zijn hoofdlettergevoelig, dus zorg ervoor dat de namen die u opgeeft in de activiteit JSON overeenkomen met de gegevenstypen die worden aangeboden door de webservice. 

## <a name="see-also"></a>Zie ook

- [Azure blogbericht: aan de slag met Azure gegevens fabriek en Azure Machine Learning](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)





[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/


 
