<properties
    pageTitle="Uw eerste gegevens factory (Visual Studio) bouwen | Microsoft Azure"
    description="In deze zelfstudie maakt u een voorbeeld Azure gegevens Factory pijplijn gebruik van Visual Studio."
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
    ms.topic="hero-article" 
    ms.date="10/17/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-azure-first-data-factory-using-microsoft-visual-studio"></a>Zelfstudie: Uw eerste Azure opbouwen gegevens factory met Microsoft Visual Studio
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-build-your-first-pipeline.md)
- [Azure-portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resourcemanager-sjabloon](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

In dit artikel kunt u Microsoft Visual Studio uw eerste Azure gegevens factory maken.

## <a name="prerequisites"></a>Vereisten voor
1. [Zelfstudie overzicht](data-factory-build-your-first-pipeline.md) artikel en voer de **vereiste** stappen uit.
2. U moet een **beheerder van het abonnement dat Azure** moeten kunnen gegevens Factory entiteiten in Visual Studio Azure gegevens Factory publiceren.
3. U moet de volgende manieren te werk op uw computer is geïnstalleerd: 
    - Visual Studio 2013 of Visual Studio-2015
    - Download Azure SDK voor Visual Studio 2013 of Visual Studio-2015. Navigeer naar de [Pagina voor het downloaden van Azure](https://azure.microsoft.com/downloads/) en op **tegenover 2013** of **tegenover 2015** in de sectie **.NET** .
    - Download de meest recente Azure gegevens Factory-invoegtoepassing voor Visual Studio: [tegenover 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) of [tegenover 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). U kunt de invoegtoepassing ook bijwerken door het volgende te doen: klik op **Extra**in het menu -> **Extensions en Updates** -> **Online** -> **Visual Studio-galerie** -> **Microsoft Azure gegevens Factory Tools voor Visual Studio** -> **bijwerken**. 
 
Nu kunnen we Visual Studio gebruiken om te maken van een fabriek Azure-gegevens. 


## <a name="create-visual-studio-project"></a>Visual Studio project maken 
1. Start de **Visual Studio 2013** of **Visual Studio-2015**. Klik op **bestand**, wijs **Nieuw**aan en klik op **Project**. Hier ziet u het dialoogvenster **Nieuw Project** .  
2. Klik in het dialoogvenster **Nieuw Project** selecteert u de sjabloon **DataFactory** en op **Lege gegevens Factory-Project**.   

    ![Dialoogvenster Nieuw project](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)

3. Voer een **naam** voor het project, **locatie**en een naam voor de **oplossing**en klik op **OK**.

    ![Oplossing Explorer](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

## <a name="create-linked-services"></a>Gekoppelde services maken
Een factory gegevens kan een of meer pijpleidingen bevatten. Een pijplijn kan een of meer activiteiten in deze hebben. Bijvoorbeeld een activiteit kopie gegevens uit een bron kopiëren naar een gegevensopslag bestemming en de activiteit in een HDInsight component naar component script uitvoeren om invoergegevens transformeren. Zie [ondersteunde gegevens worden opgeslagen](data-factory-data-movement-activities.md##supported-data-stores-and-formats) voor alle bronnen en sinks worden ondersteund door de activiteit kopiëren. Zie voor de lijst met berekeningscluster services worden ondersteund door gegevens Factory [gekoppelde services berekenen](data-factory-compute-linked-services.md) . 

In deze stap geeft u een uw opslagruimte Azure-account en een op aanvraag Azure HDInsight cluster koppeling naar uw gegevens factory. De opslag van Azure-account bevat de invoer- en uitvoerbereik gegevens voor de pijplijn in dit voorbeeld. De HDInsight gekoppeld-service wordt gebruikt om uit te voeren component script die zijn opgegeven in de activiteit van de pijplijn in dit voorbeeld. Bepalen welke gegevens store/berekeningscluster services worden gebruikt in uw scenario en deze services met het maken van gekoppelde services koppelen aan de fabriek gegevens.  

U opgeven de naam en de instellingen voor de fabriek gegevens later wanneer u uw gegevens Factory-oplossing publiceert.

#### <a name="create-azure-storage-linked-service"></a>Azure gekoppeld opslagservice maken
In deze stap kunt u uw Azure Storage-account koppelen aan uw factory gegevens. Voor deze zelfstudie gebruikt u hetzelfde account Azure Storage invoer/uitvoergegevens en de HQL script-bestand wilt opslaan. 

4. Met de rechtermuisknop op de **Gekoppelde Services** in de solution explorer, wijs **toevoegen**en klik op **Nieuw Item**.      
5. In het dialoogvenster **Nieuw Item toevoegen** , selecteer **Gekoppelde Azure Storage-Service** in de lijst en klikt u op **toevoegen**. 
3. **Accountnaam** en **accountkey** vervangen door de naam van uw account Azure opslag en de sleutel. Als u wilt leren hoe u uw opslagruimte toegangstoets, raadpleegt u [weergave, kopiëren en opnieuw genereren opslag-toegangstoetsen](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)

    ![Azure opslag gekoppeld Service](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)

4. Sla het bestand **AzureStorageLinkedService1.json** .

#### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight gekoppeld service maken
In deze stap kunt u een op aanvraag HDInsight cluster koppeling naar uw gegevens factory. Het cluster HDInsight wordt automatisch gemaakt tijdens runtime en is verwijderd nadat deze verwerking en niet-actieve voor de opgegeven tijdsduur is voltooid. U kunt uw eigen HDInsight cluster in plaats van een cluster op aanvraag-HDInsight. Zie [Gekoppelde Services berekenen](data-factory-compute-linked-services.md) voor meer informatie. 

1. Klik in de **Oplossingverkenner**met de rechtermuisknop op **Gekoppelde Services**, wijs **toevoegen**en klik op **Nieuw Item**.
2. Selecteer **HDInsight op aanvraag gekoppelde Service**en klikt u op **toevoegen**. 
3. De **JSON** vervangen door het volgende:

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService1"
            }
          }
        }
    
    De volgende tabel vindt u beschrijvingen voor de JSON-eigenschappen in het fragment gebruikt:
    
    Eigenschap | Beschrijving
    -------- | -----------
    Versie | Geeft aan dat de versie van de HDInsight 3,2 worden gemaakt. 
    ClusterSize | Hiermee geeft u de grootte van het cluster HDInsight. 
    TimeToLive | Geeft aan dat het niet-actieve tijd voor het cluster HDInsight voordat deze wordt verwijderd.
    linkedServiceName | Hiermee geeft u de opslag-account dat wordt gebruikt voor de opslag van de logboekbestanden die zijn gegenereerd met HDInsight

    Houd rekening met het volgende: 
    
    - De gegevens fabriek wordt een HDInsight **op basis van Windows** cluster voor u gemaakt met de voorgaande JSON. U kunt ook het maken van een HDInsight **Linux gebaseerde** cluster hebben. Zie [Op aanvraag HDInsight gekoppelde Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie. 
    - U kunt **uw eigen HDInsight cluster** in plaats van een cluster op aanvraag-HDInsight. Zie [Gekoppelde HDInsight-Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) voor meer informatie.
    - Het cluster HDInsight Hiermee maakt u een **standaard-container** in de blobopslag die u hebt opgegeven in de JSON (**linkedServiceName**). HDInsight worden deze container niet verwijderd wanneer het cluster wordt verwijderd. Dit probleem is inherent aan het ontwerp. Met op aanvraag HDInsight gekoppeld-service, een HDInsight cluster gemaakt telkens wanneer een segment is verwerkt, tenzij er een bestaand live cluster (**timeToLive is**). Het cluster wordt automatisch verwijderd wanneer de verwerking klaar is.
    
        Als u meer segmenten worden verwerkt, ziet u veel containers in uw Azure-blobopslag. Als u niet nodig ze hebt voor het oplossen van problemen met de taken, wilt u mogelijk verwijdert u deze als u wilt doen om de opslag te beperken. De namen van deze containers een patroon volgen: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Gebruik hulpprogramma's zoals [Microsoft opslag Explorer](http://storageexplorer.com/) containers in uw Azure-blobopslag verwijderen.

    Zie [Op aanvraag HDInsight gekoppelde Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie. 
4. Sla het bestand **HDInsightOnDemandLinkedService1.json** .

## <a name="create-datasets"></a>Gegevenssets maken
In deze stap maakt u gegevenssets om te staan voor de invoer en uitvoer van gegevens voor de verwerking van de component. Deze gegevenssets raadpleegt u de **AzureStorageLinkedService1** die u eerder in deze zelfstudie hebt gemaakt. De gekoppelde service wordt verwezen in een opslag van Azure-account en gegevenssets container, map, bestandsnaam opgeven in de opslagruimte waarin invoer en gegevens.   

#### <a name="create-input-dataset"></a>Invoer gegevensset maken

1. Klik in de **Oplossingverkenner**met de rechtermuisknop op **tabellen**, wijs **toevoegen**en klik op **Nieuw Item**. 
2. Selecteer **Azure Blob** in de lijst, wijzig de naam van het bestand in **InputDataSet.json**en klikt u op **toevoegen**.
3. De **JSON** in de editor vervangen door het volgende: 

    In het fragment JSON maakt u een gegevensset **AzureBlobInput** met invoergegevens voor een activiteit in de pijplijn genoemd. Daarnaast opgeven u dat de invoergegevens bevindt zich in de blob zogenaamd **adfgetstarted** en de map met de naam van **inputdata**
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService1",
                "typeProperties": {
                    "fileName": "input.log",
                    "folderPath": "adfgetstarted/inputdata",
                    "format": {
                        "type": "TextFormat",
                        "columnDelimiter": ","
                    }
                },
                "availability": {
                    "frequency": "Month",
                    "interval": 1
                },
                "external": true,
                "policy": {}
            }
        } 

    De volgende tabel vindt u beschrijvingen voor de JSON-eigenschappen in het fragment gebruikt:

  	| Eigenschap | Beschrijving |
  	| :------- | :---------- |
  	| type | De eigenschap type is ingesteld op AzureBlob omdat de gegevens zich bevinden in Azure-blobopslag. |  
  	| linkedServiceName | verwijst naar de AzureStorageLinkedService1 die u eerder hebt gemaakt. |
  	| fileName | Deze eigenschap is optioneel. Als u deze eigenschap weglaat, worden alle bestanden uit de mappad gekozen. In dit geval wordt alleen de input.log verwerkt. |
  	| type | De logboekbestanden zijn in tekstindeling, zodat we tekstopmaak gebruiken. | 
  	| columnDelimiter | kolommen in de logboekbestanden worden gescheiden door het kommateken () |
  	| frequentie/interval | de frequentie is ingesteld op maand en het interval is 1, wat betekent dat de invoer segmenten maandelijks beschikbaar zijn. | 
  	| externe | Deze eigenschap is ingesteld op waar als de invoergegevens niet wordt gegenereerd door de gegevens Factory-service. | 
      
    
3. Sla het bestand **InputDataset.json** . 

 
#### <a name="create-output-dataset"></a>Uitvoer gegevensset maken
Nu maken u de gegevensset uitvoer bevatten de uitvoergegevens die zijn opgeslagen in de Azure-blobopslag te. 

1. Klik in de **Oplossingverkenner**met de rechtermuisknop op **tabellen**, wijs **toevoegen**en klik op **Nieuw Item**. 
2. Selecteer **Azure Blob** in de lijst, wijzig de naam van het bestand in **OutputDataset.json**en klikt u op **toevoegen**. 
3. De **JSON** in de editor vervangen door het volgende: 

    In het fragment JSON maakt u een gegevensset genaamd **AzureBlobOutput**en de structuur van de gegevens die is gemaakt met de component-script op te geven. Daarnaast opgeven u dat de resultaten worden opgeslagen in de blob zogenaamd **adfgetstarted** en de map met de naam van **partitioneddata**. De sectie **beschikbaarheid** geeft aan dat de gegevensset uitvoer is geproduceerd op maandbasis.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adfgetstarted/partitioneddata",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Month",
              "interval": 1
            }
          }
        }

    Zie het **maken van de invoer gegevensset** gedeelte voor beschrijvingen van deze eigenschappen. U instelt niet de externe eigenschap op een gegevensset uitvoer terwijl de gegevensset is geproduceerd door de gegevens Factory-service.

4. Sla het bestand **OutputDataset.json** .


### <a name="create-pipeline"></a>Verkooppijplijn maken
In deze stap kunt u uw eerste verkooppijplijn maken met een activiteit **HDInsightHive** . De invoer segment maandelijks beschikbaar is (frequentie: maand, interval: 1), uitvoer segment maandelijks is geproduceerd en de eigenschap scheduler voor de activiteit ook is ingesteld op maandelijks. De instellingen voor de gegevensset uitvoer en de activiteit planner moeten overeenkomen met. Uitvoer gegevensset is momenteel wat stations het schema, zodat u kunt een gegevensset uitvoer maken moet, zelfs als de activiteit geen uitvoer levert. Als de activiteit niet wordt elke invoer duren, kunt u doorgaan met het maken van de invoer gegevensset. De eigenschappen die wordt gebruikt in de volgende JSON worden aan het einde van deze sectie beschreven.

1. Klik in de **Oplossingverkenner**met de rechtermuisknop op **pijpleidingen**, wijs **toevoegen**en klik op **Nieuw Item.** 
2. Selecteer **Component transformatie verkooppijplijn** in de lijst en klikt u op **toevoegen**. 
3. De **JSON** vervangen door het volgende fragment.

    > [AZURE.IMPORTANT] **storageaccountname** vervangen door de naam van uw account opslag.

        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "AzureStorageLinkedService1",
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

    In het fragment JSON maakt u een pijplijn die bestaat uit één activiteit waarin component voor proces gegevens op een cluster HDInsight wordt gebruikt.
    
    In het fragment JSON maakt u een pijplijn die bestaat uit één activiteit waarin component voor proces gegevens op een cluster HDInsight wordt gebruikt.
    
    De component-scriptbestand, **partitionweblogs.hql**, is opgeslagen in de opslag van Azure-account (opgegeven door de scriptLinkedService, **AzureStorageLinkedService1**genoemd), en in de map **script** in de container **adfgetstarted**.

    De sectie **definieert** wordt gebruikt om op te geven van de runtime-instellingen die worden doorgegeven aan het script component als component configuratiewaarden (bijvoorbeeld ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

    De eigenschappen van het **begin** en **einde** van de pijplijn Hiermee geeft u de actieve periode van de pijplijn.

    In de activiteit JSON, kunt u opgeven dat het script component wordt uitgevoerd op de opgegeven door de **linkedServiceName** – **HDInsightOnDemandLinkedService**berekeningscluster.

    > [AZURE.NOTE] Zie [anatomie van een pijplijn](data-factory-create-pipelines.md#anatomy-of-a-pipeline) voor meer informatie over de eigenschappen van JSON gebruikt in het voorbeeld. 
3. Sla het bestand **HiveActivity1.json** .

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a>Partitionweblogs.hql en input.log toevoegen als een afhankelijkheid 

1. Met de rechtermuisknop op **afhankelijkheden** in de **Oplossingverkenner** -venster, wijs **toevoegen**en klik op **Bestaande Item**.  
2. Navigeer naar de **C:\ADFGettingStarted** , selecteer **partitionweblogs.hql**, **input.log** -bestanden, en klik op **toevoegen**. U hebt deze twee bestanden als onderdeel van de vereisten van de [Zelfstudie overzicht](data-factory-build-your-first-pipeline.md)gemaakt.

Wanneer u de oplossing in de volgende stap publiceert, wordt de **partitionweblogs.hql** -bestand wordt geüpload naar de scriptmap in de **adfgetstarted** blob container.   

### <a name="publishdeploy-data-factory-entities"></a>Gegevens Factory entiteiten publiceren/implementeren

18. Met de rechtermuisknop op project in de Solution Explorer en klik op **publiceren**. 
19. Als u het dialoogvenster **aanmelden bij uw Microsoft-account** ziet, voert u uw referenties voor het account dat Azure abonnement heeft en klik op **aanmelden**.
20. Hier ziet u het volgende dialoogvenster:

    ![Het dialoogvenster publiceren](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. Ga als volgt te werk in de pagina factory configureren: 
    1. Selecteer de optie **Nieuwe gegevens Factory maken** .
    2. Voer een unieke **naam** voor de fabriek gegevens. Bijvoorbeeld: **FirstDataFactoryUsingVS09152016**. De naam moet uniek zijn.  
    
    
        > [AZURE.IMPORTANT] Als u het foutbericht **gegevens factory naam "FirstDataFactoryUsingVS" is niet beschikbaar wordt** wanneer u publiceert, moet u de naam (bijvoorbeeld yournameFirstDataFactoryUsingVS) wijzigen. Zie [Gegevens Factory - regels voor naamgeving van](data-factory-naming-rules.md) onderwerp voor naming regels voor Data Factory-onderdelen.
3. Selecteer het juiste abonnement voor het veld **abonnement** .
     
     
        > [AZURE.IMPORTANT] Als u een abonnement niet ziet, zorg ervoor dat u bent aangemeld met een account die is een beheerder of co-beheerders van het abonnement.  
        
    4. Selecteer de **resourcegroep** voor de fabriek gegevens moet worden gemaakt. 
    5. Het **gebied** voor de fabriek gegevens selecteren. 
    6. Klik op **volgende** om te schakelen naar de pagina **Publiceren Items** . (Druk op **TAB** om terug te verplaatsen uit het veld naam als de knop **volgende** is uitgeschakeld). 
23. De pagina **Publiceren Items** Zorg ervoor dat alle gegevens-bedrijven entiteiten zijn geselecteerd en klik op **volgende** om naar de pagina **Summary** te schakelen.     
24. Bekijk het overzicht en klik op **volgende** om de implementatie starten en de **Implementatie-Status**weergeven.
25. **Implementatiestatus** op de pagina ziet u de status van het implementatieproces. Klik op Voltooien wanneer de implementatie klaar is. 

 
Belangrijke punten met: 

- Als het foutbericht: '**dit abonnement niet is geregistreerd als u wilt gebruiken naamruimte Microsoft.DataFactory**', een van de volgende handelingen uit en probeer opnieuw te publiceren: 

    - Voer de volgende opdracht voor het registreren van de gegevens Factory-provider in Azure PowerShell. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        U kunt de volgende opdracht uit om te bevestigen dat de gegevens fabriek provider is geregistreerd uitvoeren. 
    
            Get-AzureRmResourceProvider
    - Meld u aan met het Azure abonnement in bij de [portal van Azure](https://portal.azure.com) en navigeer naar een gegevens Factory-blade (of) maken van een fabriek gegevens in de portal van Azure. Deze actie registreert automatisch de provider voor u.
-   De naam van de gegevens fabriek kan worden geregistreerd als een DNS-naam in de toekomst en dus worden openbaar zichtbaar.
-   Als u wilt gegevens Factory-exemplaren maken, moet u een beheerder of co-beheerders van de Azure-abonnement

 
## <a name="monitor-pipeline"></a>Monitor verkooppijplijn

### <a name="monitor-pipeline-using-diagram-view"></a>Monitor verkooppijplijn met de diagramweergave
6. Meld u aan bij de [portal van Azure](https://portal.azure.com/), als volgt te werk:
    1. Klik op **meer services** en klik op **gegevens factory's**.
        ![Gegevens factory's bladeren](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png) 
    2. Selecteer de naam van uw gegevens factory (bijvoorbeeld: **FirstDataFactoryUsingVS09152016**) in de lijst met gegevens factory's. 
        ![Selecteer uw gegevens factory](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
7. Klik in de startpagina van Lotus voor uw gegevens factory op **Diagram**.
  
    ![Diagram-tegel](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
7. In de diagramweergave te klikken ziet u een overzicht van de pijpleidingen en gegevenssets in deze zelfstudie gebruikt.
    
    ![Diagramweergave](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png) 
8. Bekijken van alle activiteiten in de pijplijn, met de rechtermuisknop op de verkooppijplijn in het diagram en klik op openen verkooppijplijn. 

    ![Menu openen verkooppijplijn](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
9. Bevestig dat u ziet dat de activiteit HDInsightHive in de pijplijn. 
  
    ![Open verkooppijplijn weergeven](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    Terug naar de vorige weergave wilt gaan, klikt u op **gegevens factory** in het menu breadcrumb aan de bovenkant. 
10. Dubbelklik in de **Diagramweergave te klikken**, op de gegevensset **AzureBlobInput**. Controleer of het segment in **Gereed** is. Het duurt enkele minuten voor het segment weergegeven op gereed. Als deze niet plaatsvindt nadat u zich ergens wachten, raadpleegt u als u de invoer-bestand (input.log hebt) in de juiste container (adfgetstarted) en een map (inputdata) geplaatst.

    ![Invoer segment in gereed](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
11. Klik op de **X** om te **AzureBlobInput** blade sluiten. 
12. Dubbelklik in de **Diagramweergave te klikken**, op de gegevensset **AzureBlobOutput**. U ziet dat het segment die momenteel wordt verwerkt.

    ![Gegevensset](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. Wanneer verwerking klaar is, ziet u het segment in **Gereed** .

    > [AZURE.IMPORTANT] Maken van een op aanvraag HDInsight cluster duurt meestal ergens (ongeveer 20 minuten). Daarom verwachten de pijplijn te nemen **ongeveer 30 minuten** verwerkingstijd van het segment.  

    ![Gegevensset](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png) 
    
10. Wanneer het segment in **Gereed** is, controleert u of de map **partitioneddata** in de container **adfgetstarted** in uw blobopslag voor de uitvoergegevens.  
 
    ![uitvoergegevens](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. Klik op het segment voor meer informatie over deze in een blade **segment** .

    ![Detailgegevens segment](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. Klik op een activiteit worden uitgevoerd in de **lijst van activiteit wordt uitgevoerd** voor meer informatie over een activiteit (Component activiteit in ons scenario) worden uitgevoerd in een activiteitvenster voor het **uitvoeren van details** .   
    ![Activiteit details uitvoeren](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)  
    
    Vanuit de logboekbestanden, kunt u de component query die is uitgevoerd en de statusinformatie bekijken. Deze logboeken zijn handig voor het oplossen van problemen.  
 

Zie [Monitor gegevenssets en pijplijn](data-factory-monitor-manage-pipelines.md) voor instructies over het gebruik van de Azure-portal om de verkooppijplijn en gegevenssets te bewaken die u in deze zelfstudie hebt gemaakt.

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Monitor verkooppijplijn met Monitor & App beheren
U kunt ook beeldscherm gebruiken en beheren van toepassing op uw pijpleidingen controleren. Zie voor gedetailleerde informatie over het gebruik van deze toepassing [beeldscherm en beheren van Azure gegevens Factory pijpleidingen controle en beheer App](data-factory-monitor-manage-app.md).

1. Klik op controleren en tegel beheren.

    ![Tegel voor monitor & beheren](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png) 
2. U moet Zie Monitor en beheren van toepassing. Wijzig de **Begintijd** en **eindtijd** wilt aanpassen aan de slag (04-01-2016 12:00 AM)- en eindtijden (04-02-2016 12:00 AM) van de pijplijn, en klik op **toepassen**.

    ![Controleren en App beheren](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png) 
3. Selecteer een venster van de activiteit in de lijst activiteit Windows informatie over deze wilt weergeven. 
    ![Details van de activiteit-venster](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)


> [AZURE.IMPORTANT] De invoer bestand wordt verwijderd wanneer het segment is verwerkt. Daarom desgewenst kunt u het segment opnieuw uit te voeren of de zelfstudie opnieuw doen het invoer bestand uploaden (input.log) naar de map inputdata van de container adfgetstarted.
 

## <a name="use-server-explorer-to-view-data-factories"></a>Server Explorer gebruiken om te bekijken van gegevens factory 's

1. Klik op **weergave** in het menu in **Visual Studio**, en klik op de **Server Explorer**.
2. Klik in het venster Server Explorer **Azure** uitvouwen en **Gegevens Factory**uitvouwen. Als u ziet, **Meld u aan bij Visual Studio**, voer wordt het **account** dat is gekoppeld aan uw Azure-abonnement en klik op **Doorgaan**. **Wachtwoord**, en klik op **aanmelden**. Visual Studio wordt geprobeerd voor informatie over alle gegevens van Azure-factory's in uw abonnement. U ziet dat de status van deze bewerking in het venster **Gegevens Factory-takenlijst** .

    ![Server Explorer](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. U kunt met de rechtermuisknop op een factory gegevens en selecteer **Factory van gegevens exporteren naar een nieuw Project** om een Visual Studio-project op basis van een bestaande gegevens factory te maken.

    ![Gegevens factory exporteren](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a>Hulpmiddelen voor gegevens Factory bijwerken voor Visual Studio

Ga als volgt te werk als u wilt bijwerken Azure gegevens Factory-hulpprogramma's voor Visual Studio:

1. Klik in het menu op **Extra** en selecteer **Extensions en Updates**.
2. Selecteer de **Updates die** in het linkerdeelvenster en selecteer vervolgens **Visual Studio-galerie**.
3. Selecteer **Azure gegevens Factory-hulpprogramma's voor Visual Studio** en klik op **bijwerken**. Als u dit item niet ziet, hebt u al de meest recente versie van de hulpmiddelen voor. 

## <a name="use-configuration-files"></a>Configuratiebestanden gebruiken
Configuratiebestanden in Visual Studio kunt u eigenschappen voor services/tabellen/pijpleidingen van de gekoppelde anders voor elke omgeving configureren. 

Houd rekening met de volgende JSON-definitie voor een service van Azure-opslag die zijn gekoppeld. Om op te geven **connectionString** met verschillende waarden voor accountnaam en accountkey op basis van de omgeving (ontwikkelaar/Test/productie) waarop u gegevens Factory entiteiten implementeert. U kunt dit gedrag bereiken met behulp van afzonderlijke configuratiebestand voor elke omgeving. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "description": "",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    } 

### <a name="add-a-configuration-file"></a>Een configuratiebestand toevoegen
Een configuratiebestand voor elke omgeving toevoegen door de volgende stappen uit te voeren:   

1. Met de rechtermuisknop op het Data Factory-project in uw Visual Studio-oplossing, wijs **toevoegen**en klik op **Nieuw item**.
2. Selecteer **Config** in de lijst met geïnstalleerde sjablonen aan de linkerkant, **Configuratiebestand**selecteren, voert u een **naam** voor het configuratiebestand en klikt u op **toevoegen**.

    ![Configuratiebestand toevoegen](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. Parameters voor de configuratie en de bijbehorende waarden in de volgende indeling toevoegen.

        {
            "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
            "AzureStorageLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            ],
            "AzureSqlLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                }
            ]
        }

    In dit voorbeeld configureert connectionString eigenschap van een Azure gekoppeld opslagservice en een gekoppelde Azure SQL-service. Zoals u ziet de syntaxis van de om aan te geven naam [JsonPath](http://goessner.net/articles/JsonPath/).   

    Als JSON een eigenschap heeft heeft die een matrix met waarden zoals weergegeven in de volgende code:  

        "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
        ],
    
    Eigenschappen configureren, zoals wordt weergegeven in het volgende configuratiebestand (gebruik nul indexeren): 
        
        {
            "name": "$.properties.structure[0].name",
            "value": "FirstName"
        }
        {
            "name": "$.properties.structure[0].type",
            "value": "String"
        }
        {
            "name": "$.properties.structure[1].name",
            "value": "LastName"
        }
        {
            "name": "$.properties.structure[1].type",
            "value": "String"
        }

### <a name="property-names-with-spaces"></a>Eigenschapnamen met spaties
Als de naam van een eigenschap spaties bevat, gebruikt u vierkante haken bevinden, zoals in het volgende voorbeeld (databaseservernaam): 

     {
         "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
         "value": "MyAsqlServer.database.windows.net"
     }


### <a name="deploy-solution-using-a-configuration"></a>Een configuratie-oplossing implementeert
Wanneer u Azure gegevens Factory entiteiten in de VS publiceert, kunt u de configuratie die u wilt gebruiken voor de publicatie bewerking opgeven. 

Entiteiten in een configuratiebestand met Azure gegevens Factory-project publiceren:   

1. Met de rechtermuisknop op Data Factory-project en klik op **publiceren** als u wilt zien van het dialoogvenster **Items publiceren** . 
2. Selecteer een bestaande gegevens factory of geef de waarden voor het maken van een fabriek gegevens op de pagina **configureren gegevens factory** en klik op **volgende**.   
3. Klik op de pagina **Publiceren Items** : u ziet een vervolgkeuzelijst met beschikbare configuraties voor het veld **Selecteer implementatie Config** .

    ![Selecteer configuratiebestand](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)

4. Selecteer het **bestand** dat u wilt gebruiken en klik op **volgende**. 
5. Controleer of u de naam van JSON-bestand in de pagina **Summary ziet** en klik op **volgende**. 
6. Nadat de implementatie-bewerking is voltooid, klikt u op **Voltooien** . 

Wanneer u implementeert, worden de waarden uit het configuratiebestand worden gebruikt voor waarden voor eigenschappen instellen in de JSON-bestanden voor Data Factory entiteiten voordat de entiteiten zijn geïmplementeerd in Azure gegevens Factory-service.   

## <a name="summary"></a>Overzicht 
In deze zelfstudie kunt u een factory Azure gegevens om gegevens te verwerken met een component script op een HDInsight hadoop cluster gemaakt. U gebruikt de gegevens Factory-Editor in de portal van Azure moet de volgende stappen uit:  

1.  Een Azure **gegevens factory**gemaakt.
2.  Twee **gekoppelde services**gemaakt:
    1.  **Azure Storage** gekoppeld service uw Azure-blobopslag die met de bestanden van de invoer/uitvoer fabriek gegevens koppelen.
    2.  **Azure HDInsight** op aanvraag gekoppelde service een op aanvraag HDInsight Hadoop cluster koppelen aan de fabriek gegevens. Azure gegevens Factory Hiermee maakt u een HDInsight Hadoop cluster just-in-time verwerken van invoergegevens en het verkrijgen van uitvoergegevens. 
3.  Gemaakt twee **gegevenssets**, welke invoer- en uitvoerbereik gegevens voor HDInsight component activiteit in de pijplijn beschrijven. 
4.  Een **verkooppijplijn** gemaakt met de activiteit in een **HDInsight-component** .  


## <a name="next-steps"></a>Volgende stappen
In dit artikel, kunt u een pijplijn hebt gemaakt met een transformatie activiteit (HDInsight) die een script component op een op aanvraag HDInsight cluster wordt uitgevoerd. Zie informatie over het gebruik van de activiteit in een kopie gegevens kopiëren van een Azure Blob naar SQL Azure, [Zelfstudie: gegevens kopiëren van een Azure blob naar SQL Azure](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
  
## <a name="see-also"></a>Zie ook
| Onderwerp | Beschrijving |
| :---- | :---- |
| [Gegevens transformatie activiteiten](data-factory-data-transformation-activities.md) | In dit artikel vindt u een lijst met gegevens transformatie activiteiten (zoals HDInsight component transformatie u in deze zelfstudie gebruikt) worden ondersteund door Factory van Azure-gegevens. | 
| [Plannings- en kan worden uitgevoerd](data-factory-scheduling-and-execution.md) | In dit artikel wordt uitgelegd dat de plannings- en execution aspecten van Azure gegevens Factory-toepassingsmodel. |
| [Pijpleidingen](data-factory-create-pipelines.md) | Dit artikel vindt u meer informatie over pijpleidingen en activiteiten in Factory van Azure-gegevens en hoe ze worden gebruikt om samen te stellen end-to-end gegevensgestuurde werkstromen voor uw scenario of bedrijf. |
| [Gegevenssets](data-factory-create-datasets.md) | Dit artikel vindt u meer informatie over gegevenssets in fabriek van Azure-gegevens.
| [Bewaken en pijpleidingen met Monitoring App beheren](data-factory-monitor-manage-app.md) | In dit artikel wordt beschreven hoe bewaken, beheren en fouten opsporen in pijpleidingen met behulp van de controle Management-App. 
