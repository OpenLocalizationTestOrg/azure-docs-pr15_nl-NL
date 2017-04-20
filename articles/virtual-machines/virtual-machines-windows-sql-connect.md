<properties
    pageTitle="Verbinding maken met een SQL Server-VM (resourcemanager) | Microsoft Azure"
    description="Leer hoe u verbinding maken met SQL Server wordt uitgevoerd op een virtuele Machine in Azure wordt aangegeven. In dit onderwerp wordt het implementatiemodel klassieke gebruikt. De scenario's al naargelang de netwerkconfiguratie en de locatie van de client."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Verbinding maken met een SQL Server virtuele Machine op Azure (resourcemanager)

> [AZURE.SELECTOR]
- [Resourcemanager](virtual-machines-windows-sql-connect.md)
- [Klassieke](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Overzicht

Dit onderwerp wordt beschreven hoe u verbinding maken met uw SQL Server-instantie uitgevoerd op een Azure virtuele machines. Er heeft betrekking op sommige [algemene connectivity scenario's](#connection-scenarios) en vervolgens bevat [gedetailleerde stappen voor het configureren van SQL Server-connectiviteit in een VM Azure](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel. Zie [verbinding maken naar een SQL Server virtuele Machine op Azure klassieke](virtual-machines-windows-classic-sql-connect.md)de lightversie van dit artikel.

Als u liever een volledig overzicht van de inrichting van en -connectiviteit, raadpleegt u de [inrichting van een SQL Server virtuele Machine op Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Scenario's voor verbinding

De manier waarop die een client verbinding maakt met SQL Server wordt uitgevoerd op een virtuele Machine verschilt afhankelijk van de locatie van de client en de configuratie machine/netwerken. Deze scenario's zijn:

- [Verbinding maken met SQL Server via internet](#connect-to-sql-server-over-the-internet)
- [Verbinding maken met SQL Server in hetzelfde virtuele netwerk](#connect-to-sql-server-in-the-same-virtual-network)

### <a name="connect-to-sql-server-over-the-internet"></a>Verbinding maken met SQL Server via Internet

Als u verbinden met uw SQL Server-database-engine van Internet wilt, zijn er verschillende stappen vereist, zoals het configureren van de firewall, SQL-verificatie inschakelen en configureren van de beveiligingsgroep van uw netwerk hebt u een regel voor netwerk-beveiligingsgroep toe te staan dat TCP-verkeer is toegestaan op poort 1433.

Als u de portal voor het inrichten van de afbeelding van een SQL Server-VM met bronbeheer gebruikt, wordt deze stappen worden uitgevoerd voor u wanneer u de optie SQL-verbinding voor **openbare** selecteert:

![Openbare SQL-connectiviteit optie tijdens het inrichten](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Als dit niet een was tijdens het inrichten, kunt klikt u vervolgens u handmatig configureren SQL Server en uw virtuele machines volgens de [stappen in dit artikel voor het handmatig configureren connectivity](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

>[AZURE.NOTE] De afbeelding VM voor SQL Server Express edition wordt niet automatisch ingeschakeld voor het TCP/IP-protocol. Voor Express edition, moet u SQL Server Configuration Manager [handmatig](#configure-sql-server-to-listen-on-the-tcp-protocol) inschakelen het TCP/IP-protocol na het maken van de VM.

Als dit is ingevuld, kunt een client met internettoegang verbinden met de SQL Server-instantie door op te geven op het openbare IP-adres van de virtuele machine of de DNS-label die zijn toegewezen aan dat IP-adres. Als de SQL Server-poort 1433 is, hoeft u niet opgeven in de verbindingsreeks.

    "Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Hoewel hierdoor kunnen connectivity voor clients via internet, betekent dit niet dat iemand verbinding met uw SQL-Server maken kunt. Buiten moeten clients u de juiste gebruikersnaam en wachtwoord. Voor extra beveiliging, kunt u de bekende poort 1433 voorkomen. Als u SQL Server poort 1500 op geconfigureerd en tot stand BEGINLETTERS firewall uit en de groep beveiligingsregels netwerk gebracht, kunt u kan bijvoorbeeld voor verbinden door het poortnummer toe te voegen aan de naam van de Server zoals in het volgende voorbeeld:

    "Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

>[AZURE.NOTE] Het is belangrijk te weten dat wanneer u deze methode gebruiken om te communiceren met SQL Server, alle uitgaande gegevens van het datacenter van de Azure is onderhevig aan normale [prijzen op uitgaande overdracht](https://azure.microsoft.com/pricing/details/data-transfers/).

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Verbinding maken met SQL Server in hetzelfde virtuele netwerk

[Virtuele netwerk](../virtual-network/virtual-networks-overview.md) kunt extra scenario's. U kunt VMs koppelen in hetzelfde virtuele netwerk, zelfs als deze VMs aanwezig zijn in verschillende resourcegroepen. En met een [site-naar-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md), kunt u een hybride architectuur die VMs maakt een verbinding met on-premises implementatie-netwerken te gebruiken en machines maken.

Virtuele netwerken kunt u uw Azure VMs toevoegen aan een domein. Dit is de enige manier om het Windows-verificatie met SQL Server gebruikt. De andere verbinding-scenario's vereisen SQL-verificatie met gebruikersnamen en wachtwoorden opnieuw instellen.

Als u de portal voor het inrichten van de afbeelding van een SQL Server-VM met bronbeheer gebruikt, zijn de juiste firewallregels voor communicatie in de virtuele netwerk instellen wanneer u de optie SQL-connectiviteit **privé** selecteert. Als dit niet een was tijdens het inrichten, kunt klikt u vervolgens u handmatig configureren SQL Server en uw virtuele machines volgens de [stappen in dit artikel voor het handmatig configureren connectivity](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm). Maar als u van plan bent om een domeinomgeving en Windows-verificatie te configureren, hoeft u niet aan de SQL-verificatie en aanmeldingen configureren via de stappen in dit artikel. U ook hoeft niet te configureren netwerk beveiligingsgroep regels voor toegang via internet.

>[AZURE.NOTE] De afbeelding VM voor SQL Server Express edition wordt niet automatisch ingeschakeld voor het TCP/IP-protocol. Voor Express edition, moet u SQL Server Configuration Manager [handmatig](#configure-sql-server-to-listen-on-the-tcp-protocol) inschakelen het TCP/IP-protocol na het maken van de VM.

Ervan uitgaande dat u DNS in uw netwerk virtuele hebt geconfigureerd, kunt u verbinding maken met uw SQL Server-instantie door het opgeven van de naam van de SQL Server VM computer in de verbindingstekenreeks. Het volgende voorbeeld wordt ook aangenomen dat ook Windows-verificatie is geconfigureerd en dat de gebruiker heeft toegang tot de SQL Server-instantie.

    "Server=mysqlvm;Integrated Security=true"

Houd er rekening mee dat in dit scenario u ook het IP-adres van de VM opgeven.

## <a name="steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm"></a>Stappen voor het handmatig configureren van SQL Server-connectiviteit in een VM Azure

De volgende stappen wordt getoond hoe u handmatig setup-connectiviteit met de SQL Server-instantie en sluit vervolgens desgewenst via internet met SQL Server Management Studio (SSMS). Houd er rekening mee dat veel van deze stappen zijn voltooid voor u wanneer u de gewenste opties voor SQL Server-connectiviteit in de portal selecteert.

Voordat u een verbinding met het exemplaar van SQL Server uit een andere VM of op internet maken kunt, moet u de volgende taken uitvoeren, zoals wordt beschreven in de volgende gedeelten:

- [TCP-poorten openen in Windows firewall](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [SQL Server op het TCP-protocol luistert configureren](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [SQL Server configureren voor verificatie in gemengde modus](#configure-sql-server-for-mixed-mode-authentication)
- [SQL Server-verificatie aanmeldingen maken](#create-sql-server-authentication-logins)
- [Een beveiligingsgroep netwerk binnenkomende regel configureren](#configure-a-network-security-group-inbound-rule-for-the-vm)
- [Een DNS-Label voor het openbare IP-adres configureren](#configure-a-dns-label-for-the-public-ip-address)
- [Verbinding maken met de Database-Engine vanuit een andere computer](#connect-to-the-database-engine-from-another-computer)

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager-nsg-rule.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Volgende stappen

Instructies samen met deze stappen connectivity inrichting raadpleegt u [een SQL Server virtuele Machine op Azure inrichten](virtual-machines-windows-portal-sql-server-provision.md).

[Verkennen leerpad](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) voor SQL Server op Azure virtuele machines.

Zie voor andere onderwerpen over met SQL Server in Azure VMs, [SQL Server op Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md).
