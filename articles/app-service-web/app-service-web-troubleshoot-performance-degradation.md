<properties
    pageTitle="Vertragen web app in de App Service | Microsoft Azure"
    description="In dit artikel vindt u traag web app prestatieproblemen oplossen in Azure App-Service."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="Web app-prestaties, traag app app traag"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Traag web app prestatieproblemen oplossen in Azure App-Service

In dit artikel vindt u traag web app prestatieproblemen oplossen in [Azure App-Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Als u meer hulp op een willekeurige plaats in dit artikel nodig hebt, kunt u contact opnemen met de Azure deskundigen op [de MSDN-Azure en de stapel overloop-forums](https://azure.microsoft.com/support/forums/). U kunt ook kunt u ook een incident Azure ondersteuning indienen. Ga naar de [site van Azure-ondersteuning](https://azure.microsoft.com/support/options/) en klik op **Ondersteuning krijgen**.

## <a name="symptom"></a>Symptoom

Wanneer u de web-app, het laden van pagina's traag bladert en soms time-outinstellingen.

## <a name="cause"></a>Oorzaak

Dit probleem is vaak veroorzaakt door problemen met niveau van toepassingen, zoals:

-   het duurt erg lang voordat aanvragen
-   -toepassing via hoog geheugen/processor
-   de toepassing vast vanwege een uitzondering.

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

Probleemoplossing kan worden onderverdeeld in drie verschillende taken, in de juiste volgorde:

1.  [Toekijken en gedrag van de toepassing controleren](#observe)
2.  [Gegevens verzamelen](#collect)
3.  [Verklein het probleem](#mitigate)

[App Service Web Apps](/services/app-service/web/) kunt u verschillende opties bij elke stap.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. toekijken en gedrag van de toepassing controleren

#### <a name="track-service-health"></a>Servicestatus bijhouden

Microsoft Azure publicizes telkens wanneer er een service onderbroken of prestaties verslechtering van is. De status van de service kunt u vinden op de [Azure-Portal](https://portal.azure.com/). Zie [Servicestatus bijhouden](../monitoring-and-diagnostics/insights-service-health.md)voor meer informatie.

#### <a name="monitor-your-web-app"></a>Bewaak uw web-app

Deze optie kunt u om vast te stellen als uw toepassing eventuele problemen ondervindt. Klik in uw web-app blade, op de tegel **aanvragen en fouten** . Het blad **Metrisch** ziet u alle parameters die u kunt toevoegen.

Enkele van de criteria die u mogelijk wilt controleren voor uw web-app zijn

-   Gemiddelde geheugen werkset
-   Gemiddelde antwoord tijd
-   CPU-tijd
-   Geheugenwerkset
-   Aanvragen

![prestaties van de web-app controleren](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Zie voor meer informatie:

-   [Monitor met een Web-Apps in Azure App-Service](web-sites-monitor.md)
-   [Meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Web Eindpuntstatus controleren

Als u uw web-app in de **standaard** prijzen van laag, wordt er Web Apps kunt u controleren 2 eindpunten van 3 geografische locaties.

Eindpunt monitoring configureert web tests uit distributed van geografische locaties die tijd antwoord en beschikbaarheid van web-URL's testen. De test kunt u een HTTP GET-bewerking op de web-URL voor het bepalen van de tijd antwoord en beschikbaarheid vanaf elke locatie uitvoeren. Elke geconfigureerde locatie wordt uitgevoerd een toets om de vijf minuten.

Beschikbaarheid wordt gecontroleerd met HTTP-antwoord codes en reactietijd wordt gemeten in milliseconden. De controle test niet als de HTTP-antwoordcode groter dan of gelijk is aan 400 is of als het antwoord langer dan 30 seconden duurt. Een eindpunt wordt beschouwd als beschikbaar als de controle tests van de opgegeven locaties slagen.

Als u wilt instellen, raadpleegt u [hoe: web Eindpuntstatus controleren](web-sites-monitor.md#webendpointstatus).

Zie ook [Azure-websites behouden omhoog plus eindpunt Monitoring - met Stefan Schackow](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/) voor een video over het controleren van eindpunt.

#### <a name="application-performance-monitoring-using-extensions"></a>Toepassing prestatiecontroles Extensions gebruiken

U kunt ook de prestaties van uw toepassingen controleren door gebruikmaken van de _site uitbreidingen_.

Elke WebApp App Service biedt een extensible management-eindpunt dat kunt u gebruikmaken van een set krachtige hulpmiddelen die zijn geïmplementeerd als site uitbreidingen. Deze hulpmiddelen voor het bereik van de bron code editors zoals [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx) naar hulpmiddelen voor projectbeheer voor verbonden resources zoals een MySQL-database die is verbonden met een web-app.

[Azure toepassing inzichten](/services/application-insights/) en [Nieuwe Relic](/marketplace/partners/newrelic/newrelic/) zijn twee van de prestaties site uitbreidingen die beschikbaar zijn voor controle. Als u wilt gebruiken in nieuwe Relic, moet u een agent gedurende runtime installeren. Als u inzicht krijgen in Azure-toepassing, u uw code met een SDK opnieuw, en u kunt ook een extensie die toegang tot aanvullende gegevens biedt installeren. De SDK kunt u code schrijven voor het gebruik en de prestaties van uw app uitgebreider controleren.

Zie [prestaties controleren in webtoepassingen](../application-insights/app-insights-web-monitor-performance.md)toepassing inzichten gebruik.

Zie Gebruik nieuwe Relic, [Nieuwe Relic prestaties Toepassingsbeheer op Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />
### <a name="2-collect-data"></a>2. gegevens verzamelen

####    <a name="enable-diagnostics-logging-for-your-web-app"></a>Diagnostische gegevens vastleggen voor uw web-app

De Web Apps-omgeving biedt diagnostische functionaliteit voor logboekgegevens van zowel de webserver, en de webtoepassing. Deze logisch gescheiden in web server diagnostische hulpprogramma's en diagnostische hulpprogramma's van toepassing.

##### <a name="web-server-diagnostics"></a>Web server diagnostische gegevens

U kunt inschakelen of uitschakelen van de volgende soorten logboeken:

-   **Gedetailleerde fout logboekregistratie** - gedetailleerde foutgegevens voor HTTP-statuscodes die een fout (statuscode 400 of hoger) aangeven. Dit kan informatie bevatten die kunt u bepalen waarom de server de foutcode geretourneerd.
-   **Aanvraag aanwijzen is mislukt** - gedetailleerde informatie over mislukte aanvragen, inclusief een trace van de IIS-onderdelen gebruikt voor het verwerken van de aanvraag en de tijd die in elk onderdeel. Dit is handig als u probeert te verbeteren de prestaties van de web-app of komen wat is een specifieke HTTP-fout veroorzaakt.
-   **Logboekregistratie van web-Server** - informatie over HTTP-transacties met de W3C uitgebreide log-bestandsindeling. Dit is handig bij het bepalen van de algehele web app aan de doelstellingen, zoals het aantal aanvragen verwerkt of hoeveel aanvragen zijn van een specifiek IP-adres.

##### <a name="application-diagnostics"></a>Diagnostische hulpprogramma's van toepassing

Toepassing diagnostische gegevens kunt u de gegevens die zijn gemaakt met een webtoepassing opnemen. ASP.NET-toepassingen kunnen gebruiken de `System.Diagnostics.Trace` class informatie om aan te melden het logboek diagnostische gegevens.

Zie [diagnostische hulpprogramma's voor web-apps in Azure App Service logboekregistratie inschakelen](web-sites-enable-diagnostic-log.md)voor gedetailleerde instructies voor het configureren van uw toepassing voor logboekregistratie.

#### <a name="use-remote-profiling"></a>Externe profielen gebruiken

In Azure App-Service, kan Web Apps, API-Apps en WebJobs extern worden profiel. Als uw proces actief is langer duren dan verwacht, of de latentie van HTTP-aanvragen hoger zijn dan normale en de CPU-gebruik van het proces ook hoog is, kunt u op afstand uw proces bekijken en krijgen de stapels CPU steekproeven gesprek om de activiteit en de code warm paden te analyseren.

Zie voor meer informatie over [ondersteuning voor externe profielen in Azure App-Service](/blog/remote-profiling-support-in-azure-app-service).


#### <a name="use-the-azure-app-service-support-portal"></a>Gebruik van de Portal van Azure App-Service ondersteuning

Web Apps biedt de mogelijkheid om op te lossen met betrekking tot uw web-app door te kijken HTTP Logboeken, gebeurtenislogboeken, proces dumps en meer. U kunt alle deze gegevens met behulp van de portal van onze ondersteuning bij weer **http://&lt;de appnaam van uw >.scm.azurewebsites.net/Support**

De portal Azure App Service ondersteuning biedt drie afzonderlijke tabbladen ter ondersteuning van de drie stappen van een scenario voor algemene probleemoplossing:

1.  Huidige gedrag toekijken
2.  Analyseren met diagnostische informatie verzamelen en uit te voeren van de ingebouwde analyzers
3.  Beperken

Als het probleem nu voordoen is, klikt u op **analyseren** > **Diagnostisch hulpprogramma** > **Nu een diagnose stellen bij** een diagnostische sessie voor u te maken, die worden verzameld HTTP zich aanmeldt, Logboeken, geheugen dumps, PHP foutenlogboeken en PHP rapport verwerken.

Nadat de gegevens worden verzameld, worden er ook een analyse uitvoeren op de gegevens en u voorzien van een HTML-rapport.

Als u downloaden van de gegevens, al dan niet standaard wilt, zou u deze opgeslagen in de map D:\home\data\DaaS.

Zie voor meer informatie over de ondersteuning van Azure Apps Service-portal, [Nieuwe Updates zijn voor Site-Ondersteuningsextensie voor Azure-Websites](/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Gebruik de Kudu foutopsporing-Console

Web Apps wordt geleverd met een Foutopsporingsconsole die u gebruiken kunt voor foutopsporing, verkennen, het uploaden van bestanden, evenals de eindpunten JSON voor informatie ophalen over uw omgeving. Dit is de _Kudu Console_ of het _SCM Dashboard_ voor uw web-app genoemd.

U kunt dit dashboard openen door te gaan naar de koppeling **https://&lt;de appnaam van uw >.scm.azurewebsites.net/**.

Enkele zaken waarmee Kudu zijn:

-   omgevingsinstellingen voor uw toepassing
-   log stream
-   diagnostische dump
-   fouten opsporen in console waarin u Powershell-cmdlets en basisopdrachten DOS kunt uitvoeren.


Een andere handige functie van Kudu is dat geval de toepassing eerste kans uitzonderingen genereren is, kunt u Kudu en het hulpmiddel SysInternals Procdump geheugen maken wordt. De geheugendumps zijn momentopnamen van het proces en vaak kunt u ingewikkelder problemen oplossen met uw web-app.

Zie voor meer informatie over functies die beschikbaar zijn in Kudu, [Azure Websites Team Services hulpmiddelen die u moet weten over](/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />
### <a name="3-mitigate-the-issue"></a>3. Verklein het probleem

####    <a name="scale-the-web-app"></a>De schaal van de web-app aanpassen

Voor betere prestaties en doorvoer in Azure App-Service, kunt u de schaal waarop u met uw toepassing aanpassen. Schaalbaarheid van een WebApp heeft betrekking op twee gerelateerde acties: uw App serviceplan wijzigen naar een hoger niveau van de prijzen en configuratie van bepaalde instellingen nadat u de hogere prijzen laag hebt gekozen.

Zie voor meer informatie over de schaal, [schaal een WebApp in Azure App-Service](web-sites-scale.md).

Bovendien kunt u uw toepassingen op meer dan één exemplaar kunnen uitvoeren. Dit niet alleen vindt u meer verwerking videomogelijkheden, maar ook kunt u bepaalde hoeveelheid fouttolerantie. Als het proces op één exemplaar uitvalt, blijft het andere exemplaar nog steeds voldoen aan aanvragen.

De schaal als u handmatig of automatisch wilt, kunt u instellen.

####    <a name="use-autoheal"></a>AutoHeal gebruiken

AutoHeal wordt herhaald het werkproces voor uw app op basis van de instellingen die u (zoals configuratiewijzigingen, aanvragen, limieten op basis van geheugen of de tijd die nodig zijn kiest voor de uitvoering van een aanvraag). De meeste gevallen, is Prullenbak het proces de snelste manier herstellen uit een probleem. Hoewel de web-app rechtstreeks vanuit de Portal Azure altijd wordt gestart, wordt AutoHeal dit automatisch voor u betekenen. U hoeft te is sommige triggers in web.config voor de hoofdmap voor uw web-app toevoegen. Houd er rekening mee dat deze instellingen werken op dezelfde manier, wilt zelfs als de toepassing niet gelijk is aan een .net een.

Zie [automatisch retoucheren Azure-websites](/blog/auto-healing-windows-azure-web-sites/)voor meer informatie.

####    <a name="restart-the-web-app"></a>De WebApp opnieuw starten

Dit is vaak de eenvoudigste manier om te herstellen uit eenmalige problemen. Klik op de [Portal van Azure](https://portal.azure.com/)hebt op uw web-app blade, u de opties om te stoppen of start uw app opnieuw.

 ![Start opnieuw op in de browser om prestatieproblemen op te lossen](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

U kunt ook uw web-app via Azure Powershell beheren. Zie [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md)voor meer informatie.
