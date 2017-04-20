<properties
   pageTitle="Testbaarheid actie | Microsoft Azure"
   description="In dit artikel moment over de testbaarheid acties die zijn gevonden in Microsoft Azure-Service stof spreekt."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="timlt"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/03/2016"
   ms.author="motanv;heeldin"/>

# <a name="testability-actions"></a>Testbaarheid acties
Om te kunnen een onbetrouwbare infrastructuur simuleren, kunt Azure-Service stof u, de ontwikkelaar, met manieren kunt u verschillende echte fouten en staat overgangen simuleren. Deze zijn beschikbaar als testbaarheid acties. De acties zijn de laag niveau API's die ertoe leiden dat een specifieke foutenstructuuranalyse webweergave, staat overgang of gegevensvalidatie. Deze acties worden gecombineerd, kunt u volledig test scenario's schrijven voor de services.

Service stof vindt u dat enkele veelvoorkomende scenario's voor testen op basis van deze acties. Het is raadzaam dat u deze ingebouwde scenario's die zorgvuldig gekozen zijn om te testen van de algemene statusovergangen en mislukt dozen gebruiken. Acties kunnen echter worden gebruikt voor het maken van aangepaste test scenario's als u wilt invoegen dekking voor scenario's die niet schuilgaan achter de ingebouwde scenario's nog of die zijn aangepaste afgestemd op uw toepassing.

C#-implementaties van de acties zijn in de vergadering System.Fabric.dll gevonden. De systeem stof PowerShell-module vindt u in de vergadering Microsoft.ServiceFabric.Powershell.dll. De ServiceFabric PowerShell-module is geïnstalleerd als onderdeel van runtime-installatie, om toe te staan voor gebruiksgemak.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Correcte versus geforceerde afsluiting foutenstructuuranalyse acties
Testbaarheid acties worden ingedeeld in twee belangrijkste gerangschikte verzamelingen:

* Geforceerde afsluiting fouten: deze fouten simuleren fouten als de computer opnieuw opstarten en loopt verwerken. In dat geval van fouten, wordt de uitvoeringscontext van proces onverwacht stopt. Dit betekent dat er geen opschoning van de staat kan worden uitgevoerd voordat de toepassing opnieuw is gestart.

* Correcte fouten: deze fouten simuleren correcte acties zoals replica worden verplaatst en val door taakverdeling geactiveerd. In dat geval de service wordt een melding van de sluiten en de stand kunt opschonen voordat u afsluit.

Voor betere kwaliteit validatie, uitvoeren, de werklast van de antwoordgroepservice en zakelijke terwijl verschillende correcte en geforceerde afsluiting fouten veroorzaken. Geforceerde afsluiting fouten oefenen scenario's waarin het serviceproces onverwacht in het midden van een werkstroom afgesloten. Dit het pad van het herstelproces is getest wanneer de replica service is hersteld door de Service stof. Hierdoor consistentie van de gegevens en of de servicestatus goed worden onderhouden na fouten testen. De andere set fouten (de correcte fouten) test dat de service correct reageert op replica's rond worden verplaatst door de Service stof. Dit afhandeling van annulering van de methode RunAsync gecontroleerd. U moet de service kunt controleren voor de annulering token wordt ingesteld, wordt de staat de juiste manier opslaan en sluiten van de methode RunAsync.

## <a name="testability-actions-list"></a>De lijst Acties testbaarheid

| Actie | Beschrijving | Beheerde API | PowerShell-cmdlet | Correcte/geforceerde afsluiting fouten |
|---------|-------------|-------------|-------------------|------------------------------|
|CleanTestState| Hiermee verwijdert alle de test staat uit het cluster voor het geval een onjuist afsluiten van het stuurprogramma test. | CleanTestStateAsync | Verwijderen ServiceFabricTestState | Niet van toepassing |
| InvokeDataLoss | Verlies van gegevens induceert in een service partition. | InvokeDataLossAsync | Roepen ServiceFabricPartitionDataLoss | Correcte |
| InvokeQuorumLoss | Een bepaald statuscontrole service partition activeert in quorum verlies. | InvokeQuorumLossAsync | Roepen ServiceFabricQuorumLoss | Correcte |
| Primair verplaatsen | Hiermee verplaatst u de opgegeven primaire replica van een statuscontrole service naar het knooppunt opgegeven cluster. | MovePrimaryAsync | Verplaatsen-ServiceFabricPrimaryReplica | Correcte |
| Secundaire verplaatsen | Hiermee verplaatst u de huidige secundaire replica van een statuscontrole service naar een ander clusterknooppunt. | MoveSecondaryAsync | Verplaatsen-ServiceFabricSecondaryReplica | Correcte |
| RemoveReplica | Een fout replica simuleert doordat een replica uit een cluster. Hiermee wordt de replica gesloten en wordt deze overgang naar rol 'Geen', verwijderen van alle van de status van het cluster. | RemoveReplicaAsync | Verwijderen ServiceFabricReplica | Correcte |
| RestartDeployedCodePackage | Een code pakket proces is mislukt simuleert door opnieuw te starten een code-pakket dat is geïmplementeerd op een knooppunt in een cluster. Hiermee wordt het proces voor het pakket van code, dat alle gebruikers service replica's die worden gehost in het proces wordt opnieuw afgebroken. | RestartDeployedCodePackageAsync | Opnieuw starten-ServiceFabricDeployedCodePackage | Geforceerde afsluiting |
| RestartNode | Een Service stof cluster knooppunt uitvalt simuleert door opnieuw een knooppunt te starten. | RestartNodeAsync | Opnieuw starten-ServiceFabricNode | Geforceerde afsluiting |
| RestartPartition | Een scenario voor datacenter wordt zwart of cluster wordt zwart simuleert door opnieuw te starten sommige of alle kopieën van een partition. | RestartPartitionAsync | Opnieuw starten-ServiceFabricPartition | Correcte |
| RestartReplica | Een fout replica simuleert door opnieuw starten van een permanente replica in een cluster, de replica sluiten en klik vervolgens opnieuw te openen. | RestartReplicaAsync | Opnieuw starten-ServiceFabricReplica | Correcte |
| Beginknooppunt | Hiermee start u een knooppunt in een cluster die al is gestopt. | StartNodeAsync | Start-ServiceFabricNode | Niet van toepassing |
| StopNode | Een fout knooppunt simuleert door een knooppunt in een cluster stoppen. Het knooppunt blijven omlaag totdat beginknooppunt wordt genoemd. | StopNodeAsync | Stop-ServiceFabricNode | Geforceerde afsluiting |
| ValidateApplication | De beschikbaarheid en de status van alle Service configuratieservices binnen een toepassing, meestal na enkele foutenstructuuranalyse veroorzaken in het systeem is gevalideerd. | ValidateApplicationAsync | Test-ServiceFabricApplication | Niet van toepassing |
| ValidateService | De beschikbaarheid en de status van een service-Service stof, meestal na enkele foutenstructuuranalyse veroorzaken in het systeem is gevalideerd. | ValidateServiceAsync | Test-ServiceFabricService | Niet van toepassing |

## <a name="running-a-testability-action-using-powershell"></a>De actie van een testbaarheid via PowerShell uitgevoerd

Deze zelfstudie ziet u hoe u een actie testbaarheid uitvoeren via PowerShell. Hier leert u hoe u een actie testbaarheid voor een lokale (één-box) cluster of een Azure cluster uitgevoerd. Microsoft.Fabric.Powershell.dll--de Service stof PowerShell-module--automatisch wordt geïnstalleerd tijdens de installatie van de Microsoft-Service stof MSI. Wanneer u een PowerShell-prompt opent, wordt de module wordt automatisch geladen.

Zelfstudievideo segmenten:

- [Een actie voor een cluster een en-klare uitgevoerd](#run-an-action-against-a-one-box-cluster)
- [Een actie voor een Azure cluster uitgevoerd](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Een actie voor een cluster een en-klare uitgevoerd

Als u wilt een actie testbaarheid voor een lokale cluster uitgevoerd, eerst verbinding maken met het cluster en de PowerShell-prompt openen in de beheerdersmodus voor. Laat ons kijkt u naar de actie **Opnieuw starten-ServiceFabricNode** .

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Hier wordt de actie **Opnieuw starten-ServiceFabricNode** wordt uitgevoerd op een knooppunt met de naam "Knooppunt1". De modus voltooiing geeft aan dat deze niet controleren moet of de actie opnieuw starten knooppunten daadwerkelijk geslaagd. De modus voltooiing geven als "Verifiëren" gaan om te controleren of de actie opnieuw starten daadwerkelijk geslaagd. In plaats van het knooppunt direct onder de naam op te geven, kunt u deze via een partitiesleutel en het soort replica, als volgt:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Opnieuw starten-ServiceFabricNode** moet worden gebruikt om een Service stof knooppunt in een cluster opnieuw te starten. Hiermee stopt u het proces Fabric.exe, waarin alle het systeem Services en service replica's die worden gehost op dat knooppunt opnieuw starten. Deze API gebruiken om te testen van uw service helpt bugs bij te werken langs de failover herstel paden schuiven. Is het handig simuleren knooppuntfouten in het cluster.

De volgende schermafbeelding ziet u de opdracht **Opnieuw starten-ServiceFabricNode** -testbaarheid in actie.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

De uitvoer van de eerste **Get-ServiceFabricNode** (een cmdlet uit de Service stof PowerShell-module) dat aangeeft dat de lokale cluster vijf knooppunten er: Node.1 naar Node.5. Nadat de actie testbaarheid (cmdlet) **Opnieuw starten-ServiceFabricNode** wordt uitgevoerd op het knooppunt, met de naam Node.4, zien we dat de beschikbaarheid van het knooppunt opnieuw is ingesteld.

### <a name="run-an-action-against-an-azure-cluster"></a>Een actie voor een Azure cluster uitgevoerd

Een actie testbaarheid (via PowerShell) uitgevoerd op een Azure cluster is vergelijkbaar met het uitvoeren van de actie ten opzichte van een lokale cluster. De enige verschil is dat voordat u de actie, in plaats van een verbinding maken met de lokale cluster kunt uitvoeren moet u eerst verbinding maken met het Azure cluster.

## <a name="running-a-testability-action-using-c35"></a>Actief testbaarheid van een andere actie met C & #35;

Als u wilt een testbaarheid actie uitvoeren met behulp van C#, moet u eerst verbinding maken met het cluster via FabricClient. Moet u de parameters die nodig zijn voor de actie uitvoeren. Verschillende parameters kunnen worden gebruikt voor dezelfde actie uitvoeren.
De actie RestartServiceFabricNode bekijkt, is een manier om te worden uitgevoerd met behulp van de knooppuntgegevens (de knooppuntnaam van het en knooppunt exemplaar-ID) in het cluster.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Uitleg van de parameter:

- **CompleteMode** geeft aan dat de modus niet controleren moet of de actie opnieuw starten daadwerkelijk geslaagd. De modus voltooiing geven als "Verifiëren" gaan om te controleren of de actie opnieuw starten daadwerkelijk geslaagd.  
- **OperationTimeout** Hiermee stelt u de hoeveelheid tijd voor de bewerking voltooid voordat een uitzondering TimeoutException wordt gegenereerd.
- **CancellationToken** kunt een in behandeling oproep te annuleren.

In plaats van het knooppunt direct onder de naam op te geven, kunt u deze via een partitiesleutel en het soort replica opgeven.

Zie [PartitionSelector en ReplicaSelector](#partition_replica_selector)voor meer informatie.


```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>PartitionSelector en ReplicaSelector

### <a name="partitionselector"></a>PartitionSelector
PartitionSelector is een helper die staan weergegeven in testbaarheid en selecteert u een specifieke partition waarop u wilt een van de testbaarheid acties uitvoert. Dit kan worden gebruikt om te selecteren een specifieke partition Als de ID van de partition vooraf bekend is. Of, kunt u de toets partition en de bewerking intern de partition-ID worden opgelost. U hebt ook een willekeurig partition kiezen.

Als u deze helper, het object PartitionSelector maken en selecteert u de partition met behulp van een van de manieren Selecteer *. Laat u vervolgens in het object PartitionSelector tot de API dat vereist is. Als de optie geen is geselecteerd, wordt standaard een willekeurig partition.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>ReplicaSelector
ReplicaSelector is een helper die staan weergegeven in testbaarheid en wordt gebruikt om te selecteren van een replica waarop u wilt een van de testbaarheid acties uitvoert. Dit kan worden gebruikt om te selecteren een specifieke replica als de replica-ID vooraf bekend is. Bovendien hebt u de optie van het selecteren van een primaire replica of een willekeurig secundair. ReplicaSelector afgeleid van PartitionSelector, zodat u nodig hebt om zowel de replica en de partition waarop u wilt uitvoeren van de bewerking testbaarheid te selecteren.

Als u deze helper, een object ReplicaSelector maken en instellen van de manier waarop u wilt de replica en de partition selecteren. U kunt deze vervolgens doorgeven naar de API dat vereist is. Als de optie geen is geselecteerd, wordt standaard een willekeurig replica en willekeurige partition.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Volgende stappen

- [Testbaarheid scenario 's](service-fabric-testability-scenarios.md)
- Het testen van uw service
   - [Fouten tijdens de service-werkbelastingen simuleren](service-fabric-testability-workload-tests.md)
   - [Fouten in de service-naar-service](service-fabric-testability-scenarios-service-communication.md)
