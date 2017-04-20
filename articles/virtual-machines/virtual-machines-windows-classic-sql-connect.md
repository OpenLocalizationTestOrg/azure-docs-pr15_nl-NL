<properties
    pageTitle="Verbinding maken met een SQL Server-VM (klassieke) | Microsoft Azure"
    description="Leer hoe u verbinding maken met SQL Server wordt uitgevoerd op een virtuele Machine in Azure wordt aangegeven. In dit onderwerp wordt het implementatiemodel klassieke gebruikt. De scenario's al naargelang de netwerkconfiguratie en de locatie van de client."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Verbinding maken met een SQL Server virtuele Machine op Azure (klassieke-implementatie)

> [AZURE.SELECTOR]
- [Resourcemanager](virtual-machines-windows-sql-connect.md)
- [Klassieke](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Overzicht

Dit onderwerp wordt beschreven hoe u verbinding maken met uw SQL Server-instantie uitgevoerd op een Azure virtuele machines. Er heeft betrekking op sommige [algemene connectivity scenario's](#connection-scenarios) en vervolgens bevat [gedetailleerde stappen voor het configureren van SQL Server-connectiviteit in een VM Azure](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Als u resourcemanager VMs gebruikt, raadpleegt u [verbinding maken naar een SQL Server virtuele Machine op Azure Resource Manager gebruiken](virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Scenario's voor verbinding

De manier waarop die een client verbinding maakt met SQL Server wordt uitgevoerd op een virtuele Machine verschilt afhankelijk van de locatie van de client en de configuratie machine/netwerken. Deze scenario's zijn:

- [Verbinding maken met SQL Server in de dezelfde cloudservice](#connect-to-sql-server-in-the-same-cloud-service)
- [Verbinding maken met SQL Server via internet](#connect-to-sql-server-over-the-internet)
- [Verbinding maken met SQL Server in hetzelfde virtuele netwerk](#connect-to-sql-server-in-the-same-virtual-network)

>[AZURE.NOTE] Voordat u verbinding met een van de volgende manieren maakt, moet u volgen de [stappen in dit artikel voor connectiviteit configureren](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Verbinding maken met SQL Server in de dezelfde cloudservice

Meerdere virtuele machines kunnen worden gemaakt in de dezelfde cloudservice. Als u wilt weten over dit scenario virtuele machines, Zie [hoe u verbinding maakt virtuele machines met een virtueel netwerk of cloud-service](virtual-machines-windows-classic-connect-vms.md#connect-vms-in-a-standalone-cloud-service). Dit scenario is wanneer u een client op één virtuele machine probeert te verbinden met SQL Server wordt uitgevoerd op een andere virtuele machine in de dezelfde cloudservice.

In dit scenario kunt u verbinding maken met de VM- **naam** (ook weergegeven als **De naam van de Computer** of **hostname** in de portal). Dit is de naam die u hebt opgegeven voor de VM tijdens het maken van. Bijvoorbeeld als u de naam van uw SQL VM **mysqlvm**, kan een client VM in dezelfde cloudservice de volgende verbindingsreeks gebruiken om verbinding te:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Verbinding maken met SQL Server via Internet

Als u verbinden met uw SQL Server-database-engine van Internet wilt, moet u een virtuele machine-eindpunt voor binnenkomende TCP-communicatie. Deze configuratiestap Azure, wordt u omgeleid zodat binnenkomende TCP-poortverkeer naar een TCP-poorten die toegankelijk is voor de virtuele machine.

Als u wilt verbinding maakt via internet, moet u van de VM DNS-naam en het poortnummer van de VM eindpunt (geconfigureerd verderop in dit artikel). Als u de naam van de DNS-zoekt, navigeer naar de Azure-Portal en selecteer **virtuele machines (klassieke)**. Selecteer uw VM. De **DNS-naam** wordt weergegeven in de sectie **Overzicht** .

Stel een klassieke virtuele machine met de naam **mysqlvm** met een DNS-naam **mysqlvm7777.cloudapp.net** en een eindpunt VM van **57500**. Als er juist geconfigureerde connectiviteit en de volgende verbindingsreeks kan worden gebruikt voor toegang tot de virtuele machine vanaf elke locatie op internet:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Hoewel hierdoor kunnen connectivity voor clients via internet, betekent dit niet dat iemand verbinding met uw SQL-Server maken kunt. Buiten moeten clients u de juiste gebruikersnaam en wachtwoord. Voor extra beveiliging, niet de bekende poort 1433 gebruikt voor de openbare VM-eindpunt. En, indien mogelijk, u kunt desgewenst een ACL toevoegen op uw eindpunt verkeer beperken alleen aan de clients u toestaan. Zie voor instructies over het gebruik van ACL's met eindpunten [beheren de ACL op een eindpunt](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

>[AZURE.NOTE] Het is belangrijk te weten dat wanneer u deze methode gebruiken om te communiceren met SQL Server, alle uitgaande gegevens van het datacenter van de Azure is onderhevig aan normale [prijzen op uitgaande overdracht](https://azure.microsoft.com/pricing/details/data-transfers/).

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Verbinding maken met SQL Server in hetzelfde virtuele netwerk

[Virtuele netwerk](../virtual-network/virtual-networks-overview.md) kunt extra scenario's. U kunt VMs koppelen in hetzelfde virtuele netwerk, zelfs als deze VMs aanwezig zijn in verschillende cloudservices. En met een [site-naar-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md), kunt u een hybride architectuur die VMs maakt een verbinding met on-premises implementatie-netwerken te gebruiken en machines maken.

Virtuele netwerken kunt u uw Azure VMs toevoegen aan een domein. Dit is de enige manier om het Windows-verificatie met SQL Server gebruikt. De andere verbinding-scenario's vereisen SQL-verificatie met gebruikersnamen en wachtwoorden opnieuw instellen.

Als u een domeinomgeving en de Windows-verificatie configureren wilt, moet u doen niet via de stappen in dit artikel de openbare eindpunt of de SQL-verificatie en aanmeldingen configureren. In dit scenario kunt u verbinding met uw SQL Server-instantie door het opgeven van de naam van de SQL Server VM in de verbindingstekenreeks. Het volgende voorbeeld wordt ervan uitgegaan dat ook Windows-verificatie is geconfigureerd en dat de gebruiker heeft toegang tot de SQL Server-instantie.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Stappen voor het configureren van SQL Server-connectiviteit in een VM Azure

De volgende stappen wordt getoond hoe u verbinding maken met de SQL Server-instantie via internet met SQL Server Management Studio (SSMS). Echter de dezelfde stappen gelden voor het maken van uw SQL Server-VM toegankelijk zijn voor uw toepassingen, die beide on-premises implementatie wordt uitgevoerd en in Azure wordt aangegeven.

Voordat u een verbinding met het exemplaar van SQL Server uit een andere VM of op internet maken kunt, moet u de volgende taken uitvoeren, zoals wordt beschreven in de volgende gedeelten:

- [Een eindpunt TCP voor de virtuele machine maken](#create-a-tcp-endpoint-for-the-virtual-machine)
- [TCP-poorten openen in Windows firewall](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [SQL Server op het TCP-protocol luistert configureren](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [SQL Server configureren voor verificatie in gemengde modus](#configure-sql-server-for-mixed-mode-authentication)
- [SQL Server-verificatie aanmeldingen maken](#create-sql-server-authentication-logins)
- [De naam van de DNS van de virtuele machine bepalen](#determine-the-dns-name-of-the-virtual-machine)
- [Verbinding maken met de Database-Engine vanuit een andere computer](#connect-to-the-database-engine-from-another-computer)

Het verbindingspad wordt samengevat met het volgende diagram:

![Verbinding maken met een SQL Server-VM](../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic Steps](../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Volgende stappen

Als u ook van plan bent u AlwaysOn beschikbaarheidsgroepen voor beschikbaarheid en herstel, kunt u overwegen een luisteraar ervan af implementeren. Database-clients verbinding maken op de luisteraar ervan af en niet rechtstreeks op een van de exemplaren van SQL Server. De luisteraar ervan af stuurt clients naar de primaire replica in de beschikbaarheidsgroep. Zie [een ILB luisteraar ervan af voor AlwaysOn beschikbaarheid van groepen in Azure configureren](virtual-machines-windows-classic-ps-sql-int-listener.md)voor meer informatie.

Het is belangrijk om al te controleren van de aanbevolen procedures voor beveiliging voor SQL Server met een Azure virtuele machines. Zie [Beveiligingsoverwegingen voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-security.md)voor meer informatie.

[Verkennen leerpad](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) voor SQL Server op Azure virtuele machines. 

Zie voor andere onderwerpen over met SQL Server in Azure VMs, [SQL Server op Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md).
