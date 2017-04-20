<properties
   pageTitle="Technische ondersteuning: herstel vanuit on-premises naar Azure | Microsoft Azure"
   description="Artikel op lidmaatschap en ontwerpen herstel systemen vanaf on-premises implementatie-infrastructuur om Azure"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-on-premises-to-azure"></a>Technische ondersteuning van Azure tolerantie: herstel vanuit on-premises naar Azure

Azure biedt een uitgebreide set van services voor het inschakelen van de extensie van een on-premises implementatie-datacenter naar Azure voor hoge beschikbaarheid:

* __Netwerkproblemen__: met een virtual private network, u veilig uw on-premises netwerk in de cloud uitbreiden.
* __Berekenen__: klanten die gebruikmaken van Hyper-V on-premises kunnen 'til en shift' bestaande virtuele machines (VMs) naar Azure.
* __Opslag__: StorSimple breidt de mogelijkheden van uw bestandssysteem met Azure Storage. De Azure back-service biedt back-up voor bestanden en SQL-databases met Azure Storage.
* __Databasereplicatie__: met SQL Server 2014 (of later) beschikbaarheidsgroepen, kunt u een hoge beschikbaarheid herstel implementeren voor uw on-premises gegevens.

##<a name="networking"></a>Netwerken

Azure Virtual Network kunt u een logisch geïsoleerd sectie maken in Azure en verbindt u deze veilig naar uw on-premises implementatie-datacenter of een enkel clientcomputer met behulp van een IPSec-verbinding. Met virtueel netwerk, kunt u profiteren van de infrastructuur scalable, op aanvraag in Azure wordt aangegeven terwijl connectiviteit met gegevens en toepassingen on-premises systemen waarop Windows Server, mainframes en UNIX inclusief. Zie [Azure toegang documentatie](../virtual-network/virtual-networks-overview.md) voor meer informatie.

##<a name="compute"></a>Berekenen

Als u Hyper-V on-premises gebruikt, u kunt 'til en shift' bestaande Azure virtuele machines en Windows Server 2012 (of later), uitgevoerd zonder aanbrengen in de VM wijzigingen of converteren VM indelingen serviceproviders. Zie voor meer informatie [over schijven en VHD's voor Azure virtuele machines](../virtual-machines/virtual-machines-linux-about-disks-vhds.md).

##<a name="azure-site-recovery"></a>Herstel van Azure-Site

Als u wilt herstel als een service (DRaaS), biedt Azure [Azure sites worden hersteld](https://azure.microsoft.com/services/site-recovery/). Azure Site herstel biedt uitgebreide bescherming van VMware, Hyper-V en fysieke servers. Met Azure sites worden hersteld, kunt u een andere on-premises implementatie-server of Azure gebruiken als uw site herstellen. Zie voor meer informatie over Azure sites worden hersteld, de [documentatie van Azure sites worden hersteld](https://azure.microsoft.com/documentation/services/site-recovery/).

##<a name="storage"></a>Opslag

Er zijn verschillende opties voor het gebruik van Azure als een back-website voor on-premises gegevens.

###<a name="storsimple"></a>StorSimple

StorSimple veilig en transparant geïntegreerd cloudopslag voor on-premises implementatie-toepassingen. Maar biedt ook een enkel toestel die krachtige doorverbonden lokaal en cloudopslag, live archivering cloudgebaseerde gegevensbescherming en herstel biedt. Zie de [productpagina StorSimple](https://azure.microsoft.com/services/storsimple/)voor meer informatie.

###<a name="azure-backup"></a>Azure back-up maken

Azure back-up kunt back-ups van cloud met behulp van de vertrouwde back-programma's in Windows Server 2012 (of later), Windows Server 2012 Essentials (of hoger) en System Center 2012 Data Protection Manager (of hoger). Deze hulpprogramma's bieden een workflow voor back-up management die onafhankelijk is van de opslaglocatie van de back-ups, of een lokale schijf of Azure Storage. Nadat de back-up in de cloud, kunnen bevoegde gebruikers eenvoudig back-ups op een server herstellen.

Met incrementele back-ups, worden alleen de wijzigingen in bestanden overgebracht naar de cloud. Hiermee kunt efficiënt gebruik van opslagruimte, het beperken van bandbreedtegebruik en het punt-in-tijd herstel van meerdere versies van de gegevens. U kunt ook extra functies, zoals bewaarbeleid voor gegevens, compressie van gegevens en de overdracht van gegevens beperken. Azure gebruiken als locatie voor de back-up heeft het duidelijke voordeel dat de back-ups automatisch worden 'elders'. Hierdoor wordt de extra vereisten voor het beveiligen en beschermen ter plaatse back-media.

Zie voor meer informatie [Wat Azure back-up is?](../backup/backup-introduction-to-azure-backup.md) en [Azure back-up voor DPM gegevens configureren](https://technet.microsoft.com/library/jj728752.aspx).

##<a name="database"></a>Database

U kunt een noodgevallen hersteloplossing voor SQL Server-databases in een hybride-IT-omgeving met behulp van AlwaysOn beschikbaarheidsgroepen, spiegelen, logboekbestanden en back-up en herstellen met Azure-blobopslag. Al deze oplossingen SQL Server wordt uitgevoerd op Azure virtuele Machines gebruiken.

AlwaysOn beschikbaarheidsgroepen kunnen worden gebruikt in een hybride-IT-omgeving waar databasereplica beide on-premises implementatie bestaat en in de cloud. Dit wordt weergegeven in het volgende diagram.

![SQL Server AlwaysOn beschikbaarheid van groepen in een hybride cloud-architectuur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-3.png)

Spiegelen van de database kan ook beslaan on-premises implementatie-servers en de cloud in een installatie op basis van certificaten. Het volgende diagram wordt dit concept.

![SQL Server-database spiegelen in een hybride cloud-architectuur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-4.png)

Logboekbestanden kan worden gebruikt voor synchronisatie van een on-premises implementatie-database met een SQL Server-database op een Azure virtuele machine.

![Logboekbestanden van SQL Server in een hybride cloud-architectuur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-5.png)

Ten slotte, u kunt een back-up een lokale database rechtstreeks met Azure-blobopslag.

![Back-up van SQL Server met Azure-blobopslag in een hybride cloud-architectuur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-6.png)

Zie [hoge beschikbaarheid herstel voor SQL Server in Azure virtuele machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md) en [back-up en herstellen voor SQL Server in Azure virtuele machines](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md)voor meer informatie.

##<a name="checklists-for-on-premises-recovery-in-microsoft-azure"></a>Controlelijsten voor on-premises implementatie herstel van Microsoft Azure

###<a name="networking"></a>Netwerken

  1. Raadpleeg de sectie netwerken van dit document.
  2. Gebruik Virtual Network on-premises implementatie veilig verbinding te maken met de cloud.

###<a name="compute"></a>Berekenen

  1. Raadpleeg de sectie berekeningscluster van dit document.
  2. VMs verplaatsen tussen Hyper-V en Azure.

###<a name="storage"></a>Opslag

  1. Raadpleeg de sectie opslag van dit document.
  2. Maak gebruik van StorSimple services voor het gebruik van cloudopslag.
  3. Gebruik van de back-up van Azure-service.

###<a name="database"></a>Database

  1. Raadpleeg de sectie Database van dit document.
  2. Overweeg het gebruik van SQL Server op Azure VMs als de back-up.
  3. AlwaysOn beschikbaarheidsgroepen instellen.
  4. Configureer spiegelen van de database op basis van certificaten.
  5. Gebruik de logboekbestanden.
  6. Back-up on-premises implementatie-databases met Azure-blobopslag.

##<a name="next-steps"></a>Volgende stappen

In dit artikel maakt deel uit van een reeks die zijn gericht op [Azure tolerantie technische richtlijnen](./resiliency-technical-guidance.md). Het volgende artikel in deze reeks is [herstel van beschadiging of per ongeluk wordt verwijderd](./resiliency-technical-guidance-recovery-data-corruption.md).