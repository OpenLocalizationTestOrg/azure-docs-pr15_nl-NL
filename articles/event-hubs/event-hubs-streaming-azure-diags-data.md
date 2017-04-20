<properties
    pageTitle="Diagnostisch hulpprogramma Azure-gegevens in het warm pad met gebeurtenis Hubs streaming | Microsoft Azure"
    description="Ziet u hoe diagnostisch hulpprogramma Azure configureren met gebeurtenis Hubs van begin tot eind, waaronder richtlijnen voor veelvoorkomende scenario's."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/14/2016"
    ms.author="sethm" />

# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>Diagnostisch hulpprogramma Azure-gegevens in het warm pad met behulp van de gebeurtenis Hubs streaming

Azure diagnostische gegevens kunt flexibele manieren maatstaven en logboeken verzamelen van cloud services virtuele machines (VMs) en resultaten overbrengen naar Azure Storage. Starten in de periode maart 2016 (SDK 2,9), kunt u diagnostische gegevens vangen met volledig aangepaste gegevensbronnen en padgegevens van warm overbrengen in seconden met behulp van [Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/).

Ondersteunde gegevenstypen bevatten:

- Gebeurtenis Aanwijzen voor Windows (ETW) gebeurtenissen
- Prestatie-items
- Gebeurtenislogboeken van Windows
- Toepassingslogboeken aan de
- Azure diagnostisch hulpprogramma infrastructuur Logboeken

In dit artikel leest u Diagnostisch hulpprogramma Azure configureren met gebeurtenis Hubs van begin tot eind. Richtlijnen is ook beschikbaar voor de volgende algemene scenario's:

- Het aanpassen van de logboeken en aan de doelstellingen die sinked met Hubs gebeurtenis
- Het wijzigen van configuraties in elke omgeving
- Hoe gebeurtenis Hubs stream gegevens moeten worden weergegeven
- Problemen met de verbinding oplossen  

## <a name="prerequisites"></a>Vereisten voor

Gebeurtenis-Hubs zich in Azure diagnostische hulpprogramma's wordt ondersteund in de Cloud Services, VMs VM schaal Sets en Service stof vanaf de Azure SDK 2,9 en de bijbehorende Azure-hulpprogramma's voor Visual Studio.

- Azure diagnostisch hulpprogramma extensie 1,6 ([Azure SDK voor .NET 2,9 of hoger](https://azure.microsoft.com/downloads/) is bedoeld voor dit al dan niet standaard)
- [Visual Studio 2013 of hoger](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- Bestaande configuraties van Azure diagnostische gegevens in een toepassing met behulp van een bestand *.wadcfgx* en een van de volgende manieren:
    - Visual Studio: [diagnostische hulpprogramma's voor Azure Cloudservices en virtuele Machines configureren](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
    - Windows PowerShell: [hulpprogramma's voor diagnose in Azure-Cloudservices via PowerShell inschakelen](../cloud-services/cloud-services-diagnostics-powershell.md)
- Gebeurtenis Hubs naamruimte deze is ingericht per het artikel [aan de slag met Hubs gebeurtenis](./event-hubs-csharp-ephcs-getstarted.md)

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>Diagnostisch hulpprogramma Azure verbinden met gebeurtenis Hubs sink

Diagnostische gegevens van Azure sinks altijd logboeken en aan de doelstellingen, al dan niet standaard, met een opslag van Azure-account. Een toepassing mogelijk bovendien vangen met gebeurtenis Hubs door een nieuwe **Sinks** sectie toe te voegen aan het element **WadCfg** in de sectie **PublicConfig** van het bestand *.wadcfgx* . Visual Studio, het *.wadcfgx* -bestand is opgeslagen in het volgende pad: **Cloud Service Project** > **rollen** >  **(functienaam)** > **diagnostics.wadcfgx** -bestand.

```
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```

In dit voorbeeld de URL van de gebeurtenis Hub is ingesteld op de volledig gekwalificeerde naamruimte van de Hub gebeurtenis: gebeurtenis Hubs naamruimte + "/" + gebeurtenis Hub-naam.  

De URL van de gebeurtenis Hub wordt weergegeven in de [portal van Azure](http://go.microsoft.com/fwlink/?LinkID=213885) op het dashboard gebeurtenis Hubs.  

De naam van de **vangen** kan worden ingesteld op een willekeurige geldige tekenreeks zo lang maken als dezelfde waarde consistente door het configuratiebestand wordt gebruikt.

> [AZURE.NOTE]  Er zijn mogelijk extra sinks, zoals *applicationInsights* geconfigureerd in deze sectie. Azure diagnostische gegevens kan een of meer sinks naar deze velden worden gedefinieerd als elke sink ook in de sectie **PrivateConfig aangegeven wordt** .  

De gebeurtenis Hubs sink moet ook worden gedeclareerd en gedefinieerd in de sectie **PrivateConfig** van het configuratiebestand *.wadcfgx* .

```
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="" key="" endpoint="" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="9B3SwghJOGEUvXigc6zHPInl2iYxrgsKHZoy4nm9CUI=" />
</PrivateConfig>
```

De `SharedAccessKeyName` waarde moet overeenkomen met een gedeeld Access handtekening (SA's) sleutel en het beleid die is gedefinieerd in de naamruimte **Gebeurtenis Hubs** . Blader naar het dashboard gebeurtenis Hubs in de [Azure-portal](https://manage.windowsazure.com), klik op het tabblad **configureren** en een benoemde beleid (bijvoorbeeld ' SendRule') met machtigingen *verzenden* instellen. De **StorageAccount** wordt ook gedeclareerd in **PrivateConfig**. Er is geen nodig om waarden hier wijzigen als ze werken. In dit voorbeeld laat wordt de waarden leeg, namelijk een teken dat een volgende activum de waarden worden ingesteld. Voorbeeld wordt de configuratiebestand *ServiceConfiguration.Cloud.cscfg* omgeving de omgeving specifieke namen en de toetsen.  

> [AZURE.WARNING] De sleutel gebeurtenis Hubs SA's wordt opgeslagen als tekst zonder opmaak in het bestand *.wadcfgx* . Vaak deze toets naar broncode beheren is ingecheckt of is beschikbaar als een actief in de opbouwen-server, zodat u deze zo nodig moet beschermen. Het is raadzaam die u hier een sleutel voor SA's met machtigingen voor *alleen verzenden* , gebruikt zodat een kwaadwillende gebruiker kunt op de gebeurtenis-Hub schrijven, maar die niet worden afgespeeld of beheren.

## <a name="configure-azure-diagnostics-logs-and-metrics-to-sink-with-event-hubs"></a>Diagnostisch hulpprogramma Azure-logboeken en aan de doelstellingen om op te vangen met gebeurtenis Hubs configureren

Zoals eerder is beschreven, alle standaard en aangepaste naar diagnostische gegevens, dat wil zeggen wordt maatstaven en zich aanmeldt, automatisch sinked met Azure Storage de geconfigureerde intervallen worden aangegeven. Met gebeurtenis Hubs en eventuele aanvullende sink, kunt u een van de hoofdsite of knooppuntniveau knooppunten in de hiërarchie worden sinked met de Hub gebeurtenis. Dit geldt ook voor ETW gebeurtenissen, prestatiemeteritems gebeurtenislogboeken van Windows en toepassingslogboeken aan de.   

Het is belangrijk u rekening moet houden hoeveel gegevenspunten daadwerkelijk moeten worden overgebracht naar gebeurtenis Hubs. Meestal overbrengen ontwikkelaars lage latentie direct pad gegevens die moeten worden verbruikt en snel beschouwd. Systemen die waarschuwingen of automatisch schalen regels bewaken zijn voorbeelden. Een ontwikkelaar mogelijk ook een alternatieve analyse winkel configureren of zoek winkel--bijvoorbeeld Azure Stream analyses, Elasticsearch, systeem van een aangepaste toezicht of een systeem van favoriete toezicht van anderen.

Hier volgen enkele voorbeeldconfiguraties.

```
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
</PerformanceCounters>
```

Klik in het volgende voorbeeld wordt wordt de sink toegepast op de bovenliggende **PerformanceCounters** knooppunt in de hiërarchie, wat inhoudt dat alle onderliggende **PerformanceCounters** worden verzonden naar gebeurtenis Hubs.  

```
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```

In het vorige voorbeeld, de sink wordt toegepast op slechts drie items: **Aanvragen in wachtrij**, **Aanvragen geweigerd**en **% processortijd**.  

Het volgende voorbeeld ziet hoe een ontwikkelaar de hoeveelheid verzonden gegevens moeten de kritieke criteria die worden gebruikt voor deze servicestatus van kunt beperken.  

```
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```

In dit voorbeeld de sink wordt toegepast op Logboeken en alleen voor fout niveau doelcellen wordt gefilterd.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Implementeren en bijwerken van een Cloud Services-toepassing en diagnostische zoekconfiguratie

Visual Studio biedt het eenvoudigste pad om te implementeren van de toepassing en gebeurtenis Hubs sink configuratie. Als u wilt weergeven en bewerken van het bestand, open het bestand *.wadcfgx* in Visual Studio, bewerken en opslaan. Het pad is **Cloud Service Project** > **rollen** > **(functienaam)** > **diagnostics.wadcfgx**.  

Nu alle implementatie en implementatie bijwerken acties in Visual Studio, Visual Studio Team System, en alle opdrachten of scripts die zijn gebaseerd op MSBuild en gebruik de **/t: publiceren** doel de *.wadcfgx* opnemen in de verpakking-proces. Daarnaast implementeren implementaties en updates het bestand naar Azure met behulp van de juiste diagnostisch hulpprogramma Azure agentextensie op uw VMs.

Nadat u de toepassing en configuratie van Azure diagnostische implementeert, ziet u direct activiteit in het dashboard van de Hub gebeurtenis. Hiermee wordt aangegeven dat u doorgaan wilt naar de gegevens direct pad in het hulpmiddel luisteraar ervan af-client of analyse van uw keuze bekijken.  

In de volgende afbeelding ziet het dashboard gebeurtenis Hubs orde verstuurd naar diagnostische gegevens op de gebeurtenis-Hub beginnend ergens na 11 PM. Als de toepassing met een bijgewerkte *.wadcfgx* -bestand is geïmplementeerd en de sink correct is geconfigureerd.

![][0]  

> [AZURE.NOTE] Wanneer u updates aanbrengt in het configuratiebestand Azure diagnostische gegevens (.wadcfgx), het raadzaam dat u de updates naar de hele toepassing, evenals de configuratie met behulp van Visual Studio publiceren, of een Windows PowerShell-script push.  

## <a name="view-hot-path-data"></a>De gegevens direct pad weergeven

Zoals eerder besproken, zijn er vaak gebruiken voor beluisteren en gebeurtenis Hubs gegevensverwerking.

Eén eenvoudige manier is om een kleine test console-toepassing te luisteren naar de gebeurtenis Hub en afdrukken van de stream uitvoer te maken. U kunt de volgende code, waarin wordt uitgelegd in [aan de slag met gebeurtenis Hubs](./event-hubs-csharp-ephcs-getstarted.md)uitgebreider worden plaatsen), in een consoletoepassing.  

Houd er rekening mee dat de consoletoepassing de [gebeurtenis Processor Host Nuget pakket](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/)moet bevatten.  

Onthoud dat de waarden in de punthaken in de functie **Hoofdgegeven** vervangen door de waarden voor uw resources.   

```
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event Hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sink"></a>Problemen met gebeurtenis Hubs sink

- De Hub gebeurtenis wordt niet weergegeven voor binnenkomende of uitgaande gebeurtenisactiviteit zoals verwacht.

    Controleer of uw Hub gebeurtenis is is ingericht. Alle verbindingsgegevens in de sectie **PrivateConfig** van *.wadcfgx* moet overeenkomen met de waarden van de resource zoals gezien in de portal. Zorg ervoor dat er een gedefinieerd SA's-beleid ('SendRule' in het voorbeeld) in de portal en dat de machtiging *verzenden* is verleend.  

- Na een update ziet u de gebeurtenis Hub niet meer gebeurtenisactiviteit voor binnenkomende of uitgaande.

    Controleer eerst of dat de gegevens van de gebeurtenis Hub- en configuratieservices correct zoals eerder is. Soms is het **PrivateConfig** opnieuw instellen in een update implementatie. De aanbevolen correctie is alle wijzigingen aanbrengen in *.wadcfgx* in het project en vervolgens push een update van de volledige toepassing. Als dit niet mogelijk is, zorg dat de update diagnostische hulpprogramma's een volledige **PrivateConfig** waarin de toets SA's verplaatst.  

- Ik heb de suggesties hebt geprobeerd en de gebeurtenis Hub nog niet werkt.

    Kijk in de tabel Azure Storage met Logboeken en fouten oplossen voor diagnostisch hulpprogramma Azure zelf: **WADDiagnosticInfrastructureLogsTable**. Eén optie is een hulpprogramma zoals [Azure opslag Explorer](http://www.storageexplorer.com) gebruiken om te verbinden met dit account opslag, weergeven in deze tabel en een query voor tijdstempel toevoegen in de afgelopen 24 uur. Het hulpmiddel kunt u een CSV-bestand exporteren en deze in een toepassing zoals Microsoft Excel openen. Excel kunt u gemakkelijk om te zoeken naar tekenreeksen met de-kaart, zoals **EventHubs**, om te zien welke fout gerapporteerd.  

## <a name="next-steps"></a>Volgende stappen

• [Meer informatie over de gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Bijlage: Voltooien diagnostisch hulpprogramma Azure configuratie-bestand (.wadcfgx) voorbeeld

```
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount />
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" key="" endpoint="" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

De bijbehorende *ServiceConfiguration.Cloud.cscfg* voor dit voorbeeld ziet er als volgt uit.

```
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<!-- Images. -->
[0]: ./media/event-hubs-streaming-azure-diags-data/dashboard.png
