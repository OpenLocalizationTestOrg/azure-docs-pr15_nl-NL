<properties
   pageTitle="Luisteraar ervan af voor AlwaysOn availabilty groep maken voor SQL Server in Azure virtuele Machines"
   description="Stapsgewijze instructies voor het maken van een luisteraar ervan af voor een groep van AlwaysOn availabilty voor SQL Server in Azure virtuele Machines"
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="07/12/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-an-internal-load-balancer-for-an-alwayson-availability-group-in-azure"></a>Een interne taakverdeling voor een groep AlwaysOn beschikbaarheid in Azure configureren

In dit onderwerp wordt uitgelegd hoe u een interne taakverdeling voor een groep van de beschikbaarheid van SQL Server AlwaysOn maakt in Azure virtuele machines uitgevoerd in resource manager model. Een groep AlwaysOn beschikbaarheid vereist een taakverdeling wanneer de exemplaren van SQL Server Azure virtuele machines. De taakverdeling slaat het IP-adres voor de beschikbaarheid van de groep luisteraar ervan af. Als een beschikbaarheidsgroep zich uitstrekt over meerdere gebieden, moet elke regio een taakverdeling.

Als u wilt deze taak uitvoert, moet u beschikken over een SQL Server AlwaysOn beschikbaarheid van de groep geïmplementeerd op Azure virtuele machines in resource manager model. Beide SQL Server virtuele machines moet behoren tot dezelfde beschikbaarheid set. U kunt de [Microsoft-sjablonen](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) automatisch maken van de groep AlwaysOn beschikbaarheid in Azure resourcemanager. De interne taakverdeling deze sjabloon worden automatisch voor u gemaakt. 

Als u liever, kunt u [een groep AlwaysOn beschikbaarheid handmatig configureren](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

In dit onderwerp is vereist dat uw beschikbaarheid groepen al zijn geconfigureerd.  

Verwante onderwerpen zijn:

 - [AlwaysOn beschikbaarheid van groepen in Azure VM (grafische gebruikersinterface) configureren](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
 
 - [Een verbinding VNet-naar-VNet configureren met behulp van Azure resourcemanager en PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="steps"></a>Stappen

Door dit document doorlopen u maken en configureren van een taakverdeling in de portal van Azure. Wanneer dat is voltooid, configureert u het cluster als u wilt gebruiken het IP-adres van de verdeling van de belasting voor de luisteraar ervan af van AlwaysOn beschikbaarheid van de groep.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Maken en configureren van de taakverdeling in de portal van Azure

In dit gedeelte van de taak wordt u de volgende stappen uit in de portal van Azure doen:

1. De taakverdeling maken en configureren van het IP-adres

1. De backend-groep configureren

1. De test maken 

1. De regels van taakverdeling instellen

>[AZURE.NOTE] Als de SQL-Servers in verschillende resourcegroepen en regio's zijn, doet u al deze stappen tweemaal, eenmaal in elke resourcegroep.

## <a name="1-create-the-load-balancer-and-configure-the-ip-address"></a>1. de taakverdeling maken en configureren van het IP-adres

De eerste stap is het opzetten van de taakverdeling. Open de resourcegroep met de SQL Server virtuele machines in de portal Azure. Klik in de resourcegroep op **toevoegen**.

- Zoek naar **de belasting voor documentconversies**. Selecteer in de lijst met zoekresultaten **Taakverdeling**, die zijn gepubliceerd door **Microsoft**.

- Klik op het blad **Taakverdeling** op **maken**.

- Klik op **maken de belasting voor documentconversies**, configureren de verdeling van de belasting als volgt:

| Instelling | Waarde |
| ----- | ----- |
| **Naam** | Een tekstnaam die de taakverdeling vertegenwoordigt. Bijvoorbeeld: **sqlLB**. |
| **Schema** | **Interne** |
| **Virtuele netwerk** | Kies het virtuele netwerk die de SQL-Servers in.   |
| **Subnet**  | Kies het subnet die de SQL-Servers in. |
| **Abonnement** | Als u meerdere abonnementen hebt, kan dit veld weergegeven. Selecteer het abonnement dat u wilt dat is gekoppeld aan deze resource. Het is gewoonlijk de hetzelfde abonnement als alle bronnen voor de van de beschikbaarheidsgroep.  |
| **Resourcegroep** | Kies de resourcegroep die de SQL-Servers in. | 
| **Locatie** | Kies de Azure locatie die de SQL-Servers in. |

- Klik op **maken**. 

Azure Hiermee maakt u de verdeling van de belasting die u hierboven hebt geconfigureerd. De taakverdeling hoort bij een specifieke netwerk, subnet, resourcegroep en locatie. Nadat Azure is voltooid, controleert u of de instellingen van de verdeling van laden in Azure wordt aangegeven. 

Nu configureren het laden de verdeling van IP-adres.  

- Klik op het blad laden de verdeling van **Instellingen** , klikt u op **IP-adres**. Het **IP-adres** blad zien dat dit een privé taakverdeling op hetzelfde virtuele netwerk als uw SQL-Servers. 

- Stel de volgende instellingen: 

| Instelling | Waarde |
| ----- | ----- |
| **Subnet** | Kies het subnet die de SQL-Servers in. |
| **Toewijzing** | **Statische** |
| **IP-adres** | Typ een niet-gebruikte virtuele IP-adres van het subnet.  |

- De instellingen opslaan.

De taakverdeling bevat nu een IP-adres. Record dit IP-adres. U kunt dit IP-adres worden gebruikt wanneer u een luisteraar ervan af op het cluster maken. In een PowerShell-script verderop in dit artikel, gebruikt u dit adres voor de `$ILBIP` variabele.



## <a name="2-configure-the-backend-pool"></a>2. de backend-groep configureren

De volgende stap is het opzetten van een resourcegroep die backend-mailadres. Azure oproepen de backend-toepassingen *backend-toepassingen*. In dit geval is de backend-toepassingen de adressen van de twee SQL-Servers in uw van de beschikbaarheidsgroep. 

- Klik op de verdeling van de belasting die u hebt gemaakt in de resourcegroep. 

- Klik op **Instellingen**, klikt u op **back-end-toepassingen**.

- Klik op **back-end-mailadres van toepassingen**, op **toevoegen** om te maken van een resourcegroep die backend-mailadres. 

- Typ op de **backend-groep toevoegen** onder **naam**een naam voor de backend-toepassingen.

- Klik op **+ toevoegen een virtuele machine**onder **virtuele machines** . 

- Klik onder **Kies virtuele machines** op **kiest u een set beschikbaarheid** en geef dat de beschikbaarheid instellen dat de SQL Server virtuele machines behoort.

- Nadat u de beschikbaarheid van de set hebt gekozen, klikt u op **de virtuele machines kiezen**. Klik op de twee virtuele machines die als host van de exemplaren van SQL Server in de beschikbaarheidsgroep. Klik op **selecteren**. 

- Klik op **OK** om te sluiten van de bladen voor **kiezen virtuele machines**en **backend-groep toevoegen**. 

Azure bijgewerkt via de instellingen voor de groep backend-adressen. Uw beschikbaarheid instellen bevat nu een resourcegroep die twee SQL-Servers.

## <a name="3-create-a-probe"></a>3. een test maken

De volgende stap is het opzetten van een test. De test wordt gedefinieerd hoe Azure wordt controleren welke van de SQL-Servers momenteel eigenaar is van de beschikbaarheid van de groep luisteraar ervan af. Azure wordt de service die op basis van IP-adres op een poort die u bij het maken van de test definieert onderzoeken.

- Klik op het blad laden de verdeling van **Instellingen** , klikt u op **controleert**. 

- Klik op het blad **controleert** op **toevoegen**.

- Configureer de test op het blad **toevoegen-test** . Gebruik de volgende waarden voor het configureren van de test:

| Instelling | Waarde |
| ----- | ----- |
| **Naam** | Een tekstnaam dat staat voor de test. Bijvoorbeeld: **SQLAlwaysOnEndPointProbe**. |
| **Protocol** | **TCP** |
| **Poort** | U kunt een beschikbare poort. Bijvoorbeeld: *59999*.    |
| **Interval**  | *5* | 
| **Drempelwaarde voor beschadigd**  | *2* | 

- Klik op **OK**. 

>[AZURE.NOTE] Zorg ervoor dat de poort die u opgeeft geopend op de firewall van beide SQL-Servers is. Beide servers vereisen een binnenkomende regel voor het TCP-poorten die u gebruikt. Zie [toevoegen of Firewall-regel bewerken](http://technet.microsoft.com/library/cc753558.aspx) voor meer informatie. 

Azure Hiermee maakt u de test. Azure wordt de test gebruikt om te testen welke SQL-Server de luisteraar ervan af voor de van de beschikbaarheidsgroep heeft.

## <a name="4-set-the-load-balancing-rules"></a>4. de regels van taakverdeling instellen

Stel de regels van taakverdeling. De belasting taakverdeling regels configureren hoe de taakverdeling stuurt verkeer door met de SQL-Servers. Voor deze taakverdeling wordt u direct server afzender inschakelen omdat slechts één van de twee SQL-Servers ooit eigenaar is van de beschikbaarheid van de groep luisteraar ervan af resource tegelijk.

- Klik op het blad laden de verdeling van **Instellingen** , klikt u op **laden van taakverdeling regels**. 

- Klik op het blad **taakverdeling regels** op **toevoegen**.

- Gebruik het blad **toevoegen laden taakverdeling regels** voor het configureren van de regel van taakverdeling. Gebruik de volgende instellingen: 

| Instelling | Waarde |
| ----- | ----- |
| **Naam** | Een tekstnaam die de regels van taakverdeling vertegenwoordigt. Bijvoorbeeld: **SQLAlwaysOnEndPointListener**. |
| **Protocol** | **TCP** |
| **Poort** | *1433*   |
| **Back-end poort** | *1433*. Houd er rekening mee dat deze wordt uitgeschakeld omdat deze regel wordt gebruikt dat **Zwevende IP (afzender directe server)**.   |
| **Zoeken** | Gebruik de naam van de test die u hebt gemaakt voor deze taakverdeling. |
| **Sessie persistentie**  | **Geen** | 
| **Inactiviteit (minuten)**  | *4* | 
| **Zwevende IP (afzender directe server)**  | **Ingeschakeld** | 

 >[AZURE.NOTE] Mogelijk moet u omlaag schuiven op het blad om alle instellingen weer te geven.

- Klik op **OK**. 

- Azure Hiermee configureert u de regel van taakverdeling. Nu is de taakverdeling geconfigureerd voor het routeren verkeer naar de SQL-Server waarop de luisteraar ervan af voor de van de beschikbaarheidsgroep. 

De resourcegroep heeft nu een taakverdeling verbinding maakt met de computers van beide SQL Server. De taakverdeling bevat ook een IP-adres voor de luisteraar ervan af van SQL Server AlwaysOn beschikbaarheid groep zodat beide machine op verzoeken om de van de beschikbaarheidsgroepen reageren kunt.

>[AZURE.NOTE] Als uw SQL-Servers in twee afzonderlijke regio's zijn, herhaalt u de stappen in de andere regio. Elke regio is een taakverdeling vereist. 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Configureer het cluster het laden de verdeling van IP-adres gebruiken 

De volgende stap is de luisteraar ervan af op het cluster configureren en online de luisteraar ervan af te brengen. U doet dit door het volgende doen: 

1. De beschikbaarheid-groep luisteraar ervan af op het failovercluster maken 

1. Online de luisteraar ervan af brengen

## <a name="1-create-the-availablity-group-listener-on-the-failover-cluster"></a>1. de beschikbaarheid-groep luisteraar ervan af op het failovercluster maken

In deze stap, moet u handmatig de luisteraar ervan af groep beschikbaarheid in Failoverclusterbeheer en SQL Server Management Studio (SSMS) maken.

- Met RDP verbinding maken met de Azure virtuele machine waarop de primaire replica. 

- Open Failoverclusterbeheer.

- Selecteer het knooppunt **netwerken** en noteer de naam van het cluster-netwerk. Deze naam wordt gebruikt in de `$ClusterNetworkName` variabele in de PowerShell-script.

- Naam van het cluster uitvouwen en klik vervolgens op **rollen**.

- Klik in het deelvenster **rollen** met de rechtermuisknop op de naam van de beschikbaarheid van de groep en selecteer vervolgens **Toevoegen Resource** > **Client-toegangspunt**.

- In het vak **naam** een naam voor deze nieuwe luisteraar ervan af, maken en klik tweemaal op **volgende** en klik vervolgens op **Voltooien**. Worden niet weergegeven in de luisteraar ervan af of de resource online op dit punt.

 >[AZURE.NOTE] De naam voor de nieuwe luisteraar ervan af is de naam van het netwerk waarmee toepassingen kunt verbinding maken met databases in de groep van de beschikbaarheid van SQL Server.

- Klik op het tabblad **Resources** en klik uitvouwen van de Client toegangspunt dat u zojuist hebt gemaakt. Met de rechtermuisknop op de IP-resource en klik op Eigenschappen. Noteer de naam van het IP-adres. U gebruikt deze naam in de `$IPResourceName` variabele in de PowerShell-script.

- Klik op **Statische IP-adres** en het statische IP-adres naar hetzelfde adres die u gebruikt wanneer u het laden de verdeling van IP-adres in de portal van Azure instellen onder **IP-adres** . NetBIOS inschakelen voor dit adres en klik op **OK**. Herhaal deze stap voor elke resource IP-als uw oplossing zich uitstrekt over meerdere Azure VNets. 

- Op het knooppunt waarop momenteel de primaire replica, opent u een verhoogde PowerShell ISE en plak de volgende opdrachten in een nieuw script.

        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    
        Import-Module FailoverClusters
    
        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

- Werk de variabelen en de PowerShell script uitvoeren om het IP-adres en poorten configureren voor de nieuwe luisteraar ervan af.

 >[AZURE.NOTE] Als uw SQL-Servers in verschillende regio's zijn, moet u de PowerShell-script tweemaal uitvoeren. De eerste keer gebruikt de naam van het cluster, cluster IP-resourcenaam, en het laden van de verdeling van IP-adres uit de eerste resourcegroep. De tweede keer gebruik naam van het cluster, cluster IP-resourcenaam, en het laden van de verdeling van IP-adres uit de tweede resourcegroep.

Het cluster bevat nu een resource beschikbaarheid van de groep luisteraar ervan af.

## <a name="2-bring-the-listener-online"></a>2. weer online de luisteraar ervan af

Met de beschikbaarheid van de groep luisteraar ervan af resource geconfigureerd, u kunt doen om de luisteraar ervan af online zodat toepassingen kunnen verbinden met databases in de beschikbaarheidsgroep met de luisteraar ervan af.

- Ga terug naar Failoverclusterbeheer. Vouw **rollen** en selecteer vervolgens de beschikbaarheid van de groep. Met de rechtermuisknop op de naam van de luisteraar ervan af op het tabblad **Resources** en klikt u op **Eigenschappen**.

- Klik op het tabblad **afhankelijkheden** . Als er meerdere resources weergegeven, controleert u of de IP-adressen of niet hebt en, afhankelijkheden. Klik op **OK**.

- Met de rechtermuisknop op de naam van de luisteraar ervan af en klik op **Online brengen**.


- Wanneer de luisteraar ervan af online, het tabblad **Resources is** , met de rechtermuisknop op de van de beschikbaarheidsgroep en klik op **Eigenschappen**.

- Maak een afhankelijkheid op de luisteraar ervan af naam resource (niet de IP-adres resources naam). Klik op **OK**.


- SQL Server Management Studio starten en verbinding maken met de primaire replica.


- Navigeer naar **de beschikbaarheid van de AlwaysOn hoog** | **beschikbaarheidsgroepen** | **groep beschikbaarheid Listeners**. 


- U ziet nu de naam van de luisteraar ervan af die u hebt gemaakt in Failoverclusterbeheer. Met de rechtermuisknop op de naam van de luisteraar ervan af en klik op **Eigenschappen**.


- Geef in het vak **poort** het poortnummer in de beschikbaarheid van de groep luisteraar ervan af met behulp van de $EndpointPort u eerder hebt gebruikt (1433 was de standaardinstelling), klikt u op **OK**.

U hebt nu een SQL Server AlwaysOn beschikbaarheid van de groep in Azure virtuele machines u resource manager afspeelt. 

## <a name="test-the-connection-to-the-listener"></a>De verbinding met de luisteraar ervan af testen

De verbinding testen:

1. RDP met een SQL Server die is in hetzelfde virtuele netwerk, maar niet de eigenaar de replica. Dit is de andere SQL Server in het cluster.

1. Hulpprogramma **sqlcmd** gebruiken om de verbinding te testen. Bijvoorbeeld het volgende script een verbinding tot stand **sqlcmd** aan de primaire replica tot en met de luisteraar ervan af met Windows-verificatie:

        sqlcmd -S <listenerName> -E

De verbinding SQLCMD automatisch verbinden met ongeacht exemplaar van SQL Server de primaire replica gehost. 

## <a name="guidelines-and-limitations"></a>Richtlijnen en beperkingen

Houd rekening met de volgende richtlijnen op beschikbaarheid groep luisteraar ervan af in Azure wordt aangegeven met een interne belasting voor documentconversies:

- Slechts één interne beschikbaarheid groep luisteraar ervan af wordt ondersteund per cloudservice omdat de luisteraar ervan af is geconfigureerd voor de taakverdeling en er slechts één interne taakverdeling. Het is echter mogelijk multipe externe listeners maken. 

- Met een interne taakverdeling u alleen toegang tot de luisteraar ervan af vanuit binnen hetzelfde virtuele netwerk.
 
