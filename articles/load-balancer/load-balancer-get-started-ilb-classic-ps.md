<properties
   pageTitle="Maken van een interne taakverdeling via PowerShell in het implementatiemodel klassieke | Microsoft Azure"
   description="Informatie over het maken van een interne taakverdeling via PowerShell in het implementatiemodel klassieke"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a>Maken van een interne taakverdeling (klassieke) via PowerShell aan de slag

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe [u deze stappen uitvoert met het model resourcemanager](load-balancer-get-started-ilb-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a>Een interne taakverdeling instellen voor virtuele machines maken

Maken van een set van de verdeling van interne laden en de servers die hun verkeer naartoe worden verzonden, moet u het volgende doen:

1. Maak een exemplaar van interne laden verdelen die het eindpunt van de binnenkomende verkeer moeten gelijkmatig verdeeld over de servers van een set verdeeld.

1. Voeg de eindpunten overeenkomt met de virtuele machines die de binnenkomende verkeer ontvangt.

1. Configureer de servers die Stuur het verkeer naar verdeeld om te verzenden van hun verkeer naar het virtuele IP-(VIP)-adres van het exemplaar interne laden verdelen.


### <a name="step-1-create-an-internal-load-balancing-instance"></a>Stap 1: Maak een exemplaar interne taakverdeling

Voor een bestaande cloudservice of een cloudservice geïmplementeerd onder een regionale virtueel netwerk, kunt u een exemplaar interne taakverdeling maken met de volgende Windows PowerShell-opdrachten:

    $svc="<Cloud Service Name>"
    $ilb="<Name of your ILB instance>"
    $subnet="<Name of the subnet within your virtual network>"
    $IP="<The IPv4 address to use on the subnet-optional>"

    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP


Houd er rekening mee dat deze gebruik van de [Toevoegen-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell-cmdlet wordt gebruikt voor het instellen van de parameter DefaultProbe. Zie voor meer informatie over extra parametersets, [Toevoegen-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).

### <a name="step-2-add-endpoints-to-the-internal-load-balancing-instance"></a>Stap 2: Eindpunten naar het interne taakverdeling exemplaar toevoegen

Hier volgt een voorbeeld:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $ilb="ilbset"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


### <a name="step-3-configure-your-servers-to-send-their-traffic-to-the-new-internal-load-balancing-endpoint"></a>Stap 3: Configureer de servers voor hun verkeer naar het nieuwe interne taakverdeling eindpunt verzenden

U moet de servers waarvan u het verkeer dient te zijn van taakverdeling gebruik het nieuwe IP-adres (de VIP) van het exemplaar interne laden verdelen configureren. Dit is het adres waarop het exemplaar interne taakverdeling luistert. In de meeste gevallen moet u net toevoegen of wijzigen van een DNS-record voor de VIP van het exemplaar interne taakverdeling.

Als u het IP-adres tijdens het maken van het exemplaar interne taakverdeling opgegeven, hebt u al de VIP. Anders ziet u de VIP uit de volgende opdrachten:

    $svc="<Cloud Service Name>"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer



Als u wilt deze opdrachten gebruiken, de waarden invullen en verwijder de < en >. Hier volgt een voorbeeld:

    $svc="mytestcloud"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer


Houd rekening met het IP-adres van de weergave van de opdracht Get-AzureInternalLoadBalancer, en breng de gewenste wijzigingen aan uw servers of de DNS-records om ervoor te zorgen dat verkeer wordt verstuurd naar de VIP.

>[AZURE.NOTE]De Microsoft Azure-platform gebruikt een statische, openbaar geschikt IPv4-adres voor een groot aantal administratieve scenario's. Het IP-adres is 168.63.129.16. Dit IP-adres moet niet worden geblokkeerd door een firewalls, omdat dit onverwachte gevolgen dit kan.
>Dit IP-adres wordt door sondes uit de taakverdeling controleren met betrekking tot Azure interne taakverdeling gebruikt om te bepalen de status voor virtuele machines in een set van taakverdeling. Als een netwerk-beveiligingsgroep wordt gebruikt om te verkeer beperken tot Azure virtuele machines in een set intern taakverdeling of wordt toegepast op een virtuele netwerk Subnet, moet u ervoor zorgen dat een regel voor netwerk is toegevoegd aan dat verkeer is toegestaan uit 168.63.129.16.


## <a name="example-of-internal-load-balancing"></a>Voorbeeld van taakverdeling interne

Als u wilt u stapsgewijs het proces van einde naar einde van het maken van een set taakverdeling voor twee voorbeeldconfiguraties, raadpleegt u de volgende secties.

### <a name="an-internet-facing-multi-tier-application"></a>Een internetverbinding, met meerdere niveaus-toepassing

U wilt bieden een service van de database taakverdeling voor een reeks internetgerichte-endwebservers. Beide sets met servers worden gehost in een enkel Azure-cloudservice. Web serververkeer met TCP-poort 1433 moet worden verdeeld over twee virtuele machines in de databaselaag. Afbeelding 1 ziet u de configuratie.

![Interne taakverdeling instellen voor de databaselaag](./media/load-balancer-internal-getstarted/IC736321.png)


De configuratie bestaat van de volgende opties:

- De bestaande cloudservice host voor de virtuele machines heet mytestcloud.

- De twee bestaande databaseservers heten BW1, DB2.

- De database-servers in de databaselaag endwebservers in de weblaag automatisch verbinden met behulp van het persoonlijke IP-adres. Er is een andere optie voor het virtuele netwerk van uw eigen DNS-en handmatig registreren een A-record voor het instellen van de verdeling van interne laden.

De volgende opdrachten configureren van een nieuw exemplaar interne taakverdeling met de naam **ILBset** en eindpunten toevoegen aan de virtuele machines die overeenkomt met de twee databaseservers:

    $svc="mytestcloud"
    $ilb="ilbset"
    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $vmname="DB1"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="TCP-1433-1433-2"
    $vmname="DB2"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


## <a name="remove-an-internal-load-balancing-configuration"></a>Een interne taakverdeling configuratie verwijderen

Als u wilt een virtuele machine als een eindpunt verwijderen uit een exemplaar van de verdeling van interne laden, gebruikt u de volgende opdrachten:

    $svc="<Cloud service name>"
    $vmname="<Name of the VM>"
    $epname="<Name of the endpoint>"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Vul de waarden, verwijderen als u wilt deze opdrachten gebruiken, de < en >.

Hier volgt een voorbeeld:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Als u wilt een exemplaar van de verdeling van interne laden verwijderen uit een cloudservice, kunt u de volgende opdrachten gebruiken:

    $svc="<Cloud service name>"
    Remove-AzureInternalLoadBalancer -ServiceName $svc

Als u deze opdrachten, voer de waarde en verwijder de < en >.

Hier volgt een voorbeeld:

    $svc="mytestcloud"
    Remove-AzureInternalLoadBalancer -ServiceName $svc



## <a name="additional-information-about-internal-load-balancer-cmdlets"></a>Meer informatie over cmdlets voor de verdeling van interne laden


Als u meer informatie over cmdlets interne taakverdeling, voert u de volgende opdrachten bij de opdrachtprompt van Windows PowerShell:

- Nieuwe AzureInternalLoadBalancerConfig get-help-volledige

- Help opvragen toevoegen AzureInternalLoadBalancer-volledige

- Help opvragen Get-AzureInternalLoadbalancer-volledige

- Help opvragen verwijderen-AzureInternalLoadBalancer-volledige

## <a name="next-steps"></a>Volgende stappen

[Een laden gelijkmatige verdeling modus bron IP-affiniteit met configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)