<properties 
    pageTitle="Component gebruiken met Hadoop voor een website logboekanalyse | Microsoft Azure" 
    description="Informatie over het gebruiken van component met HDInsight te analyseren logboeken aan de website. U kunt een logboekbestand gebruiken als invoer in een tabel HDInsight en HiveQL gebruiken om de gegevens." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/17/2016" 
    ms.author="nitinme"/>

# <a name="use-hive-with-hdinsight-to-analyze-logs-from-websites"></a>Component gebruiken met HDInsight te analyseren Logboeken vanaf websites

Informatie over het gebruiken van HiveQL met HDInsight te analyseren logboeken van een website. Website logboekanalyse kan worden gebruikt voor segment van uw publiek op basis van soortgelijke activiteiten, categoriseren bezoekers door doelgroepen, en om na te gaan de inhoud die ze weergave, de websites ze afkomstig zijn uit, enzovoort.

In dit voorbeeld kunt u een cluster HDInsight wilt gebruiken voor het analyseren van de logboekbestanden website om inzicht te krijgen in de frequentie van bezoeken naar de website van externe websites in een dag. U kunt ook een overzicht van de website van fouten die de gebruikers ervaart genereren. U leert hoe u:

- Verbinding maken met een Azure-blobopslag, waarin logboekbestanden van de website.
- COMPONENT tabellen als u wilt deze logboeken query maken.
- Maak component query's om de gegevens te analyseren.
- Microsoft Excel gebruiken om te verbinden met HDInsight (via (ODBC) open database connectivity de geanalyseerde gegevens worden opgehaald.

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

##<a name="prerequisites"></a>Vereisten voor

- U moet hebben deze is ingericht een cluster Hadoop op Azure HDInsight. Zie voor instructies voor het [Inrichten HDInsight Clusters][hdinsight-provision]. 
- U moet Microsoft Excel 2013 of Excel 2010 is geïnstalleerd.
- [ODBC-stuurprogramma voor Microsoft-Component](http://www.microsoft.com/download/details.aspx?id=40886) om gegevens te importeren uit component in Excel, moet u hebben.


##<a name="to-run-the-sample"></a>Naar het uitvoeren van de steekproef

1. In de [Portal van Azure](https://portal.azure.com/)uit de Startboard (als u het cluster er vastgemaakt), klikt u op de tegel van de cluster waarop u wilt uitvoeren van de steekproef.

2. Van het blad cluster onder **Snelkoppelingen**, klik op **Dashboard Cluster**en klik vervolgens op het blad **Cluster Dashboard** op **HDInsight Cluster Dashboard**. U kunt ook rechtstreeks het dashboard openen met behulp van de volgende URL:

        https://<clustername>.azurehdinsight.net
    
    Wanneer u wordt gevraagd, worden geverifieerd met behulp van de beheerder-gebruikersnaam en wachtwoord die u hebt gebruikt bij de inrichting van het cluster.
  
2. Klik op het tabblad **Aan de slag galerie** van de webpagina dat wordt geopend, en klik vervolgens onder de categorie **oplossingen met voorbeeldgegevens** op de **Website logboekanalyse** steekproef.

3. Volg de instructies op de webpagina om te voltooien van de steekproef.

##<a name="next-steps"></a>Volgende stappen
Probeer het volgende voorbeeld: [analyseren sensorgegevens met behulp van component met HDInsight](hdinsight-hive-analyze-sensor-data.md).


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
 
