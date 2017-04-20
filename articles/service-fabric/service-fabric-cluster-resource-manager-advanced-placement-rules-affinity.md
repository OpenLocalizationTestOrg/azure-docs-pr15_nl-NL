<properties
   pageTitle="Service stof Cluster resourcemanager - affiniteit | Microsoft Azure"
   description="Overzicht van het configureren van affiniteit voor Service configuratieservices"
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

# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Configureren en gebruiken van service affiniteit in Service stof

Affiniteit is een besturingselement dat hoofdzakelijk wordt weergegeven om te helpen bedoeld om de overgang van grotere monolithische toepassingen in de cloud en microservices wereld. Die gezegd dat deze kan ook worden gebruikt in bepaalde gevallen als een betrouwbare optimalisatie voor het verbeteren van services, hoewel dit bijwerkingen kan hebben.

Stel dat u een groter app of een die net is niet ontworpen met microservices in gedachten, naar de Service stof laten. Deze overgang is werkelijk algemene en we hebben verschillende klanten (interne en externe) in deze situatie gehad. U start door het opheffen van de hele-app in de omgeving, aan deze verpakt en uitvoeren. U start vervolgens splitsen in kleinere verschillende services van dat alle met elkaar communiceren.

Klik er is een "Oops...". De "Oops" wordt meestal in een van deze categorieën vallen:

1. Sommige onderdeel X in de monolithische app hebt u er een niet-gedocumenteerde afhankelijkheid op onderdeel Y en we in afzonderlijke services net hebt ingeschakeld. Aangezien deze nu op verschillende knooppunten in het cluster worden uitgevoerd, zijn de verbroken.
2.  Hier rekening communiceren via een (lokaal benoemde pipes | gedeeld geheugen | bestanden op schijf), maar ik echt moeten kunnen deze bij te werken onafhankelijk versnellen omhoog iets. Ik moet de harde afhankelijkheid later verwijderen.
3.  Alles werkt prima, maar het blijkt dat deze twee onderdelen heel daadwerkelijk zijn chatty/prestaties gevoelige. Wanneer ze deze naar afzonderlijke services verplaatst algehele prestaties van toepassingen tanked of latentie verhoogd. Hierdoor voldoet de algehele toepassing niet aan de verwachtingen.

In dat geval we niet wilt onze refactoring werk kwijtraakt en gaat u terug naar de monoliet niet wilt, maar we enkele komen locatie heb nodig. Hiermee blijft totdat we de onderdelen werken natuurlijk als services kunt ontwerpen of totdat we de prestatieverwachtingen een andere manier indien mogelijk kunt oplossen.

Wat moet u doen? U kunt ook proberen affiniteit inschakelen.

## <a name="how-to-configure-affinity"></a>Het configureren van affiniteit
Als u wilt affiniteit hebt ingesteld, kunt u een affiniteit relatie tussen twee verschillende services definiëren. U kunt zien affiniteit als "wijzen" één service bij een ander en zeggen "deze service kan alleen worden uitgevoerd waar of service wordt uitgevoerd." Soms verwezen naar affiniteit als een bovenliggende en onderliggende relatie (waar u wijst u de onderliggende de bovenliggende). Affiniteit zorgt ervoor dat de replica's of exemplaren van een service in de knooppunten hetzelfde als de replica's of exemplaren van een ander worden gebracht.

``` csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

## <a name="different-affinity-options"></a>Opties voor verschillende affiniteit
Affiniteit wordt aangegeven via een van de verschillende correlatie kunt herkennen en heeft twee verschillende modi. De meest voorkomende modus van affiniteit is verwijst naar de NonAlignedAffinity. Het replica's of exemplaren van de verschillende services worden NonAlignedAffinity op dezelfde knooppunten geplaatst. De andere modus is AlignedAffinity. Uitgelijnde affiniteit komt alleen voor gebruik met statuscontrole services. Configureren van twee statuscontrole services als u wilt hebben uitgelijnd affiniteit zorgt ervoor dat de primaire van deze services in de dezelfde knooppunten als elkaar worden gebracht. Het is ook zorgt ervoor dat elk paar secundaire servers voor die services op de dezelfde knooppunten wordt geplaatst. Het is ook mogelijk (hoewel minder algemeen) voor het configureren van NonAlignedAffinity voor statuscontrole services. Voor NonAlignedAffinity zou de verschillende kopieën van de twee statuscontrole services worden collocated op dezelfde knooppunten, maar geen geprobeerd zou doen om uit te lijnen hun primaire of secundaire servers.

![Affiniteit modi en hun effecten][Image1]

### <a name="best-effort-desired-state"></a>Aanbevolen hoeveelheid gewenst staat
Er zijn enkele verschillen tussen affiniteit en monolithische architecturen. Veel van deze zijn, omdat een relatie affiniteit aanbevolen hoeveelheid werk is. De services in een relatie affiniteit zijn fundamenteel anders entiteiten die kunnen mislukken en onafhankelijk worden verplaatst. Er zijn ook oorzaken voor waarom een relatie affiniteit kan verbreken. Beperkingen van de capaciteit waar slechts enkele van de service objecten in de relatie affiniteit bijvoorbeeld past op een bepaald knooppunt. In dat geval zelfs als er een relatie affiniteit op hun plaats staan, worden deze niet afgedwongen vanwege de andere beperkingen. Als het is mogelijk om te dwingen voor alle andere beperkingen en affiniteit op een later tijdstip worden de overtreding van de beperking affiniteit automatisch gecorrigeerd.  

### <a name="chains-vs-stars"></a>Komen versus sterren
Vandaag kunnen we niet model komen affiniteit relaties. Wat betekent dit dat is dat een service die is een subtitel in één affiniteit relatie kan een bovenliggend item bij een andere affiniteit relatie niet. Als u modelleren van dit type relatie wilt, moet u effectief modelleren van deze als een sterretje, in plaats van een ketting. Hiervoor de onderste onderliggende zou het bovenliggende item van het 'middelste' kind bovenliggende in plaats daarvan. Afhankelijk van de indeling van uw services, kan dit vragen om een tijdelijke aanduiding ' voor'-service moet fungeren als de bovenliggende voor meerdere kinderen maken.

![Komen versus sterren in de Context van affiniteit relaties][Image2]

Een ander wat u moet opmerking over affiniteit relaties vandaag is dat ze kunnen een bepaalde richting. Dit betekent dat de regel 'affiniteit' alleen worden toegepast dat de onderliggende is waar de bovenliggende is. Als u bijvoorbeeld de bovenliggende ineens wordt overgenomen door een ander knooppunt vervolgens de resourcemanager Cluster niet daadwerkelijk denkt dat er is iets fout gegaan totdat deze kennisgevingen dat de onderliggende bevindt zich in met een bovenliggend item; de relatie is niet onmiddellijk afgedwongen.

### <a name="partitioning-support"></a>Ondersteuning voor partitioneren
Het uiteindelijke dat opvalt over affiniteit is die affiniteit relaties, waar de bovenliggende is partities worden niet ondersteund. Dit is iets dat wordt uiteindelijk mogelijk ondersteund, maar vandaag het is niet toegestaan.

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over de andere opties die beschikbaar zijn voor het configureren van services uitchecken het onderwerp op de andere Cluster resourcemanager-configuraties beschikbaar [meer informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)
- Veel redenen waar mensen affiniteit gebruiken, zoals het beperken van services op een kleine set machines en beter wilt aggregeren het laden van een verzameling services, worden ondersteund door groepen. Bekijk de [Toepassingsgroepen](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png
