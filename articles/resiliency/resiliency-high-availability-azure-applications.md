<properties
   pageTitle="Beschikbaarheid van Azure toepassingen | Microsoft Azure"
   description="Technisch overzicht en gedetailleerde informatie over het ontwerpen en bouwen van toepassingen voor maximale beschikbaarheid op Microsoft Azure."
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

#<a name="high-availability-for-applications-built-on-microsoft-azure"></a>De beschikbaarheid voor toepassingen gebaseerd op Microsoft Azure

Een toepassing voor maximaal beschikbare absorbeert veranderingen in beschikbaarheid, laden en tijdelijke fouten in de afhankelijke services en hardware. De toepassing blijft werken met een aanvaardbaar gebruiker en systematische antwoord niveau, zoals gedefinieerd door bedrijven vereisten of toepassing serviceovereenkomsten (Sla's).

##<a name="azure-high-availability-features"></a>Functies voor Azure hoge beschikbaarheid

Azure heeft vele ingebouwde platform-functies die ondersteuning bieden voor maximaal beschikbare toepassingen. In deze sectie worden enkele van de belangrijkste functies beschreven. Zie [Azure tolerantie technische richtlijnen](./resiliency-technical-guidance.md)voor een meer uitgebreide analyse van het platform.

###<a name="fabric-controller"></a>Stof controller

De controller Azure stof bepalingen en bewaakt de voorwaarde van de exemplaren Azure berekeningscluster. De controller stof controleert de status van de hardware en software die van de exemplaren van de computer host en als gast. Wanneer wordt vastgesteld dat een fout, worden automatisch verplaatst de exemplaren VM serviceovereenkomsten toegepast. Het concept van het probleem- en upgrade domeinen verder ondersteunt de SLA berekeningscluster.

Wanneer meerdere exemplaren van de Cloudservice rol zijn geïmplementeerd, implementeert Azure deze exemplaren naar verschillende foutenstructuuranalyse domeinen. De rand van een foutenstructuuranalyse domein is in feite een verschillende hardware rek in dezelfde regio. Foutenstructuuranalyse domeinen Beperk de kans dat een hardwarefout gelokaliseerde de service van een toepassing wordt onderbroken. U kunt het aantal foutenstructuuranalyse-domeinen die zijn toegewezen aan de werknemer of web rollen niet beheren. De controller stof toegewezen resources die gescheiden van Azure gehoste toepassingen zijn gebruikt. Beschikbaarheid van 100 procent heeft omdat deze als de kern van het Azure-systeem fungeert. Deze worden gecontroleerd en beheerd rol exemplaren voor foutenstructuuranalyse domeinen.

Het volgende diagram ziet Azure gedeelde resources dat de controller stof wordt geïmplementeerd en in verschillende foutenstructuuranalyse domeinen beheert.

![Vereenvoudigde weergave van foutenstructuuranalyse domein moeten worden geïsoleerd](./media/resiliency-high-availability-azure-applications/fault-domain-isolation.png)

De upgrade domeinen zijn vergelijkbaar met foutenstructuuranalyse domeinen in de functie, maar ze upgrades in plaats van fouten ondersteunen. Een upgrade-domein is een logische eenheid exemplaar scheiding die bepaalt welke exemplaren in een bepaalde service bijgewerkt op een punt in tijd. De standaardinstelling voor de implementatie gehoste service zijn vijf upgrade domeinen gedefinieerd. U kunt echter die waarde in de service-definitiebestand wijzigen. Stel dat er acht exemplaren van de Webrol van uw. Er zijn twee instanties in drie upgrade domeinen en twee instanties in één upgrade domein. Azure bepaalt de volgorde van de update, maar deze gebaseerd op het nummer van de upgrade domeinen. Zie [een cloudservice bijwerken](../cloud-services/cloud-services-update-azure-service.md)voor meer informatie over de upgrade domeinen.

###<a name="features-in-other-services"></a>Functies in andere services

Naast de platform-functies die ondersteuning bieden voor hoge berekeningscluster beschikbaarheid, sluit Azure functies voor hoge beschikbaarheid in de andere services. Azure Storage heeft bijvoorbeeld drie kopieën van alle blob, tabel en wachtrijgegevens. Daarnaast kunt u de optie van geografische herhaling voor de opslag van back-ups van BLOB's en tabellen in een tweede regio. De bezorging van Azure inhoud netwerk kunt BLOB's in de cache opgeslagen overal ter wereld voor zowel redundantie en schaalbaarheid. Azure SQL-Database onderhoudt ook meerdere replica's.

Naast de [technische ondersteuning voor tolerantie](https://aka.ms/bctechguide) reeks artikelen, raadpleegt u het document [Aanbevolen procedures voor het ontwerpen van Large-Scale Services op Azure-Cloudservices](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/) . In deze artikelen vindt u een grondigere discussie van de functies van de beschikbaarheid van Azure platform.

Hoewel Azure meerdere functies die ondersteuning bieden voor beschikbaarheid biedt, is het belangrijk om te begrijpen hun beperkingen:

- Voor berekeningscluster garandeert Azure dat uw rollen beschikbaar en worden uitgevoerd zijn, maar deze niet weet of uw toepassing actief of overbelasting is.
- Voor Azure SQL-Database gegevens synchroon gerepliceerd in het gebied. Actieve geografische herhaling, waarmee maximaal vier extra kopieën in het hetzelfde land (of verschillende regio's), kunt u kiezen. Deze databasereplica zijn niet point-in-time-back-ups. SQL-databases bieden mogelijkheden voor back-punt-in-tijd. Lees meer informatie over de mogelijkheden van de SQL-Database point-in-time [Azure SQL Database punt in tijd herstellen](https://azure.microsoft.com/blog/azure-sql-database-point-in-time-restore/).
- Voor de opslag van Azure, wordt de tabel en de blob al dan niet standaard naar een andere regio gerepliceerd. Echter u geen toegang tot de replica's totdat Microsoft wil naar de site alternatieve mislukken. Een storing regio alleen voor een langdurige regio hele service-onderbreking en er is geen SLA voor geografische-failover-tijd. Het is ook Houd er rekening mee dat een beschadigde gegevens snel dubbele met de replica's.

Daarom moet u de functies van de beschikbaarheid van platform met functies met toepassingsspecifieke beschikbaarheid aanvullen. Toepassingsspecifieke beschikbaarheid bevatten de functie blob momentopname als u wilt maken van back-ups punt-in-tijd van blob-gegevens.

###<a name="availability-sets-for-azure-virtual-machines"></a>Beschikbaarheid wordt ingesteld voor Azure virtuele Machines

De meeste van dit artikel ligt de nadruk op cloudservices, die een platform als een servicemodel (PaaS) gebruiken. Er zijn echter ook de van de specifieke beschikbaarheidsfuncties voor Azure virtuele Machines, waarbij een infrastructuur als servicemodel (IaaS). Als u wilt bereiken beschikbaarheid met virtuele Machines, moet u beschikbaarheid sets gebruiken. Een set beschikbaarheid fungeert een soortgelijke functie naar foutenstructuuranalyse en upgrade domeinen. Azure Hiermee binnen een set beschikbaarheid plaatst u de virtuele machines op een manier die voorkomt u dat gelokaliseerde hardwarefouten en onderhoudsactiviteiten brengen alle computers in die groep. Beschikbaarheid sets moeten zijn de SLA Azure voor de beschikbaarheid van virtuele Machines bereiken.

In het volgende diagram vindt u een weergave van twee sets van de beschikbaarheid van die groep web en SQL Server virtuele machines respectievelijk.

![Beschikbaarheid wordt ingesteld voor Azure virtuele Machines](./media/resiliency-high-availability-azure-applications/availability-set-for-azure-virtual-machines.png)

>[AZURE.NOTE] Klik in het diagram is SQL Server geïnstalleerd en wordt uitgevoerd op virtuele machines. Dit is het verschil tussen de vorige discussie van Azure SQL-Database, waarmee een database als een beheerde service.

##<a name="application-strategies-for-high-availability"></a>Toepassing strategieën voor het beschikbaarheid

De meeste toepassing strategieën voor het beschikbaarheid gebruikmaakt van redundantie of het verwijderen van de harde afhankelijkheden tussen toepassingsonderdelen. Het ontwerp van toepassing moet fouttolerantie ondersteunen tijdens enkel geval downtime van Azure of externe services. De volgende secties worden toepassing patronen voor het vergroten van de beschikbaarheid van uw cloudservices.

###<a name="asynchronous-communication-and-durable-queues"></a>Asynchrone communicatie en duurzame wachtrijen

Houd rekening met asynchrone communicatie tussen losse services om beschikbaarheid in Azure-toepassingen te vergroten. In dit patroon, berichten naar u opslag of Azure Service Bus wachtrijen voor het verwerken van later te schrijven. Wanneer u het bericht naar de wachtrij schrijft, keert onmiddellijk terug naar de afzender van het bericht. Een andere laag van de toepassing is verantwoordelijk voor het bericht processing, meestal geïmplementeerd als een rol werknemer. Als de rol werknemer uitvalt, is de berichten worden verzameld in de wachtrij totdat de verwerkingsservice is hersteld. Zo lang maken als de wachtrij beschikbaar is, is er geen directe afhankelijkheid tussen de front afzender en de bericht-processor. Hierdoor wordt de vereiste voor synchroon serviceaanvragen die een doorvoer verschil in gedistribueerde toepassingen kunnen zijn.

Een variatie van deze gebruikt Azure Storage (BLOB's, tabellen, wachtrijen) of Service Bus wachtrijen als een locatie failover voor mislukte database gesprekken. Een synchroon gesprek binnen een toepassing naar een andere service (zoals Azure SQL-Database) mislukt bijvoorbeeld herhaaldelijk drukken. U kunt mogelijk serialiseren die gegevens in duurzame opslag. Op een gegeven moment later wanneer de service of de database weer online, is versturen de toepassing opnieuw het verzoek van opslag. Het verschil in dit model is dat de tussenliggende locatie niet een constant deel van de werkstroom toepassing uitmaakt. Dit wordt alleen gebruikt in scenario's is mislukt.

In beide gevallen wordt voorkomen asynchrone communicatie en tussentijdse opslag dat een uitgevallen back-enddatabase-service waarmee u de hele toepassing. Wachtrijen fungeren als logische tussenpersoon. Zie voor meer hulp bij het kiezen van de juiste wachtrij plaatsen service, [Azure wachtrijen en Azure Service Bus wachtrijen--vergeleken en in afgezet tegen](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

###<a name="fault-detection-and-retry-logic"></a>De logica detectie en probeer het opnieuw foutenstructuuranalyse

Een belangrijk punt in het toepassingsontwerp van maximaal beschikbare is opnieuw logica code gebruiken om u te worden verwerkt een service die is tijdelijk niet beschikbaar. De [Tijdelijke foutenstructuuranalyse afhandelen toepassing blokkeren](https://msdn.microsoft.com/library/hh680934.aspx), ontwikkeld door het team van Microsoft Patterns en procedures, helpt softwareontwikkelaars in dit proces. Het woord 'tijdelijk' betekent dat er een tijdelijke voorwaarde die slechts voor een relatief korte tijd duurt. In de context van dit artikel, met de verwerking van tijdelijke fouten maakt deel uit van de ontwikkeling van een toepassing voor maximaal beschikbaar. Voorbeelden van tijdelijke situaties zijn netwerk af en toe fouten en databaseverbindingen verloren.

De tijdelijke foutenstructuuranalyse afhandelen toepassing blokkeren is een eenvoudigere manier u fouten in uw code op correcte verwerken. U kunt deze de beschikbaarheid van uw toepassingen verbeteren door toe te voegen robuuste tijdelijke foutenstructuuranalyse foutafhandeling logica. In de meeste gevallen opnieuw logica omgaat met de korte onderbreking en opnieuw verbinding maakt de afzender en de ontvanger na een of meer mislukte pogingen. Een succesvolle nieuwe pogingen gaat meestal onopgemerkte naar gebruikers van toepassingen.

Ontwikkelaars hebben drie opties voor het beheren van hun opnieuw logica: incrementele, vaste interval, en exponentiële. Incrementele wachten op opnieuw meer voordat elk op toeneemt lineaire wijze (bijvoorbeeld 1, 2, 3 en 4 seconden). Vast interval wacht de dezelfde tijdsduur tussen nieuwe pogingen (bijvoorbeeld 2 seconden ingedrukt). Voor een meer willekeurige optie wacht de exponentiële back-uitschakelen meer tussen nieuwe pogingen. Deze gebruikt echter exponentiële gedrag (bijvoorbeeld 2, 4, 8 en 16 seconden).

De strategie op hoog niveau binnen uw code luidt als volgt:

1. Definieer uw strategie opnieuw en beleid.
1. Voer de bewerking die leiden een tijdelijke veroorzaakt tot kan.
1. Als tijdelijke fout zich voordoet, roepen het beleid opnieuw.
1. Als alle pogingen mislukt, variabel een definitief uitzondering.

Test uw logica opnieuw in gesimuleerd fouten om ervoor te zorgen dat nieuwe pogingen op opeenvolgende bewerkingen niet in een onverwachte lange vertraging resulteren. Dit doen voordat u besluit de algehele taak mislukt.

###<a name="reference-data-pattern-for-high-availability"></a>Verwijzing gegevenspatroon voor maximale beschikbaarheid

Referentiegegevens is het alleen-lezen gegevens van een toepassing. Deze gegevens biedt de business-context waarbinnen de toepassing transacties gegevens in de loop van een zakelijke bewerking genereert. Transacties gegevens is een functie punt-in-tijd van de verwijzingsgegevens. De integriteit is dus, afhankelijk van de momentopname van de verwijzingsgegevens op het moment van de transactie. Dit is een enigszins losse definitie, maar het moet voldoende voor ons doel hier.

Verwijzingsgegevens in de context van een toepassing is voor de werking van de toepassing nodig. De desbetreffende toepassingen maken en onderhouden van referentiegegevens; stamgegevens management (MDM) systemen uitvoeren vaak deze functie. Deze systemen zijn die verantwoordelijk is voor de levenscyclus van de verwijzingsgegevens. Voorbeelden van referentiegegevens zijn productcatalogus, werknemer outmodel onderdelen outmodel en apparatuur basispagina. Referentiegegevens ook afkomstig zijn van buiten uw organisatie, zoals postcodes of btw-tarieven. Strategieën voor verbetering van de beschikbaarheid van referentiegegevens zijn meestal minder moeilijk zijn dan die voor transacties gegevens. Referentiegegevens heeft het voordeel van voornamelijk onveranderlijke.

U kunt aanbrengen Azure web en werknemer rollen die verwijzingsgegevens zelfstandige gedurende runtime opnemen door de verwijzingsgegevens samen met de toepassing implementeren. Als de grootte van de lokale opslag zoals een implementatie toestaat, is dit een ideaal staat. Ingesloten databases (SQL, NoSQL) of XML-bestanden die zijn geïmplementeerd in een lokaal bestandssysteem helpen met de autonomie van Azure berekeningscluster schaaleenheden. U moet wel een om de gegevens in elke rol bijwerken zonder opnieuw installeren. Klik hiertoe plaats updates van de verwijzingsgegevens naar een cloud opslag eindpunt (bijvoorbeeld Azure-blobopslag of SQL-Database). Code toevoegen aan elke rol die de Gegevensupdates in de knooppunten berekeningscluster bij het opstarten van de rol is gedownload. U kunt ook code waarmee beheerders om uit te voeren van het downloaden van een afgedwongen in de rol exemplaren toevoegen.

Als u wilt vergroten beschikbaarheid, moeten de rollen ook een reeks referentiegegevens bevatten geval opslagruimte is niet beschikbaar. Hiermee worden de rollen om te beginnen met een eenvoudige set referentiegegevens totdat de resource opslagruimte beschikbaar voor de updates.

![De beschikbaarheid voor toepassingen via de zelfstandige berekeningscluster knooppunten](./media/resiliency-high-availability-azure-applications/application-high-availability-through-autonomous-compute-nodes.png)

Een overweging voor dit patroon is de implementatie- en opstartgedrag snelheid voor uw rollen. Als u implementeren of downloaden van grote hoeveelheden verwijzingsgegevens bij het opstarten, kan dit de hoeveelheid tijd die het duurt draaien van nieuwe implementaties of rol exemplaren vergroten. Dit kan een aanvaardbaar verhouding voor de autonomie van dat de verwijzingsgegevens onmiddellijk op elke rol in plaats van externe opslagservices afhankelijk zijn.

###<a name="transactional-data-pattern-for-high-availability"></a>Transacties gegevenspatroon voor maximale beschikbaarheid

Transacties gegevens zijn de gegevens die in een context bedrijven door de toepassing kan worden gegenereerd. Transacties gegevens is een combinatie van de set bedrijfsprocessen waarmee de toepassing wordt geïmplementeerd en de referentiegegevens die ondersteuning biedt voor deze processen. Voorbeelden van transacties gegevens kunnen opnemen orders, geavanceerde verzending kennisgevingen, facturen en klanten relatie management (CRM) verkoopkansen. De transacties gegevens dus gegenereerd wordt worden ingevoerd in externe systemen voor overzicht bijhouden of voor nadere verwerking.

Houd rekening met die verwijzing gegevens binnen de systemen die verantwoordelijk voor deze gegevens zijn kunt wijzigen. Daarom moet transacties gegevens de context van de gegevens point-in-time verwijzing opslaan zodat er minimale externe afhankelijkheden voor de semantische consistentie. Stel het verwijderen van een product van de catalogus met een paar maanden na de orde is voldaan. De beste manier is om de context van zoveel verwijzing gegevens als haalbaar insluiten in de transactie. Hierdoor blijft de semantiek die is gekoppeld aan de transactie, zelfs als de verwijzingsgegevens worden gewijzigd nadat de transactie wordt vastgelegd.

Zoals eerder is vermeld, uitlenen architecturen die gebruikmaken van losse koppeling en asynchrone communicatie zelf aan hogere niveaus van de beschikbaarheid van. Dit geldt voor transacties gegevens ook, maar de implementatie is complexere. Traditionele transacties begrippen is meestal afhankelijk van de database voor de transactie. Wanneer u een tussenliggende lagen introduceren, moet de gegevens van de verschillende lagen om ervoor te zorgen voldoende consistentie en levensduur correct omgaan met de toepassingscode.

Een werkstroom waarmee de opname van transacties gegevens van de verwerking gescheiden wordt beschreven in de volgende volgorde:

1. Web berekeningscluster knooppunt: presenteren verwijzen naar gegevens.
1. Externe opslag: tussenliggende transacties gegevens opslaan.
1. Web berekeningscluster knooppunt: de transactie eindgebruikers te voltooien.
1. Web berekeningscluster knooppunt: de voltooide transacties gegevens, samen met de context van de gegevens verwijzing naar duurzame tijdelijke die gegarandeerd geven een overzichtelijk antwoord verzenden.
1. Web berekeningscluster knooppunt: signaal van de eindgebruiker voltooiing van de transactie.
1. Achtergrond berekenen knooppunt: de transacties gegevens hebt geëxtraheerd, verwerken na deze zo nodig en deze verzenden naar de laatste opslaglocatie in het huidige systeem.

In het volgende diagram ziet u een mogelijke uitvoering van deze ontwerpen in een gehoste Azure-cloudservice.

![Beschikbaarheid via losse koppeling](./media/resiliency-disaster-recovery-high-availability-azure-applications/application-high-availability-through-loose-coupling.png)

De onderbroken pijlen in het diagram aangeven asynchrone verwerking. De front-Webrol is niet op de hoogte van deze asynchrone verwerking. Dit leidt tot de opslag van de transactie op de definitieve bestemming ten opzichte van het huidige systeem. Vanwege de latentie die maakt u kennis met dit asynchroon model, is de transacties gegevens niet onmiddellijk beschikbaar voor de query. Daarom elke eenheid van de transacties gegevens moet worden opgeslagen in een cache of een gebruikerssessie om te voldoen aan de onmiddellijke gebruikersinterface nodig heeft.

De Webrol van de is zelfstandige van de rest van de infrastructuur. De van beschikbaarheidsprofiel is een combinatie van de Webrol en de Azure wachtrij en niet de hele infrastructuur. Naast het beschikbaarheid kan deze methode de Webrol aan de nieuwe schaal horizontaal, onafhankelijk van de back-end-opslag. Dit model hoge beschikbaarheid kunt meteen invloed hebben op de kosten van bewerkingen. Extra onderdelen zoals Azure wachtrijen en werknemer rollen invloed kunnen zijn op maandelijkse gebruikskosten.

Houd er rekening mee dat het vorige diagram ziet u één uitvoering van deze methode losgekoppelde met transacties gegevens. Zijn er veel andere mogelijke implementaties. De volgende lijst bevat enkele alternatieven:

 * De rol van een werknemer mogelijk tussen de Webrol- en de wachtrij opslag worden geplaatst.
 * Een Service Bus wachtrij kan worden gebruikt in plaats van een wachtrij Azure Storage.
 * De uiteindelijke bestemming mogelijk Azure Storage of een andere database-provider.
 * Azure Cache kan worden gebruikt op de weblaag op te geven in de cache vereisten na de transactie.

###<a name="scalability-patterns"></a>Schaalbaarheid patronen

Het is belangrijk te weten dat de schaalbaarheid van de cloudservice rechtstreeks van invloed is op beschikbaarheid. Als verbeterde laden uw service veroorzaakt niet meer reageert, is de gebruiker indruk dat de toepassing is niet beschikbaar. Volg de aanbevolen procedures voor het schaalbaarheid op basis van de verwachte toepassing laden en toekomstige verwachtingen. De hoogste schaal heeft betrekking op veel overwegingen, zoals het gebruik van één versus meerdere opslag-accounts delen met meerdere databases, en caching van strategieën. Zie [Aanbevolen procedures voor het ontwerpen van Large-Scale Services op Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)voor een uitgebreide deze patronen wilt bekijken.

##<a name="next-steps"></a>Volgende stappen

In dit artikel maakt deel uit van een reeks artikelen die zijn gericht op [herstel en betere beschikbaarheid voor toepassingen gebaseerd op Microsoft Azure](./resiliency-disaster-recovery-high-availability-azure-applications.md). Het volgende artikel in deze reeks is [herstel voor toepassingen gebaseerd op Microsoft Azure](./resiliency-disaster-recovery-azure-applications.md).
