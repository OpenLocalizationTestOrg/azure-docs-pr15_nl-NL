<properties 
   pageTitle="DNS configureren tussen twee Azure virtuele netwerken | Microsoft Azure" 
   description="Meer informatie over het configureren van VPN-verbindingen en domeinnamen omzetten tussen twee virtuele netwerken, en hoe HBase geografische-replicatie configureren." 
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

# <a name="configure-dns-between-two-azure-virtual-networks"></a>DNS tussen twee Azure virtuele netwerken configureren

> [AZURE.SELECTOR]
- [VPN-connectiviteit configureren](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [DNS configureren](hdinsight-hbase-geo-replication-configure-dns.md)
- [HBase replicatie configureren](hdinsight-hbase-geo-replication.md) 


Informatie over het toevoegen en configureren van DNS-servers met Azure virtuele netwerken worden afgehandeld naamresolutie binnen en tussen de virtuele netwerken.

Deze zelfstudie is het tweede deel van de [reeks] [ hdinsight-hbase-geo-replication] over het maken van HBase geografische-herhaling:

- [Een VPN-verbinding tussen twee virtuele netwerken configureren][hdinsight-hbase-geo-replication-vnet]
- DNS configureren voor de virtuele netwerken (deze zelfstudie)
- [HBase geografische replicatie configureren][hdinsight-hbase-geo-replication]


In het volgende diagram ziet u de twee virtuele netwerken die u hebt gemaakt in [een VPN-verbinding tussen twee virtuele netwerken configureren][hdinsight-hbase-geo-replication-vnet]:

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

- **Twee Azure virtuele netwerk met een VPN-verbinding met**.  Zie [een VPN-verbinding tussen twee Azure virtuele netwerken configureren]voor instructies[hdinsight-hbase-geo-replication-vnet].

>[AZURE.NOTE] Servicenamen van Azure-en VM moet uniek zijn. De naam die in deze zelfstudie is Contoso-[naam van de Azure-Service/VM]-[Europese Unie / Amerikaans]. Contoso-VNet-Europese Unie is bijvoorbeeld het Azure virtuele netwerk in het datacenter Noord Europe; Contoso-DNS-nl is de DNS-server VM in het midden van de gegevens Oost VS. U moet worden met de namen van uw eigen.
 
 
##<a name="create-azure-virtual-machines-to-be-used-as-dns-servers"></a>Azure virtuele machines moet worden gebruikt als de DNS-servers maken

**Maken van een virtuele machine binnen Contoso-VNet-Europese Unie, genaamd Contoso-DNS-Europese Unie**

1.  Klik op **Nieuw**, **berekenen**, **VM**, **FROM-GALERIE**.
2.  Kies **Windows Server 2012 R2 Datacenter**.
3.  Voer in:
    - **De naam van de VM**: Contoso-DNS-Europese Unie
    - **Nieuwe gebruikersnaam in te voeren**: 
    - **Een nieuw wachtwoord**: 
4.  Voer in:
    - **CLOUDSERVICE**: een nieuwe cloudservice maken
    - **Land/AFFINITEIT groep/virtuele netwerk**: (Selecteer Contoso-VNet-Europese Unie)
    - **Virtuele NETWERKSUBNETTEN**: Subnet-1
    - **Opslag-ACCOUNT**: een account automatisch gegenereerde opslag gebruiken
    
        De naam van de cloud-service is hetzelfde als de naam van de virtuele machine. In dit geval is die Contoso-DNS-Europese Unie. Voor latere virtuele machines kunt ik kiezen om het gebruik van de dezelfde cloudservice.  De virtuele machines onder de dezelfde cloudservice delen dezelfde virtueel netwerk en domeinachtervoegsel.

        De opslag-account wordt gebruikt voor de opslag van het afbeeldingsbestand VM. 
    - **EINDPUNTEN**: (Schuif omlaag en selecteer **DNS**) 

Nadat de virtuele machine is gemaakt, lees de interne IP- en externe IP-adres.

1.  Klik op de naam van de virtuele machine, **Contoso-DNS-Europese Unie**.
2.  Klik op **DashBoard**.
3.  Noteer:
    - OPENBARE VIRTUELE IP-ADRES
    - INTERNE IP-ADRES


**Maken van een virtuele machine binnen Contoso-VNet-US, genaamd Contoso-DNS-nl** 

- Herhaal deze procedure om een virtuele machine maken met de volgende waarden:
    - De naam van de VM: Contoso-DNS-nl
    - LAND/AFFINITEIT groep/virtuele netwerk: Contoso-VNET-nl selecteren
    - VIRTUAL NETWORK SUBNETTEN: Subnet-1
    - OPSLAG-ACCOUNT: Een account automatisch gegenereerde opslag gebruiken
    - EINDPUNTEN: (DNS selecteren)

##<a name="set-static-ip-addresses-for-the-two-virtual-machines"></a>Statische IP-adressen voor de twee virtuele machines instellen

DNS-servers vereist statische IP-adressen.  Deze stap kan niet worden uitgevoerd in de klassieke Azure-Portal. Gebruikt u Azure PowerShell.

**Statische IP-adres van de twee virtuele machines configureren**

1. Open Windows filter wissen.
2. Voer de volgende cmdlets.  

        Add-AzureAccount
        Select-AzureSubscription [YourAzureSubscriptionName]
        
        Get-AzureVM -ServiceName Contoso-DNS-EU -Name Contoso-DNS-EU | Set-AzureStaticVNetIP -IPAddress 10.1.0.4 | Update-AzureVM
        Get-AzureVM -ServiceName Contoso-DNS-US -Name Contoso-DNS-US | Set-AzureStaticVNetIP -IPAddress 10.2.0.4 | Update-AzureVM 

    Servicenaam is de naam van de cloud-service. Omdat de DNS-server de eerste virtuele machine van de cloudservice is, wordt de naam van de cloud-service is hetzelfde als de naam van de virtuele machine.

    Mogelijk moet u bijwerken servicenaam en naam zodat deze overeenkomt met de namen die u hebt.


##<a name="add-the-dns-server-role-the-two-virtual-machines"></a>De DNS-Server rol de twee virtuele machines toevoegen

**Het toevoegen van de rol van de DNS-Server voor Contoso-DNS-Europese Unie**

1.  Klik in de klassieke Portal Azure **virtuele Machines** aan de linkerkant. 
2.  Klik op **Contoso-DNS-Europese Unie**.
3.  Klik op **DASHBOARD** vanaf de bovenkant.
4.  Klik op **verbinding maken** vanaf de onderkant en volg de instructies voor verbinding maken met de virtuele machine via RDP.
2.  Binnen de RDP-sessie, klikt u op de Windows-knop in de linkerbenedenhoek om te openen van het startscherm.
3.  Klik op de tegel **Serverbeheer** .
4.  Klik op **functies en onderdelen toevoegen**.
5.  Klik op **volgende**
6.  Selecteer **op basis van rollen of functies gebaseerde installatie**en klik vervolgens op **volgende**.
7.  Selecteer uw DNS-virtuele machine (deze moet worden gemarkeerd al) en klik vervolgens op **volgende**.
8.  Controleer de **DNS-Server**.
9.  Klik op **Functies toevoegen**en klik vervolgens op **Doorgaan**.
10. Klik driemaal op **volgende** en klik vervolgens op **installeren**. 

**Het toevoegen van de rol van de DNS-Server voor Contoso-DNS-nl**

- Herhaal de stappen voor het toevoegen van DNS-rol aan **Contoso-DNS-nl**.

##<a name="assign-dns-servers-to-the-virtual-networks"></a>DNS-servers toewijzen aan de virtuele netwerken

**De twee DNS-servers registreren**

1.  In de klassieke Portal Azure klikt u op **Nieuw**, **Netwerk-SERVICES**, **VIRTUAL NETWORK**, **DNS-SERVER REGISTREREN**.
2.  Voer in:
    - **Naam**: Contoso-DNS-Europese Unie
    - **IP-adres van DNS-SERVER**: 10.1.0.4 – het IP-adres moet overeenkomen met de DNS-server VM IP-adres.
     
3.  Herhaal het proces voor het registreren van Contoso-DNS-nl met de volgende instellingen:
    - **Naam**: Contoso-DNS-nl
    - **IP-adres van DNS-SERVER**: 10.2.0.4

**De twee DNS-servers toewijzen aan de twee virtuele netwerken**

1.  Klik op **netwerken te gebruiken** in het linkerdeelvenster in de klassieke Portal.
2.  Klik op **Contoso-VNet-Europese Unie**.
3.  Klik op **configureren**.
4.  Selecteer **Contoso-DNS-Europese Unie** in de sectie **DNS-servers** .
5.  Klik op **Opslaan** op de onderkant van de pagina en klik op **Ja** om te bevestigen.
6.  Herhaal het proces voor het toewijzen van de **Contoso-DNS-nl** DNS-server met de **Contoso-VNet-nl** virtueel netwerk.

Alle de virtuele machines die zijn geïmplementeerd naar de virtuele netwerken moet opnieuw worden gestart als u wilt bijwerken van de configuratie van DNS-server.

**Opnieuw opstarten de virtuele machines**

1. Klik in de klassieke Portal Azure **virtuele Machines** aan de linkerkant.
2. Klik op **Contoso-DNS-Europese Unie**.
3. Klik op **Dashboard** vanaf de bovenkant.
4. Klik op **Start opnieuw** op onder.
5. Herhaal dezelfde stappen om **Contoso-DNS-nl**opnieuw te starten.


##<a name="configure-dns-conditional-forwarders"></a>Voorwaardelijke DNS-doorstuurservers configureren

De DNS-server op elke virtueel netwerk kunt u DNS-namen in deze virtuele netwerk alleen verhelpen. U moet een voorwaardelijk doorsturen zodat deze verwijzen naar de peer DNS-server voor oplossingen van de naam in de virtuele peer-netwerk configureren.

Als u wilt configureren voorwaardelijk doorsturen, moet u weten de domeinachtervoegsels van de twee DNS-servers. De DNS-achtervoegsels kunnen afwijken zijn afhankelijk van de configuratie van de Cloud Services die u gebruikt wanneer u de virtuele machines hebt gemaakt. Voor elke DNS-achtervoegsel in het virtuele netwerk gebruikt, moet u een voorwaardelijk doorsturen toevoegen. 

**De domeinachtervoegsels van de twee DNS-servers zoeken**

1. RDP in **Contoso-DNS-Europese Unie**.
2. Open Windows PowerShell-console of opdrachtprompt.
3. **Ipconfig**uitvoeren en noteer **verbinding specifieke DNS-achtervoegsel**.
4. De RDP-sessie niet sluit, moet u deze. 
5. Herhaal deze stappen om vast te stellen het **achtervoegsel verbinding / regiospecifieke** van **Contoso-DNS-nl**.


**DNS-doorstuurservers configureren**
 
1.  Klik op de sessie RDP **Contoso-DNS-Europese Unie**, op de Windows-toets in de linkerbenedenhoek.
2.  Klik op **Systeembeheer**.
3.  Klik op **DNS**.
4.  Vouw in het linkerdeelvenster **DSN**, **Contoso-DNS-Europese Unie**.
5.  Voer de volgende gegevens:
    - **DNS-domein**: Voer het achtervoegsel van de Contoso-DNS-VS. Bijvoorbeeld: Contoso-DNS-US.b5.internal.cloudapp.net.
    - **IP-adressen van de basispagina servers**: 10.2.0.4, dat wil de Contoso-DNS-VS van IP-adres zeggen invoeren.
6.  Druk op **ENTER**en klik vervolgens op **OK**.  Nu is mogelijk om op te lossen de Contoso-DNS-VS van IP-adres van Contoso-DNS-Europese Unie.
7.  Herhaal de stappen voor het toevoegen van een DNS-doorstuurservers aan de DNS-service op de Contoso-DNS-nl virtuele machine met de volgende waarden:
    - **DNS-domein**: Voer het achtervoegsel van de Contoso-DNS-EU. 
    - **IP-adressen van de basispagina servers**: 10.2.0.4, dat wil de Contoso-DNS-EU van IP-adres zeggen invoeren.

##<a name="test-the-name-resolution-across-the-virtual-networks"></a>Test de naamresolutie in de virtuele netwerken

U kunt nu hostnamen testen in de virtuele netwerken. Ping wordt geblokkeerd door de firewall al dan niet standaard.  U kunt nslookup gebruiken voor het oplossen van de DNS-server virtuele machines (u moet gebruiken FQDN) in de peer-netwerken.  


##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe naamresolutie configureren via virtuele netwerken met een VPN-verbindingen. De andere twee artikelen in de reeks omvatten:

- [Een VPN-verbinding tussen twee Azure virtuele netwerken configureren][hdinsight-hbase-geo-replication-vnet]
- [HBase geografische replicatie configureren][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[powershell-install]: powershell-install-configure.md

[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-DNS/HDInsight.HBase.VPN.diagram.png 
