<properties
   pageTitle="Uw component query's sneller wordt uitgevoerd in HDInsight optimaliseren | Microsoft Azure"
   description="Leer hoe u uw component query's optimaliseren voor Hadoop in HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/28/2015"
   ms.author="rashimg"/>


# <a name="optimize-hive-queries-for-hadoop-in-hdinsight"></a>Query's component optimaliseren voor Hadoop in HDInsight

Standaard worden Hadoop clusters zijn niet geoptimaliseerd voor prestaties. In dit artikel worden enkele van de meest voorkomende component prestaties optimalisatie methoden die u kunt toepassen op onze query's.

##<a name="scale-out-worker-nodes"></a>De schaal van werknemer knooppunten aanpassen

Het aantal werknemer knooppunten in een cluster met groter wordende kunt gebruikmaken van meer mappers en verkleiningstoestellen moet worden uitgevoerd in parallel. Er zijn twee manieren kunt u de schaal af in HDInsight vergroten:

- Op het moment inrichten, kunt u het aantal knooppunten van werknemer via de Portal van Azure, Azure PowerShell of meerdere platforms opdracht lijn interface.  Zie [inrichten HDInsight clusters](hdinsight-provision-clusters.md)voor meer informatie. Het volgende scherm de werknemer knooppuntconfiguratie weergeven op de Azure-Portal:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]

- Bij de uitvoering kunt u ook de schaal van een cluster zonder opnieuw te maken op een. Dit is hieronder weergegeven.
![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Zie voor meer informatie over de verschillende virtuele machines worden ondersteund door HDInsight, de [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="enable-tez"></a>Tez inschakelen

[Apache Tez](http://hortonworks.com/hadoop/tez/) is een alternatief execution engine voor de engine MapReduce:

![tez_1][image-hdi-optimize-hive-tez_1]


Tez is sneller omdat:

- Hiertoe opdracht krijgt acyclische Graph (DAG) uitvoeren als één taak in de engine MapReduce, de DAG die wordt vermeld, is vereist voor elke set mappers wordt gevolgd door een reeks verkleiningstoestellen. Hierdoor meerdere MapReduce taken om te worden of uitschakelen voor elke component query. Tez heeft geen deze beperking en complexe DAG kunnen worden verwerkt als één taak, dus minimaliseren taak opstarten realiseren.
- **Onnodige Avoids schrijft** De uitvoer van elke taak is vanwege meerdere taken wordt of voor dezelfde component query in de engine MapReduce, geschreven naar HDFS voor tussenliggende gegevens. Aangezien Tez beperkt tot een minimum aantal taken voor elke component query kan voorkomen onnodige schrijven.
- **Minimizes opstart vertragingen** Tez kan beter opstart vertraging minimaliseren door waardoor het aantal mappers die deze moet worden gestart en ook verbeteren optimalisatie overal in.
- **Wordt gebruikgemaakt van containers** Wanneer is mogelijk Tez kunnen containers om ervoor te zorgen dat latentie vanwege containers opgestart wordt verminderd gebruiken.
- **Continue optimalisatie technieken** Traditioneel werden optimalisatie geëffend tijdens compilatie fase. Maar meer informatie over de invoer beschikbaar is waarmee u kunt voor betere optimalisatie tijdens runtime. Tez worden continu optimalisatie technieken waardoor hij kan het optimaliseren van het abonnement verder naar de fase runtime gebruikt.

Voor meer informatie over deze concepten, klikt u op [hier](http://hortonworks.com/hadoop/tez/)

U kunt een query component Tez ingeschakeld door de query met de onderstaande instelling voorvoegsel aanbrengen:

    set hive.execution.engine=tez;

Voor Windows gebaseerde HDInsight kolomgroepen moet Tez op het moment inrichten zijn ingeschakeld. Hier volgt een voorbeeld Azure PowerShell-script voor het inrichten van een Hadoop-cluster met Tez ingeschakeld:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $clusterName = "[HDInsightClusterName]"
    $location = "[AzureDataCenter]" #i.e. West US
    $dataNodes = 32 # number of worker nodes in the cluster

    $defaultStorageAccountName = "[DefaultStorageAccountName]"
    $defaultStorageContainerName = "[DefaultBlobContainerName]"
    $defaultStorageAccountKey = $defaultStorageAccountKey = Get-AzureStorageKey $defaultStorageAccountName.ToLower() | %{ $_.Primary }

    $hdiUserName = "[HTTPUserName]"
    $hdiPassword = "[HTTPUserPassword]"

    $hdiSecurePassword = ConvertTo-SecureString $hdiPassword -AsPlainText -Force
    $hdiCredential = New-Object System.Management.Automation.PSCredential($hdiUserName, $hdiSecurePassword)

    $hiveConfig = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
    $hiveConfig.Configuration = @{ "hive.execution.engine"="tez" }

    New-AzureHDInsightClusterConfig -ClusterSizeInNodes $dataNodes -HeadNodeVMSize Standard_D14 -DataNodeVMSize Standard_D14 |
    Set-AzureHDInsightDefaultStorage -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" -StorageAccountKey $defaultStorageAccountKey -StorageContainerName $defaultStorageContainerName |
    Add-AzureHDInsightConfigValues -Hive $hiveConfig |
    New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $hdiCredential

    
> [AZURE.NOTE] Linux gebaseerde HDInsight clusters hebben Tez standaard ingeschakeld.
    

## <a name="hive-partitioning"></a>Component partitioneren

I/o-bewerking is de belangrijkste bottleneck voor het uitvoeren van query's component. De prestaties kunnen worden verbeterd als de hoeveelheid gegevens die moeten worden gelezen kan worden afgetrokken. Query's component scannen standaard volledige component tabellen. Dit is handig voor query's zoals tabel gescand, echter voor query's die hoeft slechts eenmaal te scannen van een kleine hoeveelheid gegevens (bijvoorbeeld query waarbij filters zijn), Hiermee maakt u overbodige belasting. Component partitioneren kunt component query's voor toegang tot alleen de benodigde hoeveelheid gegevens in tabellen component.

Component partitioneren wordt geïmplementeerd door de onbewerkte gegevens ordenen in nieuwe mappen met elke partition met een eigen directory - waar de partition is gedefinieerd door de gebruiker. In het volgende diagram ziet u een tabel component partitioneren op de kolom *Year*. Een nieuwe map is gemaakt voor elk jaar.

![partitioneren][image-hdi-optimize-hive-partitioning_1]

Sommige partities overwegingen:

- **Kan niet onder partition** - partities op kolommen met slechts enkele waarden kunnen leiden tot weinig partities. Bijvoorbeeld partities op geslacht wordt alleen maken twee partities worden gemaakt (man en vrouw), dus de latentie met maximaal halverwege alleen verminderen.

- **Te weinig niet partitioneren** - treedt Klik op de andere extreme, een partition maken voor een kolom met een unieke waarde (bijvoorbeeld gebruikersnaam) meerdere partities waardoor er een groot aantal belasting op het namenode cluster zoals zal moeten de grote hoeveelheid mappen verwerken.

- **Vermijd gegevens laten** - kiest u uw sleutel partities zorgvuldig dat alle partities even grootte zijn. Een voorbeeld is partities op *staat* kan ertoe leiden dat het aantal records onder Californië moeten bijna 30 x van Vermont vanwege het verschil in de populatie.

Als u wilt maken van een partitietabel, gebruikt u de *Partities By* -component:

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE        STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Nadat de gepartitioneerde tabel is gemaakt, kunt u partitioneren van statische of dynamische partitioneren maken.

- **Statische partitioneren** betekent dat u al een laptopgeheugen gegevens in de juiste mappen hebt en kunt u component partities handmatig op basis van de maplocatie vragen. Dit wordt weergegeven in het onderstaande codefragment.

        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’

        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasbs://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'

- **Dynamische partitioneren** betekent dat u wilt dat component partities automatisch voor u te maken. Aangezien we al de partities tabel uit het tijdelijk opslaan tabel hebt gemaakt, is we hoeft te gegevens aan de gepartitioneerde tabel invoegen, zoals hieronder wordt weergegeven:

        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
             L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as       L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as       L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as   L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Zie [Partities tabellen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables)voor meer informatie.

##<a name="use-the-orcfile-format"></a>Gebruik de ORCFile-indeling

Component ondersteunt verschillende bestandsindelingen. Bijvoorbeeld:

- **Tekst**: dit is de standaardbestandsindeling en werkt met de meeste scenario's
- **Avro**: geschikt voor interoperabiliteit scenario's
- **ORC/parketvloeren**: meest geschikt is voor de prestaties

ORC (geoptimaliseerd rij kolommen)-indeling is een zeer efficiënte manier voor de opslag component gegevens. Vergeleken met andere indelingen, heeft ORC de volgende voordelen:

- ondersteuning voor complexe gegevenstypen, waaronder DateTime en complexe en gedeeltelijk gestructureerde typen
- maximaal compressie van 70%
- indexeert elke 10.000 rijen waarmee rijen overslaan
- een aanzienlijk decoratieve in runtime worden uitgevoerd

Als u wilt inschakelen ORC opmaken, maakt u eerst een tabel met de component *opgeslagen als ORC*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT     STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Vervolgens plaatst u gegevens aan de tabel ORC uit de tabel tijdelijk opslaan. Bijvoorbeeld:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
           L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

U kunt meer informatie over de notatie ORC [hier](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

##<a name="vectorization"></a>Vectorization

Vectorization kunt component verwerkingstijd van een reeks 1024 rijen samen in plaats van het verwerken van één rij per keer. Dit betekent dat eenvoudige bewerkingen sneller worden voltooid omdat minder interne code moet worden uitgevoerd.

Vectorization aanduiding om in te schakelen voor uw query component met de volgende instellingen:

    set hive.vectorized.execution.enabled = true;

Zie voor meer informatie [Vectorized query worden uitgevoerd](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).


##<a name="other-optimization-methods"></a>Andere methoden optimalisatie

Er zijn meer optimalisatie methoden die u in gedachten, bijvoorbeeld houden kunt:

- **Component bucketing:** een techniek waarmee cluster of segment grote reeksen gegevens optimaliseren van de query.
- **Deelnemen aan optimalisatie:** optimalisatie van de uitvoering van de component query plan te verbeteren de efficiëntie van joins en dat u hints voor de gebruiker te verkleinen. Zie [deelnemen aan optimalisatie](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization)voor meer informatie.
- **verkleiningstoestellen vergroten**

##<a id="nextsteps"></a>Volgende stappen
U kunt verschillende veelvoorkomende component query optimalisatie methoden hebt geleerd in dit artikel. Meer informatie raadpleegt u de volgende artikelen:

- [Gebruik Apache component in HDInsight](hdinsight-use-hive.md)
- [Gegevens over vertragingen flight analyseren met behulp van component in HDInsight](hdinsight-analyze-flight-delay-data.md)
- [Gegevens te analyseren Twitter met component in HDInsight](hdinsight-analyze-twitter-data.md)
- [Gebruik van de Query component Console op Hadoop in HDInsight sensorgegevens analyseren](hdinsight-hive-analyze-sensor-data.md)
- [Component gebruiken met HDInsight te analyseren Logboeken vanaf websites](hdinsight-hive-analyze-website-log.md)


[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
