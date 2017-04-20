<properties
    pageTitle="HBase zelfstudie: aan de slag met Linux gebaseerde HBase clusters in Hadoop | Microsoft Azure"
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
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-linux-based-hadoop-in-hdinsight"></a>HBase zelfstudie: aan de slag met Apache HBase met Linux gebaseerde Hadoop in HDInsight 

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Informatie over het maken van een cluster HBase in HDInsight en querytabellen met behulp van component HBase tabellen maken. Zie [overzicht van de HDInsight HBase]voor algemene informatie HBase,[hdinsight-hbase-overview].

De informatie in dit document is specifiek voor HDInsight Linux gebaseerde clusters. Voor informatie over Windows gebaseerde clusters, gebruikt u de tabkiezer boven aan de pagina om te schakelen.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie HBase, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- [Secure Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
- [omslaan](http://curl.haxx.se/download.html).

### <a name="access-control-requirements"></a>Vereisten voor het beheer van Access

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>HBase cluster maken

De volgende procedure wordt een resourcemanager Azure-sjabloon om een versie 3.4 Linux gebaseerde HBase cluster en de afhankelijke standaard opslag van Azure-account te maken. Als u wilt weten over de parameters in de procedure en andere methoden voor het maken van cluster gebruikt, raadpleegt u [Hadoop maken Linux gebaseerde clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Klik op de volgende afbeelding om te openen van de sjabloon in de portal van Azure. De sjabloon bevindt zich in een container openbare blob. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Voer de volgende gegevens van het blad **aangepaste-implementatie** :

    - **Abonnement**: Selecteer uw Azure abonnement die wordt gebruikt voor het maken van het cluster.
    - **Resourcegroep**: Maak een nieuwe resourcebeheer Azure-groep of een bestaande eigenschap te gebruiken.
    - **Locatie**: Geef de locatie van de resourcegroep. 
    - **Clusternaam**: Voer een naam voor het HBase cluster dat u wilt maken.
    - **Cluster aanmeldingsnaam en wachtwoord**: de standaard-aanmeldingsnaam is **beheerder**.
    - **SSH gebruikersnaam en wachtwoord**: de standaard-gebruikersnaam is **sshuser**.  U kunt dit wijzigen.
     
    Andere parameters zijn optioneel.  
    
    Elk cluster heeft een afhankelijkheid Azure Blob storage-account. Nadat u een cluster verwijdert, worden de gegevens behouden in de opslagruimte-account. De naam van het cluster standaard opslag-account is de naam van het cluster met "store" toegevoegd. Vastgelegde in het gedeelte van de variabelen sjabloon is.
        
3. Selecteer **ik ga akkoord met de bepalingen en voorwaarden hierboven**, en klik op **aanschaffen**. Het duurt ongeveer 20 minuten een cluster maken.


>[AZURE.NOTE] Nadat een cluster HBase wordt verwijderd, kunt u een ander HBase cluster kunt maken met behulp van de dezelfde standaard blob container. Het nieuwe cluster komt de HBase tabellen die u hebt gemaakt in het oorspronkelijke cluster ophalen. Als u wilt voorkomen dat inconsistenties, is het raadzaam de tabellen HBase uit te schakelen voordat u het cluster verwijderen.

## <a name="create-tables-and-insert-data"></a>Tabellen maken en gegevens invoegen

U kunt SSH verbinding maken met HBase clusters en gebruikt u HBase Shell voor HBase tabellen maken, voegt u gegevens en querygegevens. Zie voor informatie over het gebruik van SSH [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight uit Linux, Unix, of OS X](hdinsight-hadoop-linux-use-ssh-unix.md) en [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight van Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
 

Voor de meeste mensen gegevens worden weergegeven in de tabelindeling hebben:

![Tabelgegevens HDInsight HBase][img-hbase-sample-data-tabular]

In HBase een implementatie van BigTable, dezelfde gegevens ziet er zo:

![HDInsight HBase bigtable gegevens][img-hbase-sample-data-bigtable]

Deze worden beter als u klaar bent met de volgende procedure.  


**Gebruik van de HBase-shell**

1. Voer de volgende opdracht uit SSH:

        hbase shell

4. Maak een HBase met twee kolommen gezinnen:

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

    U ziet hetzelfde resultaat als met de opdracht scan omdat er slechts één rij.

    Zie [Inleiding tot HBase Schema ontwerpen]voor meer informatie over het schema van de tabel HBase,[hbase-schema]. Zie voor meer opdrachten HBase, [Naslaggids voor het Apache HBase][hbase-quick-start].

6. De shell afsluiten

        exit



**Bulksgewijs laden gegevens in de tabel Contactpersonen HBase**

HBase bevat verschillende methoden voor het laden van gegevens in tabellen.  Zie [bulksgewijs laden](http://hbase.apache.org/book.html#arch.bulk.load)voor meer informatie.


Een voorbeeld van gegevensbestand is geüpload naar een container openbare blob *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  De inhoud van het gegevensbestand luidt als volgt:

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

1. Voer de volgende opdracht naar het bestand naar StoreFiles en store op een relatieve pad dat is opgegeven door Dimporttsv.bulk.output transformeren uit SSH,:.  Als u zich in HBase Shell, gebruikt u de afsluitopdracht om af te sluiten.

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Voer de volgende opdracht voor het uploaden van de gegevens van /example/data/storeDataFileOutput aan de tabel HBase:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. U kunt de HBase shell opent en gebruikt u de opdracht gescande afbeelding voor een overzicht van de tabelinhoud.



## <a name="use-hive-to-query-hbase"></a>Component op query HBase gebruiken

U kunt gegevens in HBase tabellen zoeken met behulp van component. In dit gedeelte maakt een component-tabel die wordt toegewezen aan de tabel HBase en wordt deze gebruikt om de gegevens in uw tabel HBase query's mogelijk.

1. Open **stopverf**en verbinding maken met het cluster.  Zie de instructies in de vorige procedure.
2. Open de component shell.

       hive
3. De volgende HiveQL script uitvoeren om een component-tabel die is toegewezen aan de tabel HBase maken. Zorg dat u de voorbeeldtabel waarnaar wordt verwezen eerder in deze zelfstudie met behulp van de shell HBase voordat u deze instructie uitvoert hebt gemaakt.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

2. De volgende HiveQL script uitvoeren om de gegevens in de tabel HBase query:

        SELECT count(*) FROM hbasecontacts;

## <a name="use-hbase-rest-apis-using-curl"></a>Gebruik HBase REST API's met omslaan

> [AZURE.NOTE] Wanneer u krul of een andere REST-communicatie met WebHCat, moet u de aanvragen verifiëren doordat de gebruikersnaam en wachtwoord voor de beheerder van de cluster HDInsight. U moet de naam van het cluster ook gebruiken als onderdeel van de id URI (Uniform Resource) gebruikt voor het versturen van aanvragen op de server.
>
> Voor de opdrachten in deze sectie, vervangen door de gebruiker om te verifiëren met het cluster **gebruikersnaam** en **wachtwoord** vervangen door het wachtwoord voor het gebruikersaccount. **CLUSTERNAAM** vervangen door de naam van uw cluster.
>
> De REST API is via [Basisverificatie](http://en.wikipedia.org/wiki/Basic_access_authentication)beveiligd. U moet altijd aanvragen maken met behulp van Secure HTTP (HTTPS) om ervoor te zorgen dat uw referenties veilig worden verzonden naar de server.

1. Gebruik de volgende opdracht uit om te bevestigen dat u verbinding met uw cluster HDInsight maken kunt vanaf de opdrachtregel:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status

    U moet verschijnt een bericht als volgt uit:

        {"status":"ok","version":"v1"}

    De parameters in deze opdracht gebruikt zijn er als volgt uit:

    * **-u** - de gebruikersnaam en wachtwoord gebruikt om te verifiëren het verzoek.
    * **-G** - geeft aan dat dit een GET-aanvraag.

2. Gebruik de volgende opdracht uit voor een overzicht van de bestaande HBase-tabellen:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/

3. Gebruik de volgende opdracht uit om te maken van een nieuwe HBase-tabel met twee kolom gezinnen:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v

    Het schema is opgegeven in de indeling van JSon.

4. Gebruik de volgende opdracht uit sommige gegevens in te voegen:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":{\"key\":\"MTAwMA==\",\"Cell\":{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}}}" \
        -v

    U moet base64 coderen van de waarden die zijn opgegeven in de schakeloptie -d.  Klik in het voorbeeld:

    - MTAwMA ==: 1000
    - UGVyc29uYWw6TmFtZQ ==: Personal: naam
    - Sm9obiBEb2xl: John Dole

    [Onwaar-rij-toets](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) kunt u meerdere (batch) waarde invoegen.

5. Gebruik de volgende opdracht uit om een rij:

        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

Zie voor meer informatie over HBase Rest, [Apache HBase Naslaggids voor](https://hbase.apache.org/book.html#_rest).

## <a name="check-cluster-status"></a>Cluster status controleren

HBase in HDInsight wordt geleverd met een Web-gebruikersinterface voor het controleren van clusters. De gebruikersinterface Web gebruikt, kunt u aanvragen statistieken of informatie over regio's.

SSH kan ook worden gebruikt voor tunnel lokale aanvragen, zoals webaanvragen, voor het cluster HDInsight. Het verzoek wordt vervolgens naar de gewenste bron worden gerouteerd alsof deze afkomstig op het hoofd knooppunt van HDInsight zijn. Zie voor meer informatie, [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight van Windows](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel).

**Tot stand brengen van een SSH tunneling sessie**

1. Open **stopverf**.  
2. Als u een sleutel SSH opgegeven wanneer u uw gebruikersaccount tijdens het maken gemaakt, moet u de volgende stap uit om te selecteren van de persoonlijke sleutel wilt gebruiken wanneer u geverifieerd bij het cluster uitvoeren:

    In de **categorie** **verbinding**uitvouwen, **SSH**uitvouwen en selecteer **Auth**. Tot slot op **Bladeren** en selecteer het bestand .ppk met uw persoonlijke sleutel.

3. Klik op de **sessie**in **categorie**.
4. Voer de volgende waarden met de eenvoudige opties voor uw scherm stopverf sessie:

    - **Hostnaam**: het SSH-adres van het veld HDInsight-server in de Host name (of IP-adres). Het SSH-adres is de naam van uw cluster, klikt u vervolgens **-ssh.azurehdinsight.net**. Bijvoorbeeld: *mijncluster-ssh.azurehdinsight.net*.
    - **Poort**: 22. De ssh poort op de primaire headnode is 22.  
5. In de sectie **categorie** aan de linkerkant van het dialoogvenster **verbinding**uitvouwen, **SSH**uitvouwen en klik vervolgens op **Tunnels**.
6. Geef de volgende informatie op de opties voor SSH poort doorschakelen formulier bepalen:

    - **Bronpoort** - de poort op de client die u wilt doorsturen. Bijvoorbeeld: 9876.
    - **Dynamische** - Hiermee dynamische SOCKS-proxy-mailroutering.
7. Klik op **toevoegen** als de instellingen wilt toevoegen.
8. Klik op **openen** onderaan in het dialoogvenster om te openen van een verbinding SSH.
9. Wanneer u wordt gevraagd, meld u aan bij de server via een SSH-account. Hiermee wordt een sessie SSH tot stand brengen en inschakelen van de tunnel.

**Naar de FQDN-naam van het gebruik van Ambari zoohouder zoeken**

1. Blader naar https://<ClusterName>.azurehdinsight.net/.
2. Het hulpprogramma voor het account van uw cluster-gebruikersreferenties tweemaal op ENTER.
3. Klik in het linkermenu op **zookeeper**.
4. Klik op een van de drie **ZooKeeper Server** koppelingen uit het overzicht.
5. **Hostname**kopiëren. Bijvoorbeeld zk0-CLUSTERNAME.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net.

**U configureert een clientprogramma (Firefox) en cluster status controleren**

1. Open Firefox.
2. Klik op de knop **Menu openen** .
3. Klik op **Opties**.
4. Klik op **Geavanceerd**, klik op **netwerk**en klik vervolgens op **Instellingen**.
5. Selecteer **Handmatige proxyconfiguratie**.
6. Voer de volgende waarden:

    - **Host Socks**: localhost
    - **Poort**: u hebt geconfigureerd in de stopverf SSH tunneling dezelfde poort gebruiken.  Bijvoorbeeld: 9876.
    - **SOCKS v5**: (geselecteerd)
    - **Externe DNS**: (geselecteerd)
7. Klik op **OK** als de wijzigingen wilt opslaan.
8. Blader naar http://&lt;de FQDN-naam van een ZooKeeper >: 60010/outmodel-status.

In een cluster beschikbaarheid vindt u een koppeling naar het huidige actieve HBase basispagina knooppunt die als host de Web-gebruikersinterface fungeert.

##<a name="delete-the-cluster"></a>Het cluster verwijderen

Als u wilt voorkomen dat inconsistenties, is het raadzaam de tabellen HBase uit te schakelen voordat u het cluster verwijderen.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie HBase voor HDInsight hebt u geleerd hoe u een cluster HBase maakt en hoe u tabellen maken en weergeven van de gegevens in deze tabellen uit de shell HBase. U hebt ook geleerd het gebruik van een query component op gegevens in tabellen HBase en het gebruik van de HBase C# REST API's naar een tabel HBase maken en gegevens op te halen uit de tabel.

Meer informatie raadpleegt u:

- [Overzicht van de HDInsight HBase][hdinsight-hbase-overview]: HBase is een Apache, open source, NoSQL database gebouwd op Hadoop met RAM en sterke consistentie voor grote hoeveelheden gegevens ongestructureerde en semistructured.


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
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
