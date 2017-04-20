<properties 
    pageTitle="Pijpleidingen maken/planning, activiteiten in gegevens fabriek koppelen | Microsoft Azure" 
    description="Leer hoe u een pijplijn gegevens maken in Azure gegevens Factory verplaatsen en transformeren van gegevens. Maak een gegevensgestuurde zodat het eindresultaat gereed om informatie te gebruiken." 
    keywords="gegevens verkooppijplijn, gegevensgestuurde werkstroom"
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
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="pipelines-and-activities-in-azure-data-factory"></a>Pijpleidingen en activiteiten in Azure gegevens fabriek
In dit artikel vindt u begrijpen pijpleidingen en activiteiten in Azure gegevens fabriek en deze gebruiken om samen te stellen end-to-end gegevensgestuurde werkstromen voor uw gegevens verplaatsen en gegevensverwerking scenario's.  

> [AZURE.NOTE] In dit artikel wordt ervan uitgegaan dat u [Inleiding tot Azure gegevens Factory](data-factory-introduction.md)hebt doorlopen. Als u geen hands-op-ervaring hebt met het maken van gegevens factory's, via [uw eerste gegevens factory bouwen](data-factory-build-your-first-pipeline.md) zelfstudie waarmee u in dit artikel beter te begrijpen.  

## <a name="what-is-a-data-pipeline"></a>Wat is een pijplijn gegevens?
**Pijplijn** is een groep logisch gerelateerde **activiteiten**. Wordt gebruikt voor groepsactiviteiten in een eenheid groeperen waarmee een taak wordt uitgevoerd. Pijpleidingen beter begrip, moet u eerst een activiteit begrijpen. 

## <a name="what-is-an-activity"></a>Wat is een activiteit?
Activiteiten definiëren de acties uit te voeren op uw gegevens. Elke activiteit nul of meer [gegevenssets](data-factory-create-datasets.md) als invoer wordt ingevoegd en genereert een of meer gegevenssets als uitvoer. 

U kunt bijvoorbeeld een activiteit in een kopie kopiëren van gegevens uit één gegevensopslag naar een andere gegevensopslag goedkeuringen. U kunt ook een activiteit in een HDInsight-component een component-query uitvoeren op een cluster Azure HDInsight om uw gegevens te transformeren. Azure gegevens Factory biedt een groot aantal [gegevenstransformatie](data-factory-data-transformation-activities.md)en [verplaatsing van gegevens](data-factory-data-movement-activities.md) activiteiten. U kunt er ook voor kiezen om te maken van een aangepaste .NET-activiteit als uw eigen code wilt uitvoeren. 

## <a name="sample-copy-pipeline"></a>Voorbeeld kopiëren verkooppijplijn
In het volgende voorbeeld pijplijn is er een activiteit van het type **kopiëren** in de sectie **activiteiten** . In dit voorbeeld wordt de [activiteit kopiëren](data-factory-data-movement-activities.md) opgehaald gegevens uit een Azure-blobopslag met een Azure SQL-database. 

    {
      "name": "CopyPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2016-07-12T00:00:00Z",
        "end": "2016-07-13T00:00:00Z"
      }
    } 

Houd rekening met de volgende punten:

- Klik in de sectie activiteiten is er slechts één activiteit waarvan het **type** is ingesteld op **kopiëren**.
- Invoer voor de activiteit is ingesteld op **InputDataset** en uitvoer voor de activiteit is ingesteld op **OutputDataset**.
- Klik in de sectie **typeProperties** **BlobSource** is opgegeven als het brontype en **SqlSink** is opgegeven als het type sink.

Zie voor een volledige stapsgewijze instructies voor het maken van deze verkooppijplijn, [Zelfstudie: gegevens uit Blob Storage kopiëren met SQL-Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="sample-transformation-pipeline"></a>Voorbeeld transformatie verkooppijplijn
In het volgende voorbeeld pijplijn is er een activiteit van het type **HDInsightHive** in de sectie **activiteiten** . In dit voorbeeld wordt transformaties de [HDInsight component activiteit](data-factory-hive-activity.md) gegevens uit een Azure-blobopslag door een component script-bestand in een cluster Azure HDInsight Hadoop uit te voeren. 

    {
        "name": "TransformPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }

Houd rekening met de volgende punten: 

- Klik in de sectie activiteiten is er slechts één activiteit waarvan het **type** is ingesteld op **HDInsightHive**.
- De component-scriptbestand, **partitionweblogs.hql**, is opgeslagen in de opslag van Azure-account (opgegeven door de scriptLinkedService, **AzureStorageLinkedService**genoemd), en in de map **script** in de container **adfgetstarted**.
- De sectie **definieert** wordt gebruikt om op te geven van de runtime-instellingen die worden doorgegeven aan het script component als component configuratiewaarden (bijvoorbeeld ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

Zie voor een volledige stapsgewijze instructies voor het maken van deze verkooppijplijn, [Zelfstudie: uw eerste verkooppijplijn om gegevens te verwerken met Hadoop cluster bouwen](data-factory-build-your-first-pipeline.md). 

## <a name="chaining-activities"></a>Activiteiten koppelen
Als er meerdere activiteiten in een pijplijn en uitvoer van een activiteit niet een invoer van een andere activiteit is, kunnen de activiteiten in parallel uitgevoerd als invoergegevens segmenten van de activiteiten klaar bent. 

U kunt twee activiteiten koppelen doordat de gegevensset uitvoer van de ene activiteit als de invoer gegevensset van de andere activiteit. De activiteiten kunnen werken in de pijplijn dezelfde of verschillende pijpleidingen. De tweede activiteit wordt uitgevoerd alleen wanneer de eerste fase is voltooid. 

Houd rekening met het volgende geval bijvoorbeeld:
 
1.  Verkooppijplijn P1 heeft activiteit A1, dat is vereist externe invoer gegevensset D1 en **uitvoer** gegevensset **D2**produceren.
2.  Verkooppijplijn P2 heeft activiteit A2 die is vereist **invoer** van de gegevensset **D2**en genereert uitvoer gegevensset D3.
 
In dit scenario de activiteit A1 wordt uitgevoerd wanneer de externe gegevens beschikbaar is en de beschikbaarheid van de geplande frequentie is bereikt.  De activiteit A2 wordt uitgevoerd wanneer de geplande segmenten uit D2 beschikbaar en de beschikbaarheid van de geplande frequentie is bereikt. Als er een fout in een van de segmenten in gegevensset D2, wordt niet A2 uitgevoerd voor segment totdat deze verandert in beschikbaar.

Diagramweergave:

![Activiteiten in twee pijpleidingen koppelen](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

Diagramweergave met beide activiteiten in de dezelfde pijplijn: 

![Activiteiten in de dezelfde pijplijn koppelen](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

Zie [plannen en uitvoeren](#chaining-activities)voor meer informatie. 

## <a name="scheduling-and-execution"></a>Plannings- en kan worden uitgevoerd
U hebt dusverre begrepen wat pijpleidingen en activiteiten zijn. U hebt ook gekeken ze hoe gedefinieerd en een hoog niveau worden weergave van de activiteiten in fabriek van Azure-gegevens. Nu we bekijken hoe ze worden uitgevoerd. 

Een pijplijn is actieve alleen tussen de begintijd en eindtijd. Het is niet uitgevoerd voor de begintijd of na de eindtijd. Als de pijplijn is onderbroken, wordt deze niet uitgevoerd ongeacht de begin- en -tijd. Voor een pijplijn om uit te voeren, moet deze niet worden onderbroken. Het is in feite niet de pijplijn die wordt uitgevoerd. Dit is de activiteiten in de pijplijn die wordt uitgevoerd. Echter ze doen in de context van de pijplijn. 

Zie [plannen en uitvoeren](data-factory-scheduling-and-execution.md) voor meer informatie over hoe plannings- en kan worden uitgevoerd in Azure gegevens fabriek werkt.

## <a name="create-pipelines"></a>Pijpleidingen maken
Azure gegevens Factory biedt verschillende methoden om te ontwerpen en implementeren van pijpleidingen (die op zijn beurt bevatten een of meer activiteiten in deze). 

### <a name="using-azure-portal"></a>Met behulp van Azure portal
U kunt gegevens Factory-editor in de portal van Azure een pijplijn maken. Zie [aan de slag met Azure gegevens Factory (gegevens Factory-Editor)](data-factory-build-your-first-pipeline-using-editor.md) voor een end-to-end Stapsgewijze instructies. 

### <a name="using-visual-studio"></a>Gebruik van Visual Studio 
U kunt Visual Studio ontwerpen en implementeren van pijpleidingen naar Factory van Azure-gegevens. Zie [aan de slag met Azure gegevens Factory (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) voor een end-to-end Stapsgewijze instructies. 

### <a name="using-azure-powershell"></a>Via Azure PowerShell
U kunt de PowerShell Azure pijpleidingen maken in fabriek van Azure-gegevens. U kunt de pijplijn JSON zeggen hebt gedefinieerd in een bestand op c:\DPWikisample.json. U kunt deze uploaden naar uw exemplaar van de Azure gegevens Factory zoals wordt weergegeven in het volgende voorbeeld:

    New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -Name DPWikisample -DataFactoryName wikiADF -File c:\DPWikisample.json

Zie [aan de slag met Azure gegevens Factory (Azure PowerShell)](data-factory-build-your-first-pipeline-using-powershell.md) voor een end-to-end Stapsgewijze instructies voor het maken van een fabriek gegevens met een pijplijn. 

### <a name="using-net-sdk"></a>Met behulp van .NET SDK
U kunt maken en verkooppijplijn via .NET SDK te implementeren. Deze methode kan worden gebruikt om te pijpleidingen via programmacode maken. Zie [maken, beheren, en gegevens factory's via programmacode controleren](data-factory-create-data-factories-programmatically.md)voor meer informatie. 


### <a name="using-azure-resource-manager-template"></a>Azure resourcemanager sjabloon gebruiken
U kunt maken en implementeren van met een sjabloon van Azure resourcemanager verkooppijplijn. Zie [aan de slag met Azure gegevens Factory (Azure resourcemanager)](data-factory-build-your-first-pipeline-using-arm.md)voor meer informatie. 

### <a name="using-rest-api"></a>REST API gebruiken
U kunt maken en implementeren van verkooppijplijn REST API's te gebruiken. Deze methode kan worden gebruikt om te pijpleidingen via programmacode maken. Zie [maken of bijwerken een pijplijn](https://msdn.microsoft.com/library/azure/dn906741.aspx)voor meer informatie. 


## <a name="monitor-and-manage-pipelines"></a>Bewaken en pijpleidingen beheren  
Als een pijplijn wordt geïmplementeerd, kunt u beheren en controleren van uw pijpleidingen, segmenten en wordt uitgevoerd. Meer informatie over deze hier: [Monitor en pijpleidingen beheren](data-factory-monitor-manage-pipelines.md).


## <a name="pipeline-json"></a>Verkooppijplijn JSON   
Laat ons nader op hoe een pijplijn wordt gedefinieerd in de indeling van JSON. De structuur van de algemene voor een pijplijn ziet er als volgt uit:

    {
        "name": "PipelineName",
        "properties": 
        {
            "description" : "pipeline description",
            "activities":
            [
    
            ],
            "start": "<start date-time>",
            "end": "<end date-time>"
        }
    }

De sectie **activiteiten** kan een of meer activiteiten die zijn gedefinieerd in het bevatten. Elke activiteit heeft de volgende op het hoogste niveau structuur:

    {
        "name": "ActivityName",
        "description": "description", 
        "type": "<ActivityType>",
        "inputs":  "[]",
        "outputs":  "[]",
        "linkedServiceName": "MyLinkedService",
        "typeProperties":
        {
    
        },
        "policy":
        {
        }
        "scheduler":
        {
        }
    }

Na de tabel de eigenschappen binnen de definities van de JSON activiteit en verkooppijplijn wordt beschreven:

Tag | Beschrijving | Vereist
--- | ----------- | --------
naam | Naam van de activiteit of de pijplijn. Geef een naam voor de actie die dat de activiteit of de verkooppijplijn is geconfigureerd om uit te voeren<br/><ul><li>Maximum aantal tekens: 260</li><li>Moet beginnen met een letter getal of een onderstrepingsteken (_)</li><li>Volgende tekens zijn niet toegestaan: ". ', 'en',"?","/","< "," >"," * ","%","&",": ","\\"</li></ul> | Ja
Beschrijving | Beschrijving van wat de activiteit of verkooppijplijn wordt gebruikt voor | Ja
type | Geeft het type van de activiteit. Zie de artikelen [Gegevens verkeer activiteiten](data-factory-data-movement-activities.md) en [Gegevens transformatie activiteiten](data-factory-data-transformation-activities.md) voor verschillende soorten activiteiten. | Ja
ingangen | Invoer tabellen die worden gebruikt door de activiteit<br/><br/>een invoeren tabel<br/>"invoer": [{"naam": "inputtable1"}],<br/><br/>twee invoer tabellen <br/>"invoer": [{"naam": "inputtable1"}, {"naam": "inputtable2"}], | Ja
uitvoer | Uitvoertabellen die worden gebruikt door de uitvoer van activity.// een tabel<br/>"uitvoer": [{"naam": "outputtable1"}],<br/><br/>twee tabellen<br/>"uitvoer": [{"naam": "outputtable1"}, {"naam": "outputtable2"}], | Ja
linkedServiceName | Naam van de gekoppelde service wordt gebruikt door de activiteit. <br/><br/>Een activiteit mogelijk nodig dat u opgeeft dat de gekoppelde service die is gekoppeld aan de vereiste berekeningscluster-omgeving. | Ja voor HDInsight activiteit en Azure Machine Learning-Batch scoren activiteit <br/><br/>Niet voor alle andere
typeProperties | Eigenschappen in de sectie typeProperties, is afhankelijk van het type van de activiteit. | Nee
beleid | Beleidsregels die betrekking hebben op het gedrag van de runtime van de activiteit. Als dit niet is opgegeven, worden standaardbeleidsregels gebruikt. | Nee
starten | Begindatum-tijd voor de pijplijn. Moeten zich in het [ISO-notatie](http://en.wikipedia.org/wiki/ISO_8601). Bijvoorbeeld: 2014-10-14T16:32:41Z. <br/><br/>Het is mogelijk om op te geven van een lokale tijd, bijvoorbeeld een geschatte-tijd. Hier volgt een voorbeeld: "2016-02-27T06:00:00**-05: 00**', namelijk 6 AM EST.<br/><br/>De eigenschappen van het begin en einde opgeven samen actieve periode voor de pijplijn. Uitvoer segmenten worden alleen met geproduceerd in deze actieve periode. | Nee<br/><br/>Als u een waarde voor de eigenschap end opgeeft, moet u de waarde voor de eigenschap starten.<br/><br/>De begin- en eindtijden kunnen beide niet leeg zijn om te maken van een pijplijn. Moet u beide waarden om in te stellen van een actieve periode voor de pijplijn om uit te voeren. Als u geen begin- en eindtijden opgeeft wanneer u een pijplijn maakt, kunt u deze met de cmdlet Set-AzureRmDataFactoryPipelineActivePeriod later instellen.
einde | Datum-tijd voor de pijplijn beëindigen. Als u opgeeft moet ISO-indeling. Bijvoorbeeld: 2014-10-14T17:32:41Z <br/><br/>Het is mogelijk om op te geven van een lokale tijd, bijvoorbeeld een geschatte tijd. Hier volgt een voorbeeld: "2016-02-27T06:00:00**-05: 00**', namelijk 6 AM EST.<br/><br/>Als u wilt de pijplijn voor onbepaalde tijd uitvoert, geeft u 9999-09-09 als de waarde voor de eigenschap end. | Nee <br/><br/>Als u een waarde voor de eigenschap start opgeeft, moet u de waarde voor de eigenschap end opgeven.<br/><br/>Zie Opmerkingen voor de eigenschap **starten** .
isPaused | Als ingesteld op waar de pijplijn niet wordt uitgevoerd. Standaardwaarde = false. U kunt deze eigenschap in- of uitschakelen. | Nee 
planner | "scheduler" eigenschap wordt gebruikt om te definiëren gewenste plannen voor de activiteit. Bijbehorende subeigenschappen zijn hetzelfde als de kleuren die in de [beschikbaarheid van de eigenschap in een gegevensset](data-factory-create-datasets.md#Availability). | Nee |   
| pipelineMode | De methode voor het plannen wordt uitgevoerd voor de pijplijn. Toegestane waarden zijn: gepland (standaard), eenmalige.<br/><br/>'Gepland' wordt aangegeven dat de pijplijn wordt uitgevoerd op een bepaald tijdsinterval op basis van de actieve periode (begin- en -tijd). 'Eenmalige' wordt aangegeven dat de pijplijn slechts één keer wordt uitgevoerd. Eenmalige pijpleidingen eenmaal gemaakt mogen niet gewijzigd/bijgewerkt momenteel. Zie [Onetime pijplijn](data-factory-scheduling-and-execution.md#onetime-pipeline) voor meer informatie over het eenmalige instellen. | Nee | 
| expirationTime | De duur van de tijd na het maken waarvoor de pijplijn geldig is en ingerichte moet blijven. Als er geen ieder actief is mislukt, of in behandeling wordt uitgevoerd, de pijplijn wordt automatisch verwijderd wanneer de verlooptijd worden bereikt. | Nee | 
| gegevenssets | Lijst met gegevenssets om te worden gebruikt door activiteiten die zijn gedefinieerd in de pijplijn. Deze eigenschap kan worden gebruikt om te definiëren gegevenssets die specifiek zijn voor deze verkooppijplijn en niet in de fabriek gegevens zijn gedefinieerd. Gegevenssets gedefinieerd binnen deze verkooppijplijn kunnen alleen worden gebruikt door deze verkooppijplijn en kan niet worden gedeeld. Zie [binnen bereik van gegevenssets](data-factory-create-datasets.md#scoped-datasets) voor meer informatie.| Nee |  
 

### <a name="policies"></a>Beleidsregels
Beleidsregels van invloed op het gedrag van de uitvoering van een activiteit, specifiek wanneer het segment van een tabel wordt verwerkt. De volgende tabel bevat de informatie.

Eigenschap | Toegestane waarden | Standaardwaarde | Beschrijving
-------- | ----------- | -------------- | ---------------
bij gelijktijdigheid | Geheel getal <br/><br/>De maximale waarde: 10 | 1 | Het aantal gelijktijdige executions van de activiteit.<br/><br/>Bepaalt het aantal parallelle activiteit executions die aan andere segmenten kan worden gewerkt. Bijvoorbeeld als een activiteit leest u moet over sneller een groot aantal beschikbare gegevens, met een hogere waarde op bij gelijktijdigheid de verwerking van gegevens. 
executionPriorityOrder | NewestFirst<br/><br/>OldestFirst | OldestFirst | Bepaalt de volgorde van segmenten van gegevens die worden verwerkt.<br/><br/>Bijvoorbeeld, hebt u 2 segmenten (één foutbericht gegeven om 4 uur en een tijdelijke aanduiding aan 5 pm) en beide zijn in afwachting van uitvoering. Als u de executionPriorityOrder moeten NewestFirst instelt, worden eerst het segment om 5 uur verwerkt. Op dezelfde manier als u de executionPriorityORder OldestFIrst worden ingesteld, wordt klikt u vervolgens het segment om 16: 00 verwerkt. 
Probeer het opnieuw | Geheel getal<br/><br/>Max-waarden zijn 10 | 3 | Het aantal pogingen voordat de verwerking van gegevens voor het segment is gemarkeerd als is mislukt. Uitvoering van de activiteit voor een segment gegevens opnieuw tot aan het aantal opgegeven nieuwe pogingen wordt gestart. De nieuwe poging klaar is zo snel mogelijk na de fout.
time-out | Tijdspanne | 00:00:00 | Time-out voor de activiteit. Voorbeeld: 00:10:00 (betekent time-out 10 minuten)<br/><br/>Als een waarde niet wordt opgegeven of gelijk aan 0 is, is de time-out oneindig.<br/><br/>Als u de verwerkingstijd van gegevens op een segment groter is dan de time-outwaarde, deze is geannuleerd en wordt geprobeerd de verwerking opnieuw uit te voeren. Het aantal pogingen, is afhankelijk van de eigenschap opnieuw. Wanneer time-out optreedt, wordt de status is ingesteld op time-out.
vertraging | Tijdspanne | 00:00:00 | Geef de begintijd vertragen voordat gegevensverwerking van het segment wordt gestart.<br/><br/>De uitvoering van activiteiten voor een segment gegevens wordt gestart nadat de vertraging verstreken de verwachte execution-tijd is.<br/><br/>Voorbeeld: 00:10:00 (betekent vertraging van 10 minuten)
longRetry | Geheel getal<br/><br/>De maximale waarde: 10 | 1 | Het aantal lang herhalingspogingen voordat de uitvoering van het segment is mislukt.<br/><br/>longRetry pogingen gelijkmatig verdeeld door longRetryInterval. Als u nodig hebt om op te geven van een tijd tussen nieuwe pogingen, gebruikt u dus longRetry. Als zowel opnieuw en longRetry zijn opgegeven, elke poging longRetry herhalingspogingen bevat en is het maximum aantal pogingen opnieuw * longRetry.<br/><br/>Stel dat we hebben de volgende instellingen in het beleid activiteit:<br/>Probeer: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Wordt ervan uitgegaan dat er is slechts één segment uit te voeren (status Wacht) of de uitvoering van de activiteit niet meer elke keer. In eerste instantie zou er 3 opeenvolgende execution pogingen. Na elke poging zou het segment status opnieuw. Nadat de eerste 3 pogingen is overschreden, is de status segment zou LongRetry.<br/><br/>Na een uur (dat wil zeggen de longRetryInteval waarde), zou er een andere set 3 opeenvolgende execution pogingen. De status segment zou worden niet daarna en geen pogingen meer zou worden uitgevoerd. Daarom zijn algemene 6 pogingen aangebracht.<br/><br/>Als een execution is geslaagd, zou de status segment bij de hand en geen pogingen meer worden uitgevoerd.<br/><br/>longRetry mag worden gebruikt in situaties waarin afhankelijke gegevens geen tijde binnenkomt of de algehele omgeving is flaky onder welke gegevensverwerking plaatsvindt. Tijd resulteert in de gewenste uitvoer in dat geval pogingen achter elkaar niet kunt doen, en dit na een tijdsinterval.<br/><br/>Waarschuwing: Stel niet hoge waarden voor longRetry of longRetryInterval. Hoge waarden geven meestal andere systematische problemen. 
longRetryInterval | Tijdspanne | 00:00:00 | De vertraging op tussen lange herhalingspogingen 


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [planning en kan worden uitgevoerd in fabriek van Azure-gegevens](data-factory-scheduling-and-execution.md).  
- Lees meer over de [verplaatsing van gegevens](data-factory-data-movement-activities.md) en een [transformatie mogelijkheden om gegevens](data-factory-data-transformation-activities.md) in fabriek van Azure-gegevens
- Meer informatie over [management en in fabriek van Azure-gegevens bewaken](data-factory-monitor-manage-pipelines.md).
- [Bouwen en implementeren van uw eerste verkooppijplijn](data-factory-build-your-first-pipeline.md). 
