<properties
    pageTitle="Inleiding tot Linux in Azure | Microsoft Azure"
    description="Meer informatie over het gebruik van Linux virtuele machines op Azure."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

#<a name="introduction-to-linux-on-azure"></a>Inleiding tot Linux op Azure

In dit onderwerp biedt een overzicht van bepaalde aspecten van het gebruik van Linux virtuele machines in de cloud Azure. Een Linux virtuele machine is distribueren eenvoudig gebruik van een afbeelding uit de galerie.


## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Verificatie: Gebruikersnamen, wachtwoorden en SSH toetsen

Wanneer u een Linux virtuele machine met behulp van de Azure klassieke portal maakt, wordt u gevraagd om te leveren een gebruikersnaam, wachtwoord of een openbare sleutel SSH. De keuze van een gebruikersnaam voor de implementatie van een Linux virtuele machine op Azure is onderhevig aan de volgende beperking: namen van systeemaccounts (UID < 100) aanwezig in de virtuele machine zijn niet toegestaan, 'root' bijvoorbeeld.


 - Zie [een virtuele Machine waarop Linux maken](virtual-machines-linux-quick-create-cli.md)
 - Lees [hoe u met SSH met Linux op Azure](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="obtaining-superuser-privileges-using-sudo"></a>Het verkrijgen van beheerder bevoegdheden gebruiken`sudo`

Het gebruikersaccount waarmee wordt opgegeven tijdens de implementatie van virtuele machines exemplaar op Azure is een bevoegdheden-account. Dit account is geconfigureerd door de Azure Linux-Agent kunnen worden omgezet bevoegdheden voor het gebruik van de hoofdmap (beheerder-account) de `sudo` hulpprogramma. Nadat u bent aangemeld met deze gebruikersaccount, is mogelijk opdrachten als basis voor het gebruik van de opdrachtsyntaxis van de uitvoeren

    # sudo <COMMAND>

U kunt desgewenst een hoofdsite shell **sudo -s**met aanvragen.

- Zie [werken met hoofdsite bevoegdheden op Linux virtuele machines in Azure wordt aangegeven](virtual-machines-linux-use-root-privileges.md)


## <a name="firewall-configuration"></a>Configuratie van de firewall

Azure biedt een binnenkomend pakketfilter dat connectiviteit tot poorten in de portal van Azure klassieke wordt beperkt. Standaard is de enige toegestane poort SSH. U kunt op uw Linux virtuele machine van toegang tot extra poorten openen door het configureren van de eindpunten in de portal van Azure klassieke:

 - Zie: [het instellen van de eindpunten naar een virtuele Machine](virtual-machines-windows-classic-setup-endpoints.md)

De Linux-afbeeldingen in de galerie met Azure Schakel de firewall *iptables* al dan niet standaard. Indien gewenst, kan de firewall worden geconfigureerd voor extra filteren.


## <a name="hostname-changes"></a>Hostname wijzigingen

Wanneer u een exemplaar van de afbeelding van een Linux in eerste instantie implementeert, moet u een naam voor de virtuele machine bieden. Zodra de virtuele machine actief is, wordt deze hostname wordt gepubliceerd naar de DNS-servers van platform zodat meerdere virtuele machines met elkaar verbonden IP-adres zoekacties met hostnamen kunt uitvoeren.

Als hostname wijzigingen aanbrengen wilt nadat een virtuele machine is geïmplementeerd, gebruikt u de opdracht

    # sudo hostname <newname>

De Azure Linux-Agent bevat een functie voor automatisch detecteren deze naamswijziging en de virtuele machine voor deze wijziging behouden en deze wijziging publiceren naar de DNS-servers van platform configureren.

 - [De gebruikershandleiding Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md)

### <a name="cloud-init"></a>Cloud initialisatie
**Ubuntu** en **CoreOS** afbeeldingen gebruiken cloud initialisatie op Azure, waarmee extra mogelijkheden voor een virtuele machine bootstraps.

 - [Hoe u aangepaste gegevens invoeren](virtual-machines-windows-classic-inject-custom-data.md)
 - [Aangepaste gegevens en Cloud initialisatie op Microsoft Azure](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [Azure omwisselen partities met Cloud initialisatie maken](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [Het gebruik van CoreOS op Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="virtual-machine-image-capture"></a>VM afbeelding vastleggen

Azure biedt de mogelijkheid om vast te leggen de status van een bestaande virtuele machine in een afbeelding die vervolgens kan worden gebruikt om extra VM exemplaren implementeren. De Azure Linux-Agent is mogelijk gebruikt te draaien enkele van de aanpassing die is uitgevoerd tijdens het inrichten. U mogelijk de volgende stappen om vast te leggen een virtuele machine als een afbeelding:

1. Uitvoeren **waagent-identiteitsgegevens** om inrichten aanpassing ongedaan te maken. Of **waagent-identiteitsgegevens + gebruiker** (optioneel) het gebruikersaccount dat is opgegeven tijdens inrichten en alle bijbehorende gegevens verwijderen.

2. Sluit de virtuele machine omlaag/uitschakelen.

3. Klik op *vastleggen* in de portal van Azure klassieke of de Powershell of CLI hulpmiddelen gebruiken om vast te leggen de virtuele machine als een afbeelding.

 - Zie: [hoe om vast te leggen Linux virtuele machines gebruiken als een sjabloon](virtual-machines-linux-classic-capture-image.md)


## <a name="attaching-disks"></a>Schijven koppelen

Elke virtuele machine heeft een tijdelijke, lokale *schijf van de resource* die zijn gekoppeld. Omdat de gegevens op een resource-schijf mogelijk niet kunnen gaan over de computer opnieuw hebt opgestart, wordt deze vaak gebruikt door de toepassingen en processen worden uitgevoerd in de virtuele machine voor tijdelijk en **tijdelijke** opslag van gegevens. Dit wordt ook gebruikt voor de pagina opslaan of bestanden voor het besturingssysteem uitwisselen.

Klik op Linux, is de resource-schijf gewoonlijk beheerd door de Azure Linux-Agent en automatisch worden gekoppeld aan **/mnt/resource** (of **mnt** op Ubuntu afbeeldingen).


>[AZURE.NOTE] Houd er rekening mee dat de resource-schijf een **tijdelijke** schijf is, en kan worden verwijderd en opnieuw opgemaakt wanneer de VM opnieuw is opgestart.

Klik op Linux de gegevensschijf kan naam door de kernel als `/dev/sdc`, en gebruikers moeten kunnen partitioneren, opmaken en het koppelen van deze resource. Hierover vindt u stapsgewijze in deze zelfstudie: [hoe u een schijf gegevens aan een virtuele Machine hebt toegevoegd](virtual-machines-linux-classic-attach-disk.md).

 - **Zie ook:** [Software RAID op Linux configureren](virtual-machines-linux-configure-raid.md)  &  [LVM op een VM Linux in Azure configureren](virtual-machines-linux-configure-lvm.md)

