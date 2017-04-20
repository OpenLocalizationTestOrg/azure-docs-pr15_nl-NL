<properties
   pageTitle="Maken van een interne taakverdeling de CLI Azure gebruiken in het implementatiemodel klassieke | Microsoft Azure"
   description="Informatie over het maken van een interne taakverdeling de CLI Azure gebruiken in het implementatiemodel klassieke"
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

# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a>Een interne taakverdeling (classic) gebruik van de Azure CLI maken aan de slag

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe [u deze stappen uitvoert met het model resourcemanager](load-balancer-get-started-ilb-arm-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a>Als u wilt maken van een interne taakverdeling instellen voor virtuele machines

Als u wilt maken van een set van de verdeling van interne laden en de servers die hun verkeer naartoe worden verzonden, moet u het volgende doen:

1. Maak een exemplaar van interne laden verdelen die het eindpunt van de binnenkomende verkeer moeten gelijkmatig verdeeld over de servers van een set verdeeld.

1. Voeg de eindpunten overeenkomt met de virtuele machines die de binnenkomende verkeer ontvangt.

1. Configureer de servers die Stuur het verkeer naar verdeeld om te verzenden van hun verkeer naar het virtuele IP-(VIP)-adres van het exemplaar interne laden verdelen.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Stap voor stap maken van een interne taakverdeling CLI gebruiken

Deze handleiding ziet hoe u een interne taakverdeling op basis van de bovenstaande scenario maken.

1. Als u nooit Azure CLI gebruikt nog, Zie [installeren en configureren van de Azure CLI](../../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en een abonnement selecteren.

2. Voer de opdracht **azure config modus** overschakelen naar klassieke modus, zoals hieronder wordt weergegeven.

        azure config mode asm

    Verwachte output:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Eindpunt maken en het laden van de verdeling van instellen

Het scenario wordt ervan uitgegaan van de virtuele machines "BW1" en 'DB2' in een cloudservice 'mytestcloud' genoemd. Beide virtuele machines gebruikt een virtueel netwerk mijn "testvnet" met subnet 'subnet-1' genoemd.

Deze handleiding maakt een interne laden de verdeling van set met poort 1433 als privé poort en 1433 als lokale poort.

Dit is een algemene scenario waar u SQL virtuele machines hebt op de back-end een interne taakverdeling gebruiken om u te garanderen dat de databaseservers won't worden blootgesteld rechtstreeks met een openbare IP-adres.


### <a name="step-1"></a>Stap 1

Maken van een interne taakverdeling set met `azure network service internal-load-balancer add`.

     azure service internal-load-balancer add -r mytestcloud -n ilbset -t subnet-1 -a 192.168.2.7

Parameters die worden gebruikt:

**-r** - cloud servicenaam<BR>
**-n** - de naam van de verdeling van interne laden<BR>
**-t** - subnetnaam (hetzelfde subnet door de virtuele machines die u aan de interne taakverdeling wordt toegevoegd)<BR>
**-een** - (optioneel) een statische privé IP-adres toevoegen<BR>

Bekijk `azure service internal-load-balancer --help` voor meer informatie.

U kunt controleren met de opdracht Eigenschappen voor de verdeling van het interne laden `azure service internal-load-balancer list` *servicenaam cloud*.

Hier volgt een voorbeeld van de uitvoer:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


## <a name="step-2"></a>Stap 2

U kunt het instellen van de verdeling van interne laden configureren wanneer u het eerste eindpunt toevoegt. U kunt het eindpunt, VM en test poort met de set van de verdeling van interne laden in deze stap wilt koppelen.

    azure vm endpoint create db1 1433 -k 1433 tcp -t 1433 -r tcp -e 300 -f 600 -i ilbset

Parameters die worden gebruikt:

**-k** - lokale VM poort<BR>
**-t** - test poort<BR>
**-r** - test protocol<BR>
**-e** - test interval in seconden<BR>
**-f** - time-outinterval in seconden <BR>
**-i** - de naam van de verdeling van interne laden <BR>


## <a name="step-3"></a>Stap 3

Ga na of de belasting voor gelijkmatige verdeling configuratie `azure vm show` *VM naam*

    azure vm show DB1

De uitvoer is:

        azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK


## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Een extern bureaublad eindpunt voor een virtuele machine maken

U kunt een extern bureaublad eindpunt netwerkverkeer van een openbare poort doorsturen naar een lokale poort voor het gebruik van een specifieke virtuele machine maken `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>VM van taakverdeling verwijderen

U kunt een virtuele machine verwijderen uit een interne taakverdeling ingesteld door het bijbehorende eindpunt verwijderen. Zodra het eindpunt is verwijderd, de virtuele machine won't deel uitmaakt van de verdeling van de belasting meer instellen.

 Het bovenstaande voorbeeld gebruikt, kunt u het eindpunt dat is gemaakt voor VM "BW1" van interne taakverdeling "ilbset" met de opdracht verwijderen `azure vm endpoint delete`.

    azure vm endpoint delete DB1 tcp-1433-1433


Bekijk `azure vm endpoint --help` voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

[Een laden gelijkmatige verdeling modus bron IP-affiniteit met configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)