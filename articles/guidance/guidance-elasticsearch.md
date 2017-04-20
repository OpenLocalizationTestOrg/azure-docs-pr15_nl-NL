
<properties
   pageTitle="Elasticsearch Azure instructies op | Microsoft Azure"
   description="Elasticsearch Azure instructies op."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>

# <a name="elasticsearch-on-azure-guidance"></a>Elasticsearch Azure instructies op 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Elasticsearch is een zeer scalable open source zoekprogramma en de database. Het is handig voor situaties waarin snelle analyse en discovery van informatie in grote gegevenssets. Deze instructies kijkt naar enkele belangrijke aspecten u rekening moet houden bij het ontwerpen van een cluster Elasticsearch, met een focus op manieren om te optimaliseren en test uw configuratie.

> [AZURE.NOTE]Deze instructies wordt ervan uitgegaan enkele eenvoudige bekend zijn met Elasticsearch. Ga naar de [Elasticsearch-website](https://www.elastic.co/products/elasticsearch)voor meer informatie. 

- **[Elasticsearch uitgevoerd op Azure][]** bevat een korte inleiding tot de algemene structuur van Elasticsearch en wordt beschreven hoe u een Elasticsearch cluster met Azure implementeren. 
- **[Afstemmen gegevens opname prestaties voor Elasticsearch op Azure][]** beschrijving van de implementatie van een cluster Elasticsearch en uitgebreide analyse van de verschillende manieren u rekening moet houden wanneer u een grote hoeveelheid gegevens opname verwachten.
- **[Afstemmen gegevens aggregatie en prestaties van query's voor Elasticsearch op Azure][]** biedt een uitgebreide analyse van de opties om te houden bij het bepalen van uw systeem voor de query en zoeken prestaties optimaliseren.
- **[Configuratie van flexibiliteit en herstelbestanden op Elasticsearch op Azure][]** worden enkele belangrijke functies van een Elasticsearch cluster waarmee u kunt de kans op van gegevensverlies en uitgebreide gegevens herstel tijden minimaliseren.
- **[Maken van een prestaties testomgeving voor Elasticsearch op Azure][]** wordt beschreven hoe voor het instellen van een omgeving voor het testen van de opname van prestaties gegevens en de query werkbelasting in een cluster Elasticsearch. 
- **[Uitvoering van een JMeter testplan voor Elasticsearch][]** bevat een overzicht van prestatietests die worden uitgevoerd met JMeter test abonnementen samen met Java-code verwerkt als een JUnit-toets voor de uitvoering van taken zoals het uploaden van gegevens in het cluster Elasticsearch geïmplementeerd.
- **[Een pipet JMeter JUnit voor het testen van Elasticsearch prestaties implementeert][]** wordt beschreven hoe maken en gebruiken van een JUnit pipet waarmee u kunt genereren en gegevens uploaden naar een cluster Elasticsearch. Dit biedt een zeer flexibele aanpak om te testen laden die grote hoeveelheden testgegevens zonder afhankelijk van externe gegevensbestanden kunt genereren. 
- **[De geautomatiseerd Elasticsearch tolerantie tests uitgevoerd][]** , ziet u hoe de tolerantie tests die worden gebruikt in deze reeks uitvoeren. 
- **[De automatische Elasticsearch prestatietests uitgevoerd][]** , ziet u hoe de van prestatietests die worden gebruikt in deze reeks uitvoeren.


[Actieve Elasticsearch op Azure]: guidance-elasticsearch-running-on-azure.md
[Gegevens opname prestaties voor Elasticsearch op Azure optimaliseren]: guidance-elasticsearch-tuning-data-ingestion-performance.md
[Maken van een prestaties omgeving voor Elasticsearch op Azure testen]: guidance-elasticsearch-creating-performance-testing-environment.md
[Een testplan JMeter implementeren voor Elasticsearch]: guidance-elasticsearch-implementing-jmeter-test-plan.md
[Een pipet JMeter JUnit implementeren voor het testen van Elasticsearch prestaties]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
[Gegevens samenvoegen en prestaties van query's voor Elasticsearch op Azure optimaliseren]: guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md
[Flexibiliteit en herstel configureren op Elasticsearch op Azure]: guidance-elasticsearch-configuring-resilience-and-recovery.md
[De geautomatiseerde Elasticsearch tolerantie Tests uitvoeren]: guidance-elasticsearch-running-automated-resilience-tests.md
[De geautomatiseerde Elasticsearch prestatietests uitvoeren]: guidance-elasticsearch-running-automated-performance-tests.md
