<properties
   pageTitle="Taakverdeling voor SQL altijd configureren op | Microsoft Azure"
   description="Configureren van taakverdeling voor gebruik met SQL altijd op en hoe u gebruikmaken van powershell als u wilt maken van taakverdeling voor de SQL-implementatie"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-load-balancer-for-sql-always-on"></a>Taakverdeling voor SQL altijd configureren op

SQL Server AlwaysOn beschikbaarheidsgroepen kan nu worden uitgevoerd met ILB. Beschikbaarheid van de groep is van SQL Server vlaggenschip oplossing voor hoge beschikbaarheid herstel. De beschikbaarheid van de groep luisteraar ervan af clienttoepassingen in staat stelt naadloos verbinding maken met de primaire replica, ongeacht het aantal de replica's in de configuratie.

De naam van de luisteraar ervan af (DNS) is toegewezen aan een IP-adres van taakverdeling en Azure van taakverdeling zorgt ervoor dat de binnenkomende verkeer naar alleen de primaire server in de replicaset.

U kunt ILB ondersteuning voor SQL Server AlwaysOn (luisteraar ervan af) eindpunten. U nu controle over de toegankelijkheid van de luisteraar ervan af hebt en het IP-adres van taakverdeling kunt kiezen uit een specifiek subnet in uw virtuele netwerk (VNet).

Met behulp van ILB op de luisteraar ervan af, het eindpunt van SQL server (zoals Server = tcp:ListenerName, 1433; Database = databasenaam) alleen toegankelijk is:

- Services en VMs in hetzelfde virtuele netwerk
- Services en VMs van verbonden on-premises netwerk
- Services en VMs uit met onderling verbonden VNets

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Afbeelding 1 - SQL AlwaysOn geconfigureerd met internetgerichte taakverdeling

## <a name="add-internal-load-balancer-to-the-service"></a>Interne taakverdeling toevoegen aan de service

1. In het volgende voorbeeld configureert we een virtuele netwerk met een subnet 'Subnet-1' genoemd:

        Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc

2. Het toevoegen van taakverdeling eindpunten voor ILB op elke VM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –
        DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    In het bovenstaande voorbeeld, hebt u de 2 VM genoemd 'sqlsvc1' en 'sqlsvc2' voorlopig service 'Sqlsvc' in de cloud. Na het maken van de ILB met "DirectServerReturn" veranderen, kunt u taakverdeling eindpunten toevoegen aan de ILB toe te staan dat SQL voor het configureren van de listeners voor de van de beschikbaarheidsgroepen.

Zie voor meer informatie over SQL AlwaysOn [configureren een interne taakverdeling voor een groep AlwaysOn beschikbaarheid in Azure wordt aangegeven](../virtual-machines/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Zie ook

[Aan de slag een Internet die tegenover elkaar liggen taakverdeling configureren](load-balancer-get-started-internet-arm-ps.md)

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een modus voor het laden gelijkmatige verdeling configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
