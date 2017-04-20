
<properties
   pageTitle="Azure richtlijnen | patronen en procedures | Microsoft Azure"
   description="Aanbevolen procedures en richtlijnen voor het Azure"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="christb"/>

# <a name="azure-guidance"></a>Azure richtlijnen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Het team van Microsoft patronen en procedures maakt deel uit van het Team van Azure-klant-advies. Ons doel is om te helpen ontwikkelaars, architecten en IT-professionals lukt op het Microsoft Azure-platform. We leidraad met aanbevolen procedures voor het samenstellen van cloud oplossingen op Azure.

## <a name="checklists"></a>Controlelijsten

Deze lijsten zijn een beknopt overzicht van het controleren van de fundamentele aspecten van beschikbaarheid en schaalbaarheid. 

- [Beschikbaarheid van controlelijst][AvailabilityChecklist] 

    Een samenvatting van aanbevolen procedures voor zorgen dat beschikbaarheid.

- [Schaalbaarheid controlelijst][ScalabilityChecklist]

    Een samenvatting van aanbevolen procedures voor het ontwerpen en implementeren van scalable services en beheren van gegevens voor de verwerking.

## <a name="best-practices-articles"></a>Aanbevolen procedures artikelen

Deze artikelen bieden een uitgebreide informatie van belangrijke concepten getoond meestal dat is gekoppeld aan cloud computing. 

- [API-ontwerp][APIDesign] 

    Een discussie van ontwerpen problemen u rekening moet houden bij het ontwerpen van een web API.

- [API-implementatie][APIImplementation] 

    Een reeks aanbevolen procedures voor de implementatie en publiceren van een web API.

- [Richtlijnen voor API-beveiliging](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 

    Een discussie van verificatie en machtiging bezwaren (bijvoorbeeld token typen, autorisatie protocollen autorisatie loopt en bedreiging risicobeperking).

- [AutoScaling richtlijnen][AutoscalingGuidance] 

    Een samenvatting van aandachtspunten voor de schaalbaarheid van oplossingen zonder dat u tussenkomst.

- [Achtergrond taken richtlijnen][BackgroundJobsGuidance] 

    Een beschrijving van de beschikbare opties en aanbevolen procedures voor de uitvoering van taken die moeten worden uitgevoerd op de achtergrond, onafhankelijk van alle voorgrond of interactieve bewerkingen.

- [Richtlijnen voor inhoud bezorging netwerk (CDN)][CDNGuidance] 

    Algemene richtlijnen en aanbevolen voor het gebruik van de CDN minimaliseren de belasting van uw toepassingen, en beschikbaarheid en prestaties optimaliseren.

- [In de cache richtlijnen][CachingGuidance] 

    Een overzicht van het gebruik van de cache opslaan om te verbeteren de prestaties en schaalbaarheid van een systeem.

- [Gegevens partitionering richtlijnen][DataPartitioningGuidance]

    Strategieën waarmee u kunt partitiegegevens schaalbaarheid, te verbeteren conflict verkleinen en prestaties optimaliseren.

- [Controle- en hulpprogramma's voor diagnose richtlijnen][MonitoringandDiagnosticsGuidance] 

    Richtlijnen voor het bijhouden van hoe uw gebruikers gebruikmaken van uw systeem Resourcegebruik aanwijzen en de servicestatus en de prestaties van uw systeem in het algemeen controleren.

- [Aanbevolen naamgevingsconventies][naming-conventions] 

    Aanbevolen naamgevingsconventies voor Azure resources.

- [Probeer de algemene richtlijnen][RetryGeneralGuidance] 

    Beschrijving van de algemene concepten voor het afhandelen van tijdelijke fouten.

- [Probeer de Service-specifieke richtlijnen][RetryServiceSpecificGuidance]

    Een overzicht van de functies opnieuw voor een groot aantal Azure services, met inbegrip van informatie waarmee u kunt gebruiken, aanpassen of het opnieuw om deze service uitbreiden.

## <a name="scenario-guides"></a>Scenario hulplijnen

- [Actieve Elasticsearch op Azure][elasticsearch] 
    
    Elasticsearch is een zeer scalable open source zoekprogramma en de database. Dit is geschikt voor situaties waarin snelle analyse en discovery van informatie in grote gegevenssets. Deze instructies kijkt naar enkele belangrijke aspecten u rekening moet houden bij het ontwerpen van een cluster Elasticsearch.

- [Identiteitsbeheer voor multitenant toepassingen][identity-multitenant] 
    
    Multitenancy is een architectuur waar meerdere tenants delen van een toepassing, maar worden geïsoleerd van elkaar. Deze instructies ziet u hoe u het beheren van de identiteit van de gebruikers in een multitenant-toepassing, met [Azure Active Directory] [ AzureAD] worden afgehandeld aanmelden en verificatie.
    
- [Grote gegevensoplossingen ontwikkelen](https://msdn.microsoft.com/library/dn749874.aspx)

    Deze handleiding wordt het gebruik van HDInsight voor scenario's zoals iteratieve uitleg als een data-warehouse, voor ETL processen en integratie in de bestaande BI-systemen verkend. Het bevat ook richtlijnen voor informatie over de concepten van big data, planning en oplossingen voor grote ontwerpen en implementeren van deze oplossingen.
    
## <a name="patterns"></a>Patronen

- [Cloud ontwerppatronen: Architectuurrichtlijn voor Cloud-toepassingen](https://msdn.microsoft.com/library/dn568099.aspx)

    Cloud ontwerppatronen is een bibliotheek met het ontwerppatronen en gerelateerde richtlijnen onderwerpen. Deze articulates het voordeel van het toepassen van patronen door te laten zien hoe elk tekstgedeelte architecturen voor cloud-toepassingen kunt past.
    
- [Optimaliseren voor Cloud-toepassingen](https://github.com/mspnp/performance-optimization)

    Deze richtlijnen zijn verkennen van algemene tegen patronen die apps belemmeren van schaalbaarheid onder laden. Het bevat voorbeelden aan te tonen acht tegen patronen en een [prestaties analyse handleiding](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) en een handleiding voor [beoordeling van prestaties ten opzichte van belangrijke gegevens](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md).

## <a name="reference-architectures"></a>Verwijzing architecturen

Onze architecturen verwijzing zijn gerangschikt op scenario.
Elke afzonderlijke architectuur biedt aanbevolen procedures en aanbevolen stappen en een uitvoerbare onderdeel die de aanbevelingen.

De huidige library of verwijzing architecturen is beschikbaar op [http://aka.ms/architecture](http://aka.ms/architecture).

## <a name="resiliency-guidance"></a>Richtlijnen voor tolerantie

Deze onderwerpen wordt beschreven hoe toepassingen ontwerpen die tegen mislukt in een gedistribueerde cloud-omgeving zijn.   

- [Overzicht van de tolerantie][ResiliencyOvervew]

     Het maken van toepassingen op het Azure platform die kunnen herstellen van fouten en blijven functioneren. Beschrijving van een gestructureerde methode als u wilt bereiken tolerantie, van ontwerp implementatie, implementatie en bewerkingen.

- [Controlelijst voor tolerantie][resiliency-checklist]

    Een controlelijst met aanbevelingen waarmee u kunt plannen voor een groot aantal mislukt modi die kunnen optreden.

- [Fout bij modus analyse][resiliency-fma] 

    Fout bij modus analyse (FMA) is een proces voor het samenstellen van tolerantie in een systeem, door te identificeren mogelijke fout wordt verwezen. Als uitgangspunt voor uw FMA-proces bevat in dit artikel een catalogus van mogelijke fout modi en hun beperkingen. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md

