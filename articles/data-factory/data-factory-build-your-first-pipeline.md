<properties
    pageTitle="Gegevens Factory zelfstudie: eerste gegevens verkooppijplijn | Microsoft Azure"
    description="Deze zelfstudie Azure gegevens Factory ziet u hoe u maken en plannen van een fabriek gegevens waarmee gegevens met behulp van component script op een Hadoop-cluster worden verwerkt."
    services="data-factory"
    keywords="Azure gegevens factory zelfstudie, hadoop cluster, hadoop-component"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-pipeline-to-process-data-using-hadoop-cluster"></a>Zelfstudie: Uw eerste verkooppijplijn om gegevens te verwerken met Hadoop cluster maken 
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-build-your-first-pipeline.md)
- [Azure-portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resourcemanager-sjabloon](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

In deze zelfstudie kunt u uw eerste factory Azure-gegevens met een pijplijn gegevens waarmee gegevens worden verwerkt door het component script uitvoeren op een cluster Azure HDInsight (Hadoop) maken. 

> [AZURE.NOTE] In dit artikel biedt een overzicht van Azure gegevens Factory geen. Zie [Inleiding tot Factory van Azure-gegevens](data-factory-introduction.md)voor een overzicht van de service. Zie [gegevens Factory leerpad](https://azure.microsoft.com/documentation/learning-paths/data-factory/) voor een aanbevolen manier om te navigeren door onze inhoud voor meer informatie over Data Factory.

## <a name="whats-covered-in-this-tutorial"></a>Wat zijn van toepassing in deze zelfstudie? 
**Azure gegevens Factory** kunt u voor het opstellen van **verplaatsing** van de gegevens en gegevens **processing** taken als gegevensgestuurde werkstromen (ook wel gegevens pijpleidingen genoemd). Leert u hoe u uw eerste gegevens verkooppijplijn met een gegevensverwerking (of gegevenstransformatie) activiteit kunt maken. Deze activiteit gebruikt een cluster HDInsight Hadoop transformeren en analyseren steekproef weblogboeken.  

In deze zelfstudie kunt u de volgende stappen uitvoeren:

1.  Maak een **gegevens factory**. Een factory gegevens bevatten een of meer gegevens pijpleidingen die gegevens verplaatsen en proces. 
2.  **Gekoppelde services**maken. U maakt een gekoppelde service om te koppelen van een gegevensopslag of een berekeningscluster service fabriek gegevens. Een gegevensopslag zoals Azure Storage bevat invoer/uitvoergegevens van activiteiten in de pijplijn. Een berekeningscluster service zoals HDInsight Hadoop cluster processen/transformaties gegevens.    
3.  Invoer maken en uitvoeren van **gegevenssets**. Een invoer dataset staat voor de invoer voor een activiteit in de pijplijn en een gegevensset uitvoer voor de uitvoer voor de activiteit.
3.  Maak de **verkooppijplijn**. Een pijplijn kunt beschikken over een of meer activiteiten (voorbeelden: kopie activiteit, HDInsight component activiteit). In dit voorbeeld wordt de activiteit HDInsight component die wordt uitgevoerd als een script component op een cluster HDInsight Hadoop gebruikt. Het script maakt eerst een tabel die verwijst naar de onbewerkte web logboekgegevens die zijn opgeslagen in Azure-blobopslag en klikt u vervolgens de onbewerkte gegevens partities op jaar en maand.

    Er zijn twee soorten activiteiten die worden ondersteund door Factory van Azure-gegevens. Ze zijn: [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) en [gegevens-transformatie activiteiten](data-factory-data-transformation-activities.md). Er is slechts één gegevens verkeer activiteit, die is van de activiteit kopiëren. In deze zelfstudie gebruik u niet de activiteit kopiëren. Zie voor een zelfstudie over het gebruik van de activiteit kopie [Zelfstudie: gegevens kopiëren van een Azure blob naar SQL Azure](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). HDInsight component activiteit die u in deze zelfstudie is een van de gegevens transformatie activiteiten worden ondersteund door gegevens Factory.  
 
Hier ziet u de **diagramweergave** van de steekproef gegevens fabriek die u in deze zelfstudie maakt. 

![Diagramweergave in Data Factory zelfstudie](./media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)

In deze zelfstudie bevat **inputdata** map van de container van de Azure blob **adfgetstarted** één bestand met de naam input.log. Dit bestand heeft vermeldingen van drie maanden: januari, februari en maart van 2016. Hier vindt u de rijen van de steekproef voor elke maand bestand. 

    2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
    2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
    2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871

Wanneer het bestand wordt verwerkt door de pijplijn met HDInsight component activiteit, de activiteit wordt uitgevoerd een script component op het cluster HDInsight of partities gegevens ingevoerd op jaar en maand. Het script Hiermee maakt u drie uitvoermappen met een bestand met het posten van elke maand.  

    adfgetstarted/partitioneddata/year=2016/month=1/000000_0
    adfgetstarted/partitioneddata/year=2016/month=2/000000_0
    adfgetstarted/partitioneddata/year=2016/month=3/000000_0

Van de steekproef lijnen wordt weergegeven boven aan de eerste fase (met 2016-01-01) naar het bestand 000000_0 in de maand is geschreven = 1 map. Op dezelfde manier in het tweede voorbeeld is geschreven naar het bestand in de maand = 2 map en de derde een naar het bestand is geschreven in de maand = 3 map.  


## <a name="pre-requisites"></a>Minimumvereisten
Voordat u deze zelfstudie begint, moet u de volgende vereisten hebben:

1.  **Azure abonnement** - als u geen Azure-abonnement hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie de [Gratis proefabonnement](https://azure.microsoft.com/pricing/free-trial/) artikel over hoe u een gratis proefabonnement account kan krijgen.

2.  **Azure-opslag** – u een account Azure opslag gebruiken voor het opslaan van de gegevens in deze zelfstudie. Als u niet een Azure opslag-account hebt, raadpleegt u het artikel [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) . Nadat u het opslag-account hebt gemaakt, ziet u de **naam van het account** en **toegangstoets**. Zie de [weergave, kopiëren en opnieuw genereren opslag toegangstoetsen](../storage/storage-create-storage-account.md#view-and-copy-storage-access-keys). 

### <a name="upload-files-to-azure-storage-for-the-tutorial"></a>Bestanden uploaden naar Azure-opslag voor de zelfstudie
Voordat u de zelfstudie start, moet u uw account Azure opslagruimte met voorbeeldbestanden voorbereiden voor de zelfstudie.

1. Component query-bestand (HQL) uploaden naar de map **script** van de **adfgetstarted** blob container.
2. Invoer-bestand uploaden naar de map **inputdata** van de **adfgetstarted** blob container. 

#### <a name="create-hql-script-file"></a>HQL script-bestand maken 

1. Start **Kladblok** en plak de volgende HQL-script. Dit onderdeel script Hiermee maakt u twee tabellen: **WebLogsRaw** en **WebLogsPartitioned**. Klik op **bestand** in het menu en selecteer **OpslaanAls**. Overschakelen naar de map **C:\adfgetstarted** op uw harde schijf. Selecteer * *alle bestanden (*.*) **voor de** Opslaan als** Typ veld. Voer **partitionweblogs.hql** voor de **bestandsnaam**. Bevestigen dat de **codering** veld onderaan in het dialoogvenster is ingesteld op **ANSI**. Zo niet, stel deze in op **ANSI **.  

        set hive.exec.dynamic.partition.mode=nonstrict;
        
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
        
        LOAD DATA INPATH '${hiveconf:inputtable}' OVERWRITE INTO TABLE WebLogsRaw;
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
        
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

Gedurende runtime, de activiteit component in de pijplijn Data Factory waarden worden doorgegeven voor de **inputtable** en **partitionedtable** parameters zoals wordt weergegeven in het volgende fragment:  

        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"

De **storageaccountname** , is de naam van uw account Azure opslag.
 
#### <a name="create-a-sample-input-file"></a>Maken van een voorbeeldbestand van de invoer
Met Kladblok, maakt u een bestand met de naam **input.log** in de **c:\adfgetstarted** met de volgende inhoud: 

    #Software: Microsoft Internet Information Services 8.0
    #Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step5.png X-ARR-LOG-ID=9b0c14b1-434d-495a-9b0d-46775194257b 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 37931 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step6.png X-ARR-LOG-ID=f99cff81-ec38-4a13-b2fe-21b10adca996 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 27146 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step1.png X-ARR-LOG-ID=af94d3b4-8e05-4871-82c4-638f866d4e83 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 66259 871 140
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47

#### <a name="upload-input-file-and-hql-file-to-your-azure-blob-storage"></a>Invoer bestands- en HQL bestand uploaden naar uw Azure-blobopslag

In deze sectie bevat instructies over het gebruik van **AzCopy** hulpmiddel input.log en partitionweblogs.hql bestanden kopiëren naar Azure-blobopslag. U kunt een hulpmiddel van uw keuze (bijvoorbeeld: [Microsoft Azure opslag Explorer](http://storageexplorer.com/), [CloudXPlorer door ClumsyLeaf Software](http://clumsyleaf.com/products/cloudxplorer) voor deze taak.   
     
1. Download de [meest recente versie van **AzCopy**](http://aka.ms/downloadazcopy), of de [meest recente preview-versie](http://aka.ms/downloadazcopypr). Zie [het gebruik van AzCopy](../storage/storage-use-azcopy.md) artikel voor instructies over het gebruik van het hulpprogramma.
2. Navigeer naar de map c:\adfgetstarted en voer de volgende opdracht: 

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy" /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/inputdata /DestKey:<storageaccesskey>  /Pattern:input.log

    Deze opdracht uploadt het bestand **input.log** naar het opslag-account (**adfgetstarted** container en **inputdata** map). **Storageaccountname** vervangen door de naam van uw account voor opslagruimte en **storageaccesskey** met de toegangstoets opslag.

    > [AZURE.NOTE] Deze opdracht Hiermee maakt u een container met **adfgetstarted** in Azure Blob storage de naam en het bestand **input.log** van uw lokale harde schijf kopiëren naar de map **inputdata** in de container. 
    
3. Als het bestand is geüpload, ziet u de uitvoer die vergelijkbaar is met het volgende uit AzCopy.
    
        Finished 1 of total 1 file(s).
        [2015/12/16 23:07:33] Transfer summary:
        -----------------
        Total files transferred: 1
        Transfer successfully:   1
        Transfer skipped:        0
        Transfer failed:         0
        Elapsed time:            00.00:00:01
5. Voer de volgende opdracht uit het **partitionweblogs.hql** -bestand uploaden naar de map **script** van de container **adfgetstarted** . Hier ziet u de opdracht: 
    
        AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/script /DestKey:<storageaccesskey>  /Pattern:partitionweblogs.hql

U kunt de vereisten hebt voltooid. U kunt een factory voor gegevens met een van de volgende manieren maken. Klik op een van de tabbladen aan de bovenkant of de volgende koppelingen om uit te voeren de zelfstudie. 

- [Azure-portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resourcemanager-sjabloon](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)