<properties 
   pageTitle="Gegevens kopiëren naar andere Lake gegevensopslag en Azure SQL-database met Sqoop | Microsoft Azure"
   description="Sqoop gebruiken om gegevens tussen Azure SQL-Database en Lake gegevensopslag te kopiëren" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Gegevens kopiëren naar andere Lake gegevensopslag en Azure SQL-database met Sqoop

Informatie over het gebruik van Apache Sqoop importeren en exporteren van gegevens tussen Azure SQL-Database en Lake gegevensopslag.
 

## <a name="what-is-sqoop"></a>Wat is Sqoop?

Grote gegevenstoepassingen zijn natuurlijke kiezen voor het verwerken van niet-gestructureerde en gedeeltelijk gestructureerde gegevens, zoals Logboeken en bestanden. Er kan ook wel een nodig om gestructureerde gegevens die zijn opgeslagen in relationele databases te verwerken.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) is een hulpprogramma waarmee gegevens uitwisselen tussen relationele databases en een gegevensopslagplaats groot, zoals Lake gegevensopslag. U kunt deze gegevens uit een relationele database management-systeem (RDBMS) zoals Azure SQL-Database importeren in Lake gegevensopslag. U kunt vervolgens transformeren en de gegevens met werkbelasting groot gegevens analyseren en exporteer de gegevens weer in een RDBMS. In deze zelfstudie kunt u een Azure SQL-Database als de relationele database importeren/exporteren uit.
 

## <a name="prerequisites"></a>Vereisten voor

Voordat u in dit artikel begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
- **Uw abonnement op Azure inschakelen** voor gegevensopslag Lake openbare preview. Zie de [instructies](data-lake-store-get-started-portal.md#signup). 
- **Azure HDInsight cluster** met toegang tot een Lake gegevensopslag-account. Zie [een HDInsight cluster met Lake gegevensopslag maken](data-lake-store-hdinsight-hadoop-use-portal.md). In dit artikel wordt ervan uitgegaan dat u een HDInsight Linux cluster met Lake gegevensopslag toegang hebt.
- **Azure SQL-Database**. Zie voor instructies over hoe u een account maakt, [een Azure SQL-database maken](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Vind u meer snel met video's?

[Bekijk deze video](https://mix.office.com/watch/1butcdjxmu114) over het kopiëren van gegevens tussen Azure opslag BLOB's en gegevensopslag Lake DistCp gebruiken.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Tabellen maken in de Azure SQL-Database

1. Beginnen met, twee tabellen maken in de SQL Azure-Database. Gebruik [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) of Visual Studio verbinding maken met de Azure SQL-Database en voer de volgende query's.

    **Tabel1 maken**

        CREATE TABLE [dbo].[Table1]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

    **Tabel2 maken**

        CREATE TABLE [dbo].[Table2]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

2. **Tabel1**, voeg enkele voorbeeldgegevens. Laat **tabel2** leeg. We worden gegevens importeren uit **Table1** in Lake gegevensopslag. We wordt vervolgens gegevens exporteren uit Lake gegevensopslag naar **tabel2**. Voer de volgende fragment.

         
        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson'); 
  

## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a>Sqoop uit een HDInsight cluster met access kunt Lake gegevensopslag

Een cluster HDInsight al de Sqoop-pakketten beschikbaar. Als u het cluster HDInsight Lake gegevensopslag gebruiken als een extra opslagruimte hebt geconfigureerd, kunt u Sqoop (zonder configuratiewijzigingen aanbrengt) gebruiken om te importeren/exporteren van gegevens tussen een relationele database (in dit voorbeeld Azure SQL-Database) en een account voor gegevensopslag Lake. 

1. Voor deze zelfstudie we wordt ervan uitgegaan dat u een cluster Linux hebt gemaakt, zodat u SSH verbinding maken met de cluster moet gebruiken. Zie [verbinding maken met een cluster Linux gebaseerde HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).

2. Controleer of u kunt toegang heeft tot het account voor gegevensopslag Lake uit het cluster. Voer de volgende opdracht uit de prompt SSH:

        
        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    Dit moet een lijst met bestanden/mappen in het account voor gegevensopslag Lake bieden.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Gegevens van Azure SQL-Database importeren in Lake gegevensopslag

3. Navigeer naar de map waarin Sqoop-pakketten beschikbaar zijn. Meestal dit is bij `/usr/hdp/<version>/sqoop/bin`. 

4. De gegevens importeren uit **Table1** in het account voor gegevensopslag Lake. Gebruik de volgende syntaxis:

        
        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Houd er rekening mee dat aanduiding van de **sql-database-server-name** de naam van de server waarop de SQL Azure-database wordt uitgevoerd. **SQL-database-name** tijdelijke aanduiding de werkelijke databasenaam.

    Bijvoorbeeld:

        
        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

5. Controleer of dat de gegevens is overgedragen naar het account voor gegevensopslag Lake. Voer de volgende opdracht:

        
        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Hier ziet u de volgende uitvoer.

        
        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Elke **deel-m -** * bestand komt overeen met een rij in de brontabel * *tabel1**. U kunt de inhoud van het webonderdeel formulier weergeven-m -* bestanden om te bevestigen.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Gegevens exporteren uit Lake gegevensopslag naar Azure SQL-Database

6. Exporteer de gegevens van Lake gegevensopslag account naar de lege tabel, **tabel2**, in de Azure SQL-Database. Gebruik de volgende syntaxis.

        
        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Bijvoorbeeld:

        
        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

6. Controleer of dat de gegevens is geüpload naar de tabel SQL-Database. Gebruik [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) of Visual Studio verbinding maken met de Azure SQL-Database en de volgende query vervolgens uitvoert.

        
        SELECT * FROM TABLE2

    Dit heeft de volgende uitvoer.

        ID  FName   LName
        ------------------
        1   Neal    Kell
        2   Lila    Fulton
        3   Erna    Myers
        4   Annette Simpson

## <a name="see-also"></a>Zie ook

- [Gegevens van Azure opslag BLOB's kopiëren naar Lake gegevensopslag](data-lake-store-copy-data-azure-storage-blob.md)
- [Beveiliging van gegevens in Lake gegevensopslag](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensopslag](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensopslag](data-lake-store-hdinsight-hadoop-use-portal.md)
