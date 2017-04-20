<properties
    pageTitle="Controleren van Hadoop clusters in HDInsight met de API Ambari | Microsoft Azure"
    description="Gebruik de Apache Ambari APIs voor het maken, beheren en Hadoop clusters cmdlets voor controle. Verberg de complexiteit van Hadoop intuïtieve operator hulpprogramma's en API's."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    editor="cgronlun"
    manager="jhubbard"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>Monitor met een Hadoop clusters in HDInsight de Ambari-API gebruiken

Informatie over het controleren van HDInsight clusters met behulp van Ambari APIs.

> [AZURE.NOTE] De informatie in dit artikel is hoofdzakelijk voor Windows gebaseerde HDInsight kolomgroepen waarmee een alleen-lezen-versie van de Ambari REST API. Zie voor Linux gebaseerde kolomgroepen [beheren Hadoop clusters Ambari gebruiken](hdinsight-hadoop-manage-ambari.md).

## <a name="what-is-ambari"></a>Wat is Ambari?

[Apache Ambari] [ ambari-home] wordt gebruikt voor het inrichten, beheren en Apache Hadoop clusters cmdlets voor controle. Het bevat een intuïtieve verzameling hulpprogramma's voor operator en een reeks robuuste API's die de complexiteit van Hadoop, de werking van clusters vereenvoudigen verbergen. Zie voor meer informatie over de API [Ambari API Reference][ambari-api-reference]. 

HDInsight ondersteunt momenteel alleen de Ambari controleren functie. Ambari API 1.0 wordt ondersteund door HDInsight versie 3.0 en 2.1 clusters. In dit artikel behandelt toegang tot Ambari APIs HDInsight versie 3.1 en 2.1 clusters. De belangrijkste verschillen tussen de twee is dat sommige onderdelen zijn gewijzigd door de introductie van nieuwe mogelijkheden (zoals de taak geschiedenis Server). 

**Vereisten voor**

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- (Optioneel) [omslaan] [curl]. Als u wilt installeren, raadpleegt u [omslaan Releases en Downloads][curl-download].

    >[AZURE.NOTE] Wanneer gebruikt u de opdracht krul in Windows, gebruik dubbele aanhalingstekens in plaats van enkele aanhalingstekens voor de optiewaarden.

- **Een Azure HDInsight cluster**. Zie voor instructies over de inrichting van cluster [aan de slag met HDInsight] [ hdinsight-get-started] of [inrichten HDInsight clusters][hdinsight-provision]. U moet de volgende gegevens via de zelfstudie:

    Cluster eigenschap|Azure PowerShell variabelennaam|Waarde|Beschrijving
    ---|---|---|---
    De naam van de cluster HDInsight|$clusterName||De naam van uw cluster HDInsight.
    Cluster gebruikersnaam|$clusterUsername||De naam van de gebruiker cluster opgegeven waarop het cluster is gemaakt.
    Cluster wachtwoord|$clusterPassword||Cluster gebruikerswachtwoord.

    >[AZURE.NOTE] Fill-in de waarden in de tabel. Dit is nuttig zijn voor deze zelfstudie doorlopen.

## <a name="jump-start"></a>Start gaan

Er zijn verschillende manieren Ambari gebruiken om te controleren HDInsight clusters.

**Azure PowerShell gebruiken**

Hieronder ziet u een Azure PowerShell-script om de gegevens van MapReduce taak tracker *in een cluster HDInsight 3.1.*  Het belangrijkste verschil is dat we deze gegevens uit de garens service (plaats MapReduce) halen.

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/yarn/components/resourcemanager"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

Hier volgt een Azure PowerShell-script om de MapReduce taak tracker informatie *in een cluster HDInsight 2.1*voor:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

De uitvoer is:

![Jobtracker uitvoer][img-jobtracker-output]

**Krul gebruiken**

Hier volgt een voorbeeld van cluster informatie ophalen met behulp van krul:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

De uitvoer is:

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**Voor de 10-8-2014 los**:

Bij gebruik van het eindpunt Ambari, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", het veld *hostnaam* geeft als resultaat de FQDN-naam (Fully Qualified Domain Name) van het knooppunt in plaats van de hostnaam. Vóór 8-10-2014 wordt uitgebracht, wordt in dit voorbeeld gewoon "**headnode0**" geretourneerd. Na de release 10-8-2014, krijgt u de FQDN-naam '**headnode0. { ClusterDNS} .azurehdinsight .net**", zoals wordt weergegeven in het vorige voorbeeld. Deze wijziging is om te vergemakkelijken scenario's waarin meerdere clustertypen (zoals HBase en Hadoop) kunnen worden geïmplementeerd in één virtueel netwerk (VNET). Dit gebeurt, bijvoorbeeld wanneer u HBase als een back-enddatabase platform voor Hadoop.

## <a name="ambari-monitoring-apis"></a>Ambari API's controleren

De volgende tabel worden enkele van de meest voorkomende Ambari API oproepen cmdlets voor controle. Zie voor meer informatie over de API, [Ambari API Reference][ambari-api-reference].

Monitor met een API-oproep|URI|Beschrijving
---|---|---
Clusters ophalen|`/api/v1/clusters`|
Toon cluster info.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net`|kolomgroepen services, hosts
Onlineservices verkrijgen|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services`|Services opnemen: hdfs, mapreduce
Services-gegevens opvragen.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>`|
De onderdelen van service ophalen|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components`|HDFS: namenode, datanode<br/>MapReduce: jobtracker; tasktracker
Toon onderdeel info.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>`|ServiceComponentInfo, host-onderdelen, aan de doelstellingen
Hosts ophalen|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts`|headnode0, workernode0
Toon host info.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>`|
Onderdelen van de host ophalen|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components`|namenode, resourcemanager
Toon host onderdeel info.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>`|HostRoles, component, host, aan de doelstellingen
Configuraties ophalen|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations`|Config typen: core-site, hdfs-site, mapred-site, component-site
Informatie over de configuratie krijgen.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>`|Config typen: core-site, hdfs-site, mapred-site, component-site


##<a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd hoe Ambari API oproepen cmdlets voor controle gebruiken. Meer informatie raadpleegt u:

- [HDInsight clusters met behulp van de Azure-Portal beheren][hdinsight-admin-portal]
- [Via PowerShell Azure HDInsight-clusters beheren][hdinsight-admin-powershell]
- [HDInsight clusters opdrachtregel beheren][hdinsight-admin-cli]
- [HDInsight documentatie][hdinsight-documentation]
- [Aan de slag met HDInsight][hdinsight-get-started]



[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: /documentation/services/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
