<properties 
    pageTitle="Voorbeeldgegevens in SQL Server Azure | Microsoft Azure" 
    description="Voorbeeldgegevens in SQL Server Azure" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Voorbeeldgegevens in SQL Server Azure


In dit document ziet hoe u het voorbeeld van de gegevens die zijn opgeslagen in SQL Server Azure SQL of de programmeertaal Python gebruikt. Ook ziet u hoe steekproef om gegevens te verplaatsen naar Azure Machine Learning door deze naar een bestand op te slaan, uploaden naar een Azure blob en deze vervolgens naar Azure Machine Learning Studio te lezen.

De steekproeven Python maakt de [pyodbc](https://code.google.com/p/pyodbc/) ODBC-bibliotheek verbinding met SQL Server Azure en de bibliotheek [Pandas](http://pandas.pydata.org/) moet de steekproeven.

>[AZURE.NOTE] De SQL-code van de steekproef in dit document wordt ervan uitgegaan dat de gegevens zich in een SQL Server op Azure. Als dit niet het geval is, raadpleegt u [gegevens naar SQL Server op Azure verplaatsen](machine-learning-data-science-move-sql-server-virtual-machine.md) onderwerp voor instructies over het verplaatsen van uw gegevens naar SQL Server op Azure.

**Waarom voorbeeld van uw gegevens?**
Als de gegevensset die u wilt analyseren groot is, is het meestal een goed idee om de gegevens om deze in een kleinere maar vertegenwoordiger en beter beheerbare grootte omlaag gepaarde steekproeven. Dit vergemakkelijkt gegevens lidmaatschap, te verkennen en functie engineering. De rol in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) is het inschakelen van snel prototypen van de functies voor gegevensverwerking en machine learning-modellen.

Het **menu** onder koppelingen naar onderwerpen waarin wordt uitgelegd hoe u een voorbeeld van gegevens uit verschillende opslagomgevingen. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Deze taak steekproeven is een stap in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

##<a name="SQL"></a>Met behulp van SQL

Deze sectie worden verschillende methoden voor het uitvoeren van eenvoudige steekproeven ten opzichte van de gegevens in de database met behulp van SQL. Kies een methode op basis van de gegevensgrootte van uw en de verdeling.

De volgende twee items laten zien hoe newid in SQL Server gebruikt de steekproeven uitvoeren. De methode die u kiest, is afhankelijk van hoe willekeurige de steekproef gewenste worden (pk_id in het onderstaande voorbeeldcode wordt aangenomen dat een automatisch gegenereerde primaire sleutel).

1. Minder strikte steekproef

        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())

2. Meer steekproef 

        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

TABLESAMPLE kan worden gebruikt voor steekproeven, evenals gedemonstreerd hieronder. Dit kan een betere benadering zijn als de gegevensgrootte van uw groot is (ervan uitgaande dat gegevens op verschillende pagina's niet gerelateerd is) en voor de query uit te voeren in een redelijk tijd.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

>[AZURE.NOTE] U kunt verkennen en functies van deze steekproef gegevens genereren door op te slaan in een nieuwe tabel


###<a name="sql-aml"></a>Verbinding maken met Azure Machine Learning

U kunt rechtstreeks de voorbeeldquery's boven in de Azure Machine Learning- [Gegevens importeren] [ import-data] module omlaag gepaarde steekproeven de gegevens in de browser en in een experiment Azure Machine Learning werking doen. Hieronder vindt u een schermafbeelding van het gebruik van de lezer-module voor de steekproef gegevens lezen:
   
![lezer sql][1]

##<a name="python"></a>Gebruik van de programmeertaal Python 

Deze sectie wordt beschreven met de [bibliotheek pyodbc](https://code.google.com/p/pyodbc/) tot stand brengen van een ODBC verbinden met een SQL server-database in Python. De verbindingsreeks van de database is als volgt: (servernaam, dbname, gebruikersnaam en wachtwoord vervangen door uw configuratie):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

De bibliotheek [Pandas](http://pandas.pydata.org/) in Python biedt een uitgebreide set gegevensstructuren en hulpprogramma's voor gegevensanalyse voor gegevens manipuleren Python hoeft te programmeren. De onderstaande code wordt een voorbeeld 0,1% van de gegevens uit een tabel in SQL Azure-database op een Pandas gegevens:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Nu kunt u werken met de steekproef gegevens in het kader van de gegevens Pandas. 

###<a name="python-aml"></a>Verbinding maken met Azure Machine Learning

U kunt de volgende code gedownsampled gegevens naar een bestand opslaan en uploadt dit naar een Azure blob. De gegevens in de blob kunnen worden rechtstreeks gelezen in een Experiment Azure Machine Learning met de [Gegevens importeren] [ import-data] module. De stappen zijn als volgt: 

1. Het kader van de gegevens pandas schrijven naar een lokaal bestand

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Lokale bestand uploaden naar Azure blob

        from azure.storage import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Gegevens lezen van Azure blob met Azure Machine Learning- [Gegevens importeren] op[ import-data] module zoals wordt weergegeven in het scherm schermafbeelding hieronder:
 
![lezer blob][2]

## <a name="the-team-data-science-process-in-action-example"></a>Het proces van het Team gegevens wetenschappelijke in actie voorbeeld

Zie voor een end-to-end scenario voorbeeld van het Team gegevens wetenschap proces een met een openbare gegevensset [Team gegevens wetenschap proces in actie: met SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

 [import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
