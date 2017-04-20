<properties
    pageTitle="Hyper-V replicatie met Azure Site herstel | Microsoft Azure"
    description="Lees dit artikel voor meer informatie over de technische concepten die u kunt installeren, configureren en beheren van Azure sites worden hersteld."
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="mkjain"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/12/2016"
    ms.author="rajanaki"/>  


# <a name="hyper-v-replication-with-azure-site-recovery"></a>Hyper-V replicatie met Azure sites worden hersteld

In dit artikel worden de technische concepten die u kunt configureren en beheren van een Hyper-V-site of een site systeem Center Virtual Machine Manager (VMM) van Azure beveiliging met behulp van Azure Site herstel kunnen helpen.

## <a name="setting-up-the-source-environment-for-replication-between-on-premises-and-azure"></a>Bij het instellen van het bronomgeving voor replicatie tussen on-premises en Azure

Als onderdeel van het instellen van herstel tussen on-premises en Azure, zorg ervoor dat u downloadt en installeert Azure Site herstel Provider op de server VMM. Het installeren van Azure herstel Services Agent op elke Hyper-V-host.

![Site-implementatie VMM voor replicatie tussen on-premises en Azure](media/site-recovery-understanding-site-to-azure-protection/image00.png)

Bij het instellen van de bronomgeving in een Hyper-V beheerde-infrastructuur is vergelijkbaar met het instellen van de bronomgeving in een beheerde infrastructuur VMM. De enige verschil is dat de provider en agent zijn geïnstalleerd op de host van Hyper-V zelf. In de omgeving VMM, de provider op de server VMM is geïnstalleerd en de-agent is geïnstalleerd op de hosts Hyper-V (voor het geval replicatie Azure).

## <a name="workflows"></a>Werkstromen

### <a name="enable-protection"></a>Beveiliging inschakelen
Nadat u een virtuele machine tegen de Azure beheerportal of on-premises implementatie beschermen, wordt er een taak Site herstel **Beveiliging inschakelen** gestart. U kunt dit controleren onder het tabblad **taken** .

![De taak 'Beveiliging inschakelen' in de lijst met taken](media/site-recovery-understanding-site-to-azure-protection/image001.PNG)

De taak **Beveiliging inschakelen** Hiermee wordt gecontroleerd op de vereisten voordat de methode [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) . Deze methode maakt replicatie Azure met behulp van de invoer die zijn geconfigureerd tijdens beveiliging.

De taak **Beveiliging inschakelen** Hiermee start u de eerste replicatie vanuit on-premises aanroepen van de methode [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) . Deze methode verzendt de VM virtuele schijven naar Azure.

![Meer informatie voor de taak "Beveiliging inschakelen"](media/site-recovery-understanding-site-to-azure-protection/IMAGE002.PNG)

### <a name="finalize-protection-on-the-virtual-machine"></a>Beveiliging op de virtuele machine voltooien
Een [momentopname van Hyper-V VM](https://technet.microsoft.com/library/dd560637.aspx) is, wordt als eerste replicatie wordt geactiveerd. Virtuele vaste schijven zijn verwerkte één voor één totdat alle schijven zijn geüpload naar Azure. Hiermee neemt tijd om te voltooien, op basis van de grootte van de schijf en de bandbreedte. Zie uw om netwerkgebruik te optimaliseren, [het beheren van on-premises implementatie naar Azure beveiliging netwerkbandbreedte gebruikt](https://support.microsoft.com/kb/3056159).

Nadat de eerste replicatie is voltooid, configureert de taak **Voltooien beveiliging op de virtuele machine** de netwerk- en post replicatie-instellingen. Terwijl de eerste replicatie wordt uitgevoerd:

- Alle wijzigingen in de schijven worden bijgehouden. 
- Extra schijfruimte verbruikt voor de momentopname en Hyper-V Replica Log (HRL)-bestanden.

Klik op de eerste replicatie is voltooid, wordt de momentopname Hyper-V VM verwijderd. Deze verwijdering levert gegevenswijzigingen samenvoegen na aanvankelijke replicatie de bovenliggende schijf.

!['Beveiliging op de virtuele machine voltooien' taak in de lijst met taken](media/site-recovery-understanding-site-to-azure-protection/image03.png)

### <a name="delta-replication"></a>Delta herhaling
Hyper-V Replica herhaling Tracker deel uit van de Hyper-V Replica replicatie-Engine, nummers de wijzigingen aan een virtuele harde schijf als Hyper-V Replica Log (*.hrl)-bestanden maakt. HRL bestanden zijn opgeslagen in dezelfde map als de bijbehorende schijven.

Elke schijf die geconfigureerd voor replicatie heeft een bijbehorend HRL-bestand. Dit logboek wordt verzonden naar de opslag van het klantaccount wanneer eerste replicatie voltooid is. Wanneer een logboek onderweg zijn naar Azure is, worden de wijzigingen in de primaire in een ander logboekbestand in dezelfde map worden bijgehouden.

Tijdens de eerste replicatie of delta herhaling, kunt u VM herhaling servicestatus in de weergave VM controleren, zoals in de [Monitor herhaling servicestatus voor virtuele machine](./site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machine)is vermeld.  

### <a name="resynchronization"></a>Opnieuw synchroniseren
Een virtuele machine is gemarkeerd voor het opnieuw wanneer zowel delta replicatie is mislukt en volledige eerste replicatie erg nadelig voor netwerkbandbreedte of tijd is. Bijvoorbeeld wanneer de bestandsgrootte HRL stapels maximaal 50 procent van de totale schijfruimte, is de virtuele machine gemarkeerd voor het opnieuw. Het opnieuw wordt de hoeveelheid gegevens die zijn verzonden via het netwerk door controlesommen van de bronsite en doelsites VM schijven en alleen het verschil verzenden geminimaliseerd.

Nadat het opnieuw is voltooid, moet de normale delta herhaling hervatten. U kunt het opnieuw weer als een netwerkstoring of een andere storing zich daadwerkelijk voordoet.

Standaard is automatisch geplande opnieuw synchroniseren geconfigureerd optreden buiten kantooruren. Als de virtuele machine opnieuw handmatig worden gesynchroniseerd moet, selecteer de virtuele machine in de portal en klik op **synchroniseren**.

![Handmatig opnieuw synchroniseren](media/site-recovery-understanding-site-to-azure-protection/image04.png)

Het opnieuw gebruikt een vaste blok chunking algoritme waar bronsite en doelsites bestanden vaste stukken zijn verdeeld. Controlesommen voor elk segment worden gegenereerd en vervolgens vergeleken om te bepalen welke blokken uit de bron moeten worden toegepast op het doel.

### <a name="retry-logic"></a>Probeer logica
Er is een ingebouwde opnieuw logica voor replicatiefouten. Deze logica kan worden ingedeeld in twee categorieën:

| Categorie                  | Scenario 's                                    |
|---------------------------|----------------------------------------------|
| Niet-herstelbare fout     | Geen nieuwe poging wordt gedaan. VM herhaling status is **kritiek**en tussenkomst van de beheerder is vereist. Voorbeelden hiervan zijn: <ul><li>Verbroken VHD ketting</li><li>Ongeldige status voor de replica virtuele machine</li><li>Fout-verificatie</li><li>Autorisatiefout</li><li>Virtuele machine die niet kan worden gevonden, in het geval van een zelfstandige Hyper-V server</li></ul>|
| Herstelbare fout         | Nieuwe pogingen optreden elke replicatie-interval, met een exponentiële back-uitschakelen die het interval nieuwe vanaf het begin van de eerste poging (1, 2, 4, 8: 10 minuten vergroot). Als u een fout zich blijft voordoen, probeer u elke 30 minuten. Voorbeelden hiervan zijn: <ul><li>Fout</li><li>Er voldoende schijfruimte</li><li>Onvoldoende geheugen</li></ul>|

## <a name="hyper-v-virtual-machine-protection-and-recovery-life-cycle"></a>VM Hyper-V beveiliging en herstelbestanden cyclus

![VM Hyper-V beveiliging en herstelbestanden cyclus](media/site-recovery-understanding-site-to-azure-protection/image05.png)

## <a name="other-references"></a>Andere verwijzingen

- [Bewaken en beveiliging voor VMware, VMM, Hyper-V en fysieke sites oplossen](./site-recovery-monitoring-and-troubleshooting.md)
- [Voor Microsoft Support brengen](./site-recovery-monitoring-and-troubleshooting.md#reaching-out-for-microsoft-support)
- [Veelvoorkomende fouten in Azure Site herstel en hun oplossingen](./site-recovery-monitoring-and-troubleshooting.md#common-asr-errors-and-their-resolutions)
