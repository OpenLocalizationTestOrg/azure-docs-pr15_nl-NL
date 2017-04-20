<properties
   pageTitle="Service stof Cluster resourcemanager - toepassingsgroepen | Microsoft Azure"
   description="Overzicht van de toepassing groep functionaliteit in Service stof Cluster bronbeheer"
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

# <a name="introduction-to-application-groups"></a>Inleiding tot groepen
De service stof Cluster resourcemanager beheert meestal cluster resources door het selectievakje laden (weergegeven via de doelstellingen) gelijkmatig overal in het cluster spreiding. Service stof beheert ook de capaciteit van de knooppunten in het cluster en het cluster als geheel tot en met het begrip van de capaciteit. Dit werkt prima voor een groot aantal verschillende soorten werkbelasting, maar patronen die intensief gebruik van verschillende Servicetoepassingsexemplaren voor stof soms naar voren aanvullende vereisten maken. Sommige aanvullende vereisten zijn meestal:

- Mogelijkheid om capaciteit voor een exemplaar van de toepassing services op een aantal knooppunten te reserveren
- Mogelijkheid om te beperken het totale aantal knooppunten die een opgegeven aantal services binnen een toepassing kan worden uitgevoerd op
- Capaciteiten die aan een exemplaar van de toepassing zelf om te beperken het cijfer of de totale resourceverbruik van de services hierin

Om te voldoen aan deze vereisten is voldaan, ontwikkeld we ondersteuning voor verwijst naar de groepen.

## <a name="managing-application-capacity"></a>Beheer van de capaciteit van toepassingen
Toepassing capaciteit kan worden gebruikt om het aantal knooppunten overbrugd door een toepassing, alsmede het totale metrische belasting van die exemplaren van de toepassingen op afzonderlijke knooppunten te beperken. Deze kan ook worden gebruikt voor het reserveren van resources in het cluster voor de toepassing.

Toepassing capaciteit kan worden ingesteld voor nieuwe toepassingen wanneer ze worden gemaakt; Deze kan ook worden bijgewerkt voor bestaande toepassingen die zijn gemaakt zonder op te geven van de capaciteit van toepassingen.

### <a name="limiting-the-maximum-number-of-nodes"></a>Het maximum aantal knooppunten beperken
De eenvoudigste use-case voor de capaciteit van toepassing is als het starten van een toepassing moet worden beperkt tot een bepaalde maximum aantal knooppunten. Als er geen capaciteit van toepassing is opgegeven, het servicebeheer stof Cluster Resource wordt een exemplaar van replica's op basis van normale regels (het verdelen van of defragmentatie), dit meestal betekent dat de services worden verdeeld over alle beschikbare knooppunten in het cluster, of als Defragmentatie een willekeurige maar kleiner aantal knooppunten is ingeschakeld.

De volgende afbeelding ziet u de mogelijke positie van een toepassingsexemplaar zonder het maximum aantal knooppunten gedefinieerd en vervolgens dezelfde toepassing met een maximum aantal knooppunten instellen. Houd er rekening mee dat er is geen garanties aangebracht over welke replica's of exemplaren van de plaats waar services samen worden geplaatst.

![Toepassingsexemplaar Maximum aantal knooppunten definiëren][Image1]

De toepassing geen toepassing capaciteit instellen in het linker voorbeeld, en er drie services. CRM heeft een logische beslissing naar alle replica's over zes beschikbare knooppunten verspreid om te kunnen bereiken van de beste balans in het cluster genomen. In het juiste voorbeeld zien we dezelfde toepassing die wordt beperkt op drie knooppunten en waar Service stof CRM het beste saldo van de kopieën van van toepassingsservices heeft bereikt.

De parameter waarmee wordt bepaald dit gedrag wordt MaximumNodes genoemd. Deze parameter kunt instellen tijdens het maken van de toepassing, of bijgewerkt voor de instantie van een toepassing die al is uitgevoerd, waarin de hoofdletters/kleine letters Service stof CRM de kopieën van toepassingsservices naar de gedefinieerde maximum aantal knooppunten zullen beperken.

PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;

adUpdate.Metrics.Add(appMetric);
```

## <a name="application-metrics-load-and-capacity"></a>Toepassing maatstaven, laden en capaciteit
Groepen kunnen u ook definiëren die is gekoppeld aan een bepaalde toepassingsexemplaar, evenals de capaciteit van de toepassing met betrekking tot deze aan de doelstellingen van de doelstellingen. Dus bijvoorbeeld kunt u definiëren die als veel services als u wilt dat kunnen worden gemaakt in

Voor elke metrisch, moet u er 2 waarden die kunnen worden ingesteld om te beschrijven van de capaciteit voor dat toepassingsexemplaar zijn:

-   Totaal van de toepassing capaciteit – de totale capaciteit van de toepassing voor een bepaalde waarde weergeeft. Service stof CRM probeert de som van metrische laadtijd van deze toepassingsservices op de opgegeven waarde; beperken Als u van de toepassing services zijn al laden naar deze limiet verbruikt, wordt servicebeheer stof Cluster Resource bovendien het maken van nieuwe services of partities waardoor totale laden naar deze limiet eens slechts weigeren.
-   Maximale knooppunt capaciteit – Hiermee geeft u de totale maximumbelasting voor kopieën van de toepassingen services op één knooppunt. Als de totale belasting op het knooppunt is gewijd aan deze capaciteit, probeert Service stof CRM replica's verplaatsen naar andere knooppunten, zodat de restrictie capaciteit is voldaan.

## <a name="reserving-capacity"></a>Worden gereserveerd capaciteit
Er wordt een andere vaak gebruikt voor groepen om ervoor te zorgen dat resources binnen het cluster worden gereserveerd voor een bepaalde toepassingsexemplaar, zelfs als het toepassingsexemplaar van de de services die u erin nog geen of zelfs als ze de resources nog niet zijn verbruikt. Laten we eens kijken hoe dat werkt zou doen.  

### <a name="specifying-a-minimum-number-of-nodes-and-resource-reservation"></a>Een minimum aantal knooppunten en resource reserveren opgeven
Resources worden gereserveerd voor enkele aanvullende parameters moet een toepassingsexemplaar: *MinimumNodes* en *NodeReservationCapacity*

- MinimumNodes - net als de precisie van een doel maximum aantal knooppunten waarop de services binnen een toepassing kunnen worden uitgevoerd, kunt u ook opgeven het minimum aantal knooppunten die een toepassing moet worden uitgevoerd. Deze instelling effectief Hiermee definieert u het aantal knooppunten die de resources worden gereserveerd op ten minste capaciteit binnen het cluster als onderdeel van het maken van het toepassingsexemplaar van de te garanderen.
- NodeReservationCapacity - de NodeReservationCapacity voor elke waarde in de toepassing kan worden gedefinieerd. Hiermee definieert u de hoeveelheid metrische laden gereserveerd voor de toepassing op een willekeurig knooppunt waar elke replica of exemplaren van de services die u erin worden geplaatst.

Laten we Bekijk een voorbeeld van capaciteit reserveren:

![Toepassingsexemplaren gereserveerde capaciteit definiëren][Image2]

In het voorbeeld links hebben toepassingen geen de capaciteit van een toepassing gedefinieerd. Servicebeheer stof Cluster Resource wordt saldo vanaf de onderliggende services replica's en exemplaren samen met die van andere services (buiten de toepassing) van de toepassing om ervoor te zorgen saldo in de cluster.

In het voorbeeld aan de rechterkant, Stel dat de toepassing is gemaakt met een MinimumNodes ingesteld op 2, MaximumNodes ingesteld op 3 en een toepassing metrisch gedefinieerd met behulp van een reservering van 20, maximale capaciteit op een knooppunt van 50 en de capaciteit van een totaal toepassingen van 100, Service stof wordt op twee knooppunten voor de blauwe toepassing capaciteit gereserveerd en kan niet worden andere replica's in het cluster die capaciteit in beslag neemt. De capaciteit van deze gereserveerde toepassingen wordt beschouwd verbruikte en ten opzichte van de resterende cluster capaciteit geteld.

Wanneer een toepassing is gemaakt met reservering, wordt de resourcemanager Cluster capaciteit gelijk is aan MinimumNodes gereserveerd * NodeReservationCapacity in het cluster, maar er geen capaciteit op specifieke knooppunten gereserveerd totdat de kopieën van van de toepassing services worden gemaakt en geplaatst. In deze sectie geeft voor flexibiliteit, aangezien knooppunten zijn geselecteerd voor de nieuwe replica's alleen wanneer ze worden gemaakt. Capaciteit is gereserveerd op een specifieke knooppunt ten minste één replica is geplaatst op is geïnstalleerd.

## <a name="obtaining-the-application-load-information"></a>Het verkrijgen van de toepassing laden gegevens
Voor elke toepassing vindt dat toepassing capaciteit u omgeschreven de informatie over de werklast door kopieën van de services gerapporteerd. Service stof biedt PowerShell en beheerde API van query's voor dit doel.

Laden kan bijvoorbeeld worden opgehaald met de volgende PowerShell-cmdlet:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1

```

De uitvoer van deze query bevat de basisinformatie over de capaciteit van de toepassingen die is opgegeven voor de toepassing, zoals knooppunten voor Minimum en maximum aantal knooppunten. Er is ook informatie over het aantal knooppunten die de toepassing wordt momenteel gebruikt. Dus voor elke metrisch laden wordt er informatie over:
- De naam van de metrische: De naam van de meetwaarde.
-   Het reserveren capaciteit: Cluster capaciteit die is gereserveerd in het cluster van deze toepassing.
-   Belasting van toepassing: Totale belasting van deze toepassing onderliggende replica.
-   Toepassing capaciteit: Maximaal toegestane waarde van toepassing laden.

## <a name="removing-application-capacity"></a>Verwijderen van de capaciteit van toepassingen
Wanneer de capaciteit van toepassingen parameters zijn ingesteld voor een toepassing, kunnen ze worden verwijderd via Update toepassing-API's of PowerShell-cmdlets. Bijvoorbeeld:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Deze opdracht verwijdert u alle parameters van de capaciteit van toepassingen uit de toepassing en servicebeheer stof Cluster Resource wordt gestart met deze toepassing behandelen als een andere toepassing in het cluster die geen deze parameters die zijn gedefinieerd. Het effect van de opdracht wordt onmiddellijk en Cluster resourcemanager verwijdert u alle parameters van de capaciteit van de toepassing van deze toepassing; ze weer te geven, moet bijwerken toepassing API's om te worden aangeroepen met de juiste parameters.

## <a name="restrictions-on-application-capacity"></a>Beperkingen voor de capaciteit van toepassingen
Er zijn verschillende beperkingen van de capaciteit van toepassingen parameters die moeten worden aangehouden. Voor het geval de fouten met gegevensvalidatie, als het maken of bijwerken van de toepassing met een fout gaan verloren.
Alle parameters voor geheel getal moet niet-negatieve getallen.
Daarnaast wordt voor afzonderlijke parameters zijn beperkingen als volgt:

-   MinimumNodes zijn nooit groter dan MaximumNodes.
-   Als capaciteit voor een meting laden zijn gedefinieerd, volg ze deze regels:
  - Knooppunt reservering capaciteit zijn niet groter dan de maximale knooppunt capaciteit. Bijvoorbeeld niet kunt u beperkingen instellen voor de capaciteit voor metrisch "Processorsnelheid" voor het knooppunt 2 eenheden en probeert te reserveren 3 eenheden op elk knooppunt.
  - Als MaximumNodes is opgegeven, klikt u vervolgens mag het product van MaximumNodes en maximale knooppunt capaciteit geen groter is dan de capaciteit van toepassing. Bijvoorbeeld als u de maximale knooppunt capaciteit voor laden metrisch "CPU" naar 8 en u het maximum aantal knooppunten ingesteld op 10 hebt ingesteld, moet klikt u vervolgens totale toepassing capaciteit groter dan 80 voor deze metrisch laden.

Tijdens het maken van de toepassing (aan de clientzijde), en tijdens het bijwerken van de toepassing (aan de serverzijde), worden de beperkingen afgedwongen. Tijdens het maken van dit is een voorbeeld van een duidelijke overtreding van de vereisten sinds MaximumNodes < MinimumNodes en de opdracht loopt vast in de client voordat het verzoek zelfs naar Service stof cluster wordt verzonden:

``` posh
New-ServiceFabricApplication –Name fabric:/MyApplication1 –MinimumNodes 6 –MaximumNodes 2
```

Een voorbeeld van ongeldige update is als volgt. Als wij uitvoeren van een bestaande toepassing en het maximum aantal knooppunten naar een waarde bijwerken, wordt de update worden doorgegeven:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MaximumNodes 2
```

Daarna kunt we wilt bijwerken en minimale knooppunten:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MinimumNodes 6
```

De client heeft geen voldoende context over het gebruik van de toepassing, zodat het de update worden doorgegeven aan de Service stof cluster toegestaan. In het cluster, Service stof wordt de nieuwe parameter samen met de bestaande parameters valideren en de updatebewerking, mislukt omdat de waarde tegenstander minimale knooppunten groter is dan de waarde voor maximum aantal knooppunten. In dit geval blijven toepassing capaciteit parameters ongewijzigd.

Deze beperkingen zijn ingevoerd in volgorde voor Cluster resourcemanager moeten kunnen het beste plaatsing voor replica's van toepassingen services bieden.

## <a name="how-not-to-use-application-capacity"></a>Het niet gebruik van de capaciteit van toepassingen

-   Gebruik de capaciteit van de toepassing niet naar de toepassing op een specifiek deel van knooppunten beperken: hoewel Service stof zorgt ervoor dat het dat het maximum aantal knooppunten rekening is voor elke toepassing met de capaciteit van de toepassingen is opgegeven, gebruikers kunnen niet bepalen welke knooppunten wordt worden gemaakt op. Dit kan worden bereikt met behulp van plaatsing beperkingen voor services.
-   Gebruik de capaciteit van de toepassing niet om ervoor te zorgen dat twee services van dezelfde toepassing altijd naast elkaar worden geplaatst. Dit kan worden bereikt met behulp van affiniteit relatie tussen services en affiniteit kan worden beperkt tot de services die daadwerkelijk samen moeten worden geplaatst.

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over de andere opties die beschikbaar zijn voor het configureren van services uitchecken het onderwerp op de andere Cluster resourcemanager-configuraties beschikbaar [meer informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)
- Voor meer informatie over hoe de resourcemanager Cluster beheert en saldi laden in het cluster, raadpleegt u het artikel over [het verdelen van laden](service-fabric-cluster-resource-manager-balancing.md)
- Starten vanuit het begin en [Bekijk de inleiding bij de Service Cluster Resource Manager stof](service-fabric-cluster-resource-manager-introduction.md)
- Voor meer informatie over de manier waarop de doelstellingen in het algemeen werken, meer lezen over [De doelstellingen van Service stof laden](service-fabric-cluster-resource-manager-metrics.md)
- De Manager van de Resource Cluster heeft een groot aantal opties voor de beschrijving van het cluster. Als u wilt weten meer informatie over deze in dit artikel bij het [beschrijven van een Service stof cluster](service-fabric-cluster-resource-manager-cluster-description.md) hebt uitchecken


[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
