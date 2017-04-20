<properties
pageTitle="Migreren van HDInsight op basis van Windows met Linux gebaseerde HDInsight | Azure"
description="Informatie over het migreren van een cluster HDInsight op basis van Windows naar een cluster Linux gebaseerde HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/28/2016"
ms.author="larryfr"/>

#<a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Migreren van een Windows gebaseerde HDInsight cluster naar een cluster Linux gebaseerde

Terwijl u op basis van Windows HDInsight u een eenvoudige manier Hadoop gebruiken in de cloud, is het mogelijk dat u een cluster Linux gebaseerde moet om te profiteren van hulpprogramma's en -technologieën die vereist voor uw oplossing zijn. Veel zaken in het Hadoop-systeem op Linux-systemen zijn ontwikkeld en enkele mogelijk niet beschikbaar voor gebruik met Windows gebaseerde HDInsight. Daarnaast kunnen veel boeken, video's en andere trainingsmateriaal wordt ervan uitgegaan dat u een Linux-systeem worden gebruikt tijdens het werken met Hadoop.

In dit document bevat informatie over de verschillen tussen HDInsight voor Windows en Linux en instructies over het migreren van bestaande werkbelasting naar een cluster Linux gebaseerde.

> [AZURE.NOTE] HDInsight clusters gebruiken Ubuntu langdurig ondersteuning (LTS) als het besturingssysteem voor de knooppunten in het cluster. Zie voor informatie over de versie van Ubuntu die beschikbaar zijn met HDInsight, samen met andere onderdeel versiegegevens, [HDInsight onderdeel versies](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Migratietaken

De algemene workflow voor de migratie is als volgt.

![Migratie-werkstroomdiagram](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1.  Lees elk gedeelte van dit document voor meer informatie over wijzigingen die nodig zijn bij het migreren van uw bestaande werkstroom, taken, enzovoort naar een cluster Linux gebaseerde.

2.  Maak een cluster Linux gebaseerde als een test/kwaliteit assurance-omgeving. Zie voor meer informatie over het maken van een cluster Linux gebaseerde [maken Linux gebaseerde clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

3.  Bestaande taken, gegevensbronnen en sinks kopiëren naar de nieuwe omgeving. Zie de gegevens kopiëren naar het gedeelte van de omgeving test voor meer informatie.

4.  Gegevensvalidatie tests om ervoor te zorgen dat uw taken werkt zoals verwacht op het nieuwe cluster uitvoeren.

Nadat u hebt gecontroleerd of alles werkt zoals verwacht, plannen downtime voor de migratie. Tijdens deze downtime, moet u de volgende acties uitvoeren.

1.  Back-up maken van een tijdelijke gegevens lokaal opgeslagen knooppunten. Bijvoorbeeld als u gegevens rechtstreeks in een kop knooppunt opgeslagen.

2.  Verwijder het cluster op basis van Windows.

3.  Maak een Linux gebaseerde cluster met het dezelfde standaard gegevensopslag die het cluster op basis van Windows gebruikt. Hierdoor wordt het nieuwe cluster om verder te werken met uw bestaande productiegegevens.

4.  Een tijdelijke gegevens dat u een back-up importeren.

5.  Start taken/doorgaan met het verwerken met behulp van het nieuwe cluster.

### <a name="copy-data-to-the-test-environment"></a>Gegevens kopiëren naar de testomgeving

Er zijn veel methoden om de gegevens en taken te kopiëren, de twee besproken in deze sectie zijn echter de eenvoudigste methoden om rechtstreeks bestanden verplaatsen naar een cluster test.

#### <a name="hdfs-dfs-copy"></a>HDFS DFS kopiëren

U kunt de opdracht Hadoop HDFS rechtstreeks Kopieer gegevens uit de opslagruimte voor uw bestaande productie cluster, naar de opslag van voor een nieuwe test cluster met de volgende stappen.

1. Zoek de opslag en standaard container-gegevens voor uw bestaande cluster. U kunt dit doen met behulp van de volgende Azure PowerShell-script.

        $clusterName="Your existing HDInsight cluster name"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
        write-host "Default container: $clusterInfo.DefaultStorageContainer"

2. Volg de stappen in maken Linux gebaseerde clusters in HDInsight document om een nieuwe testomgeving te maken. Stoppen voordat u de cluster maakt en selecteert u in plaats daarvan **Optionele configuratie**.

3. Selecteer in het blad optionele configuratie **Gekoppelde opslag-Accounts**.

4. Selecteer **een sleutel opslag toevoegen**en wanneer hierom wordt gevraagd, selecteert u het opslag-account dat is geretourneerd door de PowerShell-script in stap 1. Klik op **selecteren** op elke blade om ze te sluiten. Ten slotte het cluster maken.

5. Zodra het cluster is gemaakt, verbinden met **SSH.** Als u niet bekend bent met het gebruik van SSH met HDInsight, raadpleegt u een van de volgende artikelen.

    * [SSH gebruiken met Linux gebaseerde HDInsight vanuit Windows-clients](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [SSH gebruiken met Linux gebaseerde HDInsight vanuit Linux, Unix en Mac-clients](hdinsight-hadoop-linux-use-ssh-unix.md)

6. Gebruik de volgende opdracht kopiëren van bestanden van het account gekoppelde opslag naar het nieuwe account van de standaard-opslag in de sessie SSH. CONTAINER en ACCOUNT vervangen door de container en de accountgegevens die door de PowerShell-script in stap 1 geretourneerd. Vervang het pad naar gegevens door het pad naar een gegevensbestand.

        hdfs dfs -cp wasbs://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location

    [AZURE.NOTE] Als de structuur van de map met de gegevens op de testomgeving niet bestaat, kunt u deze met de volgende opdracht kunt maken.

        hdfs dfs -mkdir -p /new/path/to/create

    De `-p` schakelt u het maken van alle mappen in het pad.

#### <a name="direct-copy-between-azure-storage-blobs"></a>Directe kopiëren tussen opslag van Azure BLOB 's

U kunt ook, gebruikt u de `Start-AzureStorageBlobCopy` Azure PowerShell-cmdlet BLOB's tussen accounts opslag buiten HDInsight kopiëren. Zie voor meer informatie de sectie Azure BLOB's van Azure PowerShell gebruiken met Azure opslag beheren.

##<a name="client-side-technologies"></a>Aan de clientzijde technologieën

In het algemeen blijft aan de clientzijde technologieën zoals [Azure PowerShell-cmdlets](../powershell-install-configure.md), [Azure CLI](../xplat-cli-install.md) of de [.NET SDK voor Hadoop](https://hadoopsdk.codeplex.com/) werken op dezelfde manier met Linux gebaseerde kolomgroepen zoals ze zijn afhankelijk van REST API's die hetzelfde voor beide clustertypen OS zijn.

##<a name="server-side-technologies"></a>Aan de clientzijde technologieën

De volgende tabel bevat richtlijnen voor het migreren serverzijde onderdelen Windows specifieke.

| Als u deze technologie gebruikt... | Deze actie uitvoeren... |
| ----- | ----- |
| **PowerShell** (serverzijde scripts, met inbegrip van scriptacties tijdens het maken van het cluster gebruikt) | Herschreven fragment als we vaker doen scripts. Zie voor scriptacties [aanpassen Linux gebaseerde HDInsight via scriptacties](hdinsight-hadoop-customize-cluster-linux.md) en [Script actie ontwikkeling voor Linux gebaseerde HDInsight](hdinsight-hadoop-script-actions-linux.md). |
| **Azure CLI** (serverzijde scripts) | Terwijl de CLI Azure op Linux beschikbaar is, is niet afkomstig vooraf geïnstalleerde HDInsight hoofd knooppunten. Als u deze nodig voor het uitvoeren van scripts servers, ziet u [de CLI Azure installeren](../xplat-cli-install.md) voor informatie over het installeren op Linux gebaseerde platforms. |
| **.NET-onderdelen** | .NET wordt niet volledig ondersteund op HDInsight Linux gebaseerde clusters. Linux gebaseerde Storm op HDInsight clusters C# Storm topologieën met het framework SCP.NET na 10-28/2017 ondersteuning hebt gemaakt. Extra ondersteuning voor .NET wordt toegevoegd in toekomstige updates. |
| **Win32-onderdelen of andere technologie voor Windows alleen-lezen** | Richtlijnen is afhankelijk van het onderdeel of technologie; u mogelijk een versie die compatibel is met Linux te vinden, of u wilt zoeken als alternatief of herschreven fragment dit onderdeel. |

##<a name="cluster-creation"></a>Cluster maken

In deze sectie bevat informatie over de verschillen in cluster maken.

### <a name="ssh-user"></a>SSH gebruiker

Linux gebaseerde HDInsight clusters gebruikmaken van het protocol **Secure Shell (SSH)** voor externe toegang tot de knooppunten. In tegenstelling tot op basis van een extern bureaublad voor Windows kolomgroepen meeste SSH clients bieden geen een grafische gebruikerservaring, maar in plaats daarvan biedt een opdrachtregel waarmee u kunt de opdrachten op het cluster uitvoeren. Bij sommige clients (zoals [MobaXterm](http://mobaxterm.mobatek.net/),) bieden een grafisch bestand systeem browser naast een externe opdrachtregel.

Gemaakt, moet u een gebruiker SSH en een **wachtwoord** of **certificaat met openbare sleutel** opgeven voor verificatie.

We raden u aan gebruiken certificaat openbare sleutel, meer veilig dan met een wachtwoord. Verificatie via clientcertificaat werkt door te genereren van een ondertekend combinatie van de openbare/persoonlijke-sleutel en vervolgens de openbare sleutel leveren bij het maken van het cluster. Wanneer u verbinding maakt met de server via SSH, biedt de persoonlijke sleutel op de client verificatie voor de verbinding.

Zie voor meer informatie over het gebruik van SSH met HDInsight:

- [SSH gebruiken met HDInsight vanuit Windows-clients](hdinsight-hadoop-linux-use-ssh-windows.md)

- [SSH gebruiken met HDInsight vanuit Linux, Unix en OS X-clients](hdinsight-hadoop-linux-use-ssh-unix.md)

### <a name="cluster-customization"></a>Cluster aanpassing

**Scriptacties** gebruikt met Linux gebaseerde clusters moet worden geschreven in script we vaker doen. Terwijl scriptacties kunnen worden gebruikt tijdens het maken van cluster, voor Linux gebaseerde clusters kunnen ze ook worden gebruikt voor aanpassing uitgevoerd nadat een cluster omhoog is en worden uitgevoerd. Zie voor meer informatie [aanpassen Linux gebaseerde HDInsight via scriptacties](hdinsight-hadoop-customize-cluster-linux.md) en [Script actie ontwikkeling voor Linux gebaseerde HDInsight](hdinsight-hadoop-script-actions-linux.md).

Functie voor het aanpassen van een andere is **bootstrap**. Voor Windows kolomgroepen kunt hiermee u de locatie van extra bibliotheken voor gebruik met onderdeel opgeven. Na het maken van het cluster deze bibliotheken zijn automatisch beschikbaar zijn voor gebruik met component-query's zonder hoeft te gebruiken `ADD JAR`.

Bootstrap voor Linux gebaseerde clusters biedt geen deze functionaliteit. Gebruik in plaats daarvan scriptactie beschreven in [bibliotheken component toevoegen tijdens het maken van cluster](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Virtuele netwerken

Op basis van Windows HDInsight clusters alleen werken met klassieke virtuele netwerken, terwijl HDInsight Linux gebaseerde clusters resourcemanager virtuele netwerken vereisen. Als u resources in een klassieke virtuele netwerk dat het cluster Linux-HDInsight verbinding met maken moet hebt, raadpleegt u [verbinden van een klassieke virtueel netwerk met een resourcemanager virtuele netwerk](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Zie voor meer informatie over configuratievereisten voor het gebruik van Azure virtuele netwerken met HDInsight, [mogelijkheden voor HDInsight uitbreiden met behulp van een virtueel netwerk](hdinsight-extend-hadoop-virtual-network.md).

##<a name="management-and-monitoring"></a>Beheer en bewaking

Veel van het web UI's die u hebt opgegeven met Windows gebaseerde HDInsight, zoals werkervaring of garens UI, zijn beschikbaar via Ambari. Daarnaast bevat de weergave Ambari component een manier om uit te voeren component query's via uw webbrowser. De gebruikersinterface van de Web Ambari is beschikbaar op Linux gebaseerde clusters bij https://CLUSTERNAME.azurehdinsight.net.

Zie de volgende documenten voor meer informatie over het werken met Ambari:

- [Ambari Web](hdinsight-hadoop-manage-ambari.md)

- [Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari waarschuwingen

Ambari heeft een systeem voor waarschuwingen die u met potentiële problemen met het cluster kunt zien. Waarschuwingen worden weergegeven als rode of gele vermeldingen in de gebruikersinterface van het Web Ambari, maar u ze ook via de REST API ophalen kunt.

> [AZURE.IMPORTANT] Ambari waarschuwingen geven aan dat er *mogelijk* een probleem, niet dat er *is* een probleem. Bijvoorbeeld mogelijk ontvangt u een melding dat HiveServer2 niet toegankelijk, hoewel u normaal gesproken kan openen.
>
> Veel meldingen worden geïmplementeerd als query's op basis van het interval voor een service en verwacht een antwoord in een bepaalde periode. Zodat de waarschuwing betekent niet u de service wordt dat ingedrukt, retourneren niet alleen dat deze resultaten binnen de verwachte tijd.

In het algemeen, moet u evalueren of een melding voor een langere periode heeft zijn optreedt, komt overeen met de problemen van de gebruikers die eerder zijn gerapporteerd met het cluster voordat u een actie op is geïnstalleerd.

##<a name="file-system-locations"></a>Systeem bestandslocaties

Het bestandssysteem Linux cluster is anders dan op basis van Windows HDInsight clusters ingedeeld. Gebruik de volgende tabel als u bestanden kunt vinden vaak gebruikt.

| Ik wil zoeken... | Het is nu bevindt. |
| ----- | ----- |
| Configuratie | `/etc`. Bijvoorbeeld:`/etc/hadoop/conf/core-site.xml` |
| Logboekbestanden | `/var/logs` |
| Hortonworks gegevens Platform (HDP) | `/usr/hdp`. Er zijn twee mappen bevinden hier dat is de huidige versie van de HDP (bijvoorbeeld `2.2.9.1-1`,) en `current`. De `current` directory bevat symbolic koppelingen naar bestanden en mappen in de adreslijst van versie-getal en wordt geleverd als een handige manier HDP-bestanden openen sinds de versie van getal wordt wijzigen, zoals de HDP-versie wordt bijgewerkt. |
| hadoop-streaming.jar | `/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Als u de naam van het bestand, kunt u de volgende opdracht uit een sessie SSH in het algemeen, gebruiken om het bestandspad:

    find / -name FILENAME 2>/dev/null

U kunt ook jokertekens gebruiken met de bestandsnaam. Bijvoorbeeld `find / -name *streaming*.jar 2>/dev/null` retourneert het pad naar een oppervlak-bestanden met het woord streaming als onderdeel van de bestandsnaam.

##<a name="hive-pig-and-mapreduce"></a>Component varken en MapReduce

Varken en MapReduce werkbelastingen lijken sterk op Linux gebaseerde clusters - het belangrijkste verschil is als u extern bureaublad gebruiken om te verbinden met een cluster op basis van Windows en taken uitvoeren, kunt u SSH met Linux gebaseerde clusters gebruiken wilt.

- [Varken met SSH gebruiken](hdinsight-hadoop-use-pig-ssh.md)

- [MapReduce gebruiken met SSH](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Component

De volgende tabel bevat richtlijnen over het migreren van uw werkbelasting component.

| Klik op op basis van Windows, ik gebruik... | Klik op Linux gebaseerde... |
| ----- | ----- |
| **Component-Editor** | [Weergave in Ambari component](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`Tez inschakelen | Tez is de standaard execution engine voor Linux gebaseerde kolomgroepen, dus de set-instructie is niet meer nodig. |
| CMD-bestanden of scripts op de server opgeroepen als onderdeel van een taak component | we vaker doen scripts gebruiken |
| `hive`opdracht Extern bureaublad | [Beeline](hdinsight-hadoop-use-hive-beeline.md) of [component vanuit een sessie SSH](hdinsight-hadoop-use-hive-ssh.md) gebruiken |

##<a name="storm"></a>Storm

| Klik op op basis van Windows, ik gebruik... | Klik op Linux gebaseerde... |
| ----- | ----- |
| Storm Dashboard | Het Dashboard Storm is niet beschikbaar. Zie [Deploy en Storm beheren topologieën op Linux gebaseerde HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md) voor manieren om in te dienen topologieën |
| Storm UI | De gebruikersinterface Storm is beschikbaar binnen https://CLUSTERNAME.azurehdinsight.net/stormui |
| Visual Studio kunt maken, implementeren en C# of hybride topologieën beheren | Visual Studio kan worden gebruikt om te maken, implementeren en C# (SCP.NET) of hybride topologieën op Linux gebaseerde Storm op HDInsight clusters gemaakt na 10-28/2017 beheren. |

##<a name="hbase"></a>HBase

Klik op Linux gebaseerde kolomgroepen de bovenliggende znode voor HBase is `/hbase-unsecure`. U moet deze instellen in de configuratie voor elke Java-client toepassingen die systeemeigen HBase Java-API gebruiken.

Zie [een toepassing Java gebaseerde HBase maakt](hdinsight-hbase-build-java-maven.md) voor een voorbeeld-client die deze waarde ingesteld.

##<a name="spark"></a>Elektrische

Elektrische clusters zijn beschikbaar op Windows-clusters tijdens voorbeeld; echter voor release is elektrische alleen beschikbaar met Linux gebaseerde clusters. Er is geen migratiepad van een Windows-programma een preview cluster naar een release elektrische Linux gebaseerde cluster.

##<a name="known-issues"></a>Bekende problemen

### <a name="azure-data-factory-custom-net-activities"></a>Azure Data Factory aangepaste .NET-activiteiten

Azure Data Factory aangepaste .NET-activiteiten worden momenteel niet ondersteund op HDInsight Linux gebaseerde clusters. In plaats daarvan moet u een van de volgende methoden voor het implementeren van aangepaste activiteiten als onderdeel van uw verkooppijplijn ADF.

-   .NET-activiteiten uitvoeren op Azure Batch toepassingen. Zie het gedeelte van de service gebruik Azure Batch gekoppeld van [aangepaste activiteiten gebruiken in een pijplijn Factory van Azure-gegevens](../data-factory/data-factory-use-custom-activities.md#AzureBatch)

-   Implementeer de activiteit als een activiteit in een MapReduce. Zie [MapReduce-programma's van gegevens Factory roepen](../data-factory/data-factory-map-reduce.md) voor meer informatie.

### <a name="line-endings"></a>Regeleinden

Regeleinden op Windows-systemen gebruiken in het algemeen, CRLF, terwijl Linux-systemen LF gebruiken. Als u produceren of, gegevens met CRLF regeleinden verwachten, moet u mogelijk de producenten of consumenten die werken met het einde van de regel LF wijzigen.

Bijvoorbeeld, retourneert Azure PowerShell gebruiken voor de query HDInsight op een Windows gebaseerde cluster gegevens met CRLF. Dezelfde query met een cluster Linux gebaseerde retourneert LF. In veel gevallen dit maakt niet uit aan de consumenten gegevens, maar deze moet worden onderzocht voordat u migreert naar een cluster Linux gebaseerde.

Als u scripts die wordt uitgevoerd rechtstreeks op Linux-knooppunten (zoals een Python script gebruikt met component of een taak MapReduce) hebt, moet u altijd LF gebruiken als het einde van de regel. Als u CRLF gebruikt, ziet u mogelijk fouten bij het uitvoeren van scripts op een cluster Linux gebaseerde.

Als u weet dat de scripts geen tekenreeksen met ingesloten CR tekens bevatten, kunt u wijzigen bulksgewijs de regeleinden met een van de volgende methoden:

-   **Als u scripts die u van plan bent uploaden naar het cluster hebt**, gebruikt u de volgende PowerShell-instructies wijzigen van de regeleinden van CRLF in LF voordat u het script uploadt naar het cluster.

        $original_file ='c:\path\to\script.py'
        $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
        [IO.File]::WriteAllText($original_file, $text)

-   **Als er scripts die zich al in de opslag door het cluster gebruikt**, kunt u de volgende opdracht uit een SSH-sessie met het cluster Linux gebaseerde gebruiken voor het wijzigen van het script.

        hdfs dfs -get wasbs:///path/to/script.py oldscript.py
        tr -d '\r' < oldscript.py > script.py
        hdfs dfs -put -f script.py wasbs:///path/to/script.py

##<a name="next-steps"></a>Volgende stappen

-   [Informatie over het maken van HDInsight Linux gebaseerde clusters](hdinsight-hadoop-provision-linux-clusters.md)

-   [Verbinding maken met een Linux gebaseerde cluster met SSH vanaf een Windows-client](hdinsight-hadoop-linux-use-ssh-windows.md)

-   [Verbinding maken met een Linux gebaseerde cluster via SSH van een client Linux, Unix of Mac](hdinsight-hadoop-linux-use-ssh-unix.md)

-   [Een Linux gebaseerde cluster met Ambari beheren](hdinsight-hadoop-manage-ambari.md)
