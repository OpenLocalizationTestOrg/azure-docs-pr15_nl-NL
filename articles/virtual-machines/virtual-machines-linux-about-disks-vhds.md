<properties
    pageTitle="Over schijven en VHD's voor Linux VMs | Microsoft Azure"
    description="Meer informatie over de basisbeginselen van schijven en VHD's voor Linux virtuele machines in Azure wordt aangegeven."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="cynthn"/>

# <a name="about-disks-and-vhds-for-azure-virtual-machines"></a>Over schijven en VHD's voor Azure virtuele machines

Net als een andere computer gebruik virtuele machines in Azure schijven als een locatie voor de opslag van een besturingssysteem gebruikt, -toepassingen en gegevens. Alle Azure virtuele machines hebt ten minste twee schijven – een Linux besturingssysteem schijf (in geval van een VM Linux) en een tijdelijke schijf. De schijf besturingssysteem wordt gemaakt van een afbeelding en zowel de schijf besturingssysteem en de afbeelding is virtual vaste schijven die zijn opgeslagen in een Azure opslag-account. Virtuele machines kunt ook een of meer gegevensschijven, die ook zijn opgeslagen als VHD's hebben. In dit artikel is ook beschikbaar voor [Windows virtuele machines](virtual-machines-windows-about-disks-vhds.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

> [AZURE.NOTE] Als u even hebt, kunt u ons helpen de documentatie Azure Linux VM verbeteren door deze [Snelle enquête](https://aka.ms/linuxdocsurvey) van uw ervaringen. Elk antwoord kan wij help u uw werk te doen krijgt.

## <a name="operating-system-disk"></a>Besturingssysteem Schijfopruiming

Elke VM heeft één bijgevoegde besturingssysteem schijf. Er is geregistreerd als een SATA schijf en als het station C: al dan niet standaard. Deze schijf heeft een maximale capaciteit van 1023 gigabyte (GB). 

##<a name="temporary-disk"></a>Tijdelijke schijf

De tijdelijke schijf wordt automatisch voor u gemaakt. Op Linux virtuele machines, de schijf is is meestal /dev/sdb en opgemaakt en gekoppeld aan /mnt/resource door de Azure Linux-Agent.

De grootte van de tijdelijke schijf variëren naar gelang de grootte van de virtuele machine. Zie [grootten voor Linux virtuele machines](virtual-machines-linux-sizes.md)voor meer informatie.

>[AZURE.WARNING] Geen gegevens niet opslaan op de tijdelijke schijf. Deze tijdelijke biedt voor toepassingen en processen en is bedoeld voor de opslag alleen gegevens zoals pagina of omwisselen-bestanden. 

Zie voor meer informatie over het gebruik van de tijdelijke schijf in Azure, [Wat betekent het tijdelijke station op Microsoft Azure virtuele Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Gegevens Schijfopruiming

Een gegevensschijf is een VHD die gekoppeld aan een virtuele machine voor de opslag van toepassingsgegevens of andere gegevens die u wilt bewaren. Gegevensschijven als SCSI stations zijn geregistreerd en worden aangeduid met een letter die u kiest.  Elke gegevensschijf heeft een maximale capaciteit van 1023 GB. De grootte van de virtuele machine bepaalt hoeveel gegevensschijven u en het type opslag koppelen kunt die u kunt gebruiken voor het hosten van de schijven.

>[AZURE.NOTE] Zie voor meer informatie over virtuele machines capaciteiten [grootten voor Linux virtuele machines](virtual-machines-linux-sizes.md).

Azure Hiermee maakt u een schijf besturingssysteem wanneer u een virtuele machine op basis van een afbeelding. Als u een afbeelding met gegevensschijven gebruikt, Azure ook de gegevensschijven gemaakt bij het maken van de virtuele machine. Anders toevoegen u gegevensschijven nadat u de virtuele machine hebt gemaakt.

U kunt gegevensschijven toevoegen aan een virtuele machine op elk gewenst moment, door te **koppelen van** de schijf is virtual machine. U kunt een VHD die u hebt geüpload of gekopieerd naar uw opslag-account of een eigenschap Azure voor u gemaakt. Een gegevensschijf koppelen koppelt het VHD-bestand uit uw account opslagruimte aan de virtuele computer door een 'lease' in de VHD plaatsen zodat deze niet worden verwijderd uit de opslag terwijl deze nog steeds gekoppeld.

## <a name="about-vhds"></a>Over VHD 's

De VHD's gebruikt in Azure zijn VHD-bestanden die zijn opgeslagen als BLOB van pagina's in een standaard- of de premium-opslag-account in Azure wordt aangegeven. Zie [lidmaatschap blok BLOB's en pagina BLOB's](https://msdn.microsoft.com/library/ee691964.aspx)voor meer informatie over pagina BLOB's. Zie voor meer informatie over de premium-opslag, [Premium opslag: High-performance-opslag voor Azure virtuele machines werkbelasting](../storage/storage-premium-storage.md).

Azure ondersteunt de vaste schijf VHD-indeling. Vaste indeling worden de logische schijf af lineair binnen het bestand, zodat deze schijf Verschuiving X is opgeslagen op blob offset X. Een kleine voettekst aan het einde van de blob beschrijft de eigenschappen van de VHD. Vaste indeling verspild vaak ruimte omdat de meeste schijven grote ongebruikte bereiken in deze hebben. Azure slaat echter VHD-bestanden in een verspreid notatie, zodat u de voordelen van de vaste en dynamische schijven op hetzelfde moment ontvangen. Zie [aan de slag met virtuele vaste schijven](https://technet.microsoft.com/library/dd979539.aspx)voor meer informatie.

Alle VHD-bestanden in Azure wordt aangegeven dat u gebruiken als een bron wilt voor schijven of afbeeldingen zijn alleen-lezen. Wanneer u een schijf of afbeelding maakt, kunt u met Azure kopieën van de VHD-bestanden. Deze kopieën zijn alleen-lezen of alleen-lezen-en-schrijven, afhankelijk van hoe u de VHD gebruiken.

Wanneer u een virtuele machine van een afbeelding maken, wordt een schijf voor de virtuele machine die een kopie van het bronbestand VHD is gemaakt in Azure. Als u wilt beveiligen tegen per ongeluk wordt verwijderd, wordt met Azure een lease op een VHD bronbestand die wordt gebruikt voor het maken van een afbeelding, een schijf besturingssysteem of een gegevensschijf geplaatst.

Voordat u een bron VHD-bestand verwijderen kunt, moet u de lease verwijderen door de schijf of de afbeelding te verwijderen. Als u wilt verwijderen van een VHD-bestand dat wordt gebruikt door een virtuele machine als een schijf besturingssysteem gebruikt, kunt u de virtuele machine, de schijf besturingssysteem, verwijderen en het bronbestand VHD allemaal tegelijk door het verwijderen van de virtuele machine en verwijder alle bijbehorende schijven. Echter enkele stappen uitvoeren in de volgorde van een set verwijderen van een VHD-bestand dat is een bron voor een gegevensschijf vereist--loskoppelen van de schijf van de virtuele machine verwijderen van de schijf en verwijder vervolgens het VHD-bestand.

>[AZURE.WARNING] Als u een bron VHD-bestand uit de opslag verwijderen of uw account opslagruimte verwijderen, herstellen niet Microsoft die gegevens voor u.


## <a name="troubleshooting"></a>Problemen oplossen
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Volgende stappen

-  [Bijvoegen een schijf](virtual-machines-linux-add-disk.md) aan extra opslagruimte voor uw VM wilt toevoegen.
-  [Configureren software RAID](virtual-machines-linux-configure-raid.md) voor redundantie.
-  [Een Linux virtuele machine vastleggen](virtual-machines-linux-classic-capture-image.md) zodat u snel extra VMs kunt implementeren.


