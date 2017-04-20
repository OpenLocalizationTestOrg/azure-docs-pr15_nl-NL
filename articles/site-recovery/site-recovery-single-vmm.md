
<properties
    pageTitle="Azure Site herstel: Repliceren Hyper-V virtuele machines op één VMM server | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe Hyper-V virtuele machines repliceren als er slechts één VMM server."
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
    ms.workload="backup-recovery"
    ms.date="08/24/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-on-a-single-vmm-server"></a>Repliceren Hyper-V virtuele machines op één VMM server

Lees dit artikel voor meer informatie over het repliceren Hyper-V virtuele machines zich bevindt op een server van de host Hyper-V in een cloud-VMM wanneer u slechts één VMM server in uw implementatie.

Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources: Azure resourcemanager en klassiek. Azure, heeft ook twee portals – de Azure klassieke portal die ondersteuning biedt voor het implementatiemodel klassieke en de Azure-portal met ondersteuning voor beide implementatiemodellen. In dit artikel bevat instructies voor het instellen van herhaling in de portal van Azure.


Als u vragen hebt na het lezen van dit artikel, zet u ze in de opmerkingen Disqus onderaan van dit artikel of op het [forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Overzicht

Als u wilt repliceren Hyper-V VMs zich bevindt op Hyper-V hosts in VMM en er slechts één VMM server, kunt u [repliceren naar Azure](site-recovery-vmm-to-azure.md), of tussen wolken op de enkele VMM-server.

Het is raadzaam dat u naar Azure repliceren omdat failover- en herstelbestanden niet naadloze zijn wanneer repliceren tussen wolken en een aantal handmatige stappen nodig zijn. Als u repliceren met alleen de VMM-server wilt, kunt u het volgende doen:

- Repliceren met een enkel zelfstandige VMM-server
- Repliceren met één VMM server geïmplementeerd in een uitgerekt cluster van Windows


## <a name="replicate-across-sites-with-a-single-standalone-vmm-server"></a>Repliceren op sites met een enkel zelfstandige VMM-server

![Zelfstandige virtuele VMM server](./media/site-recovery-single-vmm/single-vmm-standalone.png)

In dit scenario kunt u de één VMM-server te implementeren als een virtuele machine in de primaire-site en deze VM repliceren naar een secundaire site sites worden hersteld en Hyper-V Replica gebruiken.

1. Het instellen van VMM op een VM Hyper-V. Het is raadzaam om dat u de SQL Server-instantie gebruikt door VMM op de dezelfde VM om tijd te besparen installeren. Als u wilt gebruiken een extern exemplaar van SQL Server en een storing, moet u die instantie herstellen voordat u VMM kunt herstellen.
2. Zorg ervoor dat de server VMM ten minste twee wolken geconfigureerd heeft. Eén cloud bevat de VMs u wilt repliceren en de andere cloud fungeert als locatie voor de secundaire. De cloud met de VMs die u wilt beveiligen nodig hebt:

    - Een of meer VMM hostgroepen met een of meer Hyper-V host-servers in elke hostgroep.
    - Ten minste één Hyper-V virtuele machine op elke Hyper-V host-server.

3. Maken van een kluis herstel Services, genereren en een kluis registratiegegevens-sleutel downloaden en de VMM-server in de kluis registreren. Tijdens de registratie kunt u de Provider van Azure Site herstel installeren op de server VMM.
4. Een of meer wolken op de VM VMM instellen en de Hyper-V hosts toevoegen aan deze wolken.
3. Instellingen voor documentbeveiliging voor de wolken configureren. U kunt de naam van de enkele VMM-server opgeven als de bronsite en doelsites locaties. Als u wilt configureren netwerk toewijzing, wijst u het netwerk VM voor de cloud met de VMs die u beveiligen wilt, bij het netwerk VM voor de cloud herhaling.
4. Eerste replicatie voor VMs die u beveiligen via het netwerk wilt omdat beide wolken op de server bevinden zich inschakelen.
4. In de beheerconsole van Hyper-V Hyper-V Replica inschakelen op de host van Hyper-V waarin de VM VMM en herhaling op de VM inschakelen. Controleer of dat u niet de VM VMM toevoegen aan wolken die zijn beveiligd met sites worden hersteld. Dit zorgt ervoor dat Hyper-V Replica instellingen worden niet door de sites worden hersteld overschreven.
5. Als u maken van herstel-abonnementen wilt, kunt u dezelfde VMM-server voor de bronsite en doelsites opgeven.

Volg de instructies in [dit artikel](site-recovery-vmm-to-vmm.md) om te maken van een kluis, de server registreren en beveiliging instellen.

### <a name="what-to-do-in-an-outage"></a>Wat moet u doen in een storing

Als een volledige storing plaatsvindt en u moet werken uit de secundaire site, voer de volgende handelingen uit:

1.  Voer een niet-geplande failover mislukt via de VM VMM van de primaire naar secundaire site in de console Hyper-V-beheer in de secundaire site.
2.  Controleer of de VM VMM slag op de secundaire site.
3.  Voer een niet-geplande failover mislukt via de werklast VMs van de primaire naar secundaire wolken in de kluis herstel Services. U voltooit de niet-geplande overname van de VMs, doorvoeren van de overname of Selecteer een andere herstelpunt zoals vereist.
4.  Wanneer de niet-geplande overname voltooid is, de gebruikers hebben toegang tot werkbelasting resources in de secundaire site worden weergegeven.

Wanneer de primaire site normaal opnieuw functioneert, het volgende doen:

1.  In de beheerconsole van Hyper-V inschakelen omgekeerde herhaling voor de VM VMM gerepliceerd deze secundaire naar primaire starten.
2.  Voer een geplande failover om mislukt weer de VM VMM naar de primaire site in de beheerconsole van Hyper-V. De overname als u deze voltooit wilt doorvoeren. Schakel omgekeerde herhaling starten de VMM van primaire repliceren naar secundaire.
3.  In de kluis herstel Services inschakelen omgekeerde replicatie voor de werklast VMs, om te beginnen gerepliceerd ze secundaire naar primaire.
4.  In de kluis herstel Services uitgevoerd een geplande failover om mislukt weer de werklast VMs naar de primaire-site. De overname als u deze voltooit wilt doorvoeren. Schakel omgekeerde herhaling starten de werklast VMs van primaire repliceren naar secundaire.



## <a name="replicate-across-sites-with-a-single-vmm-server-in-a-stretched-cluster"></a>Repliceren op sites met één VMM server in een uitgerekt cluster

![Gegroepeerd virtuele VMM-server](./media/site-recovery-single-vmm/single-vmm-cluster.png)

In plaats van een zelfstandige VMM server wordt geïmplementeerd als een VM die wordt overgenomen door een secundaire site, kunt u VMM ten zeerste beschikbaar maken door deze als een VM in een Windows-failovercluster implementeren. Dit biedt flexibiliteit werkbelasting en bescherming tegen hardware is mislukt. Als u wilt implementeren met Site-herstel moet de VM VMM worden geïmplementeerd in een uitrekken cluster geografisch afzonderlijke sites. Dit wilt doen:

1. VMM installeren op een virtuele machine in een Windows-failovercluster en selecteer de optie voor het uitvoeren van de server als ten zeerste beschikbaar tijdens de installatie.
2. De SQL Server-instantie die wordt gebruikt door VMM moet worden gerepliceerd met SQL Server AlwaysOn beschikbaarheidsgroepen, zodat er een replica van de database in de secundaire site wordt.
3. Volg de instructies in [dit artikel](site-recovery-vmm-to-vmm.md) om te maken van een kluis, de server registreren en beveiliging instellen. U moet elke server VMM registreren in het cluster in de kluis herstel Services. Hiervoor kunt u de Provider installeren op een actief knooppunt, en de server VMM registreren. Vervolgens installeert u de Provider op andere knooppunten.

### <a name="what-to-do-in-an-outage"></a>Wat moet u doen in een storing

Wanneer een storing optreedt, worden de VMM-server en de bijbehorende SQL Server-database overgenomen en toegankelijk vanaf de secundaire site.


## <a name="next-steps"></a>Volgende stappen

[Meer informatie](site-recovery-vmm-to-vmm.md) over gedetailleerde Site herstel implementatie voor VMM VMM replicatie.
