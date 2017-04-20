<properties 
   pageTitle="Versneld toegang voor een virtuele machine - Portal | Microsoft Azure"
   description="Leer hoe u versneld netwerkproblemen configureren voor een Azure virtuele machine met behulp van de Azure-Portal."
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
   ms.date="09/26/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Versneld netwerken voor een virtuele machine

> [AZURE.SELECTOR]
- [Azure-Portal](virtual-network-accelerated-networking-portal.md)
- [PowerShell](virtual-network-accelerated-networking-powershell.md)

Versneld netwerken kunt één hoofdsite i/o-Virtualization (SR-IOV) aan een virtuele machine (VM), de netwerkprestaties aanzienlijk wordt verbeterd. Dit pad krachtige omzeilt de host van het verkleinen van latentie, jitter en CPU-gebruik voor gebruik met de meest veeleisende werkbelasting in het netwerk op ondersteunde VM typen gegevenspad. In dit artikel wordt uitgelegd hoe u met de Portal Azure versneld netwerkproblemen configureren in het implementatiemodel resourcemanager Azure. U kunt ook een VM maken met versneld toegang via Azure PowerShell. Voor meer informatie over hoe, klik in het vak PowerShell aan de bovenkant van dit artikel.

De volgende afbeelding ziet de communicatie tussen twee virtuele machines (VM) met en zonder versneld toegang:

![Vergelijking](./media/virtual-network-accelerated-networking-portal/image1.png)

Alle netwerkverkeer en afmelden bij de VM moet zonder versneld netwerken via de host en de schakeloptie voor virtual. De virtuele schakeloptie bevat alle afdwingen, zoals netwerk beveiligingsgroepen, toegangscontrolelijsten, moeten worden geïsoleerd en andere netwerkservices gevirtualiseerde op netwerkverkeer. Lees meer informatie het artikel [Hyper-V netwerk virtualisatie en Virtual Switch](https://technet.microsoft.com/library/jj945275.aspx) .

Met versneld toegang, netwerkverkeer binnenkomt op de netwerkkaart (NIC) en vervolgens wordt doorgestuurd naar de VM. Alle netwerkbeleidsregels waarmee de virtuele schakeloptie van toepassing zonder versneld netwerkproblemen worden overgenomen en toegepast in hardware. Een beleid in hardware toepast, kunt de NIC op doorsturen netwerkverkeer rechtstreeks naar de VM, de host en de schakeloptie virtuele behoud van alle het beleid dat deze in de host toegepast niet kan overslaan.

De voordelen van versneld toegang zijn alleen van toepassing op de VM die is ingeschakeld. Voor de beste resultaten is dit ideaal voor deze functie op ten minste twee VMs verbonden met de dezelfde VNet. Tijdens het communiceren via VNets of verbindende on-premises, is deze functie een minimale invloed op algemene latentie.

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

1. Registreren voor de Preview-versie door een e-mailbericht te sturen naar [Netwerkproblemen abonnementen versneld](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) met uw abonnements-ID en bedoeld. Voer de overige stappen tot niet nadat u een e-mailbericht dat u hebt geaccepteerd in de Preview-versie hebt ontvangen.
2. Meld u aan bij van de Azure-Portal op http://portal.azure.com.
3. Maak een VM de stappen uit in het [maken van een Windows-VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) artikel selecteren van de volgende opties:
    - Selecteer een besturingssysteem dat wordt weergegeven in de sectie beperkingen van dit artikel.
    - Selecteer een locatie (regio) weergegeven in de sectie beperkingen van dit artikel.
    - Selecteer de grootte van een VM weergegeven in de sectie beperkingen van dit artikel. Als een van de ondersteunde formaten niet wordt weergegeven, klikt u op **Alles weergeven** in het blad **een grootte kiezen** om een grootte van een uitgevouwen lijst te selecteren.
    - Klik in het blad **Instellingen** , klikt u op *ingeschakeld* voor **Accelerated netwerken**, zoals wordt weergegeven in de volgende afbeelding:

        ![Instellingen](./media/virtual-network-accelerated-networking-portal/image3.png)

    >[AZURE.NOTE] De optie versneld toegang is alleen zichtbaar als u hebt:
    >
    >- Het voorbeeld is goedgekeurd
    >- Geselecteerd ondersteund besturingssysteem, locatie en VM tekengrootten die worden genoemd in de sectie beperkingen van dit artikel.

5. Nadat de VM is gemaakt, download het [stuurprogramma versneld netwerkproblemen](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84), verbinding te maken en meld u aan bij de VM en het installatieprogramma van het stuurprogramma binnen de VM uitvoeren.
6. Met de rechtermuisknop op de Windows-knop en klik op **Apparaatbeheer**. Controleer of dat de **Mellanox ConnectX-3 virtuele functie Ethernet-Adapter** wordt weergegeven onder de optie **netwerk** nadat dit is vergroot, zoals wordt weergegeven in de volgende afbeelding:

    ![Apparaatbeheer](./media/virtual-network-accelerated-networking-portal/image2.png)