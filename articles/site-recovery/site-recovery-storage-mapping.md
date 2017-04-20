<properties
    pageTitle="Opslagruimte in Azure Site herstel voor Hyper-V VM replicatie tussen on-premises implementatie datacenters toewijzen | Microsoft Azure"
    description="Bereid opslag toewijzing voor Hyper-V VM replicatie tussen twee on-premises implementatie datacenters met Azure sites worden hersteld."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/06/2016"
    ms.author="raynew"/>


# <a name="prepare-storage-mapping-for-hyper-v-virtual-machine-replication-between-two-on-premises-datacenters-with-azure-site-recovery"></a>Opslag toewijzing voor Hyper-V VM replicatie tussen twee on-premises implementatie datacenters met Azure Site herstel voorbereiden


Azure Site herstel draagt bij aan uw strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR) door orchestrating herhaling, failover en herstel van virtuele machines en fysieke servers. In dit artikel beschrijft opslag toewijzing, waardoor u optimaal gebruik van opslag als u Site herstel Hyper-V virtuele machines repliceren tussen twee on-premises implementatie VMM datacenters gebruikt.

Opmerkingen of vragen onder van dit artikel of op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)posten.

## <a name="overview"></a>Overzicht

Opslag toewijzing is alleen van toepassing wanneer u bent repliceren van Hyper-V virtuele machines die zich bevinden in VMM wolken uit een primaire datacenter met een secundaire datacenter met Hyper-V Replica of SAN herhaling, als volgt:


- **On-premises on-premises implementatie replicatie met Hyper-V Replica)** — U instellen opslag toewijzing door toewijzing opslag classificaties op een bron en afstemmen VMM-servers om het volgende te doen:

    - **Identificeer doel opslag voor replica virtuele machines**, virtuele machines worden gerepliceerd naar een doelsite opslag (SMB delen of gedeelde volumes (CSVs) cluster) die u kiest.
    - **Replica VM plaatsing**, opslag toewijzing wordt gebruikt om te plaatsen optimaal replica virtuele machines op Hyper-V hostservers. Replica virtuele machines wordt geplaatst op hosts die toegang heeft tot de toegewezen opslag-indeling.
    - **Geen opslag-toewijzing**, als u geen opslagruimte toewijzing configureert, virtuele machines moet worden gerepliceerd naar de standaardopslaglocatie opgegeven op de Hyper-V host-server die is gekoppeld aan de replica virtuele machine.

- **On-premises naar on-premises implementatie replicatie met SAN**, u instellen opslag toewijzing door toewijzing opslag matrices van toepassingen op een bron- en doellijst VMM-servers.
    - **Geef toepassingen**, bepaalt welke toepassingen secundaire opslag herhaling gegevens van de primaire groep ontvangt.
    - **Identificeer doel opslag van toepassingen**, zorgt ervoor dat LUN's in een bron herhaling groep worden gerepliceerd naar toegewezen doel-groep van uw keuze.

## <a name="set-up-storage-classifications-for-hyper-v-replication"></a>Instellen opslag classificaties voor replicatie Hyper-V

Wanneer u Hyper-V Replica repliceren met Site-herstel gebruikt, wijst u tussen opslag classificaties op de bronsite en doelsites VMM-servers of op een enkele VMM-server als twee sites worden beheerd door dezelfde VMM-server. Houd rekening met:

- Als toewijzing correct is geconfigureerd en replicatie is ingeschakeld, wordt een VM virtuele harde schijf op de primaire locatie worden gerepliceerd naar opslag in de toegewezen doellocatie.
- Opslag classificaties moeten zijn beschikbaar voor de van hostgroepen in de bronsite en doelsites wolken.
- Classificaties hoeft te hebben van hetzelfde type opslag. U kunt bijvoorbeeld een bron-indeling met SMB aandelen een doel-indeling met CSVs afzetten.
- Lees meer in [het maken van classificaties opslagruimte in VMM](https://technet.microsoft.com/library/gg610685.aspx).

## <a name="example"></a>Voorbeeld

Als classificaties correct zijn geconfigureerd in VMM wanneer u de bronsite en doelsites VMM server tijdens opslag toewijzing selecteert, wordt de bronsite en doelsites classificaties worden weergegeven. Hier volgt een voorbeeld van opslag bestandsshares en classificaties voor een organisatie met twee locaties in New York en Chicago.

**Locatie** | **VMM server** | **Bestand delen (bron)** | **Classificatie (bron)** | **Toegewezen aan** | **Bestandsshare (doel)**
---|---|--- |---|---|---
New York | VMM_Source| SourceShare1 | GOUD | GOLD_TARGET | TargetShare1
 |  | SourceShare2 | SILVER | SILVER_TARGET | TargetShare2
 | | SourceShare3 | BRONZE | BRONZE_TARGET | TargetShare3
Chicago | VMM_Target |  | GOLD_TARGET | Niet toegewezen |
| | | SILVER_TARGET | Niet toegewezen |
 | | | BRONZE_TARGET | Niet toegewezen

U kunt deze op het tabblad **Server opslag** zou configureren op de pagina **Resources** van de portal-Site herstel.

![Opslag toewijzing configureren](./media/site-recovery-storage-mapping/storage-mapping1.png)

Met dit voorbeeld is:
- Wanneer een replica virtuele machines wordt gemaakt voor een virtuele machine op goud opslag (SourceShare1), wordt deze gerepliceerd naar een GOLD_TARGET opslag (TargetShare1).
- Wanneer een replica virtuele machine wordt gemaakt voor een virtuele machine op zilver opslag (SourceShare2), deze moet worden gerepliceerd naar een opslag SILVER_TARGET (TargetShare2), enzovoort.

De werkelijke bestandsshares en hun toegewezen classificaties in VMM weergegeven in de volgende schermopname.

![Classificaties opslagruimte in VMM](./media/site-recovery-storage-mapping/storage-mapping2.png)

## <a name="multiple-storage-locations"></a>Meerdere opslaglocaties

Als de doel-indeling met meerdere waarden voor aandelen SMB of CSVs is toegewezen, wordt de opslaglocatie van optimale automatisch geselecteerd wanneer de virtuele machine is beveiligd. Als u geen geschikte doel-opslag is beschikbaar voor communicatie met de opgegeven indeling, wordt de standaardopslaglocatie opgegeven op de host Hyper-V wordt gebruikt voor het plaatsen van de replica virtuele vaste schijven.

De volgende tabel wordt weergegeven hoe opslag-indeling en gedeelde clustervolumes zijn ingesteld in ons voorbeeld.

**Locatie** | **Classificatie** | **Gekoppeld opslag**
---|---|---
New York | GOUD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p>
 | SILVER | <p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p>
Chicago | GOLD_TARGET | <p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p>
 | SILVER_TARGET| <p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p>

In deze tabel bevat een overzicht van het gedrag wanneer u beveiliging voor virtuele machines (VM1 - VM5) in dit Voorbeeldomgeving inschakelt.

**VM** | **Bron-opslag** | **Bron classificatie** | **Toegewezen doel-opslag**
---|---|---|---
VM1 | C:\ClusterStorage\SourceVolume1 | GOUD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Beide GOLD_TARGET</p>
VM2 | \\FileServer\SourceShare1 | GOUD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Beide GOLD_TARGET</p>
VM3 | C:\ClusterStorage\SourceVolume2 | SILVER | <p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p>
VM4 | \FileServer\SourceShare2 | SILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Beide SILVER_TARGET</p>
VM5 | C:\ClusterStorage\SourceVolume3 | N/B | Geen toewijzing, zodat de standaardlocatie voor de opslag van de host Hyper-V wordt gebruikt

## <a name="next-steps"></a>Volgende stappen

Nu dat u een beter begrip van de toewijzing van opslag hebt, [voorbereiden op het implementeren van Azure sites worden hersteld](site-recovery-best-practices.md).
