<properties 
    pageTitle="DocumentDB Python API & SDK | Microsoft Azure" 
    description="Alle informatie over de Python API en SDK, inclusief release begindatums, buitengebruikstelling en wijzigingen hebt aangebracht tussen elke versie van de DocumentDB Python SDK." 
    services="documentdb" 
    documentationCenter="python" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API's en SDK 's

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-python-api-and-sdk"></a>DocumentDB Python API en SDK

<table>
<tr><td>**SDK downloaden**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>
<tr><td>**API-documentatie**</td><td>[Python API-documentatie](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>
<tr><td>**Installatie-instructies SDK**</td><td>[Installatie-instructies Python SDK](http://azure.github.io/azure-documentdb-python/)</td></tr>
<tr><td>**Bijdragen aan SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>
<tr><td>**Aan de slag**</td><td>[Aan de slag met de SDK Python](documentdb-python-application.md)</td></tr>
<tr><td>**Huidige ondersteunde platform**</td><td>[Python 2.7](https://www.python.org/downloads/) en [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name200200httpspypipythonorgpypipydocumentdb200"></a><a name="2.0.0"/>[2.0.0](https://pypi.python.org/pypi/pydocumentdb/2.0.0)
- Ondersteuning voor Python 3.5 is toegevoegd.
- Ondersteuning voor groepsgewijze met een module aanvragen toegevoegd.
- Ondersteuning voor consistentie van de sessie toegevoegd.
- Ondersteuning voor bovenste/ORDERBY query's voor gepartitioneerde verzamelingen toegevoegd.


### <a name="a-name190190httpspypipythonorgpypipydocumentdb190"></a><a name="1.9.0"/>[1.9.0](https://pypi.python.org/pypi/pydocumentdb/1.9.0)
- Toegevoegde opnieuw Beleidsondersteuning voor vertraagd aanvragen. (Vertraagd aanvragen ontvangt een aanvraag tarief te groot uitzondering, foutcode 429.) Standaard DocumentDB pogingen negen tijden voor elke aanvraag als foutcode 429 wordt aangetroffen, behouden blijft de retryAfter tijd in de kop van de reactie. Een tijdsinterval vaste opnieuw kan nu worden ingesteld als onderdeel van de eigenschap RetryOptions op het object ConnectionPolicy als u wilt de retryAfter-tijd die het resultaat van de server tussen de pogingen om opnieuw te negeren. DocumentDB nu wacht maximaal 30 seconden voor elk die aanvragen is wordt vertraagd (ongeacht het aantal nieuwe pogingen) en retourneert het antwoord met foutcode 429. Dit moment kan ook worden overschreven in de eigenschap RetryOptions op ConnectionPolicy object.

- DocumentDB geeft nu x-ms-restrictie-aantal nieuwe pogingen en x-ms-throttle-retry-wait-time-ms terwijl de kopteksten antwoord in elk verzoek om aan te geven van de beperking opnieuw aantal en het cummulative tijd die het verzoek tussen de nieuwe pogingen gewacht.

- De klasse RetryPolicy en de bijbehorende eigenschap (retry_policy) weergegeven in de klas document_client verwijderd en kennis in plaats daarvan een RetryOptions klasse dat de eigenschap RetryOptions op ConnectionPolicy class die kan worden gebruikt om te negeren enkele van de standaardopties voor het opnieuw.

### <a name="a-name180180httpspypipythonorgpypipydocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://pypi.python.org/pypi/pydocumentdb/1.8.0)
  - De ondersteuning voor meerdere landen/regio database accounts toegevoegd.

### <a name="a-name170170httpspypipythonorgpypipydocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://pypi.python.org/pypi/pydocumentdb/1.7.0)
- De ondersteuning voor de functie tijd naar Live(TTL) voor documenten toegevoegd.

### <a name="a-name161161httpspypipythonorgpypipydocumentdb161"></a><a name="1.6.1"/>[1.6.1](https://pypi.python.org/pypi/pydocumentdb/1.6.1)
- Correcties voor het server kant partitioneren in staat wilt stellen speciale tekens partitionkey pad.

### <a name="a-name160160httpspypipythonorgpypipydocumentdb160"></a><a name="1.6.0"/>[1.6.0](https://pypi.python.org/pypi/pydocumentdb/1.6.0)
- Geïmplementeerd [gepartitioneerde verzamelingen](documentdb-partition-data.md) en [de gebruiker gedefinieerde prestaties](documentdb-performance-levels.md). 

### <a name="a-name150150httpspypipythonorgpypipydocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://pypi.python.org/pypi/pydocumentdb/1.5.0)
- Hash & bereik toevoegen partitioneren resolvers om u te helpen met sharding toepassingen op verschillende partities.

### <a name="a-name142142httpspypipythonorgpypipydocumentdb142"></a><a name="1.4.2"/>[1.4.2](https://pypi.python.org/pypi/pydocumentdb/1.4.2)
- Implementeer Upsert. Nieuwe UpsertXXX methoden toegevoegd aan de ondersteuning voor de functie Upsert.
- Implementeren ID gebaseerd mailroutering. Geen openbare API-wijzigingen, alle wijzigingen interne.

### <a name="a-name120120httpspypipythonorgpypipydocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://pypi.python.org/pypi/pydocumentdb/1.2.0)
- Ondersteunt georuimtelijke index.
- De eigenschap id voor alle resources is gevalideerd. Id's voor resources mogen geen bevatten ?, /, #, \, tekens of end op een spatie.
- Nieuwe kop "index transformatie voortgang' toevoegen aan ResourceResponse

### <a name="a-name110110httpspypipythonorgpypipydocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://pypi.python.org/pypi/pydocumentdb/1.1.0)
- V2 indexing beleid implementeert.

### <a name="a-name101101httpspypipythonorgpypipydocumentdb101"></a><a name="1.0.1"/>[1.0.1](https://pypi.python.org/pypi/pydocumentdb/1.0.1)
- Proxyverbinding ondersteunt.

### <a name="a-name100100httpspypipythonorgpypipydocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://pypi.python.org/pypi/pydocumentdb/1.0.0)
- GA SDK.

## <a name="release--retirement-dates"></a>Release en buitengebruikstelling datums
Microsoft biedt melding ten minste **12 maanden** vóór het intrekken van een SDK om te kunnen vloeiend de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteit en optimalisaties worden alleen toegevoegd aan de huidige SDK, als zodanig dit is het beste altijd eerst naar de nieuwste SDK versie zo snel mogelijk. 

Een aanvraag om deel te DocumentDB met behulp van een teruggetrokken SDK geweigerd door de service.

> [AZURE.WARNING]
Alle versies van de Azure DocumentDB SDK voor Python vóór versie **1.0.0** wordt ingetrokken op **29 februari 2016**. 

<br/>

| Versie | Releasedatum | Einddatum 
| ---     | ---          | ---
| [2.0.0](#2.0.0) | 29 september 2016 |---
| [1.9.0](#1.9.0) | 07 juli 2016 |---
| [1.8.0](#1.8.0) | 14 juni 2016 |---
| [1.7.0](#1.7.0) | 26 april 2016 |---
| [1.6.1](#1.6.1) | 08 april 2016 |---
| [1.6.0](#1.6.0) | 29 maart 2016 |---
| [1.5.0](#1.5.0) | 03 januari 2016 |---
| [1.4.2](#1.4.2) | 06 oktober 2015 |---
| [1.4.1](#1.4.1) | 06 oktober 2015 |---
| [1.2.0](#1.2.0) | 06 augustus 2015 |---
| [1.1.0](#1.1.0) | 09 juli 2015 |---
| [1.0.1](#1.0.1) | 25 mei 2015 |---
| [1.0.0](#1.0.0) | 07 april 2015 |---
| 0.9.4-prelease | 14 januari 2015 | 29 februari 2016
| 0.9.3-prelease | 09 december 2014 | 29 februari 2016
| 0.9.2-prelease | 25 november 2014 | 29 februari 2016
| 0.9.1-prelease | 23 september 2014 | 29 februari 2016
| 0.9.0-prelease | 21 augustus 2014 | 29 februari 2016

## <a name="faq"></a>FAQ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Zie ook

Zie voor meer informatie over DocumentDB, [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) servicepagina. 
