<properties
   pageTitle="Capaciteit, planning voor Service stof apps | Microsoft Azure"
   description="Wordt beschreven hoe u het aantal berekeningscluster knooppunten vereist voor een toepassing Service stof."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="markfuss"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>


# <a name="capacity-planning-for-service-fabric-applications"></a>Capaciteit, planning voor stof Service-toepassingen


In dit document leert u hoe voor het schatten van de hoeveelheid resources (CPU's, RAM, schijfruimte) voor het uitvoeren van uw stof van Azure-Service-toepassingen. Dit geldt voor uw resourcevereisten voor de wijzigen na verloop van tijd. U moet enkele bronnen meestal tijdens uw service ontwikkelen/testen en vervolgens meer bronnen nodig hebben terwijl u in bedrijf gaat en uw toepassing in omvang in populariteit groeit. Wanneer u uw toepassing ontwerpt, denk tot en met de lange termijn vereisten en keuzes waarmee uw service aan de nieuwe schaal om te voldoen aan hoge vraag maken.

 Wanneer u een Service stof cluster maakt, kunt u bepalen welke typen virtuele machines (VMs) van het cluster aanbrengen. Elke VM wordt geleverd voor een beperkte tijdsduur van resources in de vorm van CPU's (cores en snelheid), netwerkbandbreedte RAM en opslagruimte op een schijf. Als uw service omvang na verloop van tijd groeit, kunt u een upgrade uitvoeren naar VMs die groter bronnen en/of meer VMs toevoegen aan uw cluster. De laatste doet moet u bouwen uw service in eerste instantie zodat deze kunt profiteren van nieuwe VMs die dynamisch aan het cluster wordt toegevoegd.

Sommige services beheren weinig tot geen gegevens op de VMs zelf. Daarom moet capaciteit, planning voor deze services richten hoofdzakelijk op de prestaties, wat betekent dat de juiste CPU's (cores en snelheid) van de VMs selecteren. Bovendien raadzaam netwerkbandbreedte, inclusief hoe vaak netwerk overdrachten optreden en hoeveel gegevens worden verzonden. Als uw service uitvoeren als service gebruik toeneemt moet, kunt u meer VMs toevoegen aan het cluster en laden saldo het netwerk over alle VMs aanvraagt.

Voor de services die beheren van grote hoeveelheden gegevens op de VMs, moet capaciteitsplanning richten hoofdzakelijk op grootte. U moet dus zorgvuldig de capaciteit van de VM RAM en schijfruimte overwegen. Schijfruimte RAM eruit programmacode zorgt ervoor dat het systeem voor het beheer van virtueel geheugen in Windows. Daarnaast bevat de Service stof runtime slimme paginering alleen warm gegevens behouden in het geheugen en de koudwatersystemen gegevens verplaatsen naar schijf. Toepassingen kunnen dus gebruiken voor meer geheugen dan op de VM fysiek beschikbaar is. Hebt meer RAM gewoon Hiermee verhoogt u prestaties, aangezien de VM RAM meer opslagruimte op een schijf kunt houden. De VM die u selecteert, moet een groot genoeg voor de gegevens die u wilt opslaan op de VM schijf hebben. De VM nodig op dezelfde manier voldoende RAM u voorziet van de prestaties die u nodig hebt. Als de gegevens van uw service na verloop van tijd groeit, kunt u meer VMs toevoegen aan het cluster en partition de gegevens over alle VMs.

## <a name="determine-how-many-nodes-you-need"></a>Bepalen hoeveel knooppunten die u nodig hebt

Partitioneren van uw service, kunt u de schaal van de gegevens van uw service weggegooid aanpassen. Zie voor meer informatie over het partitioneren, [Partitioneren Service stof](service-fabric-concepts-partitioning.md). Elke partition moet passen binnen een enkel VM, maar meerdere (klein) partities kunnen in een enkel VM worden geplaatst. Ja, met meer kleine partities hebt u meer flexibiliteit dan een paar grotere partities. De verhouding is dat u veel partities wordt verhoogd Service stof realiseren en u geen uitgevoerde bewerkingen op partities uitvoeren. Er is ook meer mogelijke netwerkverkeer als uw servicecode vaak moet voor toegang tot gegevens die in verschillende partities wonen. Bij het ontwerpen van uw service, moet u deze voor- en nadelen aankomt op een effectieve partities strategie zorgvuldig overwegen.

Stel dat uw toepassing heeft een enkele statuscontrole service met een grootte store dat u verwacht te laten groeien tot DB_Size GB in een jaar. U bent bereid meer toepassingen (en partities) toevoegen als u nog steeds ondervindt groei voorbij dat jaar.  De herhaling factor (RF), die het aantal replica's voor uw service bepaalt heeft gevolgen voor de totale DB_Size. De totale DB_Size over alle replica's is de replicatie-Factor vermenigvuldigd met DB_Size.  Node_Size staat voor de schijf ruimte/RAM per knooppunt dat u wilt gebruiken voor uw service. Voor de beste prestaties het DB_Size op het cluster moet passen in het geheugen en een Node_Size die rond de RAM van VM moet worden gekozen. Door het toewijzen van een Node_Size dat groter is dan de capaciteit RAM, bent u te vertrouwen op de paginering verstrekt door de Service stof runtime. Dus uw prestaties mogelijk geen optimale als uw volledige gegevens wordt beschouwd als warm (sinds typt u vervolgens de gegevens is geheugen: in- / af). Voor veel services waar alleen een deel van de gegevens is er populair, is dit echter meest efficiënt.

Het aantal knooppunten die zijn vereist voor maximale prestaties kan als volgt worden berekend:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Account voor groei

Het is raadzaam te berekenen van het aantal knooppunten op basis van de DB_Size dat u verwacht dat uw service te laten groeien tot, naast de DB_Size waarmee u bent begonnen. Het aantal knooppunten vervolgens groeien als uw service omvang groeit, zodat u het aantal knooppunten zijn niet te veel inrichten. Maar het aantal partities moet worden gebaseerd op het aantal knooppunten die nodig zijn wanneer u uw service bij maximum groei uitvoert.

Het is handig om enkele extra machines die beschikbaar zijn op elk gewenst moment zodat u een onverwachte pieken of mislukt verwerken kunt (bijvoorbeeld als een paar VMs omlaag gaan).  Terwijl de extra capaciteit moet worden bepaald met behulp van de verwachte pieken, een beginpunt is een paar extra VMs reserveren (5-10 procent extra).

De voorafgaande wordt ervan uitgegaan een service voor eenmalige statuscontrole. Als er meer dan één statuscontrole service, die u moet de DB_Size die is gekoppeld aan de andere services in de vergelijking toevoegen. U kunt ook het aantal knooppunten afzonderlijk voor elke statuscontrole service berekenen.  Uw service mogelijk replica's of partities die niet zijn verdeeld. Houd er rekening mee dat partities meer gegevens dan andere ook mogelijk. Zie [partitioneren-artikel over aanbevolen procedures](service-fabric-concepts-partitioning.md)voor meer informatie over het partitioneren. De bovenstaande vergelijking is echter partition en replica agnostische, omdat Service stof zorgt ervoor dat de replica's zijn verdeeld tussen de knooppunten in een geoptimaliseerde wijze.


## <a name="use-a-spreadsheet-for-cost-calculation"></a>Een werkblad gebruiken voor de berekening van de kosten

Nu we enkele reële getallen in de formule. Een [voorbeeld werkblad](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) ziet hoe u het plannen van de capaciteit van een toepassing die drie soorten gegevensobjecten bevat. Voor elk object benaderen we de grootte en hoeveel objecten dat we verwacht te hebben. We ook selecteren hoeveel replica we van elk objecttype wilt. De totale hoeveelheid geheugen worden opgeslagen in het cluster worden berekend in het werkblad.

We Voer vervolgens een VM grootte en de maandelijkse kosten. Op basis van de VM-grootte, ziet het spreadsheet u het minimum aantal partities die moet u uw gegevens fysiek passend maken op de knooppunten splitsen. Gewenste een groot aantal partities voor specifieke berekening van de toepassing en netwerkverkeer nodig heeft. Het werkblad ziet u het aantal partities die de gebruikersprofiel objecten beheert heeft verhoogd van één tot zes.

Nu, op basis van deze informatie, het spreadsheet ziet die u alle gegevens met de gewenste partities en replica's op een cluster 26 knooppunten fysiek kan downloaden. Echter zou dit cluster veel verpakt worden, zodat u enkele extra knooppunten kunt voor knooppuntfouten en upgrades. Het werkblad wordt ook weergegeven dat met meer dan 57 knooppunten geen extra waarde biedt omdat u lege knooppunten wilt hebben. U wilt opnieuw, gaat u hierboven 57 knooppunten toch voor knooppuntfouten en upgrades. U kunt het spreadsheet zodat deze overeenkomen met de specifieke behoeften van uw toepassing aanpassen.   

![Werkblad voor de berekening van de kosten][Image1]



## <a name="next-steps"></a>Volgende stappen

Bekijk de [Service-configuratieservices partitioneren] [ 10] voor meer informatie over het partitioneren van uw service.



<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
