<properties
   pageTitle="Betrouwbare verzamelingen | Microsoft Azure"
   description="Service statuscontrole configuratieservices bieden betrouwbare verzamelingen waarmee u kunt het schrijven van zeer beschikbaar, scalable en lage latentie cloud-toepassingen."
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Inleiding tot betrouwbare verzamelingen in Azure-Service statuscontrole configuratieservices

Betrouwbare verzamelingen kunnen u voor het schrijven van zeer beschikbaar, scalable en lage latentie cloud-toepassingen, alsof u één computer-toepassingen schrijft. De klassen in de naamruimte **Microsoft.ServiceFabric.Data.Collections** bieden een verzameling kant-van-het-box-siteverzamelingen die uw status automatisch ten zeerste beschikbaar maken. Ontwikkelaars moeten programma alleen op de betrouwbare siteverzameling-API's en laat betrouwbare siteverzamelingen beheren de gerepliceerde en lokale staat.

Het belangrijkste verschil tussen betrouwbare verzamelingen en andere technologieën hoge beschikbaarheid (zoals bestand Vgx., service Azure tabel en Azure wachtrij service) is dat de status wordt opgeslagen lokaal in het exemplaar van de service terwijl ook wordt ten zeerste beschikbaar gemaakt. Dit betekent dat:

- Alle lees zijn lokale, wat resulteert in lage latentie en hoge gegevensdoorvoer leest.
- Alle schrijft tot het minimum aantal netwerk IOs, waardoor lage latentie en hoge gegevensdoorvoer schrijft.

![Afbeelding van de ontwikkeling van siteverzamelingen.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Betrouwbare verzamelingen kunnen worden beschouwd als de natuurlijke ontwikkeling van de klassen **System.Collections** : een nieuwe set siteverzamelingen die zijn ontworpen voor de cloud en meerdere computer toepassingen zonder te verhogen complexiteit voor ontwikkelaars. Als zodanig zijn betrouwbare verzamelingen:

- Gerepliceerd: Staat wijzigingen worden gerepliceerd voor maximale beschikbaarheid.
- Behouden is gebleven: Gegevens blijft behouden naar een diskette levensduur tegen grootschalige bijvoorbeeld (bijvoorbeeld een datacenter van de stroom uitvalt).
- Asynchroon: API's zijn asynchroon om ervoor te zorgen dat threads niet worden geblokkeerd wanneer dat IO.
- Transacties: API's maken gebruik van de onttrekking van transacties zodat u eenvoudig meerdere betrouwbare verzamelingen binnen een service kunt beheren.

Betrouwbare verzamelingen bieden sterke consistentie garandeert afmelden bij het vak om te vereenvoudigen redeneren over de toepassingsstatus.
Sterke consistentie wordt doordat transactie doorvoeracties voltooien nadat de hele transactie is aangemeld een quorum meeste van replica's, met inbegrip van de primaire bereikt.
Als u wilt bereiken zwakkere consistentie, kunnen toepassingen Bevestig terug naar de client/aanvrager voordat het asynchrone doorvoeren geeft als resultaat.

De betrouwbare verzamelingen-API's zijn een ontwikkeling van gelijktijdige collecties API's (gevonden in de naamruimte **System.Collections.Concurrent** ):

- Asynchroon: Retourneert een taak aangezien, in tegenstelling tot gelijktijdige collecties, de bewerkingen worden gerepliceerd en behouden.
- Parameters niet uit: Gebruik `ConditionalValue<T>` om terug te keren een Boole-waarde en een waarde in plaats van parameters uit. `ConditionalValue<T>`is als het `Nullable<T>` , maar geen T om te worden van een structuur vereist.
- Transacties: Een transactieobject gebruikt zodat de gebruiker aan de groep acties op meerdere betrouwbare in een transactie.

Tegenwoordig kunnen bevat **Microsoft.ServiceFabric.Data.Collections** twee verzamelingen:

- [Betrouwbare woordenlijst](https://msdn.microsoft.com/library/azure/dn971511.aspx): een collectie gerepliceerde transacties en asynchroon sleutel/waardeparen. Net zoals bij **ConcurrentDictionary**, zowel de toets als de waarde mag van elk type.
- [Betrouwbare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx): een gerepliceerde, transacties en asynchroon strikte eerst hebt aangemeld formule (FIFO) wachtrij vertegenwoordigt. Net als **ConcurrentQueue**wordt de waarde van elk type kan zijn.

## <a name="isolation-levels"></a>Moeten worden geïsoleerd niveaus
Isolatieniveau bepalen in hoeverre waaraan de transactie geïsoleerd tegen wijzigingen die zijn aangebracht door andere transacties worden moet.
Er zijn twee moeten worden geïsoleerd niveaus die worden ondersteund in betrouwbare siteverzamelingen:

- **Herhaald lezen**: Hiermee wordt opgegeven dat instructies kunnen niet worden gebruikt voor het lezen van gegevens die zijn gewijzigd, maar nog niet zijn doorgevoerd door andere transacties en dat er geen andere transacties kunnen worden gebruikt voor het wijzigen van gegevens die door de huidige transactie is gelezen totdat de huidige transactie is voltooid. Zie [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx)voor meer informatie.
- **Momentopname**: geeft aan dat gegevens gelezen door eventuele-instructie in een transactie de transactioneel consistente versie van de gegevens die bestaan aan het begin van de transactie.
De transactie herkent alleen gegevenswijzigingen die zijn doorgevoerd vóór het begin van de transactie.
Gegevenswijzigingen die zijn aangebracht door andere transacties na het begin van de huidige transactie zijn niet zichtbaar voor instructies die wordt uitgevoerd in de huidige transactie.
Het effect is als wanneer de instructies in een transactie een momentopname van de vastgelegde gegevens ophalen, zoals deze aan het begin van de transactie was.
Momentopnamen zijn consistent over betrouwbare siteverzamelingen.
Zie [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx)voor meer informatie.

Betrouwbare verzamelingen Kies automatisch het isolatieniveau voor een bepaald gelezen bewerking afhankelijk van de bewerking en de rol van de replica op het moment van transactie gemaakt.
Hieronder volgt de tabel die moeten worden geïsoleerd standaardwaarden op siteniveau voor betrouwbare woordenlijst en wachtrij bewerkingen afbeeldt.

| Bewerking \ rol      | Primaire          | Secundaire        |
| --------------------- | :--------------- | :--------------- |
| Enkele entiteit lezen    | Herhaald lezen  | Momentopname         |
| Inventarisatie \ tellen   | Momentopname         | Momentopname         |

>[AZURE.NOTE] Algemene voorbeelden voor één entiteit bewerkingen zijn `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.

Zowel de betrouwbare woordenlijst en de betrouwbare wachtrij ondersteuning voor alleen uw schrijft.
Met andere woorden, is een schrijven binnen een transactie zichtbaar voor een volgende lezen die aan dezelfde transactie toebehoort.

## <a name="locking"></a>Vergrendelen
In betrouwbare verzamelingen, alle transacties zijn twee fasen: een transactie bevat geen vergrendelingen de deze heeft verkregen totdat de transactie met een afbreken of een doorvoeren eindigt.

Betrouwbare woordenlijst maakt gebruik van rijniveau vergrendelen voor alle bewerkingen van de eenheid.
Betrouwbare wachtrij trades uitschakelen bij gelijktijdigheid voor strikte transacties FIFO-eigenschap.
Betrouwbare wachtrij wordt gebruikt voor bewerking niveau vergrendelingen toestaan één transactie met `TryPeekAsync` en/of `TryDequeueAsync` en één transactie met `EnqueueAsync` tegelijk.
Houd er rekening mee dat als u wilt behouden FIFO, als een `TryPeekAsync` of `TryDequeueAsync` ooit neemt acht dat de betrouwbare wachtrij leeg is, worden ze ook wordt vergrendeld `EnqueueAsync`.

Schrijf bewerkingen altijd exclusief vergrendelingen overnemen.
Voor meer bewerkingen, de vergrendeling is afhankelijk van een aantal factoren.
Elke willekeurige lees bewerking klaar met overgeslagen is gratis vergrendelen.
Een bewerking herhaald lezen al dan niet standaard gaat gedeeld vergrendelingen.
De gebruiker kan echter voor elke gelezen bewerking die ondersteuning biedt voor herhaald lezen, vragen om een Update vergrendelen in plaats van de vergrendeling gedeeld.
Een Update-lock is een asymmetrische vergrendeling gebruikt om te voorkomen dat een gangbaar deadlock die optreedt wanneer meerdere transacties bronnen voor mogelijke updates op een later tijdstip vergrendelen.

De matrix van de compatibiliteit vergrendelen vindt u hieronder:

| Aanvragen \ verleend | Geen         | Gedeeld       | Update      | Exclusief    |
| ----------------- | :----------- | :----------- | :---------- | :----------- |
| Gedeeld            | Geen conflict  | Geen conflict  | Conflict    | Conflict     |
| Update            | Geen conflict  | Geen conflict  | Conflict    | Conflict     |
| Exclusief         | Geen conflict  | Conflict     | Conflict    | Conflict     |

Houd er rekening mee dat er een time-out van de argumenten in de betrouwbare verzamelingen-API's wordt gebruikt voor deadlock-detectie.
Bijvoorbeeld probeert twee transacties (t 1 en t 2) te lezen en K1 bijwerken.
Is het mogelijk worden impasse, omdat ze beide uiteindelijk de vergrendeling gedeeld met.
In dit geval wordt een of beide van de bewerkingen time-out.

Houd er rekening mee dat het bovenstaande deadlock scenario een goed voorbeeld is van hoe een Update vergrendeling deadlocks kunt voorkomen.

## <a name="persistence-model"></a>Permanente model
Ga als volgt een permanente-model dat wordt aangeroepen logboeken en controlepunt de betrouwbare staat Manager en betrouwbare siteverzamelingen.
Dit is een model waarbij elke wijziging staat schijf aangemeld en alleen in het geheugen toegepast.
De volledige staat zelf wordt alleen af en toe behouden (ook Controlepunt).
Het voordeel is of delta in opeenvolgende alleen toevoegen schrijft op schijf voor betere prestaties zijn ingeschakeld.

Meer informatie over het model logboeken en controlepunt, eerst eens de scenario oneindig Schijfopruiming.
De betrouwbare staat Manager logboeken elke bewerking voordat deze worden gerepliceerd.
Hierdoor wordt de betrouwbare verzameling alleen de bewerking in het geheugen toepassen.
Aangezien logboeken worden doorgevoerd, zelfs wanneer de replica mislukt en moet opnieuw worden gestart, heeft de betrouwbare staat Manager weinig gegevens in de logboeken op opnieuw afspelen alle bewerkingen die de replica gaat er verloren.
Terwijl de schijf oneindig is, logboekrecords nooit moeten worden verwijderd en de betrouwbare verzameling moet alleen de status in het geheugen beheren.

Nu we het scenario eindige schijf bekijken.
Zoals logboekrecords verzamelen, wordt de betrouwbare staat Manager voldoende schijfruimte uitgevoerd.
Voordat die gebeurt, moet de betrouwbare staat Manager afkappen van het logboek om ruimte voor de nieuwere records te maken.
Wordt automatisch de betrouwbare collecties met controlepunt aangevraagd hun status in het geheugen naar schijf.
Het is de betrouwbare verzamelingen verantwoordelijkheid om te behouden de staat tot dat punt.
Zodra de betrouwbare verzamelingen hun controlepunten hebt voltooid, kan de betrouwbare staat Manager het logboek schijfruimte vrijmaken afkappen.
Op deze manier wanneer de replica moet opnieuw worden gestart, betrouwbare verzamelingen worden hersteld met hun controlepunt staat en de betrouwbare staat Manager wordt herstellen en alle wijzigingen die zijn aangebracht sinds het controlepunt af te spelen.

>[AZURE.NOTE] Een andere waarde toevoegen van plaatst controlepunten is dat verbetert de prestaties van herstel in de algemene gevallen.
Dit is omdat controlepunten alleen de meest recente versies bevat.

## <a name="recommendations"></a>Aanbevelingen

- Een object van aangepaste type geretourneerd door meer bewerkingen niet wijzigen (bijvoorbeeld `TryPeekAsync` of `TryGetValueAsync`). Betrouwbare verzamelingen, net als gelijktijdige verzamelingen, resultaat een verwijzing naar de objecten en geen kopie.
- Het geretourneerde object van een aangepast type uitgebreide kopiëren in te doen voordat u deze wijzigt. Aangezien structs en ingebouwde typen keer-op-waarde, hoeft u niet te doen een uitgebreide kopie worden.
- Gebruik geen `TimeSpan.MaxValue` voor time-outs. Time-outs moeten worden gebruikt om te bepalen deadlocks.
- Gebruik een transactie niet nadat deze is vastgelegd, afgebroken, of verwijderd.
- Gebruik een classificering niet buiten het transactiebereik is dat het is gemaakt.
- Maak een transactie binnen een andere transactie geen `using` instructie omdat deze leiden deadlocks tot kan.
- Zorg ervoor dat uw `IComparable<TKey>` implementatie juist is. Het systeem gaat afhankelijkheid hierover voor het samenvoegen van controlepunten.
- Gebruik Update vergrendelen tijdens het lezen van een item een bedoeling deze om te voorkomen dat een bepaalde klasse deadlocks bij te werken.
- Back-up kunt u overwegen en functionaliteit als u wilt dat herstel herstellen.
- Combineer geen enkele entiteit bewerkingen en meerdere entiteit bewerkingen (bijvoorbeeld `GetCountAsync`, `CreateEnumerableAsync`) in dezelfde transactie vanwege de niveaus van de verschillende moeten worden geïsoleerd.

Hier zijn enkele dingen in gedachten moet houden:

- De standaard-time-out is vier seconden voor alle betrouwbare siteverzameling API's. De meeste gebruikers moeten niet overschreven door dit.
- Het standaard annulering token `CancellationToken.None` in alle betrouwbare verzamelingen-API's.
- De belangrijkste typeparameter (*TKey*) voor een betrouwbare woordenlijst moet correct implementeren `GetHashCode()` en `Equals()`. Toetsen moet onveranderlijke.
- Als u wilt bereiken beschikbaarheid voor de betrouwbare verzamelingen, moet elke service ten minste een doel en de minimale grootte van 3 replicaset hebben.
- Alleen bewerkingen op de secundaire kunnen versies die niet quorum vastgelegde zijn gelezen.
Dit betekent dat een versie van de gegevens die worden gelezen van één secundair false mogelijk worden vordert.
Uiteraard kunt lezen van primaire hebben altijd een stabiele: kan nooit worden ONWAAR vordert.

## <a name="next-steps"></a>Volgende stappen

- [Betrouwbare Services snel aan de slag](service-fabric-reliable-services-quick-start.md)
- [Werken met betrouwbare collecties](service-fabric-work-with-reliable-collections.md)
- [Betrouwbare Services-meldingen](service-fabric-reliable-services-notifications.md)
- [Betrouwbare Services back-up en herstellen (herstel)](service-fabric-reliable-services-backup-restore.md)
- [Configuratie van de Manager betrouwbaar staat](service-fabric-reliable-services-configuration.md)
- [Aan de slag met Service stof Web API-services](service-fabric-reliable-services-communication-webapi.md)
- [Geavanceerd gebruik van de betrouwbare Services programming model](service-fabric-reliable-services-advanced-usage.md)
- [Naslaginformatie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
