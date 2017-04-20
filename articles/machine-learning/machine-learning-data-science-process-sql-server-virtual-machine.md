<properties 
    pageTitle="Gegevens vanuit SQL Azure verwerken | Microsoft Azure" 
    description="Procesgegevens uit SQL Azure wordt aangegeven" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Procesgegevens in SQL Server-VM op Azure

In dit document wordt uitgelegd hoe u gegevens verkennen en functies voor gegevens die zijn opgeslagen in een SQL Server-VM op Azure genereren. Dit kunt doen door de gegevens worsteling met SQL of door een programmeertaal zoals Python.


> [AZURE.NOTE] De SQL-instructies voor de steekproef in dit document wordt ervan uitgegaan dat de gegevens zich in SQL Server. Als dit niet wordt weergegeven, raadpleegt u het procesoverzicht in cloud wetenschappelijke wilt weten hoe u uw gegevens verplaatsen naar SQL Server.

##<a name="SQL"></a>Met behulp van SQL

De volgende gegevens wrangling taken in deze sectie met SQL worden beschreven:

1. [Gegevens verkennen](#sql-dataexploration)
2. [Functie generatie](#sql-featuregen)

###<a name="sql-dataexploration"></a>Gegevens verkennen
Hier volgen een paar SQL-voorbeeldscripts die kunnen worden gebruikt voor het verkennen van gegevens winkels in SQL Server.


> [AZURE.NOTE] Voor een voorbeeld, kunt u de [tevens Taxi gegevensset](http://www.andresmh.com/nyctaxitrips/) gebruikt en verwijzen naar de IPNB getiteld [tevens gegevens worsteling IPython notitieblok en SQL Server gebruiken](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) voor een end-to-end-overzicht.

1. Het aantal waarnemingen per dag tellen

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Niveaus in een kolom bestaan ophalen

    `select  distinct <column_name> from <databasename>`

3. Het aantal niveaus in combinatie van twee bestaan kolommen ophalen 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Ophalen van de verdeling voor numerieke kolommen

    `select <column_name>, count(*) from <tablename> group by <column_name>`


###<a name="sql-featuregen"></a>Functie generatie

In deze sectie worden beschreven manieren voor het genereren van functies met SQL:  

1. [Tellen op basis van functie generatie](#sql-countfeature)
2. [Binning functie generatie](#sql-binningfeature)
3. [Implementeren van de functies van één kolom](#sql-featurerollout)


> [AZURE.NOTE] Als u aanvullende functies genereren, kunt u deze als kolommen toevoegen aan de bestaande tabel of een nieuwe tabel maken met de aanvullende functies en de primaire sleutel, die kan worden verbonden met de oorspronkelijke tabel. 

###<a name="sql-countfeature"></a>Tellen op basis van functie generatie

In dit document ziet u twee manieren om een aantal functies genereren. De eerste methode gebruikt Voorwaardelijke som en de tweede methode gebruikt de component 'waar'. Deze kunnen vervolgens worden verbonden met de oorspronkelijke tabel (met behulp van de primaire sleutel kolommen) als u wilt dat de functies aantal samen met de oorspronkelijke gegevens.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

###<a name="sql-binningfeature"></a>Binning functie generatie

Het volgende voorbeeld ziet u hoe genereren van binned functies met binning (5 opslaglocaties) een numerieke kolom die kan worden gebruikt als een functie in plaats daarvan:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


###<a name="sql-featurerollout"></a>Implementeren van de functies van één kolom

In dit gedeelte gedemonstreerd hoe u album vanuit één kolom in een tabel te genereren aanvullende functies. Het voorbeeld wordt ervan uitgegaan dat er een kolom breedtegraad of lengtegraad in de tabel waaruit u probeert te genereren van functies.

Hier volgt een kort handleiding op locatiegegevens breedtegraad/lengtegraad (resources voorzien van stackoverflow [hoe de nauwkeurigheid van breedte- en lengtegegevens meten?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). Dit is handig om te begrijpen voordat featurizing het locatieveld:

- Het teken vertellen of we noorden of Zuid, Oost of west op de wereldbol.
- Een andere waarde dan nul honderden cijfers vertellen gebruiken we lengtegegevens, niet breedtegraad!
- De tientallen cijfers biedt een positie tot het ongeveer 1000 kilometer. Daardoor nuttige informatie over welke continent of Oceaan we zijn op.
- Het cijfer eenheden (één decimaal graad) biedt een positie maximaal 111 kilometer (60 zeemijl, ongeveer 69 mijl). Dit kunt ons ongeveer zien welke grote provincie of land die we zijn in.
- De eerste decimaal is zegt maximaal 11.1 km: deze de positie van één grote stad van een aangrenzende grote stad kunt onderscheiden.
- De tweede decimaal is zegt maximaal 1.1 km: deze één village kunt scheiden van het volgende.
- De derde decimaal is zegt tot 110 m dat deze kan vaststellen om wat een grote de veld of institutionele campus.
- De vierde decimaal is zegt maximaal 11 m dat kunnen vaststellen om wat een pakket van land. Het is vergelijkbaar met de normale nauwkeurigheid van een niet-gecorrigeerde GPS eenheid zonder storing.
- Is de vijfde decimaal zegt maximaal 1.1 m het bomen van elkaar onderscheiden. Nauwkeurigheid tot dit niveau met commerciële GPS eenheden kan alleen worden bereikt met differentiële correctie.
- De zesde decimaal is zegt maximaal 0.11 m dat u kunt dit gebruiken voor het opmaken van structuren in detail voor het ontwerpen van landschap, wegen samenstellen. Moet meer dan goed genoeg voor de bewegingen van glaciers en rivieren. Dit kan worden bereikt door het hele maatregelen met GPS, zoals differentially gecorrigeerde GPS.

De locatiegegevens featurized als volgt kan worden, scheiden regio, locatie en plaats. Houd er rekening mee dat u ook een REST-eindpunt zoals Bing Maps API beschikbaar op [een locatie met punt zoeken bellen kunt](https://msdn.microsoft.com/library/ff701710.aspx) om de gegevens voor regio/gebied.

    select 
        <location_columnname>
        ,round(<location_columnname>,0) as l1       
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

De bovenstaande locatie gebaseerde functies kunnen verder worden gebruikt voor het genereren van extra aantal functies zoals eerder is beschreven. 


> [AZURE.TIP] U kunt de records met behulp van de taal van keuze via programmacode invoegen. Mogelijk moet u de gegevens invoegen in stukken schrijven efficiëntie (voor een voorbeeld van hoe u dit doen met pyodbc, raadpleegt u [A Hallo wereld voorbeeld voor toegang tot SQL Server met python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)) te verbeteren. Een ander alternatief is om in te voegen van gegevens in de database met het [hulpprogramma BCP](https://msdn.microsoft.com/library/ms162802.aspx).

###<a name="sql-aml"></a>Verbinding maken met Azure Machine Learning

De nieuwe functie kan worden toegevoegd als een kolom aan een bestaande tabel of die zijn opgeslagen in een nieuwe tabel en samengevoegd met de oorspronkelijke tabel voor machine learning. Functies kunnen worden gegenereerd of geopend als u al hebt gemaakt, met de [Gegevens importeren] [ import-data] module in Azure Machine Learning zoals hieronder wordt weergegeven:

![azureml lezers][1] 

##<a name="python"></a>Gebruik een programmeertaal zoals Python

Gebruik van Python voor gegevens verkennen en functies genereren wanneer de gegevens in SQL Server is vergelijkbaar met het verwerken van gegevens in Azure blob Python gebruiken zoals beschreven in [proces Azure Blob-gegevens in u gegevens wetenschappelijke omgeving](machine-learning-data-science-process-data-blob.md). De gegevens moet uit de database in een frame pandas-gegevens worden geladen en klikt u vervolgens verder kunnen worden verwerkt. We het proces van het verbinden met de database en de gegevens laden in het kader van de gegevens in deze sectie van het document.

Indeling van de volgende verbindingsreeks kan worden gebruikt om verbinding maken met een SQL Server-database van Python met pyodbc (vervangen servernaam, dbname, gebruikersnaam en wachtwoord met uw specifieke waarden):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

De [bibliotheek Pandas](http://pandas.pydata.org/) in Python biedt een uitgebreide set gegevensstructuren en hulpprogramma's voor gegevensanalyse voor gegevens manipuleren Python hoeft te programmeren. De onderstaande code leest de resultaten geretourneerd uit een SQL Server-database in een frame van de gegevens Pandas:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nu kunt u werken met het kader van de gegevens Pandas zoals beschreven in het artikel [proces Azure Blob-gegevens in u gegevens wetenschappelijke omgeving](machine-learning-data-science-process-data-blob.md).

## <a name="azure-data-science-in-action-example"></a>Azure gegevens wetenschappelijke in actie voorbeeld

Zie voor een end-to-end-scenario-voorbeeld van de Azure gegevens wetenschap proces met een openbare gegevensset, [Azure gegevens wetenschap proces in actie](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
