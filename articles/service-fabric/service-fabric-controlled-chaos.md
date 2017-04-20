<properties
   pageTitle="Chaos veroorzaken in Service stof clusters | Microsoft Azure"
   description="Gebruik foutenstructuuranalyse webweergave en Cluster Analysis Service API's voor het beheren van Chaos in het cluster."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="rsinha"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="motanv"/>

# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Beheerde Chaos in Service stof clusters veroorzaken
Grote gedistribueerde systemen alsof cloud infrastructuur inherent onbetrouwbare. Azure-Service stof kunnen ontwikkelaars naar betrouwbaar services op een onbetrouwbare infrastructuur schrijven. Als u wilt schrijven krachtige services, moeten ontwikkelaars kunnen fouten tegen die onbetrouwbare infrastructuur test de stabiliteit van hun services veroorzaken.

De fout webweergave en Cluster analyse-Service (ook wel bekend als de fout Analysis Service) biedt ontwikkelaars de mogelijkheid om te foutenstructuuranalyse acties om te testen services veroorzaken. Echter krijgt gerichte gesimuleerd fouten u alleen dusverre. Als u maakt de testen verder, kunt u Chaos.

Chaos simuleert continue, afwisselende fouten (correcte en geforceerde afsluiting) overal in het cluster via langere tijd. Nadat u met het rentepercentage en het soort fouten Chaos hebt geconfigureerd, kunt u starten of stoppen dit tot en met C#-API's of PowerShell voor het genereren van fouten in het cluster en uw service.

Terwijl Chaos actief is, is het resultaat van verschillende gebeurtenissen die de status van de uitvoeren op het moment dat vastleggen. Een ExecutingFaultsEvent bevat bijvoorbeeld alle fouten die wordt uitgevoerd in die iteratie. Een ValidationFailedEvent bevat de details van problemen die tijdens de cluster validatie is gevonden. U kunt de API GetChaosReportAsync om het rapport van Chaos uitgevoerd aanroepen.

## <a name="faults-induced-in-chaos"></a>Fouten die in Chaos
Chaos genereert fouten in het hele Service stof cluster en worden gecomprimeerd fouten die zichtbaar zijn in maanden of jaren in een paar uur. De combinatie van afwisselende fouten met het tarief weer dat hoge foutenstructuuranalyse vindt hoek zaken die anders zijn gemist. Deze oefening Chaos leidt tot een aanzienlijk verbetering van de kwaliteit van de code van de service.

Chaos induceert fouten uit de volgende categorieën:

 - Start een knooppunt opnieuw
 - Opnieuw een pakket geïmplementeerd code
 - Een replica verwijderen
 - Opnieuw een replica
 - Een primaire replica (te configureren) verplaatsen
 - Een secundaire replica (te configureren) verplaatsen

Chaos wordt uitgevoerd in meerdere iteraties. Elke iteratie bestaat uit fouten en cluster validatie voor de opgegeven periode. U kunt de tijd wordt gebruikt voor het cluster gestabiliseerd en voor validatie te kunnen uitvoeren. Als een fout in cluster gegevensvalidatie wordt gevonden, wordt Chaos genereert en zich blijft voordoen een ValidationFailedEvent met de UTC-tijdstempel en de details is mislukt.

Stel dat een exemplaar van Chaos die is ingesteld om uit te voeren voor een uur van maximaal drie gelijktijdige fouten. Chaos induceert drie fouten en klikt u vervolgens de status cluster is gevalideerd. Doorlopen de vorige stap totdat deze expliciet gestopt via de API StopChaosAsync of één uur worden doorgegeven. Als het cluster in elke iteratie beschadigd raakt (dat wil zeggen het bevat niet stabiliseren binnen een geconfigureerde tijd), Chaos genereert een ValidationFailedEvent. Deze gebeurtenis wordt aangegeven dat er iets een opgetreden fout is en verder onderzoek wellicht.

In de huidige vorm induceert Chaos alleen veilige fouten. Dit betekent dat het geen externe fouten, quorum winst of verlies van gegevens nooit plaatsvindt.

## <a name="important-configuration-options"></a>Belangrijke configuratieopties
 - **TimeToRun**: totale tijd dat Chaos wordt uitgevoerd voordat deze met succes is voltooid. U kunt Chaos stoppen voordat deze is uitgevoerd voor de periode TimeToRun via de API StopChaos.
 - **MaxClusterStabilizationTimeout**: de maximale hoeveelheid tijd moet wachten om door het cluster weer in orde zijn voordat u opnieuw controleert op is geïnstalleerd. Deze wacht is verkleinen van de belasting op het cluster terwijl deze worden hersteld. Controles zijn:
    - Als de status cluster is het OK
    - Als de servicestatus is het OK
    - Als de doelreplica grootte instelt is voor de service-partition bereikt
    - Die geen InBuild replica's bestaat
 - **MaxConcurrentFaults**: het maximum aantal gelijktijdige fouten die in elke iteratie veroorzaakt. De hoger het nummer, hoe meer agressieve Chaos is. Het resultaat meer complexe failovers en overgang combinaties. Chaos zorgt ervoor dat, bij afwezigheid van externe fouten, er is geen quorum verlies of gegevens, ongeacht hoe hoog of een waarde is deze configuratie heeft.
 - **EnableMoveReplicaFaults**: Hiermee schakelt u de fouten die ertoe leiden dat de primaire of secundaire replica's om te gaan of uit. Deze fouten zijn standaard uitgeschakeld.
 - **WaitTimeBetweenIterations**: de hoeveelheid wachttijd tussen iteraties, dat wil zeggen na een ronde van fouten en bijbehorende gegevensvalidatie.
 - **WaitTimeBetweenFaults**: de hoeveelheid wachttijd tussen twee opeenvolgende fouten in een herhaling.

## <a name="how-to-run-chaos"></a>Het uitvoeren van Chaos
**C#:**

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```
**PowerShell:**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
