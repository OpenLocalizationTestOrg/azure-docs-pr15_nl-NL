<properties
   pageTitle="De schaal van een Service stof cluster in- of uitzoomen aanpassen | Microsoft Azure"
   description="De schaal van een Service stof cluster in- of uitzoomen aanpassen zodat deze overeenkomt met de aanvraag door in te stellen automatisch schalen regels voor elke set knooppunt type/VM schaal. Toevoegen of verwijderen van knooppunten aan een cluster Service stof"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>De schaal van een Service stof cluster in- of uitzoomen met behulp van regels voor Automatische-schaal aanpassen

VM schaal sets zijn een Azure berekeningscluster resource die u gebruiken kunt om te implementeren en beheren van een verzameling virtuele machines als een set. Elk knooppunttype die is gedefinieerd in een cluster Service stof is ingesteld als een afzonderlijke VM schaal set. Elk knooppunttype vervolgens kan worden aangepast of uit verschillende groepen poorten open belooft hebt en de doelstellingen van de verschillende capaciteit kan hebben. Meer informatie over deze in het document [Service stof nodetypes](service-fabric-cluster-nodetypes.md) . Aangezien de Service-stof knooppunttypen in uw cluster worden aangebracht van VM schaal wordt ingesteld op de backend, moet u regels voor automatisch schalen voor elke set knooppunt type/VM schaal instellen.

>[AZURE.NOTE] Uw abonnement moet voldoende cores om toe te voegen van de nieuwe VMs waaruit dit cluster hebben. Er is geen validatie, zodat u een tijd implementatie is mislukt, als een van de quotalimieten worden bereikt.

## <a name="choose-the-node-typevm-scale-set-to-scale"></a>Het knooppunt type/VM schaal instellen aan de nieuwe schaal kiezen

U bent momenteel niet kunt opgeven van de regels automatisch schalen voor VM schaal sets met behulp van de portal, dus laat ons gebruiken Azure PowerShell (1.0 +) om te knooppunttypen lijst en voegt u regels voor automatische-kleurenschaal toe aan deze.

Als u de lijst met VM schaal instellen waarmee u het cluster, voert u de volgende cmdlets uit:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <VM Scale Set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevm-scale-set"></a>Automatisch schalen regels voor de verzameling type/VM schaal instellen

Als uw cluster meerdere typen van knooppunten heeft, herhaalt u dat deze optie voor elke schaal van de typen/VM knooppunt ingesteld dat u schalen wilt (in- of uitzoomen). Rekening houden met het aantal knooppunten dat u hebt nodig voordat u een auto-schaal instellen. Het minimum aantal knooppunten die u voor het type primaire knooppunt hebt nodig wordt bepaald door de betrouwbaarheid niveau dat u hebt gekozen. Lees meer over [betrouwbaarheid niveaus](service-fabric-cluster-capacity.md).

>[AZURE.NOTE]  Het verkleinen van het primaire knooppunt typt in minder dan het minimale getal tabelmaakquery het cluster vastloopt of dit omlaag te openen. Dit kan resulteren in verlies van gegevens voor uw toepassingen en voor de systeemservices.

De functie automatisch schalen is momenteel niet aangestuurd door de laadtijd dat uw toepassingen mogelijk worden meldt bij Service stof. Op dit moment de auto-schaal u zuiver wordt bepaald door de prestaties zo schalen items die zijn gegenereerd door elk van de VM set exemplaren.  

Volg deze instructies [voor het instellen van auto-schaal voor elke set van de schaal VM](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

>[AZURE.NOTE] In een schaal omlaag scenario, tenzij uw knooppunttype een niveau levensduur van goud of Silver heeft moet u de [cmdlet verwijderen-ServiceFabricNodeState](https://msdn.microsoft.com/library/azure/mt125993.aspx) met de knooppuntnaam van de juiste bellen.

## <a name="manually-add-vms-to-a-node-typevm-scale-set"></a>VMs handmatig toevoegen aan een verzameling knooppunten type/VM schaal

Volg de steekproef/instructies in de [galerie met sjablonen van aan de slag](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) om het aantal VMs in elke Nodetype te wijzigen. 

>[AZURE.NOTE] Toevoegen van VMs duurt, zodat de toevoegingen moeten momentopname niet verwacht. Zo wilt toevoegen van capaciteit ook in de tijd, om te staan voor meer dan 10 minuten voordat de capaciteit VM beschikbaar voor de replica's is / service-exemplaren moet krijgen geplaatst.

## <a name="manually-remove-vms-from-the-primary-node-typevm-scale-set"></a>Verwijder handmatig VMs vanuit het primaire knooppunt type/VM schaal instellen

>[AZURE.NOTE] Het hulpprogramma voor het systeem van de service-configuratieservices uitvoeren in het type primaire knooppunt in uw cluster. Er garandeert kleiner dan wat de betrouwbaarheid laag zodat nooit moet afgesloten of het aantal exemplaren in dat knooppunttypen verkleinen. Raadpleeg [de details van betrouwbaarheid lagen hier](service-fabric-cluster-capacity.md). 

U moet de volgende stappen uit één VM exemplaar per keer uitvoeren. In deze sectie geeft voor de systeemservices (en uw statuscontrole services) worden afsluiten op de VM exemplaar die u wilt verwijderen en de nieuwe probleem op andere knooppunten.

1. [Uitschakelen-ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) uitvoeren met de bedoeling 'RemoveNode' voor het uitschakelen van het knooppunt dat u wilt verwijderen (de hoogste exemplaar in dat knooppunttype).

2. Voer [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) om ervoor te zorgen dat het knooppunt daadwerkelijk heeft overgebracht naar uitgeschakeld. Als dat niet zo is, wacht totdat het knooppunt is uitgeschakeld. U kunt geen wacht niet langer deze stap.

2. Volg de steekproef/instructies in de [galerie met sjablonen van aan de slag](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) om het aantal VMs door een in die Nodetype te wijzigen. De hoogste VM exemplaar is van het exemplaar verwijderd. 

3. Herhaal stap 1 tot en met 3 naar wens, maar nooit schalen dat het aantal exemplaren in de primaire knooppunttypen die kleiner is dan wat de laag betrouwbaarheid garandeert. Raadpleeg [de details van betrouwbaarheid lagen hier](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevm-scale-set"></a>Verwijder handmatig VMs vanuit de niet-primaire knooppunt type/VM schaal instellen

>[AZURE.NOTE] Voor een statuscontrole service moet u een bepaald aantal knooppunten worden altijd tot het behoud van beschikbaarheid en de status van uw service behouden. Aan de zeer minimum moet u het aantal knooppunten gelijk is aan het doel replica set aantal van de partition/service. 

Moet u het uitvoeren van de volgende stappen uit één VM exemplaar tegelijk. In deze sectie geeft voor de systeemservices (en uw statuscontrole services) worden afsluiten op het VM exemplaar dat u wilt verwijderen en nieuwe replica's waar u nog meer gemaakt.

1. [Uitschakelen-ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) uitvoeren met de bedoeling 'RemoveNode' voor het uitschakelen van het knooppunt dat u wilt verwijderen (de hoogste exemplaar in dat knooppunttype).

2. Voer [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) om ervoor te zorgen dat het knooppunt daadwerkelijk heeft overgebracht naar uitgeschakeld. Als dit niet wachten tot het knooppunt is uitgeschakeld. U kunt geen wacht niet langer deze stap.

2. Volg de steekproef/instructies in de [galerie met sjablonen van aan de slag](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) om het aantal VMs door een in die Nodetype te wijzigen. Hiermee wordt het hoogste VM exemplaar nu verwijderd. 

3. Herhaal stap 1 tot en met 3 naar wens, maar nooit schalen dat het aantal exemplaren in de primaire knooppunttypen die kleiner is dan wat de laag betrouwbaarheid garandeert. Raadpleeg [de details van betrouwbaarheid lagen hier](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Gedrag kunnen zich in de Service stof Explorer

Wanneer u een cluster schaal worden de Service stof Explorer het aantal knooppunten (VM schaal ingesteld exemplaren) die deel van het cluster uitmaken doorgevoerd.  Wanneer u de schaal wordt een cluster omlaag u echter, raadpleegt u het verwijderde knooppunt/VM exemplaar weergegeven in een beschadigde status, tenzij u [verwijderen ServiceFabricNodeState cmd](https://msdn.microsoft.com/library/mt125993.aspx) met de knooppuntnaam van de juiste bellen.   

Hier is de uitleg van dit probleem.

De knooppunten weergegeven in de Service stof Explorer zijn een weerspiegeling van wat de Service systeem configuratieservices (FM specifiek) op de hoogte van het aantal knooppunten het cluster had/heeft. Wanneer u de schaal van de VM vastgestelde wilt verkleinen, de VM is verwijderd, maar de systeemservice FM nog steeds denkt dat het knooppunt (die is toegewezen aan de VM die is verwijderd) wordt keert u terug. Service stof Explorer blijft zo dat knooppunt weergeven (als de status mogelijk fout of onbekend).

Om ervoor te zorgen dat een knooppunt wordt verwijderd wanneer een VM wordt verwijderd, hebt u twee opties:

1) Kies het machtigingsniveau dat levensduur van goud of Silver (beschikbaar spoedig) voor de knooppunttypen in uw cluster, waardoor u de infrastructuur-integratie. Die vervolgens wordt automatisch verwijderd de knooppunten van onze services (FM) systeemstatus wanneer u de schaal van omlaag.
Verwijzen naar [de informatie over hier levensduur niveaus](service-fabric-cluster-capacity.md)

2) Zodra het exemplaar VM zijn verkleind, moet u de [cmdlet verwijderen-ServiceFabricNodeState](https://msdn.microsoft.com/library/mt125993.aspx)bellen.

>[AZURE.NOTE] Service stof clusters vereisen een bepaald aantal knooppunten moet zijn van de tijd om te kunnen voor het behoud van beschikbaarheid en bewaren van state - genoemd "onderhouden quorum." Zo is, is het meestal onveilige alle computers in het cluster afsluiten, tenzij u eerst een [volledige back-up van uw status](service-fabric-reliable-services-backup-restore.md)hebt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
Lees de volgende ook meer informatie over het plannen van cluster capaciteit, een cluster upgraden en partitioneren services:

- [De capaciteit van de cluster plannen](service-fabric-cluster-capacity.md)
- [Cluster upgrades](service-fabric-cluster-upgrade.md)
- [Partition statuscontrole services voor maximale schaal](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
