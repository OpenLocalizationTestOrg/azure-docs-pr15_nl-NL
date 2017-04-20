<properties
   pageTitle="Aan de slag voor het maken van een internetverbinding van taakverdeling in klassieke implementatiemodel met de CLI Azure | Microsoft Azure"
   description="Informatie over het maken van een internetverbinding van taakverdeling in het gebruik van de Azure CLI klassieke implementatiemodel"
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

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-cli"></a>Maken van een internetverbinding van taakverdeling (classic) in de CLI Azure aan de slag

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel heeft betrekking op het implementatiemodel klassieke. U kunt ook [informatie over het maken van taakverdeling met Azure Resource Manager van een internetverbinding](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a>Stap voor stap maken van een die tegenover elkaar liggen taakverdeling CLI met Internet

Deze handleiding ziet hoe u een Internet taakverdeling op basis van de bovenstaande scenario maken.

1. Als u nooit Azure CLI gebruikt nog, Zie [installeren en configureren van de Azure CLI](../../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en een abonnement selecteren.

2. Voer de opdracht **azure config modus** overschakelen naar klassieke modus, zoals hieronder wordt weergegeven.

        azure config mode asm

    Verwachte output:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Eindpunt maken en het laden van de verdeling van instellen

Het scenario wordt ervan uitgegaan van de virtuele machines "web1" en "web2" zijn gemaakt.
Deze handleiding maakt een laden de verdeling van set met poort 80 als openbare poort en poort 80 als lokale poort. Een test poort is ook geconfigureerd op poort 80 en is de belasting voor gelijkmatige verdeling instellen "lbset" met de naam.


### <a name="step-1"></a>Stap 1

Het eerste eindpunt maken en de belasting voor documentconversies set met `azure network vm endpoint create` voor virtuele machine "web1".

    azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset

Parameters die worden gebruikt:

**-k** - lokale VM poort<br>
**-o** - protocol<BR>
**-t** - test poort<BR>
**-b** - naam voor de verdeling van laden<BR>

## <a name="step-2"></a>Stap 2

Een tweede virtuele machine 'web2' toevoegen aan het instellen van de verdeling van laden.

    azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## <a name="step-3"></a>Stap 3

Ga na of de belasting voor gelijkmatige verdeling configuratie `azure vm show` .

    azure vm show web1

De uitvoer is:

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Een extern bureaublad eindpunt voor een virtuele machine maken

U kunt een extern bureaublad eindpunt netwerkverkeer van een openbare poort doorsturen naar een lokale poort voor het gebruik van een specifieke virtuele machine maken `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>VM van taakverdeling verwijderen

U moet verwijderen van het eindpunt dat is gekoppeld aan de verdeling van de belasting van de virtuele machine instellen. Zodra het eindpunt is verwijderd, de virtuele machine geen deel uitmaakt van de verdeling van de belasting meer instellen.

 Het bovenstaande voorbeeld gebruikt, kunt u het eindpunt dat is gemaakt voor VM "web1" verwijderen van taakverdeling "lbset" met de opdracht `azure vm endpoint delete`.

    azure vm endpoint delete web1 tcp-80-80


>[AZURE.NOTE] U kunt meer opties voor het beheren van de eindpunten met de opdracht verkennen`azure vm endpoint --help`


## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een modus voor het laden gelijkmatige verdeling configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)

