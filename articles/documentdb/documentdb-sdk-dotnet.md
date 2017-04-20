<properties 
    pageTitle="DocumentDB .NET API & SDK | Microsoft Azure" 
    description="Alle informatie over de .NET-API en SDK, inclusief release begindatums, buitengebruikstelling en wijzigingen die zijn gedaan tussen elke versie van de DocumentDB .NET SDK." 
    services="documentdb" 
    documentationCenter=".net" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API's en SDK 's 

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-net-api-and-sdk"></a>DocumentDB .NET-API en SDK

<table>
<tr><td>**SDK downloaden**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>
<tr><td>**API-documentatie**</td><td>[.NET-API-documentatie](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>
<tr><td>**Voorbeelden**</td><td>[Voorbeelden van .NET-code](documentdb-dotnet-samples.md)</td></tr>
<tr><td>**Aan de slag**</td><td>[Aan de slag met de DocumentDB .NET SDK](documentdb-get-started.md)</td></tr>
<tr><td>**Zelfstudie van web-app**</td><td>[Ontwikkeling van webtoepassingen met DocumentDB](documentdb-dotnet-application.md)</td></tr>
<tr><td>**Huidige ondersteunde framework**</td><td>[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Releaseopmerkingen

> [AZURE.IMPORTANT] Beginnen met versie 1.9.2, foutbericht System.NotSupportedException bij het gepartitioneerde verzamelingen opvragen. Als u wilt voorkomen dat deze fout, moet u ervoor zorgen dat uw hostproces 64-bits is. Uitvoerbaar projecten, kan dit doen door de optie 'Voorkeur voor 32-bits' in het eigenschappenvenster voor project op het tabblad opbouwen uit te schakelen.

### <a name="a-name11001100httpswwwnugetorgpackagesmicrosoftazuredocumentdb1100"></a><a name="1.10.0"/>[1.10.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.10.0)

  - Toegevoegde directe connectivity-ondersteuning voor gepartitioneerde siteverzamelingen.
  - Verbeterde prestaties voor het niveau van de consistentie Staleness gebonden.
  - Toegevoegde veelhoek en LineString DataTypes tijdens het geven van de siteverzameling indexeren beleid voor geografische hekwerk ruimte query's.
  - LINQ ondersteuning toegevoegd voor StringEnumConverter, IsoDateTimeConverter en UnixDateTimeConverter tijdens het omzetten van predicaten.
  - Verschillende SDK correcties.

### <a name="a-name195195httpswwwnugetorgpackagesmicrosoftazuredocumentdb195"></a><a name="1.9.5"/>[1.9.5](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.5)

  - Een probleem waardoor de volgende NotFoundException opgelost: de lees-sessie is niet beschikbaar voor de invoer sessietoken. Deze uitzondering opgetreden in sommige gevallen bij het opvragen van de alleen-regio van een account geografische verdeeld.
  - De eigenschap ResponseStream in de klas ResourceResponse, waarmee u rechtstreeks toegang naar de onderliggende stroom van een reactie weergegeven.

### <a name="a-name194194httpswwwnugetorgpackagesmicrosoftazuredocumentdb194"></a><a name="1.9.4"/>[1.9.4](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.4)

  - De ResourceResponse, FeedResponse, StoredProcedureResponse en MediaResponse klassen implementatie van de bijbehorende openbare interface, zodat ze kunnen worden mocked voor test basis van hoeveelheid werk implementatie (TDD) gewijzigd.
  - Een probleem met de kop van een onjuiste partition waardoor weergegeven wanneer u een aangepast JsonSerializerSettings-object voor serialiseren gegevens opgelost.

### <a name="a-name193193httpswwwnugetorgpackagesmicrosoftazuredocumentdb193"></a><a name="1.9.3"/>[1.9.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.3)

  - Een probleem waardoor langdurige query's mislukt met fout opgelost: Autorisatietoken op dit moment niet geldig is.
  - Vaste een probleem dat de oorspronkelijke SqlParameterCollection uit cross partition boven/volgorde-door query's verwijderd.

### <a name="a-name192192httpswwwnugetorgpackagesmicrosoftazuredocumentdb192"></a><a name="1.9.2"/>[1.9.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.2)

  - Ondersteuning voor parallelle query's voor gepartitioneerde verzamelingen toegevoegd.
  - Ondersteuning voor cross partition ORDER BY en TOP-query's voor gepartitioneerde siteverzamelingen.
  - De ontbrekende verwijzingen naar DocumentDB.Spatial.Sql.dll en Microsoft.Azure.Documents.ServiceInterop.dll die vereist zijn wanneer u verwijst naar een DocumentDB project met een verwijzing naar het pakket DocumentDB Nuget opgelost.
  - De mogelijkheid om te gebruiken van parameters van verschillende typen bij gebruik van de gebruiker gedefinieerde functies in LINQ opgelost. 
  - Een fout voor globaal gerepliceerde accounts vaste waar Upsert oproepen zijn wordt doorgestuurd naar locaties in plaats van schrijven locaties lezen.
  - Toegevoegde methoden voor de interface IDocumentClient die ontbrekende waren: 
      - UpsertAttachmentAsync methode waarmee mediaStream en opties als parameters
      - CreateAttachmentAsync-methode die u opties als u een parameter neemt
      - De methode CreateOfferQuery waarmee querySpec als een parameter.
  - Over niet-verzegelde openbare klassen die beschikbaar zijn in de interface IDocumentClient.

### <a name="a-name180180httpswwwnugetorgpackagesmicrosoftazuredocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.8.0)
  - De ondersteuning voor meerdere landen/regio database accounts toegevoegd.
  - Ondersteuning voor opnieuw op vertraagd aanvragen.  Gebruiker kunt u het aantal pogingen en de maximale wachttijd aanpassen door het configureren van de eigenschap ConnectionPolicy.RetryOptions.
  - Een nieuwe IDocumentClient-interface waarin de handtekeningen van alle DocumenClient eigenschappen en methoden toegevoegd.  Als onderdeel van deze wijziging, moet u ook extensie methoden die IQueryable en IOrderedQueryable naar methoden op de DocumentClient klasse zelf maken gewijzigd.
  - Configuratieoptie voor het instellen van de ServicePoint.ConnectionLimit voor een bepaald DocumentDB eindpunt Uri toegevoegd.  Gebruik ConnectionPolicy.MaxConnectionLimit om te wijzigen van de standaardwaarde en die is ingesteld op 50.
  - Vervallen IPartitionResolver en de implementatie.  Ondersteuning voor IPartitionResolver is nu verouderd. Het wordt aanbevolen partitioneren verzamelingen te gebruiken voor hoger opslagruimte en doorvoer.


### <a name="a-name171171httpswwwnugetorgpackagesmicrosoftazuredocumentdb171"></a><a name="1.7.1"/>[1.7.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.1)
  - Toegevoegd aan dat een overbelasting naar Uri op basis van ExecuteStoredProcedureAsync methode waarmee RequestOptions als een parameter.
  
### <a name="a-name170170httpswwwnugetorgpackagesmicrosoftazuredocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.0)
  - Toegevoegde tijd naar live (TTL)-ondersteuning voor documenten.

### <a name="a-name163163httpswwwnugetorgpackagesmicrosoftazuredocumentdb163"></a><a name="1.6.3"/>[1.6.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.3)
  - Vaste een fout in de verpakking Nuget van .NET SDK voor het verpakken als onderdeel van een oplossing Azure-Cloudservice.
  
### <a name="a-name162162httpswwwnugetorgpackagesmicrosoftazuredocumentdb162"></a><a name="1.6.2"/>[1.6.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.2)
  - Geïmplementeerd [gepartitioneerde verzamelingen](documentdb-partition-data.md) en [de gebruiker gedefinieerde prestaties](documentdb-performance-levels.md). 

### <a name="a-name153153httpswwwnugetorgpackagesmicrosoftazuredocumentdb153"></a><a name="1.5.3"/>[1.5.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.3)
  - **[Vaste]** Query's uitvoeren DocumentDB eindpunt genereert: ' System.Net.Http.HttpRequestException: Error while inhoud kan kopiëren naar een stroom.

### <a name="a-name152152httpswwwnugetorgpackagesmicrosoftazuredocumentdb152"></a><a name="1.5.2"/>[1.5.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.2)
  - Uitgevouwen LINQ ondersteunen waaronder nieuwe operatoren voor paginering, voorwaardelijke expressies en bereik vergelijking.
    - Operator om in te schakelen Selecteer boven gedrag in LINQ uitvoeren
    - De operator CompareTo bereik tekenreeksvergelijkingen inschakelen
    - Voorwaardelijke (?) en query operatoren (?)
  - **[Vaste]** ArgumentOutOfRangeException bij het combineren van Model raming met waar In in linq query.  [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151httpswwwnugetorgpackagesmicrosoftazuredocumentdb151"></a><a name="1.5.1"/>[1.5.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.1)
 - **[Vaste]** Als selecteert u niet de laatste expressie wordt de Provider LINQ geen raming uitgegaan en selecteer geproduceerd * onjuist worden gegroepeerd.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150httpswwwnugetorgpackagesmicrosoftazuredocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.0)
 - Geïmplementeerd Upsert, UpsertXXXAsync toegevoegd methoden
 - Prestatieverbeteringen voor alle aanvragen
 - LINQ Provider ondersteuning voor voorwaardelijke, query en CompareTo methoden voor tekenreeksen
 - **[Vaste]** LINQ provider--> implementeren bevat de methode die u in de lijst te genereren van de dezelfde SQL als op IEnumerable en matrix
 - **[Vaste]** De dezelfde HttpRequestMessage BackoffRetryUtility opnieuw gebruikt in plaats van een nieuwe record op opnieuw maken
 - **[Verouderde]** UriFactory.CreateCollection--> moet nu UriFactory.CreateDocumentCollection gebruiken
 
### <a name="a-name141141httpswwwnugetorgpackagesmicrosoftazuredocumentdb141"></a><a name="1.4.1"/>[1.4.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.1)
 - **[Vaste]** Lokalisatie bij gebruik van niet-nl cultuur info zoals nl-NL enzovoort. 
 
### <a name="a-name140140httpswwwnugetorgpackagesmicrosoftazuredocumentdb140"></a><a name="1.4.0"/>[1.4.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.0)
  - ID gebaseerd routering
    - Nieuwe UriFactory helper om u te helpen met-ID die maken op basis van resource koppelingen
    - Nieuwe overbelastingen op DocumentClient maken in URI
  - IsValid() en IsValidDetailed() in LINQ voor georuimtelijke toegevoegd
  - Verbeterde ondersteuning voor LINQ Provider
    - **Wiskundige** - Abs, BOOGCOS, BOOGSIN, BOOGTAN, maximum, Cos, Exp, Floor, logboek, Log10, Pow, ronde, aanmelden, Sin, WORTEL, Tan, afkappen
    - **Tekenreeks** - samenvoegen, bevat, EndsWith, IndexOf, aantal, ToLower, TrimStart, vervangen, omkeren, TrimEnd, StartsWith, subtekenreeks, ToUpper
    - **Matrix** - samenvoegen, bevat, tellen
    - Operator **IN**

### <a name="a-name130130httpswwwnugetorgpackagesmicrosoftazuredocumentdb130"></a><a name="1.3.0"/>[1.3.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.3.0)
  - Ondersteuning voor indexing beleid wijzigen
    - Nieuwe ReplaceDocumentCollectionAsync methode in DocumentClient
    - Nieuwe IndexTransformationProgress eigenschap in ResourceResponse<T> voor het percentage van de voortgang van index beleidswijzigingen bijhouden
    - DocumentCollection.IndexingPolicy is nu veranderlijke
  - Ondersteuning voor ruimte indexering en query
    - Nieuwe Microsoft.Azure.Documents.Spatial naamruimte voor ruimte typen serialiseren/deserialiseren, zoals komma en Veelhoek
    - Nieuwe SpatialIndex klasse voor GeoJSON-gegevens die zijn opgeslagen in DocumentDB indexeren
  - **[Vaste]** : onjuiste SQL-query die zijn gegenereerd op basis van linq expressie [#38](https://github.com/Azure/azure-documentdb-net/issues/38)

### <a name="a-name120120httpswwwnugetorgpackagesmicrosoftazuredocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.2.0)
- Afhankelijkheid van Newtonsoft.Json v5.0.7 
- Wijzigingen voor het ondersteunen Order By
  - Ondersteuning voor LINQ-provider voor OrderBy() of OrderByDescending()
  - IndexingPolicy ter ondersteuning van Order By 
  
        **NB: Possible breaking change** 
  
        If you have existing code that provisions collections with a custom indexing policy, then your existing code will need to be updated to support the new IndexingPolicy class. If you have no custom indexing policy, then this change does not affect you.

### <a name="a-name110110httpswwwnugetorgpackagesmicrosoftazuredocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.1.0)
- Ondersteuning voor gegevens met behulp van de nieuwe HashPartitionResolver en RangePartitionResolver klassen en de IPartitionResolver partitioneren
- DataContract serialisatie
- GUID-ondersteuning in LINQ-provider
- UDF-ondersteuning in LINQ

### <a name="a-name100100httpswwwnugetorgpackagesmicrosoftazuredocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.0.0)
- GA SDK

> [AZURE.NOTE]
Er is een wijziging van de pakketnaam NuGet tussen preview en GA. We verplaatst van **Microsoft.Azure.Documents.Client** naar **Microsoft.Azure.DocumentDB**
<br/>


### <a name="a-name09x-preview09x-previewhttpswwwnugetorgpackagesmicrosoftazuredocumentsclient"></a><a name="0.9.x-preview"/>[0.9.x-Preview](https://www.nuget.org/packages/Microsoft.Azure.Documents.Client)
- Voorbeeld SDK's [verouderde]

## <a name="release--retirement-dates"></a>Release en buitengebruikstelling datums
Microsoft biedt melding ten minste **12 maanden** vóór het intrekken van een SDK om te kunnen vloeiend de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteit en optimalisaties worden alleen toegevoegd aan de huidige SDK, als zodanig wordt aanbevolen dat u altijd upgraden naar de meest recente SDK versie zo snel mogelijk. 

Een aanvraag om deel te DocumentDB met behulp van een teruggetrokken SDK geweigerd door de service.

> [AZURE.WARNING]
Alle versies van de Azure DocumentDB SDK voor .NET vóór versie **1.0.0** wordt ingetrokken op **29 februari 2016**. 
 
<br/>
 
| Versie | Releasedatum | Einddatum 
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 27 september 2016 |---
| [1.9.5](#1.9.5) | 01 september 2016 |---
| [1.9.4](#1.9.4) | 24 augustus 2016 |---
| [1.9.3](#1.9.3) | 15 augustus 2016 |---
| [1.9.2](#1.9.2) | 23 juli 2016 |---
| 1.9.1 | Afgeschaft |---
| 1.9.0 | Afgeschaft |---
| [1.8.0](#1.8.0) | 14 juni 2016 |---
| [1.7.1](#1.7.1) | 06 mei 2016 |---
| [1.7.0](#1.7.0) | 26 april 2016 |---
| [1.6.3](#1.6.3) | 08 april 2016 |---
| [1.6.2](#1.6.2) | 29 maart 2016 |---
| [1.5.3](#1.5.3) | 19 februari 2016 |---
| [1.5.2](#1.5.2) | 14 december 2015 |---
| [1.5.1](#1.5.1) | 23 november 2015 |---
| [1.5.0](#1.5.0) | 05 oktober 2015 |---
| [1.4.1](#1.4.1) | 25 augustus 2015 |---
| [1.4.0](#1.4.0) | 13 augustus 2015 |---
| [1.3.0](#1.3.0) | 05 augustus 2015 |---
| [1.2.0](#1.2.0) | 06 juli 2015 |---
| [1.1.0](#1.1.0) | 30 april 2015 |---
| [1.0.0](#1.0.0) | 08 april 2015 |---
| [0.9.3-prelease](#0.9.x-preview) | 12 maart 2015 | 29 februari 2016
| [0.9.2-prelease](#0.9.x-preview) | Januari 2015 | 29 februari 2016
| [.9.1-prelease](#0.9.x-preview) | 13 oktober 2014 | 29 februari 2016
| [0.9.0-prelease](#0.9.x-preview) | 21 augustus 2014 | 29 februari 2016

## <a name="faq"></a>FAQ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Zie ook

Zie voor meer informatie over DocumentDB, [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) servicepagina. 
