<properties
   pageTitle="Gegevens kopiëren naar en vanuit WASB in Lake gegevensopslag met Distcp | Microsoft Azure"
   description="Distcp gebruiken om gegevens te kopiëren naar en vanuit Azure opslag BLOB's naar Lake gegevensopslag"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a>Distcp gebruiken om gegevens tussen Azure opslag BLOB's en Lake gegevensopslag te kopiëren

> [AZURE.SELECTOR]
- [DistCp gebruiken](data-lake-store-copy-data-wasb-distcp.md)
- [AdlCopy gebruiken](data-lake-store-copy-data-azure-storage-blob.md)


Als u een HDInsight cluster die toegang tot een Lake gegevensopslag-account heeft hebt gemaakt, kunt u Hadoop-systeem hulpprogramma's zoals Distcp gegevens te kopiëren **naar en vanuit** een HDInsight clusteropslag (WASB) naar een Lake gegevensopslag-account. In dit artikel bevat instructies voor het hiervoor.

##<a name="prerequisites"></a>Vereisten voor

Voordat u in dit artikel begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
- **Uw abonnement op Azure inschakelen** voor gegevensopslag Lake openbare preview. Zie de [instructies](data-lake-store-get-started-portal.md#signup).
- **Azure HDInsight cluster** met toegang tot een Lake gegevensopslag-account. Zie [een HDInsight cluster met Lake gegevensopslag maken](data-lake-store-hdinsight-hadoop-use-portal.md). Zorg ervoor dat u extern bureaublad inschakelen voor het cluster.

## <a name="do-you-learn-fast-with-videos"></a>Vind u meer snel met video's?

[Bekijk deze video](https://mix.office.com/watch/1liuojvdx6sie) over het kopiëren van gegevens tussen Azure opslag BLOB's en gegevensopslag Lake DistCp gebruiken.

## <a name="use-distcp-from-remote-desktop-windows-cluster-or-ssh-linux-cluster"></a>Distcp van extern bureaublad (Windows-cluster) of SSH (Linux cluster) gebruiken

Er wordt een cluster HDInsight geleverd met het hulpprogramma Distcp kan worden gebruikt om gegevens uit verschillende bronnen kopiëren naar een cluster HDInsight. Als u het cluster HDInsight Lake gegevensopslag gebruiken als een extra opslagruimte hebt geconfigureerd, kan het hulpprogramma Distcp gebruikte out-van-het-box om gegevens te kopiëren naar en vanuit een Lake gegevensopslag-account ook zijn. In deze sectie kijken we naar het gebruik van het hulpprogramma Distcp.

1. Als u een Windows-cluster hebt, externe in een cluster HDInsight die toegang heeft tot een Lake gegevensopslag-account. Zie [verbinding maken met behulp van RDP clusters](../hdinsight/hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)voor instructies. Open de opdrachtregel Hadoop vanaf het bureaublad cluster.

    Als u een cluster Linux hebt, gebruikt u SSH verbinding maken met de cluster. Zie [verbinding maken met een cluster Linux gebaseerde HDInsight](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster). Voer de opdrachten vanaf de prompt SSH.

3. Controleer of u toegang heeft tot de Azure opslag BLOB's (WASB). Voer de volgende opdracht:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    Dit moet een lijst met inhoud in de blob storage bieden.

4. Op dezelfde manier verifiëren of u kunt toegang heeft tot het account voor gegevensopslag Lake uit het cluster. Voer de volgende opdracht:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    Dit moet een lijst met bestanden/mappen in het account voor gegevensopslag Lake bieden.

5. Gebruik Distcp om gegevens uit WASB naar een account voor gegevensopslag Lake kopiëren.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Hiermee wordt de inhoud van de **** map/voorbeeld/gegevens/gutenberg/in WASB naar **/myfolder** in het account voor gegevensopslag Lake kopiëren.

6. Op deze manier Distcp gebruiken om gegevens te kopiëren van Lake gegevensopslag account naar WASB.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Hiermee wordt de inhoud van **/myfolder** in het account voor gegevensopslag Lake kopiëren **** naar/voorbeeld/gegevens/gutenberg/tijdelijke map in WASB.

## <a name="see-also"></a>Zie ook

- [Gegevens van Azure opslag BLOB's kopiëren naar Lake gegevensopslag](data-lake-store-copy-data-azure-storage-blob.md)
- [Beveiliging van gegevens in Lake gegevensopslag](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensopslag](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensopslag](data-lake-store-hdinsight-hadoop-use-portal.md)
