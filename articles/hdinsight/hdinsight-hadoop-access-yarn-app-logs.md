<properties
    pageTitle="Access Hadoop garens-toepassing via programmacode Logboeken | Microsoft Azure"
    description="Access-toepassing, aanmeldt via een programma een Hadoop-cluster in HDInsight."
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

# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Access-toepassing voor garens aanmeldt HDInsight op basis van Windows

In dit onderwerp wordt uitgelegd hoe u toegang tot de logboeken voor garens (nog een andere Resource onderhandelaar)-toepassingen die u op een Hadoop-cluster in Azure HDInsight hebt

> [AZURE.NOTE] De informatie in dit document geldt alleen voor Windows gebaseerde HDInsight clusters. Voor informatie over toegang tot garens HDInsight Linux gebaseerde clusters zich aanmeldt, raadpleegt u [garens van Access-toepassing aanmeldt Linux gebaseerde Hadoop op HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)

### <a name="prerequisites"></a>Vereisten voor

- Een cluster HDInsight op basis van Windows.  Zie [Windows maken gebaseerde Hadoop clusters in HDInsight](hdinsight-provision-clusters.md).


## <a name="yarn-timeline-server"></a>GARENS tijdlijn Server

De <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Garens tijdlijn Server</a> bevat algemene informatie over voltooide toepassingen, evenals framework / regiospecifieke toepassingsinformatie tot en met twee verschillende interfaces. Name:

* Opslag en voor het ophalen van algemene toepassingsinformatie op HDInsight clusters is ingeschakeld met versie 3.1.1.374 of hoger.
* Onderdeel van de toepassing framework / regiospecifieke gegevens van de tijdlijn-Server is momenteel niet beschikbaar op HDInsight clusters.


Algemene informatie over toepassingen bevat de volgende soorten gegevens:

* De toepassings-ID, een unieke id van een toepassing
* De gebruiker van wie de toepassing gestart
* Informatie over pogingen om te voltooien van de toepassing
* De containers die worden gebruikt door een bepaalde toepassing poging

Klik op uw kolomgroepen HDInsight wordt deze informatie opgeslagen door Azure Resource Manager naar een store geschiedenis in de standaardcontainer van uw standaardaccount voor de opslag van Azure. Deze algemene gegevens over voltooide toepassingen kan worden opgehaald met een REST API:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>Logboeken garens toepassingen en

GARENS ondersteunt meerdere programming modellen (MapReduce wordt een van deze) door het resourcebeheer van de van toepassing plannen/monitoring ontkoppeling. Dit gebeurt via een globale *ResourceManager* (RM), per werknemer-knooppunten *NodeManagers* (NMs) en per toepassing *ApplicationMasters* (AMs). De AM per toepassing onderhandelingen informatiebronnen (CPU, geheugen, schijf, netwerk) voor het uitvoeren van uw toepassing met de RM. De RM werkt met NMs om te verlenen deze resources, die als *containers*zijn verleend. De AM is verantwoordelijk voor het volgen van de voortgang van de containers toe door de RM. toegewezen Een toepassing kan veel containers afhankelijk van de aard van de toepassing vragen.

Bovendien elke toepassing kan bestaan uit meerdere *toepassing probeert* om te voltooien in de aanwezigheid van loopt of vanwege de verlies van de communicatie tussen een AM en een RM. Daarom containers toegekend aan een specifieke poging van een toepassing. Een container biedt de context voor de basiseenheid voor werk dat door een toepassing garens uitgevoerd in een zin, en alle werk dat is uitgevoerd binnen de context van een container wordt uitgevoerd op het één werknemer knooppunt waarop de container is toegewezen. Zie [Garens begrippen] [ YARN-concepts] voor verdere verwijzing.

Toepassingslogboeken aan de (en de bijbehorende container Logboeken) hebben ernstige foutopsporing problematisch Hadoop-toepassingen. GARENS biedt een nette kader voor het verzamelen en opslaan van de toepassingslogboeken met de [Log aggregatie] aggregeren[ log-aggregation] functie. De functie Log aggregatie kunt toepassingslogboeken aan de toegang tot meer deterministisch, omdat deze logboeken over alle containers op een knooppunt werknemer is samengevoegd en worden opgeslagen als één geaggregeerde logboekbestand per medewerker knooppunt in het standaardbestandssysteem nadat een toepassing is voltooid. Uw toepassing honderden of duizenden containers mogelijk gebruikt, maar logboeken voor alle containers uitgevoerd op een knooppunt één werknemer altijd worden samengevoegd in één bestand, resulteert in één logboekbestand per medewerker knooppunt gebruikt door de toepassing. Log aggregatie is standaard ingeschakeld op HDInsight clusters (versie 3.0 en hoger), en geaggregeerde logboeken kunnen worden gevonden in de standaardcontainer van uw cluster op de volgende locatie:

    wasbs:///app-logs/<user>/logs/<applicationId>

In dat locatie, de *gebruiker* is de naam van de gebruiker van wie de toepassing gestart en *applicationId* de unieke id van een toepassing is zoals die door de RM. garens toegewezen

De samengevoegde logboeken kunnen worden niet rechtstreeks gelezen, zoals ze zijn geschreven in een [TFile][T-file], [binaire indeling] [ binary-format] geïndexeerd door de container. GARENS bevat CLI functies om deze logboeken dump als tekst zonder opmaak voor toepassingen of containers belangrijke. U kunt deze logboeken weergeven als tekst zonder opmaak door een van de volgende garens uit te voeren de opdrachten rechtstreeks op knooppunten (nadat u hebt verbinding maken met deze via RDP):

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>GARENS ResourceManager UI

De gebruikersinterface van de ResourceManager garens wordt uitgevoerd op de headnode cluster en zijn toegankelijk via het dashboard van Azure portal: 

1. Meld u aan bij [Azure-portal](https://portal.azure.com/). 
2. In het linkermenu, klikt u op **Bladeren**, **HDInsight Clusters**en klik op een Windows gebaseerde cluster die u wilt toegang tot de logboeken aan de toepassing garens.
3. Klik op het bovenste menu op **Dashboard**. Hier ziet u een pagina geopend op een nieuwe browser tab-toets genoemd **HDInsight Query Console**.
4. Klik op **Garens UI**- **HDInsight Query Console**.




[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
