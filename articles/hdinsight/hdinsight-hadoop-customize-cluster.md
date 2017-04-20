<properties
    pageTitle="HDInsight Clusters scriptacties gebruiken aanpassen | Microsoft Azure"
    description="Informatie over het aanpassen van HDInsight clusters met de Script-actie."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Aanpassen op basis van Windows HDInsight clusters met de Script-actie

**Scriptactie** kan worden gebruikt om aan te roepen [aangepaste scripts](hdinsight-hadoop-script-actions.md) tijdens het maken cluster voor de installatie van extra software op een cluster.

De informatie in dit artikel is specifiek voor Windows gebaseerde HDInsight clusters. Zie voor Linux gebaseerde kolomgroepen [HDInsight aanpassen Linux gebaseerde clusters met de Script-actie](hdinsight-hadoop-customize-cluster-linux.md). 

HDInsight clusters kunnen worden aangepast op allerlei andere manieren, zoals inclusief extra opslagruimte van Azure-accounts, de Hadoop wijzigen configuratie bestanden (core-site.xml, component-site.xml, enzovoort) of bibliotheken (bijvoorbeeld component, Oozie) naar algemene locaties in het cluster toe te voegen worden gedeeld. Deze aanpassingen kunnen worden uitgevoerd via Azure PowerShell, de SDK van Azure HDInsight .NET of de Azure-Portal. Zie voor meer informatie, [Hadoop maken clusters in HDInsight][hdinsight-provision-cluster].

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>De scriptactie in het proces voor het maken van cluster

Scriptactie wordt alleen gebruikt terwijl een clusters is nog moet worden gemaakt. In het volgende diagram ziet u als scriptactie wordt uitgevoerd tijdens het maken:

![HDInsight cluster aanpassing en fasen tijdens het maken van cluster][img-hdi-cluster-states]

Wanneer het script wordt uitgevoerd, wordt in het cluster het podium **ClusterCustomization** ingevoerd. In dit stadium het script is uitgevoerd met de systeemaccount beheerder, op de opgegeven knooppunten in het cluster, parallel en biedt volledige beheerdersbevoegdheden op de knooppunten.

> [AZURE.NOTE] Omdat u beheerdersbevoegdheden knooppunten in de fase **ClusterCustomization** hebt, kunt u het script bewerkingen uitvoeren, bijvoorbeeld services, met inbegrip van Hadoop-gerelateerde services starten en stoppen. Ja, als onderdeel van het script, moet u ervoor zorgen dat Ambari en andere services Hadoop-gerelateerde goed werken voordat het script is voltooid. Deze services zijn vereist om na te gaan met succes de gezondheid en de status van het cluster terwijl deze wordt gemaakt. Als u een configuratie in het cluster dat van invloed is op deze services wijzigt, moet u het Help-functies die beschikbaar zijn. Zie voor meer informatie over de Help-functies, [ontwikkelen scriptactie-scripts voor HDInsight][hdinsight-write-script].

De uitvoer en de foutenlogboeken voor het script worden in het standaardaccount opslagruimte dat u hebt opgegeven voor het cluster opgeslagen. De logboeken zijn opgeslagen in een tabel met de naam **u < \cluster-name-fragment >< \time-stamp > bestand**. Hierna ziet u statistische logboeken van het script uitvoeren op alle knooppunten (hoofd knooppunt en werknemer knooppunten) in het cluster.

Elk cluster kunt akkoord gaan meerdere scriptacties die worden aangeroepen in de volgorde waarin ze zijn opgegeven. Een script kunt op het hoofd knooppunt, de werknemer knooppunten of beide worden uitgevoerd.

HDInsight bevat verschillende scripts voor het installeren van de volgende onderdelen op HDInsight clusters:

Naam | Script
----- | -----
**Elektrische installeren** | https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. Zie [installeren en gebruiken, dus op HDInsight clusters][hdinsight-install-spark].
**R installeren** | https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Zie [installeren en gebruiken R op HDInsight clusters][hdinsight-install-r].
**Solr installeren** | https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Zie [installeren en gebruiken Solr op HDInsight](hdinsight-hadoop-solr-install.md).
- **Giraph installeren** | https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Zie [installeren en gebruiken Giraph op HDInsight](hdinsight-hadoop-giraph-install.md).
| **Vooraf geladen component bibliotheken** | https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1. Zie [bibliotheken op HDInsight clusters component toevoegen](hdinsight-hadoop-add-hive-libraries.md) |


## <a name="call-scripts-using-the-azure-portal"></a>Gesprek scripts met behulp van de Azure-Portal

**Vanuit de Azure-Portal**

1. Beginnen met het maken van een cluster, zoals beschreven op [Hadoop maken clusters in HDInsight](hdinsight-provision-clusters.md#portal).
2. Klik onder optionele configuratie voor het blad **Scriptacties** op **scriptactie toevoegen** voor meer informatie over de scriptactie, zoals hieronder wordt weergegeven:

    ![Gebruik scriptactie om aan te passen een cluster] (./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Gebruik scriptactie om aan te passen een cluster")

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie.</td></tr>
        <tr><td>Script URI</td>
            <td>Geef de URI aan het script die als u wilt aanpassen van het cluster wordt geactiveerd. s</td></tr>
        <tr><td>Hoofd/werknemer</td>
            <td>Geef de knooppunten (**hoofd** of **werknemer**) op waarin het script aanpassing wordt uitgevoerd. </b>.
        <tr><td>Parameters</td>
            <td>Geef de parameters, indien nodig door het script.</td></tr>
    </table>

    Druk op ENTER om toe te voegen van meer dan één scriptactie om meerdere onderdelen op het cluster.

3. Klik op **selecteren** om de configuratie van de actie script opslaan en doorgaan met cluster maken.

## <a name="call-scripts-using-azure-powershell"></a>Gesprek scripts via Azure PowerShell

Deze volgende PowerShell-script laat zien hoe een installeren op Windows op basis HDInsight cluster.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    
    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.
    
    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"
    
    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    
    #############################################################
    # Connect to Azure
    #############################################################
    
    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID
    
    #############################################################
    # Prepare the dependent components
    #############################################################
    
    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext
    
    #############################################################
    # Create cluster with ApacheSpark
    #############################################################
    
    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey 
                
    
    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `
    
    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Andere als software wilt installeren, moet u het scriptbestand in het script vervangen:


Wanneer u wordt gevraagd, typt u de referenties voor het cluster. Het kan enkele minuten duren voordat het cluster is gemaakt.

## <a name="call-scripts-using-net-sdk"></a>Met .NET SDK scripts bellen 

In het onderstaande voorbeeld wordt gedemonstreerd hoe een installeren op Windows op basis HDInsight cluster. Andere als software wilt installeren, moet u het scriptbestand in de code wilt vervangen.

**Een cluster HDInsight maken met een** 

1. Maak een C#-console-toepassing in Visual Studio.
2. Voer de volgende opdracht uit vanaf de beheerconsole Nuget Package Manager.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight

2. Gebruik de volgende instructies gebruiken in het bestand Program.cs:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;

3. Plaats de code in de klas met de volgende handelingen uit:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,

                DefaultStorageAccountName = ExistingStorageName,
                DefaultStorageAccountKey = ExistingStorageKey,
                DefaultStorageContainer = ExistingContainer,

                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                ClientId, 
                new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                PromptBehavior.Always, 
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }


4. Druk op **F5** om de toepassing te starten.


## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Ondersteuning voor open source software die worden gebruikt op HDInsight clusters
De Microsoft Azure HDInsight-service is een flexibel platform waarmee u kunt grote gegevens toepassingen maken in de cloud met behulp van een selectie aan open source technologieën gevormd rond Hadoop. Microsoft Azure biedt een algemene niveau van ondersteuning voor open source technologieën, zoals is beschreven in de sectie **Bereik ondersteuning** van de <a href="http://azure.microsoft.com/support/faq/" target="_blank">website van Azure ondersteunen Veelgestelde vragen</a>. De HDInsight-service biedt een extra niveau van ondersteuning voor een deel van de onderdelen, zoals hieronder beschreven.

Er zijn twee soorten open source-onderdelen die beschikbaar in de service HDInsight zijn:

- **Ingebouwde onderdelen** - deze onderdelen zijn vooraf geïnstalleerd op HDInsight clusters en geef de belangrijkste functies van het cluster. Bijvoorbeeld garens ResourceManager, de querytaal component (HiveQL) en de bibliotheek Mahout deel uitmaakt van deze categorie. Een volledige lijst met clusteronderdelen is beschikbaar in [Wat is er nieuw in de Hadoop cluster versies geleverd door HDInsight?](hdinsight-component-versioning.md) </a>.
- **Aangepaste onderdelen** - u, als een gebruiker van het cluster, kunt installeren of gebruiken in uw werkzaamheden elk onderdeel beschikbaar in de community of door u gemaakte.

Ingebouwde onderdelen volledig worden ondersteund en Microsoft Support helpt isoleren en oplossen van problemen met deze onderdelen.

> [AZURE.WARNING] Onderdelen van het cluster HDInsight volledig worden ondersteund en Microsoft Support helpt isoleren en oplossen van problemen met deze onderdelen.
>
> Aangepaste onderdelen ontvangen commercieel redelijk ondersteuning waarmee u kunt het probleem verder kunt oplossen. Dit kan leiden tot het probleem oplossen of vraag of u kunt voeren beschikbare kanalen voor de bron openen technologieën waar uitgebreide expertise voor deze technologie is gevonden. Bijvoorbeeld, zijn er veel communitysites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache projecten tevens projectsites op [http://apache.org](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/), [een](http://spark.apache.org/).

De HDInsight-service biedt verschillende manieren voor het gebruik van aangepaste onderdelen. Ongeacht hoe een onderdeel gebruikt of is geïnstalleerd op het cluster, hetzelfde niveau van ondersteuning is van toepassing. Hierna volgt een lijst van de meest voorkomende manieren dat aangepaste onderdelen op HDInsight clusters kunnen worden gebruikt:

1. Taak indiening - Hadoop of andere soorten taken die uitvoeren of aangepaste onderdelen gebruiken kan bij het cluster worden ingediend.
2. Cluster aanpassing - gemaakt, kunt u aanvullende instellingen en aangepaste onderdelen die wordt geïnstalleerd knooppunten opgeven.
3. Voorbeelden - voor populaire aangepaste onderdelen, Microsoft en anderen kunnen voorbeelden van hoe deze onderdelen kunnen worden gebruikt op HDInsight clusters bepalen. Deze voorbeelden worden gegeven zonder ondersteuning.

## <a name="develop-script-action-scripts"></a>Scriptactie scripts ontwikkelen

Zie [ontwikkelen scriptactie-scripts voor HDInsight][hdinsight-write-script].


## <a name="see-also"></a>Zie ook

- [Hadoop clusters maken in HDInsight] [ hdinsight-provision-cluster] bevat instructies voor het maken van een HDInsight cluster met behulp van andere aangepaste opties.
- [Ontwikkel scriptactie scripts voor HDInsight][hdinsight-write-script]
- [Installeren en gebruiken van een op HDInsight clusters][hdinsight-install-spark]
- [Installeren en gebruiken van R op HDInsight clusters][hdinsight-install-r]
- [Installeren en gebruiken Solr op HDInsight clusters](hdinsight-hadoop-solr-install.md).
- [Installeren en gebruiken Giraph op HDInsight clusters](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-provision-clusters.md
[powershell-install-configure]: powershell-install-configure.md


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Fasen tijdens het maken van cluster"
