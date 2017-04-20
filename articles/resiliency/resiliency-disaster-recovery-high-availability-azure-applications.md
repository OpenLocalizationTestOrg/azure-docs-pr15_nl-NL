<properties
   pageTitle="Herstel en beschikbaarheid van Azure toepassingen | Microsoft Azure"
   description="Technische overzichten en diepteas informatie over het ontwerpen van toepassingen voor hoge beschikbaarheid herstel van toepassingen is gebaseerd op Microsoft Azure."
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

#<a name="disaster-recovery-and-high-availability-for-applications-built-on-microsoft-azure"></a>Problemen oplossen en beschikbaarheid voor toepassingen gebaseerd op Microsoft Azure

##<a name="introduction"></a>Inleiding

In dit artikel ligt de nadruk op beschikbaarheid voor toepassingen die wordt uitgevoerd in Azure wordt aangegeven. Een algemene strategie voor maximale beschikbaarheid bevat ook de aspecten van herstel. Fouten en systeemfouten in de cloud plannen, moet u de fouten snel herkend. U vervolgens implementeren een strategie die overeenkomt met uw tolerantie voor downtime van de toepassing. U moet bovendien Houd rekening met de mate van gegevensverlies die de toepassing mogelijk zonder zonder dat bedrijven nadelige gevolgen als dit is hersteld.

De meeste bedrijven dat ze zijn voorbereid voor tijdelijke en grootschalige fouten. Echter voordat u deze vraag voor uzelf beantwoorden, uw bedrijf tijdsinstellingen deze fouten? Test u het herstellen van databases zodat u de juiste processen hebt geplaatst? Waarschijnlijk niet. Dat komt doordat succesvolle herstel wordt gestart met een groot aantal plannen en uitbreidt implementatie van deze processen. Net als vele andere niet-functionele vereisten, zoals beveiliging en krijgt herstel zelden het vooraf analyse en de tijd toewijzing die is vereist. De meeste bedrijven hebben niet ook het budget voor geografisch verspreid regio's met overtollige capaciteit. Even opdracht kritieke toepassingen zijn daarom vaak uitgesloten van de juiste oplossen van problemen.

Cloud platforms, zoals Azure, bieden geografisch verspreid regio's overal ter wereld. Deze platforms bieden ook mogelijkheden die ondersteuning bieden voor beschikbaarheid en een verscheidenheid aan noodgevallen herstel scenario's. Nu elke opdracht kritieke cloud-toepassing kan worden opgegeven rekening voor noodgevallen controle van het systeem. Azure heeft tolerantie en herstel ingebouwd in veel van de services. U moet deze functies platform zorgvuldig bestuderen en aanvulling met toepassing strategieën.

In dit Artikeloverzichten de benodigde architectuur stappen nodig zijn om te noodgevallen bewijs een Azure-implementatie. Vervolgens kunt u de groter bedrijfsproces voor bedrijfscontinuïteit implementeren. Een plan voor bedrijfscontinuïteit is een routekaart voor doorlopende bewerkingen onder ongunstige omstandigheden. Dit kan een fout bij het technologie, zoals een uitgevallen service of een natuurlijke noodgevallen, zoals een storing storm of power zijn. Tolerantie voor systeemfouten is slechts een subset van de grotere herstelproces voor noodgevallen, zoals wordt beschreven in dit document NIST: [Planningshandleiding voor Information Technology Systems voor onvoorziene omstandigheden](https://www.fismacenter.com/sp800-34.pdf).

De volgende secties definiëren verschillende niveaus van fouten, technieken om af te handelen, en architecturen die ondersteuning bieden voor deze technieken. Deze informatie biedt invoer voor uw herstelproces voor noodgevallen en procedures, om te zorgen dat uw noodherstelplan efficiënt en correct werkt.

##<a name="characteristics-of-resilient-cloud-applications"></a>Kenmerken van robuuste cloud-toepassingen

Een toepassing voor de ook architectuur bestand zijn tegen de mogelijkheid fouten op een tactische niveau en deze mogelijk ook zonder strategische algemene fouten op het regioniveau van de. De volgende secties definiëren de terminologie waarnaar wordt verwezen in het hele document om te beschrijven van verschillende aspecten van robuuste cloudservices.

###<a name="high-availability"></a>Beschikbaarheid

Een toepassing voor maximaal beschikbare cloud implementeert strategieën om de storing van afhankelijkheden, zoals de beheerde services van het platform cloud absorbeert. Ondanks mogelijke fouten van het cloud-platform mogelijkheden kan deze methode de toepassing om door te gaan naar de verwachte functionele en niet-functionele systematische kenmerken vertonen. Hierover vindt u uitgebreidere in het kanaal 9 videoreeks, [Failsafe: richtlijnen voor het robuuste Cloud architecturen](https://channel9.msdn.com/Series/FailSafe).

Wanneer u de toepassing kunt implementeren, moet u rekening houden met de kans van een storing mogelijkheid. Let op de impact van een storing op de toepassing vanuit het perspectief bedrijven voordat diepblauwe verdiepen aan de implementatie-strategieën. Zonder vervaldatums rekening met de gevolgen voor bedrijven en de kans van de voorwaarde risico raken, mogelijk onnodige en duur, kan de uitvoering zijn.

Houd rekening met een auto overeenkomstige beschikbaarheid. Zelfs onderdelen van de kwaliteit en bovenliggende engineering voorkomt niet incidentele fouten. Bijvoorbeeld wanneer uw auto een platte band krijgt, de auto nog steeds wordt uitgevoerd, maar met anders werken functionaliteit wordt uitgevoerd. Als u voor dit exemplaar van de mogelijke gepland, kunt u een van deze dunne-met rand vrije banden totdat u een garage hebt bereikt. Hoewel de vrije band hoge snelheden niet toestaat, kunt u het voertuig nog steeds gebruiken totdat u de band vervangen. Daarnaast kunt een cloudservice plannen voor mogelijke verlies van functionaliteit voorkomen een relatief klein probleem waarmee u de hele toepassing. Dit geldt ook als de cloudservice moet worden uitgevoerd met functionaliteit voor anders werken.

Er zijn enkele belangrijke kenmerken van maximaal beschikbare cloudservices: beschikbaarheid, schaalbaarheid en fouttolerantie. Hoewel deze kenmerken verwant zijn, is het belangrijk om te weten elk, en hoe ze bijdragen aan de algemene beschikbaarheid van de oplossing.

###<a name="availability"></a>Beschikbaarheid

Een beschikbare toepassing houdt rekening met de beschikbaarheid van de onderliggende infrastructuur en afhankelijke services. Beschikbare toepassingen verwijderen potentiële risico via redundantie en robuuste ontwerp. Wanneer u het zoekbereik u rekening moet houden beschikbaar is in Azure vergroten, is het is belangrijk is voor meer informatie over het concept van het effectieve beschikbaarheid van het platform. De beschikbaarheid van de effectieve houdt rekening met de Service niveau overeenkomsten (SLA) van elke afhankelijke service en hun cumulatieve invloed op de systeembeschikbaarheid van de totale.

Beschikbaarheid van het systeem is de eenheid van het percentage van een tijdvenster die het systeem kunnen werken. De beschikbaarheid SLA van ten minste twee instanties van een webpagina of werknemer rol in Azure is bijvoorbeeld 99.95 percentage (niet 100 procent). Het bevat niet de prestaties of de functionaliteit van de services worden uitgevoerd op die rollen meten. De effectieve beschikbaarheid van uw cloudservice is echter ook beïnvloed door de verschillende serviceovereenkomsten van de andere afhankelijke services. Meer bewegende onderdelen binnen het systeem, de belangrijk die u uitvoeren moet om te zorgen dat de toepassing kunt resiliently voldoen aan de vereisten van de beschikbaarheid van de eindgebruikers.

Houd rekening met de volgende serviceovereenkomsten voor een Azure-service die gebruikmaakt van Azure services: berekeningscluster, Azure SQL-Database en Azure Storage.

|Azure-service|SLA   |Mogelijke minuten downtime/maand (30 dagen)|
|:------------|:-----|:----------------------------------------:|
|Berekenen      |99.95%|21,6 minuten                              |
|SQL-Database |99,99%|4.3 minuten                              |
|Opslag      |99.90%|43,2 minuten                              |

U moet plannen voor alle services mogelijk omlaag te gaan op verschillende momenten. Het totale aantal minuten per maand die de toepassing is niet beschikbaar is in dit voorbeeld vereenvoudigd 108 minuten. Een maand 30 dagen heeft een totaal van 43,200 minuten. 108 minuten is.25 procent van het totale aantal minuten in een maand 30 dagen (43,200 minuten). Hiermee kunt u een effectieve beschikbaarheid van 99.75 percentage voor de cloudservice.

Echter kunt met beschikbaarheid technieken beschreven in dit artikel verbeteren dit. Bijvoorbeeld als u de toepassing doorgaan worden uitgevoerd wanneer de SQL-Database niet beschikbaar is ontwerpt, kunt u verwijderen die uit de vergelijking. Dit betekent dat de toepassing wordt uitgevoerd met beperkte mogelijkheden, zodat er ook bedrijfsbehoeften zijn u rekening moet houden. Zie voor een volledige lijst van Azure serviceovereenkomsten, [Serviceovereenkomsten niveau](https://azure.microsoft.com/support/legal/sla/).

###<a name="scalability"></a>Schaalbaarheid

Schaalbaarheid rechtstreeks van invloed op beschikbaarheid. Een toepassing die onder verbeterde laden mislukt is niet langer beschikbaar. Scalable toepassingen kunnen om te voldoen aan grotere vraag met consistente resultaten, in windows aanvaardbaar tijd. Wanneer een systeem scalable is, het horizontaal of verticaal schalen voor het beheren van toename laden tijdens het onderhouden van consistente prestaties. Neer toevoegt horizontale schaalbaarheid meer machines van dezelfde grootte (processor, geheugen en bandbreedte), en verticale schaal vergroot de grootte van de bestaande machines. Voor Azure hebt u verticale schaal opties voor het selecteren van verschillende machine grootte voor berekeningscluster. Wijzigen van de grootte van de computer is een nieuwe implementatie vereist. De meest flexibele oplossingen zijn er daarom ontworpen voor horizontale schaalbaarheid. Dit geldt met name voor berekeningscluster, omdat u kunt het aantal actieve exemplaren van een rol web of werknemer gemakkelijk verhogen. Deze extra exemplaren afhandelen, verbeterde verkeer via de portal van Azure Web, PowerShell-scripts of code. Basiskalender deze beschikking voor de verhoging van specifieke gecontroleerde aan de doelstellingen. In dit scenario al de prestaties van de gebruiker of aan de doelstellingen niet een opvallend decoratieve onder laden. De rollen web en werknemer opslaan meestal extern elke staat. Hierdoor voor flexibele taakverdeling en correcte afhandeling van wijzigingen in exemplaar-tellingen komen. Horizontale schaalbaarheid ook werkt ook met de services, zoals Azure-opslag, waarin geen doorverbonden opties voor verticale schaalbaarheid bieden.

Cloud-implementaties moeten worden beschouwd als een verzameling schaal eenheden. Hiermee kunt de toepassing elastische in de behoeften doorvoer van eindgebruikers onderhoud. De schaaleenheden zijn eenvoudiger kunt visualiseren op het niveau van de server web en -toepassing. Dit is omdat Azure al stateless berekeningscluster knooppunten door middel van web en werknemer rollen biedt. Het toevoegen van dat meer schaal eenheden voor de implementatie berekenen treedt niet toepassing staat management nadelen, omdat berekeningscluster schaal eenheden stateless zijn. Een opslag-eenheid voor tijdschaal is verantwoordelijk voor het beheren van een partition van gegevens (gestructureerde of ongestructureerde). Voorbeelden van schaal opslageenheden zijn Azure tabel partition, Azure Blob container en Azure SQL-Database. Zelfs het gebruik van meerdere accounts van Azure Storage heeft rechtstreeks van invloed op de schaalbaarheid van de toepassing. U moet een zeer scalable cloudservice, kunt u meerdere opslag schaal eenheden ontwerpen. Bijvoorbeeld als u een toepassing relationele gegevens gebruikt, partitioneren u de gegevens over de verschillende SQL-databases. U kunt de opslag te houden met het model van de eenheid voor tijdschaal elastische berekeningscluster. Azure-opslag kunt op dezelfde manier gegevens partitioneren schema waarvoor opzettelijk ontwerpen om te voldoen aan de behoeften doorvoer van de laag berekeningscluster. Zie [Aanbevolen procedures voor het ontwerpen van Large-Scale Services op Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)voor een lijst met aanbevolen procedures voor het ontwerpen van scalable cloudservices.

###<a name="fault-tolerance"></a>Fouttolerantie

Toepassingen ervan uit dat elke mogelijkheid afhankelijke cloud kunt en worden doorgestuurd naar beneden op een gegeven moment in tijd. Een toepassing voor de fouttolerante foutenstructuuranalyse worden gedetecteerd en zetten rond mislukte elementen, te gaan en de juiste resultaat binnen een specifieke periode. Voor tijdelijke fout voorwaarden voldoen, wordt een Foutbestendigheidssysteem een beleid opnieuw gebruiken. Voor meer ernstige fouten, kan de toepassing problemen detecteren en naar alternatieve hardware of voor onvoorziene omstandigheden abonnementen mislukken totdat de fout wordt gecontroleerd. Een betrouwbare toepassing kunt correct beheren de mislukken van een of meer delen en doorgaan goed werken. Foutenstructuuranalyse fouttolerante toepassingen kunnen een of meer ontwerpstrategieën, zoals redundantie, herhaling of anders werken functionaliteit gebruiken.

##<a name="disaster-recovery"></a>Problemen oplossen

Een cloud-implementatie mogelijk niet meer vanwege een systematische storing van de afhankelijke services of de onderliggende infrastructuur functioneren. Een plan voor bedrijfscontinuïteit gebeurtenis onder voorwaarden, wordt het herstelproces noodgevallen. Dit proces betekent meestal zowel bewerkingen personeel en geautomatiseerde procedures om te activeren van de toepassing in een gebied beschikbaar. U moet hiervoor de overdracht van gebruikers van toepassingen, gegevens en services naar de nieuwe regio. Het is ook nu gaat over het gebruik van de back-media of lopende herhaling.

Houd rekening met de vorige overeenkomstige die beschikbaarheid van de mogelijkheid om te herstellen uit een platte band met behulp van een vrije vergeleken. Herstel houdt daarentegen de stappen die u hebt gemaakt na auto vastlopen, waar de auto is niet langer operationele. In dat geval is de beste oplossing om te zoeken van een efficiënte manier om te wijzigen van auto's, door te bellen van een service reizen of een vriend. In dit scenario er is waarschijnlijk het verstandig om een langere vertraging dat terug onderweg. Er is ook meer complexiteit in hersteld en het terugkeren naar de oorspronkelijke voertuig. Herstel naar een andere regio is op dezelfde manier, een complexe taak die betekent meestal dat sommige downtime en kunnen gegevens verloren gaan. Als u wilt beter te begrijpen en strategieën voor noodgevallen herstel evalueren, is het belangrijk om twee voorwaarden definiëren: herstel tijd doelstelling (RTO) en herstelbestanden wijst u doelstelling (vrijgegeven Productieorder).

###<a name="recovery-time-objective"></a>Herstel tijd doelstelling

De RTO is de maximale hoeveelheid tijd die zijn toegewezen aan de toepassingsfunctionaliteit herstellen. Dit is gebaseerd op de zakelijke behoeften en deze zijn gerelateerd aan het belang van de toepassing. Kritieke bedrijfstoepassingen vragen om een laag RTO.

###<a name="recovery-point-objective"></a>Herstel punt doelstelling

De vrijgegeven Productieorder is het aanvaardbaar tijdvenster van gegevensverlies vanwege het herstelproces. Bijvoorbeeld als de vrijgegeven Productieorder een uur is, moet u volledig back-up of de gegevens ten minste eenmaal per uur repliceren. Nadat u de toepassing weer in een andere regio, is het mogelijk dat de back-upgegevens ontbrekende snel aan de uren van gegevens. Kritieke toepassingen afstemmen zoals RTO, een veel kleinere vrijgegeven Productieorder.

##<a name="checklist"></a>Controlelijst

Laten we samenvatting maken van de belangrijkste punten die zijn behandeld in dit artikel (en de verwante artikelen op [beschikbaarheid](./resiliency-high-availability-azure-applications.md) en [herstel](./resiliency-disaster-recovery-azure-applications.md) voor Azure-toepassingen). Dit overzicht fungeert als een controlelijst met items die u rekening voor uw eigen beschikbaarheid en het oplossen van problemen houden moet. Hierna ziet u aanbevolen procedures die handig voor klanten die zijn om ernstige over het implementeren van een goede oplossing.

1. Leiden een risicobeoordeling voor elke toepassing, omdat elk verschillende vereisten hebben kunt. Sommige-toepassingen zijn belangrijker dan anderen en de extra kosten als u wilt ze ontwerpen voor herstel wilt uitvullen.
1. Deze gegevens gebruiken om de RTO en vrijgegeven Productieorder definiëren voor elke toepassing.
1. Ontwerp voor mislukt, beginnend met de toepassingsarchitectuur.
1. Aanbevolen procedures voor hoge beschikbaarheid, terwijl het verdelen van kosten, complexiteit en risico's implementeren.
1. Implementeer noodgevallen herstel-abonnementen en -processen.
  * Houd rekening met fouten die het moduleniveau van de helemaal naar een volledige cloud-storing's beslaan.
  * Back-strategieën voor alle verwijzing en transacties gegevens definiëren.
  * Kies een meerdere site noodgevallen herstel architectuur.
1. Een specifieke eigenaar voor noodgevallen herstelproces, automatisering en testen definiëren. De eigenaar van de moet beheren en bezit bent van het hele proces.
1. De processen van het document zodat ze eenvoudig herhaald. Hoewel er één eigenaar is, moeten meerdere personen kunnen begrijpen en volgt u de processen in noodgevallen.
1. Trainen het personeel het proces uit te voeren.
1. Gebruik reguliere noodgevallen simulaties voor zowel training en validatie van het proces.

##<a name="summary"></a>Overzicht

Wanneer hardware of toepassingen niet binnen Azure, zijn de technieken en strategieën voor het beheren van deze anders dan het wanneer fout op systemen voor on-premises implementatie optreedt. De belangrijkste reden hiervoor is dat cloud oplossingen meestal meer afhankelijkheden op infrastructuur dat wordt verdeeld over een Azure regio en beheerd als afzonderlijke services. U moet hebben betrekking op gedeeltelijke fouten met hoge beschikbaarheid-technieken. Als u wilt beheren ernstige fouten, mogelijk vanwege een gebeurtenis noodgevallen, gebruikt u strategieën voor noodgevallen.

Azure worden gedetecteerd en veel fouten worden verwerkt, maar er zijn tal van fouten die opgevolgd toepassingsspecifieke strategieën moeten. U moet actief voorbereiden en de fouten van toepassingen, services en gegevens beheren.

Bij het maken van uw toepassing beschikbaarheid en gaan, kunt u de gevolgen van het bedrijf van van de toepassing niet. Bepalen welke processen, beleidsregels en procedures voor het herstellen van kritieke systemen nadat een fatale gebeurtenis duurt, planning en resourcebeperkingen. En als u de abonnementen hebt ingesteld, u er niet stoppen. U moet regelmatig analyseren, testen en voortdurend verbeteren de abonnementen op basis van uw toepassing portfolio, zakelijke behoeften en de technologieën die beschikbaar zijn voor u. Azure nieuwe mogelijkheden biedt en verheft nieuwe uitdagingen voor het maken van krachtige toepassingen die fouten berekend.

##<a name="additional-resources"></a>Aanvullende informatie

[De beschikbaarheid voor toepassingen gebaseerd op Microsoft Azure](resiliency-high-availability-azure-applications.md)

[Herstel voor toepassingen gebaseerd op Microsoft Azure](resiliency-disaster-recovery-azure-applications.md)

[Technische ondersteuning van Azure tolerantie](resiliency-technical-guidance.md)

[Overzicht: Cloud bedrijven herstel bedrijfscontinuïteit en een database met SQL-Database](../sql-database/sql-database-business-continuity.md)

[Hoge beschikbaarheid herstel voor SQL Server in Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)

[Failsafe: Richtlijnen voor robuuste cloud architecturen](https://channel9.msdn.com/Series/FailSafe)

[Aanbevolen procedures voor het ontwerp van grootschalige services op Azure-Cloudservices](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)

##<a name="next-steps"></a>Volgende stappen

In dit artikel maakt deel uit van een reeks artikelen die zijn gericht op herstel en beschikbaarheid van Azure-toepassingen. Het volgende artikel in deze reeks is [beschikbaarheid voor toepassingen gebaseerd op Microsoft Azure](resiliency-high-availability-azure-applications.md).
