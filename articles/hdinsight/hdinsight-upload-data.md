<properties
    pageTitle="Gegevens voor Hadoop-projecten in HDInsight uploaden | Microsoft Azure"
    description="Informatie over het uploaden en toegang tot gegevens voor Hadoop-projecten in met de CLI Azure Azure opslag Explorer, Azure PowerShell, de opdrachtregel Hadoop of Sqoop HDInsight."
    services="hdinsight,storage"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>



#<a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Gegevens voor Hadoop-projecten in HDInsight uploaden

Azure HDInsight biedt een volledige Hadoop distributed bestand system (HDFS) via Azure-blobopslag. Dit is bedoeld als extensie HDFS om klanten een naadloze ervaring te bieden. U kunt de volledige set van onderdelen in het systeem Hadoop werken rechtstreeks bij het beheren van gegevens. Azure-blobopslag en HDFS zijn distinct bestandssystemen die zijn geoptimaliseerd voor de opslag van gegevens en berekeningen op die gegevens. Zie voor informatie over de voordelen van het gebruik van Azure-blobopslag [Gebruik Azure-blobopslag met HDInsight][hdinsight-storage].

**Vereisten voor**

Houd rekening met de volgende vereiste voordat u begint:

* Een cluster Azure HDInsight. Zie voor instructies [aan de slag met Azure HDInsight] [ hdinsight-get-started] of [inrichten HDInsight clusters][hdinsight-provision].

##<a name="why-blob-storage"></a>Waarom blob storage?

Azure HDInsight clusters worden meestal gebruikt om uit te voeren MapReduce taken en de clusters worden niet weergegeven nadat deze taken hebt voltooid. De gegevens behouden in de HDFS zou clusters wanneer berekeningen voltooid zijn een dure manier voor het opslaan van deze gegevens. Azure-blobopslag is een zeer beschikbaar, ten zeerste scalable, hoge capaciteit, lage kosten en deelbaar opslagoptie voor gegevens die moeten worden verwerkt met HDInsight. Gegevens in een blob opslaat, kunt de HDInsight-clusters die worden gebruikt voor berekening veilig verschijnen zonder gegevens kwijt te raken.

###<a name="directories"></a>Mappen

Azure Blob storage containers opslag van gegevens als sleutel/waardeparen en er is geen directory-hiërarchie. Het teken "/" kan echter binnen de naam van de sleutel worden gebruikt, zodat u deze worden weergegeven als wanneer een bestand is opgeslagen in de structuur van een map. HDInsight ziet deze wel zo werkelijke mappen.

Sleutel van een blob mogelijk bijvoorbeeld *input/log1.txt*. Er bestaat geen werkelijke 'invoer' map, maar vanwege de aanwezigheid van het teken "/" in de naam van de sleutel, heeft het uiterlijk van een bestandspad.

Reden als u extra Azure Explorer gebruiken mogelijk ziet u enkele 0 byte-bestanden. Deze bestanden voor twee doeleinden worden gebruikt:

- Als er lege mappen, worden ze markeren van het bestaan van de map. Azure-blobopslag is slimme genoeg is om te weten dat als een blob genoemd foo/balk bestaat, er een map genaamd **foo**. Maar de enige manier om aan te duiden een lege map genaamd **foo** doordat dit bestand speciale 0 byte op hun plaats staan.

- Ze houdt speciale metagegevens die nodig is voor het bestandssysteem Hadoop, met name de machtigingen en eigenaren voor de mappen.

##<a name="command-line-utilities"></a>Hulpprogramma

Microsoft biedt de volgende hulpprogramma's voor gebruik met Azure-blobopslag:

| Hulpmiddel | Linux | OS X | Windows |
| ---- |:-----:|:----:|:-------:|
| [Azure opdrachtregel-Interface][azurecli] | ✔ | ✔ | ✔ |
| [Azure PowerShell][azure-powershell] | | | ✔ |
| [AzCopy][azure-azcopy] | | | ✔ |
| [Hadoop-opdracht](#commandline) | ✔ | ✔ | ✔ |

> [AZURE.NOTE] Terwijl de Azure CLI, Azure PowerShell en AzCopy kunnen alle worden gebruikt vanaf buiten Azure, de Hadoop opdracht is alleen beschikbaar op het cluster HDInsight en kunt alleen het laden van gegevens uit het lokale bestandssysteem in Azure-blobopslag.

###<a id="xplatcli"></a>Azure CLI

De CLI Azure is een platforms hulpmiddel waarmee u Azure services beheren. Gebruik de volgende stappen uit om gegevens te uploaden naar Azure-blobopslag:

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Installeren en configureren van de Azure CLI voor Mac, Linux en Windows](../xplat-cli-install.md).

2. Open een opdrachtprompt, we vaker doen of andere shell en gebruiken van de volgende handelingen uit om te verifiëren bij uw Azure-abonnement.

        azure login

    Wanneer u wordt gevraagd, typt u de gebruikersnaam en wachtwoord voor uw abonnement.

3. Voer de volgende opdracht voor een overzicht van de accounts van de opslagruimte voor uw abonnement:

        azure storage account list

4. Selecteer het opslag-account dat de blob die u werken wilt met bevat en het gebruik van de volgende opdracht uit om de sleutel voor dit account ophalen:

        azure storage account keys list <storage-account-name>

    Dit **primaire** en **secundaire** sleutels moet worden geretourneerd. Kopieer de waarde van de **primaire** sleutel omdat deze worden gebruikt in de volgende stappen.

5. Gebruik de volgende opdracht uit een lijst met blob containers in het account opslag ophalen:

        azure storage container list -a <storage-account-name> -k <primary-key>

6. De volgende opdrachten gebruiken om te uploaden en downloaden van bestanden naar de label:

    * Een bestand te uploaden:

            azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>

    * Een bestand downloaden:

            azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Als u altijd met hetzelfde opslag-account werkt, kunt u de volgende omgevingsvariabelen in plaats van de precisie van het account instelt en sleutel voor elke opdracht:
>
> * **AZURE\_opslag\_ACCOUNT**: de naam van het opslag-account
>
> * **AZURE\_opslag\_ACCESS\_sleutel**: de accountsleutel opslag

###<a id="powershell"></a>Azure PowerShell

Azure PowerShell is een uitvoeren van scripts-omgeving waarin u gebruiken kunt om te bepalen en automatiseren de implementatie en het beheer van uw werkbelasting in Azure wordt aangegeven. Zie voor informatie over het configureren van uw werkstation om uit te voeren Azure PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Een lokaal bestand uploaden naar Azure-blobopslag**

1. Open de Azure PowerShell-console volgens de instructies [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).
2. De waarden van de eerste vijf variabelen in het volgende script instellen:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext

3. Plak het script in de Azure PowerShell-console uit te voeren om het bestand te kopiëren.

PowerShell-scripts die zijn gemaakt als u wilt werken met HDInsight, Zie bijvoorbeeld [Hulpmiddelen voor HDInsight](https://github.com/blackmist/hdinsight-tools).

###<a id="azcopy"></a>AzCopy

AzCopy is een opdrachtregel hulpprogramma dat geschikt is voor de taak van het overbrengen van gegevens in- en afmelden bij een account Azure Storage vereenvoudigen. U kunt deze gebruiken als een zelfstandig hulpprogramma of kunt u dit hulpmiddel in een bestaande-toepassing. [Download AzCopy][azure-azcopy-download].

De syntaxis van de AzCopy luidt als volgt:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Zie voor meer informatie [AzCopy - bestanden voor Azure BLOB's uploaden/downloaden][azure-azcopy].


###<a id="commandline"></a>Hadoop-opdrachtregel

De opdrachtregel Hadoop is het alleen nuttig zijn voor het opslaan van gegevens in blobopslag als de gegevens al geïnstalleerd op de kop van het knooppunt is.

Pas de Hadoop-opdracht gebruiken, moet u eerst verbinding maken met de headnode met een van de volgende methoden:

* **Op basis van Windows HDInsight**: [verbinding maken met behulp van extern bureaublad](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp)

* **Linux gebaseerde HDInsight**: verbinding maken met SSH ([de opdracht SSH](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster) of [stopverf](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster))

Zodra u verbinding hebt, kunt u de volgende syntaxis naar een bestand uploaden naar opslag.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Bijvoorbeeld:`hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Omdat het standaardbestandssysteem voor HDInsight in Azure-blobopslag, zich /example/data.txt in Azure-blobopslag. U kunt ook verwijzen naar het bestand als:

    wasbs:///example/data/data.txt

of

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Zie voor een lijst met andere Hadoop-opdrachten die met bestanden werken, [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [AZURE.WARNING] Klik op HBase kolomgroepen blokkeren standaard grootte gebruikt bij het schrijven van gegevens is 256KB. Terwijl dit prima werkt wanneer u HBase APIs of REST API's gebruikt, gebruikt u de `hadoop` of `hdfs dfs` opdrachten te schrijven gegevens groter zijn dan ~ 12GB levert een fout. Zie het gedeelte [opslag uitzondering voor schrijven op blob](#storageexception) hieronder voor meer informatie.

##<a name="graphical-clients"></a>Grafische clients

Er zijn ook verschillende toepassingen die een grafische interface biedt voor het werken met Azure Storage. Hier volgt een lijst met een paar van deze toepassingen:

| Client | Linux | OS X | Windows |
| ------ |:-----:|:----:|:-------:|
| [Microsoft Visual Studio Tools voor HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) | ✔ | ✔ | ✔ |
| [Azure Opslagverkenner](http://storageexplorer.com/) | ✔ | ✔ | ✔ |
| [Cloud opslag Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | | ✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | | ✔ |
| [Azure Explorer](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | | ✔ |
| [Cyberduck](https://cyberduck.io/) |  | ✔ | ✔ |

###<a name="visual-studio-tools-for-hdinsight"></a>Visual Studio Tools for HDInsight

Zie [Navigeren van de gekoppelde bronnen](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources)voor meer informatie.

###<a id="storageexplorer"></a>Azure Opslagverkenner

*Azure opslag Explorer* is een handig hulpmiddel voor het controleren en wijzigen van de gegevens in BLOB's. Dit is een gratis, open bron-hulpprogramma dat kan worden gedownload van [http://storageexplorer.com/](http://storageexplorer.com/). De broncode is beschikbaar via deze koppeling als u ook.

Voordat u het hulpmiddel, moet u uw Azure opslag naam en account accountsleutel weten. Zie voor instructies over het aanvragen van deze gegevens de "hoe: weergave, kopiëren en opnieuw genereren opslag toegangstoetsen" gedeelte van het [maken, beheren, of een account opslagruimte verwijderen][azure-create-storage-account].  

1. Azure opslag Explorer uitvoeren. Als dit de eerste keer dat u hebt de opslagruimte Explorer hebt uitgevoerd, wordt u gevraagd voor de ___opslagaccountnaam__ en __accountsleutel opslag__. Als u hebt uitgevoerd deze vóór, gebruik de knop __toevoegen__ om een nieuwe opslagaccountnaam en -toets te voegen.

    Voer de naam en de toets voor de opslag-account door uw cluster HDinsight gebruikt en selecteer vervolgens __Opslaan en openen__.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]

5. Klik in de lijst met containers aan de linkerkant van de interface, op de naam van de container die is gekoppeld aan uw cluster HDInsight. Standaard kunt dit is de naam van het cluster HDInsight, maar mogelijk anders als u een specifieke naam hebt ingevoerd bij het maken van het cluster.

6. Selecteer het pictogram upload in de werkbalk.

    ![Werkbalk en upload-pictogram is gemarkeerd](./media/hdinsight-upload-data/toolbar.png)

7. Een bestand wilt uploaden en klik vervolgens op **openen**. Wanneer u wordt gevraagd, selecteert u __uploaden__ naar het bestand uploaden naar de hoofdsite van de container opslag. Als u het bestand uploaden naar een specifieke pad wilt, voert u het pad in __het doelveld__ en selecteer vervolgens __uploaden__.

    ![Upload-dialoogvenster voor opslaan](./media/hdinsight-upload-data/fileupload.png)
    
    Nadat het bestand is voltooid, kunt u deze kunt gebruiken via taken op het cluster HDInsight.

##<a name="mount-azure-blob-storage-as-local-drive"></a>Azure-blobopslag koppelen als lokaal station

Zie [Mountains Azure-blobopslag als lokaal station](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

##<a name="services"></a>Services

###<a name="azure-data-factory"></a>Azure gegevens Factory

De Azure gegevens Factory-service is een volledig beheerde service voor het opstellen van opslag, gegevensverwerking en gegevens verkeer gegevensservices in gestroomlijnde, scalable en betrouwbare gegevens productie pijplijnen.

Azure gegevens Factory kan worden gebruikt om gegevens te verplaatsen naar Azure-blobopslag of gegevens pijpleidingen die rechtstreeks HDInsight-functies zoals component en varken gebruiken maken.

Zie de [documentatie Factory van Azure-gegevens](https://azure.microsoft.com/documentation/services/data-factory/)voor meer informatie.

###<a id="sqoop"></a>Apache Sqoop

Sqoop is een hulpprogramma waarmee gegevens uitwisselen tussen Hadoop en relationele databases. U kunt deze gegevens importeren uit een relationele database management (RDBMS), zoals SQL Server, MySQL of Oracle in het Hadoop distributed file system (HDFS), de gegevens in Hadoop met MapReduce of component transformeren en exporteer het vervolgens de gegevens terug naar een RDBMS.

Zie voor meer informatie, [Gebruik Sqoop met HDInsight][hdinsight-use-sqoop].

##<a name="development-sdks"></a>Ontwikkeling SDK 's

Azure-blobopslag kan ook worden geopend met een SDK Azure uit de volgende talen:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Zie voor meer informatie over het installeren van de Azure SDK's [Azure-downloads](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Problemen oplossen

### <a id="storageexception"></a>Opslag uitzondering voor schrijven op blob

__Symptomen__: bij gebruik van de `hadoop` of `hdfs dfs` opdrachten bestanden die ~ 12 GB zijn te schrijven of groter op een cluster HBase u de volgende fout tegenkomen: 

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

__Oorzaak__: HBase op HDInsight serverclusters standaard naar de blokgrootte van een van 256 KB bij het schrijven naar Azure opslag. Terwijl dit voor HBase APIs of REST API's werkt, resulteert dit in een fout bij gebruik van de `hadoop` of `hdfs dfs` hulpprogramma.

__Oplossing__: Gebruik `fs.azure.write.request.size` om op te geven groter blokkeren. Kunt u dit doen op basis van de per gebruik met behulp van de `-D` parameter. De volgende is een voorbeeld met deze parameter met de `hadoop` opdracht:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

U kunt ook de waarde van vergroten `fs.azure.write.request.size` globaal met behulp van Ambari. De volgende stappen kunnen worden gebruikt om de waarde in de gebruikersinterface van de Web Ambari gewijzigd:

1. Ga naar de gebruikersinterface van de Web Ambari voor uw cluster in uw browser. Dit is https://CLUSTERNAME.azurehdinsight.net, waar __CLUSTERNAAM__ is de naam van uw cluster.

    Wanneer u wordt gevraagd, voert u de beheerdersnaam en het wachtwoord voor het cluster.

2. Vanaf de linkerkant van het scherm, selecteer __HDFS__en selecteer vervolgens het tabblad __configuraties__ .

3. Voer in het veld __filteren...__ `fs.azure.write.request.size`. Hiermee wordt het veld en de huidige waarde in het midden van de pagina weergegeven.

4. Wijzig de waarde van 262144 past (256KB) op de nieuwe waarde. Bijvoorbeeld: 4194304 (4MB).

![Afbeelding van het wijzigen van de waarde tot en met Ambari Web UI](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Zie voor meer informatie over het gebruik van Ambari [HDInsight beheren clusters met de gebruikersinterface van de Web Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Volgende stappen

Nu dat u hoe weet u gegevens in HDInsight, leest u de volgende artikelen om te leren analyses uitvoeren:

* [Aan de slag met Azure HDInsight][hdinsight-get-started]
* [Via programmacode Hadoop taken indienen][hdinsight-submit-jobs]
* [Component gebruiken met HDInsight][hdinsight-use-hive]
* [Varken met HDInsight gebruiken][hdinsight-use-pig]




[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]: ../storage/storage-create-storage-account.md
[azure-azcopy-download]: ../storage/storage-use-azcopy.md
[azure-azcopy]: ../storage/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: ../powershell-install-configure.md

[azurecli]: ../xplat-cli-install.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
