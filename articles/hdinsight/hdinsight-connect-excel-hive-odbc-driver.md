<properties
   pageTitle="Excel verbinden met Hadoop met de component ODBC-stuurprogramma | Microsoft Azure"
   description="Informatie over het instellen en gebruiken van de component ODBC-stuurprogramma voor Excel query uitvoeren op gegevens in een cluster HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   tags="azure-portal"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

#<a name="connect-excel-to-hadoop-with-the-microsoft-hive-odbc-driver"></a>Verbinden met Excel Hadoop met de component ODBC-stuurprogramma

[AZURE.INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Onderdelen voor Microsoft Business Intelligence (BI) Microsofts Big Data oplossing geïntegreerd met Apache Hadoop-clusters die zijn geïmplementeerd door de Azure HDInsight. Een voorbeeld van deze integratie is de mogelijkheid om te Excel verbinden met de component datawarehouse van een Hadoop-cluster in HDInsight met het stuurprogramma Microsoft Component Open Database Connectivity (ODBC).

Het is ook mogelijk om verbinding maken met de gegevens die zijn gekoppeld aan een cluster HDInsight en andere gegevensbronnen, waaronder andere kolomgroepen (niet-HDInsight) Hadoop uit Excel met de invoegtoepassing Microsoft Power Query voor Excel. Zie [Excel verbinden met HDInsight via Power Query]voor informatie over het installeren en gebruiken van Power Query[hdinsight-power-query].

> [AZURE.NOTE] Terwijl de stappen in dit artikel kunnen worden gebruikt met een Linux- of Windows gebaseerde HDInsight cluster, is Windows vereist voor het clientwerkstation.

**Vereisten**:

Voordat u in dit artikel begint, hebt u het volgende:

- **Een HDInsight cluster**. Zie [aan de slag met Azure HDInsight]om een[hdinsight-get-started].
- **A workstation** met Office 2013 Professional Plus, Office 365 Pro Plus, zelfstandige versie van Excel 2013 of Office 2010 Professional Plus.


##<a name="install-microsoft-hive-odbc-driver"></a>Installeer Microsoft Component ODBC-stuurprogramma

Download en installeer Microsoft Component ODBC-stuurprogramma van het [Downloadcentrum][hive-odbc-driver-download].

Dit stuurprogramma kan worden geïnstalleerd op 32-bits of 64-bits versies van Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 en Windows Server 2012 en kan de verbinding met Azure HDInsight (versie 1,6 en hoger) en Azure HDInsight Emulator (v.1.0.0.0 en hoger). De versie die overeenkomt met de versie van de toepassing waar u het ODBC-stuurprogramma gaat gebruiken, moet u installeren. Voor deze zelfstudie wordt het stuurprogramma van Office Excel worden gebruikt.

##<a name="create-hive-odbc-data-source"></a>Component ODBC-gegevensbron maken

De volgende stappen hoe u een component ODBC-gegevensbron maken.

1. Druk op de Windows-toets om te openen van het startscherm van Windows 8 of Windows 10, en typ **gegevensbronnen**.
2. Klik op **ODBC-gegevens instellen met bronnen (32-bits)** of **ODBC-gegevensbronnen (64-bits) instellen** afhankelijk van uw Office-versie. Als u Windows 7 gebruikt, kiest u **ODBC-gegevensbronnen (32-bits)** of **ODBC-gegevensbronnen (64-bits)** van **Beheerprogramma's**. Hierdoor wordt het dialoogvenster **ODBC-gegevensbronbeheer** gestart.

    ![OBDC gegevensbronbeheer][img-hdi-simbahiveodbc-datasource-admin]

3. Klik op **toevoegen** om te openen van de wizard **Nieuwe gegevensbron maken** van gebruiker DNS.
4. Selecteer **Microsoft Component ODBC-stuurprogramma**en klik vervolgens op **Voltooien**. Hierdoor wordt het dialoogvenster **Microsoft Component ODBC-stuurprogramma DNS-instellingen** gestart.

5. Typ of Selecteer de volgende waarden:

    Eigenschap|Beschrijving
    ---|---
    Naam van de gegevensbron|Geef een naam naar uw gegevensbron
    Host|Voer &lt;HDInsightClusterName >. azurehdinsight.net. Bijvoorbeeld myHDICluster.azurehdinsight.net
    Poort|Gebruik <strong>443</strong>. (Deze poort is gewijzigd van 563 in 443.)
    Database|Gebruik <strong>standaard</strong>.
    Het servertype component|Selecteer <strong>Component Server 2</strong>
    Om|Selecteer <strong>Azure HDInsight-Service</strong>
    HTTP-pad|Laat deze leeg.
    Gebruikersnaam in te voeren|Voer de gebruikersnaam van de gebruiker cluster HDInsight. Dit is de gebruikersnaam die tijdens de voorziening cluster gemaakt. Als u de optie Snelle maken hebt gebruikt, is de standaardgebruikersnaam <strong>beheerder</strong>.
    Wachtwoord|Voer het wachtwoord HDInsight cluster-gebruiker.
    </table>

    Er zijn enkele belangrijke parameters rekening moet houden wanneer u klikt op **Geavanceerde opties**:

    Parameter|Beschrijving
    ---|---
    Systeemeigen Query gebruiken|Wanneer deze wordt geselecteerd, probeert het ODBC-stuurprogramma niet TSQL converteren naar HiveQL. U moet deze alleen als u 100% ervoor dat u verzendt puur HiveQL-instructies gebruiken. Wanneer u verbinding maakt met SQL Server- of Azure SQL-Database, laat u dit niet inschakelt.
    Rijen opgehaald per blokkeren|Wanneer een grote hoeveelheid records ophalen, deze parameter optimaliseren mogelijk moet om ervoor te zorgen optimale prestaties.
    Kolom van de standaardlengte tekenreeks, lengte binaire kolom, decimale kolom schaal|De lengte van het gegevenstype en kwaliteitscontroleprogramma invloed kunnen zijn op hoe gegevens worden geretourneerd. Ze treedt onjuiste gegevens moeten worden geretourneerd vanwege verlies van precisie van formules en/of afgekapt.


    ![Geavanceerde opties][img-HiveOdbc-DataSource-AdvancedOptions]

6. Klik op **testen** om te testen van de gegevensbron. Wanneer de gegevensbron correct is geconfigureerd, worden *TESTS voltooid!*.
7. Klik op **OK** om het dialoogvenster testen te sluiten. De nieuwe gegevensbron moet nu worden vermeld op de **ODBC-gegevensbronbeheer**.
8. Klik op **OK** om de wizard te sluiten.

##<a name="import-data-into-excel-from-hdinsight"></a>Gegevens importeren in Excel uit HDInsight

De onderstaande stappen wordt de manier waarop gegevens importeren uit een tabel component in een Excel-werkmap met de ODBC-gegevensbron die u hebt gemaakt in de bovenstaande stappen beschreven.

1. Een nieuwe of bestaande werkmap openen in Excel.
2. Vanaf het tabblad **gegevens** op **Uit andere gegevensbronnen**en klik vervolgens op **Van Wizard Gegevensverbinding** om de **Wizard Gegevensverbinding**te starten.

    ![Wizard Gegevensverbinding openen][img-hdi-simbahiveodbc.excel.dataconnection]

3. **ODBC-DSN** selecteert als de gegevensbron en klik vervolgens op **volgende**.
4. Selecteer de naam van de gegevensbron die u hebt gemaakt in de vorige stap uit ODBC-gegevensbronnen, en klik vervolgens op **volgende**.
5. Voer het wachtwoord voor het cluster in de wizard, en klik op **testen** om te controleren van de configuratie nogmaals, indien nodig.
6. Klik op **OK** om het dialoogvenster testen te sluiten.
7. Klik op **OK**. Wacht totdat het dialoogvenster **Database en tabel selecteren** om te openen. Dit kan een paar seconden duren.
8. Selecteer de tabel die u wilt importeren en klik vervolgens op **volgende**. De *hivesampletable* is een voorbeeld component-tabel die wordt geleverd met HDInsight clusters.  Als u dit nog niet hebt gemaakt, kunt u deze kiezen. Voor meer informatie over uitvoeren component van query's en component tabellen maken, raadpleegt u [Gebruik component met HDInsight][hdinsight-use-hive].
8. Klik op **Voltooien**.
9. U kunt wijzigen of de query opgeven in het dialoogvenster **Gegevens importeren** . Als u wilt doen, klikt u op **Eigenschappen**. Dit kan een paar seconden duren.
10. Klik op het tabblad **definitie** en klikt u vervolgens **LIMIET 200** toevoegen aan de component select-instructie in het tekstvak **de opdrachttekst** . De wijziging wordt de resulterende record is ingesteld op 200 beperken.

    ![Verbindingseigenschappen][img-hdi-simbahiveodbc-excel-connectionproperties]

11. Klik op **OK** om te sluiten van het dialoogvenster Eigenschappen van verbinding.
12. Klik op **OK** om het dialoogvenster **Gegevens importeren** te sluiten.  
13. Het wachtwoord opnieuw in te voeren en klik vervolgens op **OK**. Het duurt een paar seconden voordat u gegevens naar Excel wordt geïmporteerd.

##<a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u met de component ODBC-stuurprogramma gegevens ophalen uit de HDInsight-Service in Excel. Daarnaast kunt u gegevens ophalen van de HDInsight-Service in SQL-Database. Het is ook mogelijk om te uploaden van gegevens in een HDInsight-Service. Meer informatie raadpleegt u:

- [Analyseren van gegevens over vertragingen flight HDInsight gebruiken][hdinsight-analyze-flight-data]
- [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
- [Sqoop gebruiken met HDInsight] [hdinsight-use-sqoop]


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png
