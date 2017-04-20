<properties
    pageTitle="Een externe luisteraar ervan af configureren voor altijd op beschikbaarheidsgroepen | Microsoft Azure"
    description="Deze zelfstudie leert u stappen voor het maken van een altijd op beschikbaarheid van de groep luisteraar ervan af in Azure wordt aangegeven die extern toegankelijk is met behulp van de openbare virtuele IP-adres van de bijbehorende cloudservice."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Een externe luisteraar ervan af voor altijd op beschikbaarheid van groepen in Azure configureren

> [AZURE.SELECTOR]
- [Interne luisteraar ervan af](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Externe luisteraar ervan af](virtual-machines-windows-classic-ps-sql-ext-listener.md)

In dit onderwerp ziet u hoe u een luisteraar ervan af voor een altijd op beschikbaarheid van de groep die extern toegankelijk is op internet is configureren. Dit is het mogelijk gemaakt door het adres van de cloudservice **Openbare virtuele IP-(VIP)** koppelen aan de luisteraar ervan af.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


De beschikbaarheid van de groep kunnen replica's on-premises implementatie alleen Azure alleen, bevatten of zowel on-premises en Azure bij hybride configuraties's beslaan. Azure replica's kunnen zich bevinden binnen dezelfde regio of tussen meerdere regio's met meerdere virtuele netwerken (VNets). De onderstaande stappen wordt ervan uitgegaan dat u hebt al [een beschikbaarheidsgroep geconfigureerd](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , maar niet een luisteraar ervan af hebt geconfigureerd.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Richtlijnen en beperkingen voor externe listeners

Houd rekening met de volgende richtlijnen over de luisteraar ervan af groep beschikbaarheid in Azure wordt aangegeven wanneer u met de cloud service afrukken VIP-adres implementeert:

- De beschikbaarheid van de groep luisteraar ervan af wordt op Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2 ondersteund.

- De clienttoepassing moet zich bevinden op een andere cloudservice dan de tabel die de van de beschikbaarheidsgroep VMs bevat. Azure biedt geen ondersteuning voor directe server return met clients en servers in de dezelfde cloudservice.

- Standaard is de stappen in dit artikel hoe voor het configureren van één luisteraar ervan af als u wilt gebruiken, het adres van de cloud virtuele IP-(VIP). Het is echter mogelijk om reserveren en meerdere VIP-adressen voor uw cloudservice te maken. Hiermee kunt u meerdere listeners die elk zijn gekoppeld aan een ander VIP maken via de stappen in dit artikel. Zie voor informatie over het maken van meerdere VIP-adressen, [Meerdere VIP's per cloudservice](../load-balancer/load-balancer-multivip.md).

- Als u een luisteraar ervan af voor een hybride omgeving maakt, moet het on-premises netwerk verbinding met de openbare Internet naast de site-naar-site VPN-verbinding met het Azure virtuele netwerk hebben. Wanneer u zich in het Azure subnet, is de beschikbaarheid van de groep luisteraar ervan af bereikbaar alleen voor het openbare IP-adres van de desbetreffende cloudservice.

- Deze wordt niet ondersteund als u wilt maken van een externe luisteraar ervan af in dezelfde cloudservice waar u tevens een interne luisteraar ervan af met de interne laden verdeling (ILB).

## <a name="determine-the-accessibility-of-the-listener"></a>De toegankelijkheid van de luisteraar ervan af bepalen

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

In dit artikel bevat informatie over het maken van een luisteraar ervan af met **externe taakverdeling**. Als u wilt dat een luisteraar ervan af die bij het netwerk van uw virtuele privégroep is, raadpleegt u de versie van dit artikel vindt u stappen voor het instellen van een [luisteraar ervan af met ILB](virtual-machines-windows-classic-ps-sql-int-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>VM eindpunten verdeeld met directe server afzender maken

Externe taakverdeling gebruikt de virtuele de openbare virtuele IP-adres van de cloudservice waarop uw VMs. U hoeft dus niet te maken of de taakverdeling in dit geval configureren.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Het volgende PowerShell-script kopiëren in een teksteditor en stel de waarden van variabelen aanpassen aan uw omgeving (standaardwaarden zijn opgegeven voor sommige parameters). Houd er rekening mee dat als uw beschikbaarheidsgroep 's Azure regio's beslaat, u moet het script eenmaal uitgevoerd in elke datacenter voor de cloudservice en knooppunten die zich in die datacenter bevinden.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas

        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

1. Als u de variabelen hebt ingesteld, kopieert u het script in de teksteditor in uw Azure PowerShell-sessie uit te voeren. Als de vraag nog steeds >>, typt u ENTER opnieuw om ervoor te zorgen dat het script wordt gestart.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Controleer of KB2854082 zo nodig is geïnstalleerd

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Open de firewallpoorten in de beschikbaarheid van de groepsknooppunten

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>De beschikbaarheid van de groep luisteraar ervan af maken

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Voor externe taakverdeling, moet u het openbare virtuele IP-adres van de cloudservice die uw replica's bevat. Meld u aan bij de portal van Azure klassieke. Navigeer naar de cloudservice die uw van de beschikbaarheidsgroep VM bevat. Open de weergave van het **Dashboard** .

3. Houd rekening met het adres dat wordt weergegeven onder **adres van openbare virtuele IP-(VIP)**. Als uw oplossing zich uitstrekt over VNets, herhaalt u deze stap voor elke cloudservice die een VM waarop een replica bevat.

4. Klik op een van de VMs, het volgende PowerShell-script kopiëren in een teksteditor en stel de variabelen op de waarden die u eerder hebt genoteerd.

        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service

        Import-Module FailoverClusters

        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.

        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255


1. Eenmaal u hebt de variabelen, een verhoogde Windows PowerShell-venster te openen en vervolgens het script in de teksteditor kopiëren en plakken in uw Azure PowerShell-sessie uit te voeren. Als de vraag nog steeds >>, typt u ENTER opnieuw om ervoor te zorgen dat het script wordt gestart.

1. Herhaal dit op elk VM. Dit script voor het configureren van de resource IP-adres met het IP-adres van de cloudservice en andere parameters wordt ingesteld als de test-poort. Wanneer de bron IP-adres online gebracht is, kunt deze vervolgens reageren op de peiling op de test-poort van het eindpunt taakverdeling eerder in deze zelfstudie hebt gemaakt.

## <a name="bring-the-listener-online"></a>Online de luisteraar ervan af brengen

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Items voor opvolgen

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Test de luisteraar ervan af groep beschikbaarheid (binnen de dezelfde VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Test de beschikbaarheid van de groep luisteraar ervan af (via internet)

Toegang tot de luisteraar ervan af van buiten het virtuele netwerk, moet u extern/openbare taakverdeling (beschreven in dit onderwerp) in plaats van ILB, namelijk alleen toegankelijk binnen de dezelfde VNet. In de verbindingsreeks, moet u de naam van de service cloud opgeven. Als u een cloudservice met de naam *mycloudservice*had, zou de instructie sqlcmd bijvoorbeeld als volgt:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

In tegenstelling tot het vorige voorbeeld, moet SQL-verificatie worden gebruikt, omdat de beller windows-authenticatie via internet niet gebruiken. Zie voor meer informatie [altijd op beschikbaarheid van de groep in Azure VM: Client Connectivity-scenario's](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). Wanneer u een SQL-verificatie gebruikt, zorg dat u dezelfde aanmeldingsnaam op beide replica's maken. Voor meer informatie over het oplossen van aanmeldingen met beschikbaarheidsgroepen zien [hoe u aanmeldingen toewijzen of gebruik bevat SQL database-gebruiker om uit te verbinden met andere replica's en in kaart brengen op beschikbaarheid databases](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Als wordt de altijd op replica's in verschillende subnetten zijn, clients moeten opgeven **MultisubnetFailover = True** in de verbindingstekenreeks. Hierdoor parallelle verbindingen met replica's in de verschillende subnetten. Houd er rekening mee dat dit geldt ook voor de implementatie van een cross-regio altijd op beschikbaarheid van de groep.

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
