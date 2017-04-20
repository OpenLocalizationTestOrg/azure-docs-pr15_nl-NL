<properties 
    pageTitle="Eenheden in DocumentDB aanvragen | Microsoft Azure" 
    description="Meer informatie over hoe u begrijpen en schatten van de aanvraag eenheid vereisten in DocumentDB opgeven." 
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="mimig" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="syamk"/>

#<a name="request-units-in-documentdb"></a>Eenheden in DocumentDB aanvragen
Nu beschikbaar: DocumentDB [verzoek eenheid Rekenmachine](https://www.documentdb.com/capacityplanner). Meer informatie in [Estimating uw doorvoer nodig heeft](documentdb-request-units.md#estimating-throughput-needs).

![Doorvoer Rekenmachine][5]

##<a name="introduction"></a>Inleiding
Dit artikel bevat een overzicht van de aanvraag eenheden in [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). 

Lees dit artikel en kunt u wel de volgende vragen beantwoorden:  

-   Wat zijn eenheden aanvragen en aanvragen van de kosten?
-   Hoe geef ik verzoek eenheid capaciteit voor een siteverzameling?
-   Hoe u schatten dat van mijn toepassing verzoek heeft?
-   Wat gebeurt er als ik verzoek eenheid capaciteit voor een siteverzameling overschrijdt?


##<a name="request-units-and-request-charges"></a>Eenheden aanvragen en kosten aanvragen
DocumentDB biedt snelle, overzichtelijk prestaties door alle resources *worden gereserveerd* om te voldoen aan dat van de toepassing doorvoer nodig heeft.  Omdat toepassing laden en toegang tot patronen wijzigen na verloop van tijd, wordt er DocumentDB kunt u eenvoudig vergroten of verkleinen van de hoeveelheid gereserveerde doorvoer beschikbaar voor uw toepassing.

Gereserveerde doorvoer is opgegeven met DocumentDB, tussen aanvraag eenheden per seconde verwerkt.  U kunt verzoek eenheden zien als doorvoer valuta, waarbij u *Reserveer* een hoeveelheid gegarandeerd verzoek eenheden beschikbaar voor uw toepassing per seconde worden opgeteld.  Elke bewerking in DocumentDB - schrijven van een document, uitvoering van een query, het bijwerken van een document - verbruikt processor en geheugen IO's / s.  Elke bewerking bijhoudt dat wil zeggen een *aanvraag boete*, uitgedrukt in *eenheden van de aanvraag*.  Informatie over de factoren die van invloed zijn op verzoek eenheid kosten, samen met de vereisten van uw toepassing doorvoer, kunt u uw toepassing uitvoeren als kosten effectief mogelijk. 

##<a name="specifying-request-unit-capacity"></a>Aanvraag eenheid capaciteit opgeven
Wanneer u een verzameling DocumentDB maakt, kunt u het aantal eenheden van de aanvraag per seconde (RUs) u gereserveerd voor de siteverzameling wilt opgeven.  Nadat de verzameling is gemaakt, is de volledige toewijzing van RUs opgegeven gereserveerd voor gebruik van de siteverzameling.  Elke siteverzameling is gegarandeerd hebt toegewezen en geïsoleerd doorvoer kenmerken.  

Het is belangrijk te weten dat DocumentDB op een model reservering werkt; u bent dat wil zeggen gefactureerd voor de hoeveelheid doorvoer *gereserveerde* voor de collectie, ongeacht hoeveel waarop doorvoer actief *gebruikt is*.  Houd er rekening mee echter, die als van uw toepassing laden, gegevens en gebruik patronen wijzigen die u eenvoudig omhoog en omlaag de hoeveelheid schalen kunt gereserveerd RUs via DocumentDB SDK's of met behulp van de [Azure-Portal](https://portal.azure.com).  Zie voor meer informatie over schaal doorvoer omhoog en omlaag, [DocumentDB prestaties](documentdb-performance-levels.md).

##<a name="request-unit-considerations"></a>Aandachtspunten voor de eenheid aanvragen
Bij het bepalen van het aantal eenheden van de aanvraag reserveren voor uw siteverzameling DocumentDB, is het belangrijk zijn de volgende variabelen in aanmerking te nemen:

- **Grootte van document**. Als grootte van document de eenheden die zijn verbruikt om te lezen of schrijven van dat de gegevens worden ook vergroten vergroot.
- **Aantal van de eigenschap documenten**. Uitgaande standaard indexeren van alle eigenschappen, de eenheden die zijn verbruikt u schrijft een document, neemt toe naarmate de eigenschap aantal toeneemt.
- **Consistentie van de gegevens**. Wanneer u gegevens consistentie niveaus van sterke of Staleness gebonden, wordt u aanvullende eenheden verbruikt om documenten te lezen.
- **Eigenschappen van geïndexeerd**. Een index-beleid op elke verzameling bepaalt welke eigenschappen al dan niet standaard zijn geïndexeerd. Door het aantal geïndexeerde eigenschappen te beperken of doordat fikse indexeren, kunt u uw aanvraag eenheidsverbruik verkleinen.
- **Document indexeren**. Al dan niet standaard die elk document wordt automatisch geïndexeerd, wordt u minder verzoek eenheden gebruiken als u niet wilt indexeren enkele van uw documenten.
- **Query patronen**. De complexiteit van een query van invloed op hoeveel aanvragen eenheden worden verbruikt voor een bewerking. Het aantal predicaten, aard van de predicaten, prognoses aantal UDF's en de grootte van de gegevensverzameling bron alle invloed hebben op de kosten van querybewerkingen.
- **Script gebruik**.  Als u met de query's, opgeslagen procedures en triggers in beslag nemen verzoek eenheden op basis van de complexiteit van de bewerkingen die wordt uitgevoerd. Als u uw toepassing ontwikkelt, controleren de koptekst van de aanvraag boete voor meer informatie over hoe elke bewerking verzoek eenheid capaciteit verbruikt.

##<a name="estimating-throughput-needs"></a>Schatten van doorvoer behoeften
Een eenheid aanvraag is een genormaliseerde maateenheid voor de verwerking van de kosten van aanvragen. Een eenheid één aanvraag staat voor de van verwerkingscapaciteit nodig is om te lezen (via self koppeling of -id) één 1KB JSON document die bestaat uit 10 unieke eigenschapswaarden (exclusief Systeemeigenschappen). Een aanvraag om deel te maken (insert), vervangen of verwijderen van hetzelfde document wordt verbruikt meer verwerking van de service en daarmee ook meer verzoek eenheden.   

> [AZURE.NOTE] De basislijn van de eenheid 1 verzoek voor een document 1KB komt overeen met een eenvoudige GET door zelf koppeling of -id van het document.

###<a name="use-the-request-unit-calculator"></a>Het verzoek eenheid Rekenmachine gebruiken
Om te helpen klanten fijn af te stellen hun schattingen doorvoer, wordt er een op het web- [verzoek eenheid Rekenmachine](https://www.documentdb.com/capacityplanner) om te helpen schatten van de aanvraag eenheid vereisten voor veelgebruikte bewerkingen, waaronder:

- Document wordt gemaakt (schrijft)
- Document leest
- Document worden verwijderd
- Updates van documenten

Het hulpmiddel bevat ook de ondersteuning voor het schatten van gegevens opslagbehoeften op basis van de steekproef-documenten die u opgeeft.

Hulpprogramma voor het is eenvoudig:

1. Een of meer vertegenwoordiger JSON documenten uploaden.

    ![Documenten uploaden naar de aanvraag eenheid Rekenmachine][2]

2. Als u wilt schatten voor gegevensopslag, voert u het totale aantal documenten dat u verwacht om op te slaan.

3. Voer het nummer van document maken, lezen, bijwerken en verwijderen van de bewerkingen die u nodig (op basis van het per seconde hebt). Als u wilt de kosten van de aanvraag per eenheid van document updatebewerkingen schatten, upload een kopie van het voorbeelddocument uit stap 1 bovenstaande met normale veld updates.  Bijvoorbeeld documentupdates wijzigen meestal twee eigenschappen lastLogin en userVisits, klikt u vervolgens gewoon de voorbeelddocument kopiëren, de waarden voor deze twee eigenschappen bijwerken als het gekopieerde document uploaden.

    ![Doorvoer vereisten invoeren in de aanvraag eenheid Rekenmachine][3]

4. Klik op berekenen en bekijk de resultaten.

    ![Eenheid Rekenmachine resultaten aanvragen][4]

>[AZURE.NOTE]Als u documenttypen die aanzienlijk tussen de grootte en het aantal geïndexeerde eigenschappen verschillen wordt hebt, klikt u vervolgens een voorbeeld van elk *type* document dat typische uploaden naar de knop en klikt u vervolgens de resultaten te berekenen.

###<a name="use-the-documentdb-request-charge-response-header"></a>Tekenarcering DocumentDB verzoek boete gebruiken
Elk antwoord van de service DocumentDB bestaat uit een aangepaste header (x ms-verzoek-gratis) met de aanvraag eenheden verbruikt voor het verzoek. Deze koptekst is ook toegankelijk is via de DocumentDB SDK's. In de SDK .NET is RequestCharge een eigenschap van het object ResourceResponse.  Voor query's geeft de Verkenner DocumentDB Query in de portal van Azure verzoek boete voor informatie over uitgevoerde query's.

![RU kosten in de Verkenner Query controleren][1]

Met dit in gedachten, een methode voor het schatten van de hoeveelheid gereserveerde doorvoer vereist door de toepassing is de aanvraag eenheid boete die is gekoppeld aan de gebruikelijke bewerkingen uitgevoerd op een vertegenwoordiger document gebruikt door de toepassing opnemen en vervolgens het aantal bewerkingen schatting wilt u uitvoeren van elke tweede.  Zorg ervoor dat u meten en normale query's en DocumentDB script gebruik ook opnemen.

>[AZURE.NOTE]Als er documenttypen die aanzienlijk tussen de grootte en het aantal geïndexeerde eigenschappen verschillen wordt, klikt u vervolgens de toepassing bewerking verzoek eenheid boete die is gekoppeld aan elk *type* document dat typische opnemen.

Bijvoorbeeld:

1. Opnemen van de aanvraag eenheid boete maken (invoegen) een typisch document. 
2. Het verzoek eenheid kosten van een typisch document leest-record.
3. Record de aanvraag eenheid kosten van een typisch document wordt bijgewerkt.
3. Het verzoek eenheid boete van query's typische, algemene document opnemen.
4. Het verzoek eenheid boete van alle aangepaste scripts (opgeslagen procedures, triggers, de gebruiker gedefinieerde functies) overgenomen door de toepassing opnemen
5. De eenheden die zijn vereist verzoek gezien het geschatte aantal bewerkingen die u van plan om uit te voeren per seconde te berekenen.

##<a name="a-request-unit-estimation-example"></a>Een voorbeeld van een aanvraag schatting
Houd rekening met het volgende ~ 1KB-document:

    {
     "id": "08259",
    "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
    "tags": [
        {
        "name": "cereals ready-to-eat"
        },
        {
        "name": "kellogg"
        },
        {
        "name": "kellogg's crispix"
        }
    ],
    "version": 1,
    "commonName": "Includes USDA Commodity B855",
    "manufacturerName": "Kellogg, Co.",
    "isFromSurvey": false,
    "foodGroup": "Breakfast Cereals",
    "nutrients": [
        {
        "id": "262",
        "description": "Caffeine",
        "nutritionValue": 0,
        "units": "mg"
        },
        {
        "id": "307",
        "description": "Sodium, Na",
        "nutritionValue": 611,
        "units": "mg"
        },
        {
        "id": "309",
        "description": "Zinc, Zn",
        "nutritionValue": 5.2,
        "units": "mg"
        }
    ],
    "servings": [
        {
        "amount": 1,
        "description": "cup (1 NLEA serving)",
        "weightInGrams": 29
        }
    ]
    }

>[AZURE.NOTE]Documenten zijn minified in DocumentDB, zodat het systeem berekend grootte van de bovenstaande document iets kleiner is dan 1KB.


De volgende tabel bevat niet-geheel exacte verzoek kosten per eenheid voor veelgebruikte bewerkingen voor dit document (de kosten van de eenheid benadering aanvraag wordt ervan uitgegaan dat het niveau van de consistentie account is ingesteld op "Sessie" en dat alle documenten automatisch worden geïndexeerd):

Bewerking|Kosten per eenheid aanvragen 
---|---
Document maken|~ 15 RU 
Document lezen|~ 1 RU
Query-document op id|~2.5 RU

Deze tabel ziet niet-geheel exacte verzoek bovendien de kosten per eenheid voor typische query's in de toepassing gebruikt:

Query|Kosten per eenheid aanvragen|# Geretourneerde documenten
---|---|--- 
Eten selecteren door-id|~2.5 RU|1 
Selecteer het eten door fabrikant|~ 7 RU|7
Selecteren door eten groep en de volgorde van het gewicht|~ 70 RU|100
Bovenste 10 eten in een groep eten selecteren|~ 10 RU|10

>[AZURE.NOTE]RU gesprekskosten zijn afhankelijk van het aantal documenten als resultaat gegeven.

Met deze gegevens kunnen we de vereisten RU van deze toepassing gezien het aantal bewerkingen en query's die we verwachten per seconde verwachten:

Bewerking/Query|Geschatte aantal per seconde|Vereiste RUs 
---|---|--- 
Document maken|10|150 
Document lezen|100|100 
Selecteer het eten door fabrikant|25|175 
Selecteer per eten groep|10|700 
Top 10 selecteren|15|150 totaal|155|1275

In dit geval verwachten we een vereiste gemiddelde doorvoer van 1,275 RU/s.  Afronding tot de dichtstbijzijnde 100, zou doen we inrichten van 1300 RU/s voor de siteverzameling van deze toepassing.

##<a id="RequestRateTooLarge"></a>Meer dan gereserveerde doorvoer limieten
Intrekken dat verzoek eenheidsverbruik als een percentage per seconde wordt geëvalueerd. Voor toepassingen die groter is dan het tarief weer dat eenheid ingerichte verzoek voor een siteverzameling, aanvragen aan die verzameling wordt de snelheid van pas het tarief weer dat het gereserveerde niveau. Wanneer u een vertraging plaatsvindt, wordt de server preemptively beëindigen van de aanvraag met RequestRateTooLargeException (HTTP-statuscode 429) en de kop van de x-ms-opnieuw-na-ms waarin wordt aangegeven dat de hoeveelheid tijd in milliseconden, die de gebruiker wachten moet voordat deze opnieuw proberen van de aanvraag retourneren.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Als u de query's .NET Client SDK en LINQ en klik u dat nooit hoeft te handelen deze uitzondering, zoals de huidige versie van de Client .NET SDK impliciet dit antwoord houdt meestal gebruikt, houdt rekening met de server opgegeven opnieuw na de koptekst en pogingen het verzoek. Tenzij uw account is gelijktijdig door meerdere klanten wordt geopend, slagen de volgende poging.

Als er meer dan één client cumulatief werken boven het tarief weer dat verzoek, het standaardgedrag van de opnieuw mogelijk niet voldoende en de client in de weergave van een DocumentClientException met statuscode 429 met de toepassing. In gevallen zoals volgt, kunt u overwegen opnieuw gedrag en logica van uw toepassing fout routines voor het afhandelen of waardoor de gereserveerde doorvoer voor de collectie verwerken.

##<a name="next-steps"></a>Volgende stappen

Meer informatie over gereserveerde doorvoer met DocumentDB Azure-databases, bekijk het volgende materiaal:
 
- [DocumentDB prijzen](https://azure.microsoft.com/pricing/details/documentdb/)
- [DocumentDB capaciteit beheren](documentdb-manage.md) 
- [Gegevens in DocumentDB Modeling](documentdb-modeling-data.md)
- [Prestaties van DocumentDB](documentdb-partition-data.md)

Meer informatie over DocumentDB, raadpleegt u de Azure DocumentDB [documentatie](https://azure.microsoft.com/documentation/services/documentdb/). 

Zie [prestaties en schaal met Azure DocumentDB testen](documentdb-performance-testing.md)om te beginnen met schaal en prestaties testen met DocumentDB.


[1]: ./media/documentdb-request-units/queryexplorer.png 
[2]: ./media/documentdb-request-units/RUEstimatorUpload.png
[3]: ./media/documentdb-request-units/RUEstimatorDocuments.png
[4]: ./media/documentdb-request-units/RUEstimatorResults.png
[5]: ./media/documentdb-request-units/RUCalculator2.png
