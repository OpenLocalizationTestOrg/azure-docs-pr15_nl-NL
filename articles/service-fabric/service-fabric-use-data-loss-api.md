<properties
   pageTitle="Hoe u gegevensverlies op Service-configuratieservices roepen | Microsoft Azure"
   description="Wordt beschreven hoe u de gegevensverlies api"
   services="service-fabric"
   documentationCenter=".net"
   authors="LMWF"
   manager="rsinha"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="lemai"/>
   
# <a name="how-to-invoke-data-loss-on-services"></a>Hoe u het aanroepen van gegevensverlies op Services

>[AZURE.WARNING] In dit document wordt uitgelegd hoe u gegevensverlies in uw services en wees voorzichtig met moet worden gebruikt.

## <a name="introduction"></a>Inleiding
U kunt verlies van gegevens op een partition van uw Service stof Service door te bellen StartPartitionDataLossAsync() aanroepen.  Deze api maakt gebruik van de foutenstructuuranalyse webweergave en de analyse-Service de inzetten als u de gegevensverlies voorwaarden.

## <a name="using-the-fault-injection-and-analysis-service"></a>Gebruik van de foutenstructuuranalyse webweergave en de analyse-Service

De foutenstructuuranalyse webweergave en de analyse-Service ondersteunt momenteel de volgende API's in de onderstaande grafiek.  Aan de rechterkant van het diagram ziet u de bijbehorende PowerShell-cmdlet.  Raadpleeg de msdn-documentatie op elke API voor meer informatie over elkaar.

|           C#-API                    |         PowerShell-Cmdlet                      |
|-------------------------------------|-----------------------------------------------:|
|[StartPartitionDataLossAsync] [dl]   |[Start-ServiceFabricPartitionDataLoss] [psdl]   |
|[StartPartitionQuorumLossAsync] [ql] |[Start-ServiceFabricPartitionQuorumLoss] [psql] |
|[StartPartitionRestartAsync] [rp]    |[Start-ServiceFabricPartitionRestart] [psrp]    |

## <a name="conceptual-overview-of-running-a-command"></a>Overzicht van het uitvoeren van een opdracht

De foutenstructuuranalyse webweergave en de analyse-Service wordt gebruikgemaakt van een asynchrone model waar u de opdracht start met één API, wordt de API 'Begin' in dit document genoemd en controleert de voortgang van deze opdracht met een "GetProgress" API totdat het een terminal staat is bereikt of totdat u deze annuleren.
Als u wilt een opdracht, belt u met de API 'Begin' voor de bijbehorende API.  Deze API geeft als resultaat wanneer de foutenstructuuranalyse webweergave en de analyse-Service is geaccepteerd door het verzoek.  Echter het nummer geeft niet hoe ver een opdracht heeft uitgevoerd, of zelfs als het nog niet is gestart.  Om te controleren of er voortgang van een opdracht, belt u met de API die met de 'Begin'-API eerder genoemd overeenkomt 'GetProgress'.  De API 'GetProgress' retourneert een object waarin wordt aangegeven dat de huidige status van de opdracht binnen de provincie-eigenschap.  Een opdracht wordt uitgevoerd voor onbepaalde tijd tot:

1.  Dit is voltooid.  Als u 'GetProgress' in dit geval op is geïnstalleerd belt, wordt de voortgang van de object-staat worden voltooid.
2.  Er wordt een foutbericht aangetroffen.  Als u 'GetProgress' in dit geval op is geïnstalleerd belt, wordt de voortgang van de object-staat worden mislukt
3.  U deze via het [CancelTestCommandAsync] annuleren [ cancel] API of [Stoppen-ServiceFabricTestCommand]  [ cancelps] PowerShell-cmdlet.  Als u 'GetProgress' in dit geval op is geïnstalleerd belt, wordt van het object voortgang staat geannuleerd of ForceCancelled, zijn afhankelijk van een argument aan die API.  Zie de documentatie voor [CancelTestCommandAsync]  [ cancel] voor meer informatie.


## <a name="details-of-running-a-command"></a>Details van het uitvoeren van een opdracht

Om te beginnen op een opdracht, belt u met de API starten met de verwachte argumenten.  Alle begin-API's hebben een Guid argument bewerkingsnummer met de naam.  U moet Houd van het argument bewerkingsnummer, omdat deze wordt gebruikt voor het bijhouden van de voortgang van deze opdracht.  Dit moet worden doorgegeven naar de API 'GetProgress' om te kunnen de voortgang van de opdracht bijhouden.  De bewerkingsnummer moet uniek zijn.

Na het succes aanroepen van de API starten, moet de API GetProgress in een lus worden aangeroepen totdat de resulterende voortgang staat eigenschap is voltooid.  Alle [van FabricTransientException]  [ fte] en van OperationCanceledException moet opnieuw worden geprobeerd.
Als de opdracht heeft bereikt een terminal staat (voltooid, Faulted of geannuleerd), wordt de voortgang van de geretourneerde resultaat eigenschap aanvullende informatie hebben.  Als de status is voltooid, bevat Result.SelectedPartition.PartitionId de partition-id die is geselecteerd.  Result.Exception is leeg.  Als de status is mislukt, Result.Exception heeft de reden de webweergave foutenstructuuranalyse en Analysis Service mislukt de opdracht.  Result.SelectedPartition.PartitionId heeft de partition-id die is geselecteerd.  In sommige gevallen, de opdracht mogelijk niet verdergaat ver genoeg een partition kiezen.  De PartitionId worden in dat geval 0.  Als de status is geannuleerd, zijn Result.Exception null.  Als de zaak Faulted Result.SelectedPartition.PartitionId heeft de partition-id die is gekozen, maar als de opdracht procedure ver genoeg te doen, wordt dit 0 zijn.  Raadpleeg tevens het onderstaande voorbeeld.

De onderstaande voorbeeldcode ziet hoe u starten en schakel de voortgang van een opdracht kiezen om gegevens op een specifieke partition verloren.

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // The id of the target partition inside the target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have the chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

Het onderstaande voorbeeld ziet u hoe u met de PartitionSelector kiest u een willekeurig partition van een opgegeven service:

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have the Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## <a name="history-and-truncation"></a>Geschiedenis en afgekapt

Nadat een opdracht is bereikt, een terminal staat, blijven de metagegevens in de foutenstructuuranalyse webweergave en de analyse-Service voor een bepaald tijdstip voordat deze worden verwijderd om ruimte te besparen.  Als "GetProgress" het bewerkingsnummer van een opdracht gebruiken heet nadat u deze hebt verwijderd, wordt een FabricException met een foutcode KeyNotFound geretourneerd.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
