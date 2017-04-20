<properties
   pageTitle="Hadoop-component en extern bureaublad gebruiken in HDInsight | Microsoft Azure"
   description="Meer informatie over het verbinding maken met Hadoop cluster in HDInsight via Extern bureaublad en vervolgens component query's uitvoeren met behulp van de opdrachtregel component."
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

# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Component gebruiken met Hadoop op HDInsight met extern bureaublad

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In dit artikel wordt u meer informatie over het verbinding maken met een cluster HDInsight via Extern bureaublad en vervolgens component query's uitvoeren met behulp van de component opdrachtregel CLI ().

> [AZURE.NOTE] In dit document biedt geen een gedetailleerde beschrijving van wat het HiveQL-instructies die worden gebruikt in de voorbeelden doen. Voor informatie over de HiveQL die in dit voorbeeld wordt gebruikt, Zie [Gebruik component met Hadoop op HDInsight](hdinsight-use-hive.md).

##<a id="prereq"></a>Vereisten voor

U voltooit de stappen in dit artikel, moet u het volgende:

* Een cluster op basis van Windows HDInsight (Hadoop op HDInsight)

* Een clientcomputer met Windows 10, venster 8 of Windows 7

##<a id="connect"></a>Verbinding met extern bureaublad

Extern bureaublad voor het cluster HDInsight inschakelen en klik verbinden volgens de instructies in [verbinding maken met HDInsight clusters RDP gebruiken](hdinsight-administer-use-management-portal.md#rdp).

##<a id="hive"></a>Gebruik de opdracht component

Wanneer u hebt verbonden met het bureaublad voor het cluster HDInsight, gebruikt u de volgende stappen uit om te werken met onderdeel:

1. Start de **opdrachtregel Hadoop**vanaf het bureaublad HDInsight.

2. Voer de volgende opdracht uit de CLI component starten:

        %hive_home%\bin\hive

    Wanneer de CLI is gestart, ziet u de vraag component CLI: `hive>`.

3. De CLI gebruikt, voert u de volgende instructies als u wilt maken van een nieuwe tabel met de naam **log4jLogs** voorbeeldgegevens gebruikt:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Deze instructies heeft de volgende acties uitvoeren:

    * **DROP TABLE**: Hiermee verwijdert u de tabel en het gegevensbestand als de tabel al bestaat.

    * **Externe tabel maken**: maakt een nieuwe 'externe' tabel in component. Alleen de definitie van de tabel opslaan externe tabellen in component (de gegevens, is links op de oorspronkelijke locatie).

        > [AZURE.NOTE] Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens moeten worden bijgewerkt door een externe bron (zoals een geautomatiseerde gegevens uploadproces) of door een andere MapReduce bewerking verwacht, maar u wilt dat altijd component query's om de meest recente gegevens te gebruiken.
        >
        > Weghalen van een externe tabel bevat **niet** verwijderen de gegevens, alleen de definitie van de tabel.

    * **Rij-indeling**: Hiermee wordt aan component hoe de gegevens worden opgemaakt. In dit geval worden de velden in elke log gescheiden door een spatie.

    * **Opgeslagen als TEXTFILE locatie**: Hiermee wordt aan Component waar de gegevens is opgeslagen (de map met de voorbeeldgegevens /) en wordt deze opgeslagen als tekst.

    * **Selecteer**: Hiermee selecteert u een telling van alle rijen waarbij kolom **t4** de waarde **[fout bevat]**. Dit moet een waarde van **3** retourneren omdat er zijn drie rijen met deze waarde.

    * **INPUT__FILE__NAME zoals '%.log'** - Hiermee wordt aan die we alleen gegevens uit bestanden die eindigen ophalen moet op component. log. Hiermee beperkt u de zoekopdracht naar het bestand sample.log die de gegevens bevat, en behoudt uit retourneren van gegevens uit andere voorbeeld gegevensbestanden die niet overeenkomen met het schema dat zoals gedefinieerd.


4. Gebruik de volgende instructies om een nieuwe 'interne' tabel met de naam **errorLogs**te maken:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Deze instructies heeft de volgende acties uitvoeren:

    * **Maak tabel als niet bestaat**: maakt een tabel als deze nog niet bestaat. Omdat het **externe** trefwoord niet wordt gebruikt, is dit is een interne tabel, die is opgeslagen in de component datawarehouse en volledig wordt beheerd door component.

        > [AZURE.NOTE] In tegenstelling tot de **externe** tabellen verwijderd een interne tabel weghalen ook de onderliggende gegevens.

    * **Opgeslagen als ORC**: de gegevens zijn opgeslagen in geoptimaliseerde rij kolomindeling (ORC). Dit is een zeer geoptimaliseerde en efficiënt indeling voor het opslaan van de component data.

    * **Invoegen OVERSCHRIJVEN... Selecteer**: Hiermee selecteert u rijen uit de tabel **log4jLogs** met **[fout]**, klikt u vervolgens de gegevens in de tabel **errorLogs** invoegen.

    Om te bevestigen dat alleen rijen met **[fout]** in de kolom t4 aan de tabel **errorLogs** zijn opgeslagen, gebruikt u de volgende instructie om terug te keren van alle rijen uit **errorLogs**:

        SELECT * from errorLogs;

    Drie rijen met gegevens moet worden geretourneerd, alle met **[fout]** in de kolom t4.

##<a id="summary"></a>Overzicht

Zoals u ziet, de de opdracht component biedt een eenvoudige manier interactief component query's uitvoeren op een cluster HDInsight, het controleren van de taakstatus en het ophalen van de uitvoer.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over component in HDInsight:

* [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)

* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Als u Tez met onderdeel gebruikt, raadpleegt u de volgende documenten voor foutopsporing informatie:

* [De gebruikersinterface Tez op HDInsight op basis van Windows gebruiken](hdinsight-debug-tez-ui.md)

* [De weergave Ambari Tez op Linux gebaseerde HDInsight gebruiken](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

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





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

