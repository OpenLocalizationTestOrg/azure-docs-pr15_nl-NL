<properties 
   pageTitle="Cloud ontwerpoplossingen voor herstel met SQL-Database geografische-replicatie | Microsoft Azure"
   description="Informatie over het ontwerpen van uw oplossing cloud voor herstel door het juiste failover patroon te kiezen."
   services="sql-database"
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA" 
   ms.date="07/16/2016"
   ms.author="sashan"/>

# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pool"></a>Strategieën voor noodgevallen voor toepassingen met SQL-Database elastische toepassingen 

Hebt u geleerd cloudservices zijn niet betrouwbare en ernstige incidenten jaar kunt en er gebeurt. SQL-Database biedt een aantal mogelijkheden verstrekken voor bedrijfscontinuïteit van uw toepassing wanneer deze incidenten plaatsvinden. [Elastische pools](sql-database-elastic-pool.md) en zelfstandige databases ondersteuning voor hetzelfde soort mogelijkheden voor noodgevallen. In dit artikel worden de verschillende DR strategieën voor elastische die groepen gebruikmaken van deze voorzieningen bedrijfscontinuïteit SQL-Database.

We gebruiken de canonieke SaaS ISV toepassing patroon voor de toepassing van dit artikel:

<i>Een modern cloud op basis van web application bepalingen één SQL-database voor elke gebruiker. Website heeft een groot aantal klanten en daarom gebruikt veel databases, bekend als tenant-databases. Omdat de tenant-databases meestal onverwachte activiteit patronen hebt, website een elastische toepassingen gebruikt de database te maken van kosten zeer overzichtelijk via langere tijd. De prestatiebeheer eenvoudiger de elastische toepassingen ook wanneer de gebruikersactiviteit bereikt. Naast de tenant-databases de toepassing ook verschillende databases gebruikt om te beheren van gebruikersprofielen, beveiliging, verzamelen gebruikspatronen, enzovoort. Beschikbaarheid van de afzonderlijke tenants heeft geen gevolgen voor de beschikbaarheid van de toepassing als geheel. De beschikbaarheid en de prestaties van management databases kritieke voor de functie van de toepassing is echter en als het management-databases offline zijn de hele toepassing is offline.</i>  

In de rest van het papier bespreken we de DR-strategieën die betrekking hebben op een bereik van scenario's van de kosten gevoelige opstarten-toepassingen naar de kleuren die met strikte beschikbaarheid vereisten.  

## <a name="scenario-1-cost-sensitive-startup"></a>Scenario 1. Kosten gevoelige opstarten

<i>Ik ben een bedrijf opstarten en ben zeer gevoelige kosten.  Ik wil eenvoudiger implementatie en beheer van de toepassing en ik wil hebben een beperkt SLA voor afzonderlijke klanten. Maar ik wil zorgen voor de toepassing als geheel nooit offline is.</i>

Om te voldoen aan de vereiste eenvoudig, moet u alle tenant databases implementeren in één elastische toepassingen in de Azure regio van uw keuze en implementeren van de database (s) management als zelfstandige geografische-gerepliceerde database (s). Voor het herstelproces is noodgevallen van tenants, gebruikt u geografische-herstellen, die u kunt zonder extra kosten kiezen. De beschikbaarheid van de management-databases, zodat moeten deze worden geografische gerepliceerd naar een andere regio (stap 1). De lopende kosten van de configuratie van de herstel noodgevallen in dit scenario is gelijk aan de totale kosten van de secundaire database (s). Deze configuratie wordt geïllustreerd in het volgende diagram.

![Afbeelding 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

Met het volgende diagram worden de stappen om uw toepassing online weer te geven voor het geval een storing in de primaire regio geïllustreerd.

- Direct databases failover het beheer (2) de DR regio. 
- Wijzigen de verbindingsreeks van de toepassing te laten verwijzen naar het gebied DR. Alle nieuwe accounts en tenant databases worden gemaakt in de regio DR. De bestaande klanten zien hun gegevens tijdelijk niet beschikbaar is.
- U kunt de elastische toepassingen maken met dezelfde configuratie als de oorspronkelijke groep (3). 
- Gebruik geografische herstellen kopieën van de tenant om databases te maken (4). U kunt Houd rekening met de afzonderlijke Hiermee herstelt door de verbindingen eindgebruikers activeert of gebruiken van enkele andere toepassing specifieke prioriteit kleurenschema.

Nu uw toepassing in het gebied DR weer online is, maar sommige klanten ervaart vertraging bij het openen van hun gegevens.

![Afbeelding 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Als de onderbreking tijdelijk is, is het mogelijk dat de primaire regio wordt worden hersteld door Azure voordat alle Hiermee herstelt voltooid in de regio DR zijn. In dit geval moet u goedkeuringen verplaatsen van de toepassing terug naar de primaire regio. De stappen in het volgende diagram wordt geïllustreerd, worden in beslag nemen.
 
- Alle uitstaande geografische-herstellen aanvragen annuleren.   
- Failover de management database (s) de primaire regio (5). Opmerking: Na herstel van het gebied de oude primaire automatisch geworden secundaire servers. Nu wordt overgeschakeld rollen opnieuw. 
- Wijzigen de verbindingsreeks van de toepassing te laten verwijzen naar de primaire regio. Nu worden alle nieuwe accounts en tenant databases gemaakt in de primaire regio. Sommige bestaande klanten zien hun gegevens tijdelijk niet beschikbaar is.   
- Stel alle databases in de groep DR op alleen-lezen zodat ze kunnen niet worden gewijzigd in de regio DR (6). 
- Voor elke database in de DR-toepassingen die zijn gewijzigd sinds het herstelproces is, wijzig of verwijder de bijbehorende databases in de primaire groep (7). 
- Kopieer de bijgewerkte databases uit de groep DR aan de primaire groep (8). 
- Verwijderen van de DR-toepassingen (9)

Nu is uw toepassing online in de primaire regio met alle tenant databases beschikbaar in de primaire groep.

![Afbeelding 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

De belangrijkste **voordelen van uw** van deze strategie is laag lopende kosten voor overbodige laag. Back-ups worden automatisch opgehaald door de service SQL-Database met geen toepassing voor revisie en zonder extra kosten.  De kosten worden alleen wanneer de elastische databases worden hersteld. De **verhouding** is dat het herstelproces is voltooid van de tenant-databases aanzienlijk tijd duurt. Dit is afhankelijk van het totaal aantal Hiermee herstelt u in de regio DR initiëren en totale grootte van de tenant-databases. Zelfs als u de sommige tenants Hiermee herstelt boven andere prioriteit, wordt u u dat met alle andere Hiermee herstelt die in dezelfde regio worden gestart, terwijl de service wordt bemiddelen en om de algehele invloed op de bestaande klanten databases zoveel mogelijk te beperken. Bovendien kan het herstelproces is van de tenant-databases niet starten totdat de nieuwe elastische toepassingen in de regio DR wordt gemaakt.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Scenario 2. Volwaardige toepassing met doorverbonden-service 

<i>Ik ben een volwaardige SaaS-toepassing met doorverbonden service aanbiedingen en andere serviceovereenkomsten voor proefabonnement klanten en voor de betaling klanten. Voor klanten met het proefabonnement moet ik doen om de zoveel mogelijk te beperken. Proefabonnement klanten downtime kunnen uitvoeren, maar ik wil de kans. Voor klanten met de betaling is een downtime een flight risico. Ik wil om ervoor te zorgen dat betaalt kunnen klanten altijd toegang tot hun gegevens.</i> 

Voor dit scenario, moet u het proefabonnement tenants van betaalde tenants scheiden door deze te plaatsen in afzonderlijke elastische pools. Het proefabonnement klanten moeten lagere eDTU per tenant en lagere SLA met herstel langer. De betaling klanten worden in een groep met hogere eDTU per tenant en een hogere SLA. Om te garanderen de laagste hersteltijd, moet de vaste klanten tenant databases geografische gerepliceerd. Deze configuratie wordt geïllustreerd in het volgende diagram. 

![Afbeelding 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Zoals in het eerste scenario, een of meer management databases helemaal actief zijn zodat u een zelfstandige geografische-gerepliceerde database voor (1 gebruiken). Dit zorgt ervoor dat de overzichtelijk prestaties voor nieuwe klant-abonnementen, profielupdates en andere beheertaken uit te voeren. Het gebied waarin de primaire kleuren van een of meer management databases bevinden is de primaire regio en wordt de regio waarin het ontvangen van een of meer management databases bevinden de DR regio.

De betaling klanten tenant databases heeft actieve databases in de "betaalde" toepassingen deze is ingericht in de primaire regio. U moet een secundaire resourcegroep met dezelfde naam in de regio DR inrichten. Elke tenant zou geografische gerepliceerd naar de secundaire groep (2). Hiermee schakelt u een snelle herstel van de tenant-databases met failover. 

Als er een storing optreedt in de primaire regio, worden de stappen om uw toepassing online weer te geven in het volgende diagram geïllustreerd:

![Afbeelding 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

- Mislukken meteen via het beheer van database (s) de DR regio (3).
- Wijzig de verbindingsreeks van de toepassing te laten verwijzen naar het gebied DR. Nu worden alle nieuwe accounts en tenant databases gemaakt in de regio DR. De bestaande proefabonnement klanten zien hun gegevens tijdelijk niet beschikbaar is.
- Failover de betaalde tenant-databases aan de groep in het gebied DR direct herstel hun beschikbaarheid (4). Aangezien de overname een snelle metagegevens niveau wijziging die kunt u overwegen een optimalisatie waar de afzonderlijke failovers worden geactiveerd op aanvraag door de eindgebruiker verbindingen is. 
- Als uw secundaire groep eDTU kleiner dan de primaire is omdat de secundaire databases alleen de capaciteit vereist om de logboeken wijzigen terwijl ze ontvangen zijn, moet u direct de capaciteit van toepassingen nu zijn voor de volledige werklast van alle tenants (5) verhogen. 
- Maak de nieuwe elastische toepassingen met dezelfde naam en dezelfde configuratie in de regio DR voor het proefabonnement klanten databases (6). 
- Nadat het proefabonnement klanten van toepassingen is gemaakt, gebruikt u geografische herstellen naar de afzonderlijke proefversietenant-databases herstellen naar de nieuwe groep (7). U kunt Houd rekening met de afzonderlijke Hiermee herstelt door de verbindingen eindgebruikers activeert of gebruiken van enkele andere toepassing specifieke prioriteit kleurenschema.

De toepassing is op dit moment weer online in de regio DR. Alle vaste klanten hebben toegang tot hun gegevens terwijl het proefabonnement klanten vertraging ondervinden wordt bij het openen van hun gegevens.

Als de primaire regio is hersteld door Azure *nadat* u de toepassing in de DR regio die u kunt doorgaan met het uitvoeren van de toepassing in dat gebied hebt teruggezet of u kunt beslissen mislukt terug naar de primaire regio. Als de primaire regio hersteld *voordat is* is het failover voltooid, moet u rekening houden bij ontbreken terug direct af. De failback duurt de stappen in het volgende diagram wordt geïllustreerd: 
 
![Afbeelding 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

- Alle uitstaande geografische-herstellen aanvragen annuleren.   
- Failover de database (s) management (8). De oude primaire na herstel van het gebied waren automatisch de secundaire geworden. Nu wordt de primaire opnieuw.  
- Failover de betaalde tenant databases (9). Daarnaast na herstel van het gebied de oude primaire automatisch worden de secundaire servers. Nu worden ze opnieuw de primaire kleuren. 
- Stel de herstelde proefabonnement databases die zijn gewijzigd in de regio DR in alleen-lezen (10).
- Voor elke database in de proefversie klanten DR-toepassingen die sinds het herstelproces is gewijzigd, wijzig of verwijder de bijbehorende database in het proefabonnement klanten primaire groep (11). 
- Kopieer de bijgewerkte databases uit de groep DR aan de primaire groep (12). 
- Verwijderen van de DR-toepassingen (13) 

> [AZURE.NOTE] De bewerking failover is asynchroon. Minimaliseren het herstelproces is het belangrijk dat u de tenant-databases failover-opdracht in batches van ten minste 20 databases uitvoeren. 

De belangrijkste **voordelen van uw** van deze strategie is dat u de hoogste SLA voor de betaling klanten. Ook zorgt u ervoor dat de nieuwe experimenten opgeheven wordt zodra de proefabonnement DR-toepassingen is gemaakt. De **verhouding** is dat deze instelling de totale kosten van de tenant-databases door de kosten van de secundaire DR-toepassingen voor betaalde klanten neemt. Bovendien als de secundaire groep een andere grootte heeft, de vaste klanten wordt de prestaties lagere na failover totdat de upgrade van toepassingen in de regio DR is voltooid. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Scenario 3. Geografisch gedistribueerde toepassing met doorverbonden-service

<i>Ik heb een volwaardige SaaS-toepassing met doorverbonden service voorstellen. Ik wil een scherpe SLA bieden aan mijn betaalde klanten en de kans op pesterijen impact minimaliseren wanneer bijvoorbeeld optreden omdat even kort onderbroken leiden klant ontevredenheid tot kan. Het is belangrijk dat de betaling klanten altijd toegang hun gegevens tot. De experimenten zijn gratis en een SLA wordt niet tijdens de proefperiode aangeboden.</i> 

Voor dit scenario, moet u drie afzonderlijke elastische pools hebben. Twee gelijke grootte van toepassingen met hoge eDTUs per database moeten worden ingericht in twee verschillende regio's bevatten de betaalde klanten tenant databases. De derde groep met het proefabonnement tenants hebben een lagere eDTUs per database en de gegevens in een van de twee regio.

Om te garanderen de laagste herstel keer tijdens bijvoorbeeld de vaste klanten tenant moet databases geografische gerepliceerd met 50% van de primaire-databases in elk van de twee regio's. Daarnaast moet elke regio 50% van de secundaire databases. Deze manier als een gebied offline is alleen 50% van de betaalde klanten databases zou van invloed zijn op en moet foutherstel. De andere databases wilt blijven behouden. Deze configuratie wordt geïllustreerd in het volgende diagram:

![Afbeelding 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Zoals in de vorige scenario's, een of meer management databases helemaal actief zijn zodat u ze als zelfstandige geografische-gerepliceerde database (s) (1) moet configureren. Dit zorgt ervoor dat de overzichtelijk prestaties van de nieuwe klant-abonnementen, profielupdates en andere beheertaken uit te voeren. Regio A zou het primaire regio voor het beheer van database (s) en de regio B worden gebruikt voor herstel van een of meer management databases.

De betaling klanten tenant databases wordt ook geografische gerepliceerd, maar met primaire en secundaire servers gesplitst tussen regio A en B (2) regio. Deze manier de primaire tenant-databases beïnvloed door de storing kunt naar het andere regio en beschikbaar komen. De andere helft van de tenant-databases worden niet helemaal beïnvloed. 

Het volgende diagram ziet u de stappen kunt ondernemen als tijdens een storing wordt weergegeven in de regio A.

![Afbeelding 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

- Mislukken meteen via de management-databases naar regio B (3).
- Wijzig de verbindingsreeks van de toepassing te laten verwijzen naar de database (s) management in regio B. het beheer van de database (s) om te controleren of de nieuwe accounts en tenant databases worden gemaakt in regio B en de bestaande tenant-databases gevonden er ook wijzigen. De bestaande proefabonnement klanten zien hun gegevens tijdelijk niet beschikbaar is.
- Failover de betaalde tenant-databases aan toepassingen 2 in gebied B direct herstel hun beschikbaarheid (4). Aangezien de overname een snelle metagegevens niveau wijziging die kunt u overwegen een optimalisatie waar de afzonderlijke failovers worden geactiveerd op aanvraag door de eindgebruiker verbindingen is. 
- Groep 2 bevat sinds nu alleen primaire databases die de totale werkbelasting in de groep neemt, dus moet u het formaat eDTU (5) direct verhogen. 
- Maak de nieuwe elastische toepassingen met dezelfde naam en dezelfde configuratie in de regio B voor het proefabonnement klanten databases (6). 
- Wanneer de toepassingen is gemaakt via u geografische herstellen de afzonderlijke proefversietenant-database terugzetten naar de groep (7). U kunt Houd rekening met de afzonderlijke Hiermee herstelt door de verbindingen eindgebruikers activeert of gebruiken van enkele andere toepassing specifieke prioriteit kleurenschema.


> [AZURE.NOTE] De bewerking failover is asynchroon. Minimaliseren het herstelproces is het belangrijk dat u de tenant-databases failover-opdracht in batches van ten minste 20 databases uitvoeren. 

Nu is uw toepassing weer online in regio b te drukken. Alle vaste klanten hebben toegang tot hun gegevens terwijl het proefabonnement klanten vertraging ondervinden wordt bij het openen van hun gegevens.

Wanneer regio A is hersteld, moet u bepalen of u gebruiken regio B voor proefabonnement klanten of failback wilt tot het gebruik van de groep proefabonnement klanten in de regio A. Eén criteria zijn de % van proefversietenant-databases die zijn gewijzigd sinds het herstelproces is. Ongeacht dat besluit moet u opnieuw saldo vanaf het betaalde tenants tussen twee toepassingen. het proces van het volgende diagram ziet u wanneer de databases proefversietenant terug naar regio A. mislukt  
 
![Afbeelding 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

- Alle uitstaande geografische-herstellen aanvragen voor proefabonnement DR-groep opheffen.   
- Failover de database management (8). Na herstel van het gebied, hebt u er de oude primaire automatisch de secundaire kwam. Nu wordt de primaire opnieuw.  
- Selecteer welke betaalde tenant databases terug naar de groep 1 en starten failover naar hun ontvangen (9 mislukt,). Na herstel van het gebied werd alle databases in de groep 1 automatisch secundaire servers. Nu 50% van deze worden primaire opnieuw. 
- Verklein de grootte van 2 van toepassingen naar de oorspronkelijke eDTU (10).
- Alle hersteld proefabonnement databases in de regio B tot en met alleen-lezen (11).
- Voor elke database in de proefversie DR-toepassingen die zijn gewijzigd sinds het herstelproces is Wijzig of verwijder de bijbehorende database in het proefabonnement primaire groep (12). 
- Kopieer de bijgewerkte databases uit de groep DR aan de primaire groep (13). 
- Verwijderen van de DR-toepassingen (14) 

De belangrijkste **voordelen** van deze strategie zijn:

- Biedt ondersteuning voor de meest agressieve SLA voor de betaling klanten omdat deze ervoor zorgt dat een storing kan niet van invloed op meer dan 50% van de tenant-databases zijn. 
- Dit zorgt ervoor dat de nieuwe experimenten opgeheven wordt zodra het spoor DR-toepassingen wordt gemaakt tijdens het herstelproces is. 
- Kan efficiënter gebruik van de capaciteit van toepassingen zoals 50% van secundaire databases in de groep 1 en 2 van toepassingen zijn gegarandeerd minder actief en vervolgens de primaire-databases.

Het belangrijkste **voor-en nadelen** zijn:

- De CRUD-bewerkingen ten opzichte van een of meer management databases heeft lagere latentie voor eindgebruikers verbonden met regio A dan voor eindgebruikers regio B verbonden als ze worden worden uitgevoerd voor de primaire van een of meer management databases.
- Er moeten complexere ontwerp van de database management. Bijvoorbeeld zou elke record tenant moeten zijn van een locatie-code die moet worden gewijzigd tijdens failover en foutherstel.  
- De betaling klanten prestaties lager dan normaal totdat de upgrade van toepassingen in regio B is voltooid. 

## <a name="summary"></a>Overzicht

In dit artikel ligt de nadruk op de strategieën voor noodgevallen voor de databaselaag die worden gebruikt door een SaaS ISV meerdere tenant-toepassing. De strategie die u kiest moet worden gebaseerd op de behoeften van de toepassing, zoals het model bedrijven, de SLA die u wilt bieden aan uw klanten, budgetteren van beperking enzovoort... Elke beschreven strategie bevatten de voordelen en de verhouding, zodat u een weloverwogen beslissing nemen kunt. Uw specifieke toepassing wordt waarschijnlijk ook andere Azure onderdelen. Zodat u de richtlijnen van hun bedrijven bedrijfscontinuïteit Controleer en goedkeuringen door het herstelproces is van het niveau van de database met hen. Meer informatie over het beheren van herstel van databasetoepassingen in Azure wordt aangegeven, raadpleegt u [ontwerpen cloud oplossingen voor herstel](./sql-database-designing-cloud-solutions-for-disaster-recovery.md) .  


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over Azure SQL-Database wordt automatisch back-ups, raadpleegt u [SQL-Database automatische back-ups](sql-database-automated-backups.md)
- Zie [overzicht van Business-bedrijfscontinuïteit](sql-database-business-continuity.md) voor een overzicht van de bedrijfscontinuïteit bedrijven en scenario's,
- Zie voor meer informatie over het gebruik van automatische back-ups voor herstel, [een van de service gestart back-ups van een database terugzetten](sql-database-recovery-using-backups.md)
- Zie voor meer informatie over sneller herstelopties, [Actief-geografische-replicatie](sql-database-geo-replication-overview.md)  
- Voor meer informatie over het gebruik van automatische back-ups voor het archiveren, raadpleegt u [database kopiëren](sql-database-copy.md)
