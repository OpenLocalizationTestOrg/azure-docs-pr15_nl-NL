<properties
    pageTitle="VMware virtuele machines en fysieke servers repliceren naar Azure met Azure Site herstel (verouderd) | Microsoft Azure" 
    description="Wordt beschreven hoe u de on-premises implementatie VMs repliceren en Windows/Linux fysieke servers Azure Azure Site herstel gebruiken in een oudere implementatie in de klassieke portal." 
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery-using-the-classic-portal-legacy"></a>VMware virtuele machines en fysieke servers repliceren naar Azure met Azure Site herstel met behulp van de klassieke portal (verouderd)

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmware-to-azure.md)
- [Klassieke Portal](site-recovery-vmware-to-azure-classic.md)
- [Klassieke-Portal (verouderd)](site-recovery-vmware-to-azure-classic-legacy.md)


Welkom bij Azure Site herstel! In dit artikel worden de implementatie van een oudere voor het on-premises implementatie VMware virtuele machines of Windows/Linux fysieke servers repliceren naar Azure Azure Site herstel gebruiken in de klassieke portal.

## <a name="overview"></a>Overzicht

Organisaties moeten een BCDR strategie die bepaalt hoe apps, werkbelasting en gegevens blijven actief en beschikbaar tijdens geplande en ongeplande downtime en herstellen naar de normale arbeidsvoorwaarden zo snel mogelijk. Uw strategie BCDR moet u bedrijfsgegevens veilige en worden hersteld en ervoor te zorgen dat werkbelasting continu beschikbaar blijven wanneer noodgevallen plaatsvindt.

Herstel van de site is een Azure-service vergeleken met uw strategie BCDR door orchestrating herhaling van fysieke servers on-premises implementatie en virtuele machines in de cloud (Azure) of met een secundaire datacenter. Wanneer er bijvoorbeeld optreden in uw hoofdlocatie bent, u niet de secundaire locatie wilt behouden apps en werkbelasting beschikbaar. U terug naar uw hoofdlocatie bent niet en er wordt naar de normale bewerkingen. Klik hier als u meer wilt weten in [Wat Azure Site herstel is?](site-recovery-overview.md)


>[AZURE.WARNING] In dit artikel bevat **oudere instructies**. Niet, wordt deze gebruiken voor nieuwe implementaties. In plaats daarvan [Volg deze instructies](site-recovery-vmware-to-azure.md) om te implementeren herstel van de Site in het Azure-portal, of [gebruikt u deze instructies](site-recovery-vmware-to-azure-classic.md) voor het configureren van de uitgebreide implementatie in de klassieke portal. Als u hebt al geïmplementeerd met behulp van de methode die in dit artikel worden beschreven, wordt u aangeraden dat u naar de uitgebreide implementatie in de klassieke portal migreren.


## <a name="migrate-to-the-enhanced-deployment"></a>Migreren naar de uitgebreide implementatie

In dit gedeelte is alleen van toepassing als u hebt al geïmplementeerd Site herstel volgens de instructies in dit artikel.

Uw bestaande implementatie, u moet migreren:

1. Nieuwe Site herstel components implementeren in uw on-premises site.
2. Referenties voor automatische detectie van VMware VMs configureren op de nieuwe configuratieserver.
3. Ontdek de VMware-servers met de nieuwe configuratieserver.
3. Maak een nieuwe groep voor de beveiliging met de nieuwe configuratieserver.


Voordat u begint:

- Het is raadzaam dat u een onderhoudsvenster voor migratie instellen.
- De optie **Machines migreren** is alleen beschikbaar als u bestaande beveiliging groepen die zijn gemaakt tijdens een oudere implementatie hebt.
- Als u klaar bent met het migratiestappen kan duren 15 minuten of langer voor het vernieuwen van de referenties, en voor ontdekken en virtuele machines vernieuwen, zodat u ze aan een groep beveiliging toevoegen kunt. U kunt handmatig vernieuwen in plaats van wachten. 

Migreren als volgt:

1. Lees meer over [uitgebreide implementatie in de klassieke portal](site-recovery-vmware-to-azure-classic.md#enhanced-deployment). Bekijk de uitgebreide [architectuur](site-recovery-vmware-to-azure-classic.md#scenario-architecture)en [vereisten](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment).
2. De service mobiliteit van machines die u momenteel bent repliceren verwijderen. Een nieuwe versie van de service wordt geïnstalleerd op de computers wanneer u deze aan de nieuwe groep voor de beveiliging toevoegt.
3. Download een [kluis registratiegegevens sleutel](site-recovery-vmware-to-azure-classic.md#step-4-download-a-vault-registration-key) en [Voer de installatiewizard van geïntegreerd](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server) voor het installeren van de configuratieserver, processerver en onderdelen van de basispagina doel-server. Meer informatie over het [plannen van capaciteit](site-recovery-vmware-to-azure-classic.md#capacity-planning).
4. [Referenties instellen](site-recovery-vmware-to-azure-classic.md#step-6-set-up-credentials-for-the-vcenter-server) die herstel Site kunt gebruiken voor toegang tot VMware server automatisch detecteren VMware VMs. Meer informatie over de [vereiste machtigingen](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
5. Voeg [vCenter servers of vSphere hosts](site-recovery-vmware-to-azure-classic.md#step-7-add-vcenter-servers-and-esxi-hosts). Het kan duren 15 minuten voor meer informatie voor servers in de portal-Site herstel moet worden weergegeven.
6. Maak een [nieuwe groep voor beveiliging](site-recovery-vmware-to-azure-classic.md#step-8-create-a-protection-group). Het kunt maximaal 15 minuten duren voordat de portal om te vernieuwen, zodat de virtuele machines zijn gevonden en wordt weergegeven. Als u niet wilt wachten voordat u de servernaam van management kunt markeren (Klik niet op deze) > **vernieuwen**.
7. Klik onder de nieuwe groep voor de beveiliging op **Machines migreren**.

    ![Account toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration1.png)

8. Selecteer de groep van de beveiliging die u migreren wilt van in **Machines selecteren** en de machines die u wilt migreren.

    ![Account toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration2.png)

9. Opgeven of u wilt dezelfde instellingen gebruiken voor alle computers en schakelt u het processerver en Azure opslag-account in **De doel-instellingen configureren** . Als u niet over een server als afzonderlijk proces dit is de het IP-adres van de configuratieserver voor de server.


    ![Account toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration3.png)

    > [AZURE.NOTE] [Migratie van opslag accounts](../resource-group-move-resources.md) via resourcegroepen binnen hetzelfde abonnement of via abonnementen wordt niet ondersteund voor opslag-accounts gebruikt voor het implementeren van sites worden hersteld.

10. Selecteer het account dat u hebt gemaakt voor de processerver voor toegang tot de machine voor de nieuwe versie van de service mobiliteit push in **Accounts opgeven**.

    ![Account toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration4.png)

11. Herstel van de site wordt uw gerepliceerde gegevens migreren naar de opslag van Azure-account die u hebt opgegeven. Desgewenst kunt u het opslag-account waarmee u in de oudere implementatie opnieuw gebruiken.
12. Nadat de taak is voltooid wordt virtuele machines worden automatisch gesynchroniseerd. Nadat de synchronisatie is voltooid, kunt u de virtuele machines verwijderen uit de groep oudere beveiliging.
13. Als alle computers hebt gemigreerd kunt u de groep oudere beveiliging verwijderen.
14. Onthoud dat de failover-eigenschappen voor machines en de Azure netwerkinstellingen opgeven wanneer synchronisatie voltooid is.
15. Als u bestaande herstel abonnementen hebt, kunt u ze kunt migreren naar de uitgebreide-implementatie waarbij de optie voor het **Plannen van herstel migreren** . U moet dit alleen doen nadat alle beveiligde computers zijn gemigreerd. 

    ![Account toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration5.png)

>[AZURE.NOTE] Nadat u klaar bent met migratie gaat u verder met de [Verbeterde artikel](site-recovery-vmware-to-azure-classic.md). De rest van dit artikel oudere wordt niet meer relevant zijn en u niet nodig hebt om te volgen meer van de stappen in it **.




## <a name="what-do-i-need"></a>Wat heb ik nodig?

In dit diagram ziet u de implementatie-onderdelen.

![Nieuwe kluis](./media/site-recovery-vmware-to-azure-classic-legacy/architecture.png)

Hier volgt wat u nodig hebt:

**Onderdeel** | **Implementatie** | **Meer informatie**
--- | --- | ---
**Configuratieserver voor** | Een Azure standaard A3 virtuele machine in hetzelfde abonnement als sites worden hersteld. | De configuratie server coördinaten communicatie tussen beveiligde machines, het processerver en basispagina doelservers in Azure wordt aangegeven. Replicatie en ingesteld coördinaten herstel van Azure bij een storing.
**Basispagina doelserver** | Een Azure virtuele machines, op een Windows-server op basis van een afbeelding van de galerie met Windows Server 2012 R2 (naar het Windows-computers beveiligen) of als een Linux-server op basis van een afbeelding OpenLogic CentOS 6.6 (om te beschermen Linux machines).<br/><br/> Drie hoekformaatgreep opties zijn beschikbaar: standaard A4, standaard D14 en standaard DS4.<br/><br/> De server is verbonden met hetzelfde Azure netwerk als de configuratieserver.<br/><br/> U hebt ingesteld in de portal-Site herstellen | Deze ontvangt, en behoudt gerepliceerde gegevens uit uw beveiligde machines met bijgevoegde VHD's in uw account voor de opslag van Azure-blobopslag voor u heeft gemaakt.<br/><br/> Selecteer standaard DS4 specifiek voor het configureren van beveiliging voor werkbelasting vereisen van consistente krachtige en lage latentie via Premium opslag-Account.
**Processerver** | Een on-premises implementatie virtuele of fysieke server met Windows Server 2012 R2<br/><br/> Het is raadzaam om deze op de hetzelfde netwerk en LAN-segment wordt geplaatst als de machines die u wilt beveiligen, maar deze kan worden uitgevoerd op een ander netwerk zo lang maken als beveiligde machines hebben L3 netwerk zichtbaarheid toe.<br/><br/> U stelt u dit en op de configuratieserver in de portal-Site herstel registeren. | Replicatiegegevens verzenden beveiligde machines naar de on-premises implementatie proces-server. Er wordt een schijf gebaseerde cache-cache replicatie-gegevens die afkomstig zijn. Een aantal acties uitvoeren op gegevens.<br/><br/> Deze optimaliseert gegevens door caching, comprimeren en versleutelen voordat dit naar de basispagina doelserver verzonden.<br/><br/> Het verwerkt push-installatie van de Service mobiliteit.<br/><br/> Automatische detectie van VMware virtuele machines uitvoeren.
**On-premises implementatie machines** | On-premises implementatie VMware virtuele machines of fysieke servers met Windows- of Linux. | U configureren replicatie-instellingen die van toepassing zijn een of meer computers. U kunt via een afzonderlijke machine of vaker voorkomt, niet meerdere computers die u in een herstel-indeling verzamelen. 
**Mobiliteit-service** | Geïnstalleerd op elke VM of fysieke server die u wilt beveiligen<br/><br/> Kan worden geïnstalleerd handmatig of gedrukt en automatisch door de processerver wordt geïnstalleerd wanneer u replicatie voor een machine inschakelen. | De service mobiliteit verzendt gegevens naar de processerver tijdens de eerste replicatie (synchroniseren). Nadat de computer een beveiligde status is (nadat synchroniseren is voltooid) wordt de service mobiliteit wordt vastgelegd schrijven naar schijf in het geheugen en verzendt deze naar de processerver. Applicationconsistency voor Windows-servers wordt bereikt VSS. gebruiken
**Azure Site herstel kluis** | U maakt een kluis herstel van de Site met een Azure-abonnement en servers registreren in de kluis. | De kluis coördinaten en orchestrates repliceren, failover en herstel tussen uw on-premises-site en Azure.
**Replicatie om** | **Via Internet**, communicatie en gerepliceerd gegevens uit lokale beveiligde servers Azure met beveiligd SSL/TLS-kanaal via internet. Dit is de standaardoptie.<br/><br/> **VPN/ExpressRoute**, communicatie en gerepliceerd gegevens tussen servers van de on-premises implementatie en Azure via een VPN-verbinding. U moet een VPN-verbinding voor de site-naar-site of een ExpressRoute verbinding tussen uw on-premises-site en Azure netwerk instellen.<br/><br/> U moet selecteren hoe u wilt repliceren tijdens de implementatie van sites worden hersteld. U kunt de om niet meer wijzigen nadat deze geconfigureerd zonder die invloed hebben op replicatie van bestaande machines. | Geen van beide optie moet u geen binnenkomende netwerkpoorten op beveiligde computers te openen. Alle netwerkcommunicatie is gestart vanaf de on-premises implementatie-site. 

## <a name="capacity-planning"></a>Capaciteit plannen

De belangrijkste gebieden die u moet u rekening moet houden:

- **Bron-omgeving**, het VMware infrastructuur, instellingen voor gegevensbron machine en vereisten.
- **Componentservers**, het processerver, configuratieserver en basispagina doelserver 

### <a name="considerations-for-the-source-environment"></a>Aandachtspunten voor de bronomgeving

- **Maximale grootte van de schijf**, de huidige maximale grootte van de schijf die kan worden gekoppeld aan een virtuele machine is 1 TB. De maximale grootte van een bronschijf die kan worden gerepliceerd is dus ook maximaal 1 TB.
- **Maximale grootte per bron**, de maximale grootte van een machine één bron is 31 TB (met 31 schijven) en met een D14 exemplaar deze is ingericht voor de basispagina doelserver. 
- **Aantal bronnen per basispagina doelserver**, meerdere bron computers kunnen worden beveiligd met een één basispagina doelserver. Een machine één gegevensbron niet kan echter niet worden beveiligd over meerdere basispagina doelservers, omdat tijdens schijven repliceren, een VHD die gelijk is aan de grootte van de schijf is gemaakt op Azure-blobopslag en die als een gegevensschijf aan de basispagina doelserver zijn gekoppeld.  
- **Maximum dagelijks tarieven per gegevensbron wijzigen**, zijn er drie factoren die worden beschouwd als moeten wanneer u de aanbevolen wijziging tarieven per bron. Voor de aandachtspunten voor de doeltoepassing op basis van twee IO's / s nodig op de doelschijf voor elke bewerking op de bron. Dit is omdat het lezen van oude gegevens en het schrijven van de nieuwe gegevens op de doelschijf gebeurt. 
    - **Dagelijks snelheid wordt ondersteund door de processerver wijzigen**, een bron-machine niet meerdere omvatten meerdere proces-servers. Een enkel proces-server kan maximaal 1 TB van de snelheid van dagelijkse wijzigen ondersteunen. 1 TB is dus dat de maximale dagelijkse gegevens tarieven die worden ondersteund voor een machine bron wijzigen. 
    - **Maximum doorvoer wordt ondersteund door de do not use**-Maximum lospeuteren per bronschijf mag niet meer dan 144 GB/dag (met 8 kB schrijven grootte). Zie de tabel in de doelsectie basispagina voor de doorvoer en IO's / s van het doel voor verschillende grootten schrijven. Dit nummer moet worden gedeeld door twee omdat elke bron i 2 IO's / s op de doelschijf genereert. Lees meer over [Azure doelen van schaalbaarheid en prestaties](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) tijdens het configureren van het doel voor premium opslag-accounts.
    - **Maximum doorvoer wordt ondersteund door het account opslag**, een bron niet meerdere omvatten meerdere opslag-accounts. Gegeven dat een account opslag duurt maximaal 20.000 aanvragen per seconde en dat elke bron i 2 IO's / s op de basispagina doelserver gegenereerd, wordt aangeraden dat u het aantal IO's / s over de bron behouden en 10.000. Lees meer over [Azure doelen van schaalbaarheid en prestaties](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) tijdens het configureren van de bron voor premium opslag-accounts.

### <a name="considerations-for-component-servers"></a>Overwegingen voor componentservers

Tabel 1 bevat een overzicht van de VM-grootte voor de configuratie en basispagina doelservers.

**Onderdeel** | **Geïmplementeerd Azure exemplaren** | **Cores** | **Geheugen** | **Max schijven** | **Grootte van de schijf**
--- | --- | --- | --- | --- | ---
Configuratieserver voor | Standaard A3 | 4 | 7 GB | 8 | 1023 GB
Basispagina doelserver | Standaard A4 | 8 | 14 GB | 16 | 1023 GB
 | Standaard D14 | 16 | 112 GB | 32 | 1023 GB
 | Standaard DS4 | 8 | 28 GB | 16 | 1023 GB

**Tabel 1**

#### <a name="process-server-considerations"></a>Aandachtspunten voor het verwerken

In het algemeen afhankelijk proces server hoekformaatgreep van de snelheid van het dagelijkse wijzigen in alle werkbelasting in het beveiligde.


- U moet voldoende berekeningscluster taken zoals inline compressie en versleuteling uit te voeren.
- Het processerver gebruikt schijfcache. Zorg dat de ruimte aanbevolen cache en schijfdoorvoer is beschikbaar voor de wijzigingen aan de gegevens die zijn opgeslagen in het geval van netwerk knelpunt of -storing vergemakkelijken. 
- Controleer of voldoende bandbreedte zodat de processerver de gegevens naar de basispagina doelserver uploaden kunt om doorlopende gegevens te beschermen. 

Tabel 2 biedt een overzicht van de richtlijnen van de server proces.

**De snelheid van gegevens wijzigen** | **CPU** | **Geheugen** | **Cachegrootte Schijfopruiming**| **Cache-schijfdoorvoer** | **Bandbreedte ingress/egress**
--- | --- | --- | --- | --- | ---
< 300 GB | 4 vCPUs (2-sockets * 2 cores @ 2,5 GHz) | 4 GB | 600 GB | 7 tot en met 10 MB per seconde | 30 Mbps/21 Mbps
300 tot 600 GB | 8 vCPUs (2-sockets * 4 cores @ 2,5 GHz) | 6 GB | 600 GB | 11 tot 15 MB per seconde | 60 Mbps/42 Mbps
600 GB tot 1 TB | 12 vCPUs (2-sockets * 6 cores @ 2,5 GHz) | 8 GB | 600 GB | 16 tot en met 20 MB per seconde | 100 Mbps/70 Mbps
> 1 TB | Een ander processerver implementeren | | | | 

**Tabel 2**

Waarbij geldt: 

- Ingress is downloaden bandbreedte (intranet tussen de bron- en proces-server).
- Egress is upload bandbreedte (internet tussen de processerver en de basispagina doelserver). Gemiddelde 30% proces servercompressie wordt ervan uitgegaan egress getallen.
- Voor cache wordt schijf een afzonderlijke OS schijf van de minimale 128 GB aanbevolen voor proces op alle servers.
- Cache schijf worden verwerkt de volgende opslag is gebruikt voor benchmarks: 8 SA's stations van 10 RPM met RAID 10 configuratie.

#### <a name="configuration-server-considerations"></a>Overwegingen bij de configuratie-server

Elke configuratieserver kan maximaal 100 bron machines met 3 en 4 delen ondersteunen. Als uw implementatie groter is wordt u aangeraden dat u een andere configuratieserver implementeren. Zie tabel 1 voor de standaardeigenschappen van virtuele machine van de configuratieserver. 

#### <a name="master-target-server-and-storage-account-considerations"></a>Aandachtspunten voor basispagina doel server en opslag-account

De opslag van voor elke basispagina doelserver bevat een OS schijf, een bewaarbeleid volume en gegevensschijven. Het bewaarbeleid station onderhoudt het logboek van schijf wijzigingen voor de duur van het venster bewaarbeleid is gedefinieerd in de portal-Site herstellen.  Raadpleeg tabel 1 voor de VM-eigenschappen van de basispagina doelserver. Tabel 3 laat zien hoe de schijven van A4 worden gebruikt.

**Exemplaar** | **OS Schijfopruiming** | **Bewaarbeleid** | **Gegevensschijven**
--- | --- | --- | ---
 | | **Bewaarbeleid** | **Gegevensschijven**
Standaard A4 | 1 schijf (1 * 1023 GB) | 1 schijf (1 * 1023 GB) | 15 schijven (15 * 1023 GB)
Standaard D14 |  1 schijf (1 * 1023 GB) | 1 schijf (1 * 1023 GB) | 31 schijven (15 * 1023 GB)
Standaard DS4 |  1 schijf (1 * 1023 GB) | 1 schijf (1 * 1023 GB) | 15 schijven (15 * 1023 GB)

**Tabel 3**

Afhankelijk van de capaciteit, planning voor de basispagina doelserver:

- Azure opslag prestaties en beperkingen
    - Het maximum aantal schijven gebruikt voor een standaard laag VM ten zeerste, ongeveer 40 (20.000/500 IO's / s per schijf) in een enkel opslag-account. Lees meer over [schaalbaarheid doelen voor standaard opslag sccounts](../storage/storage-scalability-targets.md#scalability-targets-for-standard-storage-accounts) en voor [premium opslag sccounts](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts).
-   De snelheid van dagelijkse wijzigen 
-   Bewaarbeleid volume opslagruimte.

Houd rekening met:

- Één bronbereik niet meerdere omvatten meerdere opslag-accounts. Dit geldt voor de gegevensschijf die gaat u naar de opslag-accounts geselecteerd wanneer u beveiliging configureren. Het besturingssysteem en het bewaarbeleid schijven meestal Ga naar het account automatisch geïmplementeerd opslag.
- Het volume van het bewaarbeleid-opslag vereist, is afhankelijk van de snelheid van het dagelijkse wijzigen en het aantal dagen bewaren. Bewaarbeleid opslagruimte per basispagina doelserver = totale lospeuteren uit de bron per dag * aantal bewaarbeleid dagen. 
- Elke basispagina doelserver heeft slechts één bewaarbeleid volume. Het volume van het bewaarbeleid worden verdeeld over de schijven die is gekoppeld aan de basispagina doelserver. Bijvoorbeeld:
    - Als er een bron-machine met 5 schijven en elke schijf 120 IO's / s (8 kB grootte) op de bron wordt gegenereerd, wordt dit omgezet in 240 IO's / s per schijf (2 bewerkingen op de schijf target per bron IO). 240 IO's / s is binnen de Azure per schijf IO's / s limiet van 500.
    - Klik op het volume van het bewaarbeleid, dit 120 * 5 = 600 wordt IO's / s en dit kunnen worden omgezet in een nek flessen. In dit scenario is een goede strategie meer schijven aan het volume van het bewaarbeleid toevoegen en deze opzij, als een RAID streep configuratie's beslaan. Hiermee worden de prestaties omdat de IO's / s zijn verdeeld over meerdere stations. Het aantal stations moet worden toegevoegd aan het volume van het bewaarbeleid worden als volgt:
        - Total IO's / s uit de bronomgeving / 500
        - Totale lospeuteren per dag van de bronomgeving (niet gecomprimeerd) / 287 GB. 287 GB is de maximumdoorvoer die worden ondersteund door een Do not use per dag. Deze metrisch varieert afhankelijk van de grootte schrijven met een factor van 8K, omdat in dit geval 8K nieuwe uitgegaan schrijven grootte. Bijvoorbeeld als de grootte van het schrijven 4K zijn doorvoer kan 287/2. En als de grootte van het schrijven 16 kB is doorvoer zijn kan 287 * 2.
- Het getal = totale bron IO's / s/10000 opslag-accounts die zijn vereist.


## <a name="before-you-start"></a>Voordat u begint

**Onderdeel** | **Vereisten** | **Meer informatie**
--- | --- | --- 
**Azure-account** | U moet een [Microsoft Azure](https://azure.microsoft.com/) -account. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
**Azure-opslag** | Moet u een account Azure opslag voor de opslag gerepliceerde gegevens<br/><br/> Het account moet een [Standaardaccount geografische-redundante opslag](../storage/storage-redundancy.md#geo-redundant-storage) of [Premium opslag-Account](../storage/storage-premium-storage.md).<br/><br/> Er moet in hetzelfde gebied, als de Site herstel van Azure-service en zijn gekoppeld aan hetzelfde abonnement. We het verplaatsen van opslag-accounts die zijn gemaakt met behulp van de [nieuwe portal van Azure](../storage/storage-create-storage-account.md) via resourcegroepen wordt niet ondersteund.<br/><br/> Lees meer informatie [Inleiding tot Microsoft Azure Storage](../storage/storage-introduction.md)
**Azure virtuele netwerk** | U moet een Azure virtuele netwerk waarop de configuratieserver en basispagina doelserver wordt geïmplementeerd. Dit moet in dezelfde abonnement en regio als de kluis Azure sites worden hersteld. Als u wilt repliceren gegevens via een ExpressRoute of VPN-verbinding mag het Azure virtuele netwerk worden verbonden met uw on-premises netwerk via een ExpressRoute-verbinding of een VPN-verbinding voor de Site-naar-Site.
**Azure resources** | Zorg ervoor dat er voldoende Azure resources om alle onderdelen implementeren. Lees meer in [Azure-abonnementen](../azure-subscription-service-limits.md).
**Azure virtuele machines** | Virtuele machines die u wilt beveiligen moeten met [Azure vereisten](site-recovery-best-practices.md)voldoen.<br/><br/> **Aantal schijven**, een maximum van 31 schijven kan worden ondersteund op één beveiligde server<br/><br/> **Grootte van de schijf**, afzonderlijke schijf hoeft niet meer dan 1023 GB<br/><br/> **Cluster**, gegroepeerde servers worden niet ondersteund<br/><br/> **Opstarten**, Unified Extensible Firmware Interface UEFI () / Extensible Firmware Interface(EFI) opstarten wordt niet ondersteund<br/><br/> **Volumes**, Bitlocker gecodeerde schijfstations worden niet ondersteund<br/><br/> **Namen van servers**, namen mogen bevatten tussen 1 en 63 tekens (letters, cijfers en afbreekstreepjes). De naam moet beginnen met een letter of getal en eindigen met een letter of een getal. U kunt de naam van de Azure wijzigen nadat een machine is beveiligd.
**Configuratieserver voor** | Standaard A3 virtuele machine op basis van een afbeelding van de galerie met Azure Site herstel Windows Server 2012 R2 wordt gemaakt van uw abonnement voor de configuratieserver. Deze gemaakt als het eerste exemplaar in een nieuwe cloudservice. Als u openbare Internet selecteert als het type connectivity voor de configuratieserver voor wordt de cloudservice gemaakt met een gereserveerde openbare IP-adres.<br/><br/> Het installatiepad moet in alleen Engelse tekens.
**Basispagina doelserver** | Azure virtuele machines, standaard A4, D14 of DS4.<br/><br/> Het installatiepad moet in alleen Engelse tekens. Het pad moet bijvoorbeeld **/usr/local/ASR** voor een basispagina doelserver waarop Linux.
**Processerver** | U kunt het processerver op fysieke of Windows Server 2012 R2 uitvoert met de meest recente updates VM implementeren. Installeren op C: /.<br/><br/> Het is raadzaam om dat u de server in hetzelfde netwerk en subnet plaatsen als de machines die u wilt beveiligen.<br/><br/> Installeer VMware vSphere CLI 5.5.0 op de processerver. Het onderdeel VMware vSphere CLI is vereist op de processerver om te ontdekken virtuele machines beheerd door een server vCenter of virtuele machines waarop een ESXi-host.<br/><br/> Het installatiepad moet in alleen Engelse tekens.<br/><br/> Referenties bestandssysteem wordt niet ondersteund.
**VMware** | Een vCenter VMware server uw VMware vSphere hypervisors beheren. Deze moet vCenter versie 5.1 of 5.5 uitvoeren met de meest recente updates.<br/><br/> Een of meer vSphere hypervisors met VMware virtuele machines die u wilt beveiligen. De hypervisor moet ESX/ESXi versie 5.1 of 5.5 worden uitgevoerd met de meest recente updates.<br/><br/> VMware virtuele machines moet hebben VMware extra geïnstalleerd en uit te voeren. 
**Windows-computers** | Beveiligde fysieke servers of VMware virtuele machines waarop Windows wordt uitgevoerd kunt u een aantal vereisten hebben.<br/><br/> Een ondersteunde 64-bits besturingssysteem: **Windows Server 2012 R2**, **Windows Server 2012**, of **Windows Server 2008 R2 met bij minimaal SP1**.<br/><br/> De hostnaam, punten, namen, Windows system-pad (bijvoorbeeld: C:\Windows) alleen moeten worden in het Engels.<br/><br/> Het besturingssysteem moeten worden geïnstalleerd op C:\ schijf.<br/><br/> Alleen standaard schijven worden ondersteund. Dynamische schijven worden niet ondersteund.<br/><br/> Moeten firewallregels op beveiligde computers kunnen worden aangemeld bij de configuratie en de hoofdlijst doel-server in Azure.p ><p>U moet een beheerdersaccount opgeven (moet een lokale beheerder op de Windows-computer zijn) naar push installeren de Service mobiliteit op Windows-servers. Als de opgegeven account een account niet van het domein is moet u de toegang van externe gebruikers besturingselement op de lokale computer uitschakelen. U doet dit de vermelding van de Register LocalAccountTokenFilterPolicy DWORD met een waarde van 1 onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System toevoegen. Naar de vermelding in het register uit een geopende cmd CLI of powershell toevoegen en voer **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. [Meer informatie](https://msdn.microsoft.com/library/aa826699.aspx) over toegangsbeheer.<br/><br/> Na een failover dat desgewenst kunt u verbinding maken met Windows virtuele machines in Azure wordt aangegeven met extern bureaublad Zorg ervoor dat extern bureaublad is ingeschakeld voor de lokale computer. Als u geen verbinding via VPN maakt, moeten firewallregels extern bureaublad verbindingen toestaan via internet.
**Linux machines** | Een ondersteunde 64-bits besturingssysteem wordt uitgevoerd: **Centos 6.4, 6.5, 6.6**; **Oracle Enterprise Linux 6.4, 6.5 waarop rood rol compatibele kernel of Unbreakable Enterprise Kernel Release 3 (UEK3)**, **SUSE Linux Enterprise Server 11 SP3**.<br/><br/> Firewallregels op beveiligde computers moeten hen in staat stellen de configuratie en basispagina doelservers in Azure hebt bereikt.<br/><br/> / etc/hosts-bestanden op beveiligde machines moeten items die de naam van de lokale host aan IP-adressen die is gekoppeld aan alle NIC toewijzen bevatten <br/><br/> Als u wilt verbinden met een Azure virtuele machines waarop Linux na failover Secure Shell mailclient (ssh), zorg ervoor dat de Secure Shell-service op de computer van de beveiligde is ingesteld om te beginnen automatisch op het opstarten en dat firewallregels toestaan een ssh verbinding toe.<br/><br/> De hostnaam, punten, apparaat, en Linux systeempaden bestands- en mapnamen (bijvoorbeeld/enzovoort /; /usr) moet zijn in het Engels alleen.<br/><br/> Beveiliging kan worden ingeschakeld voor on-premises implementatie machines met de volgende opslag:-<br>Bestandssysteem: EXT3, ETX4, ReiserFS, XFS<br>Paden software-apparaat toewijzing (paden)<br>Volume manager: LVM2<br>Fysieke servers met HP CCISS controller opslagmedia worden niet ondersteund.
**Derde partij** | Sommige onderdelen implementatie in dit scenario, hangt af van derden goed. Zie voor een volledige lijst [kennisgeving over software van derden en informatie](#third-party)


### <a name="network-connectivity"></a>Netwerkconnectiviteit

U hebt twee opties tijdens het configureren van de netwerkverbindingen tussen uw on-premises site en het Azure virtuele netwerk waarin de onderdelen van de infrastructuur (configuratieserver, basispagina target servers) zijn geïmplementeerd. U moet bepalen welke netwerkoptie-connectiviteit gebruiken voordat u kunt uw configuratieserver implementeren. U moet deze instelling op het moment van implementatie. Deze worden niet later gewijzigd.

**Internet:** Communicatie en replicatie van gegevens tussen de on-premises implementatie-servers (processerver, beveiligde machines) en de componentservers Azure-infrastructuur (configuratieserver, basispagina doelserver) gebeurt via een beveiligde verbinding van SSL/TLS vanuit on-premises naar de openbare eindpunten op de configuratie en de hoofdlijst doel-servers. (De enige uitzondering is de verbinding tussen de processerver en de basispagina doelserver op TCP-poort 9080, dat wil versleutelde zeggen. Alleen besturingselement gerelateerd aan het protocol herhaling voor replicatie-instelling projectgegevens op deze verbinding.)

![Implementatie diagramweergave internet](./media/site-recovery-vmware-to-azure-classic-legacy/internet-deployment.png)

**VPN**: communicatie en replicatie van gegevens tussen de on-premises implementatie-servers (processerver, beveiligde machines) en de componentservers Azure-infrastructuur (configuratieserver, basispagina doelserver) gebeurt via een VPN-verbinding tussen uw on-premises netwerk en het Azure virtuele netwerk waarin de configuratieserver en basispagina doelservers zijn geïmplementeerd. Zorg ervoor dat uw on-premises netwerk is verbonden met het Azure virtuele netwerk door een ExpressRoute-verbinding of een VPN-verbinding voor site-naar-site.

![Implementatiediagram VPN](./media/site-recovery-vmware-to-azure-classic-legacy/vpn-deployment.png)


## <a name="step-1-create-a-vault"></a>Stap 1: Maak een kluis

1. Meld u aan bij de [beheerportal](https://portal.azure.com).


2. **Gegevensservices**uitvouwen > **Herstel Services** en klik op **Site herstel kluis**.


3. Klik op **nieuwe maken** > **snel tabellen maken**.

4. Voer in het vak **naam**een beschrijvende naam voor de kluis.

5. Selecteer in de **regio**, de geografische regio voor de kluis. Als u wilt controleren bekijken ondersteunde regio's geografische beschikbaarheid in [Azure Site herstel prijzen Details](https://azure.microsoft.com/pricing/details/site-recovery/)

6. Klik op **maken kluis**.

    ![Nieuwe kluis](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-create-vault.png)

Controleer de statusbalk om te bevestigen dat de kluis is gemaakt. De kluis worden, als **actief** vermeld op de hoofdpagina van **Herstel Services** .

## <a name="step-2-deploy-a-configuration-server"></a>Stap 2: Een configuratieserver implementeren

### <a name="configure-server-settings"></a>Serverinstellingen configureren

1. Klik in de pagina **Herstel Services** op de kluis om de pagina snel aan de slag te openen. Snel aan de slag kan ook worden geopend op elk gewenst moment met het pictogram.

    ![Snel aan de slag-pictogram](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-icon.png)

2. Selecteer in de vervolgkeuzelijst **tussen een on-premises implementatie-site met VMware/fysieke servers en Azure**.
3. Klik op **Configuratie-Server implementeren**in **Voorbereiden Target(Azure) Resources** .

    ![Configuratieserver implementeren](./media/site-recovery-vmware-to-azure-classic-legacy/deploy-cs2.png)

4. Geef in de **Nieuwe configuratie-Server-gegevens** op:

    - Een naam voor de configuratieserver en de referenties te brengen.
    - Typ in het netwerk connectivity vervolgmenu Selecteer **Openbare Internet** of **VPN**. Houd er rekening mee dat u deze instelling nadat deze toegepast niet wijzigen.
    - Selecteer het Azure netwerk waarop de server moet zich bevinden. Als u gebruikmaakt van VPN tabelmaakquery is weet u het Azure netwerk verbonden met uw on-premises netwerk zoals verwacht. 
    - Geef de interne IP-adres en subnet dat op de server worden toegewezen. Houd er rekening mee dat de eerste vier IP-adressen in een subnet worden gereserveerd voor interne Azure gebruik. Gebruik een ander beschikbaar IP-adres.
    
    ![Configuratieserver implementeren](./media/site-recovery-vmware-to-azure-classic-legacy/cs-details.png)

5. Wanneer u op **OK** een standaard A3 virtuele machine op basis van een afbeelding van de galerie met Azure Site herstel Windows Server 2012 R2 wordt gemaakt van uw abonnement voor de configuratieserver. Deze gemaakt als het eerste exemplaar in een nieuwe cloudservice. Als u verbinding maakt via internet geselecteerd wordt de cloudservice wordt gemaakt met een gereserveerde openbare IP-adres. U kunt de voortgang in het tabblad **taken** controleren.

    ![Voortgang bewaken](./media/site-recovery-vmware-to-azure-classic-legacy/monitor-cs.png)

6.  Als u verbinding via internet maakt, na de configuratieserver is geïmplementeerd notitie het openbare IP-adres toegewezen aan deze op de pagina **virtuele Machines** in de portal van Azure. Klik op de **eindpunten** tabblad notitie de openbare HTTPS-poort toegewezen aan persoonlijke poort 443. Wanneer u de basispagina doel- en proces servers met de configuratieserver registreert, hebt u deze informatie later nodig. De configuratieserver wordt geïmplementeerd met deze eindpunten:

    - HTTPS: Een openbare poort wordt gebruikt om te coördineren communicatie tussen componentservers en Azure via internet. Privé poort 443 wordt gebruikt om de communicatie tussen componentservers en Azure coördineren via VPN.
    - Aangepaste: Een openbare poort wordt gebruikt voor failback hulpmiddel communicatie via internet. Privé poort 9443 wordt gebruikt voor failback hulpmiddel communicatie via VPN.
    - PowerShell: Privé poort 5986
    - Extern bureaublad: privé poort 3389
    
    ![VM eindpunten](./media/site-recovery-vmware-to-azure-classic-legacy/vm-endpoints.png)

    >[AZURE.WARNING] Niet verwijderen of wijzigen van het openbaar of privé poortnummer van eindpunten gemaakt tijdens de configuratie server-implementatie.

De configuratieserver wordt geïmplementeerd in een automatisch gemaakte Azure-cloudservice met een gereserveerde IP-adres. Het gereserveerde adres nodig is om ervoor te zorgen dat het server cloud service IP-adres hetzelfde in de virtuele machines blijft (inclusief de configuratieserver) niet langer nodig in de cloudservice. Het gereserveerde openbare IP-adres moet worden handmatig niet-gereserveerde wanneer de configuratieserver is buiten gebruik gesteld of deze gereserveerde blijven moet. Er is een standaardlimiet van 20 gereserveerde openbare IP-adressen per abonnement. [Meer informatie](../virtual-network/virtual-networks-reserved-private-ip.md) over gereserveerde IP-adressen. 

### <a name="register-the-configuration-server-in-the-vault"></a>De configuratieserver in de kluis registreren

1. Klik in de pagina **Snel starten** op **Voorbereiden doel Resources** > **een registratiegegevens-sleutel downloaden**. Het belangrijkste bestand wordt automatisch gegenereerd. Dit geldt voor 5 dagen nadat deze gegenereerd. Kopieer deze naar de configuratieserver.
2. Selecteer de configuratieserver in de lijst virtuele machines in **virtuele Machines** . Open het tabblad **Dashboard** en klik op **verbinding maken**. **Open** het gedownloade RDP-bestand aan te melden bij de configuratieserver met extern bureaublad. Als u VPN gebruikt, gebruikt u de interne IP-adres (het adres dat u hebt opgegeven toen u de configuratieserver geïmplementeerd) voor een verbinding met extern bureaublad van de on-premises implementatie-site. De Wizard Setup van Azure Site herstel configureren Server automatisch wordt uitgevoerd wanneer u zich aanmeldt voor de eerste keer.

    ![Registratie](./media/site-recovery-vmware-to-azure-classic-legacy/splash.png)

3. Klik op **ik ga akkoord** als u wilt downloaden en installeren van MySQL in **Software-installatie van derden** .

    ![MySQL installeren](./media/site-recovery-vmware-to-azure-classic-legacy/sql-eula.png)

4. Maak referenties aan te melden bij de MySQL-server-instantie in **MySQL-Server-gegevens** .

    ![MySQL-referenties](./media/site-recovery-vmware-to-azure-classic-legacy/sql-password.png)

5. Geef in de **Instellingen voor Internet** op hoe de configuratieserver maakt verbinding met internet. Houd rekening met:

    - Als u wilt gebruiken van een aangepaste proxy moet u deze instellen voordat u de Provider installeren.
    - Wanneer u op **volgende** klikt wordt een test wordt uitgevoerd als u wilt controleren van de proxyverbinding.
    - Als u gebruik maakt van een aangepaste proxy of uw standaardproxy u de gegevens voor de proxy moet, inclusief het adres, poorten en referenties invoeren-verificatie is vereist.
    - De volgende URL's moet toegankelijk via de proxy:
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Als er IP-adres gebaseerde firewallregels Zorg ervoor dat de regels zijn ingesteld op toestaan mededeling van de configuratieserver aan het IP-adressen die worden beschreven in (443) [IP-bereiken gebruikt Azure Datacenter](https://msdn.microsoft.com/library/azure/dn175718.aspx) en HTTPS-protocol. U moet wit-lijst met IP-bereiken van de Azure regio die u wilt gebruiken en die van West VS.

    ![Registratie van proxy](./media/site-recovery-vmware-to-azure-classic-legacy/register-proxy.png)

6. In **De instellingen van Provider fout bericht lokalisatie** aangeven in welke taal foutberichten worden weergegeven.

    ![Fout bericht registratie](./media/site-recovery-vmware-to-azure-classic-legacy/register-locale.png)

7. Blader in **Azure Site herstel registratie** en selecteer het belangrijkste bestand dat u hebt gekopieerd naar de server.

    ![Belangrijke Bestandsregistratie](./media/site-recovery-vmware-to-azure-classic-legacy/register-vault.png)

8. Selecteer op de voltooiingspagina van de wizard deze opties:

    - **Dialoogvenster voor Account Management starten** om op te geven dat het dialoogvenster Accounts beheren moet worden geopend als u klaar bent met de wizard selecteren.
    - Selecteer **een bureaubladpictogram voor Cspsconfigtool maken** om toe te voegen een snelkoppeling op het bureaublad op de configuratieserver zodat u het dialoogvenster **Accounts beheren** op elk gewenst moment openen kunt zonder de wizard uit te voeren.

    ![Volledige registratie](./media/site-recovery-vmware-to-azure-classic-legacy/register-final.png)

9. Klik op **Voltooien** om de wizard. Een wachtwoordzin wordt gegenereerd. Kopieer deze op een veilige locatie. U moet deze om te verifiëren en het proces en de basispagina doelservers met de configuratieserver registreren. Dit wordt ook gebruikt om ervoor te zorgen kanaal integriteit in configuratie servercommunicatie. U kunt de wachtwoordzin genereren, maar vervolgens moet u het diamodel doel opnieuw te registreren en verwerken servers met behulp van de nieuwe wachtwoordzin.

    ![Wachtwoordzin](./media/site-recovery-vmware-to-azure-classic-legacy/passphrase.png)

Na registratie worden, de configuratieserver vermeld op de pagina **Configuratieservers** in de kluis.

### <a name="set-up-and-manage-accounts"></a>Instellen en beheren van accounts

Tijdens de implementatie aanvraagt Site herstel referenties voor de volgende bewerkingen uit:

- Een account VMware zodat thatSite herstel kunt automatisch discovery VMs op vCenter servers of vSphere hosts. 
- Wanneer u machines voor beveiliging toevoegt zodat Site herstel de service mobiliteit worden kunt installeren.

U kunt het dialoogvenster **Accounts beheren** als u wilt toevoegen en beheren van accounts die moeten worden gebruikt voor deze acties openen nadat u de configuratieserver hebt geregistreerd. Er zijn verschillende manieren u dit wilt doen:

- Open de snelkoppeling die u deelnemen aan die zijn gemaakt voor het dialoogvenster op de laatste pagina van de instelling voor de configuratieserver (cspsconfigtool).
- Open het dialoogvenster op voltooien van de installatie van de configuratie-server.

1. Klik op **Account toevoegen**in **Accounts beheren** . U kunt ook wijzigen en verwijderen van bestaande accounts.

    ![Accounts beheren](./media/site-recovery-vmware-to-azure-classic-legacy/manage-account.png)

2. Geef de naam van een account voor gebruik in **Accountgegevens** in Azure en referenties (domein-/ gebruikersnaam). 

    ![Accounts beheren](./media/site-recovery-vmware-to-azure-classic-legacy/account-details.png)

### <a name="connect-to-the-configuration-server"></a>Verbinding maken met de configuratieserver 

Er zijn twee manieren verbinding maken met de configuratieserver:

- Via een VPN-site-naar-site of ExpressRoute-verbinding
- Via internet 

Houd rekening met:

- Een internetverbinding gebruikt de eindpunten van de virtuele machine in combinatie met de openbare virtuele IP-adres van de server.
- Een VPN-verbinding wordt het interne IP-adres van de server gebruikt samen met de persoonlijke poorten eindpunt.
- Is een eenmalige beslissing te bepalen of verbinding maken (beheer en replicatie-gegevens) van uw on-premises implementatie-servers op de verschillende componentservers (configuratieserver, basispagina doelserver) uitgevoerd in Azure wordt aangegeven via een VPN-verbinding of internet. U kunt geen deze instelling achteraf wijzigen. Als u dit doet moet u het scenario implementeren en uw machines reprotect.  


## <a name="step-3-deploy-the-master-target-server"></a>Stap 3: De basispagina doelserver implementeren

1. Klik op **Voorbereiden Target(Azure) Resources** > **Deploy basispagina doelserver**.
2. Geef de details van de basispagina doel-server en de referenties. De server wordt geïmplementeerd in hetzelfde Azure netwerk als de configuratieserver. Wanneer u klikt op een Azure virtuele machines voltooid wordt gemaakt met een afbeelding Windows of Linux.

    ![De instellingen van de doel-server](./media/site-recovery-vmware-to-azure-classic-legacy/target-details.png)

Houd er rekening mee dat de eerste vier IP-adressen in een subnet worden gereserveerd voor interne Azure gebruik. Geef eventuele andere beschikbare IP-adres.

>[AZURE.NOTE] Selecteer standaard DS4 tijdens het configureren van beveiliging voor werkbelasting waarvoor consistente hoog o prestaties en lage latentie om te hosten i/o-intensief werkbelasting met [Premium opslag-Account](../storage/storage-premium-storage.md).


3. Een Windows-werkruimten doelserver VM wordt gemaakt met deze eindpunten. Houd er rekening mee dat openbare eindpunten alleen gemaakt worden als uw verbinding maakt via internet.

    - Aangepaste: Openbare poort die wordt gebruikt door de processerver herhaling om gegevens te verzenden via internet. Privé poort 9443 wordt gebruikt door de processerver herhaling via gegevens te verzenden naar de basispagina doelserver VPN.
    - 1: Openbare poort die door de processerver voor het verzenden van metagegevens via internet. Privé poort 9080 wordt gebruikt door de processerver metagegevens verzenden naar de basispagina doelserver via VPN.
    - PowerShell: Privé poort 5986
    - Extern bureaublad: privé poort 3389

4. Een basispagina doelserver VM van Linux wordt gemaakt met deze eindpunten. Houd er rekening mee dat openbare eindpunten alleen als u verbinding via internet maakt worden gemaakt.

    - Aangepaste: Openbare poort processerver die herhaling om gegevens te verzenden via internet. Privé poort 9443 wordt gebruikt door de processerver herhaling via gegevens te verzenden naar de basispagina doelserver VPN.
    - 1: Openbare poort wordt gebruikt door de processerver voor het verzenden van metagegevens via internet. Privé poort 9080 wordt door de processerver gebruikt voor het verzenden van metagegevens aan de basispagina doelserver via VPN
    - SSH: Privé poort 22

    >[AZURE.WARNING] Niet verwijderen of wijzigen van het openbaar of privé poortnummer van een van de eindpunten gemaakt tijdens de implementatie van de basispagina doel-server.

5. In de **virtuele Machines** wachten de virtuele machine te starten.

    - Als dit een Windows server Noteer de details van het externe bureaublad is.
    - Als het een Linux-server is en u verbinding via VPN maakt noteert u het interne IP-adres van de virtuele machine. Als u verbinding via internet maakt noteert u het openbare IP-adres.

6.  Meld u aan bij de server naar volledige installatie en het registreren met de configuratieserver. 
7.  Als u werkt met Windows:

    1. Een verbinding met extern bureaublad naar de virtuele machine starten. De eerste keer dat u zich aanmeldt een script worden uitgevoerd in een PowerShell-venster. Niet sluiten. Wanneer deze is voltooid wordt het hulpmiddel Host Agent Config automatisch geopend om te registreren van de server.
    2. Geef het interne IP-adres van de configuratieserver en poort 443 in **Host Agent Config** . U kunt de interne adres en privé poort 443 zelfs als u geen verbinding via VPN maakt omdat de virtuele machine is gekoppeld aan hetzelfde Azure netwerk als de configuratieserver. Laat **Gebruik HTTPS** ingeschakeld. Voer de wachtwoordzin voor de configuratieserver die u eerder hebt genoteerd. Klik op **OK** om te registreren van de server. U kunt de opties NAT negeren. Ze niet gebruikt.
    3. Als uw geschatte bewaarbeleid station vereiste meer dan 1 TB is kunt u het bewaarbeleid volume (R:) met een virtuele schijf en [opslag spaties](http://blogs.technet.com/b/askpfeplat/archive/2013/10/21/storage-spaces-how-to-configure-storage-tiers-with-windows-server-2012-r2.aspx) configureren
    
    ![Windows-basispagina doelserver](./media/site-recovery-vmware-to-azure-classic-legacy/target-register.png)

8. Als u werkt met het Linux:
    1. Zorg ervoor dat u de meest recente Linux Integration Services (hierin) geïnstalleerd voordat u de basispagina doelserver installeren hebt geïnstalleerd. U vindt de nieuwste versie van LIS samen met instructies voor het installeren [hier](https://www.microsoft.com/download/details.aspx?id=46842). Start de computer opnieuw op nadat hierin hebt geïnstalleerd.
    2. Klik op **extra software (alleen voor Linux outmodel doelserver) downloaden en installeren**in **voorbereiden doellijst (Azure) Resources** . De gedownloade tar-bestand kopiëren naar de virtuele machine met een sftp-client. U kunt ook Meld u aan bij de basispagina doelserver geïmplementeerd linux en *wget http://go.microsoft.com/fwlink/?LinkID=529757&clcid=0x409* gebruikt om te downloaden de het bestand.
    2. Meld u aan bij de server met een Secure Shell-client. Als u met het Azure netwerk via VPN verbonden bent gebruikt u de interne IP-adres. Anders gebruiken het externe IP-adres en het openbare SSH-eindpunt.
    3. De bestanden extraheren uit het installatieprogramma van gzipped door te voeren: **oelframe – xvzf Microsoft-ASR_UA_8.4.0.0_RHEL6-64***
    ![Linux basispagina doelserver](./media/site-recovery-vmware-to-azure-classic-legacy/linux-tar.png)
    4. Controleer of dat u bent in de map waarin u de inhoud van het bestand tar uitgepakt.
    5. De configuratie server wachtwoordzin kopiëren naar een lokaal bestand met de opdracht * *echo* `<passphrase>` * > passphrase.txt**
    6. De opdracht uitvoeren "**sudo. / installeren -t beide - een hosten -R -d /usr/local/ASR MasterTarget -i* `<Configuration server internal IP address>` * -p 443 -s y - c https -P passphrase.txt**".

    ![Register-doelserver](./media/site-recovery-vmware-to-azure-classic-legacy/linux-mt-install.png)

9. Wacht een paar minuten (10-15) en klik op de pagina contact dat de basispagina doelserver wordt weergegeven, zoals geregistreerd in **Servers** > tabblad van de **Details van de Server** **Configuration Servers** . Als u werkt met Linux en deze voert niet register de host config hulpprogramma opnieuw uit vanaf /usr/local/ASR/Vx/bin/hostconfigcli. U moet toegangsmachtigingen instellen door type chmod als hoofdsite uit te voeren.

    ![Controleer of de doelserver](./media/site-recovery-vmware-to-azure-classic-legacy/target-server-list.png)

>[AZURE.NOTE] Het kan maximaal 15 minuten nadat de registratie is voltooid voor de basispagina doelserver wel weergegeven in de portal duren. Als u direct bijwerken, klikt u op **vernieuwen** op de pagina **Configuratie-Servers** .

## <a name="step-4-deploy-the-on-premises-process-server"></a>Stap 4: De on-premises implementatie processerver implementeren

U wordt aangeraden de dat u een statische IP-adres op de processerver zodanig configureren dat deze heeft gegarandeerd permanente in opnieuw opstarten voordat u begint.

1. Klik op de werkbalk Snel starten > **on-premises proces-servers installeren** > **downloaden en installeren van de processerver**.

    ![Processerver installeren](./media/site-recovery-vmware-to-azure-classic-legacy/ps-deploy.png)

2.  De gedownloade zipbestand kopiëren naar de server waarop u gaat voor het installeren van de processerver. Het zipbestand bevat twee installatiebestanden:

    - Microsoft-ASR_CX_TP_8.4.0.0_Windows*
    - Microsoft-ASR_CX_8.4.0.0_Windows*

3. Pak het archief en kopieer de installatiebestanden naar een locatie op de server.
4. Voer de **Microsoft-ASR_CX_TP_8.4.0.0_Windows*** installatie bestand en volg de instructies. Hiermee installeert u onderdelen van derden die nodig zijn voor de implementatie.
5. Voer **Microsoft-ASR_CX_8.4.0.0_Windows***.
6. Selecteer op de pagina **Servermodus** **Proces-Server**.
7. Ga als volgt te werk op de pagina **Details van de omgeving** :


    - Als u wilt beveiligen VMware virtuele machines klikt u op **Ja**
    - Als u alleen wilt beveiligen met een fysieke servers en dus niet nodig VMware vCLI geïnstalleerd op de processerver hebt. Klik op **Nee** en doorgaan.

8. Houd rekening met het volgende tijdens de installatie van VMware vCLI:

    - **Alleen VMware vSphere CLI 5.5.0 wordt ondersteund**. Het processerver werkt niet met andere versies of updates van vSphere CLI.
    - VSphere CLI 5.5.0 downloaden uit [hier.](https://my.vmware.com/web/vmware/details?downloadGroup=VCLI550&productId=352)
    - Als u vSphere CLI geïnstalleerd voordat u bent begonnen met het installeren van de processerver en setup dit niet wordt gedetecteerd, wacht u maximaal vijf minuten voordat u Voer setup opnieuw. Dit zorgt ervoor dat alle omgevingsvariabelen die nodig zijn voor vSphere CLI detectie zijn geïnitialiseerd correct.

9.  Selecteer in **De selectie NIC voor processerver** de netwerkadapter die de processerver moet gebruiken.

    ![Selecteer netwerkadapter](./media/site-recovery-vmware-to-azure-classic-legacy/ps-nic.png)

10. **Configuratiegegevens voor de Server**:

    - Geef het interne IP-adres van de configuratieserver en 443 voor de poort voor de IP-adres en poort als u verbinding via VPN maakt. Anders opgeven van de openbare virtuele IP-adres en toegewezen openbare HTTP-eindpunt.
    - Typ in de wachtwoordzin van de configuratieserver.
    - Schakel **verifiëren mobiliteit service software handtekening** als u controle uitschakelen wilt wanneer u automatische push gebruikt voor het installeren van de service. Controle van de handtekening moet internetconnectiviteit van de processerver.
    - Klik op **volgende**.

    ![Register-configuratieserver](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cs.png)


11. Selecteer een cache-station in **Selecteer installatie station** . De processerver moet een cache-station met ten minste 600 GB beschikbare ruimte. Klik vervolgens op **installeren**. 

    ![Register-configuratieserver](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cache.png)

12. Houd er rekening mee dat moet u mogelijk opnieuw opstarten van de server om de installatie te voltooien. In de **Configuratieserver voor** > **Server-gegevens** Controleer of de processerver wordt weergegeven en is geregistreerd in de kluis.

>[AZURE.NOTE]Het kan maximaal 15 minuten nadat de registratie is voltooid voor de processerver moet worden weergegeven als weergegeven onder de configuratieserver voor duren. Als u direct bijwerken, vernieuwt u de configuratieserver door te klikken op de knop Vernieuwen onder aan de configuratiepagina-server
 
![Processerver valideren](./media/site-recovery-vmware-to-azure-classic-legacy/ps-register.png)

Als u de controle van de handtekening voor de service mobiliteit niet uitschakelt nadat u de processerver geregistreerd kunt u dit later als volgt doen:

1. Meld u aan bij de processerver als een beheerder en open het bestand C:\pushinstallsvc\pushinstaller.conf om te bewerken. Klik onder de sectie **[PushInstaller.transport]** deze regel toevoegen: **SignatureVerificationChecks = '0'**. Opslaan en sluit het bestand.
2. Start de InMage PushInstall-service.


## <a name="step-5-update-site-recovery-components"></a>Stap 5: Update-Site herstel onderdelen

Site herstel onderdelen worden te bezoeken bijgewerkt. Wanneer er nieuwe updates beschikbaar zijn moet u deze installeren in de volgende volgorde:

1. Configuratieserver voor
2. Processerver
3. Basispagina doelserver
4. Failback hulpmiddel (vContinuum)

### <a name="obtain-and-install-the-updates"></a>Ophalen en de updates installeren


1. Vanuit het herstelproces is Site **Dashboard**kunt u updates voor de configuratie, proces en basispagina doelservers. Voor Linux-installatie kunt u de bestanden extraheren uit het installatieprogramma van gzipped en voert u de opdracht ' sudo. / installeren ' de update te installeren.
2. [Download](http://go.microsoft.com/fwlink/?LinkID=533813) de meest recente update voor de tool(vContinuum) Failback.
3. Als u uitvoert virtuele machines of fysieke servers die al op de mobiliteit-service is geïnstalleerd, kunt u als volgt updates downloaden voor de service:

    - **Optie 1**: updates downloaden:
        - [Windows Server (alleen 64 bits)](http://download.microsoft.com/download/8/4/8/8487F25A-E7D9-4810-99E4-6C18DF13A6D3/Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe)
        - [CentOS 6.4,6.5,6.6 (alleen 64 bits)](http://download.microsoft.com/download/7/E/D/7ED50614-1FE1-41F8-B4D2-25D73F623E9B/Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz)
        - [Oracle Enterprise Linux 6.4,6.5 (alleen 64 bits)](http://download.microsoft.com/download/5/2/6/526AFE4B-7280-4DC6-B10B-BA3FD18B8091/Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz)
        - [SUSE Linux Enterprise Server SP3 (alleen 64 bits)](http://download.microsoft.com/download/B/4/2/B4229162-C25C-4DB2-AD40-D0AE90F92305/Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz)
        - Na het bijwerken van de processerver zijn de bijgewerkte versie van de service mobiliteit beschikbaar in de map C:\pushinstallsvc\repository op de processerver.
    - **Optie 2**: als u een computer met een oudere versie van de mobiliteit-service is geïnstalleerd hebt, kunt u automatisch de service mobiliteit op de computer bijwerken in de beheerportal.

        1. Zorg ervoor dat de processerver wordt bijgewerkt.
        2. Zorg ervoor dat de beveiligde computer moeten worden voldoet aan de [vereisten](#install-the-mobility-service-automatically) voor het automatisch om de service mobiliteit, zodat de update werkt zoals verwacht.
        2. Selecteer de groep voor beveiliging, de beveiligde machine markeren en klik op **Update mobiliteit service**. Deze knop is alleen beschikbaar als er een nieuwere versie van de service mobiliteit. 

            ![Selecteer vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/update-mobility.png)

Geef het beheerdersaccount moet worden gebruikt voor het bijwerken van de service mobiliteit op de beveiligde server in de Select-accounts. Klik op OK en wacht totdat de geactiveerde taak is voltooid.


## <a name="step-6-add-vcenter-servers-or-vsphere-hosts"></a>Stap 6: VCenter servers of vSphere hosts toevoegen

1. Klik op **Servers** > **Configuration Servers** > configuratieserver >**toevoegen vCenter Server** een vCenter server of vSphere host toevoegen.

    ![Selecteer vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter.png)

2. Gegevens voor de server of host opgeven en selecteer het processerver die wordt gebruikt voor het vinden.

    - Als de server vCenter niet wordt uitgevoerd op de standaardpoort naar 443 geeft u het poortnummer in waarop de vCenter-server wordt uitgevoerd.
    - De processerver moet in hetzelfde netwerk als de host van de server/vSphere vCenter en VMware vSphere CLI 5.5.0 geïnstalleerd nodig hebt.

    ![Serverinstellingen voor vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter4.png)


3. Nadat discovery is voltooid worden, de server vCenter vermeld onder de configuratiegegevens voor de server.

    ![Serverinstellingen voor vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter2.png)

4. Als u een niet-beheerdersaccount gebruikt om toe te voegen van de server of host, schakelt u dat het account heeft de volgende bevoegdheden:

    - vCenter accounts moeten Datacenter, gegevensopslag, map, Host, netwerk, Resource, opslag weergaven, VM en hebben vSphere Distributed schakeloptie bevoegdheden is ingeschakeld.
    - vSphere host accounts moeten hebben de Datacenter, gegevensopslag, map, Host-, netwerk, Resource, VM en vSphere Distributed veranderen bevoegdheden ingeschakeld



## <a name="step-7-create-a-protection-group"></a>Stap 7: Maak een groep beveiliging

1. **Beveiligde Items**openen > **Groep beveiliging** > **beveiliging-groep maken**.

    ![Beveiliging groep maken](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg1.png)

2. Klik op de pagina **Instellingen voor documentbeveiliging groep opgeven** Geef een naam voor de groep en selecteer de configuratieserver waarop u wilt maken van de groep.

    ![Instellingen voor documentbeveiliging-groep](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg2.png)

3. Klik op de pagina **Instellingen voor replicatie opgeven** door de replicatie-instellingen die worden gebruikt voor alle computers in de groep te configureren.

    ![Beveiliging groep herhaling](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg3.png)

4. Instellingen:
    - **Meerdere VM consistentie**: als u dit inschakelt dat wordt gemaakt gedeelde toepassing consistente herstel punten op de computers in de groep beveiliging. Deze instelling is meest relevante wanneer alle computers in de groep beveiliging de dezelfde werklast worden uitgevoerd. Alle computers worden dezelfde gegevens bondig worden hersteld. Alleen beschikbaar voor Windows-servers.
    - **Drempelwaarde voor vrijgegeven Productieorder**: meldingen worden gegenereerd wanneer de doorlopende gegevens beveiliging replicatie vrijgegeven Productieorder groter is dan de geconfigureerde vrijgegeven Productieorder drempelwaarde.
    - **Herstel wijst u bewaren**: Hiermee geeft u het venster bewaarbeleid. Beveiligde machines kunnen herstellen naar een willekeurige plaats in dit venster.
    - **Frequentie van toepassing-consistente momentopname**: Hiermee wordt opgegeven hoe vaak herstel punten die van toepassing-consistente momentopnamen wordt gemaakt.

Zodra deze op de pagina **Beveiligde Items** hebt gemaakt, kunt u de groep beveiliging controleren.

## <a name="step-8-set-up-machines-you-want-to-protect"></a>Stap 8: Machines die u wilt beveiligen instellen

U moet de mobiliteit-service installeren op virtuele machines en fysieke servers die u wilt beveiligen. U kunt dit op twee manieren doen:

- Automatisch push en de service installeren op elke computer van de processerver.
- De service handmatig installeren. 

### <a name="install-the-mobility-service-automatically"></a>Automatisch de service mobiliteit installeren

Wanneer u machines aan een groep beveiliging toevoegt is automatisch de service mobiliteit gedrukt en op elke computer geïnstalleerd door het processerver. 

**Push wordt automatisch de service mobiliteit installeren op Windows-servers:** 

1. Installeer de meest recente updates voor de processerver zoals is beschreven in [stap 5: Installeer de meest recente updates voor](#step-5-install-latest-updates), en zorg ervoor dat de processerver beschikbaar is. 
2. Controleer of er netwerkverbinding tussen de broncomputer en de processerver is en dat de broncomputer is toegankelijk zijn vanuit de processerver.  
3. Windows firewall toestaan **Bestands- en printerdeling** en **Windows Management Instrumentation**configureren. Klik onder instellingen van Windows Firewall, selecteer de optie "Toestaan een app of een functie via Firewall" en selecteert u de toepassingen zoals in de onderstaande afbeelding. U kunt het firewallbeleid met een GPO configureren voor machines die deel uitmaakt van een domein.

    ![Firewallinstellingen](./media/site-recovery-vmware-to-azure-classic-legacy/push-firewall.png)

4. Het account waarmee u de push-installatie uitvoert, moet zich in de groep Administrators op de computer die u wilt beveiligen. Deze referenties worden alleen gebruikt voor push-installatie van de service mobiliteit en u kunt ze opgeeft wanneer u een computer aan een groep beveiliging toevoegt.
5. Als de opgegeven account niet een domeinaccount moet u de toegang van externe gebruikers besturingselement op de lokale computer uitschakelen. U doet dit de vermelding van de Register LocalAccountTokenFilterPolicy DWORD met een waarde van 1 onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System toevoegen. Naar de vermelding in het register uit een geopende cmd CLI of powershell toevoegen en voer **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. 

**Push installeren automatisch de service mobiliteit op Linux-servers:**

1. Installeer de meest recente updates voor de processerver zoals is beschreven in [stap 5: Installeer de meest recente updates voor](#step-5-install-latest-updates), en zorg ervoor dat de processerver beschikbaar is.
2. Controleer of er netwerkverbinding tussen de broncomputer en de processerver is en dat de broncomputer is toegankelijk zijn vanuit de processerver.  
3. Controleer of dat het account is een gebruiker hoofdmap op de bronserver Linux.
4. Zorg ervoor dat het bestand/etc/hosts op de bronserver Linux vermeldingen die de naam van de lokale host aan IP-adressen die is gekoppeld aan alle NIC's toewijzen bevat.
5. Installeer de meest recente openssh, openssh-server, openssl-pakketten op de computer die u wilt beveiligen.
6. Controleer of dat SSH is ingeschakeld en wordt uitgevoerd op poort 22. 
7. SFTP subsysteem en wachtwoord voor verificatie in het bestand sshd_config als volgt inschakelen: 

    - een) aanmelden als hoofdmap.
    - b) Zoek de regel die met **PasswordAuthentication begint**in het bestand /etc/ssh/sshd_config-bestand.
    - c) Verwijder de opmerkingen bij de lijn en wijzig de waarde van 'Nee' in 'Ja'.

        ![Linux mobiliteit](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push.png)

    - d) de regel die met subsysteem begint zoeken en verwijder de opmerkingen bij de regel.
    
        ![Linux push mobiliteit](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push2.png)    

8. Controleer of dat de bron machine Linux variant wordt ondersteund. 
 
### <a name="install-the-mobility-service-manually"></a>De service mobiliteit handmatig installeren

Softwarepakketten gebruikt voor het installeren van de service mobiliteit zijn op de processerver in C:\pushinstallsvc\repository. Meld u aan bij de processerver en het juiste installatiepakket te gaan naar de bronmachine op basis van de onderstaande tabel kopiëren:-

| Bron-besturingssysteem                               | Servicepakket mobiliteit op processerver                                                            |
|---------------------------------------------------    |------------------------------------------------------------------------------------------------------ |
| Windows Server (alleen 64 bits)                          | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe`         |
| CentOS 6.4, 6.5, 6.6 (alleen 64 bits)                    | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz`     |
| SUSE Linux Enterprise Server 11 SP3 (alleen 64 bits)     | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz`|
| Oracle Enterprise Linux 6.4, 6.5 (alleen 64 bits)        | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz`       |


**De service mobiliteit handmatig op een Windows server installeren**, voer de volgende handelingen uit:

1. Kopieer het pakket **Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe** van het proces server mappad vermeld in de bovenstaande tabel met de broncomputer.
2. Installeer de service mobiliteit door het uitvoerbare bestand uit te voeren op de broncomputer.
3. Volg de instructies van het installatieprogramma.
4. Als de rol **mobiliteit service** en klik op **volgende**.
    
    ![Mobiliteit service installeren](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install.png)

5. Laat de installatiemap als het pad naar de installatie van de standaard en klik op **installeren**.
6. Geef het IP-adres en HTTPS-poort van de configuratieserver in **Host Agent Config** .

    - Als u verbinding via internet maakt geeft u de openbare virtuele IP-adres en openbare HTTPS-eindpunt als de poort.
    - Als u verbinding via VPN maakt geeft u het interne IP-adres en 443 voor de poort. Verlaten **HTTPS gebruiken** is ingeschakeld.

    ![Mobiliteit service installeren](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install2.png)

7. De configuratie server wachtwoordzin en klik op **OK** om te registreren van de service mobiliteit met de configuratieserver.

**Uitvoeren vanaf de opdrachtregel:**

1. Kopieer de wachtwoordzin uit de CX naar het bestand 'C:\connection.passphrase' op de server en deze opdracht uitvoert. In ons voorbeeld CX i 104.40.75.37 en de HTTPS-poort 62519 is:

    `C:\Microsoft-ASR_UA_8.2.0.0_Windows_PREVIEW_20Mar2015_Release.exe" -ip 104.40.75.37 -port 62519 -mode UA /LOG="C:\stdout.txt" /DIR="C:\Program Files (x86)\Microsoft Azure Site Recovery" /VERYSILENT  /SUPPRESSMSGBOXES /norestart  -usesysvolumes  /CommunicationMode https /PassphrasePath "C:\connection.passphrase"`

**De service mobiliteit handmatig op een server Linux installeren**:

1. Kopieer het juiste tar-archief op basis van de bovenstaande tabel, van het processerver naar de broncomputer.
2. Open een shellprogramma en het archief ingepakte tar op een lokaal pad extraheren door te voeren`tar -xvzf Microsoft-ASR_UA_8.2.0.0*`
3. Een bestand passphrase.txt maken in de lokale map waarin u de inhoud van het archief tar door in te voeren uitgepakt *`echo <passphrase> >passphrase.txt`* van shell.
4. De service mobiliteit installeren door in te voeren *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Geef de IP-adres en poort:

    - Als u verbinding met de configuratieserver via internet maakt Geef de server virtuele openbare IP-adres en openbare HTTPS-eindpunt in `<IP address>` en `<port>`.
    - Als u verbinding via een VPN-verbinding maakt geeft u het interne IP-adres en 443.

**Uitvoeren vanaf de opdrachtregel**:

1. Kopieer de wachtwoordzin uit de CX naar het bestand 'passphrase.txt' op de server en uitvoeren van deze opdrachten. In ons voorbeeld CX i 104.40.75.37 en de HTTPS-poort 62519 is:

Installeren op een productieserver:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt
 
Installeren op de doelserver:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

>[AZURE.NOTE] Wanneer u toevoegt machines aan een groep beveiliging die al een juiste versie van de mobiliteit-service klikt u vervolgens de installatie van de push worden overgeslagen.


## <a name="step-9-enable-protection"></a>Stap 9: Inschakelen beveiliging

Als beveiliging wilt inschakelen u een virtuele machines en fysieke servers toevoegen aan een groep beveiliging. Houd rekening met het volgende voordat u begint:

- Virtuele machines elke 15 minuten zijn gevonden en er kan maximaal 15 minuten worden weergegeven bij het herstellen van Azure Site na discovery.
- De wijzigingen op de virtuele machine (zoals VMware hulpmiddelen voor installatie) kunnen maximaal 15 minuten worden bijgewerkt bij het herstellen van de Site ook aandacht.
- U kunt de laatste keer dat ontdekt controleren in het veld **Laatste CONTACTPERSOON op** voor de host van de server/ESXi vCenter op de pagina **Configuratie-Servers** .
- Als u een groep beveiliging al hebt gemaakt en voegt u een vCenter Server of ESXi host na die, duurt het vijftien minuten virtuele machines kunnen worden vermeld in het dialoogvenster **machines toevoegen aan een groep beveiliging** en voor de Site herstel van Azure-portal te vernieuwen.
- Als u doorgaan direct wilt met de computers toevoegen aan groep zonder te wachten de geplande discovery beveiliging, selecteert u de configuratieserver (Klik niet op deze) en klik op de knop **vernieuwen** .
- Wanneer u virtuele machines of fysieke machines aan een groep beveiliging toevoegt, wordt de processerver automatisch verplaatst en installeert u de service mobiliteit op de bronserver als de it niet al is geïnstalleerd.
- Zorg dat u hebt uw beveiligde machines ingesteld zoals is beschreven in de vorige stap voor het automatische push om om te werken.

Machines bijvoegen:

1. Klik op **Items beveiligde** > **Groep beveiliging** > **Machines** > **Machines toevoegen**. Als een goede gewoonte is het raadzaam dat beveiliging groepen uw werkbelasting gespiegelde moeten zodat u computers met een specifieke toepassing tot dezelfde groep toevoegen.
2. In de **virtuele Machines selecteren** als u fysieke servers beveiligen bent, in de wizard **Fysieke Machines toevoegen** vindt u de IP-adres en het beschrijvende naam. Selecteer het besturingssysteem gezin.

    ![V-beheercentrum server toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/physical-protect.png)

3. In **virtuele Machines selecteren** als u VMware virtuele machines beveiligen bent, selecteer een vCenter-server die wordt beheerd uw virtuele machines (of de EXSi host waarop ze werkt met) en selecteer vervolgens de machines.

    ![V-beheercentrum server toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/select-vms.png) 
4. Selecteer de basispagina doelservers en opslag voor replicatie te gebruiken en selecteert u of de instellingen moeten worden gebruikt voor alle werkbelasting in **Target Resources opgeven** . Selecteer [Premium opslag Account](../storage/storage-premium-storage.md) tijdens het configureren van beveiliging voor werkbelasting waarvoor consistente IO krachtige en lage latentie om te hosten IO intensief werkbelasting. Als u een account Premium opslagruimte voor uw schijven werkbelasting gebruiken wilt, moet u het doel van de basispagina van DS-reeks gebruiken. U kunt schijven voor Premium niet gebruiken met doel van de basispagina van niet-DS-reeks.

    >[AZURE.NOTE] We het verplaatsen van opslag-accounts die zijn gemaakt met behulp van de [nieuwe portal van Azure](../storage/storage-create-storage-account.md) via resourcegroepen wordt niet ondersteund.

    ![vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/machine-resources.png)

5. Selecteer het account dat u gebruiken wilt voor het installeren van de service mobiliteit op beveiligde machines in **Accounts opgeven** . De accountreferenties nodig zijn voor automatische installatie van de service mobiliteit. Als u geen een account Zorg ervoor dat selecteren instellen u een zoals is beschreven in stap 2. Houd er rekening mee dat dit account niet kan worden gebruikt door Azure. Voor Windows moet server het account beheerdersbevoegdheden hebt op de bronserver. Het account mag voor Linux hoofdmap.

    ![Linux-referenties](./media/site-recovery-vmware-to-azure-classic-legacy/mobility-account.png)

6. Klik op het vinkje hebt machines toegevoegd aan de groep beveiliging en te starten eerste replicatie voor elke computer. U kunt de status op de pagina **taken** controleren.

    ![V-beheercentrum server toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs2.png)

7. U kunt ook beveiliging status controleren door te klikken op **Beveiligde Items** > beveiliging groepsnaam > **virtuele Machines** . Wanneer de eerste replicatie is voltooid en de machines synchroniseert gegevens wordt ze **beveiligde** status weergegeven.

    ![VM taken](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs.png)


### <a name="set-protected-machine-properties"></a>Eigenschappen van de computer instellen die zijn beveiligd

1. Nadat een computer een **beveiligde** status heeft kunt u de eigenschappen failover configureren. Groepsdetails Selecteer van de computer en open het tabblad **configureren** in de beveiliging.
2. U kunt de naam die wordt gegeven op de computer in Azure wordt aangegeven na failover en de grootte van de Azure virtuele machines wijzigen. U kunt ook het Azure netwerk waaraan de computer is verbonden na failover selecteren.

    > [AZURE.NOTE] [Migratie van netwerken](../resource-group-move-resources.md) via resourcegroepen binnen hetzelfde abonnement of via abonnementen wordt niet ondersteund voor netwerken die zijn gebruikt voor het implementeren van sites worden hersteld.

    ![VM-eigenschappen instellen](./media/site-recovery-vmware-to-azure-classic-legacy/vm-props.png)

Houd rekening met:

- De naam van de Azure computer moet voldoen aan vereisten voor Azure.
- Al dan niet standaard niet gerepliceerde virtuele machines in Azure wordt aangegeven met een Azure netwerk bent verbonden. Als u repliceren wilt zorgen virtuele machines om te communiceren hetzelfde Azure netwerk instellen voor hen.
- Als u het formaat van een volume op VMware virtuele machine of fysieke server wordt deze verplaatst naar een kritieke toestand. Als u hoeft het formaat te wijzigen, doet u het volgende:

    - een) de instelling van de grootte wijzigen.
    - b) Selecteer de virtuele machine op het tabblad **virtuele Machines** en klikt u op **verwijderen**.
    - c) Selecteer de optie **beveiliging (wordt gebruikt voor herstel inzoomen en volume formaat) uitschakelen**in de **virtuele Machine verwijderen** . Deze optie uitschakelt beveiliging, maar blijven de herstel wordt verwezen in Azure wordt aangegeven.

        ![VM-eigenschappen instellen](./media/site-recovery-vmware-to-azure-classic-legacy/remove-vm.png)

    - d) beveiliging voor de virtuele machine inschakelen. Wanneer u opnieuw beveiliging inschakelen wordt de gegevens voor het volume van het formaat is gewijzigd worden overgebracht naar Azure.

    

## <a name="step-10-run-a-failover"></a>Stap 10: Een failover uitvoeren

U kunt momenteel alleen ongeplande failovers voor beveiligde VMware virtuele machines en fysieke servers uitvoeren. Houd rekening met het volgende:



- Zorg ervoor dat de configuratie en de hoofdlijst doel-servers actief zijn en in orde zijn voordat u een failover starten. Anders failover, mislukt.
- Bron machines worden niet als onderdeel van een niet-geplande failover afgesloten. Uitvoering van een niet-geplande failover drukt, stopt gegevensreplicatie voor de beveiligde servers. U moet de computers van de groep beveiliging verwijderen en opnieuw toevoegen om te beginnen met het beveiligen van machines opnieuw nadat de niet-geplande overname is voltooid.
- Als u wilt worden uitgevoerd zonder gegevens kwijt te raken, zorg dat aan de primaire site virtuele machines zijn uitgeschakeld voordat u de overname starten.

1. Klik op het **Herstel van abonnement** pagina en een herstel-abonnement toevoegen. Geef de details van het abonnement en **Azure** selecteert als het doel.

    ![Plan voor herstel configureren](./media/site-recovery-vmware-to-azure-classic-legacy/rplan1.png)

2. Selecteer een groep beveiliging in **VM selecteren** en selecteer machines in de groep toevoegen aan het abonnement herstel. [Lees meer](site-recovery-create-recovery-plans.md) over herstel abonnementen.

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic-legacy/rplan2.png)

3. Als nodig kunt u het plan voor het maken van groepen en sequentie de volgorde aanpassen in welke computers bij het herstellen van het abonnement worden overgenomen. U kunt ook aanwijzingen voor handmatige acties en scripts toevoegen. De scripts bij het herstellen naar Azure kunnen worden toegevoegd met behulp van [Azure automatisering Runbooks](site-recovery-runbook-automation.md).

4. Op de pagina **Herstel abonnementen** het abonnement en klik op **Niet-geplande Failover**.
5. Controleer in **Failover bevestigen** of de richting failover (naar Azure) en selecteer de komma herstel uitmaakt wordt overgenomen.
6. Wacht totdat de taak failover duren en controleer vervolgens of dat de overname werkt zoals verwacht en dat de gerepliceerde virtuele machines in Azure wordt aangegeven gestart.




## <a name="step-11-fail-back-failed-over-machines-from-azure"></a>Stap 11: Mislukt terug via machines van Azure is mislukt

[Meer informatie](site-recovery-failback-azure-to-vmware-classic-legacy.md) over hoe u uw mislukte overbrengen machines die worden uitgevoerd Azure terug naar uw on-premises omgeving.


## <a name="manage-your-process-servers"></a>Uw proces-servers beheren

De processerver herhaling gegevens verzendt naar de basispagina doelserver in Azure wordt aangegeven en ontdekt nieuwe VMware virtuele machines toegevoegd aan een vCenter-server. In de volgende gevallen wilt u mogelijk de processerver in uw implementatie wijzigen:

- Als het huidige processerver uitvalt
- Als uw herstel punt doelstelling (vrijgegeven Productieorder) naar een niet acceptabel niveau voor uw organisatie.

Desgewenst kunt u de replicatie van sommige of alle van uw on-premises implementatie VMware virtuele machines en fysieke servers verplaatsen naar een ander proces-server. Bijvoorbeeld:

- **Mislukt**, als de processerver van een mislukt of is niet beschikbaar, kunt u beveiligde machine herhaling verplaatsen naar een ander processerver. Metagegevens van de broncomputer en replica machine wordt verplaatst naar de nieuwe processerver en gegevens opnieuw is gesynchroniseerd. De nieuwe processerver verbinding automatisch met de server vCenter om uit te voeren automatisch opsporen. U kunt de status van proces-servers op het dashboard herstel van de Site controleren.
- **Taakverdeling om aan te passen vrijgegeven Productieorder**, voor betere taakverdeling u kunt een ander proces-server in de portal-Site herstel selecteren en replicatie van een of meer computers naar deze voor het handmatig taakverdeling verplaatsen. In dit geval wordt metagegevens van de geselecteerde bron- en replica computers verplaatst naar de nieuwe processerver. De oorspronkelijke processerver blijft verbonden met de server vCenter. 

### <a name="monitor-the-process-server"></a>De processerver bewaken

Als een processerver een waarschuwing status wordt weergegeven op de Site herstel Dashboard kritiek status is. U kunt klikken op de status voor het openen van het tabblad gebeurtenissen en vervolgens meer details voor specifieke taken op het tabblad taken. 

### <a name="modify-the-process-server-used-for-replication"></a>Het processerver gebruikt voor replicatie wijzigen

1. Open **Servers** > **Configuration Servers** > configuratieserver > **Server-gegevens**.
2. Klik op **Proces Servers** > **Proces-Server wijzigen** naast de server die u wilt wijzigen.

    ![Processerver 1 wijzigen](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps1.png)

3. In **De Server proces wijzigen** > **Proces doelserver** selecteert u de nieuwe server die u wilt gebruiken en selecteer vervolgens de virtuele machines die u wilt repliceren naar de nieuwe server. Klik op het informatiepictogram naast de naam van de server voor meer informatie over de beschikbare ruimte en gebruikte geheugen. De gemiddelde ruimte in die moet elke geselecteerde virtuele machine gerepliceerd naar de nieuwe processerver wordt weergegeven om te laden beslissingen.

    ![Processerver 2 wijzigen](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps2.png)

4. Klik op het vinkje om te beginnen met repliceren naar de nieuwe processerver. Opmerking Als u alle virtuele machines verwijdert uit een processerver die kritieke is deze een kritieke waarschuwing niet meer in het dashboard weergegeven moet.


## <a name="third-party-software-notices-and-information"></a>Kennisgeving over software van derden en informatie

Niet doen vertalen of Localize

De software en firmware die wordt uitgevoerd in de Microsoft-product of service is gebaseerd op of materiaal uit de onderstaande projecten implementeren (gezamenlijk "derden Code').  Microsoft is niet oorspronkelijke auteur van de Code van derden.  De oorspronkelijke copyrightmelding en de licentie, waaronder Microsoft dergelijke Code van derden, ontvangen worden vierde hieronder ingesteld.

De informatie in de sectie A, wordt met betrekking tot onderdelen van derden Code uit de onderstaande projecten. Deze licenties en de gegevens worden alleen ter informatie gegeven.  Deze derden-Code is voor u wordt relicensed door Microsoft onder de licentievoorwaarden voor Microsoft software voor de Microsoft-product of service.  

De informatie in de sectie B betreft derden Code-onderdelen die beschikbaar worden gesteld aan u door Microsoft onder de oorspronkelijke licentievoorwaarden.

Het volledige bestand kan worden gevonden op het [Microsoft Downloadcentrum](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft behoudt alle rechten niet specifiek verleend hierin, of door implicatie wordt ' estoppel ' of anderszins.
