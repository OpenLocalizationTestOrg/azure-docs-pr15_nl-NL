<properties
   pageTitle="Planning van de capaciteit van de cluster Service stof | Microsoft Azure"
   description="Service stof cluster capaciteit overwegingen bij de planning. Nodetypes, levensduur en betrouwbaarheid lagen"
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


# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Service stof cluster capaciteit overwegingen bij de planning

Capaciteitsplanning is een belangrijke stap voor elke productie-implementatie. Hier zijn enkele van de items die u hebt u rekening moet houden als onderdeel van het proces.

- Het aantal knooppunttypen die uw cluster beginnen moet met
- De eigenschappen van elk knooppunttype (grootte, primair, internet die tegenover elkaar liggen, aantal VMs, enz.)
- De betrouwbaarheid en levensduur kenmerken van het cluster

Laat ons kort controleren elk van deze items.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Het aantal knooppunttypen die uw cluster beginnen moet met

Eerst moet u bepalen wat het cluster die u maakt dient te worden gebruikt voor en welke soorten toepassingen u van plan bent om te implementeren in dit cluster. Als u niet wissen op het doel van het cluster bent, u waarschijnlijk nog niet gereed voor het invoeren van de capaciteit planningsproces.

Het aantal knooppunttypen die uw cluster beginnen moet met definiëren.  Elk knooppunttype is toegewezen aan een virtuele Machine schaal instellen. Elk knooppunttype vervolgens kan worden vergroot omhoog of omlaag verschillende groepen poorten open belooft hebt en de doelstellingen van de verschillende capaciteit kan hebben. Zodat het besluit van het aantal knooppunttypen in principe wordt geleverd omlaag naar de volgende punten:

- Heeft uw toepassing meerdere services en moet een van deze worden openbaar of verbonden met internet? Standaard-toepassingen bevatten een front gatewayservice die invoer ontvangt van een client en een of meer back-enddatabase services die communiceren met de front-services. Zo in dat geval uiteindelijk u ten minste twee knooppunt typen.

- Hebben uw services (waaruit de toepassing) verschillende infrastructuur behoeften zoals groter RAM of hoger CPU-bewerkingen? Laat ons Stel dat de toepassing die u wilt implementeren een front-end-service en een back-enddatabase-service bevat. De front-service kan worden uitgevoerd op kleinere VMs (VM formaten zoals D2) die poorten zijn geopend met internet.  De back-enddatabase-service, echter is veel berekenen en moet worden uitgevoerd op grotere VMs (met de grootte van de VM zoals D4, D6, D15) die niet internet zijn die tegenover elkaar liggen.

 In dit voorbeeld, hoewel u kunt ervoor zorgen dat alle services op één knooppunttype, is het raadzaam dat u deze in een cluster met twee typen van knooppunten plaatsen.  Hiermee kunt voor elk knooppunttype distinct eigenschappen zoals internetconnectiviteit of VM grootte heeft. Het aantal VMs kan worden vergroot onafhankelijk, ook.  

- Aangezien u kunt geen de toekomst voorspellen, gaat u met u van weet feiten en bepalen van het aantal knooppunttypen die uw toepassingen beginnen moeten. U kunt altijd toevoegen of verwijderen van knooppunttypen later. Een Service stof cluster moet ten minste één knooppunttype hebben.

## <a name="the-properties-of-each-node-type"></a>De eigenschappen van elk knooppunttype

Het **knooppunttype** kan worden beschouwd als gelijk aan rollen in de Cloud Services. Knooppunttypen definiëren de VM-grootte, het aantal VMs en hun eigenschappen. Elk knooppunttype die is gedefinieerd in een cluster Service stof is ingesteld als een afzonderlijke VM schaal instellen. VM schaal Sets zijn een Azure resource die u kunt gebruiken om te implementeren en beheren van een verzameling virtuele machines als een set berekenen. Wordt gedefinieerd als afzonderlijke VM schaal ingesteld, elk knooppunttype vervolgens kan worden vergroot omhoog of omlaag verschillende groepen poorten open belooft hebt en de doelstellingen van de verschillende capaciteit kan hebben.

Uw cluster kan meer dan één knooppunttype, maar het type primaire knooppunt (de eerste fase die u op de portal definieert) moet ten minste vijf VMs voor clusters die wordt gebruikt voor productie werkbelasting (of ten minste drie VMs voor test clusters). Als u het cluster met behulp van een sjabloon resourcemanager maakt, vindt u een **primaire** -kenmerk onder de definitie van de type knooppunt. Het type primaire knooppunt is het knooppunttype waar Service systeem configuratieservices worden geplaatst.  

### <a name="primary-node-type"></a>Type primaire knooppunt
Voor een cluster met meerdere typen van knooppunten moet u kiezen voor een van deze moeten primaire. Hier volgen de kenmerken van een type primaire knooppunt:

- De minimale grootte van VMs voor het type primaire knooppunt wordt bepaald door de levensduur laag die u kiest. De standaardinstelling voor de laag levensduur is Brons. Schuif omlaag voor meer informatie over wat de levensduur laag is en de waarden die kan duren.  

- Het minimum aantal VMs voor het type primaire knooppunt wordt bepaald door de betrouwbaarheid laag die u kiest. De standaardinstelling voor de laag betrouwbaarheid is Silver. Schuif omlaag voor meer informatie over wat de betrouwbaarheid laag is en de waarden die kan duren.

- De Service systeem configuratieservices (bijvoorbeeld de Cluster Manager-service of een afbeelding van de Store-service) op het type primaire knooppunt worden geplaatst en dus de betrouwbaarheid en de levensduur van het cluster wordt bepaald door de betrouwbaarheid laag waarde en de levensduur laag waarde u voor het type primaire knooppunt.

![Schermafbeelding van een cluster met twee typen van knooppunten ][SystemServices]


### <a name="non-primary-node-type"></a>Niet-primaire knooppunttype
Voor een cluster met meerdere typen van knooppunten, er één primaire knooppunttype en de rest van deze niet-primaire. Hier volgen de kenmerken van een niet-primaire knooppunttype:

- De minimale grootte van VMs voor dit knooppunttype wordt bepaald door de levensduur laag die u kiest. De standaardinstelling voor de laag levensduur is Brons. Schuif omlaag voor meer informatie over wat de levensduur laag is en de waarden die kan duren.  

- Het minimum aantal VMs voor dit knooppunttype zijn. U kunt dit nummer op basis van het aantal kopieën van de toepassing/services die u wilt uitvoeren in dit knooppunttype echter moet kiezen. Het aantal VMs in een knooppunttype kan worden verhoogd nadat u het cluster hebt geïmplementeerd.


## <a name="the-durability-characteristics-of-the-cluster"></a>De kenmerken van de levensduur van het cluster

De laag levensduur wordt gebruikt om aan te geven aan het systeem de bevoegdheden die uw VMs met de onderliggende Azure infrastructuur hebt. Typ in het primaire knooppunt kan deze bevoegdheid Service stof onderbreken VM niveau infrastructuur verzoeken (zoals een VM opnieuw opstarten, VM reimage of VM migratie) die van invloed zijn op de quorumvoorschriften voor de systeemservices en uw statuscontrole services. Met deze bevoegdheid kan in de knooppunttypen niet-primaire Service stof elk verzoek VM niveau infrastructuur bijvoorbeeld VM opnieuw opstarten, VM reimage, VM migratie, enzovoort, die van invloed zijn op de quorumvoorschriften voor uw statuscontrole services worden uitgevoerd in deze onderbreken.

Deze bevoegdheid wordt uitgedrukt in de volgende waarden:

- Goud - de infrastructuur taken kan worden onderbroken voor een duur van 2 uur per per

- Silver - de infrastructuur taken kan worden onderbroken voor een duur van 30 minuten per per (dit is momenteel niet ingeschakeld voor gebruik. Eenmaal ingeschakeld Dit is beschikbaar op alle standaard VMs van één core en hoger).

- Bronze - geen bevoegdheden. Dit is de standaardwaarde.

## <a name="the-reliability-characteristics-of-the-cluster"></a>De betrouwbaarheidskenmerken van de van het cluster

De laag betrouwbaarheid wordt gebruikt voor het instellen van het aantal kopieën van de systeemservices die u wilt uitvoeren in dit cluster op het type primaire knooppunt. De meer het aantal replica's, de betrouwbare zijn de systeemservices in uw cluster.  

De laag betrouwbaarheid kan duren voordat de volgende waarden.

- Platinum - uitvoeren van de systeem-services met een doel replicaset tellen van 9

- Goud - uitvoeren van de systeem-services met een doel replicaset telling van 7

- Silver - uitvoeren van de systeem-services met een doel replicaset telling van 5

- Bronze - uitvoeren van de systeem-services met een doel replicaset tellen van 3

>[AZURE.NOTE] De betrouwbaarheid laag die u kiest bepaalt het minimum aantal knooppunten die uw type primaire knooppunt moet hebben. De laag betrouwbaarheid heeft geen gevolgen voor de maximale grootte van het cluster. Zodat u een cluster met 20 knooppunt hebt kunt, wordt die bij Bronze betrouwbaarheid uitgevoerd.

 U kunt de betrouwbaarheid van uw cluster met één niveau naar een andere bijwerken. Hierdoor wordt geactiveerd de upgrades cluster instellen die nodig zijn om te wijzigen van de systeem services replica tellen. Wacht totdat de upgrade is uitgevoerd om te voltooien voordat u andere wijzigingen aan het cluster, zoals het toevoegen van knooppunten enzovoort.  U kunt de voortgang van de upgrade op Service stof Explorer of door te voeren [Get-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt126012.aspx) controleren

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

Wanneer u klaar bent met de capaciteitsplanning van de en stel een cluster, lees de volgende handelingen uit:
- [Service stof cluster beveiliging](service-fabric-cluster-security.md)
- [Service stof systeemstatus model Inleiding](service-fabric-health-introduction.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
