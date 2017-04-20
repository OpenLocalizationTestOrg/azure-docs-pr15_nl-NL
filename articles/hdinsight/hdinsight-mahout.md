<properties
    pageTitle="Aanbevelingen met Mahout en WIndows gebaseerde HDInsight genereren | Microsoft Azure"
    description="Informatie over het gebruik van de Apache Mahout-machine learning-bibliotheek te genereren film aanbevelingen met Windows gebaseerde HDInsight (Hadoop)."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight"></a>Film aanbevelingen genereren met behulp van Apache Mahout met Hadoop in HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Informatie over het gebruik van de [Apache Mahout](http://mahout.apache.org) -machine learning-bibliotheek met Azure HDInsight film aanbevelingen genereren.

> [AZURE.NOTE] De stappen in dit document moet een Windows-client en een cluster HDInsight op basis van Windows. Zie voor informatie over het gebruik van Mahout uit een Linux, OS X of Unix-client, met een cluster Linux gebaseerde HDInsight [genereren film aanbevelingen met behulp van Apache Mahout met Linux gebaseerde Hadoop in HDInsight](hdinsight-hadoop-mahout-linux-mac.md)


##<a name="learn"></a>Wat u leert

Mahout is een [machine learning] [ ml] bibliotheek voor Apache Hadoop. Mahout bevat algoritmen voor het verwerken van gegevens, zoals filteren, indelen, en cluster. In dit artikel, kunt u een aanbeveling-engine wilt gebruiken voor het genereren van de film aanbevelingen die zijn gebaseerd op films die uw vrienden hebt gezien. Ook leert u hoe u classificaties met een bos beslissing uitvoert. Hier wordt uitgelegd hoe u het volgende:

* Mahout taken uitvoeren met behulp van Windows PowerShell

* Mahout taken uitvoeren vanaf de opdrachtregel Hadoop

* Het installeren van Mahout op HDInsight 3.0- en HDInsight 2.0 clusters

    > [AZURE.NOTE] Mahout wordt geleverd bij de versie 3.1 HDInsight clusters. Als u een eerdere versie van HDInsight gebruikt, raadpleegt u [Mahout installeren](#install) voordat u gaat u verder.

##<a name="prerequisites"></a>vereisten voor

- **Hadoop op basis van een Windows-cluster in HDInsight**. Zie voor informatie over het maken van een [aan de slag met Hadoop in HDInsight][getstarted]
- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a name="recommendations"></a>Aanbevelingen genereren met Windows PowerShell

> [AZURE.NOTE] Hoewel de taak wordt gebruikt in deze sectie werkt met Windows PowerShell, veel van de klassen Mahout voorzien momenteel werken niet met Windows PowerShell en moeten worden uitgevoerd met behulp van de opdrachtregel Hadoop. Zie de sectie [Probleemoplossing](#troubleshooting) voor een lijst met klassen die niet met Windows PowerShell werken.
>
> Zie voor een voorbeeld van het gebruik van de opdrachtregel Hadoop Mahout taken uitvoeren, [Classify gegevens met behulp van de opdrachtregel Hadoop](#classify).

Een van de functies die is verstrekt door Mahout is een engine aanbeveling. Deze engine accepteert gegevens in de notatie van `userID`, `itemId`, en `prefValue` (gebruikers van de voorkeur voor het item). Mahout kunt vervolgens uitvoeren voor collega exemplaar analyse om te bepalen: _gebruikers die een voorkeur voor een item heeft ook een voorkeur voor deze andere items bevatten_. Mahout bepaalt vervolgens gebruikers met vind ik leuk-item-voorkeuren, kunnen worden gebruikt om aanbevelingen.

Hier volgt een zeer eenvoudige voorbeeld met films:

* __CO exemplaar__: Jan, Lisa en Stefan leuk vindt _sterretje oorlogen_ _Terug ontvangt in het imperium_en _retourneren van het Jedi_. Mahout bepaalt dat gebruikers die een van deze films ook tevreden bent over de andere twee.

* __CO exemplaar__: Bob en Lisa ook leuk vindt _Het Phantom Menace_, _aanval van de klonen_en _Revenge van de Sith_. Mahout bepaalt dat gebruikers die de vorige drie films ook leuk vindt tevreden bent over deze drie.

* __Overeenkomsten aanbeveling__: omdat Jan de eerste drie films leuk vindt, Mahout analyseert films die anderen met soortgelijke voorkeuren leuk vindt, maar Jan heeft niet die u wilt controleren (leuk vindt/geclassificeerd). In dit geval aanbevolen Mahout _De Phantom Menace_, _aanval van de klonen_en _Revenge van de Sith_.

###<a name="understanding-the-data"></a>Informatie over de gegevens

Gemakkelijk [GroupLens onderzoek] [ movielens] classificatie-gegevens voor films in een indeling die compatibel is met Mahout bevat. Deze gegevens zijn beschikbaar op van uw cluster standaard opslagruimte met `/HdiSamples/MahoutMovieData`.

Er zijn twee bestanden, `moviedb.txt` (informatie over de films) en `user-ratings.txt`. De gebruiker-ratings.txt-bestand wordt gebruikt tijdens analyse, terwijl moviedb.txt wordt gebruikt om aan te bieden van gebruikersvriendelijke tekst gegevens bij het weergeven van de resultaten van de analyse.

De gegevens in de gebruiker-ratings.txt heeft een structuur van `userID`, `movieID`, `userRating`, en `timestamp`, waarin wordt uitgelegd ons hoe ten zeerste elke gebruiker een film geclassificeerd. Hier volgt een voorbeeld van de gegevens:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

###<a name="run-the-job"></a>De taak uitvoeren

Gebruik de volgende Windows PowerShell-script om een taak die de Mahout aanbeveling-engine met de filmgegevens gebruikt te voeren:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get HTTPS/Admin credentials for submitting the job later
    $creds = Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
            
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
            
    # NOTE: The version number in the file path
    # may change in future versions of HDInsight.
    $jarFile =  "file:///C:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar"
    #
    # If you are using an earlier version of HDInsight,
    # set $jarFile to the jar file you
    # uploaded.
    # For example,
    # $jarFile = "wasbs:///example/jars/mahout-core-0.9-job.jar"

    # The arguments for this job
    # * input - the path to the data uploaded to HDInsight
    # * output - the path to store output data
    # * tempDir - the directory for temp files
    $jobArguments = "--similarityClassname", "recommenditembased", `
                    "-s", "SIMILARITY_COOCCURRENCE", `
                    "--input", "wasbs:///HdiSamples/MahoutMovieData/user-ratings.txt",
                    "--output", "wasbs:///example/out",
                    "--tempDir", "wasbs:///example/temp"

    # Create the job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
      -JarFile $jarFile `
      -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
      -Arguments $jobArguments

    # Start the job
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    # Wait on the job to complete
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
            -Blob example/out/part-r-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
            
    # Write out any error information
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

> [AZURE.NOTE] Mahout taken verwijderd tijdelijke gegevens die zijn gemaakt tijdens het verwerken van de taak niet. De `--tempDir` parameter wordt opgegeven in de taak voorbeeld te isoleren van de tijdelijke bestanden in een specifieke map.

De taak Mahout weer niet de uitvoer STDOUT. In plaats daarvan deze opgeslagen in de uitvoermap opgegeven als __onderdeel-r-00000__. Het script is dit bestand gedownload naar __uitvoer.txt__ in de huidige map op uw werkstation.

Hier volgt een voorbeeld van de inhoud van dit bestand:

    1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

De eerste kolom is de `userID`. De waarden in ' [' en ']' zijn `movieId`:`recommendationScore`.

###<a name="view-the-output"></a>De uitvoer weergeven

Hoewel de gegenereerde uitvoer mogelijk OK voor gebruik in een toepassing, is het niet erg leesbaar. De `moviedb.txt` van de server kan worden gebruikt voor het oplossen van het `movieId` naar een film naam, maar u moet eerst downloaden en het bestand classificaties van de server met het volgende script:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
    #Download the files
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/moviedb.txt" `
    -Container $container `
    -Destination moviedb.txt `
    -Context $context
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/user-ratings.txt" `
    -Container $container `
    -Destination user-ratings.txt `
    -Context $context

Nadat u de bestanden hebt gedownload, gebruik u het volgende PowerShell-script om aanbevelingen met namen van de film weer te geven:

    <#
    .SYNOPSIS
        Displays recommendations for movies.
    .DESCRIPTION
        Displays recommendations generated by Mahout
        with HDInsight example in a human readable format.
    .EXAMPLE
        .\Show-Recommendation -userId 4
            -userDataFile "user-ratings.txt"
            -movieFile "moviedb.txt"
            -recommendationFile "output.txt"
    #>

    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
        #The user ID
        [Parameter(Mandatory = $true)]
        [String]$userId,

        [Parameter(Mandatory = $true)]
        [String]$userDataFile,

        [Parameter(Mandatory = $true)]
        [String]$movieFile,

        [Parameter(Mandatory = $true)]
        [String]$recommendationFile
    )
    # Read movie ID & description into hash table
    Write-Host "Reading movies descriptions" -ForegroundColor Green
    $movieById = @{}
    foreach($line in Get-Content $movieFile)
    {
        $tokens = $line.Split("|")
        $movieById[$tokens[0]] = $tokens[1]
    }
    # Load movies user has already seen (rated)
    # into a hash table
    Write-Host "Reading rated movies" -ForegroundColor Green
    $ratedMovieIds = @{}
    foreach($line in Get-Content $userDataFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            # Resolve the ID to the movie name
            $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
        }
    }
    # Read recommendations generated by Mahout
    Write-Host "Reading recommendations" -ForegroundColor Green
    $recommendations = @{}
    foreach($line in get-content $recommendationFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            #Trim leading/treailing [] and split at ,
            $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
            foreach($movieIdAndScore in $movieIdAndScores)
            {
                #Split at : and store title and score in a hash table
                $idAndScore = $movieIdAndScore.Split(":")
                $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
            }
            break
        }
    }

    Write-Host "Rated movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                   @{Expression={$_.Value};Label="Rating"}
    $ratedMovieIds | format-table $ratedFormat
    Write-Host "---------------------------" -ForegroundColor Green

    write-host "Recommended movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                            @{Expression={$_.Value};Label="Score"}
    $recommendations | format-table $recommendationFormat

Hier volgt een voorbeeld van het script uitvoeren:

    PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt

De uitvoer moet worden de volgende strekking weergegeven:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237  

##<a name="classify"></a>Gegevens met behulp van de opdrachtregel Hadoop classificeren

Een van de classificatiemethoden beschikbaar voor communicatie met Mahout is het bouwen van een [willekeurig bos][forest]. Dit is een meerdere stappen die heeft betrekking op het gebruik van trainingsgegevens om te genereren beslissingsbomen, die worden gebruikt om te classificeren gegevens. Hiermee worden de klasse __org.apache.mahout.classifier.df.tools.Describe__ is verstrekt door Mahout gebruikt. Deze moet momenteel worden uitgevoerd met behulp van de opdrachtregel Hadoop.

###<a name="load-the-data"></a>De gegevens te laden

1. De volgende bestanden downloaden van [het NSL-KDD gegevensset](http://nsl.cs.unb.ca/NSL-KDD/).

  * [KDDTrain +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff): het bestand training

  * [KDDTest +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff): de testgegevens

2. Elk bestand openen en verwijder de regels aan de bovenkant die beginnen met '@', en slaat u de bestanden. Als deze niet worden verwijderd, ontvangt u foutberichten worden weergegeven wanneer u deze gegevens met Mahout.

2. Upload de bestanden __naar/Voorbeeldgegevens__. U kunt dit doen met behulp van de volgende script. __CLUSTERNAAM__ vervangen door de naam van de cluster HDInsight. BESTANDSNAAM vervangen door de naam voor het bestand te uploaden.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName="CLUSTERNAME"
        $fileToUpload="FILENAME"
        $blobPath="example/data/FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context

###<a name="run-the-job"></a>De taak uitvoeren

1. Deze taak is vereist voor de opdrachtregel Hadoop. Extern bureaublad voor het cluster HDInsight inschakelen en klik verbinden volgens de instructies in [verbinding maken met HDInsight clusters RDP gebruiken](hdinsight-administer-use-management-portal.md#rdp).

3. Nadat u verbinding maakt, gebruikt u de __opdrachtregel Hadoop__ -pictogram om te openen de opdrachtregel Hadoop:

    ![hadoop cli][hadoopcli]

3. Gebruik de volgende opdracht uit om te genereren van het bestandsbeschrijving (__KDDTrain + .info__), waarbij gebruik Mahout wordt.

        hadoop jar "c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasbs:///example/data/KDDTrain+.arff" -f "wasbs:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

    De `N 3 C 2 N C 4 N C 8 N 2 C 19 N L` beschrijft de kenmerken van de gegevens in het bestand. L geeft bijvoorbeeld een label aan.

4. Een bos beslissing bomen maken met behulp van de volgende opdracht uit:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasbs:///example/data/KDDTrain+.arff -ds wasbs:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    De uitvoer van deze bewerking wordt opgeslagen in de directory __nsl-bos__ , dat in de opslagruimte voor uw cluster HDInsight bij __ wasbs://user/ bevindt zich&lt;gebruikersnaam > / nsl-bos/nsl-forest.seq. De &lt;gebruikersnaam > is de naam van de gebruiker die u voor uw extern bureaublad-sessie gebruikt. Dit bestand is niet gelezen door mensen.

5. Test de bos door de gegevensset __KDDTest + .arff__ classificeren. Gebruik de volgende opdracht uit:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasbs:///example/data/KDDTest+.arff -ds wasbs:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasbs:///example/data/predictions

    Deze opdracht het resultaat samenvatting van de gegevens over de classificatieproces ongeveer als volgt uit:

        14/07/02 14:29:28 INFO mapreduce.TestForest:

        =======================================================
        Summary
        -------------------------------------------------------
        Correctly Classified Instances          :      17560       77.8921%
        Incorrectly Classified Instances        :       4984       22.1079%
        Total Classified Instances              :      22544

        =======================================================
        Confusion Matrix
        -------------------------------------------------------
        a       b       <--Classified as
        9437    274      |  9711        a     = normal
        4710    8123     |  12833       b     = anomaly

        =======================================================
        Statistics
        -------------------------------------------------------
        Kappa                                       0.5728
        Accuracy                                   77.8921%
        Reliability                                53.4921%
        Reliability (standard deviation)            0.4933

  Deze taak maakt ook het bestand zich bevindt __wasbs:///example/data/predictions/KDDTest+.arff.out__. Dit bestand is echter niet gelezen door mensen.

> [AZURE.NOTE] Mahout taken overschrijven niet bestanden. Als u deze taken opnieuw uitvoeren wilt, moet u de bestanden die zijn gemaakt door vorige taken verwijderen.

##<a name="troubleshooting"></a>Problemen oplossen

###<a name="install"></a>Mahout installeren

Mahout op HDInsight 3.1 clusters is geïnstalleerd en deze kan worden geïnstalleerd handmatig op HDInsight 3.0 of HDInsight 2.1 clusters met behulp van de volgende stappen uit:

1. De versie van Mahout gebruiken, is afhankelijk van de HDInsight-versie van uw cluster. U kunt de versie cluster vinden door te bekijken van de eigenschappen van het cluster in de portal van Azure.

  * __Voor HDInsight 2.1__, kunt u een Java-archief (oppervlak)-bestand met [Mahout 0,9](http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar)downloaden.

  * __Voor HDInsight 3.0__, moet u [Mahout uit de bron maken] [ build] en geef de Hadoop-versie die is verstrekt door HDInsight. De vereisten die wordt vermeld op de pagina opbouwen installeert, downloadt u de bron en gebruikt u de volgende opdracht uit om de Mahout oppervlak-bestanden te maken:

            mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

        After the build completes, you can find the JAR file at __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__.

        > [AZURE.NOTE] Bij Mahout 1.0 wordt uitgebracht, moet u mogelijk zijn de vooraf gedefinieerde pakketten gebruiken met HDInsight 3.0.

2. Het oppervlak-bestand uploaden naar __voorbeeld/potten__ in de standaard-opslagruimte voor uw cluster. CLUSTERNAAM in het volgende script vervangen door de naam van uw cluster HDInsight en bestandsnaam vervangen door het pad naar het bestand __mahout-coure-0,9-job.jar__ ...

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName = "CLUSTERNAME"
        $fileToUpload = "FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob "example/jars/mahout-core-0.9-job.jar" `
            -Container $container `
            -Context $context

###<a name="cannot-overwrite-files"></a>Kan bestanden niet overschrijven

Ga op mahout taken niet opschonen tijdelijke bestanden die zijn gemaakt tijdens het verwerken. Daarnaast wordt een bestaande uitvoerbestand niet overschreven door de taken.

Fouten bij het uitvoeren van taken Mahout vermijden, tijdelijke- en uitvoerbereik bestanden tussen wordt uitgevoerd verwijderen of unieke tijdelijke- en uitvoerbereik mapnamen gebruiken.

###<a name="cannot-find-the-jar-file"></a>Het oppervlak-bestand vinden niet

HDInsight 3.1 clusters opnemen Mahout. Het pad en de bestandsnaam opnemen het versienummer van Mahout dat op het cluster is geïnstalleerd. De Windows PowerShell-voorbeeldscript in deze zelfstudie gebruikt een pad dat geldig als November 2015, maar het versienummer wordt gewijzigd in toekomstige updates met HDInsight. Gebruik de volgende Windows PowerShell-opdracht om te bepalen de huidige pad naar het bestand Mahout JAR voor uw cluster, en wijzig het script om te verwijzen naar het bestandspad die wordt geretourneerd:

    Use-AzureRmHDInsightCluster -ClusterName $clusterName
    #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "wasbs:///example/statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'

###<a name="nopowershell"></a>Klassen die niet met Windows PowerShell werken

Mahout-functies die gebruikmaken van de volgende klassen retourneren allerlei foutberichten als ze worden gebruikt in Windows PowerShell:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Verbinding maken met de cluster HDInsight wilt uitvoeren door functies die gebruikmaken van deze klassen, en de taken uitvoeren via de opdrachtregel Hadoop. Zie [Classify gegevens via de opdrachtregel Hadoop](#classify) voor een voorbeeld.

##<a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u Mahout gebruikt, kunt u andere manieren werken met gegevens in HDInsight ontdekken:

* [Component met HDInsight](hdinsight-use-hive.md)
* [Varken met HDInsight](hdinsight-use-pig.md)
* [MapReduce met HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: ../powershell-install-configure.md
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
