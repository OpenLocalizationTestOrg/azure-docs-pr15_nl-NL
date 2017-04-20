<properties
   pageTitle="Testbaarheid: Service-communicatie | Microsoft Azure"
   description="Communicatie van de service-naar-service is een punt kritieke integratie van een toepassing voor de Service stof. In dit artikel wordt beschreven hoe ontwerpoverwegingen en testen technieken."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/06/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-testability-scenarios-service-communication"></a>Service stof testbaarheid scenario's: Service-communicatie

Microservices en architectuur stijlen service gerichte oppervlak natuurlijk in Azure-Service stof. In deze typen verdeelde architecturen, samengestelde microservice toepassingen meestal bestaan uit meerdere services die moeten met elkaar communiceren. In zelfs de eenvoudigste gevallen hebt u gewoonlijk ten minste een stateless webservice en een service voor de opslag van statuscontrole gegevens die moeten communiceren.

Communicatie van de service-naar-service is een punt kritieke integratie van een toepassing, omdat elke service een externe API met andere services beschrijft. Werken met een reeks API grenzen waarvoor I/O in het algemeen is vereist voor sommige noodzakelijk, met een goede hoeveelheid testen en valideren.

Er zijn een groot aantal overwegingen wanneer de grenzen van deze services zijn samen wired in een gedistribueerd systeem:

 - *Transportprotocol*. Wilt u HTTP voor betere interoperabiliteit of een aangepaste binaire protocol gebruiken maximale worden verwerkt?
 - *Foutverwerking*. Hoe wordt permanente en tijdelijke fouten worden verwerkt? Wat gebeurt er wanneer een service wordt verplaatst naar een ander knooppunt?
 - *Time-out en latentie*. In een toepassingen, hoe elke laag die service omgaan latentie door de stapel en aan de gebruiker?

Of u een van de onderdelen van de ingebouwde service-communicatie verstrekt door de Service stof gebruiken of dat u maakt met uw eigen, het testen van de interactie tussen uw services is belangrijk dat u ervoor zorgen dat tolerantie in uw toepassing.

## <a name="prepare-for-services-to-move"></a>Voorbereiden voor services verplaatsen

Service-exemplaren mogelijk navigeren na verloop van tijd. Dit geldt met name wanneer ze zijn geconfigureerd met laden aan de doelstellingen voor optimale resource aangepast op basis van hun verdelen. Service stof Hiermee verplaatst u de service-exemplaren naar hun beschikbaarheid maximaliseren ook tijdens upgrades, failovers schalen en andere situaties die in de loop van een gedistribueerd systeem optreden.

Als services in het cluster bewegen, moeten u uw klanten en andere services voorbereid worden afgehandeld twee scenario's wanneer ze Neem op met een service contact:

- De service-exemplaar of partition replica is verplaatst sinds de laatste keer dat er naartoe is besproken. Dit is een normale gedeelte van de levenscyclus van een service en deze worden verwacht veroorzaken, gedurende de levensduur van uw toepassing.
- De service-exemplaar of partition replica wordt verplaatst. Hoewel de overname van een service van één knooppunt naar de andere zeer snel in Service stof voorkomt, kan er een vertraging in de beschikbaarheid als het onderdeel communicatie van uw service traag is te starten.

Verwerking van deze scenario's zonder problemen is belangrijk voor een systeem vloeiend uitvoeren. Klik hiervoor Denk eraan dat:

- Elke service die kan worden verbonden aan heeft een *adres* waarop deze luistert (bijvoorbeeld HTTP of WebSockets). Als u een exemplaar van de service of partition verplaatst, wordt het eindpunt adres verandert. (Deze wordt verplaatst naar een ander knooppunt met een ander IP-adres.) Als u de ingebouwde communicatie-onderdelen gebruikt, wordt deze opnieuw omzetten service-adressen voor u verwerken.
- Er zijn mogelijk een tijdelijke toename van de service latentie als het starten van de service-exemplaar van de luisteraar ervan af opnieuw. Dit hangt hoe snel de luisteraar ervan af door de service wordt geopend nadat het exemplaar van de service is verplaatst.
- Een bestaande verbindingen moeten worden gesloten en opnieuw wordt geopend wanneer u de service wordt geopend op een nieuw knooppunt. Opnieuw starten of afsluiten correcte knooppunt kunt tijd voor bestaande verbindingen worden afsluiten.

### <a name="test-it-move-service-instances"></a>Testen: exemplaren van de service verplaatsen

Met behulp van de Service stof testbaarheid hulpmiddelen, kunt u een Testscenario om te testen van deze situaties doen zich op verschillende manieren creëert:

1. Een statuscontrole service primaire replica verplaatsen.

    De primaire replica van een partition statuscontrole service kan worden verplaatst voor een aantal redenen. Gebruik deze optie als u de primaire replica van een specifieke partition om te zien hoe uw services om onderweg te reageren op een zeer gecontroleerde manier afstemmen.

    ```powershell

    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService

    ```

2. Een knooppunt stoppen.

    Wanneer een knooppunt is gestopt, worden alle exemplaren van de service of partities die op dat knooppunt naar een van de andere beschikbare knooppunten in het cluster waren Service stof verplaatst. Gebruik deze opdracht om te testen van een situatie waarbij een knooppunt verdwijnen uit uw cluster en alle van de service-exemplaren en replica's op het knooppunt moeten verplaatsen.

    U kunt een knooppunt stoppen met behulp van de cmdlet PowerShell **Stoppen-ServiceFabricNode** :

    ```powershell

    PS > Restart-ServiceFabricNode -NodeName Node_1

    ```

## <a name="maintain-service-availability"></a>Voor het behoud van beschikbare service

Als een platform, is de Service stof ontworpen beschikbaarheid van uw services. Maar in extreme omstandigheden kunnen onderliggende infrastructuur problemen kunnen nog steeds niet beschikbaar. Het is belangrijk kunt u deze scenario's te controleren.

Statuscontrole services gebruiken een quorum-systeem voor beschikbaarheid repliceren. Dit betekent dat een quorum van replica's beschikbaar moet zijn schrijven bewerkingen uit te voeren. In sommige gevallen zoals een algemene hardware is mislukt, een quorum van replica's mogelijk niet beschikbaar. In dat geval is niet mogelijk schrijven bewerkingen uit te voeren, maar is nog steeds mogelijk meer bewerkingen uit te voeren.

### <a name="test-it-write-operation-unavailability"></a>Testen: schrijven bewerking niet beschikbaar

Met behulp van de hulpmiddelen testbaarheid in Service stof, kunt u een fout die quorum verlies als test induceert invoeren. Hoewel deze situatie niet vaak voorkomt, is het belangrijk dat clients en services die afhankelijk van een statuscontrole service zijn zijn voorbereid voor situaties waarin ze niet kunnen aanbrengen aanvragen ernaar te schrijven. Het is ook belangrijk dat de statuscontrole service zelf op de hoogte van deze mogelijkheid is en kan deze zonder problemen voor bellers communiceren.

U kunt verlies quorum veroorzaken met behulp van de cmdlet PowerShell **Roep-ServiceFabricPartitionQuorumLoss** :

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

In dit voorbeeld wordt ingesteld `QuorumLossMode` naar `QuorumReplicas` om aan te geven dat we quorum verlies wilt zonder dat u omlaag alle replica's veroorzaken. Op deze manier gelezen bewerkingen zijn nog steeds mogelijk. Als u wilt testen in een scenario waarbij een hele partition niet beschikbaar is, kunt u deze schakeloptie instellen op `AllReplicas`.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over testbaarheid acties](service-fabric-testability-actions.md)

[Meer informatie over testbaarheid scenario 's](service-fabric-testability-scenarios.md)
