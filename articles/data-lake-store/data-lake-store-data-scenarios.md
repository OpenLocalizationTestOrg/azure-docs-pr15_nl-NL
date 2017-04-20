<properties 
   pageTitle="Scenario's voor gegevens betrekking hebben op personen Lake gegevensopslag | Microsoft Azure" 
   description="Meer informatie over de verschillende scenario's en hulpmiddelen voor het gebruik van welke gegevens kunnen ingenomen, verwerkt, gedownload en in een Lake gegevensopslag visualiseren" 
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
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Met behulp van Azure Lake gegevensopslag voor groot gegevensvereisten

Er zijn vier belangrijke fasen in grote gegevensverwerking:

* Ingesting grote hoeveelheden gegevens in een gegevensopslag, op realtime of in batches
* Verwerking van de gegevens
* Downloaden van de gegevens
* De gegevens visualiseren

In dit artikel kijken we naar deze fasen weergegeven met betrekking tot Azure voor gegevensopslag Lake voor meer informatie over de opties en hulpprogramma's voor uw behoeften van grote gegevens.

## <a name="ingest-data-into-data-lake-store"></a>Gegevens in Lake gegevensopslag nemen

In dit gedeelte worden de verschillende bronnen van gegevens en de verschillende manieren waarin die gegevens kan worden bij naar een account voor gegevensopslag Lake gemarkeerd.

![Ingest gegevens in Lake gegevensopslag] (./media/data-lake-store-data-scenarios/ingest-data.png "Ingest gegevens in Lake gegevensopslag")

### <a name="ad-hoc-data"></a>Ad-hoc gegevens

Hiermee wordt kleinere gegevenssets die zijn gebruikt voor het maken van prototypen een groot toepassing. Er zijn verschillende manieren ingesting ad-hoc gegevens afhankelijk van de gegevensbron.

| Gegevensbron        | Met nemen                                                                        |
|--------------------|----------------------------------------------------------------------------------------|
| Lokale computer     | <ul> <li>[Azure-Portal](/data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure platforms CLI](data-lake-store-get-started-cli.md)</li> <li>[Met behulp van Data Lake Tools voor Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md#upload-source-data-files) </li></ul> |
| Opslag van Azure Blob | <ul> <li>[Azure gegevens Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)</li> <li>[AdlCopy hulpmiddel](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp op HDInsight cluster](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

 
### <a name="streamed-data"></a>Gegevensstroom-gegevens

Hiermee worden gegevens die kunnen worden gegenereerd door verschillende bronnen zoals toepassingen, apparaten, sensoren, enzovoort. Deze gegevens kan worden binnenkrijgen in een Lake gegevensopslag diverse hulpprogramma's. Deze hulpprogramma's wordt meestal vastleggen en de gegevens op basis van door de gebeurtenis in realtime, en schrijf vervolgens de gebeurtenissen in batches in Lake gegevensopslag zodat deze verder kunnen worden verwerkt. 

Hulpprogramma's waarmee u kunt hier volgen:
 
* [Azure Stream Analytics] (.. / stream-analytics-gegevens-lake-uitvoer.) - gebeurtenissen ingenomen in gebeurtenis Hubs kunnen worden beschreven Lake van Azure-gegevens met een uitvoer Azure Lake gegevensopslag.
* [Azure HDInsight Storm](../hdinsight/hdinsight-storm-write-data-lake-store.md) - kunt u gegevens rechtstreeks naar Lake gegevensopslag uit het cluster Storm.
* [EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost) : U kunt gebeurtenissen ontvangt van gebeurtenis Hubs en schrijf vervolgens deze naar Lake gegevensopslag met de [Gegevens Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>Relationele gegevens

U kunt ook brongegevens uit relationele databases. Relationele databases verzamelen over een periode, grote hoeveelheden gegevens die bieden belangrijke inzichten die kunt als verwerkt via een pijplijn groot gegevens. U kunt de volgende hulpprogramma's zoals om gegevens te verplaatsen naar Lake gegevensopslag.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure gegevens Factory](../data-factory/data-factory-data-movement-activities.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Log voor servergegevens (uploaden met aangepaste toepassingen)

Dit soort gegevensset is specifiek genoemd omdat analyse van logboekgegevens van web server een algemene use-case voor groot gegevenstoepassingen is en grote hoeveelheden logboekbestanden om te worden geüpload naar de gegevensopslag Lake vereist. U kunt een van de volgende hulpmiddelen gebruiken om uw eigen scripts of toepassingen voor het uploaden van dergelijke gegevens te schrijven.

* [Azure platforms CLI](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Lake gegevensopslag .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Azure gegevens Factory](../data-factory/data-factory-data-movement-activities.md)

Voor het uploaden van web server logboekgegevens, en ook voor het uploaden van andere soorten gegevens (bijvoorbeeld sociale patronen gegevens), is een goede manier om te schrijven van uw eigen aangepaste scripts/toepassingen omdat dit hebt u de flexibiliteit om uw gegevens onderdeel uploaden als onderdeel van uw groter groot gegevenstoepassing te nemen. In sommige gevallen kan deze code de vorm van een script of hulpprogramma eenvoudige opdrachtregel duren. In andere gevallen mogelijk de code worden gebruikt om te groot gegevensverwerking integreren met een bedrijfstoepassing of de oplossing.


### <a name="data-associated-with-azure-hdinsight-clusters"></a>Gegevens die zijn gekoppeld aan clusters Azure HDInsight

De meeste HDInsight clustertypen (Hadoop, HBase, Storm) ondersteunen Lake gegevensopslag als gegevens opslag opslagplaats. HDInsight clusters toegang tot gegevens van Azure opslag BLOB's (WASB). Voor betere prestaties, kunt u de gegevens uit WASB kopiëren naar een Lake gegevensopslag-account dat is gekoppeld aan het cluster. U kunt de volgende hulpprogramma's gebruiken om de gegevens te kopiëren.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy-Service](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure gegevens Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)

### <a name="data-stored-in-on-premise-or-iaas-hadoop-clusters"></a>Gegevens die zijn opgeslagen in de on-premises of IaaS Hadoop clusters

Grote hoeveelheden gegevens kunnen worden opgeslagen in de bestaande Hadoop kolomgroepen lokaal op computers met HDFS. De Hadoop-clusters mogelijk in een on-premises implementatie of mogelijk binnen een cluster IaaS op Azure. Er zijn vereisten deze gegevens kopiëren naar Azure Lake gegevensopslag voor een eenmalige aanpak of in een terugkerende manier. Er zijn verschillende opties die u dit kunt gebruiken. Hieronder volgt een lijst met alternatieven en de bijbehorende voor-en nadelen.

| Methode  | Meer informatie | Voordelen   | Overwegingen  |
|-----------|---------|--------------|-----------------|
| Gebruik Azure gegevens Factory (ADF) gegevens rechtstreeks uit Hadoop clusters kopiëren naar Azure Lake gegevensopslag | [ADF ondersteunt HDFS als gegevensbron](../data-factory/data-factory-hdfs-connector.md) | ADF biedt out-van-het-box-ondersteuning voor HDFS en eerste klas end-to-end management en het controleren | Data Management Gateway naar de on-premises of in het cluster IaaS worden geïmplementeerd vereist |
| Gegevens exporteren uit Hadoop als bestanden. Kopieer vervolgens de bestanden naar Azure Lake gegevensopslag geschikt gebruiken.                                   | U kunt bestanden tot het gebruik van Azure Lake gegevensopslag kopiëren: <ul><li>[Azure PowerShell voor Windows OS](data-lake-store-get-started-powershell.md)</li><li>[Azure platforms CLI voor niet - Windows-besturingssysteem](data-lake-store-get-started-cli.md)</li><li>Aangepaste met behulp van een SDK gegevens Lake Store-app</li></ul> | Snel aan de slag. Aangepaste uploads kunt doen                                                   | Meerdere stappen waarvoor meerdere technologieën. Beheer en bewaking zal toenemen tot een uitdaging zijn na verloop van tijd gegeven van de aangepaste aard van de hulpmiddelen |
| Gebruik Distcp om gegevens uit Hadoop naar Azure Storage kopiëren. Gegevens van Azure Storage vervolgens kopiëren naar Lake gegevensopslag geschikt gebruiken. | U kunt gegevens kopiëren van Azure opslagruimte over het gebruik van Lake gegevensopslag: <ul><li>[Azure gegevens Factory](../data-factory/data-factory-data-movement-activities.md)</li><li>[AdlCopy hulpmiddel](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache DistCp HDInsight clusters waarop](data-lake-store-copy-data-wasb-distcp.md)</li></ul>| U kunt open source's. | Meerdere stappen die betrekking heeft op meerdere technologieën |

### <a name="really-large-datasets"></a>Grote gegevenssets

Voor het uploaden van gegevenssets die in verschillende TB variëren, is met behulp van de bovenstaande methoden soms traag en duur. In dat geval kunt u de onderstaande opties.

* **Azure ExpressRoute gebruiken**. Azure ExpressRoute kunt u persoonlijke verbindingen tussen Azure datacenters en de infrastructuur van uw lokaal maken. Hierdoor wordt de optie van een betrouwbare voor het overbrengen van grote hoeveelheden gegevens. Zie de [documentatie van Azure ExpressRoute](../expressroute/expressroute-introduction.md)voor meer informatie.


* **"Offline" uploaden van gegevens**. Als het gebruik van Azure ExpressRoute niet mogelijk gecombineerd voor welke reden dan ook is, kunt u [Azure importeren/exporteren service](../storage/storage-import-export-service.md) vaste schijven met uw gegevens een Azure Datacenter voor verzenden. Uw gegevens wordt eerst naar Azure opslag BLOB's geüpload. U kunt vervolgens [Azure gegevens Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store) of [AdlCopy hulpmiddel](data-lake-store-copy-data-azure-storage-blob.md) gegevens van Azure opslag BLOB's kopiëren naar Lake gegevensopslag.

    >[AZURE.NOTE] Terwijl u met de service importeren/exporteren, de grootte op de schijven die u verzendt Azure Datacenter voor mag geen groter is dan 200 GB.


## <a name="process-data-stored-in-data-lake-store"></a>Gegevens die zijn opgeslagen in Lake gegevensopslag verwerken

Als u de gegevens hebt toegevoegd in Lake gegevensopslag kunt u analyse kunt uitvoeren op die gegevens met de ondersteunde groot gegevenstoepassingen. U kunt op dit moment Azure HDInsight en Azure gegevens Lake Analytics gegevensanalyse taken uitvoeren op de gegevens die zijn opgeslagen in Lake gegevensopslag gebruiken. 

![De gegevens analyseren in Lake gegevensopslag] (./media/data-lake-store-data-scenarios/analyze-data.png "De gegevens analyseren in Lake gegevensopslag")

U kunt de volgende voorbeelden bekijken.

* [Maken van een HDInsight cluster met Lake gegevensopslag als opslag](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Azure gegevens Lake Analytics gebruiken met Lake gegevensopslag](../data-lake-analytics/data-lake-analytics-get-started-portal.md)



## <a name="download-data-from-data-lake-store"></a>Gegevens downloaden uit Lake gegevensopslag

U kunt ook kunnen downloaden of verplaats gegevens van Azure Lake gegevensopslag voor scenario's, zoals:

* Gegevens verplaatsen naar andere opslagplaatsen om een koppeling met uw bestaande gegevensverwerking pijpleidingen. U wilt bijvoorbeeld Verplaats gegevens van Lake gegevensopslag naar Azure SQL-Database of lokale SQL Server.
* Gegevens downloaden naar uw lokale computer om te worden verwerkt in IDE omgevingen tijdens het maken van prototypen.

![Egress gegevens uit Lake gegevensopslag] (./media/data-lake-store-data-scenarios/egress-data.png "Egress gegevens uit Lake gegevensopslag")

In dat geval kunt u een van de volgende opties:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure gegevens Factory](../data-factory/data-factory-data-movement-activities.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

U kunt ook de volgende methoden gebruiken om te schrijven van uw eigen script/application als u wilt gegevens downloaden uit Lake gegevensopslag.

* [Azure platforms CLI](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Lake gegevensopslag .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Gegevens visualiseren in Lake gegevensopslag

U kunt een combinatie van services visuele weergaven van gegevens die zijn opgeslagen in Lake gegevensopslag maken.

![Gegevens visualiseren in Lake gegevensopslag] (./media/data-lake-store-data-scenarios/visualize-data.png "Gegevens visualiseren in Lake gegevensopslag")

* U kunt starten via [Azure gegevens Factory om gegevens uit Lake gegevensopslag naar Azure SQL Data Warehouse te verplaatsen](../data-factory/data-factory-data-movement-activities.md#supported-data-stores)
* Daarna kunt u [Power BI met Azure SQL Data Warehouse integreren](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) als u wilt maken van visuele weergave van de gegevens.
