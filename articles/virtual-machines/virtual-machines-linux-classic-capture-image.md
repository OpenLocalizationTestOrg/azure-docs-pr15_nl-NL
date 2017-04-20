<properties
    pageTitle="Een afbeelding maken van een Linux VM | Microsoft Azure"
    description="Leer hoe u een afbeelding van een Linux-Azure virtuele machine (VM) die zijn gemaakt met het implementatiemodel klassieke vastleggen."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="iainfou"/>


# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Hoe om vast te leggen, een klassieke Linux virtuele machine als een afbeelding

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe [u deze stappen uitvoert met het model resourcemanager](virtual-machines-linux-capture-image.md).

In dit artikel leest u hoe om vast te leggen klassieke Azure virtuele machines Linux uitgevoerd als een afbeelding andere virtuele machines maken. In deze afbeelding bevat het OS schijf en gegevensschijven die zijn bijgevoegd bij de virtuele machine. Deze bevat geen netwerkconfiguratie, dus u hoeft te configureren dat wanneer u de andere virtuele machines van de afbeelding maken.

Azure slaat de afbeelding onder **afbeeldingen**, samen met alle afbeeldingen die u hebt geüpload. Zie voor meer informatie over afbeeldingen [Over VM-afbeeldingen in Azure wordt aangegeven] [].

## <a name="before-you-begin"></a>Voordat u begint

Deze stappen wordt ervan uitgegaan dat u hebt al een Azure virtuele machine met het model Klassiek implementatie gemaakt en geconfigureerd van het besturingssysteem, inclusief eventuele gegevensschijven koppelen. Als u een VM maken moet, leest u [hoe u een Linux virtuele Machine maken] [].


## <a name="capture-the-virtual-machine"></a>De virtuele machine vastleggen

1. [Verbinding maken met de virtuele machine](virtual-machines-linux-mac-create-ssh-keys.md) via een client SSH van uw keuze.

2. Typ de volgende opdracht in het venster SSH. De uitvoer van `waagent` enigszins mogelijk naar gelang de versie van dit hulpprogramma:

    `sudo waagent -deprovision+user`

    Deze opdracht probeert het systeem wissen en deze geschikt voor reprovisioning maken. Deze bewerking kunt u de volgende taken uitvoeren:

    - Hiermee verwijdert u SSH host toetsen (als Provisioning.RegenerateSshHostKeyPair 'y' in het configuratiebestand is)
    - Hiermee wist nameserver configuratie in /etc/resolv.conf
    - Hiermee verwijdert u de `root` gebruikerswachtwoord uit/enzovoort/schaduw (als Provisioning.DeleteRootPassword is 'y' in het configuratiebestand)
    - Hiermee verwijdert u DHCP-client lease met cache
    - Hiermee stelt u de hostnaam localhost.localdomain
    - Hiermee verwijdert u het laatste ingerichte gebruiker account (verkregen uit /var/lib/waagent) **en de bijbehorende gegevens**.

    >[AZURE.NOTE] Deprovisioning Hiermee verwijdert u bestanden en gegevens "generaliseren" van de afbeelding. Deze opdracht alleen uitvoeren op een virtuele machine die u van plan bent om vast te leggen als een nieuwe afbeelding van de sjabloon. Dit betekent niet dat de afbeelding van alle gevoelige informatie is uitgeschakeld of geschikt voor distributie aan derden is.


3. Typ **y** om door te gaan. U kunt toevoegen de `-force` parameter om te voorkomen dat dit bevestigen.

4. Typ **Afsluiten** de client SSH sluiten.

    >[AZURE.NOTE] De overige stappen wordt ervan uitgegaan dat u al hebt [geïnstalleerd de CLI Azure](../xplat-cli-install.md) op de clientcomputer. De volgende stappen is ook mogelijk in de [klassieke Azure-portal] [].

5. Open vanaf uw computer, Azure CLI en meld u aan bij uw Azure-abonnement. Lees voor meer informatie [verbinding maken met een Azure-abonnement van de Azure CLI](../xplat-cli-connect.md).

6. Controleer of dat u bent in de modus voor servicebeheer:

    `azure config mode asm`

7. De virtuele machine die al in de vorige stappen wordt opgeheven afsluiten:

    `azure vm shutdown <your-virtual-machine-name>`

    >[AZURE.NOTE] U vindt alle de virtuele machines in uw abonnement is gemaakt met behulp van`azure vm list`

8. Wanneer de virtuele machine is gestopt, moet u de afbeelding met de opdracht maken:

    `azure vm capture -t <your-virtual-machine-name> <new-image-name>`

    Typ de naam van de afbeelding die u in plaats van de _nieuwe afbeelding-naam wilt_. Deze opdracht Hiermee maakt u een algemene OS-afbeelding. De `-t` vervolgmenu Hiermee verwijdert u de oorspronkelijke virtuele machine.

9.  De nieuwe afbeelding is nu beschikbaar in de lijst met afbeeldingen die kunnen worden gebruikt om een nieuwe virtuele machines configureren. U kunt deze weergeven met de opdracht:

    `azure vm image list`

    In de [klassieke Azure-portal] [], wordt deze weergegeven in de lijst met **afbeeldingen** .

    ![Afbeelding van de opname geslaagd](./media/virtual-machines-linux-classic-capture-image/VMCapturedImageAvailable.png)


## <a name="next-steps"></a>Volgende stappen
De afbeelding is klaar voor het maken van virtuele machines worden gebruikt. U kunt de opdracht Azure CLI `azure vm create` en leveren van de naam van de afbeelding die u hebt gemaakt. Zie [gebruik van de Azure CLI met klassieke implementatiemodel](../virtual-machines-command-line-tools.md) voor meer informatie over de opdracht. De [portal van Azure klassieke] [] ook gebruiken om te maken van een aangepaste virtuele machine met behulp van de methode **Uit galerie** en selecteren van de afbeelding die u hebt gemaakt. Lees [hoe u een aangepaste virtuele Machine maken] [] voor meer informatie.

**Zie ook:** [De gebruikershandleiding Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md)

[Azure klassieke portal]: http://manage.windowsazure.com
[Over VM afbeeldingen in Azure wordt aangegeven]: virtual-machines-linux-classic-about-images.md
[Het maken van een aangepaste virtuele Machine]: virtual-machines-linux-classic-create-custom.md
[How to Attach a Data Disk to a Virtual Machine]: virtual-machines-windows-classic-attach-disk.md
[Het maken van een Linux virtuele Machine]: virtual-machines-linux-classic-create-custom.md
