<properties 
   pageTitle="VPN-verbinding tussen twee virtuele netwerken configureren | Microsoft Azure" 
   description="Meer informatie over het configureren van VPN-verbindingen en domeinnamen omzetten tussen twee Azure virtuele netwerken, en hoe HBase geografische-replicatie configureren." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-a-vpn-connection-between-two-azure-virtual-networks"></a>Een VPN-verbinding tussen twee Azure virtuele netwerken configureren  

> [AZURE.SELECTOR]
- [VPN-connectiviteit configureren](hdinsight-hbase-geo-replication-configure-vnets.md)
- [DNS configureren](hdinsight-hbase-geo-replication-configure-dns.md)
- [HBase replicatie configureren](hdinsight-hbase-geo-replication.md) 

Azure virtuele naar website netwerkconnectiviteit werkt met een VPN-gateway zodat een beveiligde tunnel met Ipsec/IKE. De VNets kan zijn in verschillende-abonnementen en verschillende regio's. U kunt zelfs VNet naar VNet communicatie met meerdere locaties configuraties combineren. Er zijn verschillende redenen voor VNet naar VNet connectivity:

- Cross geografische regio-redundantie en geografische-aanwezigheid 
- Regionale meerlagige toepassingen met sterke moeten worden geïsoleerd grens 
- Cross tussen organisatie communicatie in Azure-abonnement

Zie [een VNet naar VNet verbinding configureren](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)voor meer informatie. 

Deze op video zien:

> [AZURE.VIDEO configure-the-vpn-connectivity-between-two-azure-virtual-networks]

Deze zelfstudie is een onderdeel van de [reeks] [ hdinsight-hbase-replication] over het maken van HBase geografische-herhaling. 

- Een VPN-verbinding tussen twee virtuele netwerken (deze zelfstudie) configureren
- [DNS configureren voor de virtuele netwerken][hdinsight-hbase-geo-replication-dns]
- [HBase geografische replicatie configureren][hdinsight-hbase-geo-replication]

In het volgende diagram ziet u de twee virtuele netwerken die u in deze zelfstudie maakt:

![HDInsight HBase herhaling virtuele netwerkdiagram][img-vnet-diagram]
 

##<a name="prerequisites"></a>Vereisten voor
Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Een workstation met Azure PowerShell**.

    Voordat u PowerShell-scripts, zorg dat er verbinding is met uw Azure-abonnement met de volgende cmdlet:

        Add-AzureAccount

    Als u meerdere Azure abonnementen hebt, gebruikt u de volgende cmdlet voor het instellen van het huidige abonnement:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


>[AZURE.NOTE] Servicenamen van Azure-en VM moet uniek zijn. De naam die in deze zelfstudie is Contoso-[naam van de Azure-Service/VM]-[Europese Unie / Amerikaans]. Contoso-VNet-Europese Unie is bijvoorbeeld het Azure virtuele netwerk in het datacenter Noord Europe; Contoso-DNS-nl is de DNS-server VM in de Oost-tabellen in Word. U moet worden met de namen van uw eigen.
 

##<a name="create-two-azure-vnets"></a>Twee Azure VNets maken



**Een virtueel netwerk Contoso-VNet-Europese Unie genoemd in Noord-Europa maken**

1.  Meld u aan bij de [Portal van Azure klassieke][azure-portal].
2.  Klik op **Nieuw**, **NETWERKSERVICES**, **virtuele netwerk**, **aangepaste maken**.
3.  Voer in:

    - **Naam**: Contoso-VNet-Europese Unie
    - **Locatie**: Noord Europa

        In deze zelfstudie wordt Noord Europa en Oost Amerikaans datacenters. U kunt uw eigen datacenters.
4.  Voer in:

    - **DNS-SERVER**: (laat deze leeg) 
    
        Moet u uw eigen DNS-server voor naamresolutie binnen virtuele netwerken. Zie voor meer informatie over wanneer de geleverde Azure naamresolutie gebruiken en wanneer gebruikt u uw eigen DNS-server, [Name resolutie (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). Zie [DNS tussen twee Azure virtuele netwerken configureren]voor instructies voor het configureren van naamresolutie tussen VNets[hdinsight-hbase-dns].
  
    - **Een VPN-verbinding voor de punt-naar-site configureren**: (uitgeschakeld)

        Punt-naar-site niet van toepassing op dit scenario.

    - **Een VPN-verbinding voor de site-naar-site configureren**: (uitgeschakeld)
    
        De site-naar-site VPN-verbinding met het Azure virtuele netwerk configureert u in de Oost-tabellen in Word.
5.  Voer in:

    -   **Adres ruimte starten IP**: 10.1.0.0
    -   **Adres ruimte CIDR**: / 16
    -   **Subnet-1 starten IP**: 10.1.0.0
    -   **Subnet-1-CIDR**: / 24

    De adresruimte kan niet overlappen met het virtuele netwerk VS.  

**Een virtueel netwerk Contoso-VNet-Europese Unie genoemd in West-Europa maken**

- Herhaal de laatste procedure met de volgende waarden:

    - **Naam**: Contoso-VNet-nl
    - **Locatie**: Oost VS
     
    - **DNS-SERVER**: (laat deze leeg)
    - **Een VPN-verbinding voor de punt-naar-site configureren**: (uitgeschakeld)
    - **Een VPN-verbinding voor de site-naar-site configureren**: (uitgeschakeld)
     
    - **Adres ruimte starten IP**: 10.2.0.0
    - **Adres ruimte CIDR**: / 16
    - **Subnet-1 starten IP**: 10.2.0.0
    - **Subnet-1-CIDR**: / 24

















##<a name="configure-a-vpn-connection-between-the-two-vnets"></a>Een VPN-verbinding tussen de twee VNets configureren

###<a name="create-local-networks"></a>Lokale netwerken maken

Wanneer u een VNet VNet configuratie maakt, moet u elke VNet om aan te duiden elkaar als een lokale netwerk-site configureren. In deze sectie, moet u elke VNet configureren als een lokale netwerk. De lokale netwerken delen de dezelfde IP-adres spaties met de bijbehorende VNet.

![Azure VPN naar website configuratie - azure lokale netwerken configureren][img-vnet-lnet-diagram]


**Een lokale netwerk maken genaamd Contoso-LNet-EU overeenkomen met de Contoso-VNet-Europese Unie netwerk-adresruimte**

1. Klik op **Nieuw**, **Netwerk-SERVICES**, **VIRTUAL NETWORK**, **Lokale netwerk toevoegen**in de klassieke Portal Azure.
3. Voer in:

    - **Naam**: Contoso-LNet-Europese Unie
    - **VPN apparaat IP-adres**: 192.168.0.1 (dit adres wordt bijgewerkt later)

        Meestal gebruikt u het werkelijke extern IP-adres voor een VPN-apparaat. Voor VNet naar VNet configuraties gebruikt u het IP-adres van de VPN gateway. Gezien het feit dat u hebt gemaakt VPN gateways voor de twee VNets nog, kunt u een arbitary IP-adres invoeren en keert u terug naar het probleem oplossen.
4.  Voer in:

    - **Adres ruimte starten IP:** 10.1.0.0
    - **Adres ruimte CIDR:** /16
    
    Dit moet altijd exact overeen met het bereik dat u eerder hebt opgegeven voor de Contoso-VNet-Europese Unie.

**Een lokale netwerk maken genaamd Contoso-LNet-nl overeenkomen met de Contoso-VNet-nl netwerk-adresruimte**

- Herhaal de laatste procedure met de volgende parameters:

    - **Naam**: Contoso-LNet-nl
    - **VPN apparaat IP-adres**: 192.168.0.1 (dit adres wordt bijgewerkt later)
     
    - **Adres ruimte starten IP**: 10.2.0.0
    - **Adres ruimte CIDR**: / 16


###<a name="create-vpn-gateways"></a>VPN gateways maken

Er zijn twee gedeelten in deze configuratie. Eerst het configureren van een VNet naar website verbinding met een lokale netwerk en maakt u een dynamische routeren VPN. VNet naar VNet vereist Azure VPN gateways met dynamische routeren VPN's. Azure statische routeren VPN's worden niet ondersteund.

**De verbinding van de site-naar-site Contoso-VNet-Europese Unie met Contoso-LNet-nl configureren**

1.  In de klassieke Portal Azure klikt u op **netwerken te gebruiken** in het linkerdeelvenster
2.  Klik op **Contoso-VNet-Europese Unie**.
3.  Klik op het tabblad **CONFIGUE** .
4.  Controleer de **verbinding maken met lokale netwerk**.
5.  Selecteer in de **Lokale netwerk**, **Contoso-LNet-nl**.
6.  Klik op **toevoegen gateway subnet** in de sectie virtueel netwerk spaties.
7.  Klik op **Opslaan**.
8.  Klik op **OK** om te bevestigen.


**Een VPN-gateway maken voor Contoso-VNet-Europese Unie**

1.  Klik op het tabblad **DASHBOARD** van de klassieke Azure-Portal.
4.  Klik op **GATEWAY maken** vanaf de onderkant van de pagina en klik op **Dynamische routering**.
5.  Klik op **Ja** om te bevestigen. Zoals u ziet de gateway-afbeelding op de pagina wordt gewijzigd in geel en zegt Gateway maken. Het duurt meestal ongeveer 15 minuten voor de gateway te maken.

    Wanneer de status van de gateway is veranderd in verbinding maken, is het IP-adres voor elke Gateway is zichtbaar in het Dashboard. Noteer het IP-adres dat overeenkomt met naar elke VNet, niet als u wilt ze verwisselt bezighoudt. Dit zijn de IP-adressen die wordt gebruikt bij het bewerken van uw tijdelijke aanduiding voor IP-adressen voor het apparaat VPN in lokale netwerken.

6.  Hiermee maakt u een kopie van de **GATEWAY IP-adres**. U kunt deze wordt gebruikt voor het configureren van de VPN gateway IP-adres voor Contoso-VNet-EU in het volgende gedeelte.

**Een VPN-gateway maken voor Contoso-VNet-Europese Unie**

- Herhaal de laatste twee procedure voor het configureren van de Contoso-VNet-nl-site op site-connectiviteit met Contoso-LNet-EU en het maken van een gateway VPN voor Contoso-Vnet-nl. Wanneer u klaar bent, hebt u het IP-adres van de VPN gateway voor Contoso-VNet-nl.


### <a name="set-the-vpn-device-ip-addresses-for-local-networks"></a>Het apparaat VPN IP-adressen voor lokale netwerken instellen
In de laatste sectie, kunt u een VPN-gateway maken voor elk van de VNets. U kunt de IP-adressen van de gateways VPN hebt hebt ontvangen. Nu kunt u teruggaan naar het lokale netwerk VPN apparaat IP-adressen configureren.

**Het IP-adres van de VPN apparaat configureren voor Contoso-LNet-Europese Unie** 

1.  Klik in de klassieke Portal Azure **netwerken te gebruiken** in het linkerdeelvenster.
2.  Klik op **Lokale netwerken** vanaf de bovenkant.
3.  **Contoso-LNet-Europese Unie**op en klik vervolgens op **bewerken** in onder.
4.  Werk **VPN apparaat IP-adres**.  Dit is het adres dat u via het tabblad DASHBOARD van Contoso-VNET-EU openen.
5.  Klik op de juiste knop.
6.  Klik op de knop controleren.

**Het IP-adres van de VPN apparaat configureren voor Contoso-LNet-nl** 

- Herhaal de laatste procedure kunt u het IP-adres van de VPN apparaat configureren voor Contoso-LNet-nl.

###<a name="set-vnet-gateway-keys"></a>VNet gateway toetsen instellen

Een gedeelde sleutel Vnet gateways gebruiken om te verifiëren van verbindingen tussen de virtuele netwerken. De toets kan niet worden geconfigureerd in de klassieke Azure-Portal. U moet PowerShell of .NET SDK gebruiken.

**De toetsen instellen**

1. Open vanuit uw werkstation **Windows filter wissen** of de Windows PowerShell-console.
2. Bijwerken van de parameters in dit script volgen en uit te voeren:

        Add-AuzreAccount
        Select-AzureSubscription -[AzureSubscriptionName]
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-EU -LocalNetworkSiteName Contoso-LNet-US -SharedKey A1b2C3D4
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-US -LocalNetworkSiteName Contoso-LNet-EU -SharedKey A1b2C3D4 


##<a name="check-the-vpn-connection"></a>Controleer de VPN-verbinding 

Zonder een VMs geïmplementeerd in de VNets, kunt u het virtuele visuele netwerkdiagram de VNet dashboardpagina gebruiken in de klassieke Azure-Portal de verbindingsstatus controleren:

![HDInsight HBase herhaling virtueel netwerk de status van de VPN-verbinding][img-vpn-status]
  



##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe een VPN-verbinding tussen twee Azure virtuele netwerken configureren. De andere twee artikelen in de reeks omvatten:

- [DNS tussen twee Azure virtuele netwerken configureren][hdinsight-hbase-geo-replication-dns]
- [HBase geografische replicatie configureren][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com

[powershell-install]: ../install-configure-powershell.md



[hdinsight-hbase-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-dns]: hdinsight-hbase-geo-replication-configure-dns.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-diagram.png
[img-vnet-lnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-lnet-diagram.png
[img-vpn-status]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-status.png 
