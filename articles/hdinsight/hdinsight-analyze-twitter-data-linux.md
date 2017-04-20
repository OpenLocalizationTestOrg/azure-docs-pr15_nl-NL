<properties
    pageTitle="Twitter gegevens analyseren met Apache component op HDInsight | Microsoft Azure"
    description="Leer hoe u Python gebruiken om op te slaan Tweets die specifieke trefwoorden bevatten en vervolgens op HDInsight component en Hadoop met de onbewerkte TWitter-gegevens omzetten in een doorzoekbare component-tabel."
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
    ms.date="09/27/2016"
    ms.author="larryfr"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Gegevens te analyseren Twitter met component in HDInsight

In dit document, krijgt u tweets met behulp van een Twitter streaming API en vervolgens gebruik Apache component op een cluster HDInsight Linux gebaseerde verwerking de JSON opgemaakte gegevens. Het resultaat is een lijst met Twitter-gebruikers die de meeste tweets die deel uitmaakt van een bepaald woord verzonden.

> [AZURE.NOTE] Terwijl de afzonderlijke onderdelen van dit document kunnen worden gebruikt met Windows gebaseerde HDInsight kolomgroepen (Python bijvoorbeeld) veel stappen over het gebruik van een cluster Linux gebaseerde HDInsight zijn gebaseerd. Zie voor specifieke stappen voor een cluster op basis van Windows, [Twitter-analyseren gegevens met component in HDInsight](hdinsight-analyze-twitter-data.md).

###<a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- Een __cluster Linux gebaseerde Azure HDInsight__. Zie [Aan de slag met Linux gebaseerde HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) voor stapsgewijze instructies voor het maken van een cluster voor informatie over het maken van een cluster.

- Een __SSH-client__. Zie de volgende artikelen voor meer informatie over het gebruik van SSH met Linux gebaseerde HDInsight:

    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight uit Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

- __Python__ en [pip](https://pypi.python.org/pypi/pip)

##<a name="get-a-twitter-feed"></a>Een Twitter-feed ophalen

Twitter kunt u de [gegevens voor elke tweet](https://dev.twitter.com/docs/platform-objects/tweets) als een document JavaScript Object notatie (JSON) tot en met een REST API worden opgehaald. [OAuth](http://oauth.net) is vereist voor verificatie tot de API. U moet ook een _Twitter-toepassing_ waarvan de instellingen die wordt gebruikt voor toegang tot de API maken.

###<a name="create-a-twitter-application"></a>Een Twitter-toepassing maken

1. Vanuit een webbrowser en meld u aan bij [https://apps.twitter.com/](https://apps.twitter.com/). Klik op de koppeling **Nu registreren** als u geen Twitter-account hebt.
2. Klik op **nieuwe App maakt**.
3. Voer **de naam**en **Beschrijving**,- **Website**. U kunt maximaal een URL voor het veld **Website** doen. De volgende tabel ziet u enkele voorbeeldwaarden gebruiken:

  	| Veld | Waarde |
  	|:----- |:----- |
  	| Naam  | MyHDInsightApp |
  	| Beschrijving | MyHDInsightApp |
  	| Website | http://www.myhdinsightapp.com |
    
4. **Ja, ik ga akkoord**controleren en klik op **uw Twitter-toepassing maken**.
5. Klik op het tabblad **machtigingen** . De standaardmachtiging is **alleen-lezen**. Dit is voldoende voor deze zelfstudie.
6. Klik op het tabblad **toetsen en Access Tokens** .
7. Klik op **Mijn toegangstoken maken**.
8. Klik op **Testen OAuth** in de rechterbovenhoek van de pagina.
9. Noteer **consumenten-toets**, **consumentgeheim** **toegangstoken**en **Access token geheim**. Moet u de waarden later.

>[AZURE.NOTE] Wanneer u de opdracht krul in Windows gebruiken, gebruikt u dubbele aanhalingstekens is geplaatst in plaats van enkele aanhalingstekens voor de optiewaarden.

###<a name="download-tweets"></a>Tweets downloaden

De volgende Python code 10.000 tweets gedownload van Twitter en sla deze op een bestand met de naam __tweets.txt__.

> [AZURE.NOTE] De volgende stappen worden uitgevoerd op het cluster HDInsight, omdat Python al is geïnstalleerd.

1. Verbinding maken met het HDInsight cluster SSH gebruiken:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Als u een wachtwoord beveiligen van uw gebruikersaccount SSH gebruikt, wordt u gevraagd in te voeren. Als u een openbare sleutel gebruikt, moet u mogelijk gebruiken de `-i` -parameter voor de overeenkomende persoonlijke sleutel opgeven. Bijvoorbeeld `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
        
    Zie de volgende artikelen voor meer informatie over het gebruik van SSH met Linux gebaseerde HDInsight:
    
    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight uit Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows)
    
2. Het hulpprogramma __pip__ is niet standaard geïnstalleerd op het hoofd knooppunt HDInsight. Gebruik van de volgende handelingen uit om te installeren en past u dit hulpprogramma:

        sudo apt-get install python-pip
        sudo pip install --upgrade pip

3. De code wilt downloaden van tweets, is afhankelijk van [Tweepy](http://www.tweepy.org/) en [Progressbar](https://pypi.python.org/pypi/progressbar/2.2). Als u wilt deze hebt geïnstalleerd, gebruikt u de volgende opdracht uit:

        sudo apt-get install python-dev libffi-dev libssl-dev
        sudo apt-get remove python-openssl
        sudo pip install tweepy progressbar pyOpenSSL requests[security]
        
    > [AZURE.NOTE] De bits over het verwijderen van python-openssl, installatie python-ontwikkelaar, libffi-ontwikkelaar, libssl-ontwikkelaar, pyOpenSSL en aanvragen [beveiliging] is om te voorkomen dat een InsecurePlatform-waarschuwing wanneer u verbinding maakt met Twitter via SSL in Python.
    >
    > Tweepy v3.2.0 wordt gebruikt om te voorkomen dat [een fout](https://github.com/tweepy/tweepy/issues/576) die optreden kunnen bij het verwerken van tweets.

4. Gebruik de volgende opdracht uit om te maken van een nieuw bestand met de naam __gettweets.py__:

        nano gettweets.py

5. Gebruik de volgende handelingen uit als de inhoud van het bestand __gettweets.py__ . Vervang de gegevens van de tijdelijke aanduiding voor __consumenten\_geheim__, __consumenten\_sleutel__, __access /\_token__, en __access\_token\_geheim__ met de gegevens uit uw Twitter-toepassing.

        #!/usr/bin/python

        from tweepy import Stream, OAuthHandler
        from tweepy.streaming import StreamListener
        from progressbar import ProgressBar, Percentage, Bar
        import json
        import sys

        #Twitter app information
        consumer_secret='Your consumer secret'
        consumer_key='Your consumer key'
        access_token='Your access token'
        access_token_secret='Your access token secret'

        #The number of tweets we want to get
        max_tweets=10000

        #Create the listener class that will receive and save tweets
        class listener(StreamListener):
            #On init, set the counter to zero and create a progress bar
            def __init__(self, api=None):
                self.num_tweets = 0
                self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

            #When data is received, do this
            def on_data(self, data):
                #Append the tweet to the 'tweets.txt' file
                with open('tweets.txt', 'a') as tweet_file:
                    tweet_file.write(data)
                    #Increment the number of tweets
                    self.num_tweets += 1
                    #Check to see if we have hit max_tweets and exit if so
                    if self.num_tweets >= max_tweets:
                        self.pbar.finish()
                        sys.exit(0)
                    else:
                        #increment the progress bar
                        self.pbar.update(self.num_tweets)
                return True

            #Handle any errors that may occur
            def on_error(self, status):
                print status

        #Get the OAuth token
        auth = OAuthHandler(consumer_key, consumer_secret)
        auth.set_access_token(access_token, access_token_secret)
        #Use the listener class for stream processing
        twitterStream = Stream(auth, listener())
        #Filter for these topics
        twitterStream.filter(track=["azure","cloud","hdinsight"])

6. Gebruik __Ctrl + X__en __Y__ het bestand wilt opslaan.

7. Gebruik de volgende opdracht uit te voeren van het bestand en tweets downloaden:

        python gettweets.py

    Een voortgangsindicator moet worden weergegeven en tellen van maximaal 100% wanneer de tweets worden gedownload en naar bestand opgeslagen.

    > [AZURE.NOTE] Als het duurt erg lang voor de voortgangsbalk om weer te geven, wijzigt u het filter om bij te houden trending onderwerpen; Als er een groot aantal tweets over het onderwerp dat u filtert, kunt u de 10000 tweets nodig zeer snel opvragen.

###<a name="upload-the-data"></a>De gegevens uploaden

Als u wilt de gegevens uploaden naar WASB (distributed file system die worden gebruikt door HDInsight), gebruikt u de volgende opdrachten:

    hdfs dfs -mkdir -p /tutorials/twitter/data
    hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt

Hiermee worden de gegevens opgeslagen op een locatie die toegankelijk is voor alle knooppunten in het cluster.

##<a name="run-the-hiveql-job"></a>De taak HiveQL uitvoeren


1. Gebruik de volgende opdracht uit een bestand maken met HiveQL-instructies:

        nano twitter.hql
    
    Gebruik de volgende handelingen uit als de inhoud van het bestand:

        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        -- Drop table, if it exists
        DROP TABLE tweets_raw;
        -- Create it, pointing toward the tweets logged from Twitter
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
        -- Drop and recreate the destination table
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
        -- Select tweets from the imported data, parse the JSON,
        -- and insert into the tweets table
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
        
        
3. Druk op __Ctrl + X__en druk op __Y__ het bestand wilt opslaan.

4. Gebruik de volgende opdracht uit om uit te voeren de HiveQL in het bestand staan:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i twitter.hql
        
    Hiermee wordt de component shell laden, de HiveQL uitvoeren in het bestand __twitter.hql__ en ten slotte terug een `jdbc:hive2//localhost:10001/>` prompt.
    
5. Gebruik de volgende handelingen uit om te bevestigen dat u gegevens in de tabel __tweets__ is gemaakt door de HiveQL in het bestand __twitter.hql selecteren kunt__ vanaf de prompt beeline:
        
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;

    Hiermee herstelt u maximaal 10 tweets met het woord __Azure__ in de berichttekst.

##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt we hoe u een ongestructureerde JSON dataset transformeren in gestructureerde component tabel, query, te verkennen en analyseren van gegevens uit Twitter via HDInsight op Azure gezien. Meer informatie raadpleegt u:

- [Aan de slag met HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Analyseren van gegevens over vertragingen flight HDInsight gebruiken](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
