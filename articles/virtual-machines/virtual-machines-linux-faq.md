<properties
    pageTitle="Veelgestelde vragen over Linux VMs | Microsoft Azure"
    description="Hier vindt u antwoorden op enkele van de veelgestelde vragen over Linux virtuele machines die zijn gemaakt met het model resourcemanager."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Veelgestelde vragen over Linux virtuele Machines

In dit artikel komen enkele veelgestelde vragen over Linux virtuele machines is gemaakt in Azure wordt aangegeven met het implementatiemodel resourcemanager. Voor de Windows-versie van dit onderwerp, raadpleegt u [Veelgestelde vragen over Windows virtuele Machines](virtual-machines-windows-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Wat kan ik uitvoeren op een VM Azure?

Alle abonnees kunnen serversoftware uitgevoerd op een Azure virtuele machines. Zie voor meer informatie, [Linux op Azure-Endorsed onderzoeken](virtual-machines-linux-endorsed-distros.md)


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Hoeveel opslagruimte kan ik gebruiken met een virtuele machine?

Elke gegevensschijf mag maximaal 1 TB. Het aantal gegevensschijven die u kunt gebruiken, is afhankelijk van de grootte van de virtuele machine. Zie [grootten voor virtuele Machines](virtual-machines-linux-sizes.md)voor meer informatie.

Een account Azure opslagruimte biedt opslag voor de schijf besturingssysteem en gegevensschijven. Elke schijf is opgeslagen als een pagina-blob VHD-bestand. Zie voor prijzen details, [Opslag prijzen Details](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Hoe kan ik mijn VM openen?

Externe verbinding met het aanmelden bij de virtuele machine, met Secure Shell (SSH). Zie de instructies op hoe u verbinding maakt [vanuit Windows](virtual-machines-linux-ssh-from-windows.md) of [Linux- en Mac](virtual-machines-linux-mac-create-ssh-keys.md). Standaard kan SSH maximaal 10 verbindingen tegelijk. U kunt dit nummer vergroten door het configuratiebestand bewerken.


Als u problemen ondervindt, raadpleegt u [problemen met Secure Shell (SSH)-verbindingen](virtual-machines-linux-troubleshoot-ssh-connection.md).


## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Kan ik de tijdelijke schijf (/ ontwikkelaar/sdb1) voor de opslag van gegevens gebruiken?

De tijdelijke schijf (/ ontwikkelaar/sdb1) niet worden gebruikt voor de opslag van gegevens. Is het alleen er tijdelijke opslag. Verloren gegevens die kan niet worden hersteld.


## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kan ik kopiëren of een bestaande Azure VM klonen?

Ja. Zie voor instructies [hoe u een kopie van een Linux virtuele machine in het implementatiemodel resourcemanager maakt](virtual-machines-linux-copy-vm.md).


## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Waarom zie ik geen Canada centraal en Canada Oost regio's via Azure resourcemanager?

De twee nieuwe gebieden van Centraal Canada en Canada Oost worden niet automatisch geregistreerd voor VM maken voor bestaande Azure abonnementen. Deze registratie wordt automatisch uitgevoerd wanneer een virtuele machine wordt geïmplementeerd via de portal Azure naar een andere regio met Azure Resource Manager. Nadat een virtuele machine wordt geïmplementeerd op een andere Azure regio, moeten de nieuwe regio's beschikbaar zijn voor latere virtuele machines.


## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kan ik een NIC toevoegen aan mijn VM nadat deze gemaakt?

Nee. Het toevoegen van een NIC kan alleen worden uitgevoerd bij het maken.


## <a name="are-there-any-computer-name-requirements"></a>Zijn er een vereisten voor de naam van elke computer?

Ja. De naam van de computer mag maximaal 64 tekens lang zijn. Zie de [infrastructuur naming richtlijnen](virtual-machines-linux-infrastructure-naming-guidelines.md) voor meer informatie over het geven van namen aan uw resources.


## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Wat zijn de vereisten gebruikersnaam bij het maken van een VM?

Gebruikersnamen moet 1-64 tekens lang zijn.

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

Wachtwoorden moeten 6-72 tekens lang en 3 uit de volgende 4 complexiteitsvereisten voldoen:

- Lagere tekens bevatten
- Bovenste tekens bevatten
- Hebt u een cijfer
- Hebt u een speciaal teken (Regex overeenkomen met [\W_])

De volgende wachtwoorden zijn niet toegestaan:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$ $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Wachtwoord!</td>
        <td style="text-align:center">Wachtwoord1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
