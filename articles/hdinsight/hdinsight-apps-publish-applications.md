<properties
    pageTitle="Publiceren HDInsight toepassingen | Microsoft Azure"
    description="Informatie over het maken en publiceren van HDInsight-toepassingen."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/18/2016"
    ms.author="jgao"/>

# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>Publiceren HDInsight toepassingen in de Azure Marketplace

Een toepassing HDInsight is een toepassing die gebruikers op een cluster Linux gebaseerde HDInsight installeren kunnen. Deze toepassingen kunnen worden ontwikkeld door Microsoft, onafhankelijke leveranciers (ISV) of door zelf. In dit artikel leert u hoe u een HDInsight-toepassing in het Azure Marketplace publiceert.  Zie [publiceren van een aanbod met de Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md)voor algemene informatie over publiceren naar de Azure Marketplace.

HDInsight-toepassingen gebruiken het model *Doen om uw eigen licentie (BYOL)* , waar de toepassingsprovider van is verantwoordelijk voor licenties voor de toepassing voor eindgebruikers en eindgebruikers zijn alleen btw geheven door Azure voor de resources die ze, zoals het cluster HDInsight en de VMs/knooppunten maken. Op dit moment kan worden facturering voor de toepassing zelf wordt niet uitgevoerd via Azure.

Andere toepassing HDInsight gerelateerd artikel:

- [Installeer HDInsight-toepassingen](hdinsight-apps-install-applications.md): informatie over het installeren van een toepassing HDInsight uw clusters.
- [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): meer informatie over het installeren en testen van aangepaste HDInsight-toepassingen.

 
## <a name="prerequisites"></a>Vereisten voor

Als u wilt uw aangepaste aanvraag indienen bij de marketplace, moet u hebt gemaakt en getest van uw aangepaste toepassing. Zie de volgende artikelen:

- [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): meer informatie over het installeren en testen van aangepaste HDInsight-toepassingen.

U moet ook hebben uw ontwikkelaarsaccount registreren. Zie [publiceren van een aanbod met de Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md) en [een ontwikkelaar van de Microsoft-account maken](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Toepassing definiëren

Er zijn twee stappen nodig zijn voor het publiceren van de Azure Marketplace-toepassingen.  Eerste definieert u een bestand **createUiDef.json** om aan te geven welke clusters uw toepassing is compatibel met; en vervolgens het publiceren van de sjabloon van de Azure-portal. Hierna volgt een voorbeeld van een createUiDef.json-bestand.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


|Veld  | Beschrijving   | Mogelijke waarden|
|-------|---------------|----------------|
|typen  | De clustertypen die de toepassing die compatibel met is.    |Hadoop, HBase, Storm, een (of een combinatie van deze)|
|lagen  | De cluster-lagen die de toepassing die compatibel met is.    |Standaard, Premium (of beide)|
|versies|  De HDInsight clustertypen die de toepassing die compatibel met is.    |3.4|

## <a name="package-application"></a>Pakket toepassen

Maak een zipbestand dat alle benodigde bestanden voor de installatie van uw toepassingen HDInsight bevat. Moet u het zip-bestand in [de toepassing publiceren](#publish-application).

- [createUiDefinition.json](#define-application).
- mainTemplate.json. Zie een steekproef bij [aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).

    >[AZURE.IMPORTANT] De naam van de namen van de toepassingen installeren script moet uniek zijn voor een bepaald cluster met de onderstaande notatie. Daarnaast een installeren en verwijderen van scriptacties moet idempotency is ingeschakeld, wat betekent dat de scripts kan worden aangeroepen repeatly tijdens het maken van hetzelfde resultaat.
    
    >   naam":" [samenvoegen (' kleurtoon-installatie-v0 ','-', uniquestring('applicationName')] "
        
    >Houd er rekening mee er zijn drie onderdelen op de scriptnaam:
        
    >   1. Een script naam voorvoegsel, waaronder de naam van de toepassing of een naam die relevant zijn voor de toepassing.
    >   2. A "-" voor leesbaarheid.
    >   3. Een unieke tekenreeks-functie met de naam van de toepassing als de parameter.

    >   Een voorbeeld is de bovenstaande uiteinden van steeds: Kleurtoon-installatie-v0-4wkahss55hlas in de lijst met permanente script actie. Zie voor een steekproef JSON nettolading, [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).

- Alle vereiste scripts.

> [AZURE.NOTE] De toepassingsbestanden (inclusief webtoepassingsbestanden als er een) kan zich bevinden op een openbaar eindpunt.

## <a name="publish-application"></a>Toepassing publiceren

Volg de volgende stappen voor het publiceren van een HDInsight-toepassing:

1. Aanmelden bij de [portal voor publiceren Azure](https://publish.windowsazure.com/).
2. Klik op **oplossing sjablonen** van links naar een nieuwe oplossing-sjabloon maken.
3. Voer een titel en klik vervolgens op **een nieuwe oplossing-sjabloon maken**.
3. Klik op **Maken ontwikkelaar Center account en deelnemen aan het programma Azure** om te registreren van uw bedrijf, als u dit nog niet hebt gedaan.  Zie [een ontwikkelaar van de Microsoft-account maken](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
4. Klik op **definiëren sommige topologieën aan de slag**. Een sjabloon oplossing is een 'parent' op alle bijbehorende topologieën. U kunt meerdere topologieën definiëren in een aanbieding/oplossing sjabloon. Wanneer een aanbod naar tijdelijke is verplaatst, wordt deze verplaatst met alle bijbehorende topologieën. 
4. Voer een naam voor de topologie en klik vervolgens op het plusteken (+).
5. Voer een nieuwe versie en klik vervolgens op het plusteken (+).
6. Upload het zip-bestand voorbereid in [pakket-toepassing](#package-application).  
7. Klik op **certificering aanvragen**. Het team van Microsoft-certificering beoordelen van de bestanden en certificeren van de topologie.

## <a name="next-steps"></a>Volgende stappen

- [Installeer HDInsight-toepassingen](hdinsight-apps-install-applications.md): informatie over het installeren van een toepassing HDInsight uw clusters.
- [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing met HDInsight.
- [HDInsight aanpassen Linux gebaseerde clusters met de actie Script](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van scriptactie voor het installeren van extra toepassingen.
- [Hadoop maken Linux gebaseerde clusters in HDInsight met Azure resourcemanager sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md): meer informatie over het bellen van resourcemanager sjablonen HDInsight clusters maken.
- [Lege rand knooppunten in HDInsight gebruiken](hdinsight-apps-use-edge-node.md): informatie over het gebruik van een lege randknooppunt voor toegang tot HDInsight cluster, testen van HDInsight-toepassingen en hostingprovider HDInsight-toepassingen.

