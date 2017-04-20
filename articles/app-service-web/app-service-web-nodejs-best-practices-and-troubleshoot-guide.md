<properties
    pageTitle="Aanbevolen procedures en gids voor probleemoplossing voor knooppunt toepassingen op Azure Web Apps"
    description="Informatie over de aanbevolen procedures en stappen voor probleemoplossing voor knooppunt toepassingen op Azure Web Apps."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="ranjithr"
    manager="wadeh"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="06/06/2016"
    ms.author="ranjithr;wadeh"/>
    
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Aanbevolen procedures en gids voor probleemoplossing voor knooppunt toepassingen op Azure Web Apps

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

In dit artikel leert u de aanbevolen procedures en stappen voor probleemoplossing voor [knooppunt toepassingen](app-service-web-nodejs-get-started.md) op Azure-Webapps (met [iisnode](https://github.com/azure/iisnode)).

>[AZURE.WARNING] Wees voorzichtig bij gebruik van de stappen voor probleemoplossing in uw productielocatie. Aanbeveling is voor het oplossen van uw app op een niet-productieve installatie bijvoorbeeld het tijdelijk opslaan slot wanneer het probleem is opgelost, het tijdelijk opslaan slot met uw boven productie uitwisselen.

## <a name="iisnode-configuration"></a>IISNODE configuratie

Dit [schemabestand](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) ziet u de instellingen die kunnen worden geconfigureerd voor iisnode. Enkele instellingen die van pas voor uw toepassing zijn:

* nodeProcessCountPerApplication

    Deze instelling bepaalt het aantal knooppunt processen die worden uitgevoerd per IIS-toepassing. Standaardwaarde is 1. U kunt zo veel node.exe als uw VM core aantal starten door in te stellen dit 0. Aanbevolen waarde is aan 0 voor de meeste toepassing zodat u alle de boormonsters op uw computer gebruiken kunt. Node.exe is één discussielijnen in zodat één node.exe wordt een maximum van 1 core en optimale prestaties afmelden bij uw knooppunt-toepassing die te maken met het gebruik van alle cores wilt gebruiken.

* nodeProcessCommandLine

    Met deze instelling wordt het pad naar de node.exe. U kunt deze waarde te laten verwijzen naar uw node.exe-versie instellen.

* maxConcurrentRequestsPerProcess

    Met deze instelling wordt het maximum aantal gelijktijdige aanvragen is verzonden door iisnode aan elke node.exe. Klik op azure webapps is de standaardinstelling voor deze oneindig. U geen zorgen over deze instelling te maken. Buiten azure webapps is de standaardwaarde 1024. U kunt dit afhankelijk van hoeveel aanvragen dat uw toepassing krijgt en hoe snel uw toepassing verwerkt voor elk verzoek om te configureren.

* maxNamedPipeConnectionRetry

    Met deze instelling wordt het maximum aantal keren iisnode die verbinding op de pijplijn via te verzenden het verzoek naar node.exe wordt opnieuw. Deze instelling in combinatie met namedPipeConnectionRetryDelay bepaalt de totale time-out van elk verzoek om een binnen iisnode. Standaardwaarde is 200 op Azure-Webapps. Totale time-out in seconden = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

* namedPipeConnectionRetryDelay

    Deze instelling bepaalt u de hoeveelheid tijd (in ms) iisnode wordt gewacht tussen nieuwe pogingen verzoek verzenden naar node.exe via de pijplijn. Standaardwaarde is 250ms.
    Totale time-out in seconden = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

    Standaard is de totale time-out in iisnode op azure webapps 200 \* 250ms = 50 seconden.

* logDirectory

    Met deze instelling wordt de map waar iisnode stdout/stderr wordt vastgelegd. Standaardwaarde is iisnode dat wil zeggen ten opzichte van de belangrijkste script-map (directory waarin de belangrijkste server.js aanwezig is)

* debuggerExtensionDll

    Deze instelling bepaalt welke versie van iisnode knooppunt-controle wordt gebruikt als voor foutopsporing in uw toepassing knooppunt. Iisnode-controle-0.7.3.dll en iisnode-inspector.dll zijn momenteel alleen 2 geldige waarden voor deze instelling. Standaardwaarde is iisnode-controle-0.7.3.dll. iisnode-controle-0.7.3.dll versie beschikt over knooppunt-controle-0.7.3 en gebruikmaakt van websockets, zodat u moet inschakelen websockets op uw azure webapp deze versie te gebruiken. Zie <http://www.ranjithr.com/?p=98> voor meer informatie over het configureren van iisnode als u wilt gebruiken van de nieuwe knooppunt-controle.

* flushResponse

    Het standaardgedrag van IIS is dat deze antwoordgegevens buffers omhoog aan 4MB voordat of tot het einde van het antwoord, afhankelijk van wat zich het eerste voordoet. iisnode biedt een configuratie-instelling dit gedrag wijzigen: als u wilt een fragment van het hoofdgedeelte van het antwoord leegmaken zodra iisnode deze node.exe ontvangt u nodig hebt om in te stellen de iisnode/@flushResponse kenmerk in web.config in 'waar':
    
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```

    Inschakelen van leegmaken van elke fragment van het hoofdgedeelte van het antwoord beïnvloedt de systeemprestaties die Hiermee reduceert u de doorvoer van het systeem met ~ 5% (vanaf v0.1.13), is het verstandig om te bepalen van deze instelling alleen voor eindpunten waarvoor antwoord streaming (bijvoorbeeld met de <location> element in de web.config)

    Daarnaast moet voor streaming-toepassingen, u ook responseBufferLimit van uw iisnode-handler instellen op 0.
    
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```

* watchedFiles

    Dit is een komma's gescheiden lijst met bestanden die worden voor wijzigingen worden bekeken. Een wijziging naar een bestand wordt de toepassing naar Prullenbak. Elke vermelding bestaat uit een optioneel mapnaam plus de vereiste bestandsnaam die ten opzichte van de map waar het hoofdvenster van de toepassing ingangspunt zich bevindt. Jokertekens zijn toegestaan in het bestandsgedeelte alleen. Standaardwaarde is '\*. js;web.config "

* recycleSignalEnabled

    Standaardwaarde is ONWAAR. Als ingeschakeld, uw toepassing knooppunt kunt verbinden met een pijplijn (omgevingsvariabele IISNODE\_BESTURINGSELEMENT\_recht streepje) en een 'Prullenbak'-bericht verzenden. Hierdoor wordt de w3wp naar de Prullenbak zonder problemen.

* idlePageOutTimePeriod

    Standaardwaarde is aan 0, wat betekent dat deze functie is uitgeschakeld. Als de waarde naar een waarde groter dan 0, wordt iisnode pagina uit alle onderliggende processen elke milliseconden 'idlePageOutTimePeriod'. Raadpleeg deze [documentatie](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx)als u wilt weten welke pagina af gemiddelden. Deze instelling is handig voor toepassingen die veel geheugen in beslag nemen en pageout geheugen op schijf af en toe wilt vrijmaken sommige RAM.

>[AZURE.WARNING] Wees voorzichtig bij het inschakelen van de volgende configuratieinstellingen op productietoepassingen. Aanbeveling is deze niet worden ingeschakeld op live productietoepassingen.

* debugHeaderEnabled

    De standaardwaarde is ONWAAR. Als ingesteld op waar, iisnode een HTTP antwoord koptekst iisnode-foutopsporing wordt toegevoegd aan elke HTTP-antwoord verzendt deze de waarde van de koptekst iisnode-foutopsporing is een URL. Afzonderlijke onderdelen van diagnostische gegevens kunnen worden zakelijke door te kijken in de URL-fragment, maar een veel betere visualisatie wordt bereikt door de URL in de browser te openen.

* loggingEnabled

    Met deze instelling wordt de registratie van stdout en stderr door iisnode. Iisnode wordt stdout/stderr uit wordt het knooppunt-processen vastleggen en naar de map die is opgegeven in de instelling 'logDirectory' schrijven. Als u dit hebt inschakelen, uw toepassing wordt schrijft u logboeken naar het bestandssysteem en afhankelijk van de hoeveelheid logboekregistratie uitgevoerd door de toepassing, kan er prestaties consequenties.

* devErrorsEnabled

    Standaardwaarde is ONWAAR. Wanneer ingesteld op waar, iisnode de HTTP-statuscode en Win32-foutcode in uw browser weergegeven worden. De win32-code is handig in voor foutopsporing in bepaalde soorten problemen zijn.
    
* debuggingEnabled (niet inschakelt op live productiesite)

    Met deze instelling wordt de functie voor foutopsporing. Iisnode is geïntegreerd met knooppunt controle. Doordat deze instelling kunt u inschakelen voor foutopsporing in van de toepassing knooppunt. Wanneer deze instelling is ingeschakeld, wordt iisnode indeling de bestanden nodig knooppunt-controle in 'debuggerVirtualDir' map op de eerste aanvraag voor foutopsporing in uw toepassing knooppunt. U kunt de knooppunt-controle laden door een verzoek om te http://yoursite/server.js/debug sturen. U kunt het segment van de URL foutopsporing bepalen met de Omcirkelde instelling 'debuggerPathSegment'. Op standaard debuggerPathSegment = 'foutopsporing'. U kunt deze instellen naar een GUID bijvoorbeeld zodat moeilijker wordt gevonden door anderen.

    Controleer deze [koppeling](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) voor meer informatie over foutopsporing.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Scenario's en aanbevelingen/probleemoplossing

### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Mijn knooppunt-toepassing maakt te veel uitgaande oproepen.

Veel toepassingen zou willen uitgaande verbindingen maken als onderdeel van hun normale bewerking. Bijvoorbeeld wanneer u een aanvraag ontvangt, zou uw app knooppunt willen contact opnemen met een REST API elders is en u enkele gegevens verwerkingstijd van het verzoek. U wilt gebruikt een leven agent behouden bij http of https gesprekken. Bijvoorbeeld: u kunt de module agentkeepalive als uw leven agent behouden bij deze uitgaande gesprekken. Dit zorgt ervoor dat de sockets opnieuw kunnen worden gebruikt op uw azure webapp VM en de realiseren voor het maken van nieuwe sockets voor elke uitgaande aanvraag vermindert. Dit zorgt ook ervoor dat u minder aantal sockets gebruikt om te veel uitgaande aanvragen en daarom u de maxSockets die zijn toegewezen per VM niet overschrijdt. Aanbeveling op Azure-Webapps zou de waarde van de maxSockets agentKeepAlive instellen op een totaal van 160 sockets per VM. Dit betekent dat als er 4 node.exe waarop de VM, u zou willen de maxSockets agentKeepAlive ingesteld op 40 per node.exe namelijk 160 totaal per VM.

Voorbeeld agentKeepALive configuratie:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

In dit voorbeeld wordt ervan uitgegaan dat u hebt 4 node.exe waarop uw VM. Als u een verschillend aantal node.exe waarop de VM hebt, wordt er voor het wijzigen van de maxSockets overeenkomstig wordt ingesteld.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Mijn toepassing knooppunt verbruikt te veel CPU.

Waarschijnlijk krijgt u een aanbeveling van Azure-Webapps op uw portal over Hoog processorgebruik. U kunt ook instellen beeldschermen wil bekijken voor bepaalde [doelstellingen](web-sites-monitor.md). Bij het controleren van de CPU-gebruik op het [Dashboard van Azure-Portal](../application-insights/app-insights-web-monitor-performance.md), Controleer de waarden MAX voor CPU zodat u niet de waarden piek vergeten.
In als u denkt dat uw toepassing verbruikt te veel CPU en u kunt geen wordt uitgelegd waarom gevallen moet u naar het profiel van uw knooppunt-toepassing.

### 

#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>Uw toepassing knooppunt op azure webapps met V8-Profiler profielen

Bijvoorbeeld kunt Stel dat u hebt een Hallo wereld-app die u profiel wilt zoals hieronder wordt weergegeven:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Ga naar uw scm site https://yoursite.scm.azurewebsites.net/DebugConsole

U ziet een opdrachtprompt zoals hieronder wordt weergegeven. Ga in het telefoonboek van uw site/wwwroot

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

De opdracht uitvoeren 'npm geïnstalleerd v8-profiler'

Dit moet installeren v8-profiler onder knooppunt\_modules map en alle bijbehorende afhankelijkheden.
Nu uw server.js om het profiel van uw toepassing bewerken.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

De bovenstaande wijzigingen wordt de functie WriteConsoleLog profiel en schrijf vervolgens de uitvoer profiel naar 'profile.cpuprofile' bestand onder uw site wwwroot. Een aanvraag verzendt in uw toepassing. Hier ziet u een 'profile.cpuprofile'-bestand onder de wwwroot van uw site hebt gemaakt.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Download dit bestand en moet u dit bestand niet openen met Chrome F12 hulpmiddelen. Druk op F12 in chrome en klik op op het tabblad"profielen". Klik op de knop 'Laden'. Selecteer uw profile.cpuprofile-bestand dat u zojuist hebt gedownload. Klik op het profiel dat u zojuist hebt geladen.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

U ziet dat 95% van de tijd is verbruikt door WriteConsoleLog functie, zoals hieronder wordt weergegeven. Hier ziet u ook de exacte regelnummers en bronbestanden die ertoe leiden dat het probleem.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Mijn knooppunt-toepassing, is er te veel geheugen.

Waarschijnlijk krijgt u een aanbeveling van Azure-Webapps op uw portal over het gebruik van hoge geheugen. U kunt ook instellen beeldschermen wil bekijken voor bepaalde [doelstellingen](web-sites-monitor.md). Bij het controleren van de geheugengebruik op het [Dashboard van Azure-Portal](../application-insights/app-insights-web-monitor-performance.md), Controleer de waarden MAX voor geheugen zodat u niet de waarden piek vergeten.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Lekdetectie en opslagruimte vergelijken voor node.js 

U kunt [knooppunt-memwatch](https://github.com/lloyd/node-memwatch) om u te helpen u geheugenlekkages.
U kunt memwatch net als v8-profiler installeren en bewerken van uw code wilt vastleggen en vergelijken heaps om aan te geven het geheugen lekt in uw toepassing.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Van mijn node.exe zijn willekeurig ophalen beëindigd. 

Er zijn enkele redenen waarom dit kan gebeuren:

1.  Uw toepassing is genereren onbekende uitzonderingen: Please selectievakje d:\\thuis\\logboekbestanden\\toepassing\\logboekregistratie-errors.txt-bestand voor de informatie op de uitzondering opgetreden. Dit bestand heeft de stacktrace uw toepassing op basis van dit op te lossen.

2.  Uw toepassing is er te veel geheugen die dat dit gevolgen andere processen heeft is uit aan de slag. Als het totale VM geheugen lijkt op 100%, kan van uw node.exe worden beëindigd door de manager proces om te laten andere processen gaan werken. U lost dit probleem, zorg ervoor dat uw toepassing wordt niet geheugenlekkage of als u toepassing heel veel geheugen gebruiken moet, neemt schalen voor een grotere VM met veel meer RAM.

### <a name="my-node-application-does-not-start"></a>Mijn knooppunt-toepassing niet wordt gestart

Als uw toepassing als 500 fouten bij het opstarten resultaat, kan er enkele redenen:

1.  Node.exe is niet aanwezig op de juiste locatie. Controleer de instelling nodeProcessCommandLine.

2.  Belangrijkste scriptbestand is niet aanwezig op de juiste locatie. Schakel web.config en zorg ervoor dat de naam van de belangrijkste script-bestand in de sectie handlers overeenkomt met de belangrijkste scriptbestand.

3.  Web.config configuratie niet juist is: Controleer de instellingen voor namen/waarden.

4.  Koudwatersystemen Start – uw toepassing duurt te lang worden gestart. Als uw toepassing langer duren dan (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 seconden iisnode 500 fout zullen retourneren. Hiermee verhoogt u de waarden van deze instellingen zodat deze overeenkomt met uw toepassing begintijd om te voorkomen dat iisnode uit een time-out en de 500 fout retourneren.

### <a name="my-node-application-crashed"></a>Mijn knooppunt-toepassing is vastgelopen

Uw toepassing is genereren onbekende uitzonderingen: Please selectievakje d:\\thuis\\logboekbestanden\\toepassing\\logboekregistratie-errors.txt-bestand voor de informatie op de uitzondering opgetreden. Dit bestand heeft de stacktrace uw toepassing op basis van dit op te lossen.

### <a name="my-node-application-takes-too-much-time-to-startup-cold-start"></a>Mijn knooppunt-toepassing duurt te lang worden gestart (koudwatersystemen starten)

Meest voorkomende reden hiervoor is dat de toepassing een groot aantal bestanden in het knooppunt heeft\_modules en de toepassing probeert aan te melden meest van deze bestanden geladen bij het opstarten. Standaard omdat uw bestanden bevinden zich op de netwerkshare op Azure-Webapps, laden zo veel bestanden kan enige tijd duren.
Sommige oplossingen Maak deze sneller zijn:

1.  Zorg ervoor dat u hebt een platte afhankelijkheidsstructuur en geen dubbele afhankelijkheden met behulp van npm3 voor het installeren van uw modules.

2.  Probeer te fikse laden uw knooppunt\_modules en alle modules bij het opstarten niet worden geladen. Dit betekent dat de oproep door naar require('module') doen moet als er wel binnen de functie die u probeert behoefte te gebruiken van de module.

3.  Azure Webapps biedt een functie voor lokale cache. Deze functie wordt de inhoud in de netwerkshare kopiëren naar de lokale schijf op de VM. Aangezien de bestanden lokale, de laadtijd van knooppunt zijn\_modules is veel sneller. -Deze [documentatie](../app-service/app-service-local-cache.md) wordt uitgelegd hoe u lokale Cache uitgebreider.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE http status en substatus

Dit [bronbestand](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) vindt u alle de mogelijke status/substatus combinatie iisnode voor het geval de fout kan retourneren.

FREB voor uw toepassing om te zien van de win32-foutcode inschakelen (Zorg ervoor dat u FREB alleen op sites van niet-productieve Prestatieoverwegingen inschakelen).

| HTTP-Status | HTTP-SubStatus | Mogelijke oorzaak?                                                                                                                                                                                            
|-------------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------
| 500         | 1000           | Er was sommige probleem verzenden het verzoek IISNODE – Ga als node.exe is gestart. Node.exe kan hebben vastgelopen bij opstarten. Controleer uw configuratie web.config op fouten.                                                                                                                                                                                                                                                                                     |
| 500         | 1001           | -Win32Error 0x2 - App niet reageert op de URL. Kijk op URL herschrijven regels of als uw express app de juiste routes gedefinieerd heeft. -Win32Error 0x6d – named pipes is bezet: Node.exe accepteert geen aanvragen omdat de pijp bezet is. Schakel hoog cpu-gebruik. -Andere fouten – controleren als node.exe is vastgelopen.
| 500         | 1002           | Node.exe is vastgelopen – Ga d:\\thuis\\logboekbestanden\\logboekregistratie-errors.txt voor tracering van de stapel.                                                                                                                                                                                                                                                                                                                                                                                        |
| 500         | 1003           | Pipe configuratie probleem – moet u dit nooit zien, maar als u doet, de configuratie van de pijplijn is onjuist.                                                                                                                                                                                                                                                                                                                                                          |
| 500         | 1004-1018      | Er is een fout tijdens het verzenden van de aanvraag of verwerken van de reactie van node.exe. Controleer of node.exe is vastgelopen. d: controleren\\thuis\\logboekbestanden\\logboekregistratie-errors.txt voor tracering van de stapel.                                                                                                                                                                                                                                                                                    |
| 503         | 1000           | Onvoldoende geheugen meer named pipes verbindingen toewijzen. Selectievakje waarom uw app zo veel geheugen verbruikt. Controleer maxConcurrentRequestsPerProcess instellingswaarde. Als het is niet oneindig en u een groot aantal aanvragen, wordt deze waarde om te voorkomen dat deze fout vergroten.                                                                                                                                                                                                                                                                                                                  |
| 503         | 1001           | Aanvraag kan niet worden verzonden naar node.exe omdat de toepassing is hergebruik. Nadat de toepassing is herhaald, moeten aanvragen normaal worden aangeboden.                                                                                                                                                                                                                                                                                                               |
| 503         | 1002           | Selectievakje win32-foutcode voor werkelijke reden – verzoek kan niet worden verzonden naar een node.exe.                                                                                                                                                                                                                                                                                                                                                                               |
| 503         | 1003           | Pijplijn is bezet – Ga als knooppunt een groot aantal CPU verbruikt                                                                                                                                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
Er is een instelling binnen NODE.exe knooppunt genoemd\_in behandeling\_recht streepje\_exemplaren. Deze waarde is al dan niet standaard buiten azure webapps 4. Dit betekent dat node.exe kan alleen 4 aanvragen op een tijd in de pijplijn geaccepteerd. Klik op Azure-Webapps, deze waarde is ingesteld op 5000 en deze waarde moet goed voor de meeste knooppunt toepassingen op azure webapps uitgevoerd. U niet mogen zien 503.1003 op azure webapps omdat er een hoge waarde voor het knooppunt\_in behandeling\_recht streepje\_exemplaren.  |

## <a name="more-resources"></a>Meer informatiebronnen

Volg deze koppelingen voor meer informatie over node.js toepassingen op Azure App-Service.

* [Aan de slag met Node.js WebApps in Azure App-Service](app-service-web-nodejs-get-started.md)
* [Hoe u fouten opsporen in een Node.js web-app in Azure App-Service](web-sites-nodejs-debug.md)
* [Gebruik van Node.js Modules met Azure-toepassingen](../nodejs-use-node-modules-azure-apps.md)
* [Azure App-Service-WebApps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Developer Center](../nodejs-use-node-modules-azure-apps.md)
* [De geweldig geheime Kudu Foutopsporingsconsole verkennen](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)