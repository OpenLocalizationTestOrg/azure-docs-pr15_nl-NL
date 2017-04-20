<properties
   pageTitle="Fouten opsporen in uw toepassing in Visual Studio | Microsoft Azure"
   description="De betrouwbaarheid en prestaties van uw services te verbeteren door te ontwikkelen en deze op een lokale ontwikkeling cluster foutopsporing in Visual Studio."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/21/2016"
   ms.author="vturecek;mikhegn"/>

# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Fouten opsporen in uw toepassing Service stof met behulp van Visual Studio

## <a name="debug-a-local-service-fabric-application"></a>Fouten opsporen in een lokale Service stof-toepassing

U kunt tijd en geld besparen door te implementeren en voor foutopsporing in uw toepassing Azure-Service stof in een lokale computer ontwikkeling cluster. Visual Studio kunt implementeren van de toepassing op de lokale cluster en verbindt automatisch de foutopsporing naar alle exemplaren van de toepassing.

1. Start een cluster plaatselijke ontwikkeling door de stappen in [het instellen van uw Service stof ontwikkelomgeving](service-fabric-get-started.md).

2. Druk op **F5** of klikt u op **fouten opsporen in** > **Foutopsporing starten**.

    ![Een toepassing foutopsporing starten][startdebugging]

3. Onderbrekingspunten instellen in uw code en de toepassing te doorlopen door te klikken op opdrachten in het menu **Foutopsporing** .

    > [AZURE.NOTE] Visual Studio koppelt aan alle exemplaren van de toepassing. Terwijl u code doorlopen bent, mogelijk onderbrekingspunten door meerdere processen met gelijktijdige sessies resultaat krijgen raken. Kunt u de onderbrekingspunten uitschakelen nadat ze raken bent, door middel van elke onderbrekingspunt voorwaardelijke op de thread-ID of met behulp van diagnostische gebeurtenissen.

4. Het venster **Diagnostische gebeurtenissen** wordt automatisch geopend zodat u diagnostische gebeurtenissen in realtime kunt bekijken.

    ![Diagnostische gebeurtenissen weergeven in realtime][diagnosticevents]

5. U kunt ook het venster **Diagnostische gebeurtenissen** in de Cloud Explorer openen.  Klik onder **Service stof**, met de rechtermuisknop op een van de knooppunten en kies **Weergave Streaming sporen**.

    ![Open het venster diagnostische gebeurtenissen][viewdiagnosticevents]

    Als u filteren traces aan een specifieke service of toepassing wilt, schakelt u gewoon streaming traces op die specifieke service of toepassing.

6. De diagnostische gebeurtenissen ziet u in de automatisch gegenereerde **ServiceEventSource.cs** -bestand en van toepassingscode worden genoemd.

    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```

7. Het venster **Diagnostische gebeurtenissen** ondersteunt filteren, onderbreken en gebeurtenissen in realtime controleren.  Het filter is een eenvoudige tekenreeks zoeken van de gebeurtenisbericht, inclusief de inhoud ervan.

    ![Filteren, onderbreken en hervatten of gebeurtenissen in realtime controleren][diagnosticeventsactions]

8. Voor foutopsporing in services is zoals voor foutopsporing in een andere toepassing. U wordt normaal onderbrekingspunten tot en met Visual Studio instellen voor eenvoudig foutopsporing. Hoewel bij een betrouwbare verzamelingen repliceren in meerdere knooppunten, implementeren ze nog steeds IEnumerable. Dit betekent dat u de weergave van de resultaten in Visual Studio kunt tijdens het opsporen om te zien wat u hebt opgeslagen in. Stel een onderbrekingspunt ergens gewoon in uw code.

    ![Een toepassing foutopsporing starten][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Fouten opsporen in een externe Service stof-toepassing

Als uw Service stof toepassingen op een Service stof cluster in Azure wordt aangegeven uitvoert, bent u kunt extern fouten opsporen in deze, rechtstreeks vanuit de Visual Studio.

> [AZURE.NOTE] De functie vereist [Service stof SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) en [Azure SDK voor .NET 2,9](https://azure.microsoft.com/downloads/).    

<!-- -->
> [AZURE.WARNING] Externe foutopsporing is bedoeld voor ontwikkelaar/testen scenario's en niet mag worden gebruikt in productieomgevingen, vanwege de invloed op de actieve toepassingen.

1. Ga naar uw cluster in de **Cloud Explorer**, met de rechtermuisknop op en kies **Inschakelen voor foutopsporing in**

    ![Foutopsporing op afstand inschakelen][enableremotedebugging]

    Hiermee wordt uitgangspunt voor het proces van het inschakelen van de externe foutopsporing extensie op uw knooppunten, evenals de vereiste netwerkconfiguraties.

2. Met de rechtermuisknop op het clusterknooppunt in de **Cloud Explorer**en kies **Foutopsporing bijvoegen**

    ![Foutopsporing bijvoegen][attachdebugger]

3. Klik in het dialoogvenster **bijvoegen verwerkingstijd** kiest u het proces dat u wilt opsporen en klikt u op **bijlage toevoegen**

    ![Kies proces][chooseprocess]

    De naam van het proces dat u bijvoegen wilt, gelijk is aan de naam van uw service project constructie.

    Foutopsporing koppelt aan alle knooppunten het gestart.
    - In het geval is waar u een stateless service fouten wilt opsporen, zijn alle exemplaren van de service op alle knooppunten deel uit van de sessie voor foutopsporing.
    - Als u een statuscontrole service zijn foutopsporing, zijn alleen de primaire replica van elk partition actieve en kunnen daarom gevangen door foutopsporing. Als de primaire replica tijdens de sessie voor foutopsporing beweegt, wordt de verwerking van deze nog steeds deel uit van de sessie voor foutopsporing.
    - Om te kunnen alleen onderschept relevante partities of exemplaren van een bepaalde service, kunt u voorwaardelijke onderbrekingspunten wilt afbreken alleen een specifieke partition of exemplaar.

    ![Voorwaardelijke onderbrekingspunt][conditionalbreakpoint]

    > [AZURE.NOTE] We bieden momenteel geen ondersteuning voor foutopsporing in een Service stof cluster met meerdere exemplaren van dezelfde service uitvoerbare naam.

4. Wanneer u klaar voor foutopsporing in uw toepassing bent, kunt u de externe foutopsporing extensie uitschakelen door met de rechtermuisknop op het cluster in de **Cloud Explorer** en kies **Foutopsporing uitschakelen**

    ![Externe foutopsporing uitschakelen][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Traces van het knooppunt van een extern cluster streaming

Bent u ook kunnen op sporen stream rechtstreeks vanuit een extern clusterknooppunt naar Visual Studio. Deze functie kunt u op stream ETW doelcellen gebeurtenissen, geproduceerd op een Service stof clusterknooppunt, rechtstreeks in Visual Studio.

> [AZURE.NOTE] De functie vereist [Service stof SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) en [Azure SDK voor .NET 2,9](https://azure.microsoft.com/downloads/).

<!-- -->
> [AZURE.WARNING]Traces streaming is bedoeld voor ontwikkelaar/testen scenario's en niet mag worden gebruikt in productieomgevingen, vanwege de invloed op de actieve toepassingen.
> In een productieschema, moet u bij het doorsturen van gebeurtenissen met Azure diagnostische gegevens vertrouwen.

1. Ga naar uw cluster in de **Cloud Explorer**, met de rechtermuisknop op en kies **Streaming sporen inschakelen**

    ![Externe streaming sporen inschakelen][enablestreamingtraces]

    Hiermee wordt uitgangspunt voor het proces van het inschakelen van de streaming sporen extensie op uw knooppunten, evenals de vereiste netwerkconfiguraties.

2. Vouw het element **knooppunten** in de **Cloud Explorer**, met de rechtermuisknop op het knooppunt dat u wilt streamen traces van en kies **Weergave Streaming sporen**

    ![Weergave externe streaming sporen][viewremotestreamingtraces]

    Herhaal stap 2 voor zoveel knooppunten als u wilt zien van traces van. Elke gegevensstroom knooppunten wordt in een speciale venster weergegeven.

    U bent nu zien van de traces dat door de Service stof en uw services. Als u filteren wilt zodat alleen een specifieke toepassing gebeurtenissen, gewoon Typ de naam van de toepassing in het filter.

    ![Weergave streaming sporen][viewingstreamingtraces]

4. Als u klaar bent streaming sporen uit uw cluster, kunt u uitschakelen van externe streaming sporen, met de rechtermuisknop op het cluster in de **Cloud Explorer** en kies **Streaming sporen uitschakelen**

    ![Externe streaming sporen uitschakelen][disablestreamingtraces]

## <a name="next-steps"></a>Volgende stappen

- [Test een Service stof-service](service-fabric-testability-overview.md).
- [Uw Service stof-toepassingen in Visual Studio beheren](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
