<properties
   pageTitle="Tips voor het gebruik van Hadoop op Linux gebaseerde HDInsight | Microsoft Azure"
   description="Hier implementatie tips voor het gebruik van clusters Linux gebaseerde HDInsight (Hadoop) op een vertrouwde Linux-omgeving uitgevoerd in de cloud Azure."
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
   ms.date="09/13/2016"
   ms.author="larryfr"/>

# <a name="information-about-using-hdinsight-on-linux"></a>Informatie over het gebruik van HDInsight op Linux

Linux gebaseerde Azure HDInsight clusters bieden Hadoop op een vertrouwde Linux-omgeving, uitgevoerd in de cloud Azure. Voor de meeste mogelijke, moet deze werken exact wilt weergeven zoals elke andere Hadoop-op-Linux-installatie. In dit document ziet u specifieke verschillen waarmee u houden moet rekening.

##<a name="prerequisites"></a>Vereisten voor

Veel van de stappen in dit document gebruikt de volgende hulpprogramma's, die mogelijk moeten worden geïnstalleerd op uw systeem.

* [omslaan](https://curl.haxx.se/) - gebruikt om te communiceren met webservices
* [jq](https://stedolan.github.io/jq/) - gebruikt bij het parseren van JSON-documenten
* [Azure CLI](../xplat-cli-install.md) - gebruikt voor het beheren van extern Azure services

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

## <a name="domain-names"></a>Domeinnamen

De naam van de FQDN (Fully Qualified) wilt gebruiken wanneer u verbinding maakt met het cluster van internet ** &lt;Clusternaam >. azurehdinsight.net** of (voor alleen SSH) ** &lt;clusternaam-ssh >. azurehdinsight.net**.

Intern, heeft elke knooppunt in het cluster een naam die tijdens de configuratie van het cluster wordt toegewezen. Als u de clusternamen zoekt, kunt u bezoekt u de pagina __Hosts__ op de gebruikersinterface van de Web Ambari of gebruik de volgende handelingen uit om te retourneren van een lijst met hosts uit de Ambari REST API:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

__Wachtwoord__ vervangen door het wachtwoord van het beheerdersaccount en __CLUSTERNAAM__ met de naam van uw cluster. Hiermee herstelt u een JSON-document met een lijst met de hosts in het cluster en klik vervolgens jq worden opgehaald uit de `host_name` elementwaarde voor elke host in het cluster.

Als u nodig hebt om de naam van het knooppunt voor een specifieke services, kunt u Ambari zoeken voor dat onderdeel. Bijvoorbeeld, als u zoekt de hosts voor het knooppunt van de naam HDFS, gebruikt u het volgende.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Hiermee wordt een JSON-document met een beschrijving van de service en vervolgens jq worden opgehaald uit alleen de `host_name` waarde voor de hosts.

## <a name="remote-access-to-services"></a>Externe toegang tot services

* **Ambari (web)** - https://&lt;Clusternaam >. azurehdinsight.net

    Verifiëren met behulp van de gebruiker van de beheerder cluster en het wachtwoord en klikt u vervolgens aanmelden bij Ambari. Dit wordt ook gebruikt de beheerder van de cluster en het wachtwoord.

    Verificatie is leesbare - altijd HTTPS gebruiken om ervoor te zorgen dat de verbinding beveiligd is.

    > [AZURE.IMPORTANT] Terwijl Ambari voor uw cluster toegankelijk rechtstreeks via Internet is, afhankelijk van bepaalde functies toegang krijgen tot knooppunten door de interne domeinnaam die wordt gebruikt door het cluster. Aangezien dit een interne domeinnaam is en niet openbare, u "-server niet gevonden" fouten ontvangt bij het openen van bepaalde functies via Internet.
    >
    > Om de volledige functionaliteit van het web Ambari UI een tunnel SSH naar proxy webverkeer naar het hoofd clusterknooppunt te gebruiken. Zie [Gebruik SSH tunnel naar Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, en andere web van UI openen](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** - https://&lt;Clusternaam >.azurehdinsight.net/ambari

    > [AZURE.NOTE] Verifiëren met behulp van de gebruiker van de beheerder cluster en het wachtwoord.
    >
    > Verificatie is leesbare - altijd HTTPS gebruiken om ervoor te zorgen dat de verbinding beveiligd is.

* **WebHCat (Templeton)** - https://&lt;Clusternaam >.azurehdinsight.net/templeton

    > [AZURE.NOTE] Verifiëren met behulp van de gebruiker van de beheerder cluster en het wachtwoord.
    >
    > Verificatie is leesbare - altijd HTTPS gebruiken om ervoor te zorgen dat de verbinding beveiligd is.

* **SSH** - &lt;Clusternaam >-ssh.azurehdinsight.net op poort 22 of 23. Poort 22 wordt verbinding maken met de primaire headnode, terwijl 23 wordt gebruikt om te verbinden met de secundaire gebruikt. Zie voor meer informatie over het hoofd knooppunten, [beschikbaarheid en betrouwbaarheid van Hadoop clusters in HDInsight](hdinsight-high-availability-linux.md).

    > [AZURE.NOTE] U kunt alleen toegang tot hoofd knooppunten via SSH vanaf een clientcomputer. Zodra u verbinding hebt, kunt u de knooppunten werknemer kunt vervolgens via SSH van een headnode openen.

## <a name="file-locations"></a>Bestandslocaties

Hadoop-gerelateerde bestanden vindt u knooppunten op `/usr/hdp`. Deze map bevat de volgende submappen:

* __2.2.4.9-1__: deze map met de naam voor de versie van het Hortonworks gegevens Platform gebruikt door HDInsight, zodat het nummer op uw cluster mogelijk anders dan hier wordt vermeld.
* __huidige__: deze map bevat koppelingen naar mappen onder de map __2.2.4.9-1__ en ervoor te zorgen dat u niet hoeft te typen van een versienummer (dat kan veranderen), elke keer dat u wilt een bestand openen.

Voorbeeldgegevens en oppervlak-bestanden kunnen u vinden op Hadoop Distributed bestand System (HDFS) of Azure Blob storage op ' / voorbeeld ' of ' wasbs: / / / voorbeeld '.

## <a name="hdfs-azure-blob-storage-and-storage-best-practices"></a>HDFS, Azure-blobopslag en aanbevolen procedures opslag

In de meeste Hadoop onderzoeken hoort HDFS door lokale opslag op de computers in het cluster. Dit is een efficiënte, kan het zijn dure voor een cloud-oplossing waar wordt geheven per uur of door minuten voor berekeningscluster resources.

Azure-blobopslag HDInsight gebruikt als het standaardarchief de volgende voordelen biedt:

* Goedkope langdurige opslag

* Toegankelijkheid van externe services zoals websites, bestand uploaden/downloaden van hulpprogramma's, verschillende taal SDK's en webbrowsers

Omdat de standaard-store voor HDInsight, hebt u gewoonlijk niet iets doen om dit gebruiken. De volgende opdracht wordt bijvoorbeeld een lijst met bestanden in de map **/example/data** , die is opgeslagen op Azure-blobopslag:

    hdfs dfs -ls /example/data

Sommige opdrachten mogelijk moet u opgeven dat u blobopslag gebruikt. Voor deze, kunt u de opdracht met voorvoegsel **wasb: / /**, of **wasbs: / /**.

HDInsight kunt u meerdere Blob storage-accounts koppelen aan een cluster. Voor toegang tot gegevens op een niet-standaard Blob storage-account, kunt u de indeling **wasbs: / /&lt;container-name>@&lt;accountnaam >.blob.core.windows.net/**. Bijvoorbeeld het volgende wordt een lijst met de inhoud van de map **/example/data** voor de opgegeven container en Blob storage-account:

    hdfs dfs -ls wasbs://mycontainer@mystorage.blob.core.windows.net/example/data

### <a name="what-blob-storage-is-the-cluster-using"></a>Welke blobopslag is het cluster via?

Tijdens het maken van cluster u hebt geselecteerd voor het gebruik van een bestaande opslag van Azure-account en de container of een nieuw account te maken. Vervolgens vergeten u waarschijnlijk bent. U vindt het standaardaccount voor opslagruimte en de container met behulp van de Ambari REST API.

1. Gebruik van de volgende opdracht uit om op te halen HDFS configuratiegegevens met krul en filteren met de [jq](https://stedolan.github.io/jq/):

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
    > [AZURE.NOTE] Hiermee herstelt u de eerste configuratie toegepast op de server (`service_config_version=1`,) bevat dat deze informatie. Als u een waarde die is gewijzigd na het maken van het cluster ophaalt, moet u mogelijk een lijst met de configuratie-versies en de meest recente item op te halen.

    Hiermee herstelt u een waarde vergelijkbaar met de volgende opties, waarbij __CONTAINER__ is de standaardcontainer en __accountnaam__ is de naam van de Azure opslag Account:

        wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

1. De resourcegroep krijgen voor de opslag-Account, de [Azure CLI](../xplat-cli-install.md)gebruiken. In de volgende opdracht uit, kunt u __accountnaam__ vervangen door de naam van het Account van de opslagruimte opgehaald uit Ambari:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Hiermee herstelt u de naam van de resource-groep voor het account.
    
    > [AZURE.NOTE] Als er niets door deze opdracht wordt geretourneerd, moet u mogelijk de CLI Azure Azure resourcemanager modus wijzigen en de opdracht opnieuw uitvoeren. Als u wilt overschakelen naar resourcemanager Azure-modus, gebruikt u de volgende opdracht uit.
    >
    > `azure config mode arm`
    
2. Krijgen de toets voor de opslag-account. __GROEPSNAAM__ vervangen door de resourcegroep uit de vorige stap. __Accountnaam__ vervangen door de opslagruimte accountnaam:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.[0].value'

    Hiermee herstelt u de primaire sleutel voor het account.

U kunt ook de opslaggegevens met behulp van de Portal Azure vinden:

1. Selecteer in de [Portal van Azure](https://portal.azure.com/)uw cluster HDInsight.

2. Selecteer __alle instellingen__in de sectie __Essentials__ .

3. __Instellingen__en selecteer __Azure opslag toetsen__.

4. Selecteer een van de opslagruimte rekeningen in __Azure opslag toetsen__. Hiermee wordt informatie over de opslag-account weergegeven.

5. Selecteer het pictogram van de sleutel. Sneltoetsen voor dit account opslag wordt weergegeven.

### <a name="how-do-i-access-blob-storage"></a>Hoe krijg ik toegang tot blobopslag?

Anders dan via de Hadoop-opdracht uit het cluster, zijn er verschillende manieren voor toegang tot BLOB's:

* [Azure CLI voor Mac, Linux en Windows](../xplat-cli-install.md): opdrachtregel de opdrachten voor het werken met Azure. Na de installatie, gebruikt u de `azure storage` command voor informatie over het gebruik van opslag, of `azure blob` voor blob-specifieke opdrachten.

* [blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): een python script voor het werken met BLOB's in Azure opslag.

* Diverse SDK's:

    * [Java](https://github.com/Azure/azure-sdk-for-java)

    * [Node.js](https://github.com/Azure/azure-sdk-for-node)

    * [PHP](https://github.com/Azure/azure-sdk-for-php)

    * [Python](https://github.com/Azure/azure-sdk-for-python)

    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)

    * [.NET](https://github.com/Azure/azure-sdk-for-net)

* [Opslag REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx)

##<a name="scaling"></a>Schaalbaarheid van uw cluster

De schaal van de functie cluster kunt u het aantal gegevensknooppunten die worden gebruikt door een cluster die wordt uitgevoerd op Azure HDInsight zonder te verwijderen en opnieuw maken het cluster wijzigen.

U kunt schaal bewerkingen terwijl andere taken uitvoeren of processen worden uitgevoerd op een cluster.

De typen ander cluster worden beïnvloed door schaalbaarheid als volgt:

* __Hadoop__: bij het aantal knooppunten in een cluster, enkele van de services in het cluster opnieuw worden gestart. Hiermee kan leiden tot taken actief of in behandeling aan het einde van de schaal bewerking is mislukt. Zodra de bewerking voltooid is, kunt u de taken opnieuw indienen.

* __HBase__: regionale servers worden automatisch verdeeld binnen enkele minuten na voltooiing van de schaal bewerking. Als u wilt handmatig saldo vanaf regionale servers, gebruik de volgende stappen uit:

    1. Verbinding maken met het HDInsight cluster via SSH. Zie een van de volgende documenten voor meer informatie over het gebruik van SSH met HDInsight:

        * [SSH gebruiken met HDInsight uit Linux, Unix en Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

        * [SSH gebruiken met HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    1. Gebruik de volgende handelingen uit om te starten de shell HBase:

            hbase shell

    2. Nadat de shell HBase is geladen, gebruikt u de volgende handelingen uit om handmatig de regionale servers:

            balancer

* __Storm__: U moet een actieve Storm topologieën opnieuw nadat er is een schaal bewerking uitgevoerd. Hierdoor wordt de topologie parallellisme-instellingen op basis van het nieuwe aantal knooppunten in het cluster past. Als u wilt vastleggen actieve topologieën, gebruikt u een van de volgende opties:

    * __SSH__: verbinding maken met de server en de volgende opdracht uit een topologie opnieuw uit te gebruiken:

            storm rebalance TOPOLOGYNAME

        U kunt ook opgeven parameters als u wilt overschrijven de parallellisme hints oorspronkelijk is verstrekt door de topologie. Bijvoorbeeld `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` configureren, wordt de topologie 5 werknemer processen, 3 executors voor het onderdeel blauw-spout en 10 executors voor het onderdeel geel bout.

    * __Storm UI__: Gebruik de volgende stappen een topologie met behulp van de gebruikersinterface Storm opnieuw uit te.

        1. Open __https://CLUSTERNAME.azurehdinsight.net/stormui__ in uw webbrowser, waar CLUSTERNAAM de naam van uw cluster Storm is. Als u wordt gevraagd, voert u de HDInsight cluster (admin) beheerdersnaam en -wachtwoord die u hebt opgegeven bij het maken van het cluster.

        3. Selecteer de topologie die u wilt vastleggen en klik op de knop __opnieuw__ . Voer de vertraging voordat de bewerking rebalance wordt uitgevoerd.

Voor specifieke informatie over uw cluster HDInsight schaalbaarheid, raadpleegt u:

* [Hadoop clusters in HDInsight beheren met behulp van de Azure-Portal](hdinsight-administer-use-portal-linux.md#scaling)

* [Hadoop clusters in HDinsight beheren met behulp van Azure PowerShell](hdinsight-administer-use-command-line.md#scaling)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Hoe installeer ik kleurtoon (of een andere Hadoop-component)?

HDInsight is een beheerde service, wat betekent dat knooppunten in een cluster kunnen worden verwijderd en door automatisch de service door Azure als er een probleem is gevonden. Reden is het niet aanbevolen handmatig installeren dingen rechtstreeks op knooppunten. Gebruik in plaats daarvan [HDInsight scriptacties](hdinsight-hadoop-customize-cluster.md) wanneer u moet de volgende installeren:

* Een service of de website zoals elektrische of kleurtoon.
* Een onderdeel dat is vereist configuratiewijzigingen op meerdere knooppunten in de cluster. Bijvoorbeeld van een vereiste omgevingsvariabele maken van een map voor logboekregistratie of het maken van een configuratiebestand.

Scriptacties zijn we vaker doen scripts worden uitgevoerd tijdens het inrichten van cluster en kunnen worden gebruikt voor het installeren en configureren van extra onderdelen op het cluster. Voorbeeldscripts zijn beschikbaar voor de installatie van de volgende onderdelen:

* [Kleurtoon](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Zie voor informatie over het ontwikkelen van uw eigen Script-acties, [scriptactie ontwikkeling met HDInsight](hdinsight-hadoop-script-actions-linux.md).

###<a name="jar-files"></a>Oppervlak-bestanden

Vindt u enkele Hadoop-technologieën in zelfstandige oppervlak-bestanden die zijn functies die worden gebruikt als onderdeel van een taak MapReduce of uit bevatten binnen varken of component. Terwijl deze kunnen worden geïnstalleerd scriptacties gebruiken, kunnen ze vaak elke instelling niet vereisen en alleen worden geüpload naar het cluster na het inrichten en gebruikt rechtstreeks. Als u ervoor dat het onderdeel blijft reimaging van het cluster makle wilt, kunt u het oppervlak-bestand opslaan in WASB.

Bijvoorbeeld als u de nieuwste versie van [DataFu](http://datafu.incubator.apache.org/)gebruiken wilt, kunt u downloaden van een oppervlak dat het project bevat en uploadt dit naar het cluster HDInsight. Daarna volgt u de documentatie DataFu over het gebruiken van varken of component.

> [AZURE.IMPORTANT] Bepaalde onderdelen die zelfstandige oppervlak bestanden zijn HDInsight uw beschikking, maar zijn niet in het pad. Als u een bepaald onderdeel zoekt, kunt u de volgen om ernaar te zoeken op uw cluster:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Hiermee herstelt u het pad van alle overeenkomende oppervlak-bestanden.

Als het cluster al een versie van een onderdeel als een zelfstandig product oppervlak-bestand biedt, maar u wilt gebruiken, een andere versie, kunt u een nieuwe versie van het onderdeel uploaden naar het cluster en probeert u deze gebruikt in uw taken.

> [AZURE.WARNING] Onderdelen van het cluster HDInsight volledig worden ondersteund en Microsoft Support helpt isoleren en oplossen van problemen met deze onderdelen.
>
> Aangepaste onderdelen ontvangen commercieel redelijk ondersteuning om u te helpen u verder problemen met het probleem. Dit kan leiden tot het probleem oplossen of vraag of u kunt voeren beschikbare kanalen voor de bron openen technologieën waar uitgebreide expertise voor deze technologie is gevonden. Bijvoorbeeld, zijn er veel communitysites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache projecten tevens projectsites op [http://apache.org](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/), [een](http://spark.apache.org/).

## <a name="next-steps"></a>Volgende stappen

* [Migreren van Windows gebaseerde HDInsight naar Linux gebaseerde](hdinsight-migrate-from-windows-to-linux.md)
* [Component gebruiken met HDInsight](hdinsight-use-hive.md)
* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
* [Gebruik MapReduce taken met HDInsight](hdinsight-use-mapreduce.md)
