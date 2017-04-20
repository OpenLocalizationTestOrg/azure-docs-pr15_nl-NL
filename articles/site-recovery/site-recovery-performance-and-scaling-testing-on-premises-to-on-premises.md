<properties
    pageTitle="Prestaties testen en schaal van het resultaat on-premises naar on-premises implementatie Hyper-V replicatie met Site-herstel | Microsoft Azure"
    description="In dit artikel vindt u informatie over de prestaties van testen voor on-premises implementatie on-premises implementatie replicatie met Azure sites worden hersteld."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/06/2016"
    ms.author="raynew"/>

# <a name="performance-test-and-scale-results-for-on-premises-to-on-premises-hyper-v-replication-with-site-recovery"></a>Prestaties testen en de schaal van resultaten on-premises naar on-premises implementatie Hyper-V replicatie met herstel van de Site aanpassen

U kunt Microsoft Azure Site herstellen goedkeuringen en beheren van replicatie van virtuele machines en fysieke servers met Azure en met een secundaire datacenter. In dit artikel vindt u de resultaten van de prestaties testen Hyper-V virtuele machines repliceren tussen twee toen on-premises implementatie datacenters.



## <a name="overview"></a>Overzicht

Het doel van testen is om te bekijken hoe Azure Site herstel tijdens de constante status replicatie wordt uitgevoerd. Constante status herhaling treedt op wanneer virtuele machines eerste replicatie is voltooid en deltawijzigingen worden gesynchroniseerd. Het is belangrijk om met een constante status omdat dit de status waarin de meeste virtuele machines blijven is, tenzij onverwachte bijvoorbeeld optreden prestaties te meten.


De implementatie test waren van twee on-premises implementatie-sites met een server VMM in elke site. De implementatie van deze toets wordt gebruikt voor een hoofd office/tak office-implementatie, met hoofdkantoor fungeert als de primaire-site en het kantoor in als de secundaire of herstel-site.

### <a name="what-we-did"></a>We hebben gedaan

Hier ziet u wat we is in de test geslaagd:

1. Virtuele machines met VMM sjablonen gemaakt.

1. Virtuele machines en vastleggen basislijn-prestatiemetingen meer dan 12 uur gestart.

1. Gemaakte wolken op primair en herstelbestanden VMM-servers.

1. De beveiliging van de geconfigureerde cloud in Azure sites worden hersteld, met inbegrip van de toewijzing van de bron- en herstelbestanden wolken.

1. Beveiliging voor virtuele machines ingeschakeld en hen in staat stellen eerste replicatie is voltooid.

1. Een paar uur voor systeem stabilisatie gewacht.

1. Vastgelegd prestatiegegevens meer dan 12 uur, ervoor te zorgen dat alle virtuele machines bleef verwachte herhaling status voor deze 12 uur.

1. Meet de delta tussen de prestatiegegevens volgens de basislijn en de prestatiegegevens herhaling.

## <a name="test-deployment-results"></a>Testresultaten voor implementatie

### <a name="primary-server-performance"></a>Primaire serverprestaties

- Wijzigingen op een logboekbestand met minimale opslagruimte realiseren Hyper-V Replica asynchroon bijgehouden op de primaire-server.

- Selfservice onderhouden cachegeheugen om te beperken IO's / s belasting voor voortgangscontrole wordt gebruikgemaakt van Hyper-V Replica. Opgeslagen gegevens worden geschreven naar de VHDX in het geheugen en deze in het logboekbestand voordat u de tijd die het logboek wordt verzonden naar de site herstel wordt gewist. Een schijf leegmaken gebeurt ook als een vooraf ingestelde limiet bereikt, wordt het schrijven.

- Het volgende diagram ziet u de constante status IO's / s realiseren voor replicatie. U ziet dat de IO's / s belasting vanwege replicatie is ongeveer 5%, dat wil zeer beperkt zeggen.

![Primaire resultaten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744913.png)

Hyper-V Replica gebruikt geheugen op de primaire server schijf optimaliseren. Zoals u in de volgende graph, is het geheugen belasting op alle servers in de primaire cluster randnummer. Het geheugen belasting wordt weergegeven, is het percentage van geheugen die wordt gebruikt door herhaling vergeleken met het totale geïnstalleerde geheugen op de server Hyper-V.

![Primaire resultaten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744914.png)

Hyper-V Replica heeft minimale CPU realiseren. Zoals u in de grafiek, is replicatie realiseren in het bereik van 2-3%.

![Primaire resultaten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744915.png)

### <a name="secondary-recovery-server-performance"></a>Prestaties van de server secundair (herstellen)

Hyper-V Replica gebruikt een kleine hoeveelheid geheugen op de herstelserver het aantal opslagbewerkingen optimaliseren. In de grafiek bevat een overzicht van de geheugengebruik op de herstelserver. Het geheugen belasting wordt weergegeven, is het percentage van geheugen die wordt gebruikt door herhaling vergeleken met het totale geïnstalleerde geheugen op de server Hyper-V.

![Secundaire resultaten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744916.png)

De hoeveelheid i/o-bewerkingen op de site herstel is een functie van het aantal schrijven bewerkingen op de primaire-site. Laten we kijkt u naar de totale i/o-bewerkingen op de site herstel in vergelijking met het totale i/o-activiteiten en schrijven bewerkingen op de primaire-site. De grafieken wordt aangegeven dat het totaal IO's / s op de site herstel is

- Rond 1,5 keer het schrijven IO's / s op de primaire.

- Ongeveer 37% van de totale IO's / s op de primaire-site.

![Secundaire resultaten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744917.png)

![Secundaire resultaten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744918.png)

### <a name="effect-of-replication-on-network-utilization"></a>Effect van replicatie op netwerk in gebruik

Een gemiddelde van 275 MB per seconde netwerkbandbreedte is ten opzichte van een bestaande bandbreedte van 5 GB per seconde tussen de primaire en herstelbestanden knooppunten (met gebruik van compressie) gebruikt.

![Resultaten van netwerk in gebruik](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744919.png)

### <a name="effect-of-replication-on-virtual-machine-performance"></a>Effect van replicatie op VM prestaties

Een belangrijke overweging is de invloed van replicatie op productie werkbelasting waarop de virtuele machines. Als de primaire site voldoende voor replicatie is ingericht, niet mag er een invloed op de werkbelasting. Van Hyper-V Replica lightweight bijhouden om zorgt ervoor dat werkbelasting uitgevoerd in de virtuele machines niet tijdens de constante-status replicatie worden beïnvloed. Dit is geïllustreerd in de volgende grafieken.

Deze grafiek worden weergegeven IO's / s uitgevoerd door virtuele machines met verschillende werkbelastingen voordat en nadat replicatie is ingeschakeld. U kunt zien dat er geen verschil is tussen de twee is.

![Replica effect resultaten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744920.png)

Het volgende diagram ziet u de doorvoer van virtuele machines met verschillende werkbelastingen voordat en nadat replicatie is ingeschakeld. U kunt zien dat die herhaling heeft geen gevolgen voor aanzienlijk.

![Resultaten replica effecten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744921.png)

### <a name="conclusion"></a>Sluiten

De resultaten duidelijk dat Azure sites worden hersteld, in combinatie met Hyper-V Replica, afhankelijk zijn ook van minimum belasting voor een grote cluster.  Azure Site herstel biedt eenvoudige implementatie, herhaling, beheer en controle. Hyper-V Replica biedt de noodzakelijke infrastructuur voor replicatie schaalbaarheid. Voor een optimale implementatie planning wordt u aangeraden dat u de [Hyper-V Replica capaciteit teamplanner](https://www.microsoft.com/download/details.aspx?id=39057)downloaden.

## <a name="test-environment-details"></a>Details van de omgeving

### <a name="primary-site"></a>Primaire-site

- De primaire site heeft een cluster met vijf wordt uitgevoerd 470 virtuele machines Hyper-V-servers.

- De virtuele machines uitvoeren verschillende werkbelasting en hebben allemaal Azure Site hersteld is ingeschakeld.

- Opslag voor het clusterknooppunt wordt geleverd door samen. Model – Hitachi HUS130.

- Elke clusterserver heeft vier netwerkkaarten (NIC's) van één Gbps.

- Twee van de netwerkkaarten zijn verbonden met een iSCSI-privé-netwerk en twee verbonden zijn met een externe bedrijfsnetwerk. Een van de externe netwerken is gereserveerd voor alleen clustercommunicatie.

![Primaire hardwarevereisten](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744922.png)

|Server|RAM|Model|Processor|Het aantal processors|NIC|Software|
|---|---|---|---|---|---|---|
|Hyper-V servers in cluster: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25|128ESTLAB-HOST25 heeft 256|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) processor E5-4620 0 @ 2,20 GHz|4|Ik GB/s x 4|Windows Server Datacenter 2012 R2 (x64) + functie Hyper-V|
|VMM Server|2|||2|1 GB/s|Windows Server-Database 2012 R2 (x 64) + VMM 2012 R2|

### <a name="secondary-recovery-site"></a>Secundaire (herstellen)-site

- De secundaire site heeft een zes failovercluster.

- Opslag voor het clusterknooppunt wordt geleverd door samen. Model – Hitachi HUS130.

![Primaire hardware specificatie](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744923.png)

|Server|RAM|Model|Processor|Het aantal processors|NIC|Software|
|---|---|---|---|---|---|---|
|Hyper-V servers in cluster: <br />ESTLAB-HOST07<br />ESTLAB-HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10|96|Dell™ PowerEdge™ R720|Intel(R) Xeon(R) processor E5-2630 0 @ 2,30 GHz|2|Ik GB/s x 4|Windows Server Datacenter 2012 R2 (x64) + functie Hyper-V|
|ESTLAB-HOST17|128|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) processor E5-4620 0 @ 2,20 GHz|4||Windows Server Datacenter 2012 R2 (x64) + functie Hyper-V|
|ESTLAB-HOST24|256|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) processor E5-4620 0 @ 2,20 GHz|2||Windows Server Datacenter 2012 R2 (x64) + functie Hyper-V|
|VMM Server|2|||2|1 GB/s|Windows Server-Database 2012 R2 (x 64) + VMM 2012 R2|

### <a name="server-workloads"></a>Server werkbelasting

- Voor testdoeleinden gekozen we werkbelasting vaak gebruikt in de enterprise-klant-scenario's.

- We [IOMeter](http://www.iometer.org) gebruiken met de werklast-kenmerk in de tabel voor simulatie samengevat.

- Alle IOMeter profielen zijn ingesteld op willekeurig bytes simuleren slechtste schrijven patronen voor werkbelasting schrijven.

|Werkbelasting|I/o-grootte (KB)|% Access|% Lezen|Uitstaande o|I/o-patroon|
|---|---|---|---|---|---|
|Bestandsserver|48163264|60 20 %5 %5% 10%|80% 80% 80% 80% 80%|88888 verwijderd|Alle 100% willekeurig|
|SQL Server (volume 1) SQL Server (volume 2)|864|100% 100%|70 %0%|88|100% random100% opeenvolgende|
|Exchange|32|100%|67%|8|100% willekeurig|
|Workstation/VDI|464|66% 34%|95% van 70%|11|Beide willekeurig 100%|
|Bestand webserver|4864|33 34% 33%|95% 95% 95%|888|Alle 75% willekeurig|

### <a name="virtual-machine-configuration"></a>VM configuratie

- 470 virtuele machines op de primaire cluster.

- Alle virtuele machines met VHDX schijf.

- Virtuele machines werkbelasting in de tabel is samengevat uitgevoerd. Alle zijn gemaakt met VMM sjablonen.

|Werkbelasting|# VMs|Minimale RAM (GB)|Maximale RAM (GB)|Logische schijfgrootte (GB) per VM|Maximale IO's / s|
|---|---|---|---|---|---|
|SQL Server|51|1|4|167|10|
|Exchange-Server|71|1|4|552|10|
|Bestandsserver|50|1|2|552|22|
|VDI|149|.5|1|80|6|
|Webserver|149|.5|1|80|6|
|TOTAAL|470|||96.83 TB|4108|

### <a name="azure-site-recovery-settings"></a>Azure herstel van de Site-instellingen

- Azure herstel van de Site is geconfigureerd voor on-premises implementatie naar on-premises implementatie-beveiliging

- De server VMM heeft vier wolken geconfigureerd, met de servers van de cluster Hyper-V en hun virtuele machines.

|Primaire VMM cloud|Beveiligde virtuele machines in de cloud|Frequentie van replicatie|Aanvullende herstel wordt verwezen|
|---|---|---|---|
|PrimaryCloudRpo15m|142|15 minuten|Geen|
|PrimaryCloudRpo30s|47|30 seconden|Geen|
|PrimaryCloudRpo30sArp1|47|30 seconden|1|
|PrimaryCloudRpo5m|235|5 minuten|Geen|

### <a name="performance-metrics"></a>Prestatiegegevens

De tabel bevat een overzicht van de prestatiegegevens en items die zijn gemeten in de implementatie.

|Metrisch|Item|
|---|---|
|CPU|\Processor(_Total)\% processortijd|
|Beschikbare geheugen|\Memory\Available megabytes (MB)|
|IO 'S/S|\PhysicalDisk (_Totaal) \Disk overdrachten/sec|
|VM (IO's / s) bewerkingen/sec lezen|\Hyper-V virtuele opslag-apparaat (<VHD>) \Read bewerkingen/Sec|
|VM schrijven (IO's / s) bewerkingen/sec|\Hyper-V virtuele opslag-apparaat (<VHD>) \Write bewerkingen/S|
|VM doorvoer lezen|\Hyper-V virtuele opslag-apparaat (<VHD>) \Read Bytes/sec|
|VM schrijfbewerkingen|\Hyper-V virtuele opslag-apparaat (<VHD>) \Write Bytes/sec|


## <a name="next-steps"></a>Volgende stappen

- [Beveiliging tussen twee on-premises implementatie VMM sites instellen](site-recovery-vmm-to-vmm.md)
