<properties
    pageTitle="Wat is herstel van de Site? | Microsoft Azure"
    description="Geeft een overzicht van de Site herstel van Azure-service en bevat een overzicht van scenario's voor implementatie."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/13/2016"
    ms.author="raynew"/>

#  <a name="what-is-site-recovery"></a>Wat is herstel van de Site?

Welkom bij het herstelproces is Azure Site! In dit artikel biedt een beknopt overzicht van de Site herstel-service en hoe deze draagt bij aan uw bedrijf.

Uw organisatie nodig heeft een bedrijfscontinuïteit en (BCDR) noodherstelplan die bijhoudt apps, werkbelasting en gegevens veilig en beschikbaar tijdens geplande en ongeplande downtime en herstelt naar de normale arbeidsvoorwaarden zo snel mogelijk. Herstel van de site is een Azure-service vergeleken met deze strategie.

Site-herstel orchestrates replicatie van werkbelasting met on-premises implementatie fysieke servers en virtuele machines. U kunt servers en VMs uit een primaire datacenter repliceren in de cloud (Azure) of met een secundaire datacenter. Wanneer er bijvoorbeeld optreden in de primaire-site, u niet de secundaire site om apps en werkbelasting toegankelijk en beschikbaar. U terug naar uw hoofdlocatie bent niet en er wordt naar de normale bewerkingen.

## <a name="site-recovery-in-the-azure-portal"></a>Herstel van de site van de Azure-portal

Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources. Het model Azure resourcemanager en het beheermodel klassieke services. Azure heeft ook twee portals – de [Azure klassieke portal](https://manage.windowsazure.com/) die ondersteuning biedt voor het implementatiemodel klassieke en de [Azure-portal](https://portal.azure.com) met ondersteuning voor zowel het klassieke en resourcemanager-modellen.

- Herstel van de site is beschikbaar in de klassieke portal zowel de Azure-portal.
- In de klassieke Azure portal, kunt u herstel van de Site met het model voor het beheer van klassieke services ondersteunen.
- Klik in de portal Azure kunt u het klassieke model of resourcemanager implementaties ondersteunen. 

De informatie in dit artikel is van toepassing op classic en Azure portal implementaties. Verschillen worden vermeld, indien van toepassing.


## <a name="why-deploy-site-recovery"></a>Waarom implementeren herstel van de Site?

Dit is wat herstel van de Site voor uw bedrijf kunt doen:

- **BCDR vereenvoudigen**, kunt u replicatie, failover en herstel van meerdere werkbelasting op één locatie in de portal van Azure verwerken. Site-herstel orchestrates herhaling en overname bij storing, maar niet snijpunt van de toepassingsgegevens van uw of relevante informatie hierover.
- **Flexibele herhaling opgeven**, gebruik sites worden hersteld, kunt u werkbelasting ondersteunde Hyper-V VMs, VMware VMs en Windows/Linux fysieke servers waarop repliceren.
- **Eenvoudig herhaling uitvoeren testen**, Site-herstel biedt test failovers voor noodgevallen herstel boren, handhaven productieomgevingen.
- **Mislukken en herstellen**, kunt u geplande failovers voor verwachte bijvoorbeeld met een nul-gegevensverlies of niet-geplande failover uitvoeren met minimale gegevensverlies (afhankelijk van de frequentie van replicatie) voor onverwachte systeemfouten. Na een failover kunt u failback aan uw primaire-sites. Site-herstel biedt herstel-abonnementen die scripts en Azure automatisering werkmappen opnemen kunnen, zodat u failover en herstel van toepassingen met meerdere niveaus aanpassen kunt.
- **Een secundaire datacenter voorkomen**, kunt u werkbelasting repliceren aan Azure, in plaats van een secundaire site. Hierdoor wordt de kosten en complexiteit onderhouden van een secundaire datacenter. Gerepliceerde gegevens worden opgeslagen in Azure-opslag, met alle flexibiliteit vindt. VMs worden gemaakt met de gerepliceerde gegevens bij een storing.
- **Integratie met bestaande BCDR technologieën**, herstel van de Site werkt naadloos samen met andere functies BCDR. Bijvoorbeeld, kunt u herstel van de Site te beveiligen van de SQL Server-end van bedrijfs-werkbelastingen, inclusief ondersteuning voor SQL Server AlwaysOn, voor het beheren van de overname van van de beschikbaarheidsgroepen.

## <a name="what-can-i-replicate"></a>Wat kan ik repliceren?

Hier volgt een overzicht van wat u kunt repliceren met sites worden hersteld.

![On-premises implementatie naar on-premises implementatie](./media/site-recovery-overview/asr-overview-graphic.png)

**REPLICEREN** | **REPLICEREN NAAR** 
---|---
On-premises implementatie VMware VMs waarop werkbelasting | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Secundaire site](site-recovery-vmware-to-vmware.md)
Werkbelasting waarop on-premises implementatie Hyper-V VMs beheerd in VMM wolken  | [Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Secundaire site](site-recovery-vmm-to-vmm.md) 
Werkbelasting waarop on-premises implementatie Hyper-V VMs beheerd in VMM wolken, met SAN-opslagruimte|  [Secundaire site](site-recovery-vmm-san.md)
On-premises implementatie Hyper-V VMs, zonder VMM waarop werkbelasting | [Azure](site-recovery-hyper-v-site-to-azure.md)
Werkbelasting op on-premises implementatie fysieke Windows/Linux-servers | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Secundaire site](site-recovery-vmware-to-vmware.md)


## <a name="what-workloads-can-i-protect"></a>Welke werkbelasting kan ik beveiligen?

Herstel van de site wordt toepassing hoogte BCDR, zodat de werkbelasting en apps gaat u verder met het op een consistente manier worden uitgevoerd wanneer bijvoorbeeld plaatsvinden. Herstel van de site bevat:

- **Toepassing consistente momentopnamen**, Machines repliceren met behulp van toepassing-consistente momentopnamen, voor één of meerdere niveaus apps. Naast het opnemen van de schijfgegevens vastleggen toepassing consistente momentopnamen vastleggen alle gegevens in het geheugen en alle transacties in proces.
- **In de buurt van de synchroon replicatie**-Site herstel biedt frequentie van replicatie is 30 seconden voordat Hyper-V en continue replicatie voor VMware zo laag.
- **Flexibele herstel-abonnementen**, u kunt maken en aanpassen van herstel abonnementen met externe scripts en handmatige acties. Integratie met Azure automatisering runbooks kunt u een hele toepassing stapel met één klik herstellen.
- **Integratie met SQL Server AlwaysOn**, kunt u de overname van van beschikbaarheidsgroepen in Site herstel herstel abonnementen beheren.
- **Automation-bibliotheek**, een uitgebreide Azure automatisering bibliotheek bevat productie gereed, toepassingsspecifieke scripts die kunnen worden gedownload en geïntegreerd met sites worden hersteld.
- **Eenvoudige netwerk management**, geavanceerde netwerk management in herstel van de Site en Azure eenvoudiger toepassing netwerkvereisten, inclusief worden gereserveerd IP-adressen, netwerktaakverdelers configureren en Azure verkeer Manager integreert voor efficiënte netwerk switchovers.


## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie in [welke werkbelasting kunt Site herstel beveiligen?](site-recovery-workload.md)
- Meer informatie over de architectuur van de Site herstel in [hoe werkt Site herstel?](site-recovery-components.md)
 
