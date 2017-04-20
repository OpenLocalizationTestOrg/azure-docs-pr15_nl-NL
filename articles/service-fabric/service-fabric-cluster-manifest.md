<properties
   pageTitle="Uw cluster zelfstandige configureren | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe een zelfstandig product of een privé-Service stof cluster configureren."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="dkshir"/>


# <a name="configuration-settings-for-standalone-windows-cluster"></a>Configuratie-instellingen voor de zelfstandige versie van Windows cluster

In dit artikel wordt beschreven hoe u een zelfstandige Service stof cluster met behulp van het bestand _**ClusterConfig.JSON**_ configureert. U kunt dit bestand gebruiken om op te geven van gegevens, zoals de Service stof knooppunten en hun IP-adressen, verschillende typen knooppunten op het cluster, de beveiligingsconfiguraties, evenals de netwerktopologie tussen foutenstructuuranalyse/upgrade domeinen voor uw cluster zelfstandige.

Wanneer u [de zelfstandige Service stof downloaden](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), een paar voorbeelden van het bestand ClusterConfig.JSON worden gedownload naar uw computer werk. In de voorbeelden met *DevCluster* in hun namen kunt u een cluster met alle drie knooppunten op dezelfde computer, zoals logische knooppunten te maken. Afmelden bij deze, moet ten minste één knooppunt zijn gemarkeerd als een primaire knooppunt. Dit cluster is handig voor een ontwikkel- of -omgeving en wordt niet ondersteund als een cluster productie. In de voorbeelden met *MultiMachine* in hun namen, kunt u een cluster van de kwaliteit productie maken met elk knooppunt in een afzonderlijk machine. Het aantal primaire knooppunten voor deze cluster zal worden gebaseerd op het [niveau van de betrouwbaarheid](#reliability).

1. *ClusterConfig.Unsecure.DevCluster.JSON* en *ClusterConfig.Unsecure.MultiMachine.JSON* leert hoe een onbeveiligde test- of cluster respectievelijk maakt. 
    
2. *ClusterConfig.Windows.DevCluster.JSON* en *ClusterConfig.Windows.MultiMachine.JSON* leert hoe test- of cluster, beveiligd met [Windows-beveiliging](service-fabric-windows-cluster-windows-security.md)maakt.

3. *ClusterConfig.X509.DevCluster.JSON* en *ClusterConfig.X509.MultiMachine.JSON* laten zien hoe test- of cluster, beveiligd met maken [X509 certificaten gebaseerde beveiliging](service-fabric-windows-cluster-x509-security.md). 


Nu bespreken we de verschillende secties van een bestand _**ClusterConfig.JSON**_ als hieronder.

## <a name="general-cluster-configurations"></a>Algemene clusterconfiguraties
Dit behandelt de globaal cluster specifieke configuraties, zoals wordt weergegeven in het onderstaande JSON-fragment.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2015-01-01-alpha",

U kunt een beschrijvende naam geven tot uw Service stof cluster toewijzen aan de variabele **naam** . De **clusterConfigurationVersion** is het versienummer van uw cluster. Telkens wanneer u een upgrade uitvoert van uw Service stof cluster, moet u deze verhogen. U moet de **apiVersion** echter laat op de standaardwaarde.


<a id="clusternodes"></a>
## <a name="nodes-on-the-cluster"></a>Knooppunten op het cluster
U kunt de knooppunten configureren op uw cluster Service stof met behulp van de sectie **knooppunten** , zoals in het volgende fragment.

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Een Service stof cluster moet ten minste 3 knooppunten bevatten. U kunt meer knooppunten toevoegen aan deze sectie aan de hand van de configuratie. De volgende tabel wordt uitgelegd dat de configuratie-instellingen voor elk knooppunt.

|**Configuratie van knooppunten**|**Beschrijving**|
|-----------------------|--------------------------|
|Knooppuntnaam|U kunt een beschrijvende naam geven naar het knooppunt.|
|IP-adres|Het IP-adres van uw knooppunt achterhalen door een opdrachtvenster openen en te typen `ipconfig`. Houd rekening met het IPv4-adres en toewijzen aan de variabele **IP-adres** .|
|nodeTypeRef|Elk knooppunt kan een ander knooppunttype worden toegewezen. De [knooppunttypen](#nodetypes) zijn gedefinieerd in het gedeelte hierna.|
|faultDomain|Fout met domeinen kunt toe dat clusterbeheerders definiëren de fysieke knooppunten die mogelijk niet op hetzelfde moment vanwege afhankelijkheden met gedeelde fysiek.|
|upgradeDomain|De upgrade domeinen beschrijven sets van knooppunten voor Service stof upgrades bij ongeveer tegelijk worden afgesloten. U kunt welke knooppunten toewijzen aan welke Upgrade domeinen, zoals ze niet door een fysieke vereisten beperkt worden.| 


## <a name="cluster-properties"></a>Eigenschappen van cluster

De sectie **Eigenschappen** in het ClusterConfig.JSON wordt gebruikt voor het cluster als volgt configureren.

<a id="reliability"></a>
### <a name="reliability"></a>Betrouwbaarheid 
De sectie **reliabilityLevel** bepaalt het aantal exemplaren van de systeemservices die kunnen worden uitgevoerd op de primaire knooppunten van het cluster. Dit verhoogt de betrouwbaarheid van de volgende services en dus het cluster. U kunt instellen deze variabele op *Bronze*, *Silver*, *gouden* of *Platinum* voor 3, 5, 7 of 9 kopieën van de volgende services respectievelijk. Bekijk het onderstaande voorbeeld.

    "reliabilityLevel": "Bronze",
    
Houd er rekening mee dat aangezien een primaire knooppunt uitgevoerd een enkel exemplaar van de systeemservices, u ten minste 3 primaire knooppunten voor *Bronze*, 5 voor *Silver*, 7 voor *gouden* en 9 voor *Platinum* betrouwbaarheid niveaus moet.


### <a name="diagnostics"></a>Diagnostische hulpprogramma 's
De sectie **diagnosticsStore** kunt u voor het configureren van parameters om in te schakelen hulpprogramma's voor diagnose en probleemoplossing van knooppunt of cluster fouten, zoals wordt weergegeven in het volgende fragment. 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

De **metagegevens** volgt een beschrijving van uw cluster diagnostische hulpprogramma's en aan de hand van de configuratie kan worden ingesteld. Deze variabelen bij het oplossen van ETW logboeken voor het traceren, crashdumps, evenals prestatiemeteritems verzamelen. Lees [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) en [ETW-tracering](https://msdn.microsoft.com/library/ms751538.aspx) voor meer informatie over Logboeken voor het traceren van ETW. Alle logboeken inclusief [dumps vastlopen](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) en [prestatiemeteritems](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) kunnen doorgestuurd naar de map **connectionString** op uw computer. U kunt ook *AzureStorage* gebruiken voor het opslaan van diagnostische gegevens. Zie hieronder voor het codefragment van een steekproef.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Beveiliging 
De sectie **beveiliging** is vereist voor een beveiligde zelfstandige Service stof cluster. Het codefragment van de volgende ziet u een deel van deze sectie.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

De **metagegevens** volgt een beschrijving van uw secure cluster en aan de hand van de configuratie kan worden ingesteld. De **ClusterCredentialType** en **ServerCredentialType** bepalen welk type waardepapier waarvan de cluster en de knooppunten implementeert. Ze kunnen worden ingesteld op een van beide *X509* voor een waardepapier, op basis van certificaten of *Windows* voor een waardepapier op basis van Azure Active Directory. De rest van de sectie **beveiliging** zal worden gebaseerd op het type van de beveiliging. [Certificaten gebaseerde beveiliging in een cluster zelfstandig product](service-fabric-windows-cluster-x509-security.md) of [Windows-beveiliging in een cluster zelfstandige](service-fabric-windows-cluster-windows-security.md) lezen voor meer informatie over hoe u Vul de rest van de sectie **beveiliging** .


<a id="nodetypes"></a>
### <a name="node-types"></a>Knooppunttypen
Het type van de knooppunten waarop uw cluster wordt beschreven in de sectie **nodeTypes** . Type ten minste één knooppunt moet worden opgegeven voor een cluster, zoals wordt weergegeven in het onderstaande fragment. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

De **naam** is de beschrijvende naam voor dit type bepaald knooppunt. Als u wilt maken van een knooppunt van dit knooppunttype, de beschrijvende naam naar de variabele **nodeTypeRef** voor dat knooppunt, als toewijzen genoemde [hierboven](#clusternodes). Voor elk knooppunttype definiëren de eindpunten van de verbinding die wordt gebruikt. U kunt elk poortnummer voor de eindpunten van deze verbinding, zo lang maken als zij niet in strijd met een andere eindpunten in dit cluster. In een cluster met meerdere knooppunten, wordt er een of meer primaire knooppunten (dat wil zeggen **isPrimary** ingesteld op *waar*), afhankelijk van de [**reliabilityLevel**](#reliability). [Service stof cluster capaciteit overwegingen bij het plannen](service-fabric-cluster-capacity.md) voor informatie over **nodeTypes** en **reliabilityLevel** waarden, en om te weten wat zijn primaire en de typen niet-primaire knooppunten begrijpen. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Eindpunten gebruikt voor het configureren van de knooppunttypen

- *clientConnectionEndpointPort* is de poort die wordt gebruikt door de client verbinding maakt met het cluster, bij gebruik van de client-API. 
- *clusterConnectionEndpointPort* is de poort waarop de knooppunten met elkaar communiceren.
- *leaseDriverEndpointPort* is de poort die wordt gebruikt door het stuurprogramma van de lease cluster om vast te stellen als de knooppunten nog steeds actief zijn. 
- *serviceConnectionEndpointPort* is de poort die wordt gebruikt door de toepassingen en services die zijn geïmplementeerd op een knooppunt, om te communiceren met de Service stof-client op dat bepaalde knooppunt.
- *httpGatewayEndpointPort* is de poort die wordt gebruikt door de Service stof Explorer verbinding maken met de cluster.
- *ephemeralPorts* zijn de [dynamische poorten die door het besturingssysteem gebruikt](https://support.microsoft.com/kb/929851). Service stof wordt een deel van deze toepassing poorten gebruikt en wordt de resterende beschikbaar voor het besturingssysteem. Dit wordt ook in dit bereik toewijzen aan het bestaande bereik aanwezig zijn in het besturingssysteem, zodat u de bereiken gegeven in de JSON-voorbeeldbestanden voor alle doeleinden gebruiken kunt. U nodig hebt om ervoor te zorgen dat het verschil tussen de begin- en de poorten einde ten minste 255 is. 
- *applicationPorts* zijn de poorten die worden gebruikt door de Service stof-toepassingen. Deze moet een subset van de *ephemeralPorts*, genoeg is voor het eindpunt behoefte van uw toepassingen. Service stof Gebruik deze wanneer nieuwe poorten zijn vereist, evenals van het openen van de firewall voor deze poorten kunt verrichten. 
- *reverseProxyEndpointPort* is een optionele reverse-proxy-eindpunt. Zie [Service stof Reverse-proxyserver](service-fabric-reverseproxy.md) voor meer informatie. 


### <a name="other-settings"></a>Andere instellingen
De sectie **fabricSettings** kunt u voor het instellen van de hoofdsite mappen voor de Service stof gegevens en Logboeken. U kunt deze alleen tijdens het maken van het eerste cluster aanpassen. Zie hieronder voor het codefragment van een steekproef van deze sectie.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Het is raadzaam een niet-OS-station gebruiken als de FabricDataRoot en FabricLogRoot, aangezien deze biedt dat meer betrouwbaarheid ten opzichte van OS loopt vast. Houd er rekening mee dat als u alleen de hoofdsite van de gegevens wilt aanpassen, klikt u vervolgens naar de hoofdsite log wordt geplaatst één niveau onder de hoofdsite van de gegevens.


## <a name="next-steps"></a>Volgende stappen

Nadat u een volledige ClusterConfig.JSON-bestand dat is geconfigureerd aan de hand van de configuratie van de cluster zelfstandige hebt, kunt u uw cluster implementeren volgens het artikel [maken van een cluster Azure-Service stof on-premises implementatie of in de cloud](service-fabric-cluster-creation-for-windows-server.md) en gaat u verder met [uw cluster met Service stof Explorer visualiseren](service-fabric-visualizing-your-cluster.md).


