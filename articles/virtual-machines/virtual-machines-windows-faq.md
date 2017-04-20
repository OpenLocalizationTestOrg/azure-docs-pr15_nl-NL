<properties
    pageTitle="Veelgestelde vragen over Windows VMs | Microsoft Azure"
    description="Hier vindt u antwoorden op enkele van de veelgestelde vragen over Windows virtuele machines die zijn gemaakt met het model resourcemanager."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Veelgestelde vragen over Windows virtuele Machines 


In dit artikel komen enkele veelgestelde vragen over Windows virtuele machines is gemaakt in Azure wordt aangegeven met het implementatiemodel resourcemanager. Zie [Veelgestelde vragen over Linux virtuele Machines](virtual-machines-linux-faq.md) voor de Linux-versie van dit onderwerp

## <a name="what-can-i-run-on-an-azure-vm"></a>Wat kan ik uitvoeren op een VM Azure?

Alle abonnees kunnen serversoftware uitgevoerd op een Azure virtuele machines. Zie voor informatie over het ondersteuningsbeleid voor het uitvoeren van Microsoft-serversoftware in Azure [serversoftware van Microsoft ondersteuning voor Azure virtuele Machines](https://support.microsoft.com/kb/2721672)

Bepaalde versies van Windows 7 en Windows 8.1 zijn beschikbaar voor abonnees met Azure MSDN-voordeel en abonnees van MSDN-ontwikkelaar en Pay-As-You-Go testen, voor ontwikkel- en taken. Zie voor meer informatie, inclusief instructies en tekortkomingen, [Windows Client-afbeeldingen voor MSDN-abonnees](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/). 


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Hoeveel opslagruimte kan ik gebruiken met een virtuele machine?

Elke gegevensschijf mag maximaal 1 TB. Het aantal gegevensschijven die u kunt gebruiken, is afhankelijk van de grootte van de virtuele machine. Zie [grootten voor virtuele Machines](virtual-machines-windows-sizes.md)voor meer informatie.

Een account Azure opslagruimte biedt opslag voor de schijf besturingssysteem en gegevensschijven. Elke schijf is opgeslagen als een pagina-blob VHD-bestand. Zie voor prijzen details, [Opslag prijzen Details](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Hoe kan ik mijn VM openen?

Een verbinding met extern met behulp van Remote Desktop-verbinding (RDP) voor een Windows-VM opzetten. Zie voor instructies [hoe u contact kunt leggen en meld u aan bij een Azure virtuele machines waarop Windows wordt uitgevoerd](virtual-machines-windows-connect-logon.md). Maximaal twee gelijktijdige verbindingen worden ondersteund, tenzij de server is geconfigureerd als een sessiehost Remote Desktop Services.  


Als u problemen met extern bureaublad ondervindt, raadpleegt u [problemen met extern bureaublad verbindingen naar een Windows Azure virtuele Machine](virtual-machines-windows-troubleshoot-rdp-connection.md). 

Als u bekend met Hyper-V bent, kijkt u misschien voor een hulpmiddel die vergelijkbaar is met VMConnect. Azure niet mogelijk is een vergelijkbaar hulpprogramma omdat consoletoegang tot een virtuele machine wordt niet ondersteund.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Kan ik de tijdelijke schijf (het station D: al dan niet standaard) voor de opslag van gegevens gebruiken?

De tijdelijke schijf niet worden gebruikt voor de opslag van gegevens. Dit is alleen tijdelijke, zodat u zou risico gegevensverlies dat kan niet worden hersteld. Gegevensverlies kan optreden wanneer de virtuele machine wordt verplaatst naar een andere host. Het formaat van een virtuele machine wijzigen, zijn het bijwerken van de host of een hardwarefout op de host enkele redenen die een virtuele machine mogelijk verplaatsen.

Als u een toepassing die vereist zijn voor het gebruik van de letter D: hebt, kunt u stationsletters kunt toewijzen, zodat de tijdelijke schijf wordt gebruikt in een ander nummer dan D:. Zie [de aanduiding van de tijdelijke schijf Windows](virtual-machines-windows-classic-change-drive-letter.md)voor instructies.

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Hoe kan ik de aanduiding van de tijdelijke schijf wijzigen?

U kunt de letter wijzigen door het paginabestand verplaatsen en opnieuw toewijzen stationsletters, maar u nodig hebt om ervoor te zorgen dat u de stappen in een specifieke volgorde doen. Zie [de aanduiding van de tijdelijke schijf Windows](virtual-machines-windows-classic-change-drive-letter.md)voor instructies.

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Kan ik een bestaande VM toevoegen aan een set beschikbaarheid?

Nee. Als u wilt dat uw VM wilt toevoegen aan een set beschikbaarheid, moet u de VM binnen de set maken. Er is momenteel niet een manier om een VM toevoegen aan een beschikbaarheid instellen nadat deze is gemaakt.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Kan ik een virtuele machine uploaden naar Azure?

Ja. Zie voor instructies voor het [uploaden van een afbeelding met een VM van Windows Azure](virtual-machines-windows-upload-image.md)

## <a name="can-i-resize-the-os-disk"></a>Kan ik de grootte van de schijf OS wijzigen?

Ja. Zie voor instructies [hoe u het station OS van een virtuele Machine in een resourcegroep Azure uitvouwen](virtual-machines-windows-expand-os-disk.md).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kan ik kopiëren of een bestaande Azure VM klonen?

Ja. Zie voor instructies [hoe u een kopie van een virtuele Windows-computer in het implementatiemodel resourcemanager maken](virtual-machines-windows-vhd-copy.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Waarom zie ik geen Canada centraal en Canada Oost regio's via Azure resourcemanager?

De twee nieuwe gebieden van Centraal Canada en Canada Oost worden niet automatisch geregistreerd voor VM maken voor bestaande Azure abonnementen. Deze registratie wordt automatisch uitgevoerd wanneer een virtuele machine wordt geïmplementeerd via de portal Azure naar een andere regio met Azure Resource Manager. Nadat een virtuele machine wordt geïmplementeerd op een andere Azure regio, moeten de nieuwe regio's beschikbaar zijn voor latere virtuele machines.

## <a name="does-azure-support-linux-vms"></a>Ondersteunt Azure Linux VMs?

Ja. Als u wilt snel een Linux VM om uit te proberen hebt gemaakt, Zie [maken een VM Linux op Azure met behulp van de Portal](virtual-machines-linux-quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kan ik een NIC toevoegen aan mijn VM nadat deze gemaakt?

Nee. Het toevoegen van een NIC kan alleen worden uitgevoerd bij het maken.

## <a name="are-there-any-computer-name-requirements"></a>Zijn er een vereisten voor de naam van elke computer?

Ja. De naam van de computer kan maximaal 15 tekens lang zijn. Zie de [infrastructuur naming richtlijnen](virtual-machines-windows-infrastructure-naming-guidelines.md) voor meer informatie over het geven van namen aan uw resources.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Wat zijn de vereisten gebruikersnaam bij het maken van een VM?

Gebruikersnamen kan maximaal 20 tekens lang zijn en kunnen niet eindigen met een punt ("."). 

De volgende gebruikersnamen zijn niet toegestaan:

<table>
    <tr>
        <td style="text-align:center">beheerder </td><td style="text-align:center"> beheerder </td><td style="text-align:center"> gebruiker </td><td style="text-align:center"> Gebruiker1</td>
    </tr>
    <tr>
        <td style="text-align:center">testen </td><td style="text-align:center"> Gebruiker2 </td><td style="text-align:center"> Test1 </td><td style="text-align:center"> User3</td>
    </tr>
    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> een</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> adm </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> ASPNET</td>
    </tr>
    <tr>
        <td style="text-align:center">back-up maken </td><td style="text-align:center"> console </td><td style="text-align:center"> David </td><td style="text-align:center"> Gast</td>
    </tr>
    <tr>
        <td style="text-align:center">John </td><td style="text-align:center"> eigenaar </td><td style="text-align:center"> hoofdmap </td><td style="text-align:center"> Server</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL </td><td style="text-align:center"> ondersteuning </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> sys</td>
    </tr>
    <tr>
        <td style="text-align:center">Test2 </td><td style="text-align:center"> Test3 </td><td style="text-align:center"> User4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Wat zijn de wachtwoordvereisten bij het maken van een VM?

Wachtwoorden moeten 8-123 tekens lang en 3 uit de volgende 4 complexiteitsvereisten voldoen:

- Lagere tekens bevatten
- Bovenste tekens bevatten
- Hebt u een cijfer
- Hebt u een speciaal teken (Regex overeenkomen met [\W_])

De volgende wachtwoorden zijn niet toegestaan:

<table>
    <tr>
        <td style="text-align:center">abc@123</td><td style="text-align:center">P@$$w0rd</td><td style="text-align:center">P@ssw0rd</td><td style="text-align:center">P@ssword123</td><td style="text-align:center">Pa$ $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td><td style="text-align:center">Wachtwoord!</td><td style="text-align:center">Wachtwoord1</td><td style="text-align:center">Password22</td><td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
