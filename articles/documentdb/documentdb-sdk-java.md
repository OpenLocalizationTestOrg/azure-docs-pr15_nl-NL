
<properties
    pageTitle="DocumentDB Java-API & SDK | Microsoft Azure"
    description="Alle informatie over de Java-API en SDK, inclusief release begindatums, buitengebruikstelling en wijzigingen hebt aangebracht tussen elke versie van de DocumentDB Java SDK."
    services="documentdb"
    documentationCenter="java"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API's en SDK 's

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-java-api-and-sdk"></a>DocumentDB Java-API en SDK

<table>
<tr><td>**SDK downloaden**</td><td>[Maven](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>
<tr><td>**API-documentatie**</td><td>[Java-API-documentatie](http://azure.github.io/azure-documentdb-java/)</td></tr>
<tr><td>**Bijdragen aan SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>
<tr><td>**Aan de slag**</td><td>[Aan de slag met de SDK Java](documentdb-java-application.md)</td></tr>
<tr><td>**Huidige ondersteunde runtime**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name191191httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb191"></a><a name="1.9.1"/>[1.9.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.1)

  - Ondersteuning voor BoundedStaleness consistentie niveau toegevoegd.
  - Ondersteuning voor rechtstreekse connectiviteit voor CRUD-bewerkingen voor gepartitioneerde verzamelingen toegevoegd.
  - Vaste een fout bij het zoeken van een database maken met SQL.
  - Vaste een fout in de sessiecache waarop sessietoken onjuist kan worden ingesteld.

### <a name="a-name190190httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb190"></a><a name="1.9.0"/>[1.9.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.0)

  - Ondersteuning voor cross partition parallelle query's.
  - Ondersteuning voor bovenste/ORDER BY query's voor gepartitioneerde verzamelingen toegevoegd.
  - Ondersteuning voor sterke consistentie toegevoegd.
  - Ondersteuning voor naam gebaseerde aanvragen bij gebruik van directe connectivity.
  - Vast naar ActivityId blijven consistent over alle verzoek pogingen om opnieuw te maken.
  - Vaste een fout die is gerelateerd aan de sessiecache wanneer opnieuw maken van een siteverzameling met dezelfde naam.
  - Toegevoegde veelhoek en LineString DataTypes tijdens het geven van de siteverzameling indexeren beleid voor geografische hekwerk ruimte query's.
  - Vaste problemen met Java-document voor Java 1,8.

### <a name="a-name181181httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb181"></a><a name="1.8.1"/>[1.8.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.1)
  - Vaste een fout in PartitionKeyDefinitionMap één partition verzamelingen in cache en geen extra ophalen partitioneren belangrijke aanvragen.
  - Vaste een fout bij het niet opnieuw uitgevoerd wanneer een onjuiste partition sleutelwaarde is opgegeven.

### <a name="a-name180180httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb180"></a><a name="1.8.0"/>[1.8.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.0)
  - De ondersteuning voor meerdere landen/regio database accounts toegevoegd.
  - Ondersteuning voor automatische nieuwe pogingen op vertraagd aanvragen met opties voor het aanpassen van de maximale herhalingspogingen en max wachttijd toegevoegd.  Zie RetryOptions en ConnectionPolicy.getRetryOptions().
  - Vervallen IPartitionResolver op basis van aangepaste partities code. Gebruik gepartitioneerde verzamelingen voor hogere opslag en doorvoer.

### <a name="a-name171171httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb171"></a><a name="1.7.1"/>[1.7.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.1)
- Toegevoegde opnieuw Beleidsondersteuning voor het beperken.  

### <a name="a-name170170httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb170"></a><a name="1.7.0"/>[1.7.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.0)
- Toegevoegde tijd naar live (TTL)-ondersteuning voor documenten.

### <a name="a-name160160httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb160"></a><a name="1.6.0"/>[1.6.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.6.0)
- Geïmplementeerd [gepartitioneerde verzamelingen](documentdb-partition-data.md) en [de gebruiker gedefinieerde prestaties](documentdb-performance-levels.md).

### <a name="a-name151151httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb151"></a><a name="1.5.1"/>[1.5.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.1)
- Vaste een fout in HashPartitionResolver hashwaarden om in te genereren little-endian overeenkomen met andere SDK's.

### <a name="a-name150150httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb150"></a><a name="1.5.0"/>[1.5.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.0)
- Hash & bereik toevoegen partitioneren resolvers om u te helpen met sharding toepassingen op verschillende partities.

### <a name="a-name140140httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb140"></a><a name="1.4.0"/>[1.4.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.4.0)
- Implementeer Upsert. Nieuwe upsertXXX methoden toegevoegd aan de ondersteuning voor de functie Upsert.
- Implementeren ID gebaseerd mailroutering. Geen openbare API-wijzigingen, alle wijzigingen interne.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
- Release overgeslagen om versienummer in uitlijning met andere SDK's weer te geven

### <a name="a-name120120httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb120"></a><a name="1.2.0"/>[1.2.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.2.0)
- Ondersteunt georuimtelijke Index
- De eigenschap id voor alle resources is gevalideerd. Id's voor resources mogen geen bevatten ?, /, #, \, tekens of end op een spatie.
- Nieuwe kop "index transformatie voortgang' toevoegen aan ResourceResponse

### <a name="a-name110110httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb110"></a><a name="1.1.0"/>[1.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.1.0)
- V2 indexing beleid implementeert

### <a name="a-name100100httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb100"></a><a name="1.0.0"/>[1.0.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.0.0)
- GA SDK

## <a name="release--retirement-dates"></a>Release en buitengebruikstelling datums
Microsoft biedt melding ten minste **12 maanden** vóór het intrekken van een SDK om te kunnen vloeiend de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteit en optimalisaties worden alleen toegevoegd aan de huidige SDK, als zodanig dit is het beste altijd eerst naar de nieuwste SDK versie zo snel mogelijk.

Een aanvraag om deel te DocumentDB met behulp van een teruggetrokken SDK geweigerd door de service.

> [AZURE.WARNING]
Alle versies van de Azure DocumentDB SDK for Java vóór versie **1.0.0** wordt op **29 februari 2016**worden teruggetrokken.

<br/>

| Versie | Releasedatum | Einddatum
| ---     | ---          | ---
| [1.9.1](#1.9.1) | 28 oktober 2016 |---
| [1.9.0](#1.9.0) | 03 oktober 2016 |---
| [1.8.1](#1.8.1) | 30 juni 2016 |---
| [1.8.0](#1.8.0) | 14 juni 2016 |---
| [1.7.1](#1.7.1) | 30 april 2016 |---
| [1.7.0](#1.7.0) | 27 april 2016 |---
| [1.6.0](#1.6.0) | 29 maart 2016 |---
| [1.5.1](#1.5.1) | 31 december 2015 |---
| [1.5.0](#1.5.0) | 04 december 2015 |---
| [1.4.0](#1.4.0) | 05 oktober 2015 |---
| [1.3.0](#1.3.0) | 05 oktober 2015 |---
| [1.2.0](#1.2.0) | 05 augustus 2015 |---
| [1.1.0](#1.1.0) | 09 juli 2015 |---
| [1.0.1](#1.0.1) | 12 mei 2015 |---
| [1.0.0](#1.0.0) | 07 april 2015 |---
| 0.9.5-prelease | 09 mrt 2015 | 29 februari 2016
| 0.9.4-prelease | 17 februari 2015 | 29 februari 2016
| 0.9.3-prelease | 13 januari 2015 | 29 februari 2016
| 0.9.2-prelease | 19 december 2014 | 29 februari 2016
| 0.9.1-prelease | 19 december 2014 | 29 februari 2016
| 0.9.0-prelease | 10 december 2014 | 29 februari 2016

## <a name="faq"></a>FAQ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Zie ook

Zie voor meer informatie over DocumentDB, [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) servicepagina.
