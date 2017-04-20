<properties
   pageTitle="Gegevensopslag Lake integreren met andere Services Azure | Microsoft Azure"
   description="Meer informatie over hoe Lake gegevensopslag werkt naadloos samen met andere services van Azure"
   documentationCenter=""
   services="data-lake-store"
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

# <a name="integrating-data-lake-store-with-other-azure-services"></a>Gegevensopslag Lake integreren met andere Services van Azure

Azure Lake gegevensopslag kan worden gebruikt in combinatie met andere services Azure om in te schakelen van een grotere groep scenario's beschreven. Het volgende artikel worden de services die Lake gegevensopslag kan worden geïntegreerd met.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Gebruik voor gegevensopslag Lake met Azure HDInsight

U kunt een [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) -cluster dat Lake gegevensopslag als de HDFS-compatibele opslag gebruikt inrichten. Voor deze release, kunt voor Hadoop en Storm clusters voor Windows en Linux, u Lake gegevensopslag alleen als een extra opslagruimte. Azure-opslag (WASB) dergelijke clusters nog steeds gebruiken als de standaard-opslag. U kunt echter voor HBase kolomgroepen voor Windows en Linux Lake gegevensopslag gebruiken als de standaard-opslag, en/of extra opslagruimte.

Zie voor instructies over het inrichten van een HDInsight cluster met Lake gegevensopslag:

* [Een cluster HDInsight inrichten met Lake gegevensopslag met behulp van Azure-Portal](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Een cluster HDInsight inrichten met Lake gegevensopslag via Azure PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)

**Video's het beste?** Volg de onderstaande koppelingen voor het bekijken van video's met instructies over het gebruik van Lake gegevensopslag met HDInsight clusters.

* [Maken van een HDInsight cluster met toegang tot Lake gegevensopslag](https://mix.office.com/watch/l93xri2yhtp2)
* Nadat het cluster is ingesteld, [Access-gegevens in Lake gegevensopslag component en varken-scripts gebruiken](https://mix.office.com/watch/1n9g5w0fiqv1q)


## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Gebruik voor gegevensopslag Lake met Azure gegevens Lake Analytics

[Azure gegevens Lake Analytics](../data-lake-analytics/data-lake-analytics-overview.md) kunt u werken met grote gegevens bij het op schaal van de cloud. Dynamisch bepalingen bronnen en kunt u doen analytics op TB of zelfs exabytes van gegevens die kunnen worden opgeslagen in een getal met ondersteunde gegevensbronnen, een van hen Lake gegevensopslag. Gegevens Lake Analytics is speciaal geoptimaliseerd voor het werken met Azure Lake gegevensopslag - leveren optimale prestaties, en parallelization voor u grote gegevens werkbelasting.

Zie [Aan de slag met gegevens Lake analyses met Lake gegevensopslag](../data-lake-analytics/data-lake-analytics-get-started-portal.md)voor instructies over het gebruik van gegevens Lake analyses met Lake gegevensopslag.

**Video's het beste?** Volg de onderstaande koppelingen voor het bekijken van video's met instructies over het gebruik van Lake gegevensopslag met HDInsight clusters.

* [Azure gegevens Lake Analytics verbinden met Azure Lake gegevensopslag](https://mix.office.com/watch/qwji0dc9rx9k)
* [Access Azure Lake gegevensopslag via gegevens Lake Analytics](https://mix.office.com/watch/1n0s45up381a8)


## <a name="use-data-lake-store-with-azure-data-factory"></a>Gebruik voor gegevensopslag Lake met Azure gegevens Factory

[Azure gegevens Factory](https://azure.microsoft.com/services/data-factory/) kunt u gegevens van Azure tabellen, Azure SQL-Database, Azure SQL-DataWarehouse, BLOB's opslag van Azure en on-premises implementatie databases nemen. Wordt een eerste-klas burger in het Azure-systeem, kan Azure gegevens Factory goedkeuringen door de opname van gegevens uit deze bron Azure Lake gegevensopslag worden gebruikt.

Zie de [gegevens van en naar Lake gegevensopslag met gegevens Factory verplaatsen](../data-factory/data-factory-azure-datalake-connector.md)voor instructies over het gebruik van Azure gegevens Factory met Lake gegevensopslag.

**Video's opnieuw!** Zie [Gegevens-integratie met Azure gegevens Factory voor Azure Lake gegevensopslag](https://mix.office.com/watch/1oa7le7t2u4ka). 

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Gegevens uit Azure opslag BLOB's kopiëren naar Lake gegevensopslag

Azure Lake gegevensopslag biedt een hulpprogramma voor de opdrachtregel, AdlCopy, waarmee u gegevens van Azure-blobopslag kopiëren naar een Lake gegevensopslag-account. Zie de [gegevens van Azure opslag BLOB's naar Lake gegevensopslag kopiëren](data-lake-store-copy-data-azure-storage-blob.md)voor meer informatie.

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Gegevens kopiëren tussen Azure SQL-Database en Lake gegevensopslag

U kunt Apache Sqoop importeren en exporteren van gegevens tussen Azure SQL-Database en Lake gegevensopslag. Zie [gegevens kopiëren naar andere Lake gegevensopslag en Azure SQL-database met Sqoop](data-lake-store-data-transfer-sql-sqoop.md)voor meer informatie.

**Bekijk deze video** op [Apache Sqoop gebruiken om gegevens tussen relationele gegevensbronnen en Azure Lake gegevensopslag te verplaatsen](https://mix.office.com/watch/1butcdjxmu114).

## <a name="use-data-lake-store-with-stream-analytics"></a>Gebruik voor gegevensopslag Lake met Stream Analytics

U kunt Lake gegevensopslag als een van de uitvoer van voor de opslag van gegevens streamen door middel van Azure Stream analyses. Zie de [Stream gegevens van Azure opslag Blob in Lake gegevensopslag Azure Stream Analytics gebruiken](data-lake-store-stream-analytics.md)voor meer informatie.

## <a name="use-data-lake-store-with-power-bi"></a>Gebruik voor gegevensopslag Lake met Power BI

Power BI kunt u gegevens importeren uit een account voor gegevensopslag Lake analyseren en de gegevens visualiseren. Zie [de gegevens analyseren in Lake gegevensopslag met behulp van Power BI](data-lake-store-power-bi.md)voor meer informatie.

## <a name="use-data-lake-store-with-data-catalog"></a>Gebruik voor gegevensopslag Lake met gegevenscatalogus

U kunt gegevens uit Lake gegevensopslag registreren in de gegevenscatalogus Azure dat de gegevens makkelijker kan worden gevonden in de hele organisatie. Zie [gegevens uit Lake gegevensopslag in Azure-gegevenscatalogus hebt geregistreerd](data-lake-store-with-data-catalog.md)voor meer informatie.


## <a name="see-also"></a>Zie ook

- [Overzicht van Azure Lake gegevensopslag](data-lake-store-overview.md)
- [Aan de slag met Lake gegevensopslag met behulp van Portal](data-lake-store-get-started-portal.md)
- [Aan de slag met Lake gegevensopslag via PowerShell](data-lake-store-get-started-powershell.md)  
