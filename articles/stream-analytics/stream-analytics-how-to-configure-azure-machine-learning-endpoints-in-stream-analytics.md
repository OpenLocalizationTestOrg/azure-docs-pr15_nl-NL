<properties 
    pageTitle="Het configureren van Azure Machine Learning-eindpunten in Stream Analytics | Microsoft Azure" 
    description="Machine Language door gebruiker gedefinieerde functies in Stream Analytics"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
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
    ms.author="jeffstok"
/>

# <a name="machine-learning-integration-in-stream-analytics"></a>Machine Learning-integratie in de Stream Analytics

Stream Analytics ondersteunt de gebruiker gedefinieerde functies die belt u af bij Azure Machine Learning-eindpunten. REST API-ondersteuning voor deze functie wordt beschreven in de [Stream Analytics REST API-bibliotheek](https://msdn.microsoft.com/library/azure/dn835031.aspx). In dit artikel vindt u aanvullende informatie nodig hebt voor een succesvolle implementatie van deze mogelijkheid in Stream Analytics. Een zelfstudie is ook geboekt en vindt u [hier](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Overzicht: Azure Machine Learning-terminologie

Microsoft Azure Machine Learning biedt een gezamenlijke, slepen en neerzetten hulpmiddel die u gebruiken kunt om te bouwen, testen en bekijk analyses oplossingen van uw gegevens implementeren. Dit programma heet het *Azure Machine Learning Studio*. De studio wordt gebruikt om te communiceren met de Machine trainingsmaterialen en eenvoudig te bouwen, testen en doorgaan met het ontwikkelen van uw ontwerp. Deze resources en de definities zijn onder.

- **Werkruimte**: de *werkruimte* is een container met alle andere Machine trainingsmaterialen samen in een container om te beheren.
- **Experiment**: *experimenten* worden gemaakt met gegevens wetenschappers gegevenssets opnieuw gebruiken en trainen van een machine learning-model.
- **Eindpunt**: *eindpunten* zijn het Azure Machine Learning-object gebruikt om te werken met functies als invoer, een opgegeven machine learning-model toepassen en uitvoer van een score voorzien.
- **Webservice scoren**: een *webservice scoren* is een verzameling eindpunten bovengenoemde.

Elk eindpunt heeft API's voor de batch worden uitgevoerd en synchrone uitvoering. Stream Analytics synchrone uitvoering gebruikt. De naam van de specifieke service is een [Service van de aanvraag en respons](../machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs) in AzureML studio.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Machine Learning-bronnen die nodig zijn voor taken van de Stream Analytics

Verwerking van een taak, een aanvraag en respons-eindpunt, een [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md#get-an-azure-machine-learning-authorization-key)en de definitie van een swagger zijn alle benodigde voor is uitgevoerd voor de toepassing van de Stream Analytics. Stream Analytics heeft een extra eindpunt dat wordt gemaakt van de url voor swagger eindpunt, zoekt naar bepaalde de interface en geeft als resultaat de definitie van een standaard-UDF aan de gebruiker.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Een Stream analyses en Machine Learning-UDF via REST API configureren

U kunt uw taak om te bellen van Azure Machine Language functies configureren met behulp van de REST API's. De stappen zijn als volgt:

1. Een taak Stream analyses maken
2. Een invoer definiëren
3. Een uitvoer definiëren
4. Een door de gebruiker gedefinieerde functie (UDF) maken
5. Een Stream Analytics-transformatie dat u de UDF belt schrijven
6. De taak starten

## <a name="creating-a-udf-with-basic-properties"></a>Een UDF maken met eenvoudige eigenschappen

Een scalaire UDF *newudf* waaraan een Azure Machine Learning-eindpunt is gebonden met de naam heeft als u bijvoorbeeld de volgende code gemaakt. Opmerking die het *eindpunt* (service URI) u op de pagina API help voor de door u gekozen service en de *apiKey vindt* vindt u op de hoofdpagina van de Services.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Voorbeeld van de aanvraag hoofdtekst:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Gesprek RetrieveDefaultDefinition eindpunt voor standaard UDF

Nadat het skelet UDF is gemaakt is de volledige definitie van de UDF nodig. Het eindpunt RetreiveDefaultDefinition krijgt u de standaarddefinitie voor een scalaire functie die is gekoppeld aan een Azure Machine Learning-eindpunt. De onderstaande nettolading moet u de standaard UDF definitie voor een scalaire functie die is gekoppeld aan een Azure Machine Learning-eindpunt. Dit opgeven niet het werkelijke eindpunt al zijn verschaft tijdens opslag-verzoek. Stream Analytics oproepen het eindpunt die beschikbaar zijn in het verzoek als deze expliciet is opgegeven. Als deze wordt gebruikt met degene die oorspronkelijk waarnaar wordt verwezen. De UDF krijgt één tekenreeks parameter (een zin) en geeft als resultaat een enkel uitvoer van het type tekenreeks hier waarmee wordt aangegeven met het label "sentiment" voor deze zin.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Voorbeeld van de aanvraag hoofdtekst:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Een voorbeeld weergeven van deze Zoek iets wilt hieronder.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-the-response"></a>Patch UDF met het antwoord 

Nu de UDF moet worden gerepareerd met het vorige antwoord, zoals hieronder wordt weergegeven.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

Hoofdgedeelte van de aanvraag (uitvoer van RetrieveDefaultDefinition):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Stream Analytics-transformatie om te bellen van de UDF implementeren

Nu de UDF (hier scoreTweet genoemd) voor elke invoer gebeurtenis query en schrijft u een antwoord voor die gebeurtenis aan een uitvoer.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
