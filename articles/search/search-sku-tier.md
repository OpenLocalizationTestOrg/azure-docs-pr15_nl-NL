<properties
    pageTitle="Kies een SKU of laag prijzen voor zoekprogramma's Azure | Microsoft Azure"
    description="Azure zoeken op deze SKU's kan worden ingericht: gratis, Basic en Standard, waarop standaard is beschikbaar in verschillende resource configuraties en capaciteit niveaus."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/24/2016"
    ms.author="heidist"/>

# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Kies een SKU of laag prijzen voor Azure zoeken

In Azure zoeken, een [service is ingericht](search-create-service-portal.md) op een specifieke prijzen laag of SKU. Opties voor opnemen **gratis**, **eenvoudige**of **Standard**, waarop **standaard** beschikbaar in meerdere configuraties en capaciteit is. 

Het doel van dit artikel is om te helpen u een laag te kiezen. Als een tijdschaal capaciteit blijkt te laag, moet u een nieuwe service in de hogere laag inrichten en vervolgens uw indexen opnieuw te laden. Er is geen in-place upgrade van dezelfde service uit één SKU naar de andere. 

> [AZURE.NOTE] Nadat u een laag en de [voorziening een search-service](search-create-service-portal.md)kiest, kunt u replica vergroten en partition telt binnen de service. Zie [schaal resource niveaus voor de query en indexing werkbelasting](search-capacity-planning.md)voor instructies.

## <a name="how-to-approach-a-pricing-tier-decision"></a>Benadering van een prijzen laag beslissing

In Azure zoeken bepaalt de laag capaciteit, niet de beschikbaarheid van de functie. In het algemeen wordt zijn functies beschikbaar in elke laag, met inbegrip van de preview-functies. De enige uitzondering is biedt geen ondersteuning voor indexeerfuncties in S3 HD.

> [AZURE.TIP] Het is raadzaam dat u altijd inrichten van een **gratis** service (één per abonnement, zonder vervaldatum) zodat de handbereik lichte projecten. Gebruik van de **gratis** -service voor het testen en evaluatie; Maak een tweede factureerbare service in de **Basic** - of **Standard** laag gebruiks-en grotere test werkbelasting.

Ga handje in voorraad capaciteit en kosten van het uitvoeren van de service. Informatie in dit artikel kunt u bepalen welke SKU biedt de juiste verhouding, maar voor een van deze naar het handig zijn, moet u ten minste ruw maakt een schatting van het volgende:

- Aantal en de grootte van indexen die u van plan bent om te maken
- Aantal en de grootte van documenten uploaden
- Een idee van de query volume, in query's Per tweede (QPS)

Aantal en de grootte zijn belangrijk omdat maximumlimieten tot en met een vaste limiet op het aantal indexen of documenten in een service of op resources (opslag of replica's) die worden gebruikt door de service is bereikt. De werkelijke limiet voor uw service is afhankelijk van de eerste wordt gebruikt: resources of objecten.

Met maakt een schatting in Handje, moeten de volgende stappen uit het proces vereenvoudigen:

- **Stap 1** Lees de beschrijvingen SKU hieronder voor meer informatie over de beschikbare opties.
- **Stap 2** De onderstaande aankomt op een voorlopige besluit vragen beantwoorden.
- **Stap 3** Voltooi uw beslissing door te reviseren van harde beperkingen voor de opslag en prijzen.

## <a name="sku-descriptions"></a>Beschrijvingen van de SKU

De volgende tabel vindt u beschrijvingen van elke laag. 

Laag|Primaire scenario 's
----|-----------------
**Vrij te geven**|Een gedeelde service, gratis, die wordt gebruikt voor evaluatie, onderzoek of kleine werkbelasting. Omdat deze wordt gedeeld met andere abonnees, afhankelijk query doorvoer en indexeren van wie de service wordt gebruikt. Capaciteit zo klein (50 MB of 3 indexen met omhoog 10.000 documenten elk).
**Eenvoudige**|Kleine productie werkbelasting op specifieke hardware. Zeer beschikbaar. Capaciteit is maximaal 3 replica's en 1 partition (2 GB).
**S1**|Standaard 1 ondersteunt flexibele combinaties van partities (12) en replica's (12), gebruikt voor het medium productie werkbelasting op specifieke hardware. U kunt toewijzen partities en replica's in combinaties wordt ondersteund door een maximum aantal eenheden van 36 factureerbare zoeken. Op dit niveau, partities zijn 25 GB en QPS is ongeveer 15 query's per seconde.
**S2**|Standaard 2 wordt uitgevoerd groter productie werkbelasting met dezelfde 36 Zoeken eenheden als S1, maar met grotere grote partities en replica's. Op dit niveau, partities zijn 100 GB en QPS is bijna 60 query's per seconde.
**S3**|Standaard 3 wordt uitgevoerd proportioneel groter productie werkbelasting op hogere end-systemen, in configuraties van maximaal 12 partities of 12 replica's onder 36 Zoeken eenheden. Op dit niveau, partities zijn 200 GB en QPS is meer dan 60 query's per seconde. 
**S3 HD**|Standaard 3 hoge dichtheid is bedoeld voor een groot aantal kleinere indexen. U kunt maximaal 3 partities, hebben op 200 GB. QPS is meer dan 60 query's per seconde. 

> [AZURE.NOTE] Replica en partition maximumwaarden zijn gefactureerd af als Zoeken eenheden (36 maximale per service eenheid), die u in rekening brengt een effectieve ondergrens dan het maximale aantal houdt bij nominale waarde. Als u wilt gebruiken het maximale aantal van 12 replica's, kunt u bijvoorbeeld maximaal 3 partities hebt (12 * 3 = 36 eenheden). Verminder op dezelfde manier als u wilt gebruiken maximale partities, replica's met 3. Zie [schaal resource niveaus voor de query en indexing werkbelasting in Azure zoeken](search-capacity-planning.md) voor een grafiek op toegestane combinaties.

## <a name="review-limits-per-tier"></a>Limieten per laag controleren

De volgende tabel is een subset van de limieten van [Service limieten in Azure zoeken](search-limits-quotas-capacity.md). Hiermee geeft u de factoren die meestal van invloed zijn op een besluit SKU. Wanneer u bekijkt de onderstaande vragen kunt u verwijzen naar deze grafiek.

Resource|Vrij te geven|Eenvoudige|S1|S2|S3 |S3 HD
---|---|---|---|----|---|----
Serviceovereenkomst (SLA)|Geen <sup>1</sup> |Ja |Ja  |Ja |Ja  |Ja 
Index limieten|3|5|50|200|200|1000 <sup>2</sup>
Document-limieten|10.000 in totaal|1 miljoen per service|15 miljoen per partition |60 miljoen per partition|120 miljoen per partition |1 miljoen per index
Maximum aantal partities|N/B |1 |12  |12 |12|3 <sup>2</sup>
Partitiegrootte|Totaal van 50 MB|2 GB per service|25 GB per partition |100 GB per partition (maximaal 1,2 TB per service)|200 GB per partition (maximaal 2,4 TB per service)|200 GB (maximaal 600 GB per service)
Maximum aantal replica 's|N/B |3 |12 |12 |12|12
Query's per seconde|N/B|~ 3 per replica|~ 15 per replica|~ 60 per replica|> 60 per replica|> 60 per replica

<sup>1</sup> gratis en Preview SKU's niet afkomstig zijn met serviceovereenkomsten. Sla's worden afgedwongen nadat een SKU algemeen beschikbaar.

<sup>2</sup> S3 en HD-S3 worden ondersteund door identieke hoge capaciteit infrastructuur maar elkaar de maximale limiet op verschillende manieren bereikt. S3 is bedoeld voor een kleinere aantal zeer grote indexen. Als zodanig, de maximale limiet resource afhankelijk is (2,4 TB voor elke service). S3 HD is bedoeld voor een groot aantal zeer klein indexen. S3 HD bereikt 1000 indexen, de limieten in de vorm van indexbeperkingen. Als u een S3 HD-klant bent die meer dan 1000 indexen nodig heeft, moet u contact op met Microsoft Support voor meer informatie over hoe verder te gaan.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>SKU's die niet voldoen aan vereisten verwijderen 

De volgende vragen kunt u de juiste SKU beslissing voor uw werkzaamheden aankomen.

1. Hebt u de vereisten **Service niveau overeenkomst (SLA)** ? Beperk de beschikking SKU naar eenvoudige of niet-preview standaard.
2. **Hoeveel indexen** hebt u nodig? Een van de grootste variabelen in een besluit SKU waarbij is het aantal indexen worden ondersteund door elke SKU. Index ondersteuning is op aanzienlijk verschillende niveaus in het lagere niveaus van de prijzen. Vereisten voor het aantal indexen mogelijk een primaire de determinant van een besluit SKU.
3. **Het aantal documenten** in elke index wordt geladen? Het aantal en de grootte van documenten bepaalt de uiteindelijke grootte van de index. Als dat u de verwachte grootte van de index kunt schatten, kunt u dat nummer ten opzichte van de partitiegrootte per SKU, uitgebreid met het aantal partities die zijn vereist voor het opslaan van een index van die grootte vergelijken. 
4. **Wat is het laden van query verwacht**? Zodra opslagvereisten worden begrepen, kunt u query werkbelasting. S2 en beide S3 SKU's bieden bijna equivalent doorvoer, maar SLA vereisten wordt sprake is van een proefversie SKU's. 
5. Als u de laag S2 of S3, moet u bepalen of u [indexeerfuncties](search-indexer-overview.md)nodig. Indexeerfuncties zijn nog niet beschikbaar voor de laag S3 HD. Alternatieve benadering is via een push-model voor index-updates, waarin u toepassingscode aan een gegevensset push aan een index schrijft.

De meeste klanten kunnen regel een specifieke SKU in- of uitzoomen op basis van hun antwoorden op de bovenstaande vragen. Als u nog steeds niet zeker weet welke SKU om te gaan met, kunt u vragen posten naar MSDN- of StackOverflow forums of contact opnemen met Azure-ondersteuning voor verdere instructies.

## <a name="decision-validation-does-the-sku-offer-sufficient-storage-and-qps"></a>Gegevensvalidatie beschikking: de SKU biedt voldoende opslagruimte en QPS?

Als laatste stap, bezoekt u de [pagina prijzen](https://azure.microsoft.com/pricing/details/search/) en de [secties per-service en per-index in Service limieten](search-limits-quotas-capacity.md) om Controleer of uw schattingen ten opzichte van de limieten voor abonnement en service. 

Als de prijs of de opslag vereisten buiten het bereik, wilt u mogelijk de werkbelasting tussen meerdere kleinere services (bijvoorbeeld) refactoring. Op gedetailleerd niveau, kan u het ontwerp van indexen kleiner of filters gebruiken query's om efficiënter te maken.

> [AZURE.NOTE] Opslagvereisten kunnen zijn te weinig hoge als documenten overbodige gegevens bevatten. In het ideale geval bevatten documenten alleen doorzoekbare gegevens of metagegevens. Binaire gegevens niet-doorzoekbaar is en kan worden opgeslagen afzonderlijk (bijvoorbeeld in een Azure tabel of blob storage) met een veld in de index voor een URL-verwijzing naar de externe gegevens. De maximale grootte van een afzonderlijk document is 16 MB (of minder als u meerdere documenten in een aanvraag uploaden bulksgewijs zijn). Zie [beperkingen voor de Service in Azure zoeken](search-limits-quotas-capacity.md) voor meer informatie.

## <a name="next-step"></a>Volgende stap

Zodra u welke SKU is de juiste keuze te maken weet, gaat u verder met de volgende stappen uit:

- [Een search-service in de beheerportal maken](search-create-service-portal.md)
- [Gebruik van de partities en replica's aan de nieuwe schaal uw service wijzigen](search-capacity-planning.md)

