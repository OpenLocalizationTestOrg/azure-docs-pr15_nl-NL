<properties
   pageTitle="Gebruik de component shell in HDInsight (Hadoop) | Microsoft Azure"
   description="Informatie over het gebruik van de component shell met een cluster Linux gebaseerde HDInsight. U wordt informatie over het verbinding maken met de HDInsight cluster via SSh en het gebruik van de component Shell om interactief query's uitvoeren."
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
   ms.date="10/04/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-in-hdinsight-with-ssh"></a>Component gebruiken met Hadoop in HDInsight via SSH

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In dit artikel leert u hoe u verbinding maken met een Hadoop op Azure HDInsight cluster en vervolgens de component query's met behulp van de opdrachtregel component (CLI) interactief dient met Secure Shell (SSH).

> [AZURE.IMPORTANT] De opdracht component is beschikbaar op HDInsight Linux gebaseerde clusters, moet u overwegen Beeline. Beeline is een nieuwere client voor het werken met component en is opgenomen in uw cluster HDInsight. Zie [Gebruik component met Hadoop in HDInsight met Beeline](hdinsight-hadoop-use-hive-beeline.md)voor meer informatie over het gebruik van dit.

##<a id="prereq"></a>Vereisten voor

U voltooit de stappen in dit artikel, moet u het volgende:

* Een Linux gebaseerde Hadoop op HDInsight cluster.

* Een SSH-client. Linux, Unix en Mac OS moeten worden geleverd met een SSH-client. Windows-gebruikers, moeten een mailclient gebruikt, zoals [stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)downloaden.

##<a id="ssh"></a>Verbinding maken met SSH

Verbinding maken met de FQDN-naam (Fully Qualified Domain Name) van uw cluster HDInsight via de opdracht SSH. De FQDN-naam moet de naam die u vervolgens het cluster, gegeven **. azurehdinsight.net**. Bijvoorbeeld de volgende manier verbinding maken met een cluster met de naam **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Als u de sleutel van een certificaat voor SSH verificatie opgegeven** wanneer u het cluster HDInsight hebt gemaakt, mogelijk moet u de locatie van de persoonlijke sleutel op uw clientsysteem opgeven:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Als u een wachtwoord voor de verificatie SSH opgegeven** toen u het cluster HDInsight hebt gemaakt, moet u het wachtwoord wanneer u wordt gevraagd te geven.

Zie voor meer informatie over het gebruik van SSH met HDInsight, [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight uit Linux, OS X, en Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Stopverf (clients op basis van Windows)

Windows beschikt niet over een ingebouwde SSH-client. We raden u aan **stopverf**, dat kan worden gedownload van [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)gebruiken.

Zie voor meer informatie over het gebruik van stopverf, [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight van Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hive"></a>Gebruik de opdracht component

2. Zodra u verbinding hebt, start u de component CLI met behulp van de volgende opdracht uit:

        hive

3. De CLI gebruikt, voert u de volgende instructies als een nieuwe tabel met de naam **log4jLogs** met behulp van de voorbeeldgegevens wilt maken:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Deze instructies heeft de volgende acties uitvoeren:

    * **DROP TABLE** - verwijdert u de tabel en het gegevensbestand geval de tabel al bestaat.
    * **Externe tabel maken** - Hiermee maakt u een nieuwe 'externe' tabel in component. De definitie van de tabel externe tabellen alleen opgeslagen in component. De gegevens is links op de oorspronkelijke locatie.
    * **Rij-indeling** : Hiermee wordt aan component hoe de gegevens worden opgemaakt. In dit geval worden de velden in elke log gescheiden door een spatie.
    * **Opgeslagen als TEXTFILE locatie** - worden vermeld component waar de gegevens is opgeslagen (de map met de voorbeeldgegevens /), en dat deze wordt opgeslagen als tekst.
    * **Selecteer** - Hiermee selecteert u een telling van alle rijen waarbij kolom **t4** de waarde **[fout bevat]**. Dit moet een waarde van **3** retourneren als er drie rijen die deze waarde bevatten.
    * **INPUT__FILE__NAME zoals '%.log'** - Hiermee wordt aan die we alleen gegevens uit bestanden die eindigen ophalen moet op component. log. Hiermee beperkt u de zoekopdracht naar het bestand sample.log die de gegevens bevat, en behoudt uit retourneren van gegevens uit andere voorbeeld gegevensbestanden die niet overeenkomen met het schema dat zoals gedefinieerd.

    > [AZURE.NOTE] Externe tabellen moeten worden gebruikt wanneer u de onderliggende gegevens die moeten worden bijgewerkt door een externe bron, zoals een geautomatiseerde gegevens upload-proces of door een andere MapReduce bewerking, maar altijd wilt component query's voor het gebruik van de meest recente gegevens verwachten.
    >
    > Weghalen van een externe tabel bevat **niet** verwijderen de gegevens, alleen de definitie van de tabel.

4. Gebruik de volgende instructies om een nieuwe 'interne' tabel met de naam **errorLogs**te maken:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Deze instructies heeft de volgende acties uitvoeren:

    * **Maken van tabel als niet aanwezig** - Hiermee maakt u een tabel, als deze nog niet bestaat. Aangezien het **externe** trefwoord niet wordt gebruikt, is dit is een interne tabel, die is opgeslagen in de component datawarehouse en volledig wordt beheerd door component.
    * De gegevens **Die zijn opgeslagen als ORC** - opgeslagen in geoptimaliseerd rij kolommen (ORC)-indeling. Dit is een zeer geoptimaliseerde en efficiënt indeling voor het opslaan van de component data.
    * **Invoegen OVERSCHRIJVEN... Selecteer** : Hiermee selecteert u rijen uit de tabel **log4jLogs** die **[fout]**bevatten en vervolgens de gegevens in de tabel **errorLogs** invoegen.

    Om te bevestigen dat alleen de rijen met **[fout]** in de kolom t4 aan de tabel **errorLogs** zijn opgeslagen, gebruikt u de volgende instructie om terug te keren van alle rijen uit **errorLogs**:

        SELECT * from errorLogs;

    Drie rijen met gegevens moet worden geretourneerd, alle met **[fout]** in de kolom t4.

    > [AZURE.NOTE] In tegenstelling tot de externe tabellen, worden een interne tabel weghalen verwijderd als u ook de onderliggende gegevens.

##<a id="summary"></a>Overzicht

Zoals u ziet u de opdracht component een eenvoudige manier interactief component query's uitvoeren op een cluster HDInsight, het controleren van de taakstatus en het ophalen van de uitvoer.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over component in HDInsight:

* [Component gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Varken met Hadoop op HDInsight gebruiken](hdinsight-use-pig.md)

* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Als u Tez met onderdeel gebruikt, raadpleegt u de volgende documenten voor foutopsporing informatie:

* [De gebruikersinterface Tez op HDInsight op basis van Windows gebruiken](hdinsight-debug-tez-ui.md)

* [De weergave Ambari Tez op Linux gebaseerde HDInsight gebruiken](hdinsight-debug-ambari-tez-view.md)

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

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md



[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png

