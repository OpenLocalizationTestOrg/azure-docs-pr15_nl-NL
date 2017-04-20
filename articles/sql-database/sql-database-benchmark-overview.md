<properties
    pageTitle="Overzicht van de vergelijkende Azure SQL-Database"
    description="In dit onderwerp wordt het Azure SQL Database-criterium gebruikt in de prestaties van Azure SQL-Database meten beschreven."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="06/21/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-benchmark-overview"></a>Overzicht van de vergelijkende Azure SQL-Database

## <a name="overview"></a>Overzicht
Microsoft Azure SQL Database biedt drie [Servicelagen](sql-database-service-tiers.md) met meerdere prestatieniveaus. Elk prestatieniveau biedt een toeneemt set resources of power, ontworpen voor steeds sneller worden verwerkt.

Het is belangrijk moeten kunnen kwantificeer hoe de toeneemt kracht van elke prestatieniveau zet in betere databaseprestaties. U doet dit Microsoft heeft ontwikkeld Azure SQL Database vergelijkende (ASDB). Het criterium oefent een combinatie van eenvoudige bewerkingen in alle werkbelasting in het OLTP gevonden. We meten de doorvoer bereikt voor databases op elk prestatieniveau.

De resources en de kracht van elk niveau van laag en prestaties service zijn uitgedrukt in een [Database transactie-eenheden (DTUs)](sql-database-technical-overview.md#understand-dtus). DTUs om er zelf een toe om te beschrijven van de relatieve capaciteit van een prestatieniveau op basis van een gemengde maateenheid van de processor, geheugen, en lezen en schrijven tarieven door elke prestatieniveau worden aangeboden. De DTU classificatie van een database wordt verdubbeld is gelijk aan de kracht van de database wordt verdubbeld. Het criterium kan wij nagaan wat het resultaat van de databaseprestaties van de toeneemt kracht door het werkelijke database bewerkingen uitvoeren, terwijl de schaalbaarheid van databasegrootte, het aantal gebruikers en transactie tarieven in verhouding tot de resources die zijn opgegeven in de database die worden aangeboden door elk prestatieniveau.

Door de doorvoer van de servicelaag eenvoudige uitdrukken transacties per uur, de Standard servicelaag met transacties per minuut en de Premium-servicelaag met transacties per seconde, eenvoudiger om snel de prestaties mogelijke van elke servicelaag aan de vereisten van een toepassing te gebruiken.

## <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Correlerende benchmarktest naar de prestaties van de echte wereld-database
Het is belangrijk om te begrijpen dat ASDB, zoals alle benchmarks, vertegenwoordiger en indicatieve alleen is. De transactie-tarieven bereikt met de toepassing vergelijkende worden niet hetzelfde als de items die kunnen worden behaald met andere toepassingen. Het criterium bestaat uit een verzameling verschillende transactie typen uitvoeren tegen een schema met een bereik van tabellen en gegevenstypen. Terwijl het criterium eenvoudige bewerkingen die voor alle werkbelasting in het OLTP oefeningen gelden, verwijst naam niet naar een specifieke klasse van database of een toepassing. Het doel van het criterium is op te geven van een redelijk hulplijn naar de relatieve prestaties van een database die mogelijk worden verwacht wanneer schaalbaarheid omhoog of omlaag tussen prestaties. In feite databases zijn van verschillende grootte en complexiteit optreden van verschillende combinaties van werkbelasting en reageert op verschillende manieren. Bijvoorbeeld een toepassing IO veel mogelijk sneller IO drempelwaarden raken, of een toepassing CPU veel mogelijk sneller CPU limieten raken. Er is geen garantie die een bepaalde database wordt schaal op dezelfde manier als referentie onder groter wordende laden.

Het criterium en de methodologie worden hieronder in detail beschreven.

## <a name="benchmark-summary"></a>Referentie-overzicht
ASDB meet de prestaties van een combinatie van eenvoudige databasebewerkingen die optreden meest in online transactie processing (OLTP) werkbelasting. Hoewel het criterium is ontworpen met cloud computing in gedachten, het databaseschema, gegevenspopulatie en transacties zijn ontworpen moeten ruim zijn vertegenwoordiger van het meest worden gebruikt in OLTP werkbelasting basiselementen.

## <a name="schema"></a>Schema
Het schema is bedoeld om voldoende diverse en de complexiteit ter ondersteuning van een groot aantal bewerkingen. Het criterium wordt uitgevoerd ten opzichte van een database die bestaat uit zes tabellen. De tabellen in drie categorieën vallen: vaste grootte, schalen en groeiende. Er zijn twee vaste grootte tabellen. drie schaal tabellen. en één groeiende tabel. Vaste grootte tabellen hebben een constant aantal rijen. Schaal tabellen hebben een verhouding dat staat in verhouding tot de prestaties van de database, maar niet wijzigen tijdens het criterium. De groeiende tabelformaat zoals een schaal tabel op de eerste keer wordt geladen, maar de verhouding wijzigingen tijdens het uitvoeren van het criterium als rijen worden ingevoegd of verwijderd.

Het schema bevat een combinatie van gegevenstypen, waaronder geheel getal, een numeriek teken en datum/tijd. Het schema bevat primaire en secundaire sleutels, maar niet de refererende sleutels: dat wil zeggen, zijn er geen beperkingen referentiële integriteit tussen tabellen.

Een gegevens generatie programma genereert de gegevens voor de eerste database. Geheel getal en numerieke gegevens wordt gegenereerd met verschillende strategieën. Waarden worden in sommige gevallen willekeurig verdeeld over een bereik. In andere gevallen een verzameling waarden is willekeurig gepermuteerde ervoor te zorgen dat een specifieke verdeling. Tekstvelden zijn gegenereerd op basis van een lijst weergegeven met de woorden waarnaar u wilt produceren realistische ogende gegevens.

Formaat van de database wordt bepaald op basis van een 'schaalfactor". De schaalfactor (afgekort tot EB) bepaalt de verhouding van de schaal en groeiende tabellen. Zoals hieronder beschreven in de sectie gebruikers en Pacing, de grootte van de database aantal gebruikers en maximale prestaties alle schaal in verhouding tot elkaar.

## <a name="transactions"></a>Transacties
De werklast bestaat uit negen typen, zoals wordt weergegeven in de onderstaande tabel. Elke transactie is bedoeld om een bepaalde reeks kenmerken in de database engine en systeem hardware, met hoog contrast van de andere transacties markeren. Deze methode gemakkelijker nagaan wat het resultaat van de verschillende onderdelen aan de algehele prestaties. De transactie "Gelezen dik" oplevert bijvoorbeeld een groot aantal gelezen bewerkingen vanaf schijf.

| Transactietype | Beschrijving |
|---|---|
| Lees Lite | SELECTEER; in het geheugen; alleen-lezen |
| Alleen gemiddeld | SELECTEER; voornamelijk in het geheugen; alleen-lezen |
| Alleen dik | SELECTEER; voornamelijk niet in het geheugen; alleen-lezen |
| Update Lite | BIJWERKEN. in het geheugen; alleen-lezen-schrijven |
| Dik bijwerken | BIJWERKEN. voornamelijk niet in het geheugen; alleen-lezen-schrijven |
| Lite invoegen | INVOEGEN; in het geheugen; alleen-lezen-schrijven |
| Donker symbool invoegen | INVOEGEN; voornamelijk niet in het geheugen; alleen-lezen-schrijven |
| Verwijderen | VERWIJDERQUERY; combinatie van in het geheugen en niet in het geheugen; alleen-lezen-schrijven |
| CPU dik | SELECTEER; in het geheugen; relatief dik belasting van processor; alleen-lezen |

## <a name="workload-mix"></a>Werkbelasting mix
Transacties worden willekeurig geselecteerd uit een gewogen verdeling met de volgende algehele mix. De algehele mix heeft een alleen-lezen/schrijven-breedteverhouding van ongeveer 2:1.

| Transactietype | % van Mix |
|---|---|
| Lees Lite | 35 |
| Alleen gemiddeld | 20 |
| Alleen dik | 5 |
| Update Lite | 20 |
| Dik bijwerken | 3 |
| Lite invoegen | 3 |
| Donker symbool invoegen | 2 |
| Verwijderen | 2 |
| CPU dik | 10 |

## <a name="users-and-pacing"></a>Gebruikers en pacing
De werklast vergelijkende wordt van een hulpmiddel waarmee transacties voor een reeks verbindingen met simuleren het gedrag van een aantal gelijktijdige gebruikers gestuurd. Hoewel alle verbindingen en transacties machine gegenereerd, voor eenvoudig verwezen naar deze verbindingen als 'gebruikers'. Hoewel iedere gebruiker onafhankelijk is van alle andere gebruikers, wordt in alle gebruikers de dezelfde cyclus van de onderstaande stappen uitvoeren:

1. Een databaseverbinding maken.
2. Herhalen totdat gesignaleerd om af te sluiten:
    - Selecteer een transactie willekeurig (vanuit een gewogen verdeling).
    - De geselecteerde transactie uitvoeren en de tijd antwoord meten.
    - Wacht totdat een pacing vertraging.
3. Sluit de database-verbinding.
4. Afsluiten.

De pacing vertraging (in stap 2c) willekeurig is geselecteerd, maar met een verdeling die een gemiddelde van 1,0 seconde heeft. Elke gebruiker kan, dus gemiddeld genereren voor maximaal één transactie per seconde.

## <a name="scaling-rules"></a>Schaalbaarheid van regels
Het aantal gebruikers wordt bepaald door de grootte van de database (in schaalfactor eenheden). Er is een gebruiker voor elke vijf schaalfactor eenheden. Vanwege de pacing vertraging, één gebruiker gemiddeld maximaal één transactie per seconde genereren.

Bijvoorbeeld een-schaalfactor van 500 (EB = 500) database wordt 100 gebruikers hebt en een maximum tarief van 100 TPS kunt bereiken. Als u wilt een hogere TPS station vereist rentabiliteit meer gebruikers en een groter-database.

De volgende tabel ziet het aantal gebruikers dat is wel opgelopen voor elk niveau van laag en prestaties service.

| Servicelaag (prestatieniveau). | Gebruikers | Omvang van database |
|---|---|---|
| Eenvoudige | 5 | 720 MB |
| Standaard (S0) | 10 | 1 GB |
| Standaard (S1) | 20 | 2.1 GB |
| Standaard (S2) | 50 | 7.1 GB |
| Premium (P1) | 100 | 14 GB |
| Premium (P2) | 200 | 28 GB |
| Premium (P6/P3) | 800 | 114 GB |

## <a name="measurement-duration"></a>Maateenheden duur
Een geldig vergelijkende uitvoeren is vereist voor de duur van een constante-status meting ten minste één uur.

## <a name="metrics"></a>Aan de doelstellingen
De belangrijkste maateenheden in het criterium zijn doorvoer en reactie tijd.

- Doorvoer is de eenheid essentiële prestaties in het criterium. Doorvoer wordt vermeld in transacties per eenheid-van-time, alle typen tellen.
- Antwoord tijd is een maateenheid voor prestaties voorspelbaarheid. De beperking van de tijd antwoord varieert met klasse van service, met hogere soorten service met een meer strikte antwoord tijd vereiste, zoals hieronder wordt weergegeven.

| Klasse van Service  | Doorvoer maateenheid | Antwoord tijd vereiste |
|---|---|---|
| Premium | Transacties per seconde | 95e percentiel bij 0,5 seconden |
| Standaard | Transacties per minuut | 90 percentiel bij 1,0 seconden |
| Eenvoudige | Transacties per uur | 80e percentiel bij 2.0 seconden |

## <a name="conclusion"></a>Sluiten
De Azure SQL Database-vergelijkende meet de relatieve prestaties van Azure SQL-Database die over het bereik van beschikbare service niveaus en prestaties. Het criterium oefent een combinatie van eenvoudige databasebewerkingen die optreden meest in online transactie processing (OLTP) werkbelasting. Door het meten van de werkelijke prestaties, biedt het criterium een meer betekenisvolle beoordeling van het effect op doorvoer van het wijzigen van het prestatieniveau van de dan mogelijk is door de resources die is verstrekt door elk niveau zoals CPU snelheid, de grootte van geheugen en IO's / s net aan te bieden. We blijft in de toekomst ontwikkelen van het criterium als u wilt uitbreiden, de scope en vouwt u de gegevens uit.

## <a name="resources"></a>Resources
[Inleiding tot SQL-Database](sql-database-technical-overview.md)

[Service niveaus en prestaties](sql-database-service-tiers.md)

[Prestaties richtlijnen voor één databases](sql-database-performance-guidance.md)
