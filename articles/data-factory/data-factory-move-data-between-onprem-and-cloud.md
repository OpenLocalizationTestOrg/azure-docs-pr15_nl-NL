<properties 
    pageTitle="Verplaats gegevens - Data Management Gateway | Microsoft Azure"
    description="Een data gateway instellen om gegevens te verplaatsen tussen de on-premises en de cloud. Data Management Gateway in fabriek van Azure-gegevens gebruiken om uw gegevens te verplaatsen." 
    keywords="gegevensgateway, de gegevensintegratie van de, gegevens, gatewayreferenties verplaatsen"
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="jingwang"/>

# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>Gegevens verplaatsen tussen on-premises bronnen en de cloud met Data Management Gateway
Dit artikel bevat een overzicht van de gegevensintegratie tussen on-premises implementatie gegevensarchieven en cloud gegevens met gegevens Factory. Dit is gebaseerd op het artikel [Gegevens verkeer activiteiten](data-factory-data-movement-activities.md) en andere gegevens factory core concepten artikelen: [gegevenssets](data-factory-create-datasets.md) en [pijpleidingen](data-factory-create-pipelines.md). 

## <a name="data-management-gateway"></a>Data Management Gateway
U moet Data Management Gateway installeren op uw computer on-premises implementatie zwevend gegevens uit een on-premises implementatie gegevensopslag inschakelen. De gateway kan worden geïnstalleerd op dezelfde computer als de gegevensopslag of op een andere computer zo lang maken als de gateway kan worden verbonden met de gegevensopslag. 

> [AZURE.IMPORTANT] Zie [Data Management Gateway](data-factory-data-management-gateway.md) -artikel voor meer informatie over Data Management Gateway.   

De volgende procedure ziet u hoe u het maken van een fabriek gegevens met een pijplijn die gegevens uit een lokale **SQL Server** -database naar een Azure-blobopslag verplaatst. Als onderdeel van de procedure die u kunt installeren en configureren van de Data Management Gateway op uw computer. 

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a>Demonstratie: on-premises gegevens naar cloud kopiëren
  
## <a name="create-data-factory"></a>Gegevens factory maken
In deze stap gebruikt u de Azure-portal een Azure gegevens Factory-exemplaar **ADFTutorialOnPremDF**met de naam te maken. 

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com). 
2.  Klik op **+ Nieuw** **Intelligence + analytics**op en klik op **Gegevens fabriek**.

    ![Nieuw -> DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
2. Voer in het blad **nieuwe gegevens factory** **ADFTutorialOnPremDF** voor de naam.

    ![Toevoegen aan Startboard](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

    > [AZURE.IMPORTANT] 
    > De naam van de fabriek Azure gegevens moet uniek zijn. Als het foutbericht: **gegevens factory naam "ADFTutorialOnPremDF" is niet beschikbaar**, wijzig de naam van de gegevens fabriek (bijvoorbeeld yournameADFTutorialOnPremDF) en probeert opnieuw te maken. Gebruik deze naam in plaats van ADFTutorialOnPremDF tijdens het uitvoeren van de overige stappen in deze zelfstudie.
    > 
    > De naam van de gegevens fabriek kan worden geregistreerd als een **DNS-** naam in de toekomst en dus worden openbaar zichtbaar.
3. Selecteer het **Azure abonnement** waar u de gegevens fabriek moet worden gemaakt. 
4.  Selecteer bestaande **resourcegroep** of een resourcegroep maken. Maak een resourcegroep met de naam voor de zelfstudie: **ADFTutorialResourceGroup**. 
5.  Klik op **maken** op het **nieuwe gegevens factory** -blad.

    > [AZURE.IMPORTANT] Als u wilt gegevens Factory-exemplaren maken, moet u een lid van de rol [Inzender Factory](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) op het niveau van de groep abonnement of een resource zijn. 
11. Nadat het is gemaakt, ziet u het blad **Gegevens Factory** zoals wordt weergegeven in de volgende afbeelding:

    ![Gegevens Factory-startpagina](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Gateway maken
5. Klik in het blad **Gegevens Factory** op **auteur en implementeren** tegel aan het starten van de **Editor** voor de fabriek gegevens.

    ![Ontwerpen en implementeren van de tegel](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
6.  Klik in de gegevens Factory Editor op **... Meer** op de werkbalk en klik vervolgens op **nieuwe gegevensgateway**. U kunt ook met de rechtermuisknop op de **Gegevensgateways** in de boomstructuur en klik op **nieuwe gegevensgateway**. 

    ![Nieuwe gegevensgateway op werkbalk](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
2. Klik in het blad **maken** **adftutorialgateway** voor de **naam**, en klik op **OK**.    

    ![Gateway-blade maken](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)
3. Klik in het blad **configureren** op **rechtstreeks op deze computer installeren**. Deze actie in het installatiepakket voor de gateway is gedownload, installeert, configureert en registreert de gateway op de computer.  

    > [AZURE.NOTE] 
    > Internet Explorer of een Microsoft-ClickOnce compatibele webbrowser gebruiken.
    > 
    > Als u Chrome gebruikt, gaat u naar de [webwinkel Chrome](https://chrome.google.com/webstore/), zoeken met "ClickOnce" trefwoord, kies een van de ClickOnce-extensies en installeer deze. 
    >  
    > Doe hetzelfde voor Firefox (installatie-invoegtoepassing). Klik op de knop **Menu openen** op de werkbalk (**drie horizontale lijnen** in de rechterbovenhoek), klik op **invoegtoepassingen**, zoekopdracht met trefwoord 'ClickOnce', kies een van de ClickOnce-extensies en installeer deze.    

    ![Gateway - blade configureren](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    Deze manier is de eenvoudigste manier (met één muisklik) downloaden, installeren, configureren en de gateway registreren in één stap. De toepassing **Microsoft Data Management Gateway Configuration Manager** is geïnstalleerd op uw computer, kunt u zien. U kunt ook de uitvoerbare **ConfigManager.exe** vinden in de map: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**.

    U kunt ook downloaden en installeren van de gateway handmatig met behulp van de koppelingen in deze blade en registreren met de toets wordt weergegeven in het tekstvak **Nieuwe sleutel** .
    
    Zie [Data Management Gateway](data-factory-data-management-gateway.md) -artikel voor het laatste nieuws over de gateway.

    >[AZURE.NOTE] U moet een beheerder op de lokale computer installeren en configureren van de Data Management Gateway is. U kunt extra gebruikers toevoegen aan de **Data Management Gateway gebruikers** lokale Windows-groep. De leden van deze groep kunnen u het hulpmiddel Data Management Gateway Configuration Manager gebruiken voor het configureren van de gateway. 

5. Wacht enkele minuten of wacht totdat u het volgende Meldingsbericht ziet:

    ![De installatie van de gateway is voltooid](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png) 
6. Start de toepassing **Data Management Gateway Configuration Manager** op uw computer. Typ in het venster **Zoeken** **Data Management Gateway** voor toegang tot dit hulpprogramma. U kunt ook de uitvoerbare **ConfigManager.exe** vinden in de map: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared** 

    ![Gateway Configuration Manager](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
6. Bevestig dat u ziet `adftutorialgateway is connected to the cloud service` bericht. De statusbalk onder worden **verbonden met de cloudservice** samen met een **groen vinkje**weergegeven.

    Klik op het tabblad **Start** kunt u ook de volgende bewerkingen doen: 
    - **Registreren** een gateway met een sleutel van de Azure-portal met behulp van de Register-knop. 
    - **Stoppen met** de Data Management Gateway Host-Service op uw gatewaycomputer. 
    - **Planning-updates** worden geïnstalleerd op een bepaald tijdstip van de dag. 
    - Weergeven wanneer de gateway **Laatst bijgewerkt is**.
    - Geef de tijd waarop een update van de gateway kan worden geïnstalleerd. 

8. Overschakelen naar het tabblad **Instellingen** . Het certificaat dat is opgegeven in de sectie **certificaat** wordt gebruikt voor referenties voor de on-premises implementatie gegevensopslag die u in de portal opgeeft versleutelen/ontsleutelen. (optioneel) Klik op **wijzigen** als u wilt uw eigen certificaat in plaats daarvan gebruiken. De gateway standaard het certificaat dat automatisch wordt gegenereerd door de gegevens Factory-service.

    ![Configuratie van de gateway-certificaten](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    U kunt ook de volgende bewerkingen uit op het tabblad **Instellingen** doen: 
    - Bekijk of Exporteer het certificaat dat wordt gebruikt door de gateway.
    - Wijzig het HTTPS-eindpunt dat is gebruikt door de gateway.    
    - Stel een HTTP-proxy moet worden gebruikt door de gateway.   
9. (optioneel) Ga naar het tabblad **diagnostische hulpprogramma's** , schakelt u de optie **Logboekregistratie inschakelen** als u wilt inschakelen van logboekregistratie die u gebruiken kunt om eventuele problemen met de gateway. De logboekinformatie vindt u in de **Logboeken** onder **Logboeken toepassingen en Services** -> **Data Management Gateway** knooppunt. 

    ![Tabblad Diagnostische gegevens](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    U kunt ook de volgende acties uitvoeren op het tabblad **diagnostische hulpprogramma's** : 
    
    - Gebruik onderdeel van de **Testverbinding** met een lokale gegevensbron met de gateway.
    - Klik op **Logboeken weergeven** als u wilt zien van de Data Management Gateway log in een venster Logboeken. 
    - Klik op **Logboeken verzenden** als u wilt uploaden van een zip-bestand waaraan de logboekbestanden van de afgelopen zeven dagen naar Microsoft om oplossen van problemen met uw te vereenvoudigen. 
10. Klik op het tabblad **Diagnostische gegevens** in de sectie **Verbinding testen** , selecteert u **SQL Server** voor het type van de gegevensopslag, voer de naam van de database-server, naam van de database, verificatietype opgeven, gebruikersnaam en wachtwoord, en klik op **testen** om te testen of de gateway kan worden verbonden met de database. 
11. Overschakelen naar de webbrowser en klik in de **Azure-portal**, klik op **OK** op het blad **configureren** en vervolgens op het blad voor de **nieuwe gegevensgateway** .
6. Hier ziet u **adftutorialgateway** onder **Gegevensgateways** in de structuurweergave aan de linkerkant.  Als u erop klikt, ziet u de bijbehorende JSON. 
    

## <a name="create-linked-services"></a>Gekoppelde services maken 
In deze stap maakt u twee gekoppelde services: **AzureStorageLinkedService** en **SqlServerLinkedService**. De **SqlServerLinkedService** koppelingen een lokale SQL Server-database en de **AzureStorageLinkedService** gekoppelde service koppelingen een Azure blob opslaan naar de fabriek gegevens. U kunt een pijplijn maken verderop in dit scenario die gegevens van de on-premises implementatie SQL Server-database naar de store Azure blob kopiëren. 

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>Een gekoppelde service toevoegen aan een lokale SQL Server-database
1.  Klik in de **Gegevens Factory-Editor**, klikt u op de werkbalk op **nieuwe gegevens opslaan** en selecteer **SQL Server**. 

    ![Nieuwe SQL Server gekoppeld-service](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png) 
3.  Voer in de **JSON-editor** aan de rechterkant de volgende stappen uit: 
    1. Geef voor de **gatewayName**, **adftutorialgateway**. 
    2. In de **connectionString**, volgt u de volgende stappen uit: 
        1. Voer voor **servernaam**, de naam van de server waarop de SQL Server-database.
        2. Voor **databasenaam**, voer de naam van de database.
        3. Klik op de knop **versleutelen** op de werkbalk. Dit is gedownload op en start de toepassing referenties beheren.
        
            ![Referenties Manager-toepassing](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
        5. In het dialoogvenster **Instelling referenties** opgeven verificatietype, gebruikersnaam en wachtwoord en klik op **OK**. Als de verbinding geslaagd is, de versleutelde referenties zijn opgeslagen in de JSON en het dialoogvenster wordt gesloten. 
        6. Sluit het lege browsertabblad die in het dialoogvenster gestart als deze optie is niet automatisch gesloten en terug te gaan naar het tabblad met de portal van Azure. 
  
            Op de gatewaycomputer deze referenties zijn **versleuteld** met behulp van een certificaat dat u eigenaar van de gegevens Factory-service is. Als u gebruiken het certificaat dat is gekoppeld aan de Data Management Gateway in plaats daarvan wilt, raadpleegt u [referenties veilig instellen](#set-credentials-and-security).    
    1.  Klik op **Deploy** op de opdrachtbalk om te implementeren van de service SQL Server is gekoppeld. Hier ziet u de gekoppelde service in de boomstructuur. 
        
        ![SQL Server gekoppeld-service in de boomstructuur](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)  

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Een gekoppelde service voor een Azure opslag-account toevoegen
 
1. De **Gegevens Factory-Editor**, klikt u op **nieuwe gegevens opslaan** in de balk met opdrachten en klik op **Azure opslag**.
2. Voer de naam van uw account Azure opslag voor de **naam van het Account**.
3. Voer de code voor uw account Azure opslag voor de **accountsleutel**in.
4. Klik op **Deploy** als u wilt de **AzureStorageLinkedService**implementeren.
   
 
## <a name="create-datasets"></a>Gegevenssets maken
In deze stap maakt invoer en uitvoer gegevenssets die invoer- en uitvoerbereik gegevens voor de kopieeropdracht vertegenwoordigen (On-premises SQL Server-database = > Azure-blobopslag). Voordat u gegevenssets maakt, volgt u de volgende stappen (de lijst gedetailleerde stappen volgt):

- Een tabel met de naam **emp** in de SQL Server-Database die u als een gekoppelde service hebt toegevoegd aan de fabriek gegevens maken en voeg een paar voorbeelden van vermeldingen in de tabel.
- Maak een blob container met de naam **adftutorial** in het Azure blob storage mailaccount dat u als een gekoppelde service hebt toegevoegd aan de fabriek gegevens.

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a>On-premises SQL Server voorbereiden voor de zelfstudie

1. In de database die u hebt opgegeven voor de on-premises implementatie SQL Server gebruik gekoppelde service (**SqlServerLinkedService**), het volgende SQL-script de **emp** -tabel maken in de database.

        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL, 
            FirstName varchar(50),
            LastName varchar(50),
            CONSTRAINT PK_emp PRIMARY KEY (ID)
        )
        GO 
2. Sommige steekproef in de tabel invoegen: 

        INSERT INTO emp VALUES ('John', 'Doe')
        INSERT INTO emp VALUES ('Jane', 'Doe')

### <a name="create-input-dataset"></a>Invoer gegevensset maken

1. Klik in de **Gegevens Factory-Editor**op **... Meer**, klikt u op **nieuwe gegevensset** in de balk met opdrachten en **SQL Server-tabel**op. 
2.  De JSON in het rechterdeelvenster vervangen door de volgende tekst:
        
            {       
                "name": "EmpOnPremSQLTable",
                "properties": {
                    "type": "SqlServerTable",
                    "linkedServiceName": "SqlServerLinkedService",
                    "typeProperties": {
                        "tableName": "emp"
                    },
                    "external": true,
                    "availability": {
                        "frequency": "Hour",
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

    Houd rekening met de volgende punten: 

    - **type** is ingesteld op **SqlServerTable**.
    - **tabelnaam** is ingesteld op **emp**.
    - **linkedServiceName** is ingesteld op **SqlServerLinkedService** (u kunt deze gekoppelde service had gemaakt eerder in dit scenario.).
    - Voor een invoer gegevensset die niet door een andere verkooppijplijn in Azure gegevens fabriek wordt gegenereerd, moet u instellen **externe** op **true**. Dit betekent dat de invoergegevens is geproduceerd buiten de Azure gegevens Factory-service. Desgewenst kunt u met het element **externalData** in de sectie **beleid voor** externe gegevensbeleid opgeven.    

    Zie [gegevens uit SQL Server verplaatsen](data-factory-sqlserver-connector.md) voor meer informatie over de eigenschappen van JSON.
2. Klik op **Deploy** op de opdrachtbalk om te implementeren van de gegevensset.  


### <a name="create-output-dataset"></a>Uitvoer gegevensset maken

1.  Klik in de **Gegevens Factory-Editor**, klikt u op **nieuwe gegevensset** in de balk met opdrachten en **Azure-blobopslag**op.
2.  De JSON in het rechterdeelvenster vervangen door de volgende tekst: 

            {
                "name": "OutputBlobTable",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "AzureStorageLinkedService",
                    "typeProperties": {
                        "folderPath": "adftutorial/outfromonpremdf",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            }
  
    Houd rekening met de volgende punten: 
    
    - **type** is ingesteld op **AzureBlob**.
    - **linkedServiceName** is ingesteld op **AzureStorageLinkedService** (u kunt deze gekoppelde service hebt gemaakt in stap 2).
    - **mappad** is ingesteld op **adftutorial/outfromonpremdf** waar outfromonpremdf is voor de map in de container adftutorial. Maak de container **adftutorial** als deze nog niet bestaat. 
    - De **beschikbaarheid van** is ingesteld op **elk uur** (**interval** instellen op **uur** en **interval** ingesteld op **1**).  De gegevens Factory-service genereert een uitvoer gegevens segment per uur in de tabel **emp** in de Azure SQL-Database. 

    Als u een **bestandsnaam** voor een **uitvoertabel**niet opgeeft, wordt de gegenereerde bestanden in de **mappad** heten in de volgende indeling: gegevens. <Guid>.txt (bijvoorbeeld:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    Gebruik de eigenschap partitionedBy **mappad** en de **bestandsnaam** dynamisch op basis van de tijd **SliceStart** stelt. In het volgende voorbeeld, mappad gebruikt jaar, maand en dag van de SliceStart (begintijd van het segment wordt verwerkt) en bestandsnaam uur vanaf de SliceStart gebruikt. Als een segment wordt geproduceerd voor 2014 bijvoorbeeld-10-20T08:00:00, de mapnaam is ingesteld op wikidatagateway/wikisampledataout/2014/10/20 en de bestandsnaam is ingesteld op 08.csv. 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

 
    Zie de [gegevens van Azure-blobopslag verplaatsen](data-factory-azure-blob-connector.md) voor meer informatie over de eigenschappen van JSON.
2.  Klik op **Deploy** op de opdrachtbalk om te implementeren van de gegevensset. Bevestig dat u ziet dat zowel de gegevenssets in de boomstructuur.  

## <a name="create-pipeline"></a>Verkooppijplijn maken
In deze stap geeft u een **verkooppijplijn** kunt maken met één **Kopie activiteit** die **EmpOnPremSQLTable** als invoer gebruikt en **OutputBlobTable** als uitvoer.

1.  Klik in gegevens Factory Editor op **... Meer**, en klik op **nieuwe verkooppijplijn**. 
2.  De JSON in het rechterdeelvenster vervangen door de volgende tekst: 
    
            {
                "name": "ADFTutorialPipelineOnPrem",
                "properties": {
                "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
                "activities": [
                {
                    "name": "CopyFromSQLtoBlob",
                    "description": "Copy data from on-prem SQL server to blob",
                    "type": "Copy",
                    "inputs": [
                    {
                        "name": "EmpOnPremSQLTable"
                    }
                    ],
                    "outputs": [
                    {
                        "name": "OutputBlobTable"
                      }
                    ],
                    "typeProperties": {
                      "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from emp"
                      },
                      "sink": {
                        "type": "BlobSink"
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
                "start": "2016-07-05T00:00:00Z",
                "end": "2016-07-06T00:00:00Z",
                "isPaused": false
              }
            }

    > [AZURE.IMPORTANT]
    > Vervang de waarde van de eigenschap **start** met de huidige waarde voor de dag- en **eindtijd** met de volgende dag.

    Houd rekening met de volgende punten:
 
    - Klik in de sectie activiteiten is er alleen activiteit waarvan het **type** is ingesteld op **kopiëren**.
    - **Invoer** voor de activiteit is ingesteld op **EmpOnPremSQLTable** en **uitvoer** voor de activiteit is ingesteld op **OutputBlobTable**.
    - Klik in de sectie **typeProperties** **SqlSource** is opgegeven als het **brontype** en **BlobSink **is opgegeven als het **type sink**.
    - SQL-query `select * from emp` voor de eigenschap **sqlReaderQuery** van **SqlSource**is opgegeven.

     Beide begin en einde datum/tijd moet [ISO](http://en.wikipedia.org/wiki/ISO_8601)-indeling. Bijvoorbeeld: 2014-10-14T16:32:41Z. **De eindtijd** is optioneel, maar wordt deze gebruikt in deze zelfstudie. 
    
    Als u geen waarde voor de eigenschap **end** opgeeft, wordt deze worden berekend als "**starten + 48 uur**". Als u wilt de pijplijn voor onbepaalde tijd uitvoeren, geef **9/9/9999** als de waarde voor de eigenschap **end** . 
    
    U definieert de tijdsduur waarin de segmenten gegevens worden verwerkt, op basis van de **beschikbaarheid van de** eigenschappen die zijn gedefinieerd voor elke Azure gegevens Factory-gegevensset.
    
    In het voorbeeld zijn er 24 gegevens segmenten elk segment gegevens per uur is geproduceerd.     
2. Klik op **Deploy** op de opdrachtbalk om te implementeren van de gegevensset (tabel is een rechthoekig gegevensset). Bevestig dat de pijplijn in de structuurweergave onder **pijpleidingen** knooppunt weergegeven.  
5. Klik nu op **X** tweemaal om te sluiten van de bladen terug te keren naar het blad **Gegevens Factory** voor de **ADFTutorialOnPremDF**.

**Gefeliciteerd!** Je hebt gemaakt van een fabriek Azure gegevens, gekoppelde services gegevenssets en een pijplijn en de pijplijn gepland.

#### <a name="view-the-data-factory-in-a-diagram-view"></a>De fabriek gegevens weergeven in een diagramweergave 
1. Klik op de tegel **Diagram** op de introductiepagina voor de **ADFTutorialOnPremDF** gegevens fabriek in de **portal van Azure**. :

    ![Diagram koppeling](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)

2. Hier ziet u het diagram die vergelijkbaar is met de volgende afbeelding:

    ![Diagramweergave](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    U kunt inzoomen, uitzoomen, in-en uitzoomen op 100%, en uitzoomen passend maken, automatisch plaatsen pijpleidingen en gegevenssets en informatie over de afkomst weergeven (items worden gemarkeerd boven en lager van geselecteerde items).  Een object (invoer/uitvoer gegevensset of verkooppijplijn) om eigenschappen voor deze weer te geven, kunt u dubbelklikken. 

## <a name="monitor-pipeline"></a>Monitor verkooppijplijn
In deze stap gebruikt u de Azure-portal om de wat er gebeurt in een fabriek Azure gegevens te houden. U kunt ook de PowerShell-cmdlets gebruiken om de gegevenssets en pijpleidingen te houden. Zie voor meer informatie over het controleren van [Monitor en pijpleidingen beheren](data-factory-monitor-manage-pipelines.md).

5. Dubbelklik in het diagram, op **EmpOnPremSQLTable**.  

    ![EmpOnPremSQLTable segmenten](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)

6. U ziet dat alle gegevens van segmenten in **Gereed** omdat de duur van de verkooppijplijn (Begintijd Eindtijd) in het verleden. Het is ook omdat u de gegevens in de SQL Server-database hebt ingevoegd en er altijd wordt. Bevestig dat er geen segmenten worden weergegeven in de sectie **probleem segmenten** onderaan. Als u wilt weergeven op alle segmenten, klikt u op **Meer** onderaan in de lijst met segmenten. 
7. Klik nu op **OutputBlobTable**In het blad **gegevenssets** .

    ![OputputBlobTable segmenten](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
9. Klik op een segment uit de lijst en ziet u het blad **Gegevens segment** . U ziet activiteit wordt uitgevoerd voor het segment. Ziet u slechts één activiteit meestal uitvoeren.  

    ![Gegevens segment Blade](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Als het segment niet in de status **gereed is** , ziet u het boven segmenten die nog niet gereed en worden geblokkeerd door de huidige segment wordt uitgevoerd in de lijst **boven segmenten die niet gereed zijn** .

10. Klik op de **activiteit uitvoeren** in de lijst onderaan om **activiteiten uitvoeren details**weer te geven.

    ![Activiteiten uitvoeren Details blade](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

    Hier ziet u informatie zoals doorvoer, duur en de gateway die wordt gebruikt om de gegevens te brengen. 
11. Klik op de **X** om te sluiten van alle bladen totdat u 
12. terug te gaan naar het thuis blad voor de **ADFTutorialOnPremDF**.
14. (optioneel) Klik op **pijpleidingen** **ADFTutorialOnPremDF**, en op detailanalyse invoer tabellen (**verbruikt**) of uitvoer gegevenssets (**geproduceerde**).
15. Hulpprogramma's zoals gebruik hulpprogramma's zoals [Microsoft opslag Explorer](http://storageexplorer.com/) gebruiken om te controleren of een blob /-bestand is gemaakt voor elk uur.

    ![Azure opslag Explorer](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Volgende stappen

- Zie [Data Management Gateway](data-factory-data-management-gateway.md) -artikel voor het laatste nieuws over de Data Management Gateway.
- Zie [gegevens kopiëren van Azure Blob naar SQL Azure](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor meer informatie over het gebruik van de activiteit kopiëren gegevens uit een bron-gegevensopslag verplaatsen naar een gegevensopslag sink. 
