<properties
   pageTitle="Controlelijst voor tolerantie | Microsoft Azure"
   description="Controlelijst die richtlijnen voor tolerantie bezwaren tijdens het ontwerp bevat."
   services=""
   documentationCenter="na"
   authors="petertaylor9999"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="petertay"/>

# <a name="azure-resiliency-guidance-resiliency-checklist"></a>Azure tolerantie richtlijnen: Controlelijst voor tolerantie

Ontwerpen van uw toepassing voor tolerantie is vereist, plannen en beperkende verschillende mislukt modi die kunnen optreden. Controleer de artikelen in deze controlelijst ten opzichte van het ontwerp van uw toepassing zodat u deze meer robuuste.

## <a name="requirements"></a>Vereisten

- **Van uw klant beschikbaarheid vereisten definiëren.** Uw klant beschikbaarheid vereisten voor de onderdelen hebben in uw toepassing en dit ontwerp van uw toepassing beïnvloeden. Overeenkomst van uw klant krijgen voor de beschikbaarheid van de doelen van elk tekstgedeelte uw toepassing, het ontwerp van uw mogelijk anders niet aan de verwachtingen van de klant. Zie het gedeelte [definiëren van uw vereisten voor tolerantie](guidance-resiliency-overview.md#defining-your-resiliency-requirements) van het document [ontwerpen robuuste toepassingen voor Azure](guidance-resiliency-overview.md) voor meer informatie.

## <a name="failure-mode-analysis"></a>Fout bij modus analyse

- **Een fout modus analyse (FMA) voor uw toepassing uitvoeren.** FMA is een proces voor het samenstellen van tolerantie in een toepassing vroeg in de ontwerpfase. De doelstellingen van een FMA opnemen:  

    - Geef aan dat mogelijk ervaart u wat voor soort fouten een toepassing. 
    - De mogelijke effecten en het effect van elk type fout over de toepassing vastleggen.
    - Herstelstrategieën identificeren. 

    Zie voor meer informatie [ontwerpen robuuste toepassingen voor Azure: analyse op modus][fma].  

## <a name="application"></a>Toepassing

- **Meerdere exemplaren van services implementeren.** Services onvermijdelijk, mislukt, en als uw toepassing afhankelijk van één exemplaar van een service is zal onvermijdelijk mislukken ook. Als u wilt inrichten meerdere exemplaren voor [Azure App-Service](../app-service/app-service-value-prop-what-is.md), selecteert u een [App-Service plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) met meerdere exemplaren. Voor Azure Cloud Services, configureert u elk van uw rollen voor het gebruik van [meerdere exemplaren](../cloud-services/cloud-services-choose-me.md#scaling-and-management). Zorg ervoor dat uw architectuur VM meer dan één VM bevat en dat elke VM is opgenomen in een [beschikbaarheid instellen]voor [Azure virtuele Machines (VMs)](../virtual-machines/virtual-machines-windows-about.md),[availability-sets].   

- **Gebruik een taakverdeling distributie van aanvragen.** Taakverdeling uw aanvragen in orde service-exemplaren doordat beschadigd exemplaren uit draaiing. Als uw service Azure App Service of Azure Cloud Services gebruikt, is het al taakverdeling voor u. Echter als uw toepassing gebruikmaakt van Azure VMs, moet u voor het inrichten van een taakverdeling. Zie overzicht van de [Azure taakverdeling](../load-balancer/load-balancer-overview.md) voor meer informatie. 

- **Azure-Toepassingsgateways als u meerdere exemplaren wilt configureren.** Afhankelijk van de vereisten van uw toepassing, een [Gateway van Azure-toepassing](../application-gateway/application-gateway-introduction.md) mogelijk beter geschikt voor het distribueren van aanvragen voor services van uw toepassing. Echter zijn enkele exemplaren van de toepassingsgateway-service niet gegarandeerd door een SLA zodat het is mogelijk dat uw toepassing mislukken kan als het exemplaar van de toepassingsgateway is mislukt. Meer dan één medium of groter toepassingsgateway exemplaar om te garanderen beschikbaarheid van de service onder de voorwaarden van de [SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/)inrichten.

- **Gebruik beschikbaarheid Sets voor elke toepassingslaag**. Uw varianten plaatsen in een [beschikbaarheid instellen] [ availability-sets] connectiviteit met ten minste één exemplaar van de VM garandeert binnen de voorwaarden van de [SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_2/). Als uw VMs niet in een beschikbaarheid instellen dat er geen garanties ze te beveiligen en is het mogelijk dat ze kunnen mislukken alle of tegelijk worden bijgewerkt. 

- **Houd rekening met het implementeren van uw toepassing via meerdere regio's.** Als uw toepassing wordt geïmplementeerd op een bepaalde regio, wordt de hele regio in de zelden niet beschikbaar, uw toepassing ook niet beschikbaar. Dit is mogelijk niet acceptabel in het kader van van uw toepassing SLA. Als dat het geval is, kunt u uw toepassing en de bijbehorende services implementeert tussen meerdere regio's. Een meerdere landen/regio-implementatie kunt u een actieve patroon (distribueren aanvragen alle meerdere actieve werkstroomexemplaren) of een actieve-passieve patroon (een 'warme' exemplaar houden in reserveren, geval de primaire-instantie mislukt) gebruiken. Het is raadzaam dat u meerdere exemplaren van van uw toepassing services in regionale waardeparen installeren. Zie voor meer informatie [bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR): Azure gepaarde t-toets regio's](../best-practices-availability-paired-regions.md).

- **Implementeer tolerantie patronen voor externe bewerkingen wanneer van toepassing.** Als uw toepassing afhankelijk van de communicatie tussen externe services, mislukt het communicatiepad onvermijdelijk. Als er meerdere fouten zijn, kunnen u de resterende orde exemplaren van uw services van ondergesneeuwd met aanvragen. Er zijn verschillende patronen handig voor omgaan met veelvoorkomende fouten, waaronder het patroon time-out, het [patroon opnieuw][retry-pattern], de [stroomonderbreker] [ circuit-breaker] patroon en anderen. Zie [ontwerpen robuuste toepassingen voor Azure](guidance-resiliency-overview.md#implementing-resiliency-strategies)voor meer informatie. 

- **Gebruik autoscaling reageren op stijging laden.** Als uw toepassing niet is geconfigureerd als u automatisch naarmate de belasting toeneemt wilt verkleinen, is het mogelijk dat van uw toepassing services vast loopt als ze worden verzadigd met aanvragen van gebruikers. Zie de volgende onderwerpen voor meer informatie:

    - Algemeen: [schaalbaarheid controlelijst](../best-practices-scalability-checklist.md) 
    - Azure App-Service: [schalen dat exemplaar tellen handmatig of automatisch][app-service-autoscale]
    - Cloudservices: [hoe u automatisch schalen een cloudservice][cloud-service-autoscale]
    - Virtuele Machines: [Hiermee stelt u automatische schaalbaarheid en VM schaal][vmss-autoscale]


- **Implementeer asynchrone bewerkingen indien mogelijk.** Synchrone bewerkingen kunnen resources in beslag nemen en andere bewerkingen blokkeren terwijl de beller dit proces wacht te voltooien. Ontwerp van elk onderdeel van uw toepassing om te staan voor asynchrone bewerkingen indien mogelijk. Zie voor meer informatie over het implementeren van asynchrone hoeft te programmeren in C# [asynchroon programmeren met asynchrone en wacht tot][asynchronous-c-sharp].

- **Azure verkeer Manager gebruiken om te leiden van uw toepassing-verkeer is toegestaan voor verschillende regio's.**  [Azure verkeer Manager] [ traffic-manager] worden uitgevoerd op het niveau van de DNS-taakverdeling en verkeer naar verschillende regio's op basis van het [verkeersroutering] kunt doorstuurt[ traffic-manager-routing] methode die u opgeeft en de status van de eindpunten van de toepassing. 

- **Configureren en systeemstatus sondes voor uw netwerktaakverdelers en verkeer managers testen.** Zorg ervoor dat uw status logica de kritieke delen van het systeem gecontroleerd en correct op servicestatus sondes reageert.

    - Controleert of de status voor [Azure verkeer Manager] [ traffic-manager] en [Azure taakverdeling] [ load-balancer] een specifieke functie. Voor verkeer Manager, met de systeemstatus-test bepaalt of naar een andere regio mislukken. Voor een taakverdeling bepaalt of u een VM uit draaiing verwijderen.      

    - Voor een test verkeer Manager, moet uw eindpunt status kritieke afhankelijkheden die zijn geïmplementeerd in dezelfde regio en waarvan mislukt moet leiden tot een failover naar een andere regio controleren.  

    - Voor een taakverdeling, moet het eindpunt van de status rapporteren van de status van de VM. Stuur geen andere lagen of externe services. Anders treedt een fout die buiten de VM optreedt de verdeling van de belasting de VM uit draaiing verwijderen.

    - Zie voor instructies over het implementeren van statuscontrole in uw toepassing, [Systeemstatus eindpunt Monitoring patroon](https://msdn.microsoft.com/library/dn589789.aspx).

- **Externe services bewaken.** Als uw toepassing afhankelijkheden op externe services heeft, waar u identificeren en hoe deze services derden kunnen mislukken en wat de fouten effect zijn op uw toepassing. Een derde partij-service mogelijk niet inbegrepen cmdlets voor controle en diagnostische hulpprogramma's, zodat u belangrijke aan te melden uw aanroepen van deze en deze afstemmen met de status van uw toepassing en het gebruik van een unieke id vastleggen van diagnostische. Zie voor meer informatie over aanbevolen procedures voor controle en diagnostische hulpprogramma's, de [controle- en hulpprogramma's voor diagnose richtlijnen] [ monitoring-and-diagnostics-guidance] document.

- **Ervoor zorgen dat elke derde partij-service die u gebruiken een SLA voorzien.** Als uw toepassing afhankelijk van een derde partij-service is, maar de derde geen garantie van beschikbaarheid in de vorm van een SLA biedt, kan ook de beschikbaarheid van de toepassing niet worden gegarandeerd. Uw SLA is alleen nuttig als de minste beschikbaar onderdeel van uw toepassing.


## <a name="data-management"></a>Gegevensbeheer

- **Meer informatie over de herhaling methoden voor gegevensbronnen van uw toepassing.** Uw toepassingsgegevens wordt opgeslagen in verschillende gegevensbronnen en de beschikbaarheid van de verschillende vereisten hebben. De herhaling methoden voor elk type gegevensopslag in Azure evalueren, inclusief [Azure opslag herhaling](../storage/storage-redundancy.md) en [SQL actieve geografische-databasereplicatie](../sql-database/sql-database-geo-replication-overview.md) om ervoor te zorgen dat van uw toepassing gegevensvereisten is voldaan.

- **Zorg ervoor dat er geen Eén gebruikersaccount toegang tot gegevens zowel productie en back-up heeft.** Back-ups van uw gegevens worden is gehackt als één Eén gebruikersaccount daartoe is gemachtigd om te schrijven op zowel productie en back-bronnen. Een kwaadwillende gebruiker kan alle gegevens, opzettelijk verwijderen terwijl een gewone gebruiker kan per ongeluk verwijdert. Ontwerp uw toepassing wilt beperken van de machtigingen van elke gebruikersaccount, zodat alleen de gebruikers waarvoor schrijftoegang schrijftoegang hebben en alleen aan de productie of back-up maken, maar niet beide.

- **Documenten die uw gegevensbron mislukken en achterste proces mislukt en testen.** In het geval is waar uw gegevensbron is mislukt catastrophically, hebt een menselijke operator om te volgen van een reeks instructies worden uitgevoerd met een nieuwe gegevensbron. Als de beschreven stappen fouten hebt, is een operator niet mogelijk wilt succes volgt u deze en over de resource is mislukt. Regelmatig test de instructie stappen om te bevestigen dat een operator volgen ze kunnen wel mislukken en mislukt weer de gegevensbron is.

- **Valideer de back-ups van uw gegevens.** Controleer regelmatig of uw back-upgegevens is wat u verwachten door een script uitvoeren om deze te valideren gegevensintegriteit, schema en query's. Er is geen komma met een back-up als dit nog niet nuttige informatie voor uw gegevensbronnen herstellen. Meld u aan en rapporteren van eventuele inconsistenties, zodat de back-service kan worden hersteld.

- **Kunt u overwegen een opslag-accounttype waarvoor geografische overbodig.** Gegevens die zijn opgeslagen in een opslag van Azure-account wordt altijd lokaal gerepliceerd. Er zijn echter meerdere herhaling strategieën kunt kiezen uit een opslag-Account is ingericht. Selecteer [Azure leestoegang geografische redundante opslag (AB-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) aan uw toepassingsgegevens beschermen tegen de zelden wanneer een hele regio niet meer beschikbaar.

    > [AZURE.NOTE] Voor VMs, niet te vertrouwen op AB-GRS herhaling herstellen van de VM schijven (VHD-bestanden). In plaats daarvan met [Back-up van Azure][azure-backup].   

## <a name="operations"></a>Bewerkingen

- **Controle en signalering aanbevolen procedures in uw toepassing implementeren.** Zonder BEGINLETTERS controle, diagnostische hulpprogramma's en waarschuwingen is er geen manier om fouten opsporen in uw toepassing en Waarschuw van een operator u ze kunt oplossen. Zie de [richtlijnen voor controle- en diagnostische hulpprogramma's] voor meer informatie over aanbevolen procedures,[ monitoring-and-diagnostics-guidance] document.

- **Externe oproep statistieken meten en brengt u de gegevens beschikbaar zijn voor het team van toepassing.**  Als u niet bijhouden en externe oproep statistieken in realtime rapporteren en kunnen gemakkelijk deze informatie bekijken, het operationele team geen een momentopname weergave in de status van uw toepassing. En als u alleen maateenheid gemiddelde externe oproep tijd, wordt er geen weinig gegevens om weer te geven oplossen in de services. Een samenvatting maken van de doelstellingen van de externe oproep zoals latentie, doorvoer en fouten oplossen in de percentielen 99 en 95. Statistische analyses uitvoeren op de Maatstelsel van fouten die binnen elke percentiel optreden schuiven.

- **Bijhouden het aantal tijdelijke uitzonderingen en nieuwe pogingen gedurende een bepaalde een juiste periode.** Als u niet bijhouden en tijdelijke uitzonderingen bewaken en na verloop van tijd herhalingspogingen, is het mogelijk dat een probleem of mislukt door van de toepassing opnieuw logica kan worden verborgen. Dat wil zeggen als tijdens de controle- en registratie alleen vastgesteld slagen of mislukken van een bewerking, worden het feit dat de bewerking heeft opnieuw worden meerdere keren geprobeerd vanwege uitzonderingen verborgen. Een trendlijn met groter wordende uitzonderingen na verloop van tijd wordt aangegeven dat de service is een probleem ondervindt en kan mislukken. Voor meer informatie raadpleegt u [specifieke richtlijnen voor services opnieuw][retry-service-guidance].

- **Implementeer een systeem voor vroegtijdige waarschuwing die meldingen van een operator.** Geef aan dat de prestatie indicatoren van van uw toepassing status, zoals tijdelijke uitzonderingen en externe latentie bellen en juiste drempelwaarden voor elk van deze instellen. Een waarschuwing verzenden naar bewerkingen wanneer de drempelwaarde is bereikt. Stel deze drempels op niveaus die problemen identificeren voordat ze kritieke worden en een reactie herstel vereist.

- **Het proces release voor uw toepassing van het document.** Zonder gedetailleerde release Procesdocumentatie mogelijk een operator een ongeldige update implementeren of onjuist instellingen configureren voor uw toepassing. Duidelijk definiëren document van het release-proces en zorg ervoor dat deze beschikbaar voor het hele team voor bewerkingen is. Aanbevolen procedures voor het robuuste implementatie van uw toepassing in de [robuuste implementatie] zijn gedetailleerde[ guidance-resilient-deployment] sectie van het document tolerantie richtlijnen.

- **Zorg ervoor dat meer dan één persoon in het team is training om te controleren van de toepassing en eventuele handmatige herstel stappen uitvoeren.** Als u slechts één operator van het team op wie kan de toepassing bewaken en procedure starten, wordt deze persoon een potentieel risico. Meerdere personen op detectie en herstel trainen en zorg ervoor dat er altijd ten minste één actieve op elk gewenst moment.

- **Van uw toepassing implementatie-automatiseren.** Als uw medewerkers verplicht is voor het handmatig uw toepassing implementeren, kan de menselijke fouten leiden tot de implementatie mislukt. Zie voor meer informatie over aanbevolen procedures voor het automatiseren van toepassingsimplementatie, de [robuuste implementatie] [ guidance-resilient-deployment] sectie van het document tolerantie richtlijnen.

- **Ontwerp uw release-proces voor het maximale beschikbaarheid van toepassingen.** Als uw proces release services vereist off tijdens de implementatie, is uw toepassing niet beschikbaar totdat ze weer online komt. De [blauw-groen](http://martinfowler.com/bliki/BlueGreenDeployment.html) of [Canarische vrijgeven](http://martinfowler.com/bliki/CanaryRelease.html) implementatie-methode gebruiken om te implementeren van de toepassing op de productie. Beide van de volgende manieren gebruikmaakt van uw code release samen met productiecode implementeert, zodat gebruikers van release code kunnen worden omgeleid naar productiecode bij een storing. Zie voor meer informatie de [robuuste implementatie] [ guidance-resilient-deployment] sectie van het document tolerantie richtlijnen.

- **Meld u aan en controleren van uw toepassing implementaties.** Als u gefaseerde implementatie technieken zoals blauw-groen of kokospalm releases zal er meer dan één versie van uw toepassing uitgevoerd in productie. Als u een probleem moet worden uitgevoerd, is het is belangrijk om te bepalen welke versie van uw toepassing een probleem wordt veroorzaakt. Implementeer een strategie robuuste logboekregistratie zoveel mogelijk informatie versie / regiospecifieke vastleggen. 

- **Zorg ervoor dat uw toepassing wordt niet uitgevoerd zich verhouden tot [Azure abonnement beperkt](../azure-subscription-service-limits.md).** Azure abonnementen hebben limieten op bepaalde resourcetypen, zoals het aantal resourcegroepen, aantal kernen en aantal opslag-accounts.  Als uw toepassingsvereisten voor de Azure abonnementen overschrijdt, een andere Azure-abonnement en inrichten voldoende resources maken er.

- **Zorg ervoor dat uw toepassing wordt niet uitgevoerd zich verhouden tot [per service beperkt](../azure-subscription-service-limits.md).** Afzonderlijke Azure services hebben verbruik limieten &mdash; bijvoorbeeld de maximale van opslag, doorvoer, aantal verbindingen, aanvragen per seconde en andere aan de doelstellingen. Uw toepassing loopt vast als bronnen buiten deze limieten wordt gebruikt. Hierdoor wordt de service bandbreedteregeling en mogelijke downtime voor getroffen gebruikers. 

    Afhankelijk van de specifieke service en uw toepassingsvereisten voor de, kunt u deze limieten vaak voorkomen door het schaalbaarheid omhoog (bijvoorbeeld als u een andere prijzen laag te kiezen) of schalen (nieuwe exemplaren toevoegen).  

- **Ontwerp van uw toepassing opslagvereisten te Azure opslag schaalbaarheid en prestaties doelen vallen.** Azure opslag is bedoeld om te werken binnen vooraf gedefinieerde schaalbaarheid en prestaties doelen, zodat uw toepassing gebruikmaken van de opslag in deze doelen ontwerpen. Als u deze doelen overschrijdt ervaart uw toepassing opslag beperken. U lost dit inrichten van extra opslagruimte-Accounts. Als u zich verhouden tot de limiet van opslag Account uitvoert, inrichten aanvullende Azure-abonnementen en vervolgens inrichten er extra opslagruimte-Accounts. Zie [Azure opslag schaalbaarheid en prestaties doelen](../storage/storage-scalability-targets.md)voor meer informatie.

- **Selecteer de juiste grootte VM voor uw toepassing.** Meet de werkelijke CPU, geheugen, schijf en i/o-van uw VMs in productie en controleer of de VM-grootte die u hebt geselecteerd is voldoende. Zo niet, kan uw toepassing capaciteitsproblemen optreden terwijl de VMs hun limieten benaderen. VM tekengrootten worden beschreven in detail in het document [grootten voor virtuele machines in Azure wordt aangegeven](../virtual-machines/virtual-machines-windows-sizes.md) .

- **Bepalen of de werklast van de toepassing stabiele of wisselende na verloop van tijd.** Als uw werkzaamheden na verloop van tijd fluctueert, gebruik Azure VM schaal wordt ingesteld op automatisch schaal het aantal exemplaren VM. U moet anders handmatig vergroten of verkleinen van het aantal VMs. Zie de [VM schaal Sets overzicht](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)voor meer informatie.

- **Selecteer de juiste servicelaag voor Azure SQL-Database.** Als uw toepassing gebruikmaakt van Azure SQL-Database, moet u ervoor zorgen dat u de juiste servicelaag hebt geselecteerd. Als u een laag die niet kan worden afgehandeld van uw toepassing database transactie eenheid (DTU) vereisten selecteert, is het gegevensgebruik wordt vertraagd. Zie voor meer informatie over het selecteren van het juiste abonnement, de [SQL-Database-opties en prestaties: begrijpen wat is beschikbaar in elke servicelaag](../sql-database/sql-database-service-tiers.md) document. 

- **Een plan voor terugdraaien voor implementatie hebben.** Het is mogelijk dat uw toepassingsimplementatie kan mislukken en zorgen dat uw toepassing niet meer beschikbaar. Een herstelprocedure gaat u terug naar een laatste versie van de bekende goede en uitvaltijd ontwerpen. Zie de [robuuste implementatie] [ guidance-resilient-deployment] sectie van het document tolerantie richtlijnen voor meer informatie. 

- **Maak een proces voor de communicatie met Azure ondersteuning.** Als het proces voor het contact wilt opnemen met [ondersteuning voor Azure](https://azure.microsoft.com/support/plans/) niet is ingesteld voordat u contact kunt opnemen met ondersteuning nodig is, wordt er downtime worden verlengd terwijl het ondersteuningsproces voor de eerste keer wordt doorgegeven. Het proces voor contact opnemen met ondersteuning en escaleren van problemen als onderdeel van van uw toepassing tolerantie vanaf het begin opnemen.

- **Zorg ervoor dat uw toepassing meer dan het maximum aantal opslag accounts per abonnement niet wordt gebruikt.** Azure kan maximaal 200 opslag accounts per abonnement. Als uw toepassing meer opslagruimte accounts vereist dan zijn momenteel beschikbaar in uw abonnement, kunt u moet maken van een nieuw abonnement en er extra opslagruimte-accounts maken. Zie [Azure-abonnement en limieten van de service, quota's en beperkingen](../azure-subscription-service-limits.md#storage-limits)voor meer informatie.

- **Zorg ervoor dat uw toepassing niet groter is dan de doelen schaalbaarheid voor VM schijven.** Een Azure IaaS VM ondersteunt het toevoegen van een aantal gegevensschijven afhankelijk van de volgende factoren, inclusief de VM grootte en het type account dat opslag. Als uw toepassing groter is dan de doelen schaalbaarheid voor VM schijven, inrichten van extra opslagruimte accounts en de VM schijven er maken. Zie voor meer informatie, [Azure opslag schaalbaarheid en prestaties doelen] (.. / storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks)

## <a name="test"></a>Test

- **Failover en foutherstel die voor uw toepassing testen uitvoeren.** Als u nog niet volledig failover en foutherstel getest, kunt geen u er zeker dat de afhankelijke services in uw toepassing weer in een gesynchroniseerde wijze tijdens herstel bovenkomen. Zorg ervoor dat van uw toepassing afhankelijke services failover en fail terug in de juiste volgorde.

- **Fout-webweergave testen voor uw toepassing uitvoeren.** Uw toepassing kan mislukken verschillende oorzaken hebben, zoals certificaat verlooptijd, uitputting van systeembronnen in een VM of opslag fouten. Test uw toepassing in een omgeving zo dicht mogelijk met productie, door simuleren of reële fouten activeert. Bijvoorbeeld certificaten verwijderen, kunstmatig systeembronnen of verwijderen uit een opslagbron. Controleer of de mogelijkheid van uw toepassing herstellen uit alle soorten fouten, alleen en in combinatie. Controleer of fouten niet is doorgevoerd of trapsgewijs via uw systeem.

- **Tests uitvoeren in productie met beide synthetische en reële gebruikersgegevens.** Test- en zelden identiek zijn, dus het is belangrijk u blauw-groen of een kokospalm implementatie en testen van de toepassing in productie. Hiermee kunt u uw toepassing testen in kader reële laden en zorg ervoor dat werkt zoals verwacht wanneer volledig is geïmplementeerd.

## <a name="security"></a>Beveiliging

- **Implementeer niveau van de webtoepassing bescherming tegen verdeelde weigering aanvallen (DDoS).** Azure services zijn beveiligd tegen dergelijke aanval op de netwerklaag. Azure beveiligen niet echter tegen toepassingslaag aanvallen, omdat het is moeilijk te onderscheiden waar gebruiker aanmeldingsaanvragen van kwaadwillende gebruikersaanvragen. Zie het gedeelte 'Beveiligen tegen DDoS' van [Microsoft Azure netwerkbeveiliging](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf) (PDF-bestand downloaden) voor meer informatie over het beveiligen tegen toepassingslaag dergelijke aanval.

- **Het principe van het laagste bevoegdheidsniveau voor toegang tot systeembronnen van de toepassing implementeren.** De standaardindeling voor toegang tot de bronnen van de toepassing moet zo strikt mogelijk. Hogere niveau machtigingen op basis van de goedkeuring verlenen. Te soepele toegang verlenen tot van uw toepassing resources al dan niet standaard kan resulteren in iemand per ongeluk of opzettelijk u resources verwijdert. Azure biedt [Rolgebaseerd toegangsbeheer](../active-directory/role-based-access-built-in-roles.md) om te beheren gebruikersbevoegdheden, maar het is belangrijk om te controleren of laagste bevoegdheidsniveau machtigingen voor andere bronnen die hun eigen machtigingen zoals SQL Server systemen. 

## <a name="telemetry"></a>Telemetrielogboek

- **Meld u telemetriegegevens terwijl de toepassing wordt uitgevoerd in de productieomgeving.** Robuuste telemetrielogboek gegevens vastleggen terwijl de toepassing wordt uitgevoerd in de productieomgeving of hebt u geen voldoende gegevens aan een diagnose stellen bij de oorzaak van problemen terwijl deze gebruikers actief fungeert. Meer informatie vindt u in de logboekregistratie aanbevolen procedures is beschikbaar in de [controle- en hulpprogramma's voor diagnose richtlijnen] [ monitoring-and-diagnostics-guidance] document.

- **Implementeer logboekregistratie met een asynchroon patroon.** Als logboekregistratie bewerkingen synchroon, kunnen ze uw toepassingscode blokkeren. Controleer of uw bewerkingen logboekregistratie zijn geïmplementeerd als asynchrone bewerkingen. 

- **Relateren logboekgegevens naar een service.** In een normale n lagen-toepassing, kan een gebruikersaanvraag grenzen van de verschillende services bladeren. Bijvoorbeeld is een aanvraag meestal afkomstig is in de weblaag en doorgegeven aan de laag bedrijven en ten slotte doorgevoerd in de gegevenslaag. In meer complexe scenario's, kan een gebruikersaanvraag worden verdeeld over veel verschillende services en gegevens winkels. Controleer of uw systeem logboekregistratie verbindt oproepen naar een service, zodat u de aanvraag in uw toepassing bijhouden kunt.

##  <a name="azure-resources"></a>Azure Resources 

- **Azure resourcemanager sjablonen naar inrichten bronnen gebruiken.** Resourcemanager sjablonen kunnen u eenvoudiger implementaties via PowerShell of de CLI Azure, die naar een betrouwbare implementatieproces leidt te automatiseren. Zie [overzicht van de Azure resourcemanager]voor meer informatie[resource-manager].

- **Geef resources betekenisvolle namen.** Resources betekenisvolle namen geeft gemakkelijker naar een specifieke bron zoekt en begrijpt de rol. Zie [aanbevolen naamgevingsconventies voor Azure resources](guidance-naming-conventions.md) voor meer informatie 

- **Toegangsbeheer gebruiken op basis van rollen (RBAC)**. RBAC gebruiken voor toegang tot de Azure bronnen die u implementeert. RBAC kunt u autorisatie rollen toewijzen aan leden van uw team DevOps die u wilt voorkomen dat per ongeluk wordt verwijderd of wijzigingen geïmplementeerd resources. Zie voor meer informatie [aan de slag met toegangsbeheer in de portal van Azure](../active-directory/role-based-access-control-what-is.md) 

- **Resource-vergrendelingen voor kritieke resources, zoals VMs gebruiken.** Resource-vergrendelingen voorkomen dat een operator per ongeluk verwijdert een resource. Zie voor meer informatie, [vergrendelen resources met Azure Resource Manager](../resource-group-lock-resources.md) 

- **Regionale paren.** Wanneer u de implementatie in twee delen, kiest u regio's van dezelfde regionale twee. Herstel van één regio is in het geval van een brede storing prioriteit afmelden bij elk paar gegevenspunten. Sommige services zoals geografische-redundante opslag bieden automatische replicatie de gepaarde regio. Zie voor meer informatie [bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR): Azure gepaarde t-toets regio's](../best-practices-availability-paired-regions.md) 

- **Resourcegroepen indelen door de functie en de levenscyclus**.  Een resourcegroep moet in het algemeen, de resources die delen van de levenscyclus van dezelfde bevatten. Dit eenvoudiger implementaties beheren, test implementaties verwijderen en toewijzen van toegangsrechten, waarmee de kans dat een productie-implementatie per ongeluk wordt verwijderd of gewijzigd. Afzonderlijke resourcegroepen voor productie, ontwikkeling, maken en test omgevingen. In een implementatie meerdere landen/regio, plaatst u resources voor elke regio in afzonderlijke resourcegroepen. Dit vergemakkelijkt het implementeren van één regio handhaven van de andere regio('s). 

## <a name="azure-services"></a>Azure Services 

De volgende controlelijstitems zijn van toepassing op specifieke services in Azure wordt aangegeven.

###  <a name="app-service"></a>App-Service 

- **Gebruik standaard of Premium laag.** Deze lagen tijdelijk opslaan sleuven ondersteuning en automatische back-ups. Zie voor meer informatie [uitgebreide overzicht van Azure App-Service plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) 

- **Voorkom schaalbaarheid omhoog of omlaag.** In plaats daarvan een laag en -exemplaar grootte selecteren die voldoen aan de vereisten van uw prestaties onder typische laden en [schaal van](../app-service-web/web-sites-scale.md) de exemplaren u omgaat met wijzigingen in het verkeersvolume van. Schaalbaarheid kan omhoog en omlaag starten en een toepassing opnieuw starten.  

- **Store-configuratie als de app-instellingen.** Gebruik de app-instellingen wilt configuratie-instellingen als de app-instellingen. De instellingen in uw resourcemanager sjablonen of via PowerShell, zodat u kunt deze als onderdeel van een geautomatiseerde implementatie toepast / bijwerken proces, dat wil meer betrouwbare zeggen definiëren. Zie [web-apps in Azure App-Service configureren](../app-service-web/web-sites-configure.md)voor meer informatie. 

- **Maak afzonderlijke App Service-abonnementen voor productie en test.** Sleuven in de implementatie productie niet worden gebruikt voor het testen.  Alle apps binnen hetzelfde App Service abonnement delen de dezelfde VM exemplaren. Als u productie en test implementaties in hetzelfde abonnement hebt opgeslagen, kan dit een negatieve invloed op de productie-implementatie. Laden tests kunnen bijvoorbeeld de live productiesite afnemen. Door te plaatsen test implementaties in een afzonderlijk abonnement, isoleren u ze uit de versie van de.  

- **Afzonderlijke WebApps vanaf het web API's**. Als uw oplossing een front web en een web API heeft, kunt u ze ontleedt in afzonderlijke App Service-apps. Dit ontwerp gemakkelijker om de oplossing door werkbelasting. U kunt de web-app en de API uitvoeren in afzonderlijke App Service-abonnementen, zodat ze onafhankelijk van elkaar kunnen worden aangepast. Als u niet nodig hebt dat niveau van schaalbaarheid bij eerst, kunt u de apps implementeren in hetzelfde plan, en deze verplaatsen in afzonderlijke plannen later, indien nodig.

- **Vermijd het gebruik van de App Service back-upfunctie back-up Azure SQL-databases.** In plaats daarvan gebruiken [SQL-Database automatische back-ups][sql-backup]. Back-up App Service Hiermee exporteert u de database naar een SQL .bacpac-bestand, dat DTUs kosten.  

- **Dashboard implementeren naar een tijdelijk opslaan slot.** Maak een slot implementatie voor tijdelijke. Toepassingsupdates implementeren naar het tijdelijk opslaan slot en controleer of de implementatie voordat u deze in bedrijf verwisselen. Hierdoor wordt de kans van een ongeldige update in productie. Ook zorgt u ervoor dat alle processen worden temperatuur voordat het wordt omgewisseld Exchange. Veel toepassingen hebben een aanzienlijk warmup en verkoudheid begintijd. Zie de [tijdelijke omgevingen voor web-apps in Azure-Service voor App instellen](../app-service-web/web-sites-staged-publishing.md)voor meer informatie. 

-  **Maak een slot implementatie waarin de laatste bekende goede (LKG)-implementatie.** Wanneer u een update productie implementeert, moet u de vorige productie-implementatie verplaatsen naar de LKG slot. Dit eenvoudiger een ongeldige implementatie terugdraaien. Als u later een probleem ontdekken, kunt u snel de LKG versie herstellen. Zie voor meer informatie [Azure verwijzing architectuur: eenvoudige webtoepassing](guidance-web-apps-basic.md). 

- **Diagnostische gegevens vastleggen inschakelen**, inclusief toepassing logboekregistratie en logboekregistratie van webserver. Logboekregistratie is belangrijk voor controle en diagnostische gegevens. Zie [Diagnostische gegevens vastleggen voor web-apps in Azure App-Service inschakelen](../app-service-web/web-sites-enable-diagnostic-log.md)

- **Log to blob storage**. Zo makkelijker voor het verzamelen en de gegevens te analyseren. 

- **Maak een account afzonderlijk opslag voor Logboeken.** Gebruik geen hetzelfde account opslag voor logboeken en toepassingsgegevens. Dit helpt voorkomen dat logboekregistratie van het verkleinen van de prestaties van toepassingen. 

- **Prestaties controleren.** U kunt gebruiken een service zoals [Nieuwe Relic](http://newrelic.com/) of [Toepassing inzichten](../application-insights/app-insights-overview.md) monitor toepassing prestatie en werking onder laden.  Prestatiecontroles biedt u realtime inzicht in de toepassing. U kunt een diagnose stellen bij problemen en de onderliggende oorzaak uitvoeren van fouten. 


###  <a name="application-gateway"></a>Toepassingsgateway 

- **Ten minste twee instanties inrichten.** Gateway-toepassing implementeren met ten minste twee exemplaren. Een enkel exemplaar is een potentieel risico. Twee of meer exemplaren voor redundantie en schaalbaarheid gebruiken. Om de [SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/), moet u twee of meer medium of groter exemplaren inrichten. 

### <a name="azure-search"></a>Azure zoeken

- **Meer dan één replica inrichten.** Gebruik ten minste twee replica's voor meer hoge beschikbaarheid of drie voor alleen-lezen-schrijven hoge beschikbaarheid.

- **Configureer indexeerfuncties voor meerdere landen/regio-implementaties.** Als u een meerdere landen/regio-implementatie hebt, kunt u opties voor bedrijfscontinuïteit in het indexeren.

    - Als de gegevensbron geografische gerepliceerd is, wijst u elke indexering van elke regionale Azure-zoekservice de bronreplica lokale gegevens.  

    - Als de gegevensbron niet geografische gerepliceerd is, wijst u meerdere indexeerfuncties aan dezelfde gegevensbron, zodat de services Azure zoeken in meerdere regio's continu en onafhankelijk uit de gegevensbron indexeren. Zie voor meer informatie, [Azure zoeken prestaties en optimalisatie overwegingen][search-optimization].

###  <a name="azure-storage"></a>Azure-opslag 

- **Voor de toepassingsgegevens, gebruikt u leestoegang geografische-redundante opslag (AB-GRS).** AB-GRS opslag wordt overgenomen door de gegevens naar een secundaire gebied en bevat alleen-lezentoegang uit de tweede regio. Als er een storing opslag in de primaire regio, kan de gegevens in de toepassing lezen uit de tweede regio. Zie [Azure Storage replicatie](../storage/storage-redundancy.md)voor meer informatie.

- **Voor VM schijven Premium opslagruimte gebruiken** Zie voor meer informatie [Premium opslag: High-Performance-opslag voor Azure virtuele machines werkbelasting](../storage/storage-premium-storage.md).

- **Maak een back-wachtrij in een ander gebied voor de opslag in de wachtrij.** Voor de opslag in de wachtrij, heeft een alleen-lezen-replica gebruiken, beperkt omdat u geen wachtrij of items in wachtrij. Maak een back-wachtrij in plaats daarvan in een account opslag in een andere regio. Als er een storing opslag, kan de toepassing de back-wachtrij gebruiken totdat de primaire regio weer beschikbaar. Op deze manier de toepassing kan nog steeds verzoeken om nieuwe verwerkt.  

###  <a name="documentdb"></a>DocumentDB 

- **De database repliceren in regio's.** Met een account voor meerdere landen/regio heeft uw database DocumentDB één schrijven regio en meerdere gelezen regio's. Als er een fout in de regio schrijven, kunt u lezen in een andere replica. De Client SDK verwerkt dit automatisch. U kunt ook niet via de regio schrijven naar een andere regio. Zie [verdelen gegevens globaal met DocumentDB](../documentdb/documentdb-distribute-data-globally.md)voor meer informatie.


###  <a name="sql-database"></a>SQL-Database 

- **Gebruik standaard of Premium laag.** Deze lagen bieden een periode meer point-in-time herstellen (35 dagen). Zie [Opties voor de SQL-Database en prestaties](../sql-database/sql-database-service-tiers.md)voor meer informatie.

- **Schakel controle in SQL-Database.** Controle kan worden gebruikt voor de diagnose stellen bij aanvallen of menselijke fouten. Zie [aan de slag met het controleren van de SQL-database](../sql-database/sql-database-auditing-get-started.md)voor meer informatie. 

- **Gebruik actieve geografische-herhaling** Actieve geografische-replicatie gebruiken om te maken van een leesbare secundair in een andere regio.  Als de primaire database mislukt, of gewoon moet worden verbroken, een handmatige failover naar de secundaire database uitvoeren.  Totdat u niet, blijft de secundaire database alleen-lezen.  Zie [SQL actieve geografische-databasereplicatie](../sql-database/sql-database-geo-replication-overview.md)voor meer informatie. 

- **Gebruik sharding**. Kunt u de database horizontaal partitioneren met sharding. Sharding kan foutenstructuuranalyse moeten worden geïsoleerd bieden. Zie [schaal af met Azure SQL-Database](../sql-database/sql-database-elastic-scale-introduction.md)voor meer informatie. 

- **Gebruik point-in-time herstellen herstellen uit menselijke fouten.**  Point-in-time herstellen retourneert de database op een eerder in tijd. Voor meer informatie raadpleegt u [een automatische back-ups met Azure SQL-database herstellen][sql-restore].

- **Gebruik geografische herstellen herstellen uit een servicestoring.** Geografische herstellen herstelt u een database van een geografische-redundante back-up.  Voor meer informatie raadpleegt u [een automatische back-ups met Azure SQL-database herstellen][sql-restore].


###  <a name="sql-server-running-in-a-vm"></a>SQL Server (deze optie wordt uitgevoerd in een VM)

- **De database repliceren.** SQL Server altijd op beschikbaarheid hierin groepen wilt gebruiken om de database te repliceren. Biedt beschikbaarheid als één SQL Server-instantie mislukt. Voor meer informatie raadpleegt u [meer informatie...](guidance-compute-n-tier-vm.md) 

- Een **back-up van de database**. Als u al [Azure back-up](https://azure.microsoft.com/documentation/services/backup/) back-up uw VMs gebruikt, kunt u overwegen [Azure back-up voor SQL Server-werkbelastingen DPM gebruiken](../backup/backup-azure-backup-sql.md). Met deze methode, moet u er een back-beheerdersrol voor de organisatie en een geïntegreerd herstelprocedure voor VMs en SQL Server is. [SQL Server beheerd back-up Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx)anders gebruiken. 


###  <a name="traffic-manager"></a>Verkeer Manager 

- **Handmatige failback uitvoeren.** Na een failover verkeer Manager uitvoeren handmatige failback, in plaats van de automatisch verbroken terug. Controleer of alle toepassing subsystemen in orde zijn voordat terug verbroken.  Anders kunt u een situatie waar de toepassing gespiegeld heen en weer tussen datacenters maken. Zie [VMs uitgevoerd in meerdere regio's](guidance-compute-multiple-datacenters.md)voor meer informatie.

- **Een systeemstatus test-eindpunt maken**. Maak een aangepaste eindpunt die over de algemene status van de toepassing rapporten. Hiermee worden verkeer Manager worden uitgevoerd als een kritieke pad mislukt, niet alleen de front-end. Het eindpunt moet een HTTP-foutcode retourneren als een kritieke afhankelijkheid beschadigd of niet bereikbaar is. Fouten voor niet-kritieke services, niet echter rapporteren De status-test mogelijk anders leiden tot failover wanneer deze niet nodig, onrechte maken. Zie [verkeer Manager eindpunt controle en failover](../traffic-manager/traffic-manager-monitoring.md)voor meer informatie.


###  <a name="virtual-machines"></a>Virtuele Machines 

- **Voorkomen dat een werkbelasting productie uitgevoerd op een enkele VM.** Een enkel VM-implementatie is niet tegen of niet gepland onderhoud. In plaats daarvan meerdere VMs in een beschikbaarheid instellen of VM schaal instellen, met een taakverdeling op voorgrond plaatsen. Om in aanmerking voor de SLA, te moet ten minste twee virtuele Machines zijn geïmplementeerd in dezelfde beschikbaarheid van de reeks. Zie [VM schaal Sets overzicht](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)voor meer informatie. 

- **Geef de beschikbaarheid instellen wanneer u de VM inrichten.** Er is momenteel geen manier om een VM resourcemanager toevoegen aan een beschikbaarheid instellen nadat de VM is ingericht. Wanneer u toevoegt een nieuwe VM aan een bestaande beschikbaarheid instellen, zorg ervoor dat een NIC maken voor de VM, en de NIC toevoegen aan de groep back-enddatabase adressen op de taakverdeling. De taakverdeling won't anders netwerkverkeer doorsturen naar die VM. 

- **Zet elk toepassingslaag in een afzonderlijk beschikbaarheid instellen.** In een toepassing N lagen, niet plaats VMs van verschillende niveaus in dezelfde beschikbaarheid set. VMs in een set beschikbaarheid voor foutenstructuuranalyse domeinen (FDs) worden geplaatst en domeinen (per) bijwerken. Als u de redundantie wilt profiteren van FDs en UDs, moet elke VM in de set beschikbaarheid wel het aanvragen van dezelfde clients aankunnen. 

- **Kies de juiste VM grootte op basis van prestaties vereisten.** Wanneer u een bestaande werklast verplaatst naar Azure, begint u met de VM-grootte die het meest overeenkomt met uw on-premises implementatie-servers. Vervolgens de prestaties van uw werkelijke werkbelasting met betrekking tot processor en geheugen schijf IO's / s meten en de tekengrootte aanpassen indien nodig. Dit zorgt ervoor dat de toepassing naar verwachting in een cloud-omgeving werkt. Als u meerdere NIC nodig hebt, moet u op de hoogte van de limiet NIC voor elke grootte. 

- **Gebruik premium opslag voor VHD's.** Azure Premium opslagruimte biedt ondersteuning voor high-performance, lage latentie schijven. Zie voor meer informatie [Premium opslag: High-Performance-opslag voor Azure virtuele machines werkbelasting](../storage/storage-premium-storage.md) Kies een grootte VM die ondersteuning biedt voor premium opslag. 

- **Maak een account afzonderlijk opslag voor elke VM.** Plaats de VHD's voor één VM in een aparte opslag-account. Deze manier kunt u door de limieten IO's / s voor opslag-accounts voorkomt. Zie [Azure opslag schaalbaarheid en prestaties doelen](../storage/storage-scalability-targets.md)voor meer informatie. Als u een groot aantal VMs implementeert, worden echter op de hoogte van de limiet per abonnement voor opslag-accounts. Zie [opslaglimieten](../azure-subscription-service-limits.md#storage-limits).

- **Een account afzonderlijk opslag voor diagnostische logboeken maken**. Niet schrijven diagnostische logboeken aan hetzelfde opslag-account als de VHD's, om te voorkomen dat de diagnostische gegevens vastleggen van invloed op de IO's / s voor de VM schijven. Een standaardaccount lokaal redundante opslag (LRS) is voldoende voor diagnostische logboeken. 

- **-Toepassingen installeren op een gegevensschijf, niet de schijf OS.** U kunt de schijf groottelimiet overschrijden anders mogelijk bereikt. 

- **Azure back-up back-up VMs gebruiken.** Back-ups bescherming tegen verlies van gegevens per ongeluk. Zie [Azure VMs beveiligen met een herstel services kluis](../backup/backup-azure-vms-first-look-arm.md)voor meer informatie.

- **Diagnostische logboeken inschakelen**, met inbegrip van eenvoudige systeemstatus maatstaven, infrastructuur-logboeken en [opstarten diagnostisch hulpprogramma][boot-diagnostics]. Opstarten diagnostische hulpprogramma's kunt u een diagnose stellen bij een fout bij het opstarten als uw VM in een niet-opgestart wordt. Voor meer informatie raadpleegt u [Overzicht van Azure diagnostische logboeken][diagnostics-logs].

- **Gebruik de extensie AzureLogCollector**. (Alleen in Windows-VMs.) Dit toestel Azure platform Logboeken is samengevoegd en uploadt ze naar de Azure opslag, zonder de operator extern aanmelden bij de VM. Zie [AzureLogCollector-extensie](../virtual-machines/virtual-machines-windows-log-collector-extension.md)voor meer informatie.


###  <a name="virtual-network"></a>Virtual Network 

- **"Witte" lijst of blok toevoegen openbare IP-adressen, een NSG aan het subnet.** Toegang via schadelijke gebruikers, van blokkeren of toestaan access alleen gebruikers met bevoegdheden voor toegang tot de toepassing.  

- **Maak een aangepaste status-test.** Controleert of laden verdeling systeemstatus kunt testen HTTP of TCP. Als een VM een HTTP-server wordt uitgevoerd, is de HTTP-test een betere indicator systeemstatus status dan een TCP-test. Voor een HTTP-test, gebruikt u een aangepaste-eindpunt dat de algemene status van de toepassing, inclusief alle kritieke afhankelijkheden-rapporten. Zie [overzicht van taakverdeling Azure](../load-balancer/load-balancer-overview.md)voor meer informatie.

- **De status-test niet worden geblokkeerd.** De belasting voor gelijkmatige verdeling systeemstatus-test is verzonden vanuit een bekende IP-adres, 168.63.129.16. Niet blokkeren verkeer naar of uit deze IP-in een firewall beleidsregels of netwerk-beveiligingsregels groep (NSG). Blokkeren van de servicestatus-test zou leiden tot de verdeling van de belasting de VM uit draaiing verwijderen. 

- **Taakverdeling-logboekregistratie inschakelt.** De logboeken hoeveel VMs weergeven op de back-enddatabase ontvangen geen netwerkverkeer vanwege mislukte test antwoorden. Zie [Log analytische gegevens voor de verdeling van Azure belasting](../load-balancer/load-balancer-monitor-log.md)voor meer informatie.


<!-- links -->
[app-service-autoscale]: ../monitoring-and-diagnostics/insights-how-to-scale.md
[asynchronous-c-sharp]:https://msdn.microsoft.com/library/mt674882.aspx
[availability-sets]:../virtual-machines/virtual-machines-windows-manage-availability.md
[azure-backup]: https://azure.microsoft.com/documentation/services/backup/
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[cloud-service-autoscale]: ../cloud-services/cloud-services-how-to-scale.md
[diagnostics-logs]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[fma]: guidance-resiliency-failure-mode-analysis.md
[guidance-resilient-deployment]: guidance-resiliency-overview.md#resilient-deployment
[load-balancer]: load-balancer/load-balancer-overview.md
[monitoring-and-diagnostics-guidance]: ../best-practices-monitoring.md
[resource-manager]: ../azure-resource-manager/resource-group-overview.md
[retry-pattern]: https://msdn.microsoft.com/library/dn589788.aspx
[retry-service-guidance]: ../best-practices-retry-service-specific.md
[search-optimization]: ../search/search-performance-optimization.md
[sql-backup]: ../sql-database/sql-database-automated-backups.md
[sql-restore]: ../sql-database/sql-database-recovery-using-backups.md
[traffic-manager]: ../traffic-manager/traffic-manager-overview.md
[traffic-manager-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[vmss-autoscale]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md
