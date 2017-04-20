<properties
    pageTitle="De stroom in een Cloud Services-toepassing met diagnostisch hulpprogramma Azure aanwijzen | Microsoft Azure"
    description="Berichten voor tracering toevoegen aan een Azure-toepassing om te helpen foutopsporing, meten prestaties, controle, verkeer analyse en meer."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>De stroom van een Cloud Services-toepassing met diagnostisch hulpprogramma Azure aanwijzen

Tracering is een manier om te controleren van de uitvoering van een toepassing terwijl deze wordt uitgevoerd. Informatie over fouten en uitvoeren van toepassingen opnemen in Logboeken, tekstbestanden of andere apparaten voor latere analyse kunt u de klassen [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx)en [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) . Zie voor meer informatie over tracering [tracering en implementeren van toepassingen](https://msdn.microsoft.com/library/zs6s4h68.aspx).


## <a name="use-trace-statements-and-trace-switches"></a>Trace-instructies en doelcellen schakelopties gebruiken

Implementeer tracing in de Cloud Services-toepassing door de [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) toevoegen aan de configuratie van de toepassing en het plaatsen van oproepen naar System.Diagnostics.Trace of System.Diagnostics.Debug in uw toepassingscode. Gebruik de configuratie bestand *app.config* voor werknemer rollen en *web.config* voor web rollen. Wanneer u een nieuwe gehoste service met een Visual Studio-sjabloon maakt, Azure diagnostische gegevens automatisch toegevoegd aan het project en de DiagnosticMonitorTraceListener wordt toegevoegd aan het juiste configuratiebestand voor de rollen die u toevoegt.

Zie voor informatie over het plaatsen van trace-instructies, [hoe: Trace-instructies toevoegen aan toepassingscode](https://msdn.microsoft.com/library/zd83saa2.aspx).

[Doelcellen schakelopties](https://msdn.microsoft.com/library/3at424ac.aspx) in code plaatst, kunt u bepalen of tracering plaatsvindt en hoe de uitgebreide is. Hiermee kunt u de status van uw toepassing in een productieomgeving controleren. Dit is vooral belangrijk in een business-toepassing die meerdere onderdelen op meerdere computers gebruikt. Zie voor meer informatie [hoe: configureren doelcellen schakelopties](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>De trace luisteraar ervan af in een Azure-toepassing configureren

Trace blijkt, foutopsporing en TraceSource, moet u instellen "listeners" verzamelen en vastleggen van de berichten die worden verzonden. Listeners verzamelen, opslaan en doorsturen van berichten voor tracering. Ze verwijzen de tracering uitvoer aan een juiste doel, zoals een logboek, venster of tekstbestand. Azure diagnostische gegevens worden gebruikt voor de klas [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) .

Voordat u de volgende procedure hebt voltooid, moet u de Azure diagnostische monitor geïnitialiseerd. Zie [Diagnostisch hulpprogramma inschakelen in Microsoft Azure](cloud-services-dotnet-diagnostics.md)hiervoor.

Houd er rekening mee dat als u de sjablonen die worden verstrekt door Visual Studio gebruikt, de configuratie van de luisteraar ervan af is toegevoegd automatisch voor u.


### <a name="add-a-trace-listener"></a>Een spoor luisteraar ervan af toevoegen

1. Open het bestand web.config of app.config voor uw rol.
2. Voeg de volgende code naar het bestand. Wijzig het kenmerk versie als u wilt gebruiken het versienummer van de vergadering waarnaar u verwijst. De constructie-versie ongewijzigd niet per se met elke versie van Azure SDK tenzij er updates zijn.

    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                  <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
    >[AZURE.IMPORTANT] Zorg ervoor dat u hebt een projectverwijzing naar de vergadering Microsoft.WindowsAzure.Diagnostics. Het versienummer weergegeven in de bovenstaande xml zodat deze overeenkomen met de versie van de waarnaar wordt verwezen Microsoft.WindowsAzure.Diagnostics constructie bijwerken.

3. Sla het configuratiebestand.

Zie voor meer informatie over listeners, [Traceer-Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Nadat u de stappen voor het toevoegen van de luisteraar ervan af hebt voltooid, kunt u de instructies trace toevoegen aan uw code.


### <a name="to-add-trace-statement-to-your-code"></a>Traceringsinstructie toevoegen aan uw code

1. Een bronbestand voor uw toepassing openen. Bijvoorbeeld de <RoleName>:. cs-bestand voor de werknemer rol of Webrol.
2. Voeg de volgende met de instructie als dit nog niet is toegevoegd:
    ```
        using System.Diagnostics;
    ```
3. Voeg instructies Trace waarin u wilt vastleggen van informatie over de status van uw toepassing. U kunt verschillende methoden gebruiken om de uitvoer van de instructie Trace. Zie voor meer informatie [hoe: Trace-instructies toevoegen aan toepassingscode](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Sla het bronbestand.
