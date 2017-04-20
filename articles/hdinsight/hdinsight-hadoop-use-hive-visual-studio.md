<properties
   pageTitle="Query met Hadoop tools voor Visual Studio component | Microsoft Azure"
   description="Informatie over het gebruiken van component met Hadoop in HDInsight met hulpmiddelen voor Visual Studio Hadoop."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/06/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a>Component query's met de HDInsight-hulpmiddelen voor Visual Studio uitvoeren

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In dit artikel leert u hoe om in te dienen component query's aan een HDInsight cluster met behulp van de hulpmiddelen HDInsight voor Visual Studio.

> [AZURE.NOTE] In dit document biedt geen een gedetailleerde beschrijving van wat het HiveQL-instructies in de voorbeelden gebruikt doen. Voor informatie over de HiveQL die in dit voorbeeld wordt gebruikt, Zie [Gebruik component met Hadoop op HDInsight](hdinsight-use-hive.md).

##<a id="prereq"></a>Vereisten voor

U voltooit de stappen in dit artikel, moet u het volgende.

* Een cluster Azure HDInsight (Hadoop op HDInsight) (Linux of op basis van Windows)

* Visual Studio (één van de volgende versies):

    Visual Studio 2013 Community/Professional/Premium/Ultimate met [4 bijwerken](https://www.microsoft.com/download/details.aspx?id=44921)

    Visual Studio 2015 (Community/ondernemingsresources zijn)

- HDInsight hulpprogramma's voor Visual studio. Zie [aan de slag met Hadoop voor Visual Studio tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) voor informatie over het installeren en configureren van de hulpmiddelen.

##<a id="run"></a>Component query's met de HDInsight-hulpmiddelen voor Visual Studio uitvoeren

1. Open **Visual Studio** en selecteert u daarna **Nieuw** > **Project** > **HDInsight** > **Component toepassing**. Geef een naam voor dit project.

2. Open het bestand **Script.hql** die is gemaakt met dit project en plakken in de volgende HiveQL-instructies:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Deze instructies heeft de volgende acties uitvoeren:

    * **DROP TABLE**: Hiermee verwijdert u de tabel en het gegevensbestand als de tabel al bestaat.
    * **Externe tabel maken**: maakt een nieuwe 'externe' tabel in component. De definitie van de tabel externe tabellen alleen opgeslagen in component (de gegevens, is links op de oorspronkelijke locatie).

        > [AZURE.NOTE] Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens moeten worden bijgewerkt door een externe bron (zoals een geautomatiseerde gegevens uploadproces) of door een andere MapReduce bewerking verwacht, maar u wilt dat altijd component query's om de meest recente gegevens te gebruiken.
        >
        > Weghalen van een externe tabel bevat **niet** verwijderen de gegevens, alleen de definitie van de tabel.

    * **Rij-indeling**: Hiermee wordt aan component hoe de gegevens worden opgemaakt. In dit geval worden de velden in elke log gescheiden door een spatie.
    * **Opgeslagen als TEXTFILE locatie**: Hiermee wordt aan Component waar de gegevens is opgeslagen (de map met de voorbeeldgegevens /) en wordt deze opgeslagen als tekst.
    * **Selecteer**: Selecteer een telling van alle rijen waar kolom **t4** de waarde **[fout]**bevatten. Dit moet een waarde van **3** retourneren omdat er zijn drie rijen met deze waarde.
    * **INPUT__FILE__NAME zoals '%.log'** - Hiermee wordt aan die we alleen gegevens uit bestanden die eindigen ophalen moet op component. log. Hiermee beperkt u de zoekopdracht naar het bestand sample.log die de gegevens bevat, en behoudt uit retourneren van gegevens uit andere voorbeeld gegevensbestanden die niet overeenkomen met het schema dat zoals gedefinieerd.

3. Selecteer het **HDInsight Cluster** die u wilt gebruiken voor deze query uit de werkbalk en selecteer **verzenden naar WebHCat** bij het uitvoeren van de instructies als een taak, component die gebruikmaakt van WebHCat. U kunt ook de taak met behulp van de knop __uitvoeren via HiveServer2__ als HiveServer2 beschikbaar op de versie van uw cluster is indienen. De **Component samenvatting** wordt weergegeven en wordt informatie over de actieve taak. Gebruik de koppeling **vernieuwen** te vernieuwen van de taakgegevens, totdat de **Taakstatus** moet **voltooid verandert**.

4. Gebruik de **Uitvoer van de taak** -koppeling naar de weergave van de uitvoer van deze taak. Moet worden weergegeven `[ERROR] 3`, namelijk de waarde die het resultaat van de SELECT-instructie.

5. U kunt ook component query's uitvoeren zonder een project te maken. **Azure** **Server Explorer**, uitvouwen > **HDInsight**, met de rechtermuisknop op de server HDInsight en selecteer vervolgens **een Query component schrijven**.

6. In het **temp.hql** -document dat wordt weergegeven, moet u de volgende HiveQL-instructies toevoegen:

        set hive.execution.engine=tez;
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Deze instructies heeft de volgende acties uitvoeren:

    * **Maak tabel als niet bestaat**: maakt een tabel als deze nog niet bestaat. Omdat het **externe** trefwoord niet wordt gebruikt, is dit is een interne tabel, die is opgeslagen in de component datawarehouse en volledig wordt beheerd door component.

        > [AZURE.NOTE] In tegenstelling tot de **externe** tabellen verwijderd een interne tabel weghalen ook de onderliggende gegevens.

    * **Opgeslagen als ORC**: de gegevens zijn opgeslagen in geoptimaliseerde rij kolomindeling (ORC). Dit is een zeer geoptimaliseerde en efficiënt indeling voor het opslaan van de component data.
    * **Invoegen OVERSCHRIJVEN... Selecteer**: Hiermee selecteert u rijen uit de tabel **log4jLogs** met **[fout]**, klikt u vervolgens de gegevens in de tabel **errorLogs** invoegen.

7. Selecteer **verzenden** de taak uit te voeren in de werkbalk. Gebruik de **Taakstatus** om te bepalen dat de taak is voltooid.

8. **Server Explorer** gebruiken om te bevestigen dat de taak voltooid en een nieuwe tabel hebt gemaakt, en vouwen van **Azure** > **HDInsight** > uw cluster HDInsight > **Component Databases** > en **standaard**. Zie de tabel **errorLogs** en de tabel **log4jLogs** .

##<a id="summary"></a>Overzicht

Zoals u ziet, de de HDInsight tools voor Visual Studio bieden een gemakkelijke manier component query's uitvoeren op een cluster HDInsight, het controleren van de taakstatus en het ophalen van de uitvoer.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over component in HDInsight:

* [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)

* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Voor meer informatie over de HDInsight-hulpprogramma's voor Visual Studio:

* [Aan de slag met HDInsight tools voor Visual Studio](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)


[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
