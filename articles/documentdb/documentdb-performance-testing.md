<properties 
    pageTitle="DocumentDB schaal en prestaties testen | Microsoft Azure" 
    description="Leer hoe u de schaal en prestaties testen met Azure DocumentDB uitvoeren"
    keywords="prestaties testen"
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="performance-and-scale-testing-with-azure-documentdb"></a>Prestaties en schaal testen met Azure DocumentDB
Prestaties en schaal testen is een belangrijke stap bij het ontwikkelen van toepassingen. Voor veel toepassingen, de databaselaag heeft een belangrijke invloed op de algehele prestaties en schaalbaarheid en daarom een essentieel onderdeel van de prestaties testen. [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) specifiek samengestelde voor elastische schaal en overzichtelijk prestaties is en daarom uitstekend geschikt voor toepassingen die een databaselaag krachtige nodig. 

In dit artikel is een verwijzing voor ontwikkelaars prestaties test-suites voor hun werkbelasting DocumentDB implementeren of evaluatie van DocumentDB voor toepassing van de krachtige scenario's. Het gaat hoofdzakelijk over geïsoleerd prestaties testen van de database, maar ook bevat aanbevolen procedures voor het productietoepassingen.

Lees dit artikel en kunnen u de volgende vragen beantwoorden:   

- Waar kan ik een voorbeeld .NET-clienttoepassing voor het testen van de prestaties van Azure DocumentDB vinden? 
- Hoe ik hoge doorvoer niveaus met Azure DocumentDB van mijn clienttoepassing bereiken?

Download het project uit [DocumentDB prestaties testen voorbeeld](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)om te beginnen met code. 

> [AZURE.NOTE] Het doel van deze toepassing is om te laten zien aanbevolen procedures voor het extraheren van betere prestaties afmelden bij DocumentDB met een klein aantal clientcomputers. Dit is niet gemaakt om te laten zien van de capaciteit piek van de service, die limitlessly upgrades.

Als u op zoek bent naar aan de clientzijde configuratieopties DocumentDB prestaties te verbeteren, raadpleegt u [tips voor betere prestaties DocumentDB](documentdb-performance-tips.md).

## <a name="run-the-performance-testing-application"></a>De prestaties toepassing testen uitvoeren
De snelste manier om aan de slag is compileren en uitvoeren van de steekproef .NET onder, zoals is beschreven in de onderstaande stappen. U kunt ook bekijken van de broncode en soortgelijke configuraties implementeren naar uw eigen clienttoepassingen.

**Stap 1:** Downloaden van het project uit [DocumentDB prestaties testen voorbeeld](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)of zich op de bibliotheek Github splitsen.

**Stap 2:** De instellingen voor EndpointUrl, AuthorizationKey, CollectionThroughput en DocumentTemplate (optioneel) in App.config wijzigen.

> [AZURE.NOTE] Raadpleeg vóór de inrichting van siteverzamelingen met hoge doorvoer, naar de [Pagina prijzen](https://azure.microsoft.com/pricing/details/documentdb/) voor het schatten van de kosten per siteverzameling. DocumentDB facturen opslag en tijdens het onafhankelijk per uur worden berekend, zodat u kunt kosten opslaan door te verwijderen of de doorvoer van uw siteverzamelingen DocumentDB verlagen na testen.

**Stap 3:** Compileren en de console-app uitvoeren vanaf de opdrachtregel. Hier ziet u de volgende uitvoer:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Stap 4 (indien nodig):** De doorvoer gerapporteerd (RU/s) van de functie moet hetzelfde of hoger zijn dan de ingerichte doorvoer van de siteverzameling. Als dat niet zo is, vergroten de DegreeOfParallelism in kleine stappen, kunt u de limiet hebt bereikt. Als de doorvoer uit uw client-app plateaus, kunt starten van meerdere exemplaren van de app op de dezelfde of een andere computers u de ingerichte limiet over de verschillende exemplaren hebt bereikt. Als u hulp nodig bij deze stap hebt, schrijf een e-mailbericht naar askdocdb@microsoft.com of opvulling van een ondersteuningsticket.

Nadat u de app actief hebt, kunt u verschillende [Indexing beleidsregels](documentdb-indexing-policies.md) en [consistentie niveaus](documentdb-consistency-levels.md) begrip van hun invloed op doorvoer en latentie proberen. U kunt ook bekijken van de broncode en soortgelijke configuraties voor uw eigen testsuites of -productietoepassingen implementeren.

## <a name="next-steps"></a>Volgende stappen
In dit artikel die we kijken hoe u de prestaties en schaal testen met DocumentDB met een .NET-console-app kunt uitvoeren. Zie de onderstaande koppelingen voor meer informatie over het werken met DocumentDB.

* [DocumentDB prestaties testen steekproef](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Client-configuratieopties DocumentDB prestaties te verbeteren](documentdb-performance-tips.md)
* [Servers partitioneren in DocumentDB](documentdb-partition-data.md)
* [DocumentDB verzamelingen en prestaties](documentdb-performance-levels.md)
* [DocumentDB .NET SDK-documentatie op MSDN](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [Voorbeelden van DocumentDB .NET](https://github.com/Azure/azure-documentdb-net)
* [Blog DocumentDB op tips voor betere prestaties](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)
