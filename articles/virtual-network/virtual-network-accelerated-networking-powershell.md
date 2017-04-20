<properties 
   pageTitle="Versneld toegang voor een virtuele machine - PowerShell | Microsoft Azure"
   description="Informatie over het configureren van versneld toegang voor een Azure virtuele machines via PowerShell."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Versneld netwerken voor een virtuele machine

> [AZURE.SELECTOR]
- [Azure-Portal](virtual-network-accelerated-networking-portal.md)
- [PowerShell](virtual-network-accelerated-networking-powershell.md)

Versneld netwerken kunt één hoofdsite i/o-Virtualization (SR-IOV) aan een virtuele machine (VM), de netwerkprestaties aanzienlijk wordt verbeterd. Dit pad krachtige omzeilt de host van het verkleinen van latentie, jitter en CPU-gebruik voor gebruik met de meest veeleisende werkbelasting in het netwerk op ondersteunde VM typen gegevenspad. In dit artikel wordt uitgelegd hoe Azure PowerShell gebruiken voor het configureren van versneld toegang in het implementatiemodel resourcemanager Azure. U kunt ook een VM maken met versneld toegang met behulp van de Azure-Portal. Voor meer informatie over hoe, klik in het vak Azure-Portal boven aan dit artikel.

De volgende afbeelding ziet de communicatie tussen twee virtuele machines (VM) met en zonder versneld toegang:

![Vergelijking](./media/virtual-network-accelerated-networking-powershell/image1.png)

Alle netwerkverkeer en afmelden bij de VM moet zonder versneld netwerken via de host en de schakeloptie voor virtual. De virtuele schakeloptie bevat alle afdwingen, zoals netwerk beveiligingsgroepen, toegangscontrolelijsten, moeten worden geïsoleerd en andere netwerkservices gevirtualiseerde op netwerkverkeer. Lees meer informatie het artikel [Hyper-V netwerk virtualisatie en Virtual Switch](https://technet.microsoft.com/library/jj945275.aspx) .

Met versneld toegang, netwerkverkeer binnenkomt op de netwerkkaart (NIC) en vervolgens wordt doorgestuurd naar de VM. Alle netwerkbeleidsregels waarmee de virtuele schakeloptie van toepassing zonder versneld netwerkproblemen worden overgenomen en toegepast in hardware. Een beleid in hardware toepast, kunt de NIC op doorsturen netwerkverkeer rechtstreeks naar de VM, de host en de schakeloptie virtuele behoud van alle het beleid dat deze in de host toegepast niet kan overslaan.

De voordelen van versneld toegang zijn alleen van toepassing op de VM die is ingeschakeld. Voor de beste resultaten is dit ideaal voor deze functie op ten minste twee VMs verbonden met de dezelfde VNet.  Tijdens het communiceren via VNets of verbindende on-premises, is deze functie een minimale invloed op algemene latentie.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Voordelen

- **Lagere latentie / hoger pakketten per seconde (pps):** De virtuele schakeloptie verwijderen uit de datapath, verwijdert de tijd pakketten besteden in de host voor de beleidsverwerking van het en het aantal pakketten dat kan worden verwerkt binnen de VM verhogen.
- **Beperkte jitter:** Verwerking van virtuele veranderen, is afhankelijk van de hoeveelheid beleid dat moet worden toegepast en de werklast van de CPU dat de verwerking doet. Het beleid afdwingen aan de hardware offloading Hiermee verwijdert u die variaties door pakketten rechtstreeks aan de VM, verwijderen van de host VM communicatie en alle software interrupts en schakelopties context te bieden.
- **Afgenomen CPU-gebruik:** De virtuele schakeloptie in de host overslaan leidt tot minder CPU-gebruik voor het verwerken van netwerkverkeer.

## <a name="limitations"></a>Beperkingen

De volgende beperkingen bestaat bij gebruik van deze mogelijkheid:
 
- **Netwerk interface maken:** Versneld netwerken kan alleen worden ingeschakeld voor een nieuwe netwerkinterface.  Dit kan niet worden ingeschakeld op een bestaande netwerkinterface.
- **VM maken:** De netwerkinterface van een met versneld netwerken die zijn ingeschakeld, kan alleen worden gekoppeld aan een VM wanneer de VM wordt gemaakt. De netwerkinterface niet kan worden gekoppeld aan een bestaande VM.
- **Regio's:** In de West Centraal ons West Europe Azure regio's en alleen wordt aangeboden. De set regio's wordt in de toekomst uitgebreid.
- **Ondersteunde-besturingssysteem:** Microsoft Windows Server 2012 R2 en Windows Server 2016 technische bètaversie van 5. Ondersteuning voor Linux en Windows Server 2012 worden, snel toegevoegd.
- **VM grootte:** Standard_D15_v2 en Standard_DS15_v2 zijn dat de enige ondersteunde VM exemplaar grootte. Zie het artikel [Windows VM grootten](../virtual-machines/virtual-machines-windows-sizes.md) voor meer informatie. De set ondersteunde VM exemplaar grootte wordt in de toekomst uitgebreid.

Wijzigingen in deze beperkingen wordt aangekondigd via de pagina [updates Azure virtuele netwerkproblemen](https://azure.microsoft.com/updates/accelerated-networking-in-preview) .

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Een Windows VM met versneld netwerken maken

1. Open een opdrachtprompt PowerShell en vul de overige stappen in deze sectie binnen een enkel PowerShell-sessie. Als u nog niet PowerShell geïnstalleerd en geconfigureerd, voert u de stappen in het artikel [installeren en configureren van Azure PowerShell](../powershell-install-configure.md) .
2. Om u te registreren voor de Preview-versie, kunt u een e-mail verzenden naar [Netwerkproblemen abonnementen versneld](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) met uw abonnements-ID en gebruik bedoeld. Voer de overige stappen tot niet nadat u een e-mailbericht dat u hebt geaccepteerd in de Preview-versie hebt ontvangen.
3. De mogelijkheid met uw abonnement registreren met het invoeren van de volgende opdrachten:

        Register-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingFeature -ProviderNamespace Microsoft.Network
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network

4. *Westcentralus* vervangen door de naam van een andere locatie wordt ondersteund door deze mogelijkheid weergegeven in de sectie [beperkingen](#limitations) van dit artikel (indien gewenst). Voer de volgende opdracht voor het instellen van een variabele voor de locatie:

        $locName = "westcentralus"

5. *RG1* vervangen door een naam voor de resourcegroep die het nieuwe netwerkinterface bevatten en voer de volgende opdrachten moet maken:

        $rgName = "RG1"
        New-AzureRmResourceGroup -Name $rgName -Location $locName

6. Wijzig *VM1-NIC1* wat u wilt naam voor de netwerkinterface en voer de volgende opdracht uit:

        $NICName = "VM1-NIC1"

7. De netwerkinterface moet zijn verbonden met een subnet binnen een bestaand Azure virtuele netwerk (VNet) in de locatie en het [abonnement](../azure-glossary-cloud-terminology.md#subscription) als de netwerkinterface. Meer informatie over VNets Lees het artikel [virtuele netwerk overzicht](virtual-networks-overview.md) als u niet bekend met deze bent. Als u wilt een VNet hebt gemaakt, voert u de stappen in het artikel [een VNet maken](virtual-networks-create-vnet-arm-ps.md) . Wijzig de "waarden" van de volgende $Variables op de naam van de VNet en subnetten die u wilt de netwerkinterface om te koppelen.

        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"

    Als u niet dat de naam van een bestaande VNet op de locatie die u in stap 3 hebt gekozen weet, voert u de volgende opdrachten:
        
        $VirtualNetworks = Get-AzureRmVirtualNetwork
        $VirtualNetworks | Where-Object {$_.Location -eq $locName} | Format-Table Name, Location
        
    Als de lijst als resultaat gegeven leeg is, moet u een VNet maken op de locatie. Als u wilt een VNet hebt gemaakt, voert u de stappen in het artikel [een virtueel netwerk maken](virtual-networks-create-vnet-arm-ps.md) .

    Als u de naam van de subnetten binnen de VNet, typt u de volgende opdrachten en *Subnet1* hierboven vervangen door de naam van een subnet:
        
        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

8. Voer de volgende opdrachten voor het ophalen van de VNet en subnet en toewijzen aan variabelen.

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

9. Geef aan dat een bestaande openbare IP-adres resource die gekoppeld aan de netwerkinterface worden kan zodat u verbinding met deze via Internet maken kunt. Als u niet dat de VM met het netwerkinterface openen via Internet wilt, kunt u deze stap overslaan. Zonder een openbare IP-adres, moet u verbinding met de VM uit een andere VM verbonden met de dezelfde VNet. 

    *PIP1* op de naam van een bestaande openbare IP-adres resource die zich in de locatie als u de netwerkinterface in en die is gekoppeld aan een andere netwerkinterface niet wijzigen. Wijzig zo nodig $rgName op de naam van de resourcegroep het openbare IP-adres resource bestaat in en voer de volgende opdracht uit:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $rgName

    Als u de naam van een bestaande bron van openbare IP-adres niet weet, voert u de volgende opdrachten:

        $PublicIPAddresses = Get-AzureRmPublicIPAddress
        $PublicIPAddresses | Where-Object { $_.Location -eq $locName } |Format-Table Name, Location, IPAddress, IpConfiguration

    Als de kolom **IPConfiguration** geen waarde in de uitvoer geretourneerd heeft, wordt de bron van het openbare IP-adres is niet gekoppeld aan een bestaande netwerkinterface en kan worden gebruikt. Als de lijst leeg is, of er geen resources beschikbaar openbare IP-adres zijn, kunt u een met de opdracht Nieuw AzureRmPublicIPAddress maken.

    >[AZURE.NOTE] Openbare IP-adressen hebben een nominale kosten. Meer informatie over de [prijzen van IP-adres van](https://azure.microsoft.com/pricing/details/ip-addresses) de pagina prijzen.
10. Als u ervoor kiest niet naar een openbare IP-adres resource toevoegen aan de interface, verwijdert u *- PublicIPAddress $PIP1* aan het einde van de opdracht die volgt op. De netwerkinterface met versneld netwerken maken door de volgende opdracht uit te voeren:

        $nic = New-AzureRmNetworkInterface -Location $locName -Name $NICName -ResourceGroupName $rgName -Subnet $Subnet -EnableAcceleratedNetworking -PublicIpAddress $PIP1 

11. Toewijzen aan een VM de netwerkinterface bij het maken van de VM volgens de instructies in stap 3 en 6 van het artikel [een VM maken](../virtual-machines/virtual-machines-windows-ps-create.md) . Vervang in stap 6-2, *Standard_A1* met een van de VM formaten weergegeven in de sectie [beperkingen](#limitations) van dit artikel.

    >[AZURE.NOTE] Als u de *naam* van de $locName, $rgName of $nic variabelen in dit artikel hebt gewijzigd, mislukt de volgende stap 6 in het maken van een artikel VM. U kunt wel de *waarden* van de variabelen wijzigen.

12. Nadat de VM is gemaakt, download het [stuurprogramma versneld netwerkproblemen](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84), verbinding maken met de VM en voert u het installatieprogramma stuurprogramma binnen de VM.

13. Met de rechtermuisknop op de Windows-knop en klik op **Apparaatbeheer**. Controleer of dat de **Mellanox ConnectX-3 virtuele functie Ethernet-Adapter** wordt weergegeven onder de optie **netwerk** nadat dit is vergroot, zoals wordt weergegeven in de volgende afbeelding:

    ![Apparaatbeheer](./media/virtual-network-accelerated-networking-powershell/image2.png)