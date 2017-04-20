<properties
    pageTitle="Uw eerste gegevens factory (Azure portal) bouwen | Microsoft Azure"
    description="In deze zelfstudie maakt u een voorbeeld Azure gegevens Factory pijplijn met gegevens Factory-Editor in de portal van Azure."
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
    ms.date="09/14/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a>Zelfstudie: Uw eerste Azure gegevens factory met behulp van Azure portal maken
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-build-your-first-pipeline.md)
- [Azure-portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resourcemanager-sjabloon](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

In dit artikel leert u hoe de [Azure-portal](https://portal.azure.com/) gebruiken om te maken van uw eerste Azure gegevens factory. 

## <a name="prerequisites"></a>Vereisten voor        
1. [Zelfstudie overzicht](data-factory-build-your-first-pipeline.md) artikel en voer de **vereiste** stappen uit.
2. In dit artikel biedt een overzicht van de Azure gegevens Factory-service geen. Het is raadzaam dat u leest u over [Inleiding tot Azure gegevens Factory](data-factory-introduction.md) -artikel voor een gedetailleerd overzicht van de service.  

## <a name="create-data-factory"></a>Gegevens factory maken
Een factory gegevens kan een of meer pijpleidingen bevatten. Een pijplijn kan een of meer activiteiten in deze hebben. Een kopie naar gegevens uit een bron voor een gegevensopslag bestemming kopiëren en een activiteit HDInsight component naar component script uitvoeren om transformeren invoergegevens product bijvoorbeeld uitvoeren gegevens. Laten we beginnen met het maken van de fabriek gegevens in deze stap. 

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2.  Klik op **Nieuw** in het linkermenu op **gegevens + Analytics**en klik op **Gegevens fabriek**.
        
    ![Blade maken](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)

2.  Voer in het blad **nieuwe gegevens factory** **GetStartedDF** voor de naam.

    ![Nieuwe gegevens factory blade](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

    > [AZURE.IMPORTANT] 
    > De naam van de fabriek Azure gegevens moet **uniek**zijn. Als het foutbericht: **gegevens factory naam "GetStartedDF" is niet beschikbaar**. Wijzig de naam van de gegevens fabriek (bijvoorbeeld yournameGetStartedDF) en probeert opnieuw te maken. Zie [Gegevens Factory - regels voor naamgeving van](data-factory-naming-rules.md) onderwerp voor naming regels voor gegevens Factory onderdelen.
    > 
    > De naam van de gegevens fabriek kan worden geregistreerd als een **DNS-** naam in de toekomst en dus worden openbaar zichtbaar.

3.  Selecteer het **Azure abonnement** waar u de gegevens fabriek moet worden gemaakt. 
4.  Selecteer bestaande **resourcegroep** of een resourcegroep maken. Maak een resourcegroep met de naam voor de zelfstudie: **ADFGetStartedRG**. 
5.  Klik op **maken** op het **nieuwe gegevens factory** -blad.

    > [AZURE.IMPORTANT] Als u wilt maken Data Factory-exemplaren, moet u lid zijn van de rol [Inzender Factory](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) op het niveau van de groep abonnement/resource. 
6.  Ziet u de gegevens fabriek worden gemaakt in de **Startboard** van de Azure-portal als volgt:   

    ![Factory-status van gegevens maken](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
7. Gefeliciteerd! U hebt uw eerste gegevens factory gemaakt. Nadat de gegevens fabriek is gemaakt, kunt u de gegevens factory-pagina, waarin u de inhoud van de fabriek gegevens zien.   

    ![Gegevens Factory blade](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

Voordat u een pijplijn maakt in de fabriek gegevens, moet u eerst een paar gegevens Factory entiteiten maken. U maakt eerst gekoppelde services om gegevens winkels/berekent de koppeling naar uw gegevensopslag, invoer definiëren en gegevenssets om gegevens van de invoer/uitvoer in gekoppelde gegevens winkels voor te stellen en vervolgens de pijplijn te maken met een activiteit waarin deze gegevenssets uitvoer. 

## <a name="create-linked-services"></a>Gekoppelde services maken
In deze stap geeft u een uw opslagruimte van Azure-account en een op aanvraag Azure HDInsight cluster koppeling naar uw gegevens factory. De opslag van Azure-account bevat de invoer- en uitvoerbereik gegevens voor de pijplijn in dit voorbeeld. De HDInsight gekoppeld-service wordt gebruikt om uit te voeren component script die zijn opgegeven in de activiteit van de pijplijn in dit voorbeeld. Bepalen welke [gegevens opslaan](data-factory-data-movement-activities.md)/[berekenen services](data-factory-compute-linked-services.md) worden gebruikt in uw scenario en deze services koppelen aan de fabriek gegevens door te maken van gekoppelde services.  

### <a name="create-azure-storage-linked-service"></a>Azure gekoppeld opslagservice maken
In deze stap geeft koppelen u uw opslagruimte van Azure-account aan uw factory gegevens. In deze zelfstudie gebruikt u hetzelfde account Azure Storage invoer/uitvoergegevens en de HQL script-bestand wilt opslaan. 

1.  Klik op **auteur en implementeren** op het blad **Gegevens FACTORY** voor **GetStartedDF**. Hier ziet u de gegevens Factory-Editor. 
     
    ![Ontwerpen en implementeren van de tegel](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2.  Klik op **nieuwe gegevens opslaan** en kies **Azure opslag**.

    ![Nieuwe gegevens store - opslag Azure - menu](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)

3.  Hier ziet u de JSON-script voor het maken van een Azure gekoppeld opslagservice in de editor. 
    
    ![Azure gekoppeld opslagservice](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
     
4. **Accountnaam** vervangen door de naam van uw Azure opslag-account en **accountsleutel** met de access-toets van de Azure opslag-account. Als u wilt leren hoe u uw opslagruimte toegangstoets, raadpleegt u [weergave, kopiëren en opnieuw genereren opslag-toegangstoetsen](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
5. Klik op **Deploy** op de opdrachtbalk om te implementeren van de gekoppelde service.

    ![Knop implementeren](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   Nadat de gekoppelde service wordt geïmplementeerd, het **concept-1** -venster moet verdwijnen en u **AzureStorageLinkedService** ziet in de structuurweergave aan de linkerkant. 
    ![Gekoppelde opslagservice in menu](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)   

 
### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight gekoppeld service maken
In deze stap kunt u een op aanvraag HDInsight cluster koppeling naar uw gegevens factory. Het cluster HDInsight wordt automatisch gemaakt tijdens runtime en is verwijderd nadat deze verwerking en niet-actieve voor de opgegeven tijdsduur is voltooid. 

1. Klik in de **Gegevens Factory-Editor**op **... Meer**en klik op **Nieuw berekenen** **op aanvraag HDInsight cluster**hebt geselecteerd.

    ![Nieuwe berekenen](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Kopieer en plak de volgende fragment naar het **concept-1** -venster. Het codefragment van de JSON worden de eigenschappen die worden gebruikt voor het maken van de HDInsight cluster op aanvraag. 

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService"
            }
          }
        }
    
    De volgende tabel vindt u beschrijvingen voor de JSON-eigenschappen in het fragment gebruikt:
    
  	| Eigenschap | Beschrijving |
  	| :------- | :---------- |
  	| Versie | Geeft aan dat de versie van de HDInsight 3,2 worden gemaakt. | 
  	| ClusterSize | Hiermee geeft u de grootte van het cluster HDInsight. | 
  	| TimeToLive | Geeft aan dat het niet-actieve tijd voor het cluster HDInsight voordat deze wordt verwijderd. |
  	| linkedServiceName | Hiermee geeft u de opslag-account dat wordt gebruikt voor de opslag van de logboekbestanden die zijn gegenereerd met HDInsight. |

    Houd rekening met de volgende punten: 
    
    - De gegevens fabriek wordt een HDInsight **op basis van Windows** cluster voor u gemaakt met de JSON. U kunt ook het maken van een HDInsight **Linux gebaseerde** cluster hebben. Zie [Op aanvraag HDInsight gekoppelde Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie. 
    - U kunt **uw eigen HDInsight cluster** in plaats van een cluster op aanvraag-HDInsight. Zie [Gekoppelde HDInsight-Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) voor meer informatie.
    - Het cluster HDInsight Hiermee maakt u een **standaard-container** in de blobopslag die u hebt opgegeven in de JSON (**linkedServiceName**). HDInsight worden deze container niet verwijderd wanneer het cluster wordt verwijderd. Dit probleem is inherent aan het ontwerp. Met op aanvraag HDInsight gekoppeld-service, een HDInsight cluster gemaakt telkens wanneer een segment is verwerkt, tenzij er een bestaand live cluster (**timeToLive is**). Het cluster wordt automatisch verwijderd wanneer de verwerking klaar is.
    
        Als u meer segmenten worden verwerkt, ziet u veel containers in uw Azure-blobopslag. Als u niet nodig ze hebt voor het oplossen van problemen met de taken, wilt u mogelijk verwijdert u deze als u wilt doen om de opslag te beperken. De namen van deze containers een patroon volgen: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Gebruik hulpprogramma's zoals [Microsoft opslag Explorer](http://storageexplorer.com/) containers in uw Azure-blobopslag verwijderen.

    Zie [Op aanvraag HDInsight gekoppelde Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie.
3. Klik op **Deploy** op de opdrachtbalk om te implementeren van de gekoppelde service. 

    ![Op aanvraag HDInsight gekoppeld service implementeren](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)

4. Bevestig dat u zowel **AzureStorageLinkedService** als **HDInsightOnDemandLinkedService** in de structuur weergeven aan de linkerkant ziet.

    ![Structuurweergave met gekoppelde services](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a>Gegevenssets maken
In deze stap maakt u gegevenssets om te staan voor de invoer en uitvoer van gegevens voor de verwerking van de component. Deze gegevenssets raadpleegt u de **AzureStorageLinkedService** die u eerder in deze zelfstudie hebt gemaakt. De gekoppelde service wordt verwezen in een opslag van Azure-account en gegevenssets container, map, bestandsnaam opgeven in de opslagruimte waarin invoer en gegevens.   

### <a name="create-input-dataset"></a>Invoer gegevensset maken

1. Klik in de **Gegevens Factory-Editor**op **... Meer** op de balk met opdrachten, klikt u op **nieuwe gegevensset**en selecteer **Azure-blobopslag**.

    ![Nieuwe gegevensset](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Kopieer en plak de volgende fragment naar het concept-1-venster. In het fragment JSON maakt u een gegevensset **AzureBlobInput** met invoergegevens voor een activiteit in de pijplijn genoemd. Daarnaast opgeven u dat de invoergegevens bevindt zich in de blob zogenaamd **adfgetstarted** en de map met de naam van **inputdata**.
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
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
  	| linkedServiceName | verwijst naar de AzureStorageLinkedService die u eerder hebt gemaakt. |
  	| fileName | Deze eigenschap is optioneel. Als u deze eigenschap weglaat, worden alle bestanden uit de mappad gekozen. In dit geval wordt alleen de input.log verwerkt. |
  	| type | De logboekbestanden zijn in tekstindeling, zodat we tekstopmaak gebruiken. | 
  	| columnDelimiter | kolommen in de logboekbestanden worden gescheiden door kommateken () |
  	| frequentie/interval | de frequentie is ingesteld op maand en het interval is 1, wat betekent dat de invoer segmenten maandelijks beschikbaar zijn. | 
  	| externe | Deze eigenschap is ingesteld op waar als de invoergegevens niet wordt gegenereerd door de gegevens Factory-service. | 
        
3. Klik op **Deploy** op de opdrachtbalk naar de zojuist gemaakte gegevensset implementeren. Hier ziet u de gegevensset in de structuurweergave aan de linkerkant. 


### <a name="create-output-dataset"></a>Uitvoer gegevensset maken
Nu maken u de gegevensset uitvoer bevatten de uitvoergegevens die zijn opgeslagen in de Azure-blobopslag te. 

1. Klik in de **Gegevens Factory-Editor**op **... Meer** op de balk met opdrachten, klikt u op **nieuwe gegevensset**en selecteer **Azure-blobopslag**.  
2. Kopieer en plak de volgende fragment naar het concept-1-venster. In het fragment JSON maakt u een gegevensset genaamd **AzureBlobOutput**en de structuur van de gegevens die is gemaakt met de component-script op te geven. Daarnaast opgeven u dat de resultaten worden opgeslagen in de blob zogenaamd **adfgetstarted** en de map met de naam van **partitioneddata**. De sectie **beschikbaarheid** geeft aan dat de gegevensset uitvoer is geproduceerd op maandbasis.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
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
3. Klik op **Deploy** op de opdrachtbalk naar de zojuist gemaakte gegevensset implementeren.
4. Controleer of dat de gegevensset is gemaakt.

    ![Structuurweergave met gekoppelde services](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a>Verkooppijplijn maken
In deze stap kunt u uw eerste verkooppijplijn maken met een activiteit **HDInsightHive** . Invoer segment maandelijks beschikbaar is (frequentie: maand, interval: 1), uitvoer segment maandelijks is geproduceerd en de eigenschap scheduler voor de activiteit ook is ingesteld op maandelijks. De instellingen voor de gegevensset uitvoer en de activiteit planner moeten overeenkomen met. Uitvoer gegevensset is momenteel wat stations het schema, zodat u kunt een gegevensset uitvoer maken moet, zelfs als de activiteit geen uitvoer levert. Als de activiteit niet wordt elke invoer duren, kunt u doorgaan met het maken van de invoer gegevensset. De eigenschappen die wordt gebruikt in de volgende JSON worden aan het einde van deze sectie beschreven. 

1. Klik in de **Gegevens Factory-Editor**op **weglatingsteken (...) Meer opdrachten** en klik vervolgens op **nieuwe verkooppijplijn**.
    
    ![de knop Nieuw verkooppijplijn](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Kopieer en plak de volgende fragment naar het concept-1-venster.

    > [AZURE.IMPORTANT] **Storageaccountname** vervangen door de naam van uw account opslag in de JSON.
        
        {
            "name": "MyFirstPipeline",
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
 
    In het fragment JSON maakt u een pijplijn die bestaat uit één activiteit waarin component voor proces gegevens op een cluster HDInsight wordt gebruikt.
    
    De component-scriptbestand, **partitionweblogs.hql**, is opgeslagen in de opslag van Azure-account (opgegeven door de scriptLinkedService, **AzureStorageLinkedService**genoemd), en in de map **script** in de container **adfgetstarted**.

    De sectie **definieert** wordt gebruikt om op te geven van de runtime-instellingen die worden doorgegeven aan het script component als component configuratiewaarden (bijvoorbeeld ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

    De eigenschappen van het **begin** en **einde** van de pijplijn Hiermee geeft u de actieve periode van de pijplijn.

    In de activiteit JSON, kunt u opgeven dat het script component wordt uitgevoerd op de opgegeven door de **linkedServiceName** – **HDInsightOnDemandLinkedService**berekeningscluster.

    > [AZURE.NOTE] Zie [anatomie van een pijplijn](data-factory-create-pipelines.md#anatomy-of-a-pipeline) voor meer informatie over de eigenschappen van JSON gebruikt in het voorbeeld. 

3. Controleer het volgende: 
    1. Er bestaat een **Input.log** bestand in de map **inputdata** van de container **adfgetstarted** in de Azure-blobopslag
    2. Er bestaat een **partitionweblogs.hql** bestand in de map **script** van de container **adfgetstarted** in de Azure-blobopslag. Voltooid de vereiste stappen in de [Zelfstudie overzicht](data-factory-build-your-first-pipeline.md) als u deze bestanden niet ziet. 
    3. Bevestig dat u **storageaccountname** door de naam van uw account opslag in de pijplijn JSON vervangen. 
2. Klik op **Deploy** op de opdrachtbalk om te implementeren van de pijplijn. Aangezien de ** **begin** - en** eindtijden worden ingesteld in het verleden en **isPaused** is ingesteld op ONWAAR, wordt de pijplijn (activiteit in de pijplijn) uitgevoerd direct nadat u implementeren. 
4. Bevestig dat u ziet dat de pijplijn in de boomstructuur.

    ![Structuurweergave met verkooppijplijn](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
5. Gefeliciteerd, u uw eerste verkooppijplijn hebt gemaakt.

## <a name="monitor-pipeline"></a>Monitor verkooppijplijn

### <a name="monitor-pipeline-using-diagram-view"></a>Monitor verkooppijplijn met de diagramweergave

6. Klik op **X** om te gegevens Factory Editor bladen sluiten en terug naar het blad gegevens Factory en op **Diagram**.
  
    ![Diagram-tegel](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
7. In de diagramweergave te klikken ziet u een overzicht van de pijpleidingen en gegevenssets in deze zelfstudie gebruikt.
    
    ![Diagramweergave](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png) 
8. Bekijken van alle activiteiten in de pijplijn, met de rechtermuisknop op de verkooppijplijn in het diagram en klik op openen verkooppijplijn. 

    ![Menu openen verkooppijplijn](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
9. Bevestig dat u ziet dat de activiteit HDInsightHive in de pijplijn. 
  
    ![Open verkooppijplijn weergeven](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    Terug naar de vorige weergave wilt gaan, klikt u op **gegevens factory** in het menu breadcrumb aan de bovenkant. 
10. Dubbelklik in de **Diagramweergave te klikken**, op de gegevensset **AzureBlobInput**. Controleer of het segment in **Gereed** is. Het duurt enkele minuten voor het segment weergegeven op gereed. Als deze niet plaatsvindt nadat u zich ergens wachten, raadpleegt u als u de invoer-bestand (input.log hebt) in de juiste container (adfgetstarted) en een map (inputdata) geplaatst.

    ![Invoer segment in gereed](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
11. Klik op de **X** om te **AzureBlobInput** blade sluiten. 
12. Dubbelklik in de **Diagramweergave te klikken**, op de gegevensset **AzureBlobOutput**. U ziet dat het segment die momenteel wordt verwerkt.

    ![Gegevensset](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
9. Wanneer verwerking klaar is, ziet u het segment in **Gereed** .
    
>[AZURE.IMPORTANT] Maken van een op aanvraag HDInsight cluster duurt meestal ergens (ongeveer 20 minuten). Daarom verwachten de pijplijn te nemen **ongeveer 30 minuten** verwerkingstijd van het segment.    

    ![Dataset](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png) 
    
10. Wanneer het segment in **Gereed** is, controleert u of de map **partitioneddata** in de container **adfgetstarted** in uw blobopslag voor de uitvoergegevens.  
 
    ![uitvoergegevens](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
11. Klik op het segment voor meer informatie over deze in een blade **segment** .

    ![Detailgegevens segment](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
12. Klik op een activiteit worden uitgevoerd in de **lijst van activiteit wordt uitgevoerd** voor meer informatie over een activiteit (Component activiteit in ons scenario) worden uitgevoerd in een activiteitvenster voor het **uitvoeren van details** .   

    ![Activiteit details uitvoeren](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)  
    
    Vanuit de logboekbestanden, kunt u de component query die is uitgevoerd en de statusinformatie bekijken. Deze logboeken zijn handig voor het oplossen van problemen.
Zie [beeldscherm en beheren van pijpleidingen met Azure portal bladen](data-factory-monitor-manage-pipelines.md) artikel voor meer informatie. 

> [AZURE.IMPORTANT] De invoer bestand wordt verwijderd wanneer het segment is verwerkt. Daarom desgewenst kunt u het segment opnieuw uit te voeren of de zelfstudie opnieuw doen het invoer bestand uploaden (input.log) naar de map inputdata van de container adfgetstarted.

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Monitor verkooppijplijn met Monitor & App beheren
U kunt ook beeldscherm gebruiken en beheren van toepassing op uw pijpleidingen controleren. Zie voor gedetailleerde informatie over het gebruik van deze toepassing [beeldscherm en beheren van Azure gegevens Factory pijpleidingen controle en beheer App](data-factory-monitor-manage-app.md).

1. Klik op **controleren en beheren** tegel op de startpagina voor uw gegevens factory.

    ![Tegel voor monitor & beheren](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png) 
2. Hier ziet u **Monitor & beheren-toepassing**. Wijzig de **Begintijd** en **eindtijd** wilt aanpassen aan de slag (04-01-2016 12:00 AM)- en eindtijden (04-02-2016 12:00 AM) van de pijplijn, en klik op **toepassen**.

    ![Controleren en App beheren](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png) 
3. Selecteer een venster van de activiteit in de lijst **Activiteit Windows** informatie over deze wilt weergeven. 
    ![Details van de activiteit-venster](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)


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

  

