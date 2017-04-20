<properties 
    pageTitle="Gegevens verplaatsen naar een Azure SQL-Database voor Azure Machine Learning | Azure" 
    description="SQL-tabel en de gegevens laden naar SQL-tabel maken" 
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
    ms.date="09/14/2016"
    ms.author="bradsev" /> 

# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Gegevens verplaatsen naar een Azure SQL-Database voor Azure Machine Learning

Dit onderwerp worden de opties voor gegevens verplaatsen van bestanden (CSV- of TSV opmaken) of van gegevens die zijn opgeslagen in een on-premises SQL Server met een Azure SQL-database. Deze taken voor het verplaatsen van gegevens in de cloud maken deel uit van het Team gegevens wetenschap proces.

Voor een onderwerp omtrek van de opties voor het verplaatsen van gegevens naar een on-premises SQL Server voor Machine Learning, raadpleegt u de [gegevens naar SQL Server op een Azure virtuele machine verplaatsen](machine-learning-data-science-move-sql-server-virtual-machine.md).

Het volgende **menu** koppelingen naar onderwerpen waarin wordt uitgelegd hoe u het nemen van gegevens in de doel-omgevingen waar de gegevens kunnen worden opgeslagen en verwerkt tijdens het Team gegevens wetenschap proces (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

De volgende tabel worden de opties voor het verplaatsen van gegevens naar een Azure SQL-Database.

<b>BRON</b> |<b>DOEL: Azure SQL-Database</b> |
-------------- |--------------------------------|
<b>Plat bestand (CSV- of TSV opgemaakt)</b> |<a href="#bulk-insert-sql-query">Bulksgewijs invoegen SQL-Query |
<b>On-premises SQL Server</b> | 1. <a href="#export-flat-file">exporteren naar een plat bestand<br> 2. <a href="#insert-tables-bcp">Wizard migratie SQL-Database<br> 3. <a href="#db-migration">database back-en herstellen<br> 4. <a href="#adf">azure gegevens Factory |


## <a name="prereqs"></a>Vereisten voor
De procedures hier vereisen dat u hebt:

* Een **Azure-abonnement**. Als u niet een abonnement hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* Een **account van Azure opslag**. U kunt een Azure opslag-account gebruiken voor het opslaan van de gegevens in deze zelfstudie. Als u niet een Azure opslag-account hebt, raadpleegt u het artikel [een opslag-account maken](storage-create-storage-account.md#create-a-storage-account) . Nadat u het opslag-account hebt gemaakt, moet u de accountsleutel gebruikt voor toegang tot de opslag verkrijgen. Zie de [weergave, kopiëren en opnieuw genereren opslag toegangstoetsen](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Toegang tot een **Azure SQL-Database**. Als u een Azure SQL-Database, moet vindt [Aan de slag met Microsoft Azure SQL Database](../sql-database/sql-database-get-started.md) u informatie over het inrichten van een nieuw exemplaar van een Azure SQL-Database.
* Geïnstalleerd en geconfigureerd **Azure PowerShell** lokaal. Zie voor instructies voor het [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

**Gegevens**: de migratie-processen worden gedemonstreerd met de [tevens Taxi gegevensset](http://chriswhong.com/open-data/foil_nyc_taxi/). De gegevensset tevens Taxi bevat informatie over reis gegevens en beurzen en beschikbaar is, zoals aangegeven dat bericht, klikt u op Azure-blobopslag: [Tevens Taxi gegevens](http://www.andresmh.com/nyctaxitrips/). Een voorbeeld en een beschrijving van deze bestanden worden gegeven in het vak [Tevens Taxi reizen gegevensset beschrijving](machine-learning-data-science-process-sql-walkthrough.md#dataset).
 
U kunt de hieronder beschreven tot een set met uw eigen gegevens procedures aanpassen of volg de stappen beschreven met behulp van de gegevensset tevens Taxi. Als u wilt de gegevensset tevens Taxi uploaden naar uw on-premises SQL Server-database, volgt u de procedure in de [Gegevens van de bulksgewijs importeren in SQL Server-Database](machine-learning-data-science-process-sql-walkthrough.md#dbload). Deze instructies zijn bedoeld voor een SQL Server op een Azure virtuele machines, maar de procedure voor het uploaden naar de on-premises SQL Server is dezelfde.


## <a name="file-to-azure-sql-database"></a>Gegevens uit de bron van een plat bestand verplaatsen naar een Azure SQL-database

Gegevens in een platte bestanden (CSV- of TSV opgemaakt) kan worden verplaatst naar een Azure SQL-database met behulp van een groot aantal invoegen SQL-Query.

### <a name="bulk-insert-sql-query"></a>Bulksgewijs invoegen SQL-Query

De stappen voor de procedure voor het gebruik van de bulksgewijs invoegen SQL-Query lijken op de beschreven in de secties voor het verplaatsen van gegevens uit de bron van een plat bestand naar SQL Server op een VM Azure. Zie [Bulksgewijs invoegen SQL-Query](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery)voor meer informatie.


##<a name="sql-on-prem-to-sazure-sql-database"></a>Gegevens uit lokale SQL Server verplaatsen naar een Azure SQL-database

Als de brongegevens in een on-premises SQL Server is opgeslagen, zijn er verschillende mogelijkheden voor het verplaatsen van de gegevens naar een Azure SQL-database:

1. [Exporteren naar een plat bestand](#export-flat-file) 
2. [Wizard migratie SQL-Database](#insert-tables-bcp)
3. [Database back-en herstellen](#db-migration)
4. [Azure gegevens Factory](#adf)

De stappen voor de eerste drie zijn vergelijkbaar met de secties in [de gegevens verplaatsen naar SQL Server op een Azure virtuele machine](machine-learning-data-science-move-sql-server-virtual-machine.md) die worden bedekt dezelfde procedure. Koppelingen naar de betreffende secties in dit onderwerp vindt u in de volgende instructies.

###<a name="export-flat-file"></a>Exporteren naar een plat bestand

De stappen voor deze exporteren naar een plat bestand lijken op de beschreven in [exporteren naar platte bestand](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file).

###<a name="insert-tables-bcp"></a>Wizard migratie SQL-Database

De stappen voor het gebruik van de Wizard SQL-Database migreren lijken op de beschreven in de [Wizard SQL Database-migratie](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration).

###<a name="db-migration"></a>Database back-en herstellen

De stappen voor het gebruik van database back-up en herstellen lijken op de beschreven in de [Database een back-up en herstellen](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup).

###<a name="adf"></a>Azure gegevens Factory

De procedure voor het verplaatsen van gegevens naar een Azure SQL-database met Azure gegevens Factory (ADF) is opgegeven in het onderwerp met [gegevens uit een on-premises SQL server naar SQL Azure wordt aangegeven met Azure gegevens Factory verplaatsen](machine-learning-data-science-move-sql-azure-adf.md). Dit onderwerp wordt uitgelegd hoe u gegevens uit een on-premises SQL Server-database verplaatsen naar een Azure SQL-database via Azure-blobopslag ADF gebruiken. 

Kunt u overwegen ADF wanneer gegevens moet worden continu gemigreerd in een scenario voor hybride dat zowel on-premises en cloud resources opent, of wanneer de gegevens is verwerkt of moet worden gewijzigd of bedrijfslogica die zijn toegevoegd aan deze wanneer het wordt gemigreerd. ADF kunt voor de planning en controle van taken met behulp van eenvoudige JSON-scripts die het verkeer van gegevens op basis van periodieke beheren. ADF, heeft ook andere mogelijkheden zoals ondersteuning voor complexe bewerkingen.




