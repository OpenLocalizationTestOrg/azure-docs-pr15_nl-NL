<properties
   pageTitle="Resourcemanager voor service stof Cluster: verkeer kosten | Microsoft Azure"
   description="Overzicht van de verplaatsing van de kosten voor Service configuratieservices"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="service-movement-cost-for-influencing-cluster-resource-manager-choices"></a>Service verkeer kosten voor Cluster resourcemanager keuzen beïnvloeden
Een belangrijke factor u rekening moet houden wanneer u probeert om te bepalen welke wijzigingen aanbrengen in een cluster en de score van een oplossing is de totale kosten van dat nodig is om deze oplossing.

Service exemplaren of replica's kosten CPU tijd en netwerk bandbreedte ten minste verplaatsen. Voor statuscontrole services kost deze ook de hoeveelheid ruimte op schijf die u een kopie van de stand maken moet voordat u oude replica's afsluit. Zou willen u duidelijk de kosten van een oplossing die Azure stof Cluster Resource servicebeheer met verschijnt minimaliseren. Maar u ook niet wilt negeren oplossingen waarmee u zou de toewijzing van resources in het cluster aanzienlijk verbeteren.

Resourcemanager cluster heeft twee manieren om een computing kosten en beperkt, zelfs wanneer wordt geprobeerd het cluster op basis van de andere doelstellingen beheren. De eerste is dat bij de Cluster Resource Manager is een nieuwe lay-out voor het cluster planning, deze telt elke verplaatsen die zou. Een eenvoudig voorbeeld, als er twee oplossingen met over hetzelfde totale saldo vanaf (score) aan het einde, en maak daarna de tabel met de laagste kosten (totale aantal verplaatst).

Dit werkt heel goed. Maar net als bij de standaard- of statische laadtijd, is het niet waarschijnlijk in een complexe systeem dat alle verplaatsingen gelijk zijn. Sommige zijn waarschijnlijk veel meer dure.

## <a name="changing-a-replicas-move-cost-and-factors-to-consider"></a>De kosten van verplaatsen en de factoren u rekening moet houden op basis van wijzigen
Als met het rapporteren van laden (een andere functie van Cluster Resource Manager), kunt u de service van selfservice rapportage hoe dure de service is om te verplaatsen op een bepaald moment.

Code:

```csharp
this.ServicePartition.ReportMoveCost(MoveCost.Medium);
```

MoveCost vier niveaus heeft: nul, laag, normaal en hoog. Hierna ziet u ten opzichte van elkaar, behalve voor nul. Nul betekent dat het verplaatsen van een replica is gratis en moet niet meegeteld in de score van de oplossing. Het instellen van uw kosten verplaatsen op hoog is *niet* een garantie die de replica niet verplaatst, zojuist hebt dat het niet verplaatst tenzij er is een goede reden aan.

![Kosten als factor bij het selecteren van replica's voor verkeer verplaatsen][Image1]

MoveCost kunt u de oplossingen die ertoe leiden dat de algehele minimaal verstoring en makkelijkste manier om te bereiken nog steeds worden bezorgd bij equivalente saldo zijn te vinden. Kosten van een service begrip mag ten opzichte van veel zaken zijn. De meest voorkomende factoren bij het berekenen van de kosten van uw verplaatsen zijn:

- De hoeveelheid staat of gegevens die bestaan uit de service om te verplaatsen.
- De kosten van verbreken klanten. De kosten van het verplaatsen van een primaire replica is meestal hoger zijn dan de kosten van een secundaire replica verplaatsen.
- De kosten van een tijdens een vlucht bewerking onderbreken. Sommige bewerkingen gegevens opslaan niveau of bewerkingen die zijn uitgevoerd in antwoord op een gesprek client dure zijn. Niet wilt u deze stoppen als u niet te hoeft na een bepaald punt. Dus voor de duur van de bewerking vergroten u de kosten voor de kans dat de service replica of het exemplaar wordt verplaatst. Wanneer de bewerking is voltooid, kunt u deze naar de normale plaatsen.

## <a name="next-steps"></a>Volgende stappen
- Aan de doelstellingen-stof Cluster Resource servicebeheer gebruikt om verbruik en capaciteit in het cluster te beheren. Meer informatie over de doelstellingen en hoe u deze configureert, raadpleegt u [verbruik van beheren en laden in Service stof met aan de doelstellingen](service-fabric-cluster-resource-manager-metrics.md).
- Meer informatie over hoe de resourcemanager Cluster beheert en laden in het cluster saldi, Bekijk [het verdelen van uw cluster Service stof](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
