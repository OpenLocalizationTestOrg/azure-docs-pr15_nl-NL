<properties
    pageTitle="IaaS Azure virtuele machines van de ene Azure regio migreren naar de andere met Site-herstel | Microsoft Azure"
    description="Gebruik Azure Site herstel IaaS Azure virtuele machines migreren van één Azure regio naar de andere."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="raynew"/>

#  <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>IaaS Azure virtuele machines tussen Azure regio's met Azure Site herstel migreren

## <a name="overview"></a>Overzicht

Welkom bij Azure Site herstel! Lees dit artikel als u wilt migreren van Azure VMs tussen Azure regio's. Houd rekening met het volgende voordat u begint:

- Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: Azure resourcemanager en klassiek. Azure, heeft ook twee portals – de Azure klassieke portal die ondersteuning biedt voor het implementatiemodel klassieke en de Azure-portal met ondersteuning voor beide implementatiemodellen. De basisstappen voor migratie zijn hetzelfde, ongeacht of u Site herstel in resourcemanager of in de klassieke configureren bent. Echter de UI-instructies en schermafbeeldingen in dit artikel relevant zijn voor de Azure-portal.
- **Momenteel kunt u alleen migreren vanuit één regio naar de andere. U kunt niet via VMs van de ene Azure regio naar een andere, maar u kunt geen mislukken ze terug opnieuw.**
- De migratie-instructies in dit artikel zijn gebaseerd op de instructies voor het een fysieke machine repliceren naar Azure. Het bevat koppelingen naar de stappen in [repliceren VMware VMs of fysieke servers Azure](site-recovery-vmware-to-azure.md), waarin wordt beschreven hoe u een fysieke server in de portal van Azure repliceren.
- Als u Site herstel in de klassieke-portal instelt, volgt u de gedetailleerde instructies in [dit artikel](site-recovery-vmware-to-azure-classic.md). **U moet niet meer gebruiken** de instructies in dit [artikel oudere](site-recovery-vmware-to-azure-classic-legacy.md).

Opmerkingen of vragen onder van dit artikel of op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)posten.


## <a name="prerequisites"></a>Vereisten voor

Hier volgt wat u nodig hebt voor dit implementatie:

- **Configuratie-server**: een on-premises implementatie VM met Windows Server 2012 R2 die als de configuratieserver fungeert. U installeren de andere herstel van de Site-onderdelen (inclusief de processerver en de basispagina doelserver) te op deze VM. Lees meer in [scenario architectuur](site-recovery-vmware-to-azure.md#scenario-architecture) en [configuratie serververeisten](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **IaaS virtuele machines**: het VMs die u wilt migreren. U kunt deze VMs migreren door ze behandelen als fysieke machines.

## <a name="deployment-steps"></a>Implementatiestappen

Dit onderwerp vindt de van implementatiestappen in de nieuwe portal van Azure. Als u deze implementatiestappen nodig voor herstel van de Site in de klassieke-portal, raadpleegt u [in dit artikel](site-recovery-vmware-to-azure-classic.md).

1. [Maken een kluis](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Een configuratieserver Deploy](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Nadat u de configuratieserver hebt geïmplementeerd, raadpleegt u kunnen communiceren met de VMs die u wilt migreren.
4. [Replicatie-instellingen instellen](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Replicatiebeleid maken en toewijzen aan de configuratieserver.
5. [De service mobiliteit installeren](site-recovery-vmware-to-azure.md#step-6-replication-application). Elke VM die u wilt beveiligen, moet de mobiliteit-service is geïnstalleerd. Deze service verzendt gegevens naar de processerver. De service mobiliteit, kan worden geïnstalleerd handmatig of gedrukt, en worden automatisch door de processerver wordt geïnstalleerd wanneer bescherming tegen voor VM is ingeschakeld. Firewallregels op de VMs die u wilt migreren moeten worden geconfigureerd dat push-installatie van deze service.
6. [Replicatie inschakelen](site-recovery-vmware-to-azure.md#enable-replication). Replicatie voor de VMs die u wilt migreren inschakelen. U kunt de IaaS virtuele machines die u wilt migreren naar Azure via het privé IP-adres van de virtuele machines kan detecteren. Dit adres zoeken op het dashboard VM in Azure wordt aangegeven. Wanneer u replicatie inschakelt, stelt u het computertype voor de VMs als fysieke machines.
7. [Een niet-geplande failover uitvoeren](site-recovery-failover.md#run-an-unplanned-failover). Nadat de eerste replicatie is voltooid, kunt u een niet-geplande failover van de ene Azure regio uitvoeren naar de andere. U kunt desgewenst een herstelplan opstellen en uitvoeren van een niet-geplande failover, voor het migreren van meerdere virtuele machines tussen regio's. [Meer informatie](site-recovery-create-recovery-plans.md) over herstel abonnementen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over andere replicatie-scenario's in [Wat Azure Site herstel is?](site-recovery-overview.md)
