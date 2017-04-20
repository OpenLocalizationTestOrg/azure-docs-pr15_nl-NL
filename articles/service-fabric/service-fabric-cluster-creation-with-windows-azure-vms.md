<properties
   pageTitle="Maken van een zelfstandige cluster met met Windows Azure-VMs | Microsoft Azure"
   description="Informatie over het maken en beheren van een cluster Azure-Service stof op Azure virtuele machines met Windows Server."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2016"
   ms.author="dkshir;chackdan"/>



# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Maak een cluster met drie knooppunten zelfstandige Service stof met Azure virtuele machines met Windows Server

In dit artikel wordt beschreven hoe een cluster op op basis van Windows Azure virtuele machines (VMs), met behulp van het installatieprogramma van de Service stof zelfstandige voor Windows Server maken. Dit is een speciale geval van [maken en beheren van een cluster wordt uitgevoerd op Windows Server](service-fabric-cluster-creation-for-windows-server.md) waar de VMs zijn [Azure VMs met Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md), maar u niet [een Azure cloudgebaseerde Service stof cluster](service-fabric-cluster-creation-via-portal.md)maakt. Het verschil is dat het zelfstandige Service stof cluster gemaakt door de volgende stappen geheel wordt beheerd door u, terwijl de Azure-Service stof clusters cloudgebaseerde worden beheerd en bijgewerkt door de provider van de resource Service stof.


## <a name="steps-to-create-the-standalone-cluster"></a>Stappen voor het maken van het cluster zelfstandig

1. Meld u aan bij de portal van Azure en maak een nieuwe Windows Server 2012 R2 Datacenter VM in een resourcegroep. Lees het artikel [een Windows-VM in de portal van Azure maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor meer informatie.
2. Enkele meer Windows Server 2012 R2 Datacenter VMs toevoegen aan dezelfde resourcegroep. Zorg ervoor dat elk van de VMs heeft de beheerder-gebruikersnaam en het wachtwoord als gemaakt. Eenmaal gemaakte ziet u alle drie VMs in hetzelfde virtuele netwerk.
3. Verbinding maken met elk van de VMs en het Windows Firewall met de [Server Manager, lokale Server dashboard](https://technet.microsoft.com/library/jj134147.aspx)uitschakelen. Dit zorgt ervoor dat het netwerkverkeer tussen de computers kan communiceren. Verbinding gemaakt met elke computer en u u het IP-adres op een opdrachtprompt openen en te typen `ipconfig`. U kunt ook het IP-adres van elke computer weergeven door het selecteren van de resource virtueel netwerk voor de resourcegroep in de portal van Azure.
4. Verbinding maken met een van de VMs en test dat u de andere twee VMs kunt pingen.
5. Verbinding maken met een van de VMs en [de zelfstandige Service stof downloaden voor Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) naar een nieuwe map op de computer en het pakket uitpakken.
6. Open het bestand *ClusterConfig.Unsecure.MultiMachine.json* in Kladblok en bewerken van elk knooppunt met de drie IP-adressen van de computers. Wijzig de naam van het cluster aan het begin en sla het bestand.  Een gedeeltelijke voorbeeld van het cluster manifest is hieronder wordt weergegeven.

    ```
    {
        "name": "TestCluster",
        "clusterManifestVersion": "1.0.0",
        "apiVersion": "2015-01-01-alpha",
        "nodes": [
        {
            "nodeName": "vm0",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "vm2",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
    ],
    ```

7. Een [Filter wissen-venster](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise)openen. Navigeer naar de map waar u het gedownloade zelfstandige-installatiepakket uitgepakt en het cluster manifest-bestand hebt opgeslagen. Voer de volgende PowerShell-opdracht.

    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
    ```

8. Hier ziet u de PowerShell uitvoeren, sluit aan op elke computer en een cluster maken. Nadat het ongeveer een minuut, kunt u controleren of het cluster operationele is door te verbinden met de Service stof Verkenner op een van de IP-adres van de computer bijvoorbeeld met behulp van `http://10.7.0.5:19080/Explorer/index.html`. Aangezien dit een zelfstandige cluster met Azure VMs is, zodat u deze secure u moet [de VMs Azure certificaten implementeren](service-fabric-windows-cluster-x509-security.md) of een van de machines als [Windows Server AD (Active Directory) controller voor Windows-verificatie](service-fabric-windows-cluster-windows-security.md), net zoals u on-premises doen zou instellen.


## <a name="next-steps"></a>Volgende stappen
- [Zelfstandige Service stof clusters op Windows Server- of Linux maken](service-fabric-deploy-anywhere.md)
- [Toevoegen of verwijderen van knooppunten aan een zelfstandige Service stof cluster](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Configuratie-instellingen voor de zelfstandige versie van Windows cluster](service-fabric-cluster-manifest.md)
- [Secure een zelfstandige cluster op Windows met behulp van Windows-beveiliging](service-fabric-windows-cluster-windows-security.md)
- [Een zelfstandige cluster op Windows met behulp van X509 Secure certificaten](service-fabric-windows-cluster-x509-security.md)
