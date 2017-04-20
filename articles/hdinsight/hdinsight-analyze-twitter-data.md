<properties
    pageTitle="Twitter gegevens analyseren met Hadoop in HDInsight | Microsoft Azure"
    description="Leer hoe u de component gebruiken om Twitter-gegevens op Hadoop in HDInsight om te vinden van de frequentie van het gebruik van een bepaald woord te analyseren."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Gegevens te analyseren Twitter met component in HDInsight

Sociale websites zijn een van de belangrijkste groot kracht voor goedkeuring van groot-gegevens. Openbare API's dat is verstrekt door sites zoals Twitter zijn handig gegevensbron voor het analyseren en informatie over populaire trends. In deze zelfstudie wordt u tweets verkrijgen met behulp van een Twitter streaming API en gebruikt u Apache component in Azure HDInsight om een lijst met Twitter-gebruikers die de meeste tweets die deel uitmaakt van een bepaald woord verzonden.

> [AZURE.NOTE] De stappen in dit document moet een cluster HDInsight op basis van Windows. Zie voor specifieke stappen voor een cluster Linux gebaseerde, [Twitter-analyseren gegevens met component in HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).


##<a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een werkstation** met Azure PowerShell geïnstalleerd en geconfigureerd. 

    Als u wilt uitvoeren in Windows PowerShell-scripts, moet u Azure PowerShell als administrator uitvoeren en instellen van het beleid kan worden uitgevoerd op *RemoteSigned*. Zie [uitvoeren Windows PowerShell-scripts][powershell-script].

    Voordat u Windows PowerShell-scripts, zorg dat er verbinding is met uw Azure-abonnement met behulp van de volgende cmdlet:

        Login-AzureRmAccount

    Als u meerdere Azure abonnementen hebt, gebruikt u de volgende cmdlet voor het instellen van het huidige abonnement:

        Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Een Azure HDInsight cluster**. Zie voor instructies voor het inrichten van cluster, [aan de slag met HDInsight] [ hdinsight-get-started] of [inrichten HDInsight clusters] [hdinsight-provision]. Verderop in deze zelfstudie moet u de naam van de cluster.

De volgende tabel bevat de bestanden die in deze zelfstudie gebruikt:

Bestanden|Beschrijving
---|---
/tutorials/Twitter/Data/tweets.txt|De brongegevens voor de taak component.
/tutorials/Twitter/output|De uitvoermap voor de taak component. De component taak uitvoer standaardbestandsnaam is **000000_0**.
tutorials/Twitter/Twitter.hql|De HiveQL script-bestand.
/tutorials/Twitter/JobStatus|De status van de Hadoop-taak.


##<a name="get-twitter-feed"></a>Get-Twitter-feed

In deze zelfstudie gebruikt u de [Twitter-streaming API's][twitter-streaming-api]. De specifieke Twitter-API gewenste streaming [statussen/filter]is[twitter-statuses-filter].

>[AZURE.NOTE] Een bestand met 10.000 tweets en de component script-bestand (in het volgende gedeelte behandeld) zijn geüpload in een openbare Blob container. Als u wilt de geüploade bestanden gebruiken, kunt u dit gedeelte overslaan.

[Tweets gegevens](https://dev.twitter.com/docs/platform-objects/tweets) worden opgeslagen in de notatie voor JavaScript-Object (JSON)-indeling met een complexe geneste structuur. In plaats van een groot aantal regels code met behulp van een gebruikelijke programmeertaal schrijven, kunt u deze geneste structuur in een tabel component transformeren zodat deze kan worden opgevraagd door een taal SQL (Structured Query)-zoals taal HiveQL genoemd.

Twitter gebruikt OAuth geautoriseerde toegang geven tot de API. OAuth is een verificatieprotocol waarmee gebruikers kunnen goedkeuren namens hun zonder het delen van hun wachtwoord-toepassingen. Meer informatie vindt u op mijn [oauth.net](http://oauth.net/) of in de bronnen met uitstekende [basisinformatie over OAuth](http://hueniverse.com/oauth/) uit Hueniverse.

De eerste stap bij het OAuth gebruiken is het opzetten van een nieuwe toepassing op de site Twitter-ontwikkelaars.

**Een Twitter-toepassing maken**

1. Meld u aan bij [https://apps.twitter.com/](https://apps.twitter.com/). Klik op de koppeling **Nu registreren** als u geen Twitter-account hebt.
2. Klik op **nieuwe App maakt**.
3. Voer **de naam**en **Beschrijving**,- **Website**. U kunt maximaal een URL voor het veld **Website** doen. De volgende tabel ziet u enkele voorbeeldwaarden gebruiken:

    Veld|Waarde
    ---|---
    Naam|MyHDInsightApp
    Beschrijving|MyHDInsightApp
    Website|http://www.myhdinsightapp.com

4. **Ja, ik ga akkoord**controleren en klik op **uw Twitter-toepassing maken**.
5. Klik op het tabblad **machtigingen** . De standaardmachtiging is **alleen-lezen**. Dit is voldoende voor deze zelfstudie.
6. Klik op het tabblad **toetsen en Access Tokens** .
7. Klik op **Mijn toegangstoken maken**.
8. Klik op **Testen OAuth** in de rechterbovenhoek van de pagina.
9. Noteer **consumenten-toets**, **consumentgeheim** **toegangstoken**en **Access token geheim**. Verderop in deze zelfstudie moet u de waarden.

In deze zelfstudie wordt u Windows PowerShell gebruiken om de webservice bellen. Zie voor een voorbeeld .NET C# [realtime Twitter-sentiment analyseren met HBase in HDInsight][hdinsight-hbase-twitter-sentiment]. Het andere populaire hulpmiddel web service gesprekken is [*omslaan*][curl]. Krul kan worden gedownload van [hier][curl-download].

>[AZURE.NOTE] Wanneer u de opdracht krul in Windows gebruiken, gebruikt u dubbele aanhalingstekens is geplaatst in plaats van enkele aanhalingstekens voor de optiewaarden.

**Tweets ophalen**

1. Open de Windows PowerShell geïntegreerd uitvoeren van scripts-omgeving (wissen). (In het scherm Start van Windows 8 typt u **PowerShell_ISE** en klik op **Windows filter wissen**. Zie [Windows PowerShell in Windows 8 en Windows starten][powershell-start].)

2. Kopieer de volgende script naar het deelvenster script:

        #region - variables and constants
        $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

        # Enter the OAuth information for your Twitter application
        $oauth_consumer_key = "<TwitterAppConsumerKey>";
        $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
        $oauth_token = "<TwitterAppAccessToken>";
        $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

        $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

        $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
        $track = [System.Uri]::EscapeDataString($trackString);
        $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
        #endregion

        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        Login-AzureRmAccount
        #endregion

        #region - Create a block blob object for writing tweets into Blob storage
        Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $containerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

        Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
        Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

        Write-Host "Create block blob object ..." -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($containerName)
        $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        #end region

        # region - Format OAuth strings
        Write-Host "Format oauth strings ..." -ForegroundColor Green
        $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
        $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
        $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

        $signature = "POST&";
        $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
        $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
        $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
        $signature += [System.Uri]::EscapeDataString("track=" + $track);

        $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

        $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
        $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
        $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

        $oauth_authorization = 'OAuth ';
        $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
        $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
        $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
        $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
        $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
        $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
        $oauth_authorization += 'oauth_version="1.0"';

        $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
        #endregion

        #region - Read tweets
        Write-Host "Create HTTP web request ..." -ForegroundColor Green
        [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
        $request.Method = "POST";
        $request.Headers.Add("Authorization", $oauth_authorization);
        $request.ContentType = "application/x-www-form-urlencoded";
        $body = $request.GetRequestStream();

        $body.write($post_body, 0, $post_body.length);
        $body.flush();
        $body.close();
        $response = $request.GetResponse() ;

        Write-Host "Start stream reading ..." -ForegroundColor Green

        Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream

        $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

        $inrec = $sReader.ReadLine()
        $count = 0
        while (($inrec -ne $null) -and ($count -le $lineMax))
        {
            if ($inrec -ne "")
            {
                Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

                $writeStream.WriteLine($inrec)
                $count ++
            }

            $inrec=$sReader.ReadLine()
        }
        #endregion

        #region - Write tweets to Blob storage
        Write-Host "Write to the destination blob ..." -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $destBlob.UploadFromStream($memStream)

        $sReader.close()
        #endregion

        Write-Host "Completed!" -ForegroundColor Green

3. Stel de eerste vijf tot acht variabelen in het script in:


    Variabele|Beschrijving
    ---|---
    $clusterName|Dit is de naam van het HDInsight cluster waar u de toepassing wordt uitgevoerd.
    $oauth_consumer_key|Dit is de Twitter-toepassing **consumenten toets** die u genoteerd eerder bij het maken van de Twitter-toepassing.
    $oauth_consumer_secret|Dit is het Twitter-toepassing **consumentgeheim** dat u zich eerder genoteerd.
    $oauth_token|Dit is de Twitter-toepassing **toegangstoken** die u zich eerder genoteerd.
    $oauth_token_secret|Dit is het Twitter-toepassing **toegang token geheim** dat u zich eerder genoteerd.
    $destBlobName|Dit is de naam van de blob uitvoer. De standaardwaarde is **tutorials/twitter/data/tweets.txt**. Als u de standaardwaarde wijzigt, moet u bij de Windows PowerShell-scripts dienovereenkomstig gewijzigd.
    $trackString|De webservice retourneert tweets die betrekking hebben op deze trefwoorden. De standaardwaarde is **Azure, Cloud, HDInsight**. Als u de standaardwaarde verandert, verandert u de Windows PowerShell-scripts dienovereenkomstig gewijzigd.
    $lineMax|De waarde bepaalt hoeveel tweets het script wordt gelezen. Het duurt ongeveer drie minuten 100 tweets lezen. U kunt instellen dat een hoog getal, maar het duurt langer om te downloaden.

5. Druk op **F5** het script uitvoeren. Als u problemen als een tijdelijke oplossing ondervindt, selecteert u alle regels en druk vervolgens op **F8**.
6. Ziet u "Voltooid!" aan het einde van de uitvoer. Foutberichten wordt weergegeven in het rood.

Als een validatieprocedure, kunt u het uitvoerbestand, **/tutorials/twitter/data/tweets.txt**, controleren of uw Azure-blobopslag met behulp van een explorer Azure opslag of Azure PowerShell. Zie voor een voorbeeld Windows PowerShell-script voor het aanbieden van bestanden, [Gebruik blobopslag met HDInsight][hdinsight-storage-powershell].



##<a name="create-hiveql-script"></a>HiveQL script maken

Azure PowerShell gebruikt, kunt u meerdere HiveQL instructies één tegelijkertijd worden uitgevoerd, of de instructie HiveQL in een scriptbestand als pakket inpakken. In deze zelfstudie maakt u een script HiveQL. Het scriptbestand moet worden geüpload naar Azure-blobopslag. In de volgende sectie, wordt u het scriptbestand uitvoeren met behulp van Azure PowerShell.

>[AZURE.NOTE] Het component script-bestand en een bestand met 10.000 tweets zijn geüpload in een openbare Blob container. Als u wilt de geüploade bestanden gebruiken, kunt u dit gedeelte overslaan.

Het script HiveQL wordt als volgt te werk:

1. **De tabel tweets_raw neerzetten** in geval de tabel bestaat al.
2. **De tweets_raw component-tabel maken**. Deze tijdelijke component gestructureerde tabel staan de gegevens voor verdere uitpakken, transformeren en laden (ETL) verwerking. Zie voor informatie over partities, [component zelfstudie][apache-hive-tutorial].  
3. **Laden gegevens** uit de bronmap, /tutorials/twitter/data. De gegevensset grote tweets in geneste JSON-indeling heeft nu veranderd in een tijdelijke component tabelstructuur.
3. **De tabel tweets neerzetten** in geval de tabel bestaat al.
4. **De tabel tweets maken**. Voordat u een query ten opzichte van de gegevensset tweets uitvoert kunt met behulp van component, moet u een ander ETL proces uitvoeren. Dit proces ETL Hiermee definieert u een tabelschema voor meer gedetailleerde voor de gegevens die u hebt opgeslagen in de tabel "twitter_raw".  
5. **Overschrijven tabel invoegen**. Dit complexe component script wordt een set lange MapReduce taken starten door het cluster Hadoop. Afhankelijk van uw gegevensset en de grootte van uw cluster, kan dit ongeveer 10 minuten duren.
6. **Invoegen map overschrijven**. Een query uitvoeren en uitvoer van de gegevensset naar een bestand. Deze query retourneert een lijst met Twitter-gebruikers die de meeste tweets of het woord "Azure".

**Maken van een script component en uploadt dit naar Azure**

1. Open Windows filter wissen.
2. Kopieer de volgende script naar het deelvenster script:

        #region - variables and constants
        $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
        $subscriptionID = "<Azure Subscription ID>"
        
        $sourceDataPath = "/tutorials/twitter/data"
        $outputPath = "/tutorials/twitter/output"
        $hqlScriptFile = "tutorials/twitter/twitter.hql"
        
        $hqlStatements = @"
        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        
        DROP TABLE tweets_raw;
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '$sourceDataPath';
        
        DROP TABLE tweets;
        CREATE TABLE tweets
        (
            id BIGINT,
            created_at STRING,
            created_at_date STRING,
            created_at_year STRING,
            created_at_month STRING,
            created_at_day STRING,
            created_at_time STRING,
            in_reply_to_user_id_str STRING,
            text STRING,
            contributors STRING,
            retweeted STRING,
            truncated STRING,
            coordinates STRING,
            source STRING,
            retweet_count INT,
            url STRING,
            hashtags array<STRING>,
            user_mentions array<STRING>,
            first_hashtag STRING,
            first_user_mention STRING,
            screen_name STRING,
            name STRING,
            followers_count INT,
            listed_count INT,
            friends_count INT,
            lang STRING,
            user_location STRING,
            time_zone STRING,
            profile_image_url STRING,
            json_response STRING
        );
        
        FROM tweets_raw
        INSERT OVERWRITE TABLE tweets
        SELECT
            cast(get_json_object(json_response, '$.id_str') as BIGINT),
            get_json_object(json_response, '$.created_at'),
            concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
            substr (get_json_object(json_response, '$.created_at'),27,4)),
            substr (get_json_object(json_response, '$.created_at'),27,4),
            case substr (get_json_object(json_response, '$.created_at'),5,3)
                when "Jan" then "01"
                when "Feb" then "02"
                when "Mar" then "03"
                when "Apr" then "04"
                when "May" then "05"
                when "Jun" then "06"
                when "Jul" then "07"
                when "Aug" then "08"
                when "Sep" then "09"
                when "Oct" then "10"
                when "Nov" then "11"
                when "Dec" then "12" end,
            substr (get_json_object(json_response, '$.created_at'),9,2),
            substr (get_json_object(json_response, '$.created_at'),12,8),
            get_json_object(json_response, '$.in_reply_to_user_id_str'),
            get_json_object(json_response, '$.text'),
            get_json_object(json_response, '$.contributors'),
            get_json_object(json_response, '$.retweeted'),
            get_json_object(json_response, '$.truncated'),
            get_json_object(json_response, '$.coordinates'),
            get_json_object(json_response, '$.source'),
            cast (get_json_object(json_response, '$.retweet_count') as INT),
            get_json_object(json_response, '$.entities.display_url'),
            array(
                trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
            array(
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            get_json_object(json_response, '$.user.screen_name'),
            get_json_object(json_response, '$.user.name'),
            cast (get_json_object(json_response, '$.user.followers_count') as INT),
            cast (get_json_object(json_response, '$.user.listed_count') as INT),
            cast (get_json_object(json_response, '$.user.friends_count') as INT),
            get_json_object(json_response, '$.user.lang'),
            get_json_object(json_response, '$.user.location'),
            get_json_object(json_response, '$.user.time_zone'),
            get_json_object(json_response, '$.user.profile_image_url'),
            json_response
        WHERE (length(json_response) > 500);
        
        INSERT OVERWRITE DIRECTORY '$outputPath'
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;
        "@
        #endregion
        
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        
        Try{
            Get-AzureRmSubscription
        }
        Catch{
            Login-AzureRmAccount
        }
        
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
        
        #endregion
        
        #region - Create a block blob object for writing the Hive script file
        Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $defaultBlobContainerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
        
        Write-Host "Define the connection string ..." -ForegroundColor Green
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
        Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
        $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)
        
        Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream
        $writeStream.Writeline($hqlStatements)
        #endregion
        
        #region - Write the Hive script file to Blob storage
        Write-Host "Write to the destination blob ... " -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $hqlScriptBlob.UploadFromStream($memStream)
        #endregion
        
        Write-Host "Completed!" -ForegroundColor Green

        

4. Stel de eerste twee variabelen in het script:

    Variabele|Beschrijving
    ---|---
    $clusterName|Voer de naam van het HDInsight cluster waar u de toepassing wordt uitgevoerd.
    $subscriptionID|Voer uw Azure abonnement-ID.
    $sourceDataPath|De opslaglocatie van Azure Blob de component query's leest waar de gegevens uit. U hoeft niet te wijzigen van deze variabele.
    $outputPath|De opslaglocatie van Azure Blob waar de component query's worden de resultaten. U hoeft niet te wijzigen van deze variabele.
    $hqlScriptFile|De locatie en de bestandsnaam van het HiveQL script-bestand. U hoeft niet te wijzigen van deze variabele.

5. Druk op **F5** het script uitvoeren. Als u problemen als een tijdelijke oplossing ondervindt, selecteert u alle regels en druk vervolgens op **F8**.
6. Ziet u "Voltooid!" aan het einde van de uitvoer. Foutberichten wordt weergegeven in het rood.

Als een validatieprocedure, kunt u het uitvoerbestand, **/tutorials/twitter/twitter.hql**, controleren op uw Azure-blobopslag met behulp van een explorer Azure opslag of Azure PowerShell. Zie voor een voorbeeld Windows PowerShell-script voor het aanbieden van bestanden, [Gebruik blobopslag met HDInsight][hdinsight-storage-powershell].  


##<a name="process-twitter-data-by-using-hive"></a>Proces Twitter-gegevens met behulp van component

U hebt al het voorbereidende werk. U kunt nu de component script starten en de resultaten te controleren.

### <a name="submit-a-hive-job"></a>Een taak onderdeel verzenden

Gebruik het volgende Windows PowerShell-script aan de component-script uitvoeren. U moet de eerste variabele instellen.

>[AZURE.NOTE] Als u de tweets en het HiveQL-script die u hebt geüpload in de laatste twee secties, ingesteld $hqlScriptFile "/ tutorials/twitter/twitter.hql". Met de bouwstenen die voor u zijn geüpload naar een openbare blob, stelt $hqlScriptFile op "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    $httpUserName = "admin"
    $httpUserPassword = "<HDInsight Cluster HTTP User Password>"
    
    #use one of the following
    $hqlScriptFile = "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
    $hqlScriptFile = "/tutorials/twitter/twitter.hql"
    
    $statusFolder = "/tutorials/twitter/jobstatus"
    #endregion
    
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    
    #region - Invoke Hive
    Write-Host "Invoke Hive ... " -ForegroundColor Green
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential 
    $response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable
    
    Write-Host "Display the standard error log ... " -ForegroundColor Green
    $jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
    #endregion

### <a name="check-the-results"></a>De resultaten controleren

Gebruik de volgende Windows PowerShell-script om te controleren van de uitvoer van de taak component. U moet de eerste twee variabelen instellen.

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    
    $blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
    #engregion
    
    #region - Create an Azure storage context object
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey  
    #endregion
    
    #region - Download blob and display blob
    Write-Host "Download the blob ..." -ForegroundColor Green
    cd $HOME
    Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "Display the output ..." -ForegroundColor Green
    Write-Host "==================================" -ForegroundColor Green
    cat "./$blob"
    Write-Host "==================================" -ForegroundColor Green
    #end region

> [AZURE.NOTE] De tabel component gebruikt \001 als het scheidingsteken voor velden. Het scheidingsteken is niet zichtbaar zijn in de uitvoer.

Nadat de analyseresultaten in Azure-blobopslag zijn geplaatst, kunt u de gegevens exporteren naar een Azure SQL database/SQL server, de gegevens exporteren naar Excel via Power Query of verbinding maken met uw toepassing tot de gegevens met behulp van de component ODBC-stuurprogramma. Zie voor meer informatie, [Gebruik Sqoop met HDInsight][hdinsight-use-sqoop], [analyseren flight gegevens over vertragingen met HDInsight][hdinsight-analyze-flight-delay-data], [Excel verbinden met HDInsight via Power Query][hdinsight-power-query], en [Excel verbinden met HDInsight via het Microsoft Component ODBC-stuurprogramma][hdinsight-hive-odbc].

##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt we hoe u een ongestructureerde JSON dataset transformeren in gestructureerde component tabel, query, te verkennen en analyseren van gegevens uit Twitter via HDInsight op Azure gezien. Meer informatie raadpleegt u:

- [Aan de slag met HDInsight][hdinsight-get-started]
- [Realtime Twitter sentiment met HBase in HDInsight analyseren][hdinsight-hbase-twitter-sentiment]
- [Analyseren van gegevens over vertragingen flight HDInsight gebruiken][hdinsight-analyze-flight-delay-data]
- [Excel verbinden met HDInsight via Power Query][hdinsight-power-query]
- [Verbinding maken met Excel met HDInsight via het Microsoft-Component ODBC-stuurprogramma][hdinsight-hive-odbc]
- [Sqoop gebruiken met HDInsight][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
