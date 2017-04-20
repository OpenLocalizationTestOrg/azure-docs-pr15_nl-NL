<properties 
    pageTitle="WebApps in Azure App-Service configureren" 
    description="Hoe u een web-app in Azure App Services configureren" 
    services="app-service\web" 
    documentationCenter="" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="configure-web-apps-in-azure-app-service"></a>WebApps in Azure App-Service configureren #

Dit onderwerp wordt uitgelegd hoe u een web-app met behulp van de [Portal van Azure]configureren.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="application-settings"></a>Instellingen voor toepassingen

1. In de [Portal van Azure], opent u het blad voor de web-app.
2. Klik op **alle instellingen**.
3. Klik op **Toepassingsinstellingen**.

![Instellingen voor toepassingen][configure01]

Het blad **Toepassingsinstellingen** heeft instellingen gegroepeerd in verschillende categorieën onderverdeeld.

### <a name="general-settings"></a>Algemene instellingen

**Framework versies**. Deze opties instellen als uw app alle deze kaders gebruikt: 

- **.NET Framework**: Stel de .NET framework-versie. 
- **PHP**: PHP versie of **uit** voor het uitschakelen van PHP instellen. 
- **Java**: Selecteer de Java-versie of **uit** voor het uitschakelen van Java. Gebruik de optie **Web Container** kiezen tussen Tomcat en Jetty versies.
- **Python**: Selecteer het Python versie of de **uit** Python uitschakelen.

Voor technische redenen Java inschakelen voor uw app .NET, PHP en Python worden de opties uitgeschakeld.

<a name="platform"></a>
**Platform**. Hiermee selecteert u of uw web-app wordt uitgevoerd in een 32-bits of 64-bits-omgeving. De omgeving 64-bits vereist modus Basic of standaard. Vrij te geven en gedeeld modi altijd uitvoeren in een 32-bits-omgeving.

**Web Sockets**. Instellen **op** om het inschakelen van het protocol WebSocket; Stel dat uw WebApp wordt gebruikt dat [ASP.NET SignalR] of [socket.io].

<a name="alwayson"></a>
**Altijd op**. Standaard is WebApps worden verwijderd als deze actief voor een bepaalde periode zijn. Hiermee kunt het systeem te besparen. Modus Basic of standaard kunt u **Altijd op** wilt behouden de app geladen altijd inschakelen. Als uw app wordt uitgevoerd continue web taken, moet u **Altijd op**inschakelen of de web-taken niet betrouwbaar worden uitgevoerd.

**Beheerde verkooppijplijn versie**. Hiermee stelt u de IIS- [verkooppijplijn modus]. Laat deze waarde op geïntegreerde (de standaardinstelling), tenzij u een oudere app waarvoor een oudere versie van IIS hebt.

**Automatisch uitwisselen**. Als u automatisch omwisselen voor een slot implementatie inschakelt, wordt App-Service automatisch de web-app uitwisselen Exchange wanneer u een update naar die slot push. Zie voor meer informatie, [distribueren naar tijdelijke voor WebApps in Azure App Service elementen] (web-sites-gefaseerde-publishing.md).

### <a name="debugging"></a>Voor foutopsporing in

**Foutopsporing op afstand**. U kunt foutopsporing op afstand. Wanneer ingeschakeld, kunt u de externe foutopsporing in Visual Studio rechtstreeks verbinden met uw web-app. Externe foutopsporing blijven ingeschakeld voor 48 uur. 

### <a name="app-settings"></a>App-instellingen

In deze sectie bevat de naam/waardeparen die u web app worden geladen bij start. 

- .NET-apps voor deze instellingen zijn toegevoegd in uw .NET-configuratie `AppSettings` gedurende runtime, bestaande instellingen negeren. 

- PHP, Python, Java en knooppunt toepassingen hebben toegang tot deze instellingen als omgevingsvariabelen gedurende runtime. Voor elke instelling app worden twee omgevingsvariabelen gemaakt. een door de naam die is opgegeven door de vermelding van de instelling app, en een andere met het voorvoegsel APPSETTING_. Beide bevatten dezelfde waarde.

### <a name="connection-strings"></a>Verbindingstekenreeksen met de

Verbindingstekenreeksen met de voor gekoppelde resources. 

.NET-apps voor deze verbindingstekenreeksen zijn toegevoegd in uw .NET-configuratie `connectionStrings` instellingen gedurende runtime, bestaande posten waar de sleutel is gelijk aan de naam van de gekoppelde database overschrijven. 

Voor PHP, Python, Java en knooppunt-toepassingen zijn deze instellingen beschikbaar als omgevingsvariabelen gedurende runtime, voorafgegaan door het verbindingstype. De omgeving variabele voorvoegsels zijn als volgt: 

- SQL Server:`SQLCONNSTR_`
- MySQL:`MYSQLCONNSTR_`
- SQL-Database:`SQLAZURECONNSTR_`
- Aangepaste:`CUSTOMCONNSTR_`

Als een MySql-verbindingsreeks zijn met de naam bijvoorbeeld `connectionstring1`, deze zou zijn toegankelijk via de omgevingsvariabele `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Standaarddocumenten

Het standaarddocument is de pagina met webonderdelen die wordt weergegeven bij de basis-URL voor een website.  Het eerste overeenkomende bestand in de lijst wordt gebruikt. 

WebApps mogelijk modules dat route op basis van URL, maar niet voor statische inhoud, zodat er geen standaarddocument als zodanig is gebruikt.    

### <a name="handler-mappings"></a>Handlertoewijzingen

Gebruik dit gebied aangepast script processors verwerken van aanvragen voor specifieke bestandsextensies toevoegen. 

- **Extensie**. De bestandsextensie moeten worden verwerkt, zoals *.php of handler.fcgi. 
- **Script Processor pad**. Het absolute pad van de processor script. Aanvragen voor bestanden die overeenkomen met de bestandsextensie worden verwerkt door de script-processor. Gebruik het pad `D:\home\site\wwwroot` om te verwijzen naar de hoofdmap van uw app.
- **Aanvullende argumenten**. Optionele argumenten voor de opdrachtregel voor de script-processor 


### <a name="virtual-applications-and-directories"></a>Virtuele toepassingen en mappen 
 
Als u wilt configureren virtuele toepassingen en mappen, geef elke virtuele map en de bijbehorende fysieke pad ten opzichte van de hoofdmap van de website. U kunt desgewenst het selectievakje **toepassing** wilt instellen voor een virtuele map als een toepassing selecteren.


## <a name="enabling-diagnostic-logs"></a>Diagnostische logboeken inschakelen

Diagnostische logboeken inschakelen:

1. Klik in het blad voor de WebApp, op **alle instellingen**.
2. Klik op **Diagnostische logboeken**. 

Opties voor het schrijven van diagnostische logboeken uit een webtoepassing die ondersteuning biedt voor logboekregistratie: 

- **Logboekregistratie van toepassing**. Schrijft toepassingslogboeken naar het bestandssysteem. Logboekregistratie duurt voor een periode van 12 uur. 

**Niveau**. Wanneer toepassing logboekregistratie is ingeschakeld, wordt deze optie wordt de hoeveelheid informatie die is opgenomen (fout, waarschuwing, informatie of uitgebreid).

**Logboekregistratie van webserver**. Logboeken worden opgeslagen in de bestandsindeling van de W3C-uitgebreide log. 

**Gedetailleerde foutberichten**. Hiermee slaat foutberichten gedetailleerde .htm-bestanden. De bestanden worden opgeslagen onder /LogFiles/DetailedErrors. 

**Mislukt verzoek tracing**. Logboeken mislukte aanvragen tot XML-bestanden. De bestanden worden opgeslagen onder /LogFiles/W3SVC*lengte*, waar lengte een unieke id is. Deze map bevat een XSL-bestand en een of meer XML-bestanden. Zorg ervoor dat het bestand XSL downloaden omdat deze functionaliteit biedt voor opmaak en filteren van de inhoud van de XML-bestanden.

Als u wilt de logboekbestanden weergeven, moet u als volgt FTP-referenties maken:

1. Klik in het blad voor de WebApp, op **alle instellingen**.
2. Klik op **referenties voor implementatie**.
3. Voer een gebruikersnaam en wachtwoord.
4. Klik op **Opslaan**.

![Implementatie-referenties instellen][configure03]

De volledige naam van de FTP-gebruiker is 'app\username' waar *app* is de naam van uw web-app. De gebruikersnaam wordt weergegeven in het blad web app, onder **Essentials**.  

![Referenties voor FTP-implementatie][configure02]

## <a name="other-configuration-tasks"></a>Andere configuratietaken uit te voeren

### <a name="ssl"></a>SSL 

U kunt SSL-certificaten voor een aangepast domein uploaden modus Basic of standaard. Zie voor meer informatie [HTTPS inschakelen voor een web-app]. 

Als u wilt uw geüploade certificaten weergeven, klikt u op **Alle instellingen** > **aangepaste domeinen en SSL**.

### <a name="domain-names"></a>Domeinnamen

Aangepaste domeinnamen die voor uw web-app toevoegen. Zie voor meer informatie [configureren voor een web-app in Azure-Service voor App een aangepaste domeinnaam].

Als u wilt de domeinnamen van uw weergeven, klikt u op **Alle instellingen** > **aangepaste domeinen en SSL**.

### <a name="deployments"></a>Implementaties

- Het instellen van continue implementatie. Zie [Cijfer gebruiken voor de implementatie Web-Apps in Azure App-Service](./web-sites-deploy.md).
- Implementatie sleuven. Zie [implementeren naar tijdelijke omgevingen voor Web-Apps in Azure App-Service].


Als u wilt uw implementatie-sleuven weergeven, klikt u op **Alle instellingen** > **implementatie sleuven**.

### <a name="monitoring"></a>Cmdlets voor controle

U kunt de beschikbaarheid van HTTP of HTTPS eindpunten, van maximaal drie verdeeld geografische locaties testen modus Basic of standaard. De controle test niet als de HTTP-antwoord-code een fout (4xx of 5xx is) of het antwoord langer dan 30 seconden duurt. Een eindpunt wordt beschouwd als beschikbaar als de controle tests van de opgegeven locaties slagen. 

Zie voor meer informatie [hoe: web Eindpuntstatus controleren].

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert], waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="next-steps"></a>Volgende stappen

- [Een aangepaste domeinnaam in Azure App-Service configureren]
- [HTTPS inschakelen voor een app in Azure App-Service]
- [De schaal van een web-app in Azure App-Service]
- [Basisbeginselen voor Web-Apps in Azure App Service bewaken]

<!-- URL List -->

[ASP.NET-SignalR]: http://www.asp.net/signalr
[Azure-Portal]: https://portal.azure.com/
[Een aangepaste domeinnaam in Azure App-Service configureren]: ./web-sites-custom-domain-name.md
[Implementeren naar tijdelijke omgevingen voor Web-Apps in Azure App-Service]: ./web-sites-staged-publishing.md
[HTTPS inschakelen voor een app in Azure App-Service]: ./web-sites-configure-ssl-certificate.md
[Hoe u: web Eindpuntstatus controleren]: http://go.microsoft.com/fwLink/?LinkID=279906
[Basisbeginselen voor Web-Apps in Azure App Service bewaken]: ./web-sites-monitor.md
[pijplijnmodus]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[De schaal van een web-app in Azure App-Service]: ./web-sites-scale.md
[socket.IO]: ./web-sites-nodejs-chat-app-socketio.md
[Probeer de App-Service]: http://go.microsoft.com/fwlink/?LinkId=523751

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
