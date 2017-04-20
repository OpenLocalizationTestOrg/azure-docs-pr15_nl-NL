<properties
   pageTitle="API-versies van Azure zoeken | Microsoft Azure | Zoek-API"
   description="Beleid versie voor Azure zoeken REST API's en de clientbibliotheek in de .NET SDK."
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/16/2016"
   ms.author="brjohnst"/>

# <a name="api-versions-in-azure-search"></a>API-versies in Azure zoeken

Azure zoeken uitgevouwen functie-updates regelmatig. Soms, maar niet altijd, moeten deze updates wij een nieuwe versie van onze API publiceren om te kunnen compatibiliteit met eerdere versies behouden. Een nieuwe versie publiceren, kunt u aangeven wanneer en hoe u de service-updates zoeken in uw code integreren.

Als een regel willen we publiceren van nieuwe versies alleen indien nodig, omdat deze mogelijk moeite bijwerken van uw code als u wilt een nieuwe versie van de API gebruiken. Er wordt een nieuwe versie alleen publiceren als moeten we enkele aspecten van de API in een manier die achterwaartse compatibiliteit wijzigen. Dit kan gebeuren vanwege reparaties bij bestaande functies of vanwege nieuwe functies die worden gewijzigd van bestaande API oppervlakte.

We Volg dezelfde regel voor SDK updates. De SDK van Azure zoeken volgt de regels [semantic versiebeheer](http://semver.org/) , wat betekent dat de versie die bestaat uit drie delen: primaire, secundaire en build-nummer (bijvoorbeeld 1.1.0). We wordt een nieuwe primaire versie van de SDK alleen voor het geval de wijzigingen die problemen met achterwaartse compatibiliteit uitgebracht. We de secundaire versie opgehoogd voor vaste functie-updates, en voor correcties, wordt alleen de buildversie neemt.

##<a name="snapshot-of-current-versions"></a>Momentopname van de huidige versies 

Hieronder wordt een momentopname van de huidige versies van alle programming interface Azure zoeken. Werken met schema's en andere details vindt u in de volgende secties van dit document.

Interfaces|Meest recente primaire versie|Status
----------|-------------------------|------
[.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)|1.1|In het algemeen beschikbaar, vrijgegeven februari 2016
[.NET SDK Preview](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx)|2.0-voorbeeld|Preview is uitgegeven augustus 2016
[Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx)|2015-02-28|Algemeen beschikbaar
[Service REST API Preview](search-api-2015-02-28-preview.md)|2015-02-28-voorbeeld|Voorbeeld
[Management REST API](https://msdn.microsoft.com/library/azure/dn832684.aspx)|2015-08-19|Algemeen beschikbaar

Voor de REST API's, inclusief de `api-version` elk gesprek is vereist. Hiermee kunt u gemakkelijk een specifieke versie, zoals een voorbeeld API afstemmen. Het volgende voorbeeld ziet u hoe de `api-version` parameter is opgegeven:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2015-02-28

> [AZURE.NOTE] Hoewel u elk verzoek om een heeft een `api-version`, is raadzaam dat u dezelfde versie als die voor alle API aanvragen gebruikt. Dit geldt met name wanneer nieuwe API-versies introduceren kenmerken of bewerkingen die niet in eerdere versies worden herkend. API-versies mengen kan hebben onverwachte gevolgen en te voorkomen.
> 
De Service REST API en Management REST API zijn versienummer onafhankelijk van elkaar. Eventuele soortgelijke in versienummers is incidentele of collega.

Algemeen beschikbaar (of GA) API's kan worden gebruikt in productie en Azure serviceovereenkomsten vallen. Preview-versies hebben experiment functies die niet altijd worden gemigreerd naar een GA-versie. **We raden u ten zeerste aan het gebruik van preview API's in productietoepassingen.**

##<a name="sdk-version-roadmap"></a>Routekaart voor SDK-versie

Met elke versie van de .NET SDK is bedoeld voor een bepaalde versie van de REST API-Service. Functies zijn eerst geïmplementeerd in de REST API en klik vervolgens in de SDK geïmplementeerd.

De .NET SDK is nu algemeen beschikbaar en werk al al bezig is op de volgende versie. De volgende tabel ziet er uit vooruit voor toekomstige versies van de SDK zodat u een idee hebt van wat te staat wachten.

.NET SDK versie|REST API-versie|Functies|ETA
----------------|----------------|--------|---
1.1|2015-02-28|De syntaxis van de query Lucene|Februari 2016
2.0-voorbeeld|2015-02-28-voorbeeld|Aangepaste analyzers, Azure Blob en tabel indexeerfuncties, veldtoewijzingen ETags|Augustus 2016
2.x|Nieuwe GA API-versie|Zelfde als 2.0-voorbeeld|Vroege Q4 2016

##<a name="about-preview-and-generally-available-versions"></a>Over Preview en algemeen beschikbaar versies

Azure zoeken loslaat altijd vooraf experiment functies via de REST API eerst vervolgens tot en met voorlopige versies van de .NET SDK.

Functies zijn niet gegarandeerd moeten worden gemigreerd naar een GA-versie. Dat functies in een versie GA worden beschouwd als stabiele en waarschijnlijk wijzigen met uitzondering van kleine compatibel correcties en uitbreidingen, zijn preview-functies beschikbaar voor testen en experimenten, met het doel van het verzamelen van feedback over de functie ontwerpen en implementeren. 

Echter omdat functies kunnen gewijzigd worden, wordt aangeraden tegen schrijven productiecode waarmee een afhankelijkheid in de preview-versies. Als u een oudere versie van de Preview-versie gebruikt, wordt u aangeraden migreren naar de algemeen beschikbaar (GA)-versie. 

Voor de .NET SDK: richtlijnen voor het code migratie kunt u vinden op [de .NET SDK upgraden](search-dotnet-sdk-migration.md).

Algemene beschikbaarheid betekent dat Azure tijdens het zoeken is nu onder de serviceovereenkomst (SLA). De SLA kan worden gevonden op [Azure zoeken serviceovereenkomsten niveau](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

