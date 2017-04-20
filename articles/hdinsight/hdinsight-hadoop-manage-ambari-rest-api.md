<properties
   pageTitle="Bewaken en beheren van HDInsight clusters met de Apache Ambari REST API | Microsoft Azure"
   description="Informatie over het gebruik van Ambari om te controleren en beheren van HDInsight Linux gebaseerde clusters. In dit document leert u hoe de Ambari REST API wordt geleverd bij HDInsight clusters kunt gebruiken."
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
   ms.date="09/20/2016"
   ms.author="larryfr"/>

#<a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>HDInsight clusters beheren met behulp van de Ambari REST API

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari vergemakkelijkt het beheer en controle van een Hadoop-cluster doordat een eenvoudig te web UI en REST API gebruiken. Ambari is opgenomen op HDInsight Linux gebaseerde clusters en het cluster bewaken en wijzigingen in de configuratie wordt gebruikt. In dit document leert u de basisbeginselen van het werken met de Ambari REST API door het uitvoeren van veelvoorkomende taken met krul.

##<a name="prerequisites"></a>Vereisten voor

* [omslaan](http://curl.haxx.se/): krul is een hulpprogramma voor het platforms die kan worden gebruikt om te werken met REST API's vanaf de opdrachtregel. In dit document, wordt deze gebruikt om te communiceren met de Ambari REST API.
* [jq](https://stedolan.github.io/jq/): jq is een platforms opdrachtregel hulpprogramma voor het werken met JSON-documenten. In dit document, wordt deze gebruikt de JSON-documenten als resultaat gegeven van de Ambari REST API parseren.
* [Azure CLI](../xplat-cli-install.md): een hulpprogramma voor meerdere platforms de opdrachtregel voor het werken met Azure-services.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##<a id="whatis"></a>Wat is Ambari?

[Apache Ambari](http://ambari.apache.org) zorgt ervoor dat Hadoop management eenvoudiger doordat een web eenvoudig te gebruiken gebruikersinterface die kan worden gebruikt om inrichten, beheren en controleren van Hadoop clusters. Ontwikkelaars kunnen deze mogelijkheden in hun toepassingen integreren met behulp van de [Ambari REST API's](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari is al dan niet standaard met HDInsight Linux gebaseerde clusters opgegeven.

##<a name="rest-api"></a>REST API

De basis-URI voor de Ambari REST API op HDInsight is https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, waar __CLUSTERNAAM__ is de naam van uw cluster.

> [AZURE.IMPORTANT] De naam van het cluster in het deel van de FQDN-naam (Fully Qualified) van de URI (CLUSTERNAME.azurehdinsight.net) is niet hoofdlettergevoelig, zijn andere vermeldingen in de URI hoofdlettergevoelig. Bijvoorbeeld: als uw cluster mijncluster heet, volgen geldige URI's:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> De volgende URI's, wordt er een fout geretourneerd omdat het tweede exemplaar van de naam is niet het juiste hoofdlettergebruik.
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

Verbinding maken met Ambari op HDInsight vereist HTTPS. Bij het verifiëren van de verbinding, moet u de naam van de admin-account (de standaardinstelling is __beheerder__) en het wachtwoord die u hebt opgegeven toen het cluster is gemaakt.

Het volgende voorbeeld wordt krul een GET-aanvraag tegen de REST API te doen. __Wachtwoord__ vervangen door de admin-wachtwoord voor het cluster. __CLUSTERNAAM__ vervangen door de naam van het cluster:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
Het antwoord is een JSON-document die begint met informatie ongeveer als volgt uit:

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

Aangezien dit JSON, is het eenvoudiger een JSON-parser gebruiken om te werken met de gegevens. Bijvoorbeeld het volgende voorbeeld jq wordt alleen wilt laten de `health_report` element.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'

##<a name="example-get-the-fqdn-of-cluster-nodes"></a>Voorbeeld: De FQDN van knooppunten ophalen

Tijdens het werken met HDInsight, moet u mogelijk de FQDN-naam (Fully Qualified Domain Name) van een clusterknooppunt weten. U kunt eenvoudig de FQDN-naam voor de verschillende knooppunten in het gebruik van de volgende cluster ophalen:

* __Hoofd knooppunten__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __Werknemer knooppunten__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Zookeeper knooppunten__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Opmerking dat elk van deze voorbeelden hetzelfde patroon volgen:

1. Een onderdeel dat we weten query wordt uitgevoerd op die knooppunten.
2. Ophalen de `host_name` elementen, die de FQDN-naam voor deze knooppunten bevatten.

De `host_components` element van de afzender document meerdere items bevat. Met `.host_components[]`, en vervolgens op te geven een pad binnen het element wordt doorlopen van elk item en de waarde van het specifieke pad uitlichten. Desgewenst kunt u slechts één waarde, zoals het eerste item van de FQDN-naam, kunt u de items retourneren als een verzameling en selecteer vervolgens de invoer in een specifieke:

    jq '[.host_components[].HostRoles.host_name][0]'

Dit geeft als resultaat de eerste FQDN-naam van de siteverzameling.

##<a name="example-get-the-default-storage-account-and-container"></a>Voorbeeld: Het standaardaccount voor opslagruimte en de container ophalen

Wanneer u een cluster HDInsight maakt, moet u een Azure opslag-Account en een container blob gebruiken als de standaard-opslag voor het cluster. U kunt Ambari gebruiken om op te halen deze informatie nadat het cluster is gemaakt. Bijvoorbeeld als u via een programma wilt schrijven gegevens rechtstreeks naar de container.

De volgende ophaalt de WASB URI van de opslag van de standaard clusters:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] Hiermee wordt de eerste configuratie toegepast op de server (`service_config_version=1`,) die deze informatie bevat. Als u een waarde die is gewijzigd na het maken van het cluster ophaalt, moet u mogelijk een lijst met de configuratie-versies en de meest recente item op te halen.

Hiermee wordt een waarde vergelijkbaar met het volgende voorbeeld, waarbij __CONTAINER__ is de standaardcontainer en __accountnaam__ is de naam van de Azure opslag Account:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Vervolgens kunt u deze gegevens gebruiken met de [Azure CLI](../xplat-cli-install.md) uploaden of downloaden van gegevens uit de container.

1. De resourcegroep krijgen voor de opslag-Account. __Accountnaam__ vervangen door de naam van het Account van de opslagruimte opgehaald uit Ambari:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Hiermee wordt de naam van de resource-groep voor het account.
    
    > [AZURE.NOTE] Als er niets door deze opdracht wordt geretourneerd, moet u mogelijk de CLI Azure Azure resourcemanager modus wijzigen en de opdracht opnieuw uitvoeren. Als u wilt overschakelen naar resourcemanager Azure-modus, gebruikt u de volgende opdracht:
    >
    > `azure config mode arm`
    
2. Krijgen de toets voor de opslag-account. __GROEPSNAAM__ vervangen door de resourcegroep uit de vorige stap. __Accountnaam__ vervangen door de opslagruimte accountnaam:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    In dit voorbeeld wordt de primaire sleutel voor het account.
    
3. Gebruik de opdracht uploaden naar een bestand opslaan in de container:

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    __Accountnaam__ vervangen door de naam van de opslag-Account. __ACCOUNTKEY__ vervangen door de sleutel die eerder opgehaald. __BESTANDSPAD__ is het pad naar het bestand dat u uploaden wilt, terwijl __BLOBPATH__ het pad tussen de container is.

    Als u het bestand wilt weergeven in HDInsight aan wasbs://example/data/filename.txt wilt, klikt u vervolgens __BLOBPATH__ zou bijvoorbeeld `example/data/filename.txt`.

##<a name="example-update-ambari-configuration"></a>Voorbeeld: Update-Ambari configuratie

1. De huidige configuratie, waarin Ambari worden opgeslagen als de "gewenste configuratie' ophalen:

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    In dit voorbeeld geeft als resultaat een JSON-document met de huidige configuratie (aangeduid met een waarde van het _label_ ) voor de onderdelen die op het cluster zijn geïnstalleerd. Het volgende voorbeeld is een uittreksel uit de gegevens uit een elektrische clustertype geretourneerd.
    
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }

    In deze lijst, moet u de naam van het onderdeel kopiëren (bijvoorbeeld __elektrische\_thrift\_sparkconf__ en de waarde van het __label__ .
    
2. De configuratie voor het onderdeel en tag ophalen met behulp van de volgende opdracht uit. __Elektrische-thrift-sparkconf__ en __AANVANKELIJKE__ vervangen door het onderdeel en de code die u wilt ophalen van de configuratie voor.

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    
    Krul haalt de JSON-document en klik vervolgens jq wordt gebruikt om te wijzigingen aanbrengen in de gegevens om te maken van een sjabloon. De sjabloon wordt vervolgens gebruikt om de configuratiewaarden van de toevoegen/wijzigen. Specifiek dit gebeurt het volgende:
    
    * Hiermee maakt u een unieke waarde die de tekenreeks 'versie' en de datum die is opgeslagen in __newtag__bevat.
    * Hiermee maakt u een document hoofdmap voor de nieuwe gewenste configuratie.
    * Wordt de inhoud van de `.items[]` matrix en klik onder het element __desired_config__ toegevoegd.
    * Hiermee verwijdert u de __href__, __versie__en __Config__ -elementen, zoals deze elementen niet meer nodig hebt om in te dienen van een nieuwe configuratie.
    * Een nieuwe __tag__ -element wordt toegevoegd en wordt de waarde ingesteld op __versie ###__. Het numerieke gedeelte is gebaseerd op de huidige datum. Elke configuratie moet een unieke code hebben.
    
    Ten slotte wordt de gegevens opgeslagen in het document __newconfig.json__ . De documentstructuur moet worden de volgende strekking weergegeven:
    
        {
            "Clusters": {
                "desired_config": {
                "tag": "version1459260185774265400",
                "type": "spark-thrift-sparkconf",
                "properties": {
                    ....
                 },
                 "properties_attributes": {
                     ....
                 }
            }
        }

3. Open de __newconfig.json__ document en wijzigen/toevoegen de waarden in het object __Eigenschappen__ . Het volgende voorbeeld wordt de waarde van __"spark.yarn.am.memory"__ uit __"1 g"__ naar __"3 g"__ en, en een nieuw element voor met een waarde van __"256 m"__, __"spark.kryoserializer.buffer.max"__ toevoegt.

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    Sla het bestand als u klaar bent met wijzigingen aanbrengen.

4. Gebruik de volgende opdracht uit om in te dienen de bijgewerkte configuratie naar Ambari.

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    Deze opdracht buizen de inhoud van het bestand __newconfig.json__ naar de aanvraag krul, die dit naar het cluster als de nieuwe gewenste configuratie verstuurt. Het verzoek krul geeft als resultaat een JSON-document. Het element __versionTag__ in dit document moet overeenkomen met de versie die u hebt verzonden en het __configuraties__ -object bevat de aangevraagde configuratiewijzigingen.

###<a name="example-restart-a-service-component"></a>Voorbeeld: Een onderdeel van de service opnieuw starten

Op dit moment als u het web Ambari UI bekijkt, wordt de een-service aangegeven dat deze opnieuw worden gestart moeten voordat de nieuwe configuratie van kracht. Gebruik de volgende stappen uit om de service opnieuw te starten.

1. Gebruik de volgende onderhoudsmodus voor de een-service in te schakelen:

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Deze opdracht stuurt een JSON-document naar de server (opgenomen in de `echo` instructie,) waarin onderhoudsmodus ingeschakeld.
    U kunt controleren of de service is nu in de onderhoudsmodus voor met de volgende aanvraag:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    Hiermee herstelt u een waarde van `"ON"`.

3. Gebruik vervolgens de volgende handelingen uit de service om uit te schakelen:

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    Deze opdracht geeft als resultaat een reactie ongeveer als volgt uit.
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    De `href` waarde die het resultaat van deze URI het interne IP-adres van het clusterknooppunt wordt gebruikt. Als u wilt gebruiken van buiten het cluster, kunt u het gedeelte '10.0.0.18:8080' vervangen door de FQDN-naam van het cluster. Bijvoorbeeld haalt de volgende opdracht de status van de aanvraag kunt invullen.
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    Als dit geeft als een waarde van resultaat `"COMPLETED"` en vervolgens de aanvraag is voltooid.

4. Zodra het vorige verzoek is voltooid, gebruikt u de volgende handelingen uit om de service te starten.

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Zodra de service opnieuw is opgestart, is de nieuwe configuratie-instellingen.

5. Ten slotte, gebruikt u de volgende onderhoudsmodus wilt uitschakelen.

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##<a name="next-steps"></a>Volgende stappen

Zie voor volledige informatie van de REST API [Ambari API verwijzing V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

> [AZURE.NOTE] Sommige functies Ambari is uitgeschakeld, zoals deze wordt beheerd door de HDInsight-cloudservice; bijvoorbeeld: toevoegen of verwijderen van hosts uit het cluster of nieuwe services toevoegen.
