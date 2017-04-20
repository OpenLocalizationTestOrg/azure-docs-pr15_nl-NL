<properties
   pageTitle="Cloud noodgevallen herstel solutions - SQL actieve geografische-databasereplicatie | Microsoft Azure"
   description="Informatie over het ontwerpen van cloud noodgevallen hersteloplossingen voor bedrijven bedrijfscontinuïteit planning geografische-replicatie met voor app gegevens back-up met Azure SQL-Database."
   keywords="cloud herstel, noodgevallen hersteloplossingen, back-up van app-gegevens, geografische-replicatie, bedrijven bedrijfscontinuïteit plannen"
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
   ms.workload="data-management"
   ms.date="07/20/2016"
   ms.author="sashan"/>

# <a name="design-an-application-for-cloud-disaster-recovery-using-active-geo-replication-in-sql-database"></a>Een toepassing voor de cloud-herstel met actieve geografische-herhaling in SQL-Database ontwerpen


> [AZURE.NOTE] [Actieve geografische-replicatie](sql-database-geo-replication-overview.md) is nu beschikbaar voor alle databases in alle lagen.



Leer hoe u het [Actieve geografische-herhaling](sql-database-geo-replication-overview.md) in SQL-Database kunt ontwerpen databasetoepassingen robuuste regionale fouten en fatale bijvoorbeeld. Voor bedrijven bedrijfscontinuïteit planning, kunt u bovendien de topologie van de implementatie van toepassing, de serviceovereenkomst die u hebt samengesteld, wordt verkeer latentie en kosten. In dit artikel we kijkt u naar de algemene toepassing patronen en bespreken van de voordelen en de voor-en nadelen van elke optie. Zie voor informatie over actieve geografische-replicatie met elastische toepassingen [elastische groep noodgevallen herstelstrategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Ontwerppatroon 1: actieve-passieve implementatie van cloud herstel met een reserveren database

Deze optie is het meest geschikt is voor toepassingen met de volgende kenmerken:

+ Actieve exemplaar in één Azure regio
+ Sterke afhankelijkheid van alleen-lezen-schrijven (RW) toegang tot gegevens
+ Cross-regio connectiviteit tussen de toepassingslogica en de database is niet acceptabel vanwege latentie en verkeer kosten    

In dit geval is de topologie zoektoepassing implementatie geoptimaliseerd voor het afhandelen van regionale systeemfouten wanneer alle toepassingsonderdelen ondervindt en nodig foutherstel als een eenheid. De toepassingslogica en de database worden gerepliceerd naar een andere regio voor geografische redundantie, maar ze niet worden gebruikt voor de werklast van de toepassing bij de normaal. De toepassing in de tweede regio moet worden geconfigureerd voor gebruik van een SQL-verbindingsreeks voor de secundaire database. Verkeer manager is geconfigureerd voor het [routeren failover-methode](../traffic-manager/traffic-manager-configure-failover-routing-method.md)te gebruiken.  

> [AZURE.NOTE] [Azure verkeer manager](../traffic-manager/traffic-manager-overview.md) wordt gebruikt in dit artikel alleen ter illustratie. U kunt een taakverdeling oplossing die ondersteuning biedt voor failover-methode gebruiken.    

Naast de exemplaren van het hoofdvenster van de toepassing, moet u rekening houden met een kleine [werknemer rol-toepassing](cloud-services-choose-me.md#tellmecs) die uw primaire database bewaakt door periodieke T-SQL alleen-lezen (RO) opdrachten implementeren. U kunt deze automatisch failover geactiveerd, een waarschuwing op de beheerconsole van de toepassing wordt gegenereerd, of beide. Om ervoor te zorgen dat monitoring niet wordt beïnvloed door regio hele bijvoorbeeld, moet u de controle toepassingsexemplaren implementeren naar elke regio en elk exemplaar verbonden met de hoofddatabase in de primaire regio en de secundaire database in de lokale regio. 

> [AZURE.NOTE] Beide controleren toepassingen moet actief zijn en zowel primaire als secundaire databases zoeken. De laatste kan worden gebruikt om te bepalen van een fout in de tweede regio en een melding wanneer de toepassing niet is beveiligd.     

In het volgende diagram ziet u deze configuratie voordat u een storing.

![SQL geografische-databasereplicatie configuratie. Herstel van de cloud.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Na een storing in de primaire regio, wordt de controle-toepassing gedetecteerd dat de hoofddatabase niet toegankelijk is en wordt een melding geregistreerd. Afhankelijk van uw toepassing SLA, kunt u bepalen hoeveel opeenvolgende controleren sondes moeten mislukt voordat een storing database gedeclareerd. Als u wilt bereiken gecoördineerde overname van de toepassing fungeert en de database, hebt u de controle-toepassing de volgende stappen uitvoeren:

1. [De status van de primaire fungeert bijwerken](https://msdn.microsoft.com/library/hh758250.aspx) om te leiden tot fungeert failover.
2. Bel de secundaire database voor het [starten van de database failover](sql-database-geo-replication-portal.md).

Na een failover is de toepassing worden verwerkt door het verzoek van de gebruiker in de tweede regio, maar blijven met de database omdat de primaire database nu weergegeven in de tweede regio worden. Dit scenario wordt geïllustreerd door het volgende diagram. In alle diagrammen, ononderbroken lijn actieve verbinding aangeeft, stippellijnen geschorst verbindingen en stoppen positief of negatief actie triggers aangeven.


![Geografische-replicatie: Failover met secundaire database. Back-up-App gegevens.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

Als een storing gebeurt er in de tweede regio, wordt de replicatiekoppeling tussen de primaire en secundaire database is geschorst en wordt de controle-toepassing een melding dat de hoofddatabase wordt blootgesteld registreert. Prestaties van de toepassing wordt niet beïnvloed, maar de toepassing weergegeven werkt en daarom hoger risico in hoofdletters/kleine letters beide regio's mislukken achter elkaar.

> [AZURE.NOTE] Wordt aangeraden alleen implementatieconfiguraties met één DR regio. Dit komt omdat de meeste van de Azure locaties twee regio's hebben. Deze configuraties wordt niet uw toepassing beschermen tegen een volledige uitval van beide regio's. U kunt uw databases in een derde regio met [geografische terugzetten](sql-database-disaster-recovery.md#recovery-using-geo-restore)herstellen in niet waarschijnlijk geval van een dergelijke storing.

Zodra de storing beperkt, wordt de secundaire database automatisch gesynchroniseerd met de primaire. Tijdens de synchronisatie kan prestaties van de primaire iets worden beïnvloed afhankelijk van de hoeveelheid gegevens die moeten worden gesynchroniseerd. In het volgende diagram ziet u een storing in de tweede regio.

![Secundaire database gesynchroniseerd met primair. Herstel van de cloud.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)


De belangrijkste **voordelen** van dit ontwerppatroon zijn:

+ De SQL-verbindingsreeks tijdens de toepassingsimplementatie is ingesteld in elke regio en niet na failover.
+ Prestaties van de toepassing wordt niet beïnvloed door failover als de toepassing en de database hebben altijd een gezamenlijk bevindt.

De belangrijkste **verhouding** is het toepassingsexemplaar overtollige in de tweede regio wordt alleen gebruikt voor problemen oplossen.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Ontwerppatroon 2: actieve implementatie van taakverdeling van toepassing
Deze optie cloud noodgevallen herstel is het meest geschikt is voor toepassingen met de volgende kenmerken:

+ Hoogte-breedteverhouding van database naar schrijft wordt gelezen
+ Database schrijven latentie heeft geen gevolgen voor de eindgebruiker  
+ Alleen-lezen logica kan van alleen-lezen-schrijven logica worden gescheiden met behulp van een andere verbindingsreeks
+ Alleen-lezen logica is niet afhankelijk van gegevens volledig worden gesynchroniseerd met de meest recente updates  

Als uw toepassingen hebt deze kenmerken, wordt de verbindingen eindgebruikers voor taakverdeling tussen meerdere toepassingsexemplaren in verschillende regio's kunt verbeteren prestaties en de eindgebruiker. Implementatie van taakverdeling, moet elke regio een actief exemplaar van de toepassing hebben met de alleen-lezen-schrijven (RW) logica verbonden met de hoofddatabase in de primaire regio. De logica van de (RO) alleen-lezen moet worden verbonden met een secundaire database in hetzelfde gebied, als het toepassingsexemplaar van de. Verkeer manager moet worden ingesteld voor gebruik van [round robin routering](../traffic-manager/traffic-manager-configure-round-robin-routing-method.md) of [omleiding van prestaties](../traffic-manager/traffic-manager-configure-performance-routing-method.md) met [fungeert monitoring](../traffic-manager/traffic-manager-monitoring.md) ingeschakeld voor elk toepassingsexemplaar.

Zoals in het patroon #1, moet u rekening houden met een soortgelijke controleren toepassing implementeren. Maar in tegenstelling tot patroon #1, de controle-toepassing worden niet verantwoordelijk voor de overname fungeert activeert.

> [AZURE.NOTE] Terwijl dit patroon gebruikmaakt van meer dan één secundaire database, zou slechts één van de secundaire servers voor failover worden gebruikt om de redenen hierboven. Omdat dit patroon alleen-lezen toegang tot de secundaire, moet deze actieve geografische-herhaling.

Verkeer manager moet worden geconfigureerd voor de prestaties routering rechtstreeks van de gebruikersverbindingen met een exemplaar van de toepassing dichtst bevindt bij de geografische locatie van de gebruiker. In het volgende diagram ziet u deze configuratie voordat u een storing.
![Geen storing: prestaties zoekresultaten omleiden naar het dichtstbijzijnde toepassing. Geografische-herhaling.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Als er een storing database is gevonden in de primaire regio, start u overname van de primaire database op een van de tweede regio's wijzigen van de locatie van de hoofddatabase. De manager verkeer automatisch worden uitgesloten van de offline fungeert van de routering tabel, maar blijft het verkeer eindgebruikers zoekresultaten omleiden naar de resterende exemplaren online. Omdat de primaire database bevindt zich nu in een andere regio, moeten de overal online hun alleen-lezen-schrijven SQL-verbindingsreeks verbinding maken met de nieuwe primaire wijzigen. Het is belangrijk dat u deze wijziging voordat u de overname van de database wordt gestart. De alleen-lezen SQL verbindingstekenreeksen moeten blijven ongewijzigd terwijl ze altijd wijst u de database in dezelfde regio. De stappen failover zijn:  

1. Alleen-lezen-schrijven SQL verbindingstekenreeksen zodat deze verwijzen naar de nieuwe primaire wijzigen.
2. De aangewezen secundaire database in de volgorde [starten database](sql-database-geo-replication-portal.md)foutherstel bellen.

In het volgende diagram ziet u de nieuwe configuratie na de overname.
![Configuratie na failover. Herstel van de cloud.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

In geval van een storing in een van de tweede regio's verwijderen de manager verkeer automatisch de offline fungeert in dat gebied uit de routering tabel. Het kanaal herhaling met de secundaire database in dat land is geschorst. Omdat de resterende regio's u aanvullende gebruiker-verkeer is toegestaan in dit scenario, worden de prestaties van de toepassing wordt beïnvloed tijdens de storing. Zodra de storing beperkt, wordt onmiddellijk de secundaire database in het risico regio gesynchroniseerd met de primaire. Tijdens de synchronisatie kan prestaties van de primaire iets worden beïnvloed afhankelijk van de hoeveelheid gegevens die moeten worden gesynchroniseerd. In het volgende diagram ziet u een storing in een van de tweede regio's.

![Storing in secundaire regio. Cloud herstel - geografische-herhaling.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

Het belangrijkste **voordeel** van deze ontwerppatroon is dat u de werklast van de toepassing kunt schalen over meerdere ontvangen bereiken van de eindgebruiker optimale prestaties. De **compromissen nodig zijn** van deze optie zijn:

+ Alleen-lezen-schrijven verbindingen tussen de toepassingsexemplaren en de database hebben verschillende latentie en kosten
+ Toepassingsprestaties wordt beïnvloed tijdens de storing
+ Toepassingsexemplaren zijn vereist om de SQL-verbindingsreeks dynamisch na failover van de database te wijzigen.  

> [AZURE.NOTE] Een soortgelijke manier kan worden gebruikt te dragen gespecialiseerde werkbelastingen, zoals het rapporteren van taken, bedrijfsinformatiehulpprogramma's of back-ups. Meestal de aanzienlijk database resources voor het gebruik van deze werkbelasting daarom het wordt aanbevolen dat u een van de secundaire databases deze met het prestatieniveau afgestemd op de verwachte werklast wijst.

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Ontwerppatroon 3: actieve-passieve implementatie van gegevens bewaren  
Deze optie is het meest geschikt is voor toepassingen met de volgende kenmerken:

+ Elk verlies van gegevens is hoog bedrijven risico. De overname van de database kan alleen worden gebruikt als noodgevallen als de storing niet kan hersteld worden.
+ De toepassing kan worden uitgevoerd in 'alleen-lezenmodus' voor een bepaalde periode.

In dit patroon schakelt u de toepassing naar alleen-lezen wanneer u verbinding hebt met de secundaire database. De toepassingslogica in de primaire regio bevindt zich samen met de hoofddatabase en werkt in de modus alleen-lezen-schrijven (RW). De toepassingslogica in de tweede regio bevindt zich samen met de secundaire database en is gereed voor gebruik in de modus alleen-lezen (RO).  Verkeer manager moet worden ingesteld voor gebruik van de [failover-mailroutering](../traffic-manager/traffic-manager-configure-failover-routing-method.md) met [fungeert monitoring](../traffic-manager/traffic-manager-monitoring.md) ingeschakeld voor beide van de toepassing.

In het volgende diagram ziet u deze configuratie voordat u een storing.
![De implementatie van de actieve-passieve voordat failover wordt uitgevoerd. Herstel van de cloud.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Wanneer de manager verkeer vastgesteld een fout is connectiviteit met de primaire regio, schakelt deze automatisch gebruikersverkeer naar het exemplaar van de toepassing in de tweede regio. Met dit patroon is het belangrijk dat u **niet** initiëren database failover nadat de storing wordt aangetroffen. De toepassing in de tweede regio is geactiveerd en werkt in de modus alleen-lezen voor de secundaire database gebruikt. Dit wordt geïllustreerd door het volgende diagram.

![Storing: Toepassing in de modus alleen-lezen. Herstel van de cloud.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Wanneer de storing in de primaire regio is beperkt, wordt verkeer manager het herstel van connectivity vastgesteld in de primaire regio en schakelt u gebruikersverkeer terug naar het exemplaar van de toepassing in de primaire regio. Dat toepassingsexemplaar cv's en werkt in de modus alleen-lezen-schrijven met de hoofddatabase.

> [AZURE.NOTE] Omdat dit patroon voor alleen-lezen toegang tot de secundaire moet deze actieve geografische-herhaling.

De manager verkeer Hiermee markeert u de toepassing fungeert in de primaire regio als niet beschikbaar voor het geval een storing in de tweede regio, en het kanaal replicatie is geschorst. Deze storing heeft echter geen gevolgen voor de prestaties van de toepassing tijdens de storing. Zodra de storing beperkt, wordt onmiddellijk de secundaire database gesynchroniseerd met de primaire. Tijdens de synchronisatie kan prestaties van de primaire iets worden beïnvloed afhankelijk van de hoeveelheid gegevens die moeten worden gesynchroniseerd.

![Storing: Secundaire database. Herstel van de cloud.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

Dit ontwerppatroon kent diverse **voordelen**:

+ Verlies van gegevens wordt voorkomen tijdens het tijdelijke bijvoorbeeld.
+ Deze hoeft u niet aan een controle-toepassing implementeren, zoals het herstelproces is door verkeer manager wordt geactiveerd.
+ Downtime afhankelijk alleen hoe snel de fout connectivity kan worden geconfigureerd door verkeer manager worden gedetecteerd.

De **compromissen nodig zijn** zijn:

+ Toepassing moet kunnen werken in de modus alleen-lezen.
+ Dit is vereist voor dynamisch overschakelen naar deze wanneer deze is verbonden met een alleen-lezen-database.
+ Machtiging om de lezen / schrijven bewerkingen hervatten vereist herstel van de primaire regio, wat betekent de toegang tot volledige gegevens kan worden uitgeschakeld dat voor uren of zelfs dagen.

> [AZURE.NOTE] In geval van een servicestoring permanente in de regio, moet u handmatig database failover activeren en accepteer de gegevens verloren gaan. De toepassing, worden functionele in de tweede regio met alleen-lezen-schrijven toegang tot de database.

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Zakelijke bedrijfscontinuïteit planning: Kies een toepassing ontwerpen voor herstel van de cloud

Uw specifieke cloud noodherstelplan kunt combineren of deze ontwerppatronen om het beste aan de behoeften van uw toepassing uit te breiden.  Zoals eerder is vermeld, wordt de strategie die u kiest is gebaseerd op de SLA die u wilt bieden naar uw klanten en de topologie zoektoepassing implementatie. Om u te helpen uw beslissing, de volgende tabel worden vergeleken de opties op basis van de geschatte gegevensverlies of herstellen doelstelling (vrijgegeven Productieorder) en geschatte hersteltijd (invoegen).

| Patroon | VRIJGEGEVEN PRODUCTIEORDER | INVOEGEN
| :--- |:--- | :---
| Actieve-passieve implementatie van herstel met toegang tot de database samen bevindt | Lees-/ schrijftoegang < 5 sec | Fout bij detectie tijd + failover-API bellen + controle van toepassingen testen
| Actieve implementatie van taakverdeling van toepassing | Lees-/ schrijftoegang < 5 sec | Fout bij detectie tijd failover-API-oproep + SQL-verbindingsreeks wijzigen + controle van toepassingen testen
| Actieve-passieve implementatie van gegevens bewaren | Alleen-lezentoegang < 5 sec lees-/ schrijftoegang = nul | Alleen-lezentoegang = connectivity mislukt detectie tijd + toepassing verificatie testen <br>Lees-/ schrijftoegang = tijd te verminderen de storing

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over Azure SQL-Database wordt automatisch back-ups, raadpleegt u [SQL-Database automatische back-ups](sql-database-automated-backups.md)
- Zie [overzicht van Business-bedrijfscontinuïteit](sql-database-business-continuity.md) voor een overzicht van de bedrijfscontinuïteit bedrijven en scenario's,
- Zie voor meer informatie over het gebruik van automatische back-ups voor herstel, [een van de service gestart back-ups van een database terugzetten](sql-database-recovery-using-backups.md)
- Zie voor meer informatie over sneller herstelopties, [Actief-geografische-replicatie](sql-database-geo-replication-overview.md)  
- Voor meer informatie over het gebruik van automatische back-ups voor het archiveren, raadpleegt u [database kopiëren](sql-database-copy.md)
- Zie voor informatie over actieve geografische-replicatie met elastische toepassingen [elastische groep noodgevallen herstelstrategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
