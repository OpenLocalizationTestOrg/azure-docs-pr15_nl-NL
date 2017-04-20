<properties
    pageTitle="HBase zelfstudie: aan de slag met HBase in Hadoop | Microsoft Azure"
    description="Volg deze zelfstudie HBase aan de slag met Apache HBase met Hadoop in HDInsight. Tabellen maken op basis van de shell HBase en deze query component gebruiken."
    keywords="Apache hbase, hbase, hbase shell hbase zelfstudie"
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
    ms.date="10/21/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-windows-based-hadoop-in-hdinsight"></a>HBase zelfstudie: aan de slag met Apache HBase met Windows gebaseerde Hadoop in HDInsight

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Informatie over het maken van HBase clusters in HDInsight, HBase tabellen maken en de tabellen met behulp van Apache component query. Zie [overzicht van de HDInsight HBase]voor algemene informatie HBase,[hdinsight-hbase-overview].

De informatie in dit document hoort bij op basis van Windows HDInsight clusters. Voor informatie over Windows gebaseerde clusters, gebruikt u de tabkiezer boven aan de pagina om te schakelen.

> [AZURE.NOTE] HBase (versie 0.98.0) op Windows gebaseerde HDInsight is alleen beschikbaar voor gebruik met HDInsight 3.1 clusters (afhankelijk van Apache Hadoop en garens 2.4.0). Zie voor informatie over documentversie, [Wat is er nieuw in de Hadoop cluster versies geleverd door HDInsight?][hdinsight-versions]

## <a name="before-you-begin"></a>Voordat u begint

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Voordat u deze zelfstudie HBase, hebt u het volgende:

- **Een Microsoft Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Een werkstation** met Visual Studio 2013 of hoger: Zie [Visual Studio installeren](http://msdn.microsoft.com/library/e2h7fzkw.aspx)voor instructies.

### <a name="access-control-requirements"></a>Vereisten voor het beheer van Access

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>HBase cluster maken

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**Een cluster HBase maken met behulp van de Azure-portal**

1. Meld u aan bij de [portal van Azure][azure-management-portal].
2. Klik op **Nieuw** of **+** linksboven in de rechterbovenhoek naar links en klik op **gegevens + analyses**, **HDInsight**.
3. Voer de volgende waarden:

    - **De naam van de cluster** - Voer een naam voor dit cluster.
    - **Clustertype** - Select **HBase**.
    - **Cluster besturingssysteem** - Select **Windows**.  Voor het maken van Linux gebaseerde HBase cluster, raadpleegt u [HBase zelfstudie: aan de slag met Apache HBase met Hadoop in HDInsight (Linux)](hdinsight-hbase-tutorial-get-started-linux.md).
    - **Versie** - Selecteer een HBase-versie.
    - **Abonnement** - Selecteer uw Azure-abonnement gebruikt voor het maken van dit cluster.
    - **Resourcegroep** - maken van een nieuwe Azure resourcegroep of Selecteer een bestaande eigenschap. Zie [Overzicht van de Azure resourcemanager](azure-resource-manager/resource-group-overview.md) voor meer informatie
    - **Referenties** - voor Windows op basis van cluster, kunt u een gebruiker cluster (a.k.a HTTP-gebruiker, HTTP web servicegebruiker) en een gebruiker extern bureaublad. Klik op **Extern bureaublad inschakelen** als u wilt toevoegen van het externe bureaublad gebruikersreferenties. Het volgende gedeelte is RDP vereist.
    - **Gegevensbron** - Maak een nieuwe Azure opslag-account of Selecteer een bestaand Azure opslag-account moet worden gebruikt als het standaardbestandssysteem voor het cluster. De standaardopslaglocatie voor account bepaalt de locatie van de cluster-locatie. Het standaardaccount voor opslagruimte en het cluster moeten samen Zoek in het dezelfde Datacenter.
    - **Knooppunt prijzen lagen** - Selecteer het aantal regio-servers voor het cluster HBase

        > [AZURE.WARNING] De beschikbaarheid van HBase services, moet u een cluster met ten minste **drie** knooppunten. Dit zorgt ervoor dat, als één knooppunt uitvalt, de HBase gegevens regio's beschikbaar op andere knooppunten zijn.

        > Als u HBase leren, altijd kiest u 1 voor de clustergrootte en verwijder het cluster na elke gebruiken om shapes te verkleinen van de kosten.

    - **Optionele configuratie** - configureren-Azure virtuele netwerk scriptacties configureren en extra opslagruimte accounts toevoegen.

4. Klik op **maken**.

>[AZURE.NOTE] Nadat een cluster HBase wordt verwijderd, kunt u een ander HBase cluster kunt maken met behulp van het dezelfde standaardaccount voor opslagruimte en de standaard blob container. Het nieuwe cluster komt de HBase tabellen die u hebt gemaakt in het oorspronkelijke cluster ophalen. Als u wilt voorkomen dat inconsistenties, is het raadzaam de tabellen HBase uit te schakelen voordat u het cluster verwijderen.

## <a name="create-tables-and-insert-data"></a>Tabellen maken en gegevens invoegen

Er zijn twee manier voor toegang tot HBase. Deze sectie bevat de shell HBase gebruiken. Het volgende gedeelte beschreven hoe u met de .NET SDK.

Voor de meeste mensen gegevens worden weergegeven in de tabelindeling hebben:

![hdinsight hbase tabelgegevens][img-hbase-sample-data-tabular]

In HBase een implementatie van BigTable, dezelfde gegevens ziet er zo:

![hdinsight hbase bigtable gegevens][img-hbase-sample-data-bigtable]

Deze wordt beter als u klaar bent met de volgende procedure.  

**Gebruik van de HBase-shell**

1. Gebruik RDP verbinding maken met uw cluster HBase in HDInsight. Zie de instructies voor het RDP [beheren Hadoop-clusters in met behulp van de Portal Azure HDInsight][hdinsight-manage-portal].
2. Binnen uw RDP-sessie, klikt u op de **opdrachtregel Hadoop** -snelkoppeling zich op het bureaublad.
3. De HBase shell opent:

        cd %HBASE_HOME%\bin
        hbase shell

4. Maak een HBase met twee kolom gezinnen:

        create 'Contacts', 'Personal', 'Office'
        list
5. Sommige gegevens invoegen:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![hdinsight hadoop hbase shell][img-hbase-shell]

6. Eén rij ophalen

        get 'Contacts', '1000'

    Als de opdracht scannen met omdat er slechts één rij ziet u dezelfde resultaten.

    Zie [Inleiding tot HBase Schema ontwerpen]voor meer informatie over het schema van de tabel Hbase,[hbase-schema]. Zie voor meer opdrachten HBase, [Naslaggids voor het Apache HBase][hbase-quick-start].


6. De shell afsluiten

        exit

**Bulksgewijs laden gegevens in de tabel Contactpersonen HBase**

HBase bevat verschillende methoden voor het laden van gegevens in tabellen. Zie [bulksgewijs laden](http://hbase.apache.org/book.html#arch.bulk.load)voor meer informatie.


Een voorbeeld van gegevensbestand is geüpload naar een container openbare blob wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt. De inhoud van het gegevensbestand luidt als volgt:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

U kunt maken van een tekstbestand en upload het bestand op uw eigen opslag-account als u wilt. Zie de instructies voor het [uploaden van gegevens voor Hadoop-projecten in HDInsight][hdinsight-upload-data].

> [AZURE.NOTE] Deze procedure de contactpersonen HBase-tabel die u hebt gemaakt in de laatste procedure gebruikt.

1. Binnen uw RDP-sessie, klikt u op de **opdrachtregel Hadoop** -snelkoppeling zich op het bureaublad.
2. Klik op map wijzigen:

        cd %HBASE_HOME%\bin

3. Voer de volgende opdracht naar het bestand naar StoreFiles en store op een relatieve pad dat is opgegeven door Dimporttsv.bulk.output transformeren:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Voer de volgende opdracht voor het uploaden van de gegevens van /example/data/storeDataFileOutput aan de tabel HBase:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. U kunt de HBase shell opent en gebruikt u de opdracht gescande afbeelding voor een overzicht van de tabelinhoud.



## <a name="use-hive-to-query-hbase-tables"></a>Component aan query HBase tabellen gebruiken

U kunt gegevens die zijn opgeslagen in HBase met behulp van component zoeken. In dit gedeelte maakt een component-tabel die wordt toegewezen aan de tabel HBase en wordt deze gebruikt om de gegevens in uw tabel HBase query's mogelijk.

**Het dashboard cluster openen**

1. Blader naar **https://<HDInsight Cluster Name>.azurehdinsight.net/**.
5. Voer het account Hadoop-gebruiker gebruikersnaam en wachtwoord. De standaardnaam van de gebruiker is **beheerder** en het wachtwoord is wat u tijdens het maken hebt ingevoerd. Een nieuw browsertabblad wordt geopend.
6. Klik op **Editor component** boven aan de pagina. De Editor component ziet er zo uit:

    ![HDInsight cluster dashboard.][img-hdinsight-hbase-hive-editor]

**Component query's uitvoeren**

1. Voer de volgende HiveQL-script in component Editor en klik op **verzenden** als een Componententabel die is toegewezen aan de tabel HBase wilt maken. Zorg dat u hebt gemaakt op de voorbeeldtabel waarnaar wordt verwezen eerder in deze zelfstudie met behulp van de shell HBase voordat u deze instructie uitvoert.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

    Wacht totdat **de statusupdates moet **voltooid**** .

2. Voer de volgende HiveQL-script in component Editor en klik vervolgens op **verzenden**. De component query opvraagt de gegevens in de tabel HBase:

        SELECT count(*) FROM hbasecontacts;

4. Als u wilt de resultaten van de query component ophalen, klikt u op de koppeling **Details weergeven** in het venster **Taak sessie** wanneer de taak is voltooid. Er zijn slechts één taak uitvoerbestand omdat u één record in de tabel HBase plaatsen.




**Het uitvoerbestand zoeken**

1. Klik op **Bestandsbrowser**in de Query-Console.
2. Klik op de opslag van Azure-account dat wordt gebruikt als het standaardbestandssysteem voor het cluster HBase.
3. Klik op de naam van het HBase cluster. De standaardcontainer Azure opslag account wordt de naam van het cluster gebruikt.
4. Klik op **gebruiker**en klik vervolgens op **beheerder**. (Dit is de naam van de gebruiker Hadoop.)
6. Klik op de naam van de taak met de **Laatst gewijzigd** tijd die overeenkomt met de tijd waarop de query selecteren component is uitgevoerd.
4. Klik op **stdout**. Sla het bestand en open het bestand met Kladblok. Er is een uitvoerbestand.

    ![HDInsight HBase component Editor bestand Browser][img-hdinsight-hbase-file-browser]

## <a name="use-the-net-hbase-rest-api-client-library"></a>Gebruik van de bibliotheek van de client .NET HBase REST API

U moet de bibliotheek van de client HBase REST API voor .NET uit GitHub downloaden en maken van het project zodat u de HBase .NET SDK kunt. De volgende procedure bevat de instructies voor deze taak.

1. Maak een nieuwe C# Visual Studio Windows bureaublad Console-toepassing.
2. Open de NuGet Package Manager-Console door te klikken op **Extra** > **NuGet Package Manager** > **Package Manager Console**.
3. Voer de volgende opdracht uit NuGet in de console:

        Install-Package Microsoft.HBase.Client

5. De volgende instructies voor het **gebruik van** toevoegen aan de bovenkant van het bestand:

        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;

6. De functie **Hoofdgegeven** vervangen door het volgende:

        static void Main(string[] args)
        {
            string clusterURL = "https://<yourHBaseClusterName>.azurehdinsight.net";
            string hadoopUsername= "<yourHadoopUsername>";
            string hadoopUserPassword = "<yourHadoopUserPassword>";

            string hbaseTableName = "sampleHbaseTable";

            // Create a new instance of an HBase client.
            ClusterCredentials creds = new ClusterCredentials(new Uri(clusterURL), hadoopUsername, hadoopUserPassword);
            HBaseClient hbaseClient = new HBaseClient(creds);

            // Retrieve the cluster version.
            var version = hbaseClient.GetVersion();
            Console.WriteLine("The HBase cluster version is " + version);

            // Create a new HBase table.
            TableSchema testTableSchema = new TableSchema();
            testTableSchema.name = hbaseTableName;
            testTableSchema.columns.Add(new ColumnSchema() { name = "d" });
            testTableSchema.columns.Add(new ColumnSchema() { name = "f" });
            hbaseClient.CreateTable(testTableSchema);

            // Insert data into the HBase table.
            string testKey = "content";
            string testValue = "the force is strong in this column";
            CellSet cellSet = new CellSet();
            CellSet.Row cellSetRow = new CellSet.Row { key = Encoding.UTF8.GetBytes(testKey) };
            cellSet.rows.Add(cellSetRow);

            Cell value = new Cell { column = Encoding.UTF8.GetBytes("d:starwars"), data = Encoding.UTF8.GetBytes(testValue) };
            cellSetRow.values.Add(value);
            hbaseClient.StoreCells(hbaseTableName, cellSet);

            // Retrieve a cell by its key.
            cellSet = hbaseClient.GetCells(hbaseTableName, testKey);
            Console.WriteLine("The data with the key '" + testKey + "' is: " + Encoding.UTF8.GetString(cellSet.rows[0].values[0].data));
            // with the previous insert, it should yield: "the force is strong in this column"

            //Scan over rows in a table. Assume the table has integer keys and you want data between keys 25 and 35.
            Scanner scanSettings = new Scanner()
            {
                batch = 10,
                startRow = BitConverter.GetBytes(25),
                endRow = BitConverter.GetBytes(35)
            };

            ScannerInformation scannerInfo = hbaseClient.CreateScanner(hbaseTableName, scanSettings);
            CellSet next = null;
            Console.WriteLine("Scan results");

            while ((next = hbaseClient.ScannerGetNext(scannerInfo)) != null)
            {
                foreach (CellSet.Row row in next.rows)
                {
                    Console.WriteLine(row.key + " : " + Encoding.UTF8.GetString(row.values[0].data));
                }
            }

            Console.WriteLine("Press ENTER to continue ...");
            Console.ReadLine();
        }

7. Stel de eerste drie variabelen in de functie **Hoofdgegeven** .
8. Druk op **F5** om de toepassing te starten.

## <a name="check-cluster-status"></a>Cluster status controleren

HBase in HDInsight wordt geleverd met een Web-gebruikersinterface voor het controleren van clusters. De gebruikersinterface Web gebruikt, kunt u aanvragen statistieken of informatie over regio's.

Als u wilt openen in de Web-gebruikersinterface, u RDP moet in het cluster, en vervolgens op de snelkoppeling HMaster Info Web UI op uw bureaublad of gebruik van de volgende URL in een webbrowser:

    http://zookeeper[0-2]:60010/master-status

In een cluster beschikbaarheid vindt u een koppeling naar het huidige actieve HBase basispagina knooppunt die als host de Web-gebruikersinterface fungeert.

##<a name="delete-the-cluster"></a>Het cluster verwijderen
Als u wilt voorkomen dat inconsistenties, is het raadzaam de tabellen HBase uit te schakelen voordat u het cluster verwijderen.
[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="whats-next"></a>Hoe nu verder?
In deze zelfstudie HBase voor HDInsight hebt u geleerd hoe u een cluster HBase maakt en hoe u tabellen maken en weergeven van de gegevens in deze tabellen uit de shell HBase. U hebt ook geleerd hoe een component query op gegevens in tabellen HBase en het gebruik van de HBase C# REST API's naar een HBase-tabel maken en gegevens op te halen uit de tabel te gebruiken.

Zie voor meer informatie:

- [Overzicht van de HDInsight HBase][hdinsight-hbase-overview].
HBase is een Apache, open source, NoSQL database gebouwd op Hadoop met RAM en sterke consistentie voor grote hoeveelheden gegevens ongestructureerde en semistructured.
- [HBase clusters maken in een netwerk met Azure Virtual][hdinsight-hbase-provision-vnet].
Dankzij virtuele kunnen HBase clusters worden geïmplementeerd op hetzelfde virtuele netwerk als uw toepassingen zodat toepassingen met HBase rechtstreeks communiceren kunnen.
- [HBase configureren herhaling in HDInsight](hdinsight-hbase-geo-replication.md). Informatie over het configureren van HBase herhaling over twee Azure datacenters.
- [Twitter sentiment met HBase in HDInsight analyseren][hbase-twitter-sentiment].
Leer hoe u realtime [sentiment analyses](http://en.wikipedia.org/wiki/Sentiment_analysis) van grote gegevens uitvoeren met behulp van HBase in een Hadoop-cluster in HDInsight.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-bigtable.png
