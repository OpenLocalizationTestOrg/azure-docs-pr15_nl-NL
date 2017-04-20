<properties 
    pageTitle="Opgeslagen Procedure activiteit van SQL Server" 
    description="Leer hoe u de opgeslagen Procedure activiteit van SQL Server kunt gebruiken om aan te roepen van een opgeslagen procedure in een Azure SQL-Database of Azure SQL Data Warehouse uit een pijplijn Data Factory." 
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
    ms.date="09/30/2016" 
    ms.author="spelluru"/>

# <a name="sql-server-stored-procedure-activity"></a>Opgeslagen Procedure activiteit van SQL Server
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics I-SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)

De opgeslagen Procedure van SQL Server-activiteit in een Data Factory- [verkooppijplijn](data-factory-create-pipelines.md) kunt u een opgeslagen procedure in een van de volgende gegevensarchieven roepen: 


- Azure SQL-Database 
- Azure SQL datawarehouse  
- SQL Server-Database in uw onderneming of een VM Azure. U moet Data Management Gateway installeren op dezelfde computer waarop de database of op een aparte computer om te voorkomen dat voor resources met de database. Data Management Gateway is een software dat verbinding maakt met on-premises gegevensbronnen/gegevensbronnen hosed in Azure VMs met cloudservices in een veilige en beheerde manier. Zie [gegevens tussen on-premises en cloud verplaatsen](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway. 

In dit artikel is gebaseerd op het artikel [gegevens transformatie activiteiten](data-factory-data-transformation-activities.md) , waarbij een een algemeen overzicht van gegevenstransformatie en de ondersteunde transformatie activiteiten wordt weergegeven.

## <a name="walkthrough"></a>Stapsgewijze instructies

### <a name="sample-table-and-stored-procedure"></a>Voorbeeldtabel en opgeslagen procedure
1. De volgende **tabel** in uw SQL Server Management Studio met Azure SQL-Database maken of een ander hulpmiddel u vertrouwd bent met. De kolom datetimestamp bevat de datum en tijd waarop de bijbehorende-ID wordt gegenereerd. 

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    ID is de uniek identificeren en de kolom datetimestamp is de datum en tijd waarop de bijbehorende-ID wordt gegenereerd.
    ![Voorbeeldgegevens](./media/data-factory-stored-proc-activity/sample-data.png)

    > [AZURE.NOTE] In dit voorbeeld wordt gebruikt Azure SQL-Database, maar werkt op dezelfde manier voor Azure SQL Data Warehouse en SQL Server-Database. 
2. Maak de volgende **opgeslagen procedure** die gegevens in de **sampletable**invoegt.

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS
        
        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

    > [AZURE.IMPORTANT] **Naam** en **behuizing** van de parameter (DateTime in dit voorbeeld) moeten overeenkomen met die van de parameter is opgegeven in de verkooppijplijn/activiteit JSON. Zorg ervoor dat in de definitie opgeslagen procedure **@** wordt gebruikt als een voorvoegsel voor de parameter.
    
### <a name="create-a-data-factory"></a>Maken van een fabriek gegevens  
4. Meld u aan bij [Azure-portal](https://portal.azure.com/). 
5. Klik op **Nieuw** in het linkermenu op **bedrijfsinformatie + Analytics**en klik op **Gegevens fabriek**.
    
    ![Nieuwe gegevens factory](media/data-factory-stored-proc-activity/new-data-factory.png)   
4.  Voer in het blad **nieuwe gegevens factory** **SProcDF** voor de naam. Azure Data Factory namen zijn **globaal uniek**. U moet de aanduiding voor de naam van de gegevens fabriek met uw naam, de succesvolle maken van de fabriek inschakelen.

    ![Nieuwe gegevens factory](media/data-factory-stored-proc-activity/new-data-factory-blade.png)      
3.  Selecteer uw **Azure-abonnement**. 
4.  Voer een van de volgende stappen uit voor de **Resourcegroep**: 
    1.  Klik op **Nieuw** en voer een naam voor de resourcegroep.
    2.  Klik op **bestaande gebruiken** en selecteer een bestaande resourcegroep.  
5.  Selecteer de **locatie** voor de fabriek gegevens.
6.  Selecteer **vastmaken aan dashboard** zodat u zien de fabriek gegevens op het dashboard volgende keer dat u zich hebt kunt aangemeld. 
6.  Klik op **maken** op het **nieuwe gegevens factory** -blad.
6.  Ziet u de gegevens fabriek wordt gemaakt in het **dashboard** van de Azure-portal. Nadat de gegevens fabriek is gemaakt, kunt u de gegevens factory-pagina, waarin u de inhoud van de fabriek gegevens zien.
    ![Gegevens Factory-startpagina](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Maak een gekoppelde Azure SQL-service  
Na het maken van de gegevens fabriek, moet u een gekoppelde Azure SQL-service die uw Azure SQL-Database is gekoppeld aan de fabriek gegevens maken. Deze database bevat de sampletable tabel en sp_sample opgeslagen procedure.

7.  Klik op **auteur en implementeren** op het blad **Gegevens Factory** voor **SProcDF** aan de gegevens Factory-Editor te starten.
2.  Klik op **nieuwe gegevens opslaan** op de balk met opdrachten en kies **Azure SQL-Database**. Hier ziet u de JSON-script voor het maken van een gekoppelde Azure SQL-service in de editor. 

    ![Nieuwe gegevensopslag](media/data-factory-stored-proc-activity/new-data-store.png)
4. In het script JSON, moet u de volgende wijzigingen aanbrengen: 
    1. Vervang ** &lt;servernaam&gt; ** met de naam van de Azure SQL Database-server.
    2. Vervang ** &lt;databasenaam&gt; ** met de database waarin u de tabel en de opgeslagen procedure hebt gemaakt.
    3. Vervang ** &lt; username@servername ** met het gebruikersaccount die toegang tot de database heeft.
    4. Vervang ** &lt;wachtwoord&gt; ** met het wachtwoord voor het gebruikersaccount. 

    ![Nieuwe gegevensopslag](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
5. Klik op **Deploy** op de opdrachtbalk om te implementeren van de gekoppelde service. Bevestig dat u ziet dat de AzureSqlLinkedService in de structuurweergave aan de linkerkant. 

    ![structuurweergave met gekoppelde-service](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Een gegevensset uitvoer maken
6. Klik op **... Meer** op de werkbalk op **nieuwe gegevensset**en op van **Azure SQL**. **Nieuwe gegevensset** op de balk met opdrachten en selecteer **Azure SQL**.

    ![structuurweergave met gekoppelde-service](media/data-factory-stored-proc-activity/new-dataset.png)
7. Het volgende JSON-script in naar de JSON-editor kopiëren en plakken.

        {               
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
7. Klik op **Deploy** op de opdrachtbalk om te implementeren van de gegevensset. Bevestig dat u ziet dat de gegevensset in de boomstructuur. 

    ![structuurweergave met gekoppelde services](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Een pijplijn met SqlServerStoredProcedure activiteit maken
Nu kunnen we een pijplijn maken met een activiteit SqlServerStoredProcedure.
 
9. Klik op **... Meer** weer te geven staaf- en klik op **nieuwe verkooppijplijn**. 
9. Het codefragment van de volgende JSON kopiëren/plakken. De **storedProcedureName** ingesteld op **sp_sample**. Naam en behuizing van de parameter **DateTime** moeten overeenkomen met de naam en behuizing van de parameter weer in de definitie van de opgeslagen procedure.  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                "start": "2016-08-02T00:00:00Z",
                "end": "2016-08-02T05:00:00Z",
                "isPaused": false
            }
        }

    Als u nodig hebt voor een parameter null doorgeeft, gebruik de volgende syntaxis: "param1": null (alle kleine letters). 
9. Klik op **Deploy** op de werkbalk om te implementeren van de pijplijn.  

### <a name="monitor-the-pipeline"></a>De pijplijn controleren

6. Klik op **X** om te gegevens Factory Editor bladen sluiten en terug naar het blad gegevens Factory en op **Diagram**.

    ![diagram-tegel](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
7. In de **Diagramweergave te klikken**ziet u een overzicht van de pijpleidingen en gegevenssets in deze zelfstudie gebruikt. 

    ![diagram-tegel](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
8. Dubbelklik in de diagramweergave te klikken, op de gegevensset **sprocsampleout**. U ziet de segmenten in gereed. Er moet worden vijf segmenten omdat een segment voor elk uur tussen de begintijd en eindtijd van de JSON is gemaakt.

    ![diagram-tegel](media/data-factory-stored-proc-activity/data-factory-slices.png) 
10. Als een segment **klaar** is, worden uitgevoerd een * *selecteren* in sampletable** query ten opzichte van de Azure SQL-database om te bevestigen dat de gegevens is ingevoegd in aan de tabel door de opgeslagen procedure.

    ![Uitvoergegevens](./media/data-factory-stored-proc-activity/output.png)

    Zie [Monitor de pijplijn](data-factory-monitor-manage-pipelines.md) voor gedetailleerde informatie over het controleren van Azure gegevens Factory pijpleidingen.  

> [AZURE.NOTE] In dit voorbeeld bevat de SprocActivitySample geen invoer. Als u wilt koppelen deze activiteit aan een activiteit stroom (dat wil zeggen voorafgaande verwerking), de uitvoer van de activiteit boven kunnen worden gebruikt als invoer bij deze activiteit. In dat geval deze activiteit niet wordt uitgevoerd totdat de boven activiteit is voltooid en de uitvoer van de voorafgaande activiteiten beschikbaar (in de status gereed zijn). De invoer kunnen niet rechtstreeks als een parameter voor de opgeslagen procedure activiteit worden gebruikt

## <a name="json-format"></a>Indeling van JSON
    {
        "name": "SQLSPROCActivity",
        "description": "description", 
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## <a name="json-properties"></a>JSON-eigenschappen

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
naam | Naam van de activiteit | Ja
Beschrijving | Beschrijving van wat de activiteit wordt gebruikt voor | Nee
type | SqlServerStoredProcedure | Ja
ingangen | Optioneel. Als u een invoer dataset opgeeft, moet deze zijn beschikbaar (in status 'Gereed') voor de activiteit opgeslagen procedure om uit te voeren. De invoer gegevensset kan niet worden gebruikt in de opgeslagen procedure als u een parameter. Het wordt alleen gebruikt om te controleren van de afhankelijkheid voordat de opgeslagen procedure activiteit wordt gestart. | Nee
uitvoer | U moet een gegevensset uitvoer voor een activiteit in een opgeslagen procedure opgeven. Uitvoer gegevensset Hiermee geeft u de **planning** voor de opgeslagen procedure activiteit (uur, wekelijks, maandelijks, enz.). <br/><br/>De gegevensset uitvoer moet een **gekoppelde service** die verwijst naar een Azure SQL-Database of een Azure SQL Data Warehouse of een SQL Server-Database waarin u de opgeslagen procedure om uit te voeren wilt gebruiken. <br/><br/>De uitvoer gegevensset kan dienen als een manier om door te geven dat het resultaat van de opgeslagen procedure voor latere verwerking door een andere activiteit (het[koppelen van activiteiten](data-factory-scheduling-and-execution.md#chaining-activities)) in de pijplijn. Echter wordt gegevens Factory niet automatisch schrijven de uitvoer van een opgeslagen procedure bij deze dataset. De opgeslagen procedure waarmee gegevens worden geschreven naar een SQL-tabel die de gegevensset uitvoer naar verwijst is. <br/><br/>In sommige gevallen mag de gegevensset uitvoer een **pop gegevensset**, die alleen gebruikt wordt om het schema voor het uitvoeren van de opgeslagen procedure activiteit opgeven. | Ja
storedProcedureName | Geef de naam van de opgeslagen procedure in de SQL Azure-database of Azure SQL Data Warehouse die wordt aangeduid door de gekoppelde service die in de uitvoertabel wordt gebruikt. | Ja
storedProcedureParameters | Geef de waarden voor de opgeslagen procedureparameters. Als u nodig hebt om door te geven voor een parameter null, gebruik de volgende syntaxis: "param1": null-waarden (alle kleine letters). Zie in het onderstaande voorbeeld voor meer informatie over het gebruik van deze eigenschap.| Nee

## <a name="passing-a-static-value"></a>Een statische waarde doorgegeven 
Nu kunnen we eens toevoegen van een andere kolom met de naam 'Scenario' in de tabel met een statische waarde 'Document voorbeeld' genoemd.

![Voorbeeldgegevens 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)
    
    AS
    
    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

Nu geeft u de parameter Scenario en de waarde van de opgeslagen procedure activiteit. De sectie typeProperties in het voorgaande voorbeeld ziet er als het volgende fragment uit:

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": 
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }

