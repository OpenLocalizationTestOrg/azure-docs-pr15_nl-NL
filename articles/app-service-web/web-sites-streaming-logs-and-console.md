<properties 
    pageTitle="Streaming logboeken en console" 
    description="Logboeken en het overzicht van console-streaming" 
    authors="btardif" 
    manager="wpickett" 
    editor="" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="byvinyal"/>

# <a name="streaming-logs-and-the-console"></a>Logboeken en de Console-streaming

## <a name="streaming-logs"></a>Streaming Logboeken

De **Azure-portal** vindt u een geïntegreerde streaming logboeken waarmee u traceringsgebeurtenissen van uw **App Service** -apps in realtime weergeven.  

Instellen van deze functie is een paar eenvoudige stappen vereist:

- Traces schrijven in uw code
- **Diagnostische logboeken** aan de toepassing voor de app inschakelen
- De stroom van de ingebouwde **Logboeken Streaming** -gebruikersinterface in de **portal van Azure**weergeven.

### <a name="how-to-write-traces-in-your-code"></a>Traces schrijven in uw code ###

Traces schrijven in uw code gaat heel eenvoudig.  In C# is zo eenvoudig als het schrijven van de volgende code:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

De klas doelcellen bevindt zich in de naamruimte System.Diagnostics.

In een node.js-app kunt u deze code als u wilt bereiken hetzelfde resultaat:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>Gebeurtenislogboeken inschakelen en de streaming weergeven
![][BrowseSitesScreenshot]Diagnostische hulpprogramma's zijn ingeschakeld op basis van de per app. Begin door te bladeren naar de site die u wilt inschakelen voor deze functie.  
  
![][DiagnosticsLogs]Via het instellingenmenu, schuif omlaag naar de sectie **controleren** en klik op **Diagnostische logboeken (1)**. Vervolgens **Toepassing logboekregistratie (bestandssysteem)** **(2) inschakelen** of **Toepassing logboekregistratie (blob)** de optie **niveau** kunt u de ernst van sporen om vast te leggen wijzigen. Als u alleen probeert om vertrouwd te raken met de functie, stelt u het niveau op **uitgebreid** om ervoor te zorgen dat al uw trace-instructies zijn verzameld.

Klik op **Opslaan** aan de bovenkant van het blad en u klaar bent om te bekijken van Logboeken.

>[AZURE.NOTE] Hoe hoger worden het **prioriteitsniveau** extra resources worden verbruikt log en de meer traces geproduceerd. Zorg ervoor dat **prioriteitsniveau** is geconfigureerd voor de juiste gegevens opnemen in een gebruiks-en hoog verkeer site. 

![][StreamingLogsScreenshot]Als u wilt weergeven op de **Logboeken streaming** van binnen de Azure-portal, klik op **(1) Log Stream** ook in de sectie **cmdlets voor controle** van het instellingenmenu. Als uw app actief instructies trace schrijft is, klikt u vervolgens ziet u ze in de **gebruikersinterface (2) streaming logboeken** nabije realtime.

## <a name="console"></a>Console
De **portal van Azure** biedt consoletoegang tot uw app. U kunt verkennen van uw app-bestandssysteem en uitvoeren van scripts powershell/cmd. U gebonden aan dezelfde machtigingen instellen als uw actieve app-code bij het uitvoeren van consoleopdrachten. Toegang tot beveiligde mappen of uitvoeren van scripts die zijn verhoogde machtigingen vereist is geblokkeerd.  

![][ConsoleScreenshot]Via het instellingenmenu, schuif omlaag naar de sectie **Hulpmiddelen voor de ontwikkeling** , klik op **(1)-Console** en de **(2)-console** UI geopend aan de rechterkant.

Als u vertrouwd met de **console**, probeert u basisopdrachten zoals:

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
