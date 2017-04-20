<properties
   pageTitle="Beschikbaarheid controlelijst | Microsoft Azure"
   description="Een snelle controlelijst met instellingen en acties die u uitvoeren kunt om ervoor te zorgen dat u de beschikbaarheid van uw toepassingen met Azure verbeteren."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="high-availability-checklist"></a>Beschikbaarheid controlelijst
Een van de voordelen van het gebruik van Azure is de mogelijkheid om uit te breiden beschikbaarheid (en schaalbaarheid) van uw toepassingen met behulp van de cloud. Om ervoor te zorgen dat u optimaal gebruik van deze opties wilt maken, worden de onderstaande controlelijst is bedoeld om u te helpen met een deel van de belangrijkste infrastructuur basisbeginselen naar ervoor te zorgen dat uw toepassingen robuuste. 

>[AZURE.NOTE] De meeste van de volgende suggesties zijn zaken aan bod die kunnen worden geïmplementeerd op elk gewenst moment in uw toepassing en kan dus zijn zeer geschikt voor snelle reparaties",". De beste oplossing vaak heeft betrekking op het ontwerp van een toepassing die is ingebouwd voor de cloud.  Voor een controlelijst op deze (meer ontwerp oriënteren gebieden, Lees onze [beschikbaarheid controlelijst](../best-practices-availability-checklist.md).

###<a name="are-you-using-traffic-manager-in-front-of-your-resources"></a>Gebruikt u verkeer Manager vóór uw resources?
Gebruikt, wordt verkeer Manager kunt routeren u internetverkeer via Azure regio of Azure en on-premises locatie. U kunt dit doen voor een aantal redenen waaronder latentie en beschikbaarheid. Als u meer informatie over het gebruik van Manager verkeer wilt naar uw tolerantie vergroten en uw verkeer naar meerdere gebieden verspreiden, leest u [VMs uitgevoerd in meerdere datacenters op Azure beschikbaarheid](../guidance/guidance-compute-multiple-datacenters.md).

__Wat gebeurt er als u niet verkeer Manager gebruikt?__ Als u geen verkeer Manager vóór de toepassing gebruikt, bent u beperkt tot één regio voor uw resources. Dit beperkt schaal, wordt verhoogd latentie aan gebruikers die niet dicht bij uw door u gekozen regio zijn en Hiermee verlaagt u de beveiliging in het geval van een regio hele service-onderbreking.

###<a name="have-you-avoided-using-a-single-virtual-machine-for-any-role"></a>Hebt u voorkomen met behulp van een één virtuele machine voor een functie?
Goede ontwerp voorkomt u eventuele potentieel risico. Dit is belangrijk in het ontwerp van alle service (on-premises implementatie of in de cloud), maar is vooral handig zijn in de cloud schaalbaarheid en tolerantie maar schalen (toe te voegen virtuele machines) kunt te vergroten in plaats van de schaalbaarheid (met behulp van een krachtiger VM). Als u meer informatie over het ontwerp van scalable toepassing achterhalen wilt, leest u [de beschikbaarheid voor toepassingen gebaseerd op Microsoft Azure](resiliency-high-availability-azure-applications.md).

__Wat gebeurt er als er een één virtuele machine voor een rol?__ Één computer is een potentieel risico en is niet beschikbaar voor de [Azure virtuele machines Service Level Agreement](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/). In de beste gevallen, de toepassing wordt uitgevoerd dan grote maar dit is niet het ontwerp van een robuuste, valt buiten de Azure virtuele machines SLA en eventuele potentieel mislukt vergroot de kans op downtime als er iets is mislukt.

###<a name="are-you-using-a-load-balancer-in-front-of-your-applications-internet-facing-vms"></a>Gebruikt u een taakverdeling vóór van uw toepassing internetgerichte VMs?
Netwerktaakverdelers kunnen u het binnenkomende verkeer naar uw toepassing verdeeld over een willekeurig aantal machines. U kunt toevoegen/verwijderen machines uit uw taakverdeling op elk gewenst moment, werkt ook met virtuele Machines (en ook met de automatisch schalen met VM schaal Sets) zodat u kunt gemakkelijk afhandelen toename van verkeer en VM mislukte. Als u meer weten over netwerktaakverdelers wilt, lees dan het [overzicht van taakverdeling Azure](../load-balancer/load-balancer-overview.md) en uit te [voeren meerdere VMs op Azure voor schaalbaarheid en beschikbaarheid](../guidance/guidance-compute-multi-vm.md).

__Wat gebeurt er als u niet werkt met een taakverdeling vóór uw VMs internetgerichte?__ Zonder een taakverdeling is niet mogelijk aan de nieuwe schaal af (meer virtuele machines toevoegen) en wordt de enige optie om uit te breiden (vergroot het formaat van uw web-omlaag virtuele machine). U wordt ook een potentieel risico met die virtuele machine betrokken. U moet ook DNS-code zoals u ziet als u de computer van een internetverbinding hebt verloren en in kaart brengen opnieuw wilt opnemen in de DNS-met de nieuwe computer die u gaat overnemen schrijven.

###<a name="are-you-using-availability-sets-for-your-stateless-application-and-web-servers"></a>Weet u beschikbaarheid gebruiken voor uw stateless-toepassing en web-servers wordt ingesteld?
Uw machines plaatsen in de dezelfde toepassingslaag in een set beschikbaarheid zorgt ervoor dat uw VMs in aanmerking komen voor de Azure VM SLA. Deel van een beschikbaarheid ook zorgt ervoor dat uw computers in andere update domeinen (dat wil zeggen andere host machines die is geïnstalleerd op verschillende momenten) worden gebracht en fault domeinen (dat wil zeggen host machines die een gemeenschappelijke power bron- en netwerk delen activeren). Uw VMs kunnen op dezelfde hostcomputer vinden zonder dat ze in een set beschikbaarheid, en dus is er mogelijk een potentieel risico die niet zichtbaar is voor u. Als u zoeken naar meer informatie wilt over de beschikbaarheid van uw beschikbaarheid sets met VMs met groter wordende, leest u [de beschikbaarheid van virtuele machines beheren](../virtual-machines/virtual-machines-windows-manage-availability.md).

__Wat gebeurt er als u niet een beschikbaarheid instellen met uw stateless toepassingen en -endwebservers gebruikt?__ Geen gebruikmaakt van een beschikbaarheid instellen betekent dat u niet kunt profiteren van de Azure VM SLA. Het betekent ook dat machines in die laag van uw toepassing kunnen alle offline gaan als er is een update op de hostcomputer (de computer waarop de VMs die u gebruikt) of een algemene hardware is mislukt.

###<a name="are-you-using-virtual-machine-scale-sets-vmss-for-your-stateless-application-or-web-servers"></a>Gebruikt u VM schaal Sets (VMSS) voor uw stateless toepassing of web-servers?
Een goede scalable en robuust ontwerp wordt VMSS gebruikt om ervoor te zorgen dat u kunt vergroten/verkleinen het aantal computers in een laag van uw toepassing (zoals uw weblaag). VMSS, kunt u bepalen hoe uw toepassingslaag schalen (servers toevoegen of verwijderen op basis van criteria die u kiest). Als u meer informatie over het gebruik van Azure virtuele machines schaal Sets om uit te breiden uw tolerantie om verkeer pieken te vinden wilt, leest u [VM schaal Sets overzicht](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

__Wat gebeurt er als u ons een virtuele Machine schaal instellen met mijn stateless toepassing van de webserver niet?__ Zonder een VMSS, kunt u de mogelijkheid om te schalen zonder beperkingen en het gebruik van resources optimaliseren beperken. Een ontwerp VMSS zonder schaal bovengrens is die moet worden afgehandeld met extra code (of handmatig). Het gebrek aan een VMSS betekent ook dat uw toepassing niet gemakkelijk kunt toevoegen en verwijderen van machines (ongeacht het schaal) zodat u grote pieken verkeer verwerken (bijvoorbeeld als u tijdens een promotie of als uw site, de app/het product virus gaat).

###<a name="are-you-using-premium-storage-and-separate-storage-accounts-for-each-of-your-virtual-machines"></a>Gebruikt u premium-opslag en aparte opslag accounts voor elk van uw virtuele machines?
Dit is een goede gewoonte om premium opslagruimte voor uw productie virtuele machines gebruiken. Daarnaast moet u ervoor zorgen dat u een aparte opslag-account voor elke virtuele machine gebruikt (dit is waar voor kleine implementaties. Voor grotere implementaties die u opnieuw kunt gebruiken opslag accounts voor meerdere computers maar er is een verdelen dat moet worden uitgevoerd om ervoor te zorgen u zijn verdeeld tussen update domeinen en lagen van uw toepassing). Als u zoeken naar meer informatie over de opslag van Azure prestaties en schaalbaarheid wilt, leest u [Microsoft Azure opslag prestaties en schaalbaarheid controlelijst](../storage/storage-performance-checklist.md).

__Wat gebeurt er als u niet afzonderlijk opslag-accounts voor elke virtuele machine gebruikt?__ Een opslag-account, zoals vele andere bronnen is een potentieel risico. Hoewel er veel bescherming en tolerantiefuncties van Azure-opslag, is een potentieel risico nooit een goede ontwerp. Bijvoorbeeld als toegangsrechten beschadigd aan dat account, een opslaglimiet is bereikt, of een [IO's / s beperken](../azure-subscription-service-limits.md#virtual-machine-disk-limits) is bereikt, worden alle virtuele machines met dat account opslag beïnvloed. Als er een service-onderbreking die een tijdstempel opslagruimte met dat account bepaalde opslag van invloed kan u ook meerdere virtuele machines beïnvloed hebben.

###<a name="are-you-using-a-load-balancer-or-a-queue-between-each-tier-of-your-application"></a>Gebruikt u een taakverdeling of een wachtrij tussen elke laag van uw toepassing?
Netwerktaakverdelers of wachtrijen tussen elke laag van uw toepassing gebruikt, kunt u gemakkelijk de schaal van elke laag van uw toepassing eenvoudig en onafhankelijk. U moet kiezen tussen deze technologieën op basis van uw latentie, complexiteit, en verdeling (dat wil zeggen hoeverre u verdeelt uw app) nodig heeft. In het algemeen, wachtrijen meestal hoger latentie hebt en complexiteit toevoegen maar voordelen aan wordt meer robuuste en waarmee u kunt uw toepassing over grotere gebieden verdelen (zoals tussen regio's). Als u meer informatie over het gebruik van interne netwerktaakverdelers of wachtrijen wilt, Lees [interne taakverdeling overzicht](../load-balancer/load-balancer-internal-overview.md) en [Azure wachtrijen en Service Bus wachtrijen - vergeleken en in afgezet tegen](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)opvragen.

__Wat gebeurt er als u niet een taakverdeling of een wachtrij tussen elke laag van uw toepassing gebruikt?__ Zonder een taakverdeling of wachtrij, tussen elke laag van uw toepassing is moeilijk te schalen dat uw toepassing omhoog of omlaag en distribueren van de belasting op meerdere computers. Hierdoor niet kan leiden tot boven of onder de inrichting van uw bronnen en een kans op pesterijen downtime of slechte gebruikerservaring, hebt u onverwachte wijzigingen in verkeer of.
 
###<a name="are-your-sql-databases-using-active-geo-replication"></a>Uw SQL-Databases actieve geografische-replicatie gebruikt? 
Actieve geografische-herhaling kunt u maximaal 4 leesbare secundaire databases in de regio's dezelfde, of een andere, configureren. Secundaire databases zijn beschikbaar in het geval van een service-onderbreking of problemen met de verbinding maken met de hoofddatabase. Als u meer weten over SQL-Database actieve geografische-replicatie wilt, leest u [Overzicht: SQL actieve geografische-databasereplicatie](../sql-database/sql-database-geo-replication-overview.md).
 
 __Wat gebeurt er als u niet de actieve geografische-replicatie met uw SQL-databases gebruikt?__ Zonder actieve geografische-herhaling, als de primaire database ooit offline gaat (gepland onderhoud, service-onderbreking, hardware mislukt, enzovoort) uw databasetoepassing ook offline totdat u weer online uw primaire database in orde zijn overbrengen kunt. 
 
###<a name="are-you-using-a-cache-azure-redis-cache-in-front-of-your-databases"></a>Gebruikt u een cache (Azure bestand Vgx. Cache) voor uw databases?
Als uw toepassing een hoge database laden waar de meeste van de database-oproepen lezen zijn heeft, kunt u de snelheid van uw toepassing vergroten en het selectievakje laden in uw database verkleinen door het implementeren van een in de cache laag vóór de database te dragen deze gelezen bewerkingen. U kunt de snelheid van uw toepassing vergroten en verkleinen van uw database laden (dus de schaal die kan verwerken vergroten) door een in de cache laag vóór de database plaatsen. Als u wilt meer informatie over de cache Azure bestand Vgx., leest u [cache richtlijnen](../best-practices-caching.md).
 
 __Wat gebeurt er als u een cache niet voor uw database gebruikt?__ Als de databasecomputer krachtig genoeg is voor de belasting plaatsen u op is geïnstalleerd en vervolgens uw toepassing als normaal reageert, hoewel dit betekent dat bij lagere belasting u betaalt voor een database machine die meer dure dan nodig is. Als de databasecomputer niet krachtige is ervaart genoeg te doen bij uw laden en u beginnen met wilt te veel slechte gebruiker u met uw toepassing (latentie-outs en mogelijk service downtime).
 
###<a name="have-you-contacted-microsoft-azure-support-if-you-are-expecting-a-high-scale-event"></a>Hebt u contact hebt opgenomen Microsoft Azure ondersteuning als u een hoge schaal gebeurtenis verwacht?
Azure ondersteuning kunt u uw service-limieten voor de geplande hoog verkeer gebeurtenissen (zoals nieuwe producten op de markt of speciale feestdagen) handelen vergroten. Azure ondersteuning ook mogelijk om te communiceren met experts die u helpen uw ontwerp met uw accountteam controleren en helpen u vindt de beste oplossing aan uw behoeften hoog schaal gebeurtenis. Als u meer informatie over hoe wilt u contact opnemen met ondersteuning van Azure vinden, leest u de [Azure ondersteunen Veelgestelde vragen](https://azure.microsoft.com/support/faq/).

__Wat gebeurt er als u geen contact opnemen met ondersteuning van Azure voor een gebeurtenis hoge schaalbaarheid?__ Als u niet communiceren of voor plannen, een gebeurtenis hoog verkeer, kunnen kunt u door bepaalde [Azure services bestandsgrootten](../azure-subscription-service-limits.md) en kan dus maken van een slechte gebruikerservaring (of erger, downtime) tijdens de gebeurtenis. Architectuur beoordelingen en communiceren ligt piekspanningen kunnen helpen deze risico's te beperken.

###<a name="are-you-using-a-content-delivery-network-azure-cdn-in-front-of-your-web-facing-storage-blobs-and-static-assets"></a>Gebruikt u een inhoud bezorging netwerk (Azure CDN) vóór uw web-omlaag opslag BLOB's en statische activa?
Een CDN gebruikt, kunt u laden uit uw servers door de cache van de inhoud op de CDN POP/edge-locaties die overal ter wereld bevinden zich te nemen. U kunt dit latentie verkleinen, schaalbaarheid vergroten, verkleinen van de belasting van de server, doen en als onderdeel van een strategie voor de beveiliging tegen weigering van service(DOS) aanvallen. Als u meer informatie over het gebruik van Azure CDN wilt naar uw tolerantie vergroten en verkleinen van uw klant-latentie vinden, leest u [overzicht van de Azure inhoud bezorging netwerk (CDN)](../cdn/cdn-overview.md).

__Wat gebeurt er als u niet een CDN gebruikt?__ Als u geen een CDN gebruikt vervolgens al uw klantverkeer wordt geleverd rechtstreeks naar uw resources. Dit betekent dat u hoger laadtijd ziet op de servers die hun schaalbaarheid afneemt. Uw klanten waarnemen bovendien hoger vertragingstijden als CDN's bieden locaties overal ter wereld die waarschijnlijk dichter naar uw klanten.

##<a name="next-steps"></a>Volgende stappen:
Als u wilt meer informatie over het ontwerpen van uw toepassingen voor hoge beschikbaarheid, Lees [beschikbaarheid voor toepassingen gebaseerd op Microsoft Azure](resiliency-high-availability-azure-applications.md).
