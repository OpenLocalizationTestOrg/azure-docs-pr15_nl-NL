 <properties
   pageTitle="Azure en Linux | Microsoft Azure"
   description="Beschrijving van Azure berekenen, opslag en netwerken services met Linux virtuele machines."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines-linux"
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/14/2016"
   ms.author="v-livech"/>

# <a name="azure-and-linux"></a>Azure en Linux

Microsoft Azure is een groeiende verzameling geïntegreerde openbare cloud services zoals analytics, virtuele Machines databases, mobiele, netwerken, opslag, en het web, ideaal voor het hosten van uw oplossingen.  Microsoft Azure biedt een scalable platform waarmee u betaalt alleen voor wat u gebruikt, wanneer u dat - zonder dat hoeft te investeren in on-premises implementatie hardware.  Azure is gereed wanneer u bent uw oplossingen om uit te breiden en af ongeacht schaalfactor u nodig hebt om de behoeften van uw klanten.

Als u bekend met de verschillende functies van Amazon bent AWS, kunt u het Azure tegenover AWS [definitie toewijzing document](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/)controleren.


## <a name="regions"></a>Regio 's
Microsoft Azure resources zijn verdeeld over meerdere geografische regio's overal ter wereld.  Een "regio" staat voor meerdere datacenters in één geografisch gebied.  Vanaf 1 januari 2016, deze groep omvat: 8 in 2 in Europa, 6 in Azië en stille, 2 in vaste land van China en 3 in India-Amerika.  Als u een volledige lijst van alle Azure regio's wilt, kunt u het onderhouden dat een lijst van bestaande en nieuwe aangekondigd regio's.

- [Azure regio 's](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Beschikbaarheid
In de volgorde voor uw implementatie in aanmerking komt voor onze 99.95 VM Service Level Agreement, moet u implementeren twee of meer VMs uitgevoerd van uw werkzaamheden binnen een beschikbaarheid instellen. Dit zorgt ervoor dat uw VMs zijn verdeeld over meerdere foutenstructuuranalyse domeinen in onze datacenters, evenals geïmplementeerd op hosts met verschillende onderhoud windows. De volledige [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) wordt uitgelegd dat de gegarandeerd beschikbaarheid van Azure als geheel.

## <a name="azure-virtual-machines--instances"></a>Azure virtuele Machines & exemplaren
Microsoft Azure ondersteunt de uitvoering van een aantal populaire Linux onderzoeken geleverd en beheerd door een aantal partners.  U vindt onderzoeken zoals rood rol Enterprise, CentOS, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD en meer in de Azure Marketplace. We werken actief met verschillende Linux-community's nog meer typen toevoegen aan de lijst [dat Azure Linux Distros goedgekeurd](virtual-machines-linux-endorsed-distros.md) .

Als uw voorkeur Linux distro van keuze niet in de galerie momenteel aanwezig is, kunt u 'overbrengen uw eigen Linux"VM door te [maken en uploaden van een VHD Linux in Azure wordt aangegeven](virtual-machines-linux-create-upload-generic.md).

Azure virtuele machines kunt u een groot aantal oplossingen computing in een agile manier implementeren. U kunt implementeren vrijwel elke werkbelasting en een taal op vrijwel elk besturingssysteem - Windows, Linux, of een aangepaste hebt gemaakt via een van onze groeiende lijst met partners. Nog steeds niet ziet wat u zoekt?  Geen probleem: u kunt ook uw eigen afbeeldingen vanuit on-premises implementatie overbrengen.

## <a name="vm-sizes"></a>VM grootte
Wanneer u een VM in Azure implementeert, gaat u om de grootte van een VM binnen een van onze reeks grootte die geschikt is voor uw werkzaamheden te selecteren. De grootte heeft ook gevolgen voor de verwerking power, geheugen en opslag de capaciteit van de virtuele machine. Afschrijving op basis van de hoeveelheid tijd die de VM wordt uitgevoerd en gebruik de toegewezen resources. Een volledige lijst van de [grootte van virtuele Machines](virtual-machines-linux-sizes.md).

Hier volgen enkele eenvoudige richtlijnen voor het selecteren van een VM-grootte van een van onze reeks (A, D, DS, G en GS).

* A-reeks VMs zijn onze waarde prijs op instapniveau VMs voor light werkbelasting en ontwikkelaar/testen scenario's. Ze kunnen zijn algemeen beschikbaar in alle regio's en verbinding maken en gebruiken van alle standaard resources beschikbaar zijn voor virtuele machines.
* A-reeks grootte (A8 - A11) zijn speciale berekeningscluster intensief configuraties geschikt voor high-performance computing clustertoepassingen.
* D-reeks VMs zijn ontworpen om uit te voeren toepassingen die hoger rekenkracht en prestaties van de tijdelijke schijf. D-reeks VMs bieden sneller processors, een hogere geheugen-naar-core-breedteverhouding en een solid-state drive (SSD) voor de tijdelijke schijf.
* Dv2-reeks, is de nieuwste versie van onze D-reeks, een krachtiger CPU functies. De CPU Dv2-reeks is ongeveer 35% sneller dan de CPU D-reeks. Dit is gebaseerd op de nieuwste 2,4 GHz Intel Xeon® E5-2673 v3 processor (Haskell), en met de Intel Turbo Microfoonversterking technologie 2.0, gaat u kunt maximaal 3,2 GHz. De Dv2-reeks heeft de dezelfde geheugen en schijfruimte configuraties als de D-reeks.
* G-serie VMs bieden de meeste geheugen en worden uitgevoerd op hosts met Intel Xeon E5 V3 family processors.

Opmerking: DS-reeks en GS-reeks VMs hebben toegang tot Premium opslag - onze SSD back krachtige, lage latentie opslag voor i/o-intensief werkbelasting. Premium opslag is beschikbaar in bepaalde regio's. Zie voor meer informatie:

- [Premium-opslag: High-performance opslag voor Azure virtuele machines werkbelasting](../storage/storage-premium-storage.md)

## <a name="automation"></a>Automatisering
Als u wilt bereiken een juiste DevOps cultuur, moeten alle infrastructuur code.  Wanneer alle de infrastructuur worden opgeslagen in de code die eenvoudig mogelijk opnieuw gemaakt (Phoenix Servers).  Azure werkt met de belangrijkste automatisering gereedschap zoals Ansible, Chef, SaltStack en Puppet.  Azure heeft ook een eigen tooling voor automatisering:

- [Azure-sjablonen](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

- [Azure VMAccess](virtual-machines-linux-using-vmaccess-extension.md)

Azure is via de meeste Linux Distros die ondersteuning bieden voor het implementeren van ondersteuning voor [cloud initialisatie](http://cloud-init.io/) .  Momenteel zijn van Canonical Ubuntu VMs geïmplementeerd met cloud initialisatie standaard ingeschakeld.  RedHats RHEL, CentOS en Fedora cloud initialisatie ondersteund, worden echter de Azure afbeeldingen beheerd door RedHat heb cloud initialisatie is geïnstalleerd.  Als u wilt gebruiken cloud initialisatie op een huishouden RedHat OS, moet u een aangepaste afbeelding maken met de cloud initialisatie is geïnstalleerd.

- [Cloud initialisatie op Azure Linux VMs gebruiken](virtual-machines-linux-using-cloud-init.md)

## <a name="quotas"></a>Quota 's
Elk abonnement Azure heeft standaard quotumlimiet in de locatie waarop van invloed op de implementatie van een groot aantal VMs voor uw project zijn kan. De huidige limiet op basis van de per abonnement is 20 VMs per regio.  Quotumlimiet kunnen worden verhoogd door het indienen van een ondersteuningsticket aanvragen van een stijging van de limiet.  Voor meer informatie over quotumlimiet:

- [Azure-Service-abonnementen](../azure-subscription-service-limits.md)


## <a name="partners"></a>Partners

Microsoft nauw met onze partners om ervoor te zorgen de afbeeldingen die beschikbaar zijn bijgewerkt en geoptimaliseerd voor een Azure runtime.  Controleer de onderstaande marketplace-'s voor meer informatie over onze partners.

- [Linux op Azure ondersteunde onderzoeken](virtual-machines-linux-endorsed-distros.md)

RedHat - [Azure Marketplace - RedHat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)

Canonieke - [Azure Marketplace - Ubuntu Server 16.04 LTS](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)

Debian - [Azure Marketplace - Debian 8 "Jessie"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)

FreeBSD - [Azure Marketplace - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)

CoreOS - [Azure Marketplace - CoreOS (stabiele)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)

RancherOS - [Azure Marketplace - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)

Bitnami - [Bitnami-bibliotheek voor Azure](https://azure.bitnami.com/)

Mesosphere - [Azure Marketplace - Mesosphere domeincontroller/OS op Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)

Docker - [Azure Marketplace - Service van Azure Container met Docker Swarm](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)

Jenkins - [Azure Marketplace - CloudBees Jenkins Platform](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)


## <a name="getting-setup-on-azure"></a>Aan de instelling op Azure
Als u wilt gebruiken Azure moet u een Azure-account, de Azure CLI geïnstalleerd en een paar SSH openbare en persoonlijke sleutels.

## <a name="sign-up-for-an-account"></a>Registreren voor een account
De eerste stap bij het gebruik van de Cloud Azure is te registreren voor een Azure-account.  Ga naar de aanmeldingspagina van [Azure Account](https://azure.microsoft.com/pricing/free-trial/) aan de slag.

## <a name="install-the-cli"></a>De CLI installeren
Met uw nieuwe Azure-account, u kunt aan de slag direct met behulp van de Azure portal, dat wil een deelvenster admin op het web zeggen.  Als u wilt beheren in de Cloud Azure via de opdrachtregel, installeert u de `azure-cli`.  Het installeren van de [Azure CLI ](../xplat-cli-install.md)op uw Mac of Linux-werkstation.

## <a name="create-an-ssh-key-pair"></a>Een combinatie van SSH-sleutel maken
U hebt nu een Azure-account, de webportal Azure en de CLI Azure.  De volgende stap is het opzetten van een combinatie van SSH sleutel die wordt gebruikt voor SSH in Linux zonder met een wachtwoord.  [Maak SSH toetsen op Linux en Mac](virtual-machines-linux-mac-create-ssh-keys.md) wachtwoord minder aanmeldingen en een betere beveiliging kunt inschakelen.

## <a name="getting-started-with-linux-on-microsoft-azure"></a>Aan de slag met Linux op Microsoft Azure
Met de configuratie van Azure-account, de Azure CLI geïnstalleerd en SSH toetsen gemaakt bent u nu klaar om te beginnen met het samenstellen van een infrastructuur in de Cloud Azure.  De eerste taak is het opzetten van een aantal VMs.

## <a name="create-a-vm-using-the-cli"></a>Een VM met de CLI maken
Maken van een Linux VM met de CLI is een snelle manier om te implementeren van een VM zonder de terminal die u werkt.  Alles die kunt u op de webportal is beschikbaar via een opdrachtregel vlag of veranderen.  

- [Een Linux VM met de CLI maken](virtual-machines-linux-quick-create-cli.md)

## <a name="create-a-vm-in-the-portal"></a>Een VM maken in de portal
Een VM Linux maken in de webportal Azure, is een manier om eenvoudig wijzen en op door de verschillende opties om een implementatie.  In plaats van de opdrachtregel vlaggen of schakelopties gebruikt, bent u kunnen een nette webindeling van verschillende opties en instellingen weergeven.  Alles beschikbaar via de opdrachtregel is ook beschikbaar in de portal.

- [Een Linux VM met behulp van de Portal maken](virtual-machines-linux-quick-create-portal.md)

## <a name="login-using-ssh-without-a-password"></a>Meld u aan met SSH zonder een wachtwoord
De VM wordt nu uitgevoerd op Azure en u klaar bent voor het aanmelden.  Gebruik van wachtwoorden aanmelden via SSH is onveilig en tijd in beslag nemen.  Met toetsen SSH is de meest veilige manier en ook de snelste manier om aan te melden.  Wanneer u u Linux VM via de portal of de CLI maakt, hebt u twee verificatie-opties.  Als u een wachtwoord voor SSH kiest, configureert Azure de VM zodat aanmeldingen via wachtwoorden.  Als u ervoor hebt gekozen voor het gebruik van een openbare sleutel SSH,-Azure de VM als u wilt dat alleen aanmeldingen via SSH toetsen configureert en wachtwoord aanmeldingen uitgeschakeld. Als u wilt beveiligen uw VM Linux door alleen SSH belangrijke aanmeldingen toe te staan, gebruikt u de SSH openbare sleutel tijdens het VM maken in de portal of CLI.

- [Wachtwoorden voor SSH op uw VM Linux uitschakelen door te SSHD configureren](virtual-machines-linux-mac-disable-ssh-password-usage.md)

## <a name="related-azure-components"></a>Verwante Azure onderdelen

## <a name="storage"></a>Opslag

- [Inleiding tot Microsoft Azure-opslag](../storage/storage-introduction.md)

- [Een schijf toevoegen aan een Linux VM gebruik van de azure-cli](virtual-machines-linux-add-disk.md)

- [Hoe u een gegevensschijf koppelt aan een VM Linux in de portal van Azure](virtual-machines-linux-attach-disk-portal.md)

## <a name="networking"></a>Netwerken

- [Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)

- [IP-adressen in Azure wordt aangegeven](../virtual-network/virtual-network-ip-addresses-overview-arm.md)

- [Poorten openen voor een VM Linux in Azure wordt aangegeven](virtual-machines-linux-nsg-quickstart.md)

- [Een volledig gekwalificeerde domeinnaam maken in de portal van Azure](virtual-machines-linux-portal-create-fqdn.md)


## <a name="containers"></a>Containers

- [Virtuele Machines en Containers in Azure wordt aangegeven](virtual-machines-linux-containers.md)

- [Azure Container Service-Inleiding](../container-service/container-service-intro.md)

- [Een cluster Azure Container-Service implementeren](../container-service/container-service-deployment.md)

## <a name="next-steps"></a>Volgende stappen

U hebt nu een overzicht van Linux op Azure.  De volgende stap wordt u meteen aan de slag en maakt een paar VMs!

- [Maak een VM Linux op Azure met behulp van de Portal](virtual-machines-linux-quick-create-portal.md)

- [Een VM Linux op Azure maken met behulp van de CLI](virtual-machines-linux-quick-create-cli.md)
