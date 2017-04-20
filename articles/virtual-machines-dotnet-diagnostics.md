<properties
    pageTitle="Het gebruik van Azure diagnostische gegevens in de virtuele Machines | Microsoft Azure"
    description="Met behulp van Azure diagnostische gegevens verzamelen van gegevens van Azure virtuele Machines voor foutopsporing, meten prestaties, controle, verkeer analyse en meer."
    services="virtual-machines"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Diagnostische gegevens in Azure virtuele Machines inschakelen

Zie [Overzicht van de Azure-diagnostische hulpprogramma's](azure-diagnostics.md) voor een achtergrond op Azure diagnostische gegevens.

## <a name="how-to-enable-diagnostics-in-a-virtual-machine"></a>Diagnostische gegevens in een virtuele Machine inschakelen

Deze stapsgewijze beschrijving wordt beschreven hoe extern diagnostische gegevens aan een Azure virtuele machines te installeren vanaf een computer ontwikkeling. Ook leert u hoe u een toepassing die wordt uitgevoerd op die Azure virtuele machine en genereert met de .NET [EventSource Class][]telemetriegegevens implementeert. Azure diagnostische gegevens wordt gebruikt voor het verzamelen van het telemetrielogboek en opslaan in een Azure opslag-account.

### <a name="pre-requisites"></a>Minimumvereisten
Deze stapsgewijze beschrijving wordt ervan uitgegaan dat u Azure-abonnement hebt en Visual Studio 2013 gebruikt met de SDK Azure. Als u niet een Azure-abonnement hebt, kunt u zich aanmeldt voor de [Gratis proefversie][]. Zorg ervoor dat [installeren en configureren van Azure PowerShell versie 0.8.7 of hoger][].

### <a name="step-1-create-a-virtual-machine"></a>Stap 1: Een virtuele Machine maken
1.  Op uw computer ontwikkeling, start u Visual Studio-2013.
2.  Klik in de Visual Studio **Server Explorer** **Azure**uitvouwen, met de rechtermuisknop op **virtuele Machines** en selecteer **VM maken**.
3.  Selecteer uw Azure-abonnement in het dialoogvenster **Kies een abonnement** en klik op **volgende**.
4.  **Windows Server 2012 R2 Datacenter, November 2014** in het dialoogvenster **Selecteer de afbeelding van een virtuele Machine** en klik op **volgende**.
5.  In de **Virtuele Machine basisinstellingen**, moet u de naam van de virtuele machine naar "wadexample" instellen. Stel uw beheerder-gebruikersnaam en wachtwoord in en klik op **volgende**.
6.  Maak een nieuwe cloudservice met de naam 'wadexampleVM' in het dialoogvenster **Cloud Service-instellingen** . Maak een nieuwe opslag-account met de naam "wadexample" en klik op **volgende**.
7.  Klik op **maken**.

### <a name="step-2-create-your-application"></a>Stap 2: Uw toepassing maken
1.  Op uw computer ontwikkeling, start u Visual Studio-2013.
2.  Maak een nieuwe Visual C#-Console-toepassing die is bedoeld voor .NET Framework 4.5. Geef het project "WadExampleVM".
    ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3.  De inhoud van Program.cs vervangen door de volgende code. De klasse **SampleEventSourceWriter** implementeert vier logboekregistratiemethoden: **SendEnums**, **MessageMethod**, **SetOther** en **HighFreq**. De eerste parameter voor de methode WriteEvent Hiermee definieert u de ID voor de desbetreffende gebeurtenis. De methode Run implementeert een lus dat u elk van de logboekregistratiemethoden geïmplementeerd in de klas **SampleEventSourceWriter** elke 10 seconden belt.

        using System;
        using System.Diagnostics;
        using System.Diagnostics.Tracing;
        using System.Threading;

        namespace WadExampleVM
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

        class Program
        {
        static void Main(string[] args)
        {
            Trace.TraceInformation("My application entry point called");

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
        }
        }


4.  Sla het bestand en selecteer **Build Solution** in het menu **Build** maken van uw code.


### <a name="step-3-deploy-your-application"></a>Stap 3: Uw toepassing implementeren
1.  Met de rechtermuisknop op het project **WadExampleVM** in **Solution Explorer** en kies **De map openen in Verkenner**.
2.  Navigeer naar de map *bin\Debug* en kopieer alle bestanden (WadExampleVM.*)
3.  In **Server Explorer** met de rechtermuisknop op de virtuele machine en kies **verbinding maken met behulp van extern bureaublad**.
4.  Wanneer een verbinding met de VM WadExampleVM en plakken die uw toepassing naar de map bestanden mappen te maken.
5.  Start de toepassing WadExampleVM.exe. Hier ziet u een lege consolevenster.

### <a name="step-4-create-your-diagnostics-configuration-and-install-the-extension"></a>Stap 4: De configuratie van uw diagnostische maken en de extensie installeren
1.  De schemadefinitie van de openbare configuratie-bestand downloaden naar uw computer ontwikkeling door de volgende PowerShell-opdracht uit te voeren:

        (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'

2.  Open een nieuwe XML-bestand in Visual Studio, hetzij in een project dat u al hebt geopend of in een Visual Studio exemplaar met geen geopende projecten. Selecteer **toevoegen**in Visual Studio, -> **Nieuw Item...**  ->  **Visual C# items** -> **gegevens** -> **XML-bestand**. De bestandsnaam "WadExample.xml"
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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a>Stap 5: Extern installeren diagnostische hulpprogramma's op uw Azure virtuele machines
Zijn de PowerShell-cmdlets voor het beheren van diagnostische gegevens op een VM: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension en verwijderen-AzureVMDiagnosticsExtension.

1.  Open op uw computer ontwikkelaars Azure PowerShell.
2.  Het script om te kunnen installeren op afstand diagnostische hulpprogramma's op uw VM (vervangen *StorageAccountKey* met de accountsleutel opslagruimte voor uw account van de opslagruimte wadexamplevm) uitvoeren:

        $storage_name = "wadexamplevm"
        $key = "<StorageAccountKey>"
        $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
        $service_name="wadexamplevm"
        $vm_name="WadExample"
        $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
        $VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
        $VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
        $VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM


### <a name="step-6-look-at-your-telemetry-data"></a>Stap 6: Kennismaking met uw telemetriegegevens
Ga naar het wadexample opslag-account in Visual Studio **Server Explorer** . Nadat de VM al actief ongeveer 5 minuten ziet u de tabellen **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** en **WADSetOtherTable**. Dubbelklik op een van de tabellen om weer te geven van de telemetrielogboek die zijn verzameld.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Configuratie bestand schema

Het configuratiebestand diagnostische hulpprogramma's definieert waarden die worden gebruikt voor diagnostische configuratie-instellingen geïnitialiseerd wanneer de hulpprogramma's voor diagnose-agent wordt gestart. Zie de [meest recente Naslaggids voor het schema](https://msdn.microsoft.com/library/azure/mt634524.aspx) voor geldige waarden en voorbeelden.

## <a name="troubleshooting"></a>Problemen oplossen

Zie [Azure diagnostisch hulpprogramma probleemoplossing](azure-diagnostics-troubleshooting.md) voor meer informatie.


## <a name="next-steps"></a>Volgende stappen
[Zie een lijst met VM verwante artikelen van Azure diagnostische hulpprogramma's](azure-diagnostics.md#virtual-machines-using-azure-diagnostics) om te wijzigen van de gegevens die u verzamelt, problemen oplossen of meer informatie over diagnostische gegevens in het algemeen.


[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Gratis proefversie]: http://azure.microsoft.com/pricing/free-trial/
[Installeren en configureren van Azure PowerShell versie 0.8.7 of hoger]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
