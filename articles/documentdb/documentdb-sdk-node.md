<properties
    pageTitle="DocumentDB Node.js API & SDK | Microsoft Azure"
    description="Alle informatie over de Node.js API en SDK, inclusief release begindatums, buitengebruikstelling en wijzigingen hebt aangebracht tussen elke versie van de DocumentDB Node.js SDK."
    services="documentdb"
    documentationCenter="nodejs"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API's en SDK 's

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

##<a name="documentdb-nodejs-api-and-sdk"></a>DocumentDB Node.js API en SDK

<table>
<tr><td>**SDK downloaden**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>
<tr><td>**API-documentatie**</td><td>[Node.js API-documentatie](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>
<tr><td>**Installatie-instructies SDK**</td><td>[Installatie-instructies](http://azure.github.io/azure-documentdb-node/)</td></tr>
<tr><td>**Bijdragen aan SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>
<tr><td>**Voorbeelden**</td><td>[Voorbeelden van de code node.js](documentdb-nodejs-samples.md)</td></tr>
<tr><td>**Get-slag zelfstudie**</td><td>[Aan de slag met de SDK Node.js](documentdb-nodejs-get-started.md)</td></tr>
<tr><td>**Zelfstudie van web-app**</td><td>[Een Node.js webtoepassing met DocumentDB maken](documentdb-nodejs-application.md)</td></tr>
<tr><td>**Huidige ondersteunde platform**</td><td>[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/)<br/>[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/>[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)</td></tr>
</table></br>

##<a name="release-notes"></a>Releaseopmerkingen

###<a name="1.10.0"/>1.10.0</a>

- Ondersteuning voor cross partition parallelle query's.
- Ondersteuning voor bovenste/ORDER BY query's voor gepartitioneerde verzamelingen toegevoegd.

###<a name="1.9.0"/>1.9.0</a>

- Toegevoegde opnieuw Beleidsondersteuning voor vertraagd aanvragen. (Vertraagd aanvragen ontvangt een aanvraag tarief te groot uitzondering, foutcode 429.) Standaard DocumentDB pogingen negen tijden voor elke aanvraag als foutcode 429 wordt aangetroffen, behouden blijft de retryAfter tijd in de kop van de reactie. Een tijdsinterval vaste opnieuw kan nu worden ingesteld als onderdeel van de eigenschap RetryOptions op het object ConnectionPolicy als u wilt de retryAfter-tijd die het resultaat van de server tussen de pogingen om opnieuw te negeren. DocumentDB nu wacht maximaal 30 seconden voor elk die aanvragen is wordt vertraagd (ongeacht het aantal nieuwe pogingen) en retourneert het antwoord met foutcode 429. Dit moment kan ook worden overschreven in de eigenschap RetryOptions op ConnectionPolicy object.

- DocumentDB geeft nu x-ms-restrictie-aantal nieuwe pogingen en x-ms-throttle-retry-wait-time-ms terwijl de kopteksten antwoord in elk verzoek om aan te geven van de beperking opnieuw aantal en het cummulative tijd die het verzoek tussen de nieuwe pogingen gewacht.

- De RetryOptions-klasse is toegevoegd, dat de eigenschap RetryOptions in de klas ConnectionPolicy die kan worden gebruikt om te negeren enkele van de standaardopties voor het opnieuw.

###<a name="1.8.0"/>1.8.0</a>

 - De ondersteuning voor meerdere landen/regio database accounts toegevoegd.

###<a name="1.7.0"/>1.7.0</a>

- De ondersteuning voor de functie tijd naar Live(TTL) voor documenten toegevoegd.

###<a name="1.6.0"/>1.6.0</a>

- Geïmplementeerd [gepartitioneerde verzamelingen](documentdb-partition-data.md) en [de gebruiker gedefinieerde prestaties](documentdb-performance-levels.md).

###<a name="1.5.6"/>1.5.6 worden uitgezet</a>

- Vaste RangePartitionResolver.resolveForRead bug waar heeft dit geen koppelingen vanwege een ongeldige samenvoegen met resultaten geretourneerd.

###<a name="1.5.5"/>1.5.5</a>

- Vaste hashParitionResolver resolveForRead(): wanneer geen partition toets opgegeven uitzondering, in plaats van een lijst met alle geregistreerde koppelingen retourneren is genereren.

###<a name="1.5.4"/>1.5.4</a>

- Reparaties actie [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - specifiek HTTPS-Agent: voorkomen dat de globale-agent voor DocumentDB doeleinden te wijzigen. Agent van een speciale gebruiken voor alle aanvragen van de bibliotheek.

###<a name="1.5.3"/>1.5.3</a>

- Reparaties actie [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - correct greep streepjes media-ID's.

###<a name="1.5.2"/>1.5.2</a>

- Reparaties actie [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - EventEmitter luisteraar ervan af lekken waarschuwing.

###<a name="1.5.1"/>1.5.1</a>

- Reparaties verlenen [#92](https://github.com/Azure/azure-documentdb-node/issues/90) - naam map Hash aan hash voor hoofdlettergevoelig systemen.

### <a name="1.5.0"/>1.5.0</a>

- Implementeer sharding ondersteuning toe te voegen hash & bereik partition resolvers.

### <a name="1.4.0"/>1.4.0</a>

- Implementeer Upsert. Nieuwe upsertXXX methoden op documentClient.

### <a name="1.3.0"/>1.3.0</a>

- Als u wilt uitlijnen met andere SDK's naar voren versienummers overgeslagen.

### <a name="1.2.2"/>1.2.2</a>

- Gesplitste Q belooft Wikkel naar nieuwe bibliotheek.
- Bijwerken naar pakketbestand voor npm register.

### <a name="1.2.1"/>1.2.1</a>

- Implementeert ID verzending.
- Corrigeert probleem [#49](https://github.com/Azure/azure-documentdb-node/issues/49) - huidige eigenschap conflicten met de methode current().

### <a name="1.2.0"/>1.2.0</a>

- Ondersteuning voor georuimtelijke index toegevoegd.
- De eigenschap id voor alle resources is gevalideerd. Id's voor resources mogen geen bevatten?, /, #, & #47; & #47; tekens of end op een spatie.
- Nieuwe kop "index transformatie voortgang' toevoegen aan ResourceResponse

### <a name="1.1.0"/>1.1.0</a>

- V2 indexing beleid implementeert.

### <a name="1.0.3"/>1.0.3</a>

- Probleem #40 (https://github.com/Azure/azure-documentdb-node/issues/40) - geïmplementeerd configuraties eslint en knorvis in de core en toegezegd SDK.

### <a name="1.0.2"/>1.0.2</a>

- Probleem [#45](https://github.com/Azure/azure-documentdb-node/issues/45) - pas Wikkel bevat geen koptekst met fout.

### <a name="1.0.1"/>1.0.1</a>

- De mogelijkheid tot query voor conflicten door toe te voegen readConflicts, readConflictAsync en queryConflicts is geïmplementeerd.
- Bijgewerkte API-documentatie.
- Actie [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - client.createDocumentAsync fout.

### <a name="1.0.0"/>1.0.0</a>

- GA SDK.

## <a name="release--retirement-dates"></a>Release en buitengebruikstelling datums
Microsoft biedt melding ten minste **12 maanden** vóór het intrekken van een SDK om te kunnen vloeiend de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteit en optimalisaties worden alleen toegevoegd aan de huidige SDK, als zodanig dit is het beste altijd eerst naar de nieuwste SDK versie zo snel mogelijk.

Een aanvraag om deel te DocumentDB met behulp van een teruggetrokken SDK geweigerd door de service.

> [AZURE.WARNING]
Alle versies van de Azure DocumentDB SDK voor Node.js vóór versie **1.0.0** wordt ingetrokken op **29 februari 2016**.

<br/>

| Versie | Releasedatum | Einddatum
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 03 oktober 2016 |---
| [1.9.0](#1.9.0) | 07 juli 2016 |---
| [1.8.0](#1.8.0) | 14 juni 2016 |---
| [1.7.0](#1.7.0) | 26 april 2016 |---
| [1.6.0](#1.6.0) | 29 maart 2016 |---
| [1.5.6 worden uitgezet](#1.5.6) | 08 maart 2016 |---
| [1.5.5](#1.5.5) | 02 februari 2016 |---
| [1.5.4](#1.5.4) | 01 februari 2016 |---
| [1.5.2](#1.5.2) | 26 januari 2016 |---
| [1.5.2](#1.5.2) | 22 januari 2016 |---
| [1.5.1](#1.5.1) | 4 januari 2016 |---
| [1.5.0](#1.5.0) | 31 december 2015 |---
| [1.4.0](#1.4.0) | 06 oktober 2015 |---
| [1.3.0](#1.3.0) | 06 oktober 2015 |---
| [1.2.2](#1.2.2) | 10 september 2015 |---
| [1.2.1](#1.2.1) | 15 augustus 2015 |---
| [1.2.0](#1.2.0) | 05 augustus 2015 |---
| [1.1.0](#1.1.0) | 09 juli 2015 |---
| [1.0.3](#1.0.3) | 04 juni 2015 |---
| [1.0.2](#1.0.2) | 23 mei 2015 |---
| [1.0.1](#1.0.1) | 15 mei 2015 |---
| [1.0.0](#1.0.0) | 08 april 2015 |---
| 0.9.4-prerelease | 06 april 2015 | 29 februari 2016
| 0.9.3-prerelease | 14 januari 2015 | 29 februari 2016
| 0.9.2-prerelease | 18 december 2014 | 29 februari 2016
| 0.9.1-prerelease | 22 augustus 2014 | 29 februari 2016
| 0.9.0-prerelease | 21 augustus 2014 | 29 februari 2016


## <a name="faq"></a>FAQ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Zie ook

Zie voor meer informatie over DocumentDB, [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) servicepagina.
