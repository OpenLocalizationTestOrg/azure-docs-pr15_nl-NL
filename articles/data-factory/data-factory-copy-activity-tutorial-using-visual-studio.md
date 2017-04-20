<properties 
    pageTitle="Zelfstudie: Een pijplijn maken met kopie activiteit gebruik van Visual Studio | Microsoft Azure" 
    description="In deze zelfstudie maakt u een verkooppijplijn Factory van Azure-gegevens met de activiteit in een kopie met behulp van Visual Studio." 
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
    ms.date="10/17/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Zelfstudie: Een pijplijn maken met kopie activiteit gebruik van Visual Studio
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md)
- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure resourcemanager-sjabloon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Deze zelfstudie ziet u hoe u maken en controleren van een Azure gegevens factory gebruik van de Visual Studio. De pijplijn in de fabriek gegevens gebruikt de activiteit in een kopie gegevens van Azure-blobopslag kopiëren met Azure SQL-Database.

Hier volgen de stappen die u als onderdeel van deze zelfstudie uitvoeren:

1. Maken van twee gekoppelde services: **AzureStorageLinkedService1** en **AzureSqlinkedService1**. 

    De AzureStorageLinkedService1 koppelingen een Azure opslag en AzureSqlLinkedService1 koppelingen een Azure SQL-database naar de gegevens fabriek: **ADFTutorialDataFactoryVS**. De ingevoerde gegevens voor de pijplijn bevindt zich in een container blob in de Azure-blobopslag en uitvoergegevens is opgeslagen in een tabel in de SQL Azure-database. Daarom toevoegen u deze twee gegevensopslag als gekoppelde services fabriek gegevens.
2. Twee gegevenssets maken: **InputDataset** en **OutputDataset**, die de gegevens van de invoer/uitvoer die is opgeslagen in de gegevens voor te stellen. 

    Voor de InputDataset geeft u de container blob met een blob met de brongegevens. Voor de OutputDataset geeft u de SQL-tabel waarin de uitvoergegevens. U ook opgeven andere eigenschappen zoals structuur, beschikbaarheid en beleid.
3. Maak een pijplijn **ADFTutorialPipeline** in de ADFTutorialDataFactoryVS met de naam. 

    De pijplijn heeft een **Kopie activiteit** kopieën input gegevens uit de Azure BLOB aan de uitvoer Azure SQL-tabel. De activiteit kopie kunt u de verplaatsing van gegevens uitvoeren in fabriek van Azure-gegevens. De activiteit is mogelijk gemaakt door een globaal beschikbare service die gegevens tussen verschillende opgeslagen in de gegevens op een veilige, betrouwbare en scalable manier kunt kopiëren. Zie [Gegevens verkeer activiteiten](data-factory-data-movement-activities.md) artikel voor meer informatie over de activiteit kopiëren. 
4. Maak een gegevens factory met de naam **VSTutorialFactory**. De gegevens fabriek en alle gegevens Factory-entiteiten (gekoppelde services, tabellen en de pijplijn) implementeren.    

## <a name="prerequisites"></a>Vereisten voor

1. [Zelfstudie overzicht](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) artikel en voer de **vereiste** stappen uit. 
2. U moet een **beheerder van het abonnement dat Azure** kunnen gegevens Factory entiteiten publiceren naar Factory van Azure-gegevens.  
3. U moet de volgende manieren te werk op uw computer is geïnstalleerd: 
    - Visual Studio 2013 of Visual Studio-2015
    - Download Azure SDK voor Visual Studio 2013 of Visual Studio-2015. Navigeer naar de [Pagina voor het downloaden van Azure](https://azure.microsoft.com/downloads/) en op **tegenover 2013** of **tegenover 2015** in de sectie **.NET** .
    - Download de meest recente Azure gegevens Factory-invoegtoepassing voor Visual Studio: [tegenover 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) of [tegenover 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). U kunt de invoegtoepassing ook bijwerken door de volgende stappen uit te voeren: klik op **Extra**in het menu -> **Extensions en Updates** -> **Online** -> **Visual Studio-galerie** -> **Microsoft Azure gegevens Factory Tools voor Visual Studio** -> **bijwerken**.

## <a name="create-visual-studio-project"></a>Visual Studio-project maken 
1. Start **Visual Studio-2013**. Klik op **bestand**, wijs **Nieuw**aan en klik op **Project**. Hier ziet u het dialoogvenster **Nieuw Project** .  
2. Klik in het dialoogvenster **Nieuw Project** selecteert u de sjabloon **DataFactory** en op **Lege gegevens Factory-Project**. Als u de sjabloon DataFactory niet ziet, sluit Visual Studio, Azure SDK voor Visual Studio 2013 installeren, en open Visual Studio.  

    ![Dialoogvenster Nieuw project](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)

3. Voer een **naam** voor het project, **locatie**en een naam voor de **oplossing**en klik op **OK**.

    ![Oplossing Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png) 

## <a name="create-linked-services"></a>Gekoppelde services maken
Gekoppelde services opgeslagen gegevens koppelen of services naar een factory Azure gegevens berekenen. Zie [ondersteunde gegevens worden opgeslagen](data-factory-data-movement-activities.md##supported-data-stores-and-formats) voor alle bronnen en sinks worden ondersteund door de activiteit kopiëren. Zie voor de lijst met berekeningscluster services worden ondersteund door gegevens Factory [gekoppelde services berekenen](data-factory-compute-linked-services.md) . In deze zelfstudie u niet elke service berekeningscluster gebruikt. 

In deze stap maakt u twee gekoppelde services: **AzureStorageLinkedService1** en **AzureSqlLinkedService1**. AzureStorageLinkedService1 gekoppeld service koppelingen een Azure-Account voor opslagruimte en AzureSqlLinkedService koppelingen een Azure SQL-database naar de fabriek gegevens: **ADFTutorialDataFactory**. 

### <a name="create-the-azure-storage-linked-service"></a>Maken van de Azure gekoppeld opslagservice

4. Met de rechtermuisknop op de **Gekoppelde Services** in de solution explorer, wijs **toevoegen**en klik op **Nieuw Item**.      
5. In het dialoogvenster **Nieuw Item toevoegen** , selecteer **Gekoppelde Azure Storage-Service** in de lijst en klikt u op **toevoegen**. 

    ![Nieuwe gekoppelde Service](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
 
3. Vervang `<accountname>` en `<accountkey>`* met de naam van uw account Azure opslag en de sleutel. 

    ![Azure opslag gekoppeld Service](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)

4. Sla het bestand **AzureStorageLinkedService1.json** .

> Zie de [gegevens van/naar Azure Blob verplaatsen](data-factory-azure-blob-connector.md#azure-storage-linked-service) voor meer informatie over de eigenschappen van JSON.

### <a name="create-the-azure-sql-linked-service"></a>Maken van de gekoppelde Azure SQL-service

5. Met de rechtermuisknop op knooppunt **Gekoppelde Services** in de **Oplossing Explorer** opnieuw, wijs **toevoegen**en klik op **Nieuw Item**. 
6. Schakel ditmaal Selecteer **Gekoppelde Azure SQL-Service**en klikt u op **toevoegen**. 
7. Vervang in het **bestand AzureSqlLinkedService1.json**, `<servername>`, `<databasename>`, `<username@servername>`, en `<password>` met namen van uw Azure SQL server-database, gebruikersaccount en wachtwoord.    
8.  Sla het bestand **AzureSqlLinkedService1.json** . 

> [AZURE.NOTE]
> Zie de [gegevens van/naar Azure SQL-Database verplaatsen](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) voor meer informatie over de eigenschappen van JSON.

## <a name="create-datasets"></a>Gegevenssets maken
In de vorige stap, u gekoppelde services **AzureStorageLinkedService1** en **AzureSqlLinkedService1** een Azure Storage-account en Azure SQL-database aan de fabriek gegevens hebt gemaakt: **ADFTutorialDataFactory**. In deze stap definieert u twee gegevenssets-- **InputDataset** en **OutputDataset** --die de gegevens van de invoer/uitvoer die is opgeslagen in de gegevens waarnaar wordt verwezen door AzureStorageLinkedService1 en AzureSqlLinkedService1 respectievelijk vertegenwoordigen. Voor InputDataset geeft u de container blob met een blob met de brongegevens. Voor OutputDataset geeft u de SQL-tabel waarin de uitvoergegevens.

### <a name="create-input-dataset"></a>Invoer gegevensset maken
In deze stap maakt u een gegevensset met de naam **InputDataset** die naar een container blob in de opslagruimte van Azure dat wordt aangeduid door de service **AzureStorageLinkedService1** gekoppeld verwijst. Een tabel is van een rechthoekig gegevensset en is het enige type gegevensset nu ondersteund. 

9. Met de rechtermuisknop op de **tabellen** in de **Oplossing Explorer**, wijs **toevoegen**en klik op **Nieuw Item**.
10. Klik in het dialoogvenster **Nieuw Item toevoegen** **Azure Blob**selecteren en klik op **toevoegen**.   
10. De JSON-tekst vervangen door de volgende tekst en sla het bestand **AzureBlobLocation1.json** . 

        {
          "name": "InputDataset",
          "properties": {
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
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

     Houd rekening met de volgende punten: 
    
    - gegevensset **type** is ingesteld op **AzureBlob**.
    - **linkedServiceName** is ingesteld op **AzureStorageLinkedService**. U kunt deze gekoppelde service gemaakt in stap 2.
    - **mappad** is ingesteld op de container **adftutorial** . U kunt ook de naam van een blob in de map met de eigenschap **bestandsnaam** opgeven. Aangezien u niet de naam van de label opgeeft, worden gegevens uit alle BLOB's in de container wordt beschouwd als een invoergegevens.  
    - opmaak **type** is ingesteld op **tekstopmaak**
    - Er zijn twee velden in het tekstbestand – **Voornaam** en **Achternaam** , gescheiden door een kommateken (**columnDelimiter**) 
    - De **beschikbaarheid van** is ingesteld op **elk uur** (**frequentie** is ingesteld op **uur** en **interval** is ingesteld op **1**). Daarom Data Factory Hiermee wordt gezocht naar invoergegevens per uur in de hoofdmap van blob container (**adftutorial**) dat u hebt opgegeven. 
    
    Als u een **bestandsnaam** voor een gegevensset **invoer** niet opgeeft, worden alle bestanden/BLOB's uit de invoer map (**mappad**) worden beschouwd als invoer. Als u een bestandsnaam in de JSON opgeeft, wordt alleen het opgegeven bestand/blob asn invoer.
 
    Als u een **bestandsnaam** voor een **uitvoertabel**niet opgeeft, wordt de gegenereerde bestanden in de **mappad** heten in de volgende indeling: gegevens. &lt;Guid\&BT;. txt (voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    Gebruik de eigenschap **partitionedBy** **mappad** en de **bestandsnaam** dynamisch op basis van de tijd **SliceStart** stelt. In het volgende voorbeeld, mappad gebruikt jaar, maand en dag van de SliceStart (begintijd van het segment wordt verwerkt) en bestandsnaam uur vanaf de SliceStart gebruikt. Als een segment wordt geproduceerd voor 2016 bijvoorbeeld-09-20T08:00:00, de mapnaam is ingesteld op wikidatagateway/wikisampledataout/2016/09/20 en de bestandsnaam is ingesteld op 08.csv. 

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 

> [AZURE.NOTE]
> Zie de [gegevens van/naar Azure Blob verplaatsen](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) voor meer informatie over de eigenschappen van JSON.

### <a name="create-output-dataset"></a>Uitvoer gegevensset maken
In deze stap maakt u een uitvoer gegevensset met de naam **OutputDataset**. Deze dataset verwijst naar een SQL-tabel in de dat wordt aangeduid met **AzureSqlLinkedService1**Azure SQL-database. 

11. Opnieuw met de rechtermuisknop op de **tabellen** in de **Oplossing Explorer** , wijs **toevoegen**en klik op **Nieuw Item**.
12. Selecteer **Azure SQL**in het dialoogvenster **Nieuw Item toevoegen** en klik op **toevoegen**. 
13. De JSON-tekst vervangen door de volgende JSON en sla het bestand **AzureSqlTableLocation1.json** .

        {
          "name": "OutputDataset",
          "properties": {
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
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService1",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

     Houd rekening met de volgende punten: 
    
    - gegevensset **type** is ingesteld op **AzureSQLTable**.
    - **linkedServiceName** is ingesteld op **AzureSqlLinkedService** (u hebt deze gekoppelde service gemaakt in stap 2).
    - **tabelnaam** is ingesteld op **emp**.
    - Er zijn drie kolommen – **ID**, **Voornaam**en **Achternaam** – in de tabel emp in de database. ID is een id-kolom, dus u hier opgeeft moet, alleen **Voornaam** en **Achternaam** .
    - De **beschikbaarheid van** is ingesteld op **elk uur** (**interval** instellen op **uur** en **interval** ingesteld op **1**).  De gegevens Factory-service genereert een uitvoer gegevens segment per uur in de tabel **emp** in de SQL Azure-database.

> [AZURE.NOTE]
> Zie de [gegevens van/naar Azure SQL-Database verplaatsen](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) voor meer informatie over de eigenschappen van JSON.

## <a name="create-pipeline"></a>Verkooppijplijn maken 
U hebt invoer/uitvoer gekoppelde services en tabellen dusverre gemaakt. Nu u een pijplijn maken met een **Kopie activiteit** om te kopiëren gegevens uit de Azure blob met Azure SQL-database. 


1. Met de rechtermuisknop op **pijpleidingen** in **Solution Explorer**, wijs **toevoegen**en klik op **Nieuw Item**.  
15. Selecteer **Kopiëren gegevens verkooppijplijn** in het dialoogvenster **Nieuw Item toevoegen** en klik op **toevoegen**. 
16. De JSON vervangen door de volgende JSON en sla het bestand **CopyActivity1.json** .
            
        {
          "name": "ADFTutorialPipeline",
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
                  "style": "StartOfInterval",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z",
            "isPaused": false
          }
        }

    Houd rekening met de volgende punten:

    - Klik in de sectie activiteiten is er slechts één activiteit waarvan het **type** is ingesteld op **kopiëren**.
    - Invoer voor de activiteit is ingesteld op **InputDataset** en uitvoer voor de activiteit is ingesteld op **OutputDataset**.
    - Klik in de sectie **typeProperties** **BlobSource** is opgegeven als het brontype en **SqlSink** is opgegeven als het type sink.

    Vervang de waarde van de eigenschap **start** met de huidige waarde voor de dag- en **eindtijd** met de volgende dag. U kunt opgeven alleen het deel van de datum en het tijdgedeelte van het datum-tijd overslaan. Bijvoorbeeld: "2016-02-03', die equivalent is aan ' 2016-02-03T00:00:00Z"
    
    Beide begin en einde datum/tijd moet [ISO](http://en.wikipedia.org/wiki/ISO_8601)-indeling. Bijvoorbeeld: 2016-10-14T16:32:41Z. **De eindtijd** is optioneel, maar wordt deze gebruikt in deze zelfstudie. 
    
    Als u geen waarde voor de eigenschap **end** opgeeft, wordt deze worden berekend als "**starten + 48 uur**". Als u wilt de pijplijn voor onbepaalde tijd uitvoeren, geeft u **9999-09-09** als de waarde voor de eigenschap **end** .
    
    Klik in het voorgaande voorbeeld zijn er 24 gegevens segmenten elk segment gegevens per uur is geproduceerd.

## <a name="publishdeploy-data-factory-entities"></a>Gegevens Factory entiteiten publiceren/implementeren
In deze stap geeft u de gegevens Factory entiteiten (gekoppelde services, gegevenssets en verkooppijplijn) publiceert u eerder hebt gemaakt. U ook opgeven de naam van de nieuwe gegevens fabriek moet worden gemaakt waarin deze entiteiten.  

18. Met de rechtermuisknop op project in de Solution Explorer en klik op **publiceren**. 
19. Als u het dialoogvenster **aanmelden bij uw Microsoft-account** ziet, voert u uw referenties voor het account dat Azure abonnement heeft en klik op **aanmelden**.
20. Hier ziet u het volgende dialoogvenster:

    ![Het dialoogvenster publiceren](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
21. Voer de volgende stappen uit in de pagina factory configureren: 
    1. Selecteer de optie **Nieuwe gegevens Factory maken** .
    2. Voer **VSTutorialFactory** **in te voeren**.  
    
        > [AZURE.IMPORTANT]  
        > De naam van de fabriek Azure gegevens moet uniek zijn. Als u een foutmelding over de naam van de gegevens factory ontvangt wanneer u publiceert, wijzig de naam van de gegevens factory (bijvoorbeeld yournameVSTutorialFactory) en probeer opnieuw publiceren. Zie [Gegevens Factory - regels voor naamgeving van](data-factory-naming-rules.md) onderwerp voor naming regels voor Data Factory-onderdelen.     
    3. Selecteer uw Azure-abonnement voor het veld **abonnement** .
     
        > [AZURE.IMPORTANT]Als u een abonnement niet ziet, zorg ervoor dat u bent aangemeld met een account die is een beheerder of co-beheerders van het abonnement.  
    4. Selecteer de **resourcegroep** voor de fabriek gegevens moet worden gemaakt. 5. Het **gebied** voor de fabriek gegevens selecteren. Alleen regio's die worden ondersteund door de service Factory gegevens worden weergegeven in de vervolgkeuzelijst.
6. Klik op **volgende** om te schakelen naar de pagina **Publiceren Items** .
    
        ![Data factory-pagina configureren](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
23. De pagina **Publiceren Items** Zorg ervoor dat alle gegevens-bedrijven entiteiten zijn geselecteerd en klik op **volgende** om naar de pagina **Summary** te schakelen.
    
    ![Items pagina publiceren](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
24. Bekijk het overzicht en klik op **volgende** om de implementatie starten en de **Implementatie-Status**weergeven.

    ![Overzichtspagina publiceren](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
25. **Implementatiestatus** op de pagina ziet u de status van het implementatieproces. Klik op Voltooien wanneer de implementatie klaar is. 
    ![Statuspagina voor implementatie](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png) Houd rekening met de volgende punten: 

- Als het foutbericht: '**dit abonnement niet is geregistreerd als u wilt gebruiken naamruimte Microsoft.DataFactory**', een van de volgende handelingen uit en probeer opnieuw te publiceren: 

    - Voer de volgende opdracht voor het registreren van de gegevens Factory-provider in Azure PowerShell. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        U kunt de volgende opdracht uit om te bevestigen dat de gegevens fabriek provider is geregistreerd uitvoeren. 
    
            Get-AzureRmResourceProvider
    - Meld u aan met het Azure abonnement in de [portal van Azure](https://portal.azure.com) en navigeer naar een gegevens Factory-blade (of) maken van een fabriek gegevens in de portal van Azure. Deze actie worden automatisch de provider voor u geregistreerd.
-   De naam van de gegevens fabriek kan worden geregistreerd als een DNS-naam in de toekomst en dus worden openbaar zichtbaar.

> [AZURE.IMPORTANT] Als u wilt gegevens Factory-exemplaren maken, moet u een beheerder/co-beheerders van de Azure-abonnement

## <a name="summary"></a>Overzicht
In deze zelfstudie hebt u een factory Azure-gegevens kopiëren hebt gemaakt gegevens uit een Azure blob met een Azure SQL-database. U gebruikte Visual Studio voor het maken van de gegevens fabriek, gekoppelde services gegevenssets en een pijplijn. Hier volgen de hoofdstappen beschreven die u in deze zelfstudie hebt uitgevoerd:  

1.  Een Azure **gegevens factory**gemaakt.
2.  **Gekoppelde services**gemaakt:
    1. Een service **Azure Storage** gekoppeld aan uw opslagruimte van Azure-account waarin invoergegevens koppelen.    
    2. Een gekoppelde **SQL Azure** -service om te koppelen van de Azure SQL-database waarin de uitvoergegevens. 
3.  Gemaakt **gegevenssets**, waarin invoergegevens en uitvoergegevens voor pijpleidingen.
4.  Een **verkooppijplijn** gemaakt met een **Kopie activiteit** met **BlobSource** als de bron- en **SqlSink** als sink. 


## <a name="use-server-explorer-to-view-data-factories"></a>Server Explorer gebruiken om te bekijken van gegevens factory 's

1. Klik op **weergave** in het menu in **Visual Studio**, en klik op de **Server Explorer**.
2. Klik in het venster Server Explorer **Azure** uitvouwen en **Gegevens Factory**uitvouwen. Als u ziet, **Meld u aan bij Visual Studio**, voer wordt het **account** dat is gekoppeld aan uw Azure-abonnement en klik op **Doorgaan**. **Wachtwoord**, en klik op **aanmelden**. Visual Studio wordt geprobeerd voor informatie over alle gegevens van Azure-factory's in uw abonnement. U ziet dat de status van deze bewerking in het venster **Gegevens Factory-takenlijst** .
    ![Server Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. U kunt met de rechtermuisknop op een factory gegevens en selecteer Data Factory exporteren naar een nieuw Project om een Visual Studio-project op basis van een bestaande gegevens factory te maken.
    ![Gegevens factory exporteren naar een project VS](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Hulpmiddelen voor gegevens Factory bijwerken voor Visual Studio
Als u wilt bijwerken Azure gegevens Factory-hulpprogramma's voor Visual Studio, volgt u de volgende stappen uit:

1. Klik in het menu op **Extra** en selecteer **Extensions en Updates**. 
2. Selecteer de **Updates die** in het linkerdeelvenster en selecteer vervolgens **Visual Studio-galerie**.
4. Selecteer **Azure gegevens Factory-hulpprogramma's voor Visual Studio** en klik op **bijwerken**. Als u dit item niet ziet, hebt u al de meest recente versie van de hulpmiddelen voor. 

Zie [Monitor gegevenssets en pijplijn](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) voor instructies over het gebruik van de Azure-portal om de verkooppijplijn en gegevenssets te bewaken die u in deze zelfstudie hebt gemaakt.

## <a name="see-also"></a>Zie ook
| Onderwerp | Beschrijving |
| :---- | :---- |
| [Gegevens verkeer activiteiten](data-factory-data-movement-activities.md) | In dit artikel vindt u gedetailleerde informatie over de kopie activiteit die u in deze zelfstudie hebt gebruikt. |
| [Plannings- en kan worden uitgevoerd](data-factory-scheduling-and-execution.md) | In dit artikel wordt uitgelegd dat de plannings- en execution aspecten van Azure gegevens Factory-toepassingsmodel. |
| [Pijpleidingen](data-factory-create-pipelines.md) | In dit artikel vindt u meer informatie over pijpleidingen en activiteiten in fabriek van Azure-gegevens |
| [Gegevenssets](data-factory-create-datasets.md) | Dit artikel vindt u meer informatie over gegevenssets in fabriek van Azure-gegevens.
| [Bewaken en pijpleidingen met Monitoring App beheren](data-factory-monitor-manage-app.md) | In dit artikel wordt beschreven hoe bewaken, beheren en fouten opsporen in pijpleidingen met behulp van de controle Management-App. 
