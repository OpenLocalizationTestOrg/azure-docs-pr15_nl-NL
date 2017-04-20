<properties
    pageTitle="Hadoop Sqoop gebruiken in Linux gebaseerde HDInsight | Microsoft Azure"
    description="Informatie over het uitvoeren van Sqoop importeren en exporteren tussen een Hadoop Linux gebaseerde op HDInsight cluster en een Azure SQL-database."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight-ssh"></a>Sqoop gebruiken met Hadoop in HDInsight (SSH)

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het gebruik van Sqoop importeren en exporteren tussen een HDInsight Linux gebaseerde cluster en Azure SQL-Database of SQL Server-database.

> [AZURE.NOTE] De stappen in dit artikel met SSH verbinding maken met een cluster Linux gebaseerde HDInsight. Windows-clients kunnen ook Azure PowerShell en .NET-SDK HDInsight gebruiken om te werken met Sqoop op Linux gebaseerde clusters. Gebruik de tabkiezer te openen die artikelen.

##<a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **A Hadoop cluster in HDInsight** en een __Azure SQL-Database__: de stappen in dit document zijn gebaseerd op de cluster en de database gemaakt met behulp van het document [cluster maken en SQL-database](hdinsight-use-sqoop.md#create-cluster-and-sql-database) . Als u al een HDInsight cluster en SQL-Database, kunt u referenties voor de waarden die worden gebruikt in dit document vervangen.
- **Workstation**: een computer met een SSH-client.

##<a name="install-freetds"></a>FreeTDS installeren

1. Met SSH verbinding maken met het cluster Linux gebaseerde HDInsight. Het adres wilt gebruiken wanneer u verbinding maakt is `CLUSTERNAME-ssh.azurehdinsight.net` en de poort is `22`.

    Zie de volgende documenten voor meer informatie over het gebruik van SSH verbinding maken met HDInsight:

    * **Linux, Unix of OS X-clients**: Zie [verbinding maken met een HDInsight Linux gebaseerde cluster met Linux, OS X of Unix](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)

    * **Windows-clients**: Zie [verbinding maken met een HDInsight Linux gebaseerde cluster met Windows](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)

3. Gebruik de volgende opdracht uit om te FreeTDS installeren:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

    FreeTDS wordt gebruikt in verschillende stappen verbinding maken met SQL-Database.

##<a name="create-the-table-in-sql-database"></a>De tabel in SQL-Database maken

> [AZURE.IMPORTANT] Als u een HDInsight cluster en SQL-Database gemaakt met behulp van de stappen in [cluster maken en SQL-database](hdinsight-use-sqoop.md)gebruikt, negeert u de stappen in deze sectie als de database en tabel zijn gemaakt als onderdeel van de stappen in dit document.

1. Gebruik van de SSH-verbinding met HDInsight, de volgende opdracht verbinding maken met de SQL-databaseserver en het maken van de tabel die wordt gebruikt in de rest van de volgende stappen uit:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    U ontvangt uitvoer ongeveer als volgt uit:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Aan de `1>` wordt gevraagd, voert u de volgende regels:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO

    Wanneer de `GO` instructie wordt ingevoerd, de vorige beweringen wordt geëvalueerd. Eerst moet de tabel **mobiledata** wordt gemaakt, vervolgens een gegroepeerde index wordt toegevoegd aan deze (vereist voor SQL-Database).

    Gebruik de volgende handelingen uit om te bevestigen dat de tabel is gemaakt:

        SELECT * FROM information_schema.tables
        GO

    Hier ziet u uitvoer ongeveer als volgt uit:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

8. Voer `exit` bij de `1>` vragen om het hulpprogramma tsql af te sluiten.

##<a name="sqoop-export"></a>Sqoop exporteren

3. Vanuit de SSH-verbinding met HDInsight, se de volgende opdracht uit om te bevestigen dat Sqoop uw SQL-Database kunnen zien:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Dit moet retourneren een lijst met databases, inclusief de **sqooptest** -database die u eerder hebt gemaakt.

4. Gebruik de volgende opdracht gegevens exporteren vanuit **hivesampletable** aan de tabel **mobiledata** :

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasbs:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    Hiermee wordt aangegeven wanneer Sqoop verbinding maken met SQL-Database, met de database **sqooptest** , en exporteren van gegevens uit de **wasbs: / / / component/warehouse/hivesampletable** (fysieke bestanden voor de *hivesampletable*) aan de tabel **mobiledata** .

5. Nadat de opdracht is voltooid, gebruikt u de volgende verbinding maken met de database met TSQL:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Zodra u verbinding hebt, gebruikt u de volgende instructies om te bevestigen dat de gegevens zijn geëxporteerd naar de tabel **mobiledata** :

        SELECT * FROM mobiledata
        GO

    Hier ziet u een lijst met gegevens in de tabel. Type `exit` wanneer u het hulpprogramma tsql afsluit.

##<a name="sqoop-import"></a>Sqoop importeren

1. Gebruik van de volgende handelingen uit om gegevens te importeren uit de tabel **mobiledata** in SQL-Database, naar de **wasbs: / / / zelfstudies/usesqoop/importeddata** op HDInsight:

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    De geïmporteerde gegevens worden velden die worden gescheiden door een tabteken hebt en de regels worden door een nieuwe regel-teken worden beëindigd.

2. Wanneer het importeren is voltooid, moet u de volgende opdracht aan lijst met de gegevens weggegooid gebruiken in de nieuwe map:

        hadoop fs -text wasbs:///tutorials/usesqoop/importeddata/part-m-00000

##<a name="using-sql-server"></a>SQL Server gebruiken

U kunt ook Sqoop importeren en exporteren van gegevens uit SQL Server, in uw datacenter of op een virtuele Machine die worden gehost in Azure wordt aangegeven. De verschillen tussen het gebruik van de SQL-Database en SQL Server zijn:

* Zowel HDInsight en de SQL Server moeten zich op hetzelfde Azure virtuele netwerk

    > [AZURE.NOTE] HDInsight ondersteunt alleen locatie gebaseerde virtuele netwerken en deze niet werkt momenteel met virtuele affiniteit groep-netwerken.

    Wanneer u SQL Server in uw datacenter gebruikt, moet u het virtuele netwerk configureren als *site-naar-site* of *punt-naar-site*.

    > [AZURE.NOTE] Virtuele netwerken **punt-naar-site** , wordt SQL Server uitgevoerd de VPN-client configuratie-toepassing, dat beschikbaar via het **Dashboard** van uw Azure virtuele netwerkconfiguratie is.

    Zie voor meer informatie Azure Virtual Network [Virtuele netwerk-overzicht](../virtual-network/virtual-networks-overview.md).

* SQL Server moet worden geconfigureerd toestaan van verificatie van SQL. Zie voor meer informatie [kiezen een verificatiemodus](https://msdn.microsoft.com/ms144284.aspx)

* U moet SQL Server als u wilt toestaan van externe verbindingen configureren. Zie [problemen met verbinding maken met de SQL Server-database-engine](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) voor meer informatie

* U moet de **sqooptest** -database maken in SQL Server met een programma zoals **SQL Server Management Studio** of **tsql** - de stappen voor het gebruik van de Azure CLI werken alleen voor Azure SQL-Database

    De TSQL-instructies voor de tabel **mobiledata** lijken die worden gebruikt voor SQL-Database, met uitzondering van maken van een clusterd index - dit is niet vereist is voor SQL Server:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* Wanneer u verbinding maakt met de SQL Server uit HDInsight, is het wellicht gebruik van het IP-adres van de SQL Server, tenzij u een Domain Name System (DNS) om op te lossen namen in de virtuele Azure-netwerk hebt geconfigureerd. Bijvoorbeeld:

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

##<a name="limitations"></a>Beperkingen

* Bulksgewijs exporteren - HDInsight met Linux gebaseerde, de Sqoop verbindingslijn die wordt gebruikt om gegevens te exporteren naar Microsoft SQL Server of Azure SQL-Database ondersteunt momenteel niet bulksgewijs invoegen.

* Batchen - met Linux gebaseerde HDInsight, bij gebruik van de `-batch` overschakelt bij het uitvoeren van wordt ingevoegd, Sqoop meerdere wordt ingevoegd in plaats van de invoegbewerkingen batchen wordt uitgevoerd.

##<a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd hoe Sqoop gebruiken. Meer informatie raadpleegt u:

- [Oozie gebruiken met HDInsight][hdinsight-use-oozie]: gebruik Sqoop actie in een werkstroom Oozie.
- [Analyseren van gegevens over vertragingen flight met HDInsight][hdinsight-analyze-flight-data]: component gebruiken om te analyseren flight uitstellen gegevens en gebruikt u Sqoop gegevens exporteren naar een Azure SQL-database.
- [Gegevens uploaden naar HDInsight][hdinsight-upload-data]: zoeken naar andere methoden voor het uploaden van gegevens naar HDInsight/Azure-blobopslag.



[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
