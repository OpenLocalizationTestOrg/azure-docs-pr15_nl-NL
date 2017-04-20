<properties 
    pageTitle="Kennismaken met gegevens in SQL Server Virtual Machine op Azure | Microsoft Azure" 
    description="Hoe u de gegevens die zijn opgeslagen in een SQL Server-VM op Azure verkennen." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Gegevens in SQL Server Virtual Machine op Azure verkennen


In dit document wordt uitgelegd hoe u gegevens die zijn opgeslagen in een SQL Server-VM op Azure verkennen. Dit kunt doen door de gegevens worsteling met SQL of door een programmeertaal zoals Python.

Het volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u met hulpmiddelen voor gegevens verkennen van verschillende opslagomgevingen. Deze taak is een stap in de Cortana Analytics proces (Initiaal).

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


> [AZURE.NOTE] De SQL-instructies voor de steekproef in dit document wordt ervan uitgegaan dat de gegevens zich in SQL Server. Als dit niet wordt weergegeven, raadpleegt u het procesoverzicht in cloud wetenschappelijk wilt weten hoe u uw gegevens verplaatsen naar SQL Server.



## <a name="sql-dataexploration"></a>SQL-gegevens met SQL-scripts verkennen

Hier volgen een paar SQL-voorbeeldscripts die kunnen worden gebruikt voor het verkennen van gegevens winkels in SQL Server.

1. Het aantal waarnemingen per dag tellen

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Niveaus in een kolom bestaan ophalen

    `select  distinct <column_name> from <databasename>`

3. Het aantal niveaus in combinatie van twee bestaan kolommen ophalen 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Ophalen van de verdeling voor numerieke kolommen

    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [AZURE.NOTE] Voor een voorbeeld, kunt u de [tevens Taxi gegevensset](http://www.andresmh.com/nyctaxitrips/) gebruikt en verwijzen naar de IPNB getiteld [tevens gegevens worsteling IPython notitieblok en SQL Server gebruiken](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) voor een end-to-end-overzicht.

##<a name="python"></a>SQL-gegevens met Python verkennen

Gebruik van Python voor gegevens verkennen en functies genereren wanneer de gegevens in SQL Server is vergelijkbaar met het verwerken van gegevens in Azure blob met Python, zoals beschreven in [proces Azure Blob-gegevens in uw gegevens wetenschappelijke-omgeving](machine-learning-data-science-process-data-blob.md). De gegevens moet in een pandas DataFrame uit de database worden geladen en klikt u vervolgens verder kunnen worden verwerkt. We het proces van het verbinden met de database en het laden van de gegevens in de DataFrame in deze sectie van het document.

Indeling van de volgende verbindingsreeks kan worden gebruikt om verbinding maken met een SQL Server-database van Python met pyodbc (vervangen servernaam, dbname, gebruikersnaam en wachtwoord met uw specifieke waarden):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

De [bibliotheek Pandas](http://pandas.pydata.org/) in Python biedt een uitgebreide set gegevensstructuren en hulpprogramma's voor gegevensanalyse voor gegevens manipuleren Python hoeft te programmeren. De volgende code leest de resultaten geretourneerd uit een SQL Server-database in een frame van de gegevens Pandas:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nu kunt u werken met de DataFrame Pandas zoals besproken in het onderwerp [proces Azure Blob-gegevens in uw gegevens wetenschappelijke-omgeving](machine-learning-data-science-process-data-blob.md).

## <a name="cortana-analytics-process-in-action-example"></a>Cortana Analytics-proces in actie voorbeeld

Zie voor een end-to-end-scenario-voorbeeld van het Cortana Analytics-proces met een openbare gegevensset, [het Team gegevens wetenschap proces in actie: met SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

 
