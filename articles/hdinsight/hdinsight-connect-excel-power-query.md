<properties
    pageTitle="Excel verbinden met Hadoop met Power Query | Microsoft Azure"
    description="Informatie over het profiteren van business intelligence-onderdelen met behulp van Power Query voor Excel en access-gegevens die zijn opgeslagen in Hadoop op HDInsight."
    services="hdinsight"
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
    ms.date="10/19/2016"
    ms.author="jgao"/>


#<a name="connect-excel-to-hadoop-by-using-power-query"></a>Verbinden met Excel Hadoop met behulp van Power Query

Een belangrijke functie van de Microsoft-oplossing groot-gegevens is de integratie van Microsoft business intelligence (BI)-onderdelen met Hadoop clusters in Azure HDInsight. Een primaire voorbeeld van deze integratie is de mogelijkheid Excel koppelen aan de opslag van Azure-account met de gegevens die zijn gekoppeld aan uw Hadoop-cluster met behulp van de Microsoft Power Query voor Excel-invoegtoepassing. In dit artikel begeleidt u bij het instellen en gebruiken van Power Query query uitvoeren op gegevens die zijn gekoppeld aan een Hadoop-cluster beheerd met HDInsight.

> [AZURE.NOTE] Terwijl de stappen in dit artikel kunnen worden gebruikt met een Linux- of Windows gebaseerde HDInsight cluster, is Windows vereist voor het clientwerkstation.

### <a name="prerequisites"></a>Vereisten voor

Voordat u in dit artikel begint, hebt u het volgende:

- **Een HDInsight cluster**. Zie [aan de slag met Azure HDInsight]configureren van een[hdinsight-get-started].
- **A werkstation** waarop Windows 7, Windows Server 2008 R2 of een later besturingssysteem wordt uitgevoerd.
- **Office 2013 Professional Plus, Office 365 ProPlus, zelfstandige versie van Excel 2013 of Office 2010 Professional Plus**.


## <a name="install-power-query"></a>Power Query installeren

Power Query kan worden gebruikt om gegevens te importeren uit een verscheidenheid aan bronnen in Microsoft Excel, waar deze kan power BI-functies zoals PowerPivot en Power View. Power Query kunt met name de gegevens die heeft zijn uitvoer of die is gegenereerd door een Hadoop-taak op een cluster HDInsight importeren.

Microsoft Power Query voor Excel vanuit het [Microsoft Download Center] downloaden[ powerquery-download] en installeer deze.

## <a name="import-hdinsight-data-into-excel"></a>HDInsight-gegevens importeren in Excel

De invoegtoepassing Power Query voor Excel kunt u gemakkelijk om gegevens te importeren uit uw cluster HDInsight in Excel, waar BI hulpprogramma's zoals PowerPivot en Power Map kan worden gebruikt om te controleren, analyseren en de gegevens te presenteren.

**Gegevens importeren uit een cluster HDInsight**

1. Open Excel.

2. Maak een nieuwe lege werkmap.

3. Klik op het menu **Power Query** op **Uit Azure**en klik vervolgens op **Uit Microsoft Azure HDInsight**.

    ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]

    **Notitie:** Als u het **Power Query** -menu niet ziet, gaat u naar **bestand** > **Opties** > **- Invoegtoepassingen**en selecteer **COM-invoegtoepassingen** uit het vak voor het **beheren** van vervolgkeuzelijst onder aan de pagina. Selecteer de knop **Ga naar...** en controleer of het vakje in voor de Power Query voor Excel-invoegtoepassing is ingeschakeld.

    **Notitie:** Power Query kunt u gegevens importeren uit HDFS door te klikken op **Uit een andere bron**.

3. Voer de naam van de Azure Blob storage-account dat is gekoppeld aan uw cluster voor de **Naam van het Account**en klik vervolgens op **OK**. Dit is het [standaardaccount voor de opslag](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) of een account gekoppelde opslag.  De indeling is *https://<StorageAccountName>.blob.core.windows.net/*.

4. Voor **Accountsleutel**, voert u de toets voor de Blob storage-account en klik vervolgens op **Opslaan**. (U moet hiervoor alleen de eerste keer dat u toegang hebt tot dit archief.)

5. Dubbelklik op de naam van Blob storage container in het deelvenster **Navigator** aan de linkerkant van de Query-Editor. Standaard is de containernaam van de dezelfde naam als de naam van het cluster.

6. Zoek **HiveSampleData.txt** in de kolom **naam** (pad naar de map is **.. / component/magazijn/hivesampletable/**), en klik vervolgens op **binaire** aan de linkerkant van HiveSampleData.txt. HiveSampleData.txt wordt geleverd met alle bijbehorende. Desgewenst kunt u uw eigen bestand.

    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]

7. Als u wilt, kunt u de naam van de kolomnamen wijzigen. Wanneer u klaar bent, klikt u op **sluiten en laden**.  De gegevens zijn geladen aan uw werkmap:

    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Volgende stappen

In dit artikel, hebt u geleerd bij het gebruik van Power Query gegevens ophalen uit HDInsight in Excel. Daarnaast kunt u gegevens ophalen uit HDInsight naar Azure SQL-Database. Het is ook mogelijk om te uploaden van gegevens in HDInsight. Meer informatie raadpleegt u de volgende artikelen:

* [Verbinding maken met Excel met HDInsight via het Microsoft-Component ODBC-stuurprogramma][hdinsight-ODBC]
* [Gegevens uploaden naar HDInsight][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.SelectHdiSource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportData.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportedTable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
