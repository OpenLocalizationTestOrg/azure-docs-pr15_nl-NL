<properties
    pageTitle="Gebruik van lege rand knooppunten in HDInsight | Microsoft Azure"
    description="Het toevoegen van een ampty randknooppunt aan HDInsight cluster die kan worden gebruikt als een client en te testen/host uw HDInsight-toepassingen."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="use-empty-edge-nodes-in-hdinsight"></a>Lege rand knooppunten in HDInsight gebruiken

Leer hoe u een lege edge-knooppunt toevoegen aan een cluster Linux gebaseerde HDInsight. Een lege randknooppunt is een Linux virtuele machine met de dezelfde clienthulpprogramma's geïnstalleerd en geconfigureerd zoals in de headnodes, maar met geen hadoop services worden uitgevoerd. U kunt het randknooppunt gebruiken voor toegang tot het cluster, uw clienttoepassingen testen en uw clienttoepassingen hostingprovider. 

U kunt een randknooppunt lege toevoegen aan een bestaand HDInsight cluster, naar een nieuw cluster wanneer u de cluster maakt. Toevoegen van een lege randknooppunt is klaar met Azure resourcemanager sjabloon.  Het volgende voorbeeld wordt gedemonstreerd hoe het werkt met een sjabloon:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "[variables('clusterApiVersion')]",
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "[parameters('edgeNodeSize')]"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Zoals u ziet in de steekproef, kunt u desgewenst een [scriptactie](hdinsight-hadoop-customize-cluster-linux.md) om uit te voeren extra configuratie, zoals het installeren van [Apache kleurtoon](hdinsight-hadoop-hue-linux.md) in het randknooppunt bellen.

Nadat u een randknooppunt hebt gemaakt, kunt u verbinding maken met het knooppunt van de rand is via SSH en uitvoeren van clienthulpprogramma's voor toegang tot het cluster Hadoop in HDInsight.

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Het randknooppunt van een toevoegen aan een bestaand cluster

In deze sectie kunt u een sjabloon resourcemanager het randknooppunt van een toevoegen aan een bestaand HDInsight cluster.  De sjabloon resourcemanager vindt u in [GitHub](https://github.com/hdinsight/Iaas-Applications/tree/master/EmptyNode). De sjabloon resourcemanager roept een script actie script https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh zich bevindt. Het script uitvoeren niet bewerkingen.  Dit is om te laten bellen scriptactie van een sjabloon resourcemanager zien.

**Een randknooppunt lege toevoegen aan een bestaand cluster**

1. Maak een cluster HDInsight als u nog niet hebt.  Zie [Hadoop zelfstudie: aan de slag met Linux gebaseerde Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klik op de volgende afbeelding om te melden bij Azure en open de sjabloon Azure resourcemanager in de portal van Azure. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FEmptyNode%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Configureer de volgende eigenschappen:

    - CLUSTERNAAM: Voer de naam van een bestaand HDInsight cluster.
    - EDGENODESIZE: Selecteer een van de grootte VM.
    - EDGENODEPREFIX: De standaardwaarde is **Nieuw**.  De standaardwaarde gebruikt, is de naam van de rand knooppunt **nieuwe edgenode**.  U kunt het voorvoegsel van de portal aanpassen. U kunt de volledige naam van de sjabloon ook aanpassen.


4. Klik op **OK** als de wijzigingen wilt opslaan.
5. Selecteer een resourcegroep in **resourcegroep**.
6. Klik op **de juridische voorwaarden controleren**en klik op **aanschaffen**.
7. Selecteer **vastmaken aan dashboard**en klik vervolgens op **maken**.

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Een randknooppunt toevoegen bij het maken van een cluster

In deze sectie kunt u een sjabloon resourcemanager HDInsight cluster met een randknooppunt maken.  De sjabloon resourcemanager vindt u in een openbare [Azure Blob storage container](http://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json). De sjabloon resourcemanager roept een script actie script https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh zich bevindt. Het script uitvoeren niet bewerkingen.  Dit is om te laten bellen scriptactie van een sjabloon resourcemanager zien.

**Een randknooppunt lege toevoegen aan een bestaand cluster**

1. Maak een cluster HDInsight als u nog niet hebt.  Zie [Hadoop zelfstudie: aan de slag met Linux gebaseerde Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klik op de volgende afbeelding om te melden bij Azure en open de sjabloon Azure resourcemanager in de portal van Azure. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Configureer de volgende eigenschappen:
        
    - CLUSTERNAAM: Voer een naam voor het nieuwe cluster maken.
    - CLUSTERLOGINUSERNAME: Typ de naam van de Hadoop HTTP-gebruiker.  De standaardnaam is **beheerder**.
    - CLUSTERLOGINPASSWORD: Voer het wachtwoord van de gebruiker Hadoop HTTP.
    - SSHUSERNAME: Voer de naam van de gebruiker SSH. De standaardnaam is **sshuser**.
    - SSHPASSWORD: Voer het wachtwoord van de gebruiker SSH.
    - LOCATIE: Voer de locatie van de naam van de Resource, het cluster en het standaardaccount voor de opslag.
    - CLUSTERTYPE: De standaardwaarde is **hadoop**.
    - CLUSTERWORKERNODECOUNT: De standaardwaarde is 2.
    - EDGENODESIZE: Selecteer een van de grootte VM.
    - EDGENODEPREFIX: De standaardwaarde is **Nieuw**.  De standaardwaarde gebruikt, is de naam van de rand knooppunt **nieuwe edgenode**.  U kunt het voorvoegsel van de portal aanpassen. U kunt de volledige naam van de sjabloon ook aanpassen.

4. Klik op **OK** als de wijzigingen wilt opslaan.
5. Voer een nieuwe naam voor de resourcegroep in **resourcegroep**.
6. Klik op **de juridische voorwaarden controleren**en klik op **aanschaffen**.
7. Selecteer **vastmaken aan dashboard**en klik vervolgens op **maken**. 


## <a name="access-an-edge-node"></a>Toegang tot een randknooppunt

Het randknooppunt ssh eindpunt is <EdgeNodeName>. <ClusterName>-ssh.azurehdinsight.net:22.  Bijvoorbeeld nieuwe-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Het randknooppunt wordt weergegeven als een toepassing op de Azure-portal.  De portal vindt u de informatie voor toegang tot het knooppunt van de rand is via SSH.

**Om te controleren of de rand knooppunt SSH eindpunt**

1. Aanmelden bij de [portal van Azure](https://portal.azure.com).
2. Open het HDInsight cluster met een randknooppunt.
3. Klik op **toepassingen** van het blad cluster. Er wordt het randknooppunt.  De standaardnaam is **nieuwe edgenode**.
4. Klik op het randknooppunt. U kunt het eindpunt SSH moet zien.

**Gebruik van de component op het randknooppunt**

5. Met SSH verbinding maken met het randknooppunt.  Zie [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight uit Linux, Unix, of OS X](hdinsight-hadoop-linux-use-ssh-unix.md) of [SSH met Linux gebaseerde Hadoop op HDInsight vanuit Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md).
6. Nadat u hebt verbonden met het knooppunt van de rand is via SSH, gebruikt u de volgende opdracht uit de component-console openen:

        hive
7. Voer de volgende opdracht om weer te geven component tabellen in het cluster:

        show tables;

## <a name="delete-an-edge-node"></a>Een randknooppunt verwijderen

U kunt een randknooppunt verwijderen uit de Azure-portal.

**Toegang tot een randknooppunt**

1. Aanmelden bij de [portal van Azure](https://portal.azure.com).
2. Open het HDInsight cluster met een randknooppunt.
3. Klik op **toepassingen** van het blad cluster. Er wordt een lijst van knooppunten van de rand.  
4. Met de rechtermuisknop op de randknooppunt dat u wilt verwijderen en klik vervolgens op **verwijderen**.
5. Klik op **Ja** om te bevestigen.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een randknooppunt toevoegt en hoe u toegang tot het randknooppunt in dit artikel. Meer informatie raadpleegt u de volgende artikelen:

- [Installeer HDInsight-toepassingen](hdinsight-apps-install-applications.md): informatie over het installeren van een toepassing HDInsight uw clusters.
- [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing met HDInsight.
- [Publiceren HDInsight-toepassingen](hdinsight-apps-publish-applications.md): informatie over het publiceren van uw aangepaste HDInsight-toepassingen met Azure Marketplace.
- [MSDN: installatie van een toepassing HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): meer informatie over het definiëren van HDInsight-toepassingen.
- [HDInsight aanpassen Linux gebaseerde clusters met de actie Script](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van scriptactie voor het installeren van extra toepassingen.
- [Hadoop maken Linux gebaseerde clusters in HDInsight resourcemanager sjablonen gebruiken](hdinsight-hadoop-create-linux-clusters-arm-templates.md): meer informatie over het bellen van resourcemanager sjablonen HDInsight clusters maken.

