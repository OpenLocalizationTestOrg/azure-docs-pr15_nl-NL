<properties
   pageTitle="Mutiple VIP's voor een cloudservice"
   description="Overzicht van multiVIP en het instellen van meerdere VIP's op een cloudservice"
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

# <a name="configure-multiple-vips-for-a-cloud-service"></a>Meerdere VIP's voor een cloudservice configureren

U kunt Azure cloudservices openen via de openbare Internet met behulp van een IP-adres dat is verstrekt door Azure. Dit openbare IP-adres is een VIP (virtual IP) genoemd omdat deze is gekoppeld aan de Azure taakverdeling en niet de virtuele-Machine (VM) in de cloudservice exemplaren. U kunt een willekeurig exemplaar VM binnen een cloudservice openen met behulp van een enkel VIP.

Er zijn echter scenario's waarin u mogelijk moet u meer dan één VIP als een vermelding wijst u de dezelfde cloudservice. Uw cloudservice kan bijvoorbeeld meerdere websites die vereisen SSL-connectiviteit met de standaardpoort 443, zoals elke site wordt gehost voor een andere klant of tenant hosten. In dit scenario moet u beschikken over een ander openbare omlaag IP-adres voor elke website. Het onderstaande diagram ziet u een typisch met meerdere tenant webhosting voor meerdere SSL-certificaten op dezelfde openbare poort.

![Meerdere VIP SSL-scenario](./media/load-balancer-multivip/Figure1.png)

In het bovenstaande voorbeeld alle VIP's dezelfde openbare poort (443) gebruiken en verkeer omgeleid naar een of meer laden verdeeld VMs op een unieke privé poort voor het interne IP-adres van de cloudservice die als host fungeert voor alle websites.

>[AZURE.NOTE] Een andere situatie verplichten de meerdere VIP's is meerdere SQL AlwaysOn beschikbaarheid van de groep listeners op dezelfde reeks virtuele Machines hosten.

VIP's zijn standaard dynamisch, wat betekent dat het werkelijke IP-adres dat is toegewezen aan de cloudservice na verloop van tijd kan worden gewijzigd. Als u wilt voorkomen, kunt u een VIP reserveren voor uw service. Meer informatie over gereserveerde VIP's, Zie [Gereserveerde openbare IP](../virtual-network/virtual-networks-reserved-public-ip.md).

>[AZURE.NOTE] Zie [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses/) voor informatie over de prijzen van VIP's en gereserveerde IP-adressen.

U kunt PowerShell gebruiken om te controleren of de VIP's die worden gebruikt door uw cloudservices, evenals toevoegen en verwijderen van VIP's, een VIP naar een eindpunt koppelen en configureren van taakverdeling op een specifieke VIP.

## <a name="limitations"></a>Beperkingen

Op dit moment kan is meerdere VIP-functionaliteit beperkt tot de volgende scenario's:

- **Alleen IaaS**. U kunt meerdere VIP alleen inschakelen voor cloudservices die VMs bevatten. U kunt meerdere VIP niet gebruiken in PaaS scenario's met exemplaren van de rol.
- **Alleen PowerShell**. U kunt meerdere VIP alleen beheren via PowerShell.

Deze beperkingen zijn tijdelijke en op elk gewenst moment kunnen worden gewijzigd. Zorg ervoor dat u terugkeren naar deze pagina om toekomstige wijzigingen te controleren.


## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Het toevoegen van een VIP naar een cloudservice

Als u wilt een VIP aan uw service hebt toegevoegd, voert u de volgende PowerShell-opdracht:

    Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

Deze opdracht is vergelijkbaar met het onderstaande voorbeeld resultaat weergegeven:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>Hoe een VIP verwijdert uit een cloudservice

Als u wilt verwijderen de VIP toegevoegd aan uw service in het bovenstaande voorbeeld, voert u de volgende PowerShell-opdracht:

    Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

>[AZURE.IMPORTANT] U kunt alleen een VIP verwijderen als er geen eindpunten die is gekoppeld aan.

## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Hoe VIP gegevens ophalen uit een Cloudservice

Als u wilt ophalen de VIP's die zijn gekoppeld aan een cloudservice, voert u het volgende PowerShell-script:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Het script vergelijkbaar met het onderstaande voorbeeld resultaat wordt weergegeven:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

In dit voorbeeld is de cloudservice 3 VIP's:

- **Vip1** is de standaard-VIP, u weet dat omdat de waarde voor IsDnsProgrammedName is ingesteld op waar.
- **Vip2** en **Vip3** niet worden gebruikt als er geen IP-adressen. Ze worden alleen worden gebruikt als u een eindpunt naar de VIP koppelen.

>[AZURE.NOTE] Uw abonnement wordt alleen btw geheven voor extra VIP's nadat ze gekoppeld aan een eindpunt zijn. Zie [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses/)voor meer informatie over prijzen.

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Hoe u kunt een VIP naar een eindpunt koppelen

Als u wilt een VIP op een cloudservice om een eindpunt te koppelen, voer de volgende PowerShell-opdracht:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 `
  	| Update-AzureVM

De opdracht Hiermee maakt u een eindpunt dat is gekoppeld aan de VIP *Vip2* op poort *80*en deze naar de VM *myVM1* in een cloudservice met de naam *MijnService* met *TCP* op poort *8080*benoemde koppelingen genoemd.

Als u wilt controleren of de configuratie, voert u de volgende PowerShell-opdracht:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

De uitvoer ziet er ongeveer als volgt:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Het inschakelen van taakverdeling op een specifieke VIP

U kunt een enkel VIP koppelen aan meerdere virtuele machines voor doeleinden van taakverdeling. Stel, hebt u een cloudservice met de naam *MijnService*en twee virtuele machines met de naam *myVM1* en *myVM2*. En uw cloudservice heeft meerdere VIP's, een van deze met de naam *Vip2*. Als u wilt Zorg ervoor dat alle verkeer naar poort *81* op *Vip2* , wordt verdeeld tussen *myVM1* en *myVM2* op poort *8181*, voer het volgende PowerShell-script:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

    Get-AzureVM -ServiceName myService -Name myVM2 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

U kunt ook uw taakverdeling als u wilt gebruiken, een andere VIP bijwerken. Als u de onderstaande PowerShell-opdracht uitvoert, wordt u bijvoorbeeld de instellen voor het gebruik van een VIP Vip1 met de naam van taakverdeling wijzigen:

    Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1

## <a name="next-steps"></a>Volgende stappen

[Log analytics voor taakverdeling van Azure](load-balancer-monitor-log.md)

[Internet omlaag laden de verdeling van overzicht](load-balancer-internet-overview.md)

[Aan de slag op Internet die tegenover elkaar liggen taakverdeling](load-balancer-get-started-internet-arm-ps.md)

[Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)

[Gereserveerde IP-REST API 's](https://msdn.microsoft.com/library/azure/dn722420.aspx)