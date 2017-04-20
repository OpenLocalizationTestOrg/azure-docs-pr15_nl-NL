<properties
    pageTitle="Scenario's voor de geavanceerde analyses proces en technologie in Azure Machine Learning | Microsoft Azure"
    description="Selecteer het juiste scenario's voor geavanceerde Bekijk analyses met het Team gegevens wetenschap proces doen."
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
    ms.author="bradsev" />


# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Scenario's voor geavanceerde analyses in Azure Machine Learning

In dit artikel worden de verschillende voorbeeldgegevensbronnen en doel-scenario's die kunnen worden verwerkt door het [Team gegevens wetenschap proces (TDSP)](data-science-process-overview.md). De TDSP biedt een systematische aanpak voor teams samenwerken aan het bouwen van intelligente toepassingen. De scenario's hier gepresenteerd illustreren opties beschikbaar in de werkstroom gegevensverwerking die afhankelijk van de gegevenskenmerken, bronlocaties en doel opslagplaatsen in Azure wordt aangegeven zijn.

De **beslissingsstructuur** voor het selecteren van de steekproef scenario's die geschikt is voor uw gegevens en doelstelling wordt gepresenteerd in de laatste sectie.

>[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Elk van de volgende secties worden in een voorbeeldscenario weergegeven. Voor elk scenario, een mogelijke gegevens wetenschappelijke of geavanceerde analyses stroom en ondersteunende Azure resources worden weergegeven.

>[AZURE.NOTE]**Voor alle van de volgende scenario's, moet u:**
><br/>
>* [Een opslag-account maken](../storage/storage-create-storage-account.md)
><br/>
>* [Een Azure Machine Learning-werkruimte maken](machine-learning-create-workspace.md)


## <a name="smalllocal"></a>Scenario \#1: klein is voor het medium in tabelvorm gegevensset in een lokale bestanden

![Kleine en middelgrote lokale bestanden][1]

#### <a name="additional-azure-resources-none"></a>Extra Azure bronnen: geen

1.  Meld u aan bij de [Azure Machine Learning Studio](https://studio.azureml.net/).

2.  Upload een gegevensset.

3.  Maken op een Azure Machine Learning experiment stroom beginnen met geüploade gegevensset (s).

## <a name="smalllocalprocess"></a>Scenario \#2: klein is voor het medium dataset van lokale bestanden waarvoor verwerking

![Kleine en middelgrote lokale bestanden met verwerking][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Extra Azure bronnen: Azure virtuele machines (IPython notitieblok server)

1.  Maak een Azure virtuele machines IPython notitieblok uitgevoerd.

2.  Gegevens uploaden naar een container Azure opslag.

3.  Vooraf verwerken en opschonen van gegevens in IPython notitieblok, toegang krijgen tot gegevens in de container Azure opslag.

4.  Transformeer gegevens naar opgeschoond, tabelweergave.

5.  Getransformeerd gegevens opslaan in Azure BLOB's.

6.  Meld u aan bij de [Azure Machine Learning Studio](https://studio.azureml.net/).

7.  De gegevens lezen van Azure BLOB's met de [Gegevens importeren] [ import-data] module.

8. Maken op een Azure Machine Learning experiment stroom beginnen met geconsumeerde gegevensset (s).

## <a name="largelocal"></a>Scenario \#3: grote gegevensset lokale bestanden bevat, hebt samengesteld Azure BLOB's

![Grote lokale bestanden][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Extra Azure bronnen: Azure virtuele machines (IPython notitieblok server)

1.  Maak een Azure virtuele machines IPython notitieblok uitgevoerd.

2.  Gegevens uploaden naar een container Azure opslag.

3.  Vooraf verwerken en opschonen van gegevens in IPython notitieblok, toegang krijgen tot gegevens in Azure BLOB's.

4.  Transformeer gegevens naar opgeschoond, tabelweergave, indien nodig.

5.  Gegevens verkennen en functies maken naar wens.

6.  Een steekproef voor kleine en middelgrote gegevens ophalen.

7.  De steekproef gegevens opslaan in Azure BLOB's.

8. Meld u aan bij de [Azure Machine Learning Studio](https://studio.azureml.net/).

9. De gegevens lezen van Azure BLOB's met de [Gegevens importeren] [ import-data] module.

10. Azure Machine Learning experiment stroom beginnen met geconsumeerde gegevensset (s) maken.


## <a name="smalllocaltodb"></a>Scenario \#4: klein is voor het medium dataset van lokale bestanden, SQL Server in een Azure Virtal Machine doelgroepen

![Kleine en middelgrote lokale bestanden naar SQL DB in Azure wordt aangegeven][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Extra Azure bronnen: Azure virtuele machines (SQL Server / IPython notitieblok server)

1.  Maak een Azure virtuele machines met SQL Server + IPython notitieblok.

2.  Gegevens uploaden naar een container Azure opslag.

3.  Vooraf verwerken en opschonen van gegevens in Azure opslag container IPython notitieblok.

4.  Transformeer gegevens naar opgeschoond, tabelweergave, indien nodig.

5.  Gegevens in VM-local bestanden opslaan (IPython notitieblok wordt uitgevoerd op VM, lokale stations verwijzen naar VM stations).

6.  Gegevens met SQL Server-database op een VM Azure uitgevoerd laden.

    Optie \#1: met SQL Server Management Studio.

    - Meld u aan bij SQL Server VM
    - SQL Server Management Studio uitvoeren.
    - Database-en doeltabellen maken.
    - Gebruik een van de bulksgewijs importeren methoden voor het laden van de gegevens van VM-local-bestanden.

    Optie \#2: gebruik IPython notitieblok – niet raadzaam voor middelgrote en grote gegevenssets<!-- -->    
    - ODBC-verbindingsreeks gebruiken voor toegang tot SQL Server op VM.
    - Database-en doeltabellen maken.
    - Gebruik een van de bulksgewijs importeren methoden voor het laden van de gegevens van VM-local-bestanden.

7.  Gegevens verkennen, zo nodig functies maken. Houd er rekening mee dat de functies niet hoeft te worden gerealiseerd in de databasetabellen. U ziet alleen de benodigde query om ze te maken.

8. Besluiten van een steekproef-grootte van het gegevens zo nodig en/of gewenst.

9. Meld u aan bij de [Azure Machine Learning Studio](https://studio.azureml.net/).

10. Lees de gegevens rechtstreeks vanuit de SQL Server met de [Gegevens importeren] [ import-data] module. Plak de benodigde query die velden haalt en voorbeelden van gegevens, indien nodig rechtstreeks in de [Gegevens importeren] wordt gemaakt van functies[ import-data] query.

11. Azure Machine Learning experiment stroom beginnen met geconsumeerde gegevensset (s) maken.

## <a name="largelocaltodb"></a>Scenario \#5: grote gegevensset in een lokale bestanden, SQL Server in Azure VM afstemmen

![Grote lokale bestanden naar SQL DB in Azure wordt aangegeven][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Extra Azure bronnen: Azure virtuele machines (SQL Server / IPython notitieblok server)

1.  Maak een Azure virtuele machines met SQL Server en IPython notitieblok server.

2.  Gegevens uploaden naar een container Azure opslag.

3.  (Optioneel) Vooraf verwerken en opschonen van gegevens.

    een.  Vooraf verwerken en opschonen van gegevens in IPython notitieblok, toegang krijgen tot gegevens in Azure BLOB's.

    b.  Transformeer gegevens naar opgeschoond, tabelweergave, indien nodig.

    c.  Gegevens in VM-local bestanden opslaan (IPython notitieblok wordt uitgevoerd op VM, lokale stations verwijzen naar VM stations).

4.  Gegevens met SQL Server-database op een VM Azure uitgevoerd laden.

    een.  Meld u aan bij van SQL Server VM.

    b.  Als gegevens worden niet al opgeslagen, kunt u gegevensbestanden downloaden van Azure opslag container naar de map local-VM.

    c.  SQL Server Management Studio uitvoeren.

    d.  Database-en doeltabellen maken.

    e.  Gebruik een van de bulksgewijs importeren methoden om de gegevens te laden.

    f.  Als tabel joins vereist zijn, maakt u indexen sneller joins.

     > [AZURE.NOTE] Voor sneller laden van grote gegevens formaten, wordt het aanbevolen dat u gepartitioneerde tabellen maken en de gegevens parallel voor bulksgewijs importeren. Zie [Parallelle gegevens importeren naar SQL partitioneren tabellen](machine-learning-data-science-parallel-load-sql-partitioned-tables.md)voor meer informatie.

5.  Gegevens verkennen, zo nodig functies maken. Houd er rekening mee dat de functies niet hoeft te worden gerealiseerd in de databasetabellen. U ziet alleen de benodigde query om ze te maken.

6.  Besluiten van een steekproef-grootte van het gegevens zo nodig en/of gewenst.

7.  Meld u aan bij de [Azure Machine Learning Studio](https://studio.azureml.net/).

8. Lees de gegevens rechtstreeks vanuit de SQL Server met de [Gegevens importeren] [ import-data] module. Plak de benodigde query die velden haalt en voorbeelden van gegevens, indien nodig rechtstreeks in de [Gegevens importeren] wordt gemaakt van functies[ import-data] query.

9. Eenvoudige Azure Machine Learning experiment stroom beginnen met geüploade gegevensset

## <a name="largedbtodb"></a>Scenario \#6: grote gegevensset in een SQL Server-database on-premises, SQL Server in een Azure Virtual Machine doelgroepen

![Grote SQL DB on-premises naar SQL DB in Azure wordt aangegeven][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Extra Azure bronnen: Azure virtuele machines (SQL Server / IPython notitieblok server)

1.  Maak een Azure virtuele machines met SQL Server en IPython notitieblok server.

2.  Gebruik een van de gegevens exporteren methoden voor het exporteren van de gegevens uit SQL Server naar bestanden.

    > [AZURE.NOTE] Als u besluit om te zorgen dat alle gegevens verplaatsen uit de on-premises-database, een alternatieve (sneller) methode de volledige database verplaatsen naar de SQL Server-instantie in Azure wordt aangegeven. De stappen voor het exporteren van gegevens, database, maken en gegevens naar de doeldatabase laden/importeren en volgt u de alternatieve methode overslaan.

3.  Upload bestanden naar Azure opslag container.

4.  De gegevens naar een SQL Server-database op een Azure virtuele machines te laden.

    een.  Meld u met de SQL Server VM.

    b.  Gegevensbestanden naar de map local-VM downloaden vanuit een container Azure opslag.

    c.  SQL Server Management Studio uitvoeren.

    d.  Database-en doeltabellen maken.

    e.  Gebruik een van de bulksgewijs importeren methoden om de gegevens te laden.

    f.  Als tabel joins vereist zijn, maakt u indexen sneller joins.

    > [AZURE.NOTE] Voor sneller laden van grote gegevens formaten, gepartitioneerde tabellen maken en naar bulksgewijs kunt u de gegevens parallel importeren. Zie [Parallelle gegevens importeren naar SQL partitioneren tabellen](machine-learning-data-science-parallel-load-sql-partitioned-tables.md)voor meer informatie.

5.  Gegevens verkennen, zo nodig functies maken. Houd er rekening mee dat de functies niet hoeft te worden gerealiseerd in de databasetabellen. U ziet alleen de benodigde query om ze te maken.

6.  Besluiten van een steekproef-grootte van het gegevens zo nodig en/of gewenst.

7.  Meld u aan bij de [Azure Machine Learning Studio](https://studio.azureml.net/).

8. Lees de gegevens rechtstreeks vanuit de SQL Server met de [Gegevens importeren] [ import-data] module. Plak de benodigde query die velden haalt en voorbeelden van gegevens, indien nodig rechtstreeks in de [Gegevens importeren] wordt gemaakt van functies[ import-data] query.

9. Eenvoudige Azure Machine Learning experimenteren stroom beginnen met geüploade gegevensset.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Alternatieve methode kopiëren van een volledige database van een lokale SQL Server naar Azure SQL-Database

![Lokale DB loskoppelen en als bijlage toevoegen aan de SQL-DB in Azure wordt aangegeven][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Extra Azure bronnen: Azure virtuele machines (SQL Server / IPython notitieblok server)

Als wilt repliceren de hele SQL Server-database in uw SQL Server-VM, u moet een database kopiëren van de ene locatie/server naar een andere, ervan uitgaande dat de database kan worden gehouden die tijdelijk offline. Dit doet u in de SQL Server Management Studio Object Explorer of de overeenkomstige Transact-SQL-opdrachten gebruiken.

1. Ontkoppel de database op de bronlocatie. Zie [ontkoppelen van een database](https://technet.microsoft.com/library/ms191491(v=sql.110).aspx)voor meer informatie.
2. Kopieer het losgekoppelde databasebestand of de bestanden en het logboekbestand of de bestanden naar de doellocatie op de SQL Server-VM in Azure wordt aangegeven in Windows Verkenner of opdrachtprompt van Windows-venster.
3. De gekopieerde bestanden toevoegen aan de SQL Server-instantie van het doel. Zie [bijvoegen een Database](https://technet.microsoft.com/library/ms190209(v=sql.110).aspx)voor meer informatie.

[Een Database met loskoppelen en bijvoegen (Transact-SQL) verplaatsen](https://technet.microsoft.com/library/ms187858(v=sql.110).aspx)

## <a name="largedbtohive"></a>Scenario \#7: Big data in lokale bestanden, component database in Azure HDInsight Hadoop clusters afstemmen

![Grote gegevens in de lokale doel component][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Extra Azure bronnen: Azure HDInsight Hadoop Cluster en Azure virtuele machines (IPython notitieblok server)

1.  Maak een Azure virtuele machines IPython notitieblok server uitgevoerd.

2.  Maak een cluster Azure HDInsight Hadoop.

3.  (Optioneel) Vooraf verwerken en opschonen van gegevens.

    een.  Vooraf verwerken en opschonen van gegevens in IPython notitieblok, toegang krijgen tot gegevens in Azure BLOB's.

    b.  Transformeer gegevens naar opgeschoond, tabelweergave, indien nodig.

    c.  Gegevens in VM-local bestanden opslaan (IPython notitieblok wordt uitgevoerd op VM, lokale stations verwijzen naar VM stations).

4.  Gegevens uploaden naar de standaard-container van het Hadoop-cluster in de stap 2 hebt geselecteerd.

5.  Gegevens met component-database in Azure HDInsight Hadoop cluster laden.

    een.  Meld u aan bij het hoofd knooppunt van de cluster Hadoop

    b.  Open de opdrachtregel Hadoop.

    c.  Voer de hoofdmap van de component door de opdracht `cd %hive_home%\bin` in Hadoop opdrachtregel.

    d.  De component query's uitvoeren om database en tabellen maken en gegevens laden vanuit blobopslag naar component tabellen.

    > [AZURE.NOTE] Als de gegevens groot is, kunnen gebruikers de component tabel kunnen maken met partities. Klik, kunnen gebruikers gebruiken een `for` lus in de Hadoop opdrachtregel op het hoofd knooppunt gegevens in de component tabel partitioneren door partition laden.

6.  Gegevens verkennen en functies zo nodig in Hadoop opdrachtregel maken. Houd er rekening mee dat de functies niet hoeft te worden gerealiseerd in de databasetabellen. U ziet alleen de benodigde query om ze te maken.

    een.  Meld u aan bij het hoofd knooppunt van de cluster Hadoop

    b.  Open de opdrachtregel Hadoop.

    c.  Voer de hoofdmap van de component door de opdracht `cd %hive_home%\bin` in Hadoop opdrachtregel.

    d.  De component query's in de Hadoop-opdrachtregel op de kop knooppunt van de cluster Hadoop om de gegevens verkennen en functies te maken naar wens uitvoeren.

7.  Als nodig en/of gewenst, voorbeeld van de gegevens zodat deze past in Azure Machine Learning Studio.

8.  Meld u aan bij de [Azure Machine Learning Studio](https://studio.azureml.net/).

9. Lees de gegevens rechtstreeks vanuit de `Hive Queries` met de [Gegevens importeren] [ import-data] module. Plak de benodigde query die velden haalt en voorbeelden van gegevens, indien nodig rechtstreeks in de [Gegevens importeren] wordt gemaakt van functies[ import-data] query.

10. Eenvoudige Azure Machine Learning experimenteren stroom beginnen met geüploade gegevensset.

## <a name="decisiontree"></a>Beslissingsstructuur voor scenario selectie
------------------------

In het volgende diagram ziet u de scenario's hierboven beschreven en het proces van geavanceerde analyses en technologie keuzen die zijn gemaakt die verwijzen naar de gespecificeerde scenario's. Gegevensverwerking, te verkennen functie engineering en steekproeven duurt plaatsen in een of meer methode/omgeving--bij de bron, gevorderde, en/of target omgevingen – en iteratief naar wens kunt doorgaan. Het diagram als een afbeelding van een aantal mogelijke loopt fungeert alleen en biedt een uitgebreide opsomming geen.

![Voorbeeldscenario's DS proces Stapsgewijze instructies][8]

### <a name="advanced-analytics-in-action-examples"></a>Geavanceerde analyses in actie voorbeelden

Voor end-to-end Azure Machine Learning-scenario's die gebruikmaken van het proces van geavanceerde analyses en technologie met openbare gegevenssets raadpleegt u:


* [Team gegevens wetenschap proces in actie: met SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
* [Team gegevens wetenschap proces in actie: HDInsight Hadoop clusters met](machine-learning-data-science-process-hive-walkthrough.md).


[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
