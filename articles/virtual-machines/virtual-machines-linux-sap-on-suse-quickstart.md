<properties
   pageTitle="SAP NetWeaver op Microsoft Azure SUSE Linux VMs testen | Microsoft Azure"
   description="SAP NetWeaver op Microsoft Azure SUSE Linux VMs testen"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Actieve SAP NetWeaver op Microsoft Azure SUSE Linux VMs


In dit artikel worden de verschillende overwegingen wanneer u werkt met het SAP NetWeaver op Microsoft Azure SUSE Linux virtuele machines (VMs). SAP NetWeaver wordt vanaf mei 19 2016 officieel ondersteund op SUSE Linux VMs op Azure. Alle details over Linux versies, SAP versies, enzovoort vindt u in SAP notitie 1928533 "SAP-toepassingen op Azure: ondersteunde producten en Azure VM typen '.
Verdere documentatie over SAP op Linux VMs vindt u hier: [Gebruik van SAP op Linux virtuele machines (VMs)](virtual-machines-linux-sap-get-started.md).


Met behulp van de volgende informatie moet u enkele mogelijke nadelen kunt voorkomen.

## <a name="suse-images-on-azure-for-running-sap"></a>SUSE afbeeldingen op Azure voor het uitvoeren van SAP

Voor het uitvoeren van SAP NetWeaver op Azure, gebruikt u alleen SUSE Linux Enterprise Server SLES 12 (SPx): Zie ook SAP notitie 1928533. Afbeelding van een speciaal SUSE is in de Azure Marketplace ("SLES 11 SP3 voor SAP-ROEP"), maar dit niet is bedoeld voor algemene gebruik. Gebruik deze afbeelding niet omdat deze gereserveerd voor de oplossing [SAP Cloud toestel bibliotheek](https://cal.sap.com/) .  

U moet Azure Resource Manager gebruiken voor alle nieuwe tests en installaties op Azure. Als u wilt zoeken SUSE SLES afbeeldingen en versies met behulp van Azure PowerShell of de opdrachtregel Azure (CLI), gebruikt u de volgende opdrachten. Vervolgens kunt u de uitvoer, bijvoorbeeld naar de afbeelding OS definiëren in een JSON-sjabloon voor een nieuwe SUSE Linux VM implementeren.
Deze PowerShell-opdrachten zijn geldig voor het Azure PowerShell versie 1.0.1 en hoger.

* Let op bestaande uitgevers, inclusief SUSE:

   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```

* Let op bestaande aanbiedingen van SUSE:

   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```

* Let op SUSE SLES aanbiedingen:

   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   CLI : azure vm image list-skus westeurope SUSE SLES
   ```

* Zoek naar een specifieke versie van een SKU SLES:

   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12"
   CLI : azure vm image list westeurope SUSE SLES 12
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>Installatie van WALinuxAgent in een VM SUSE

De agent genoemd WALinuxAgent maakt deel uit van de afbeeldingen SLES in de Azure Marketplace. Voor informatie over het installeren van deze handmatig (bijvoorbeeld tijdens het uploaden van een SLES OS virtuele harde schijf (VHD) vanuit on-premises), raadpleegt u:

- [OpenSUSE] (http://software.opensuse.org/package/WALinuxAgent)

- [Azure] (virtual-machines-linux-goedgekeurd-distros.md)

- [SUSE] (https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP: "uitgebreide controle"

SAP: "enhanced aan monitoring" is een verplicht vereiste SAP uitvoeren op Azure. Controleer de details in SAP Houd er rekening mee 2191498 "SAP op Linux met Azure: Enhanced Monitoring".

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Azure gegevensschijven koppelen aan een Azure Linux VM

U moet nooit mountains Azure gegevensschijven voor een Azure Linux VM met behulp van het apparaat-ID. Gebruik in plaats daarvan de overal unieke id (UUID). Wees voorzichtig wanneer u de grafische functies waarmee u schijven mountains Azure-gegevens, bijvoorbeeld gebruiken. Controleer of u de items in /etc/fstab.

Het probleem met de apparaat-ID is dat deze kan worden gewijzigd en de VM Azure vervolgens in het proces te starten hangt mogelijk. Als u wilt beperken het probleem, kunt u de parameter nofail in /etc/fstab toevoegen. Maar wees voorzichtig met nofail omdat toepassingen de komma mountains als voordat u gebruiken kunnen en mogelijk Schrijf in het bestandssysteem hoofdsite geval een schijf externe Azure gegevens tijdens het opstarten niet is gekoppeld.

De enige uitzondering koppelen via UUID is een schijf OS voor het oplossen van problemen, toevoegen, zoals wordt beschreven in de volgende sectie.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Problemen met een SUSE VM die niet meer toegankelijk

Het is mogelijk dat er situaties waarin een VM SUSE op Azure in het proces te starten (bijvoorbeeld met een fout opgetreden vastloopt met het koppelen van schijven). U kunt dit probleem verifiëren met behulp van de functie voor opstarten diagnostische hulpprogramma's voor Azure virtuele Machines v2 in de portal van Azure. Zie voor meer informatie het artikel [opstarten diagnostics] (https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Een manier om het probleem zich is de schijf OS uit de beschadigde VM bijvoegen bij een andere SUSE VM op Azure. Vervolgens aanbrengen van wijzigingen zoals /etc/fstab bewerken of verwijderen van netwerk udev regels, zoals wordt beschreven in de volgende sectie.

Er is een belangrijk punt u rekening moet houden. Implementatie van verschillende SUSE VMs uit dezelfde Azure Marketplace afbeelding (bijvoorbeeld SLES 11 SP4) zorgt ervoor dat de schijf OS moet altijd op dezelfde UUID worden gekoppeld. Daarom leidt gebruik van de UUID naar een schijf OS bijvoegen vanuit een ander VM dat is geïmplementeerd met behulp van de afbeelding met dezelfde Azure Marketplace twee identieke UUID's. Hierdoor problemen en kan betekenen dat de VM bedoeld voor probleemoplossing in feite wordt opgestart van het bijgevoegde en beschadigde OS schijf in plaats van de oorspronkelijke map.

Er zijn twee manieren om dit te voorkomen:

* Gebruik een andere afbeelding van Azure Marketplace voor het oplossen van problemen VM (bijvoorbeeld SLES 11 SPx in plaats van SLES 12).
* De beschadigde OS schijf niet bijvoegen vanuit een andere VM met behulp van UUID--gebruik iets anders.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>Een VM SUSE vanuit on-premises naar Azure uploaden

Zie voor een beschrijving van de stappen voor het uploaden van een VM SUSE vanuit on-premises naar Azure, [voorbereiden een SLES of openSUSE virtuele machine voor Azure] (virtual-machines-linux-suse-create-upload-vhd.md).

Als u wilt uploaden van een VM zonder de stap identiteitsgegevens aan het eind (bijvoorbeeld om te behouden een bestaande SAP-installatie, evenals de hostnaam), controleert u de volgende items:

* Zorg ervoor dat de schijf OS is gekoppeld via UUID en niet de apparaat-ID. Wijzigen in UUID alleen in /etc/fstab is niet genoeg voor de schijf OS. Ook niet vergeten om aan te passen de opstartlaadprogramma via YaST of door te /boot/grub/menu.lst bewerken.
* Als u de indeling VHDX voor de schijf SUSE OS gebruiken en naar VHD converteren voor het uploaden naar Azure, is het zeer waarschijnlijk dat het netwerkapparaat wordt gewijzigd van eth0 in eth1. Om problemen te voorkomen wanneer u later op Azure bent opgestart, wijzig terug naar eth0 zoals beschreven in [eth0 oplossen in gekloonde SLES 11 VMware] (https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Naast het wat in het artikel wordt beschreven, raden we u aan dat u dit verwijderen:

   /lib/udev/Rules.d/75-persistent-NET-generator.Rules

U kunt ook de Azure Linux-Agent (waagent) om u te helpen voorkomen van potentiële problemen, zo lang maken als er niet meerdere NIC installeren.

## <a name="deploying-a-suse-vm-on-azure"></a>Een VM SUSE op Azure implementeren

U moet nieuwe SUSE VMs maken met behulp van JSON sjabloonbestanden in het nieuwe resourcemanager Azure-model. Nadat de JSON-sjabloonbestand is gemaakt, kunt u de VM kunt implementeren met behulp van de volgende opdracht uit CLI als alternatief voor PowerShell:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Zie voor meer informatie over JSON sjabloonbestanden, [Azure resourcemanager Authoring sjablonen] (resource-groep-authoring-templates.md) en [Azure quickstart sjablonen] (https://azure.microsoft.com/documentation/templates/).

Zie voor meer informatie over CLI en Azure resourcemanager, [Gebruik de CLI Azure voor Mac, Linux en Windows met Azure resourcemanager] (xplat-cli-azure-resource-manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP-licentie en hardware-toets

Voor de officiële SAP-Azure-certificering, is een nieuwe om geïntroduceerd als u wilt berekenen van de toets van SAP hardware die wordt gebruikt voor de SAP-licentie. De SAP-kernel moest maken gebruik van dit. Voormalige SAP versies voor Linux bevat geen deze codewijziging. Daarom in bepaalde situaties (bijvoorbeeld Azure VM het formaat wijzigen), de SAP hardwaresleutel gewijzigd en de SAP-licentie is niet langer geldig. Hiermee wordt opgelost in de meest recente SAP Linux kernels. Controleer SAP notitie 1928533 voor meer informatie.

## <a name="suse-sapconf-package--tuned-adm"></a>SUSE sapconf pakket / afgestemd adm

SUSE biedt een pakket 'sapconf' die worden beheerd in een reeks SAP-specifieke instellingen genoemd. Voor meer informatie over de werking van dit pakket en hoe u installeren en gebruiken, Zie [sapconf gebruiken voor het voorbereiden van een SUSE Linux Enterprise Server om uit te voeren SAP-systemen] (https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) en [wat sapconf is of hoe u een SUSE Linux Enterprise Server voorbereiden voor het uitvoeren van SAP-systemen?] (http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

In de tussentijd wordt er een nieuw hulpmiddel die sapconf - in-of adm. vervangt Een vindt meer informatie over dit hulpprogramma de onderstaande koppelingen volgen.

SLES documentatie over afgestemd adm profiel sap-hana vindt u [hier](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

Afstemmen systemen voor SAP-werkbelasting met afgestemd-adm - vindt u [hier](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) in hoofdstuk 6.2


## <a name="nfs-share-in-distributed-sap-installations"></a>NFS delen in gedistribueerde SAP-installaties

Als u hebt een gedistribueerde installatie--bijvoorbeeld waarop u wilt installeren van de database en de SAP-toepassing-servers in afzonderlijke VMs--kunt u de map /sapmnt via NFS Network File System () kunt delen. Als u problemen met de installatie-instructies ondervindt nadat u de NFS-aandeel voor /sapmnt hebt gemaakt, controleert u of "no_root_squash" is ingesteld voor de optie voor delen.

## <a name="logical-volumes"></a>Logische volumes

In het verleden desgewenst een een grote logisch volume over meerdere Azure gegevensschijven (bijvoorbeeld voor het SAP-database), is het raadzaam te gebruiken mdadm lvm is niet volledig nog gevalideerd op Azure. Zie informatie over het instellen van Linux RAID op Azure met behulp van mdadm, [configureren software RAID op Linux](virtual-machines-linux-configure-raid.md). In de tussentijd vanaf het begin van mei 2016 ook lvm wordt volledig ondersteund op Azure en kan worden gebruikt als alternatief voor mdadm. Zie voor meer informatie over lvm op Azure [LVM configureren op een VM Linux in Azure wordt aangegeven](virtual-machines-linux-configure-lvm.md).


## <a name="azure-suse-repository"></a>Azure SUSE opslagplaats

Als u een probleem met access naar het standard Azure SUSE-bibliotheek hebt, kunt u een eenvoudige opdracht opnieuw instellen. Dit kan gebeuren als u de afbeelding van een privé OS in een Azure gebied maakt en de afbeelding vervolgens kopiëren naar een ander gebied waar u wilt implementeren van nieuwe VMs die zijn gebaseerd op deze privé OS-afbeelding. Alleen de volgende opdracht binnen de VM uitvoeren:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gnome bureaublad

Als u het bureaublad Gnome gebruiken wilt voor het installeren van een voltooid SAP-demo systeem binnen een enkel VM--een SAP-gebruikersinterface, inclusief browser en SAP-beheerconsole--gebruikt u deze geheugensteun op de afbeeldingen Azure SLES installeren:

   Voor SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   Voor SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>SAP-ondersteuning voor Oracle voor Linux in de cloud

Er is een beperking van de ondersteuning van Oracle voor Linux in een gevirtualiseerde omgeving. Hoewel dit niet het onderwerp van een Azure-specifieke, is het is belangrijk om te begrijpen. SAP biedt geen ondersteuning voor Oracle op SUSE of rood rol in een openbare cloud zoals Azure. Als u wilt bespreken in dit onderwerp, neemt u rechtstreeks contact Oracle.
