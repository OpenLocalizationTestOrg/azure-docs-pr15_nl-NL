<properties
    pageTitle="Sensorgegevens te analyseren met component en Hadoop | Microsoft Azure"
    description="Meer informatie over het om sensorgegevens te analyseren met behulp van de Query component Console met HDInsight (Hadoop) en vervolgens de gegevens visualiseren in Microsoft Excel met Power View."
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
    ms.date="09/20/2016" 
    ms.author="larryfr"/>

#<a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Gebruik van de Query component Console op Hadoop in HDInsight sensorgegevens analyseren

Meer informatie over het om sensorgegevens te analyseren met behulp van de Query component Console met HDInsight (Hadoop) en vervolgens de gegevens in Microsoft Excel visualiseren met Power View.

> [AZURE.NOTE] De stappen in dit document werken alleen met Windows gebaseerde HDInsight clusters.

In dit voorbeeld gebruikt u component historische om gegevens te verwerken geproduceerd door verwarming, ventilatie en airconditioning (Aircoschema) systemen systemen die niet kunnen naar betrouwbaar voor het behoud van een set temperatuur identificeren. U leert hoe u:

- Maak component tabellen query uitvoeren op gegevens die zijn opgeslagen in bestanden met door komma's gescheiden waarden (.csv).
- Maak component query's om de gegevens te analyseren.
- Microsoft Excel gebruiken om te verbinden met HDInsight (open database connectivity (ODBC) via de geanalyseerde gegevens worden opgehaald.
- Power View gebruiken de gegevens kunt visualiseren.

![Een diagram van de oplossingsarchitectuur](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

##<a name="prerequisites"></a>Vereisten voor

* Een cluster HDInsight (Hadoop): Zie [inrichten Hadoop clusters in HDInsight](hdinsight-provision-clusters.md) voor informatie over het maken van een cluster.

* Microsoft Excel 2013

    > [AZURE.NOTE] Microsoft Excel wordt gebruikt voor weergave van gegevens met [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Microsoft-Component ODBC-stuurprogramma](http://www.microsoft.com/download/details.aspx?id=40886)

##<a name="to-run-the-sample"></a>Naar het uitvoeren van de steekproef

1. Navigeer naar de volgende URL van uw webbrowser. Vervang `<clustername>` met de naam van uw cluster HDInsight.

        https://<clustername>.azurehdinsight.net

    Wanneer u wordt gevraagd, worden geverifieerd met behulp van de beheerder-gebruikersnaam en wachtwoord die u hebt gebruikt bij de inrichting van dit cluster.

2. Klik op het tabblad **Aan de slag galerie** van de webpagina dat wordt geopend, en klik vervolgens onder de categorie **oplossingen met voorbeeldgegevens** op de steekproef **Sensor gegevensanalyse** .

    ![Aan de slag-Galerij](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. Volg de instructies op de webpagina om te voltooien van de steekproef.
