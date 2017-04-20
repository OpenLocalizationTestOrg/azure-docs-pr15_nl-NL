<properties
  pageTitle="Azure en Linux VM opslagruimte | Microsoft Azure"
  description="Azure Standard en Premium opslagruimte met Linux virtuele machines wordt beschreven."
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
  ms.date="10/04/2016"
  ms.author="v-livech"/>

# <a name="azure-and-linux-vm-storage"></a>Azure en Linux VM opslag

Azure opslag is de cloud-opslag-oplossing voor moderne toepassingen die afhankelijk zijn van de levensduur, beschikbaarheid en schaalbaarheid om te voldoen aan de behoeften van hun klanten.  Naast het geboden die het mogelijk voor ontwikkelaars op grootschalige toepassingen voor de ondersteuning van nieuwe scenario's, Azure opslag ook kunt u het fundament opslag voor Azure virtuele Machines.

## <a name="azure-storage-standard-and-premium"></a>Azure opslag: Standard en Premium

Azure VM kan worden gebaseerd op standaard schijven of schijven voor premium.  Wanneer u met de Portal kiest u uw VM moet u een vervolgkeuzelijst op het scherm basisbeginselen standard en premium schijven weergeven of uitschakelen.  De onderstaande schermafbeelding wordt gemarkeerd dat wisselknop-menu.  Wanneer ingeschakeld naar SSD, alleen premium opslag ingeschakeld VMs worden weergegeven, wordt alle ondersteund door SSD stations.  Wanneer ingeschakeld op de harde schijf, ingeschakeld standaard opslag VMs back-ups ronddraait schijfstations worden weergegeven, samen met de premium-opslag VMs ondersteund door SSD.

  ![scherm1](../virtual-machines/media/virtual-machines-linux-azure-vm-storage-overview/screen1.png)

Bij het maken van een VM uit de `azure-cli` u kunt kiezen tussen standard en premium bij het kiezen van de grootte VM via de `-z` of `--vm-size` cli vlag.

### <a name="create-a-vm-with-standard-storage-vm-on-the-cli"></a>Een VM met standaard opslag VM op de cli maken

De vlag cli `-z` Standard_A1 kiest met A1 Linux VM wordt een standaard-opslag gebaseerd.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_A1
```

### <a name="create-a-vm-with-premium-storage-on-the-cli"></a>Een VM met premium opslagruimte op de cli maken

De vlag cli `-z` Standard_DS1 kiest met DS1 Linux VM een premium-opslag worden gebaseerd.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_DS1
```

## <a name="standard-storage"></a>Standaard opslag

Azure standaardopslag is het standaardtype opslag.  Standaard opslag is kosten effectieve terwijl deze wordt nog steeds worden zodat.  

## <a name="premium-storage"></a>Premium-opslag

Azure Premium opslagruimte biedt krachtige, lage latentie schijfondersteuning voor virtuele machines ik/O-intensief werkbelasting uitgevoerd. VM (VM) schijven die gebruikmaken van de Premium-opslag opslag van gegevens op effen staat stations (SSD). U kunt van uw toepassing VM schijven migreren met Azure Premium Storage om te profiteren van de snelheid en prestaties van deze schijven meenemen.

Opslag van Premiumfuncties:

- Schijven voor Premium: Azure Premium-opslag ondersteunt VM schijven die DS, DSv2 of GS reeks Azure VMs kunnen worden gekoppeld.

- Premium pagina Blob: Premium-opslag ondersteunt Azure pagina-BLOB's, die worden gebruikt voor permanente schijven voor Azure virtuele Machines (VMs).

- Premium lokaal redundante opslag: Een Premium-opslag-account alleen ondersteunt lokaal overtollige opslag (LRS) als de optie herhaling en drie kopieën van de gegevens binnen een enkel blijft.

- [Premium-opslag](../storage/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Premium opslag ondersteund VMs

Premium opslag ondersteunt DS-reeks, DSv2-reeks GS-reeks en Fs-reeks Azure virtuele Machines (VMs). U kunt zowel Standard en Premium schijven gebruiken met de Premium-opslag van VMs ondersteund. Maar u kunt schijven voor Premium niet gebruiken met VM reeks, die niet compatibel Premium-opslag.

Hier volgen de Linux onderzoeken die we gevalideerd met Premium opslagmedia.

| Verdeling | Versie                 | Ondersteunde Kernel    |
|--------------|-------------------------|---------------------|
| Ubuntu       | 12.04                   | 3.2.0-75.110+       |
| Ubuntu       | 14.04                   | 3.13.0-44.73+       |
| Debian       | 7.x, 8.x                | 3.16.7-ckt4-1+      |
| SLES         | SLES 12                 | 3.12.36-38.1+       |
| SLES         | SLES 11 SP4             | 3.0.101-0.63.1+     |
| CoreOS       | 584.0.0+                | 3.18.4+             |
| Centos       | 6.5, 6.6, 6,7, 7.0, 7.1 | 3.10.0-229.1.2.el7+ |
| RHEL         | 6,8 +, 7.2 +              |                     |


## <a name="file-storage"></a>Bestanden opslaan

Azure bestandsopslag biedt gedeelde bestanden in de cloud met behulp van de standaard SMB-protocol. U kunt bedrijfstoepassingen die afhankelijk van bestandsservers Azure zijn migreren met Azure-bestanden. Azure applicaties kunnen bestandsshares eenvoudig koppelen van Azure virtuele machines waarop Linux. En met de meest recente versie van bestandsopslag, kunt u ook een bestandsshare koppelen vanuit een on-premises implementatie-toepassing die ondersteuning biedt voor SMB 3.0.  Aangezien bestandsshares SMB aandelen, kunt u deze kunt openen via het standaard bestandssysteem API's.

Bestandsopslag is gebaseerd op dezelfde technologie als Blob, tabel en wachtrij opslag, zodat bestandsopslag biedt de beschikbaarheid, levensduur, schaalbaarheid en geografische-redundantie die is ingebouwd in het platform Azure opslag. Zie voor meer informatie over het bestand opslag prestaties doelen en limieten Azure opslag schaalbaarheid en prestaties doelen.

- [Het gebruik van Azure bestandsopslag met Linux](../storage/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Warm opslag

De laag Azure warm opslag is geoptimaliseerd voor het opslaan van gegevens die vaak wordt geopend.  Warm opslag is het standaardtype voor de opslag voor blob winkels.

## <a name="cool-storage"></a>Leuke opslag

De laag Azure leuke opslag is geoptimaliseerd voor het opslaan van gegevens die niet vaak worden geopend en lange levensduur. Voorbeeld van gebruik dozen voor indrukwekkende opslag zijn back-ups, media-inhoud, wetenschappelijke gegevens, naleving en archivering gegevens. Alle gegevens die zelden worden gebruikt, is in het algemeen, een perfecte keuze voor indrukwekkende opslag.

|                             | Warm opslag laag      | Leuke opslag laag     |
|:----------------------------|:---------------------:|:---------------------:|
| Beschikbaarheid                | 99,9%                 | 99%                   |
| Beschikbaarheid (AB-GRS lezen) | 99,99%                | 99,9%                 |
| Gebruikskosten               | Hogere kosten voor de opslag  | Lagere kosten voor de opslag   |
|                             | Lagere access          | Hogere access         |
|                             | en transactiekosten | en transactiekosten |


## <a name="redundancy"></a>Redundantie

De gegevens in uw account van Microsoft Azure opslag is altijd gerepliceerd om ervoor te zorgen levensduur en betere beschikbaarheid, de Azure opslag SLA zelfs vlak van problemen met de tijdelijke hardware de vergadering.

Wanneer u een opslag-account hebt gemaakt, moet u een van de volgende replicatieopties selecteren:

- Lokaal redundante opslag (LRS)
- Zone-redundante opslag (ZRS)
- Geografische-redundante opslag (GRS)
- Leestoegang geografische-redundante opslag (AB-GRS)

### <a name="locally-redundant-storage"></a>Lokaal redundante opslag

Lokaal redundante opslag (LRS) wordt overgenomen door uw gegevens in het gebied waarin u uw opslagruimte-account hebt gemaakt. Als u wilt maximaliseren levensduur, wordt elke aanvraag ten opzichte van gegevens in uw account opslag driemaal gerepliceerd. Deze drie replica's elke zijn opgeslagen in afzonderlijke foutenstructuuranalyse domeinen en de upgrade domeinen.  Een nieuw vergaderverzoek retourneert succes alleen zodra deze is geschreven op alle drie replica's.

### <a name="zone-redundant-storage"></a>Zone-redundante opslag

Zone-redundante opslag (ZRS) wordt overgenomen door uw gegevens over twee of drie faciliteiten, binnen een enkel of tussen twee regio's, hoger levensduur dan LRS leveren. Als uw account opslagruimte ZRS ingeschakeld heeft, zijn uw gegevens is duurzame zelfs wanneer een fout in een van de installaties.

### <a name="geo-redundant-storage"></a>Geografische-redundante opslag

Geografische-redundante opslag (GRS) wordt overgenomen door uw gegevens een secundaire gebied dat honderden mijl weg van het primaire regio. Als uw account opslagruimte GRS ingeschakeld heeft, zijn uw gegevens is duurzame zelfs in geval van een volledige regionale storing of een noodgevallen waarin de primaire regio kan niet worden hersteld.

### <a name="read-access-geo-redundant-storage"></a>Leestoegang geografische-redundante opslag

Leestoegang geografische-redundante opslag (AB-GRS) maximaal beschikbaar voor uw account opslag via de alleen-lezen toegang tot de gegevens in de tweede locatie, naast de replicatie tussen de twee regio's GRS gekregen. In het geval dat gegevens niet meer beschikbaar in de primaire regio, kan uw toepassing gegevens lezen uit de tweede regio.

Voor een uitgebreide Kennismaking in Azure opslag redundantie Zie:

- [Azure opslag herhaling](../storage/storage-redundancy.md)

## <a name="scalability"></a>Schaalbaarheid

Azure opslag is sterk schaalbare, zodat u kunt opslaan en proces honderden van TB aan gegevens ter ondersteuning van de grote gegevens scenario's wetenschappelijke, financiële analyse en mediatoepassingen vereist. Of u de kleine hoeveelheden gegevens die zijn vereist voor een website voor professionals en kleine bedrijven kunt opslaan. Waar uw behoeften vallen, betaalt u alleen voor de gegevens die u hebt opgeslagen. Azure opslagruimte momenteel worden opgeslagen tientallen trillions unieke klant-objecten en miljoenen aanvragen per seconde gemiddeld verwerkt.

Voor standaard opslag-accounts: een standaard-opslag-account heeft een maximum totale verzoek tarief van 20.000 IO's / s. De totale IO's / s over alle VM schijven in een standaard-opslag-account mag niet meer dan deze limiet.

Voor premium opslag-accounts: een premium-opslag-account heeft een totale maximumdoorvoer tarief van 50 GB/s. De totale doorvoer over alle VM schijven mag niet meer dan deze limiet.

## <a name="availability"></a>Beschikbaarheid

We garanderen dat ten minste 99,99% (99,9% voor indrukwekkende Access laag) van de tijd, we wordt verwerkt aanvragen voor het lezen van gegevens uit leestoegang-geografische redundante opslag (AB-GRS)-Accounts, mits mislukte pogingen om te lezen van gegevens uit het primaire regio opnieuw zijn uitgevoerd op de tweede regio.

We garanderen dat ten minste 99,9% (99% voor indrukwekkende Access laag) van de tijd, we wordt verwerkt aanvragen voor het lezen van gegevens uit lokaal overtollige opslag (LRS), Zone overtollige opslag (ZRS) en de geografische overtollige opslag (GRS)-Accounts.

We garanderen dat ten minste 99,9% (99% voor indrukwekkende Access laag) van de tijd, we wordt verwerkt aanvragen voor het schrijven van gegevens lokaal overtollige opslag (LRS), Zone overtollige opslag (ZRS), en geografische overtollige opslag (GRS)-Accounts en leestoegang-geografische redundante opslag (AB-GRS)-Accounts.

- [Azure SLA voor opslag](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)


## <a name="regions"></a>Regio 's

Azure algemeen beschikbaar is in 30 regio's overal ter wereld en abonnementen voor 4 extra gebieden heeft aangekondigd. Geografische uitbreiding is een prioriteit voor Azure omdat deze kan onze klanten betere prestaties en deze ondersteuning voor hun vereisten en voorkeuren over gegevenslocatie.  De meest recente regio azures aan starten is in Duitsland.

De Microsoft Cloud-Duitsland biedt een gesplitste optie om de Microsoft Cloud services al beschikbaar in heel Europa, verbeterde mogelijkheden voor vernieuwing en groei voor ten zeerste gereglementeerde partners en klanten in Duitsland, de Europese Unie (EU) en de Europese Vrijhandelsassociatie (EVA) maken.

Gegevens van de klant in deze nieuwe datacenters, in Magdeburg en Frankfurt, wordt onder de besturing van een beheerder van de gegevens, T-systemen internationale, een onafhankelijke Duitse bedrijf en vestiging van Deutsche Telekom beheerd. Commerciële cloudservices van Microsoft in deze datacenters voldoen aan Duitse gegevens voor de verwerking van overheidswege en geef klanten meer keuzemogelijkheden van hoe en waar gegevens worden verwerkt.


- [Azure regio's toewijzen](https://azure.microsoft.com/regions/)

## <a name="security"></a>Beveiliging

Azure opslagruimte biedt een uitgebreide set van beveiligingsmogelijkheden voor waarmee samen ontwikkelaars op beveiligde toepassingen. De opslag-account zelf kan worden beveiligd met toegangsbeheer op basis van rollen en Azure Active Directory. Gegevens kunnen worden beveiligd tijdens overdracht tussen een toepassing en Azure met behulp van aan de clientzijde versleuteling, HTTPS of SMB 3.0. Gegevens kan worden ingesteld op automatisch worden gecodeerd als geschreven met Azure Storage met opslag Service versleuteling (SSE). OS en gegevens schijven die worden gebruikt door virtuele machines kunnen worden ingesteld te coderen met Azure schijfversleuteling. Gedelegeerd toegang tot de gegevensobjecten in Azure-opslag kan worden verleend gedeeld Access handtekeningen gebruiken.

### <a name="management-plane-security"></a>Management vlak beveiliging

Het vlak van management bestaat uit de resources die worden gebruikt voor het beheren van uw account opslag. In dit gedeelte bespreken we het implementatiemodel van Azure resourcemanager en het gebruik van Rolgebaseerd toegangsbeheer (RBAC) toegang tot uw accounts opslag beheren. We wordt ook overleggen over het beheren van uw opslagruimte account sleutels en hoe u ze opnieuw te genereren.

### <a name="data-plane-security"></a>Beveiliging van gegevens vlak

In deze sectie, zullen we er toegang tot de werkelijke gegevens-objecten in uw opslag-account, zoals BLOB's, bestanden, wachtrijen en tabellen, met Access handtekeningen gedeeld en Clienttoegangsbeleid die zijn opgeslagen. Aan bod komen zowel service level SA's en -account op gebruikersniveau SA's. We ziet er ook het beperken van toegang tot een specifiek IP-adres (of een bereik van IP-adressen), hoe u beperkingen instellen voor het protocol dat wordt gebruikt in HTTPS en hoe een handtekening gedeeld toegang intrekken zonder te wachten op verlopen.

## <a name="encryption-in-transit"></a>Versleuteling tijdens overdracht

In deze sectie wordt beschreven hoe om gegevens te beveiligen wanneer u deze in- of uitzoomen op Azure Storage overbrengen. Bespreken we de aanbevolen gebruik van HTTPS en de versleuteling van SMB 3.0 voor bestandsshares Azure. We wordt ook kost, bekijkt aan de clientzijde versleuteling, waarmee u de gegevens versleutelen voordat deze wordt overgebracht op te slaan in een clienttoepassing en tot de versleutelde gegevens na het overbrengen geen opslagruimte meer.

## <a name="encryption-at-rest"></a>Versleuteling in rust

We wordt communiceren over opslag Service-versleuteling (SSE), en hoe u deze inschakelen voor een account opslag, wat resulteert in uw blok BLOB's, pagina BLOB's, en BLOB's automatisch gecodeerde geschreven met Azure Storage toevoegt. We gaan ook bekijken hoe u Azure-schijf versleuteling en de eenvoudige verschillen en dozen van schijfversleuteling versus SSE versus aan de clientzijde versleuteling verkennen. We bekijkt FIPS-naleving voor Amerikaanse overheid computers kort.

- [Azure handleiding voor opslag-beveiliging](../storage/storage-security-guide.md)

## <a name="cost-savings"></a>Kosten besparen

- [Opslag kosten](https://azure.microsoft.com/pricing/details/storage/)

- [Opslag van kosten Rekenmachine](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Opslaglimieten

- [Opslaglimieten voor de Service](../azure-subscription-service-limits.md#storage-limits)
