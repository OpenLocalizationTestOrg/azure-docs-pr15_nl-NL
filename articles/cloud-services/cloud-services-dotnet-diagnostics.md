<properties
    pageTitle="Het gebruik van Azure diagnostische gegevens (.NET) met Cloudservices | Microsoft Azure"
    description="Met behulp van Azure diagnostische gegevens verzamelen van gegevens van Azure Cloudservices voor foutopsporing, meten prestaties, controle, verkeer analyse en meer."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="01/25/2016"
    ms.author="robb"/>



# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Azure diagnostische gegevens van Azure Cloudservices inschakelen

Zie [Overzicht van de Azure-diagnostische hulpprogramma's](../azure-diagnostics.md) voor een achtergrond op Azure diagnostische gegevens.


## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Het inschakelen van diagnostische gegevens in een rol werknemer

Deze stapsgewijze beschrijving beschreven hoe u een rol Azure werknemer die met de klasse .NET EventSource telemetriegegevens genereert implementeren. Azure diagnostische gegevens wordt gebruikt voor het verzamelen van de telemetriegegevens en opslaan in een Azure opslag-account. Bij het maken van een rol werknemer kunt Visual Studio automatisch Diagnostics 1.0 als onderdeel van de oplossing in Azure SDK's voor .NET 2,4 en eerdere versies. De volgende instructies beschrijven het proces voor het maken van de rol van werknemer, Diagnostics 1.0 uitschakelen van de oplossing, en het implementeren van diagnostische gegevens 1.2 of 1.3 aan uw rol werknemer.

### <a name="pre-requisites"></a>Minimumvereisten
In dit artikel wordt ervan uitgegaan dat u hebt een Azure-abonnement en Visual Studio 2013 gebruikt met de SDK Azure. Als u niet een Azure-abonnement hebt, kunt u zich aanmeldt voor de [Gratis proefversie][]. Zorg ervoor dat [installeren en configureren van Azure PowerShell versie 0.8.7 of hoger][].

### <a name="step-1-create-a-worker-role"></a>Stap 1: Maak een rol werknemer
1.  Start **Visual Studio-2013**.
2.  Maak een nieuw project van **Azure-Cloudservice** vanuit de **Cloud** -sjabloon die doelen .NET Framework 4.5.  Naam van het project "WadExample" en klik op Ok.
3.  Selecteer **Werknemer rol** en klik op Ok. Het project wordt gemaakt.
4.  Dubbelklik op het bestand **WorkerRole1** -eigenschappen in **Solution Explorer**.
5.  Tab Schakel het selectievakje **Diagnostische hulpprogramma's inschakelen** om uit te schakelen Diagnostics 1.0 (Azure SDK 2,4 en eariler) in de **configuratie** .
6.  Maak uw oplossing om te bevestigen dat u geen fouten hebt.

### <a name="step-2-instrument-your-code"></a>Stap 2: Uw code Instrument
De inhoud van WorkerRole.cs vervangen door de volgende code. De klasse SampleEventSourceWriter, overgenomen van de [EventSource Class][], implementeert vier logboekregistratiemethoden: **SendEnums**, **MessageMethod**, **SetOther** en **HighFreq**. De eerste parameter voor de methode **WriteEvent** Hiermee definieert u de ID voor de desbetreffende gebeurtenis. De methode Run implementeert een lus dat u elk van de logboekregistratiemethoden geïmplementeerd in de klas **SampleEventSourceWriter** elke 10 seconden belt.

    using Microsoft.WindowsAzure.ServiceRuntime;
    using System;
    using System.Diagnostics;
    using System.Diagnostics.Tracing;
    using System.Net;
    using System.Threading;

    namespace WorkerRole1
    {
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
    }


### <a name="step-3-deploy-your-worker-role"></a>Stap 3: Uw rol werknemer implementeren
1.  Uw rol werknemer dashboard implementeren naar Azure uit binnen Visual Studio het project **WadExample** door in te schakelen en de oplossing Explorer vervolgens **publiceren** in het menu **maken** .
2.  Kies uw abonnement.
3.  Selecteer **Nieuw**in het dialoogvenster **Microsoft Azure Publish Settings** .
4.  Voer in het dialoogvenster **Cloudservice maken en opslag Account** een **naam** (bijvoorbeeld "WadExample") en selecteer een regio of affiniteit groep.
5.  Stel de **omgeving** op **tijdelijke**.
6.  Wijzig desgewenst ook andere **Instellingen** zo nodig en klik op **publiceren**.
7.  Na implementatie Controleer of in de portal van Azure klassieke dat uw cloudservice zich in een staat **uitgevoerd** .

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>Stap 4: Het configuratiebestand diagnostische hulpprogramma's maken en de extensie installeren
1.  Download de schemadefinitie van de openbare configuratie-bestand door de volgende PowerShell-opdracht uit te voeren:
2.
        (Get-AzureServiceAvailableExtension - Extensienaam 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics'). PublicConfigurationSchema | Uitgaande bestand-codering utf8 - bestandspad 'WadConfig.xsd'

2.  Een XML-bestand toevoegen aan uw project **WorkerRole1** door met de rechtermuisknop op het project **WorkerRole1** en selecteer **toevoegen** -> **Nieuw Item...**  ->  **Visual C# items** -> **gegevens** -> **XML-bestand**. Geef het bestand 'WadExample.xml'.

    ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)

3.  Koppel de WadConfig.xsd aan het configuratiebestand. Controleer of dat het venster WadExample.xml editor het actieve venster is. Druk op **F4** om het venster **Eigenschappen** te openen. Klik op de eigenschap **schema's maken** in het venster **Eigenschappen** . Klik op de **...** in de eigenschap **schema's** . Klik op de **toevoegen...** knop en navigeer naar de locatie waar u het XSD-bestand hebt opgeslagen en selecteer het bestand WadConfig.xsd. Klik op **OK**.
4.  De inhoud van het configuratiebestand WadExample.xml vervangen door de volgende XML en sla het bestand. Dit configuratiebestand definieert de enkele prestatiemeteritems voor het verzamelen van: een voor CPU-gebruik en een voor geheugengebruik. Klik in de configuratie de vier gebeurtenissen die overeenkomt met de methoden in de klas SampleEventSourceWriter wordt gedefinieerd.

```
        <?xml version="1.0" encoding="utf-8"?>
        <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
            <WadCfg>
                <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
                <PerformanceCounters scheduledTransferPeriod="PT1M">
                    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
                    <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
                    </PerformanceCounters>
                    <EtwProviders>
                        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
                            <Event id="1" eventDestination="EnumsTable"/>
                            <Event id="2" eventDestination="MessageTable"/>
                            <Event id="3" eventDestination="SetOtherTable"/>
                            <Event id="4" eventDestination="HighFreqTable"/>
                            <DefaultEvents eventDestination="DefaultTable" />
                        </EtwEventSourceProviderConfiguration>
                    </EtwProviders>
                </DiagnosticMonitorConfiguration>
            </WadCfg>
        </PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Stap 5: Hulpprogramma's voor diagnose installeren op uw rol werknemer
De PowerShell-cmdlets voor het beheren van diagnostische gegevens op een webpagina of werknemer rol zijn: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension en verwijderen-AzureServiceDiagnosticsExtension.

1.  Open Azure PowerShell.
2.  Het script om diagnostische hulpprogramma's installeren op uw rol werknemer ( *StorageAccountKey* vervangen door de accountsleutel opslagruimte voor uw account van de opslagruimte wadexample) uitvoeren:

```
    $storage_name = "wadexample"
    $key = "<StorageAccountKey>"
    $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
    $service_name="wadexample"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Stap 6: Kennismaking met uw telemetriegegevens
Ga naar het wadexample opslag-account in Visual Studio **Server Explorer** . Na de cloud wordt service uitgevoerd ongeveer 5 minuten ziet u de tabellen **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** en **WADSetOtherTable**. Dubbelklik op een van de tabellen om weer te geven van de telemetrielogboek die zijn verzameld.
    ![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)


## <a name="configuration-file-schema"></a>Configuratie bestand Schema

Het configuratiebestand diagnostische hulpprogramma's definieert waarden die worden gebruikt voor diagnostische configuratie-instellingen geïnitialiseerd wanneer de hulpprogramma's voor diagnose-agent wordt gestart. Zie de [meest recente Naslaggids voor het schema](https://msdn.microsoft.com/library/azure/mt634524.aspx) voor geldige waarden en voorbeelden.

## <a name="troubleshooting"></a>Problemen oplossen

Als u problemen ondervindt, raadpleegt u [Diagnostische gegevens van Azure problemen oplossen](../azure-diagnostics-troubleshooting.md) voor hulp bij veelvoorkomende problemen.

## <a name="next-steps"></a>Volgende stappen
[Zie een lijst met VM verwante artikelen van Azure diagnostische hulpprogramma's](azure-diagnostics.md#cloud-services) om te wijzigen van de gegevens die u verzamelt, problemen oplossen of meer informatie over diagnostische gegevens in het algemeen.


[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Gratis proefversie]: http://azure.microsoft.com/pricing/free-trial/
[Installeren en configureren van Azure PowerShell versie 0.8.7 of hoger]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
