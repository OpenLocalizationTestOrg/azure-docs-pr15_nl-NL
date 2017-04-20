<properties
    pageTitle="Apps controleren in Azure App-Service"
    description="Leer hoe u Apps in Azure App-Service controleren met behulp van de Azure-Portal."
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="byvinyal"/>

# <a name="how-to-monitor-apps-in-azure-app-service"></a>Hoe u: Apps in Azure App-Service controleren

[App-Service](http://go.microsoft.com/fwlink/?LinkId=529714) biedt de functionaliteit voor in-en-klare controleren in de [Portal van Azure](https://portal.azure.com).
Het gaat hierbij om de mogelijkheid om **quota** en **aan de doelstellingen** voor een app, evenals de App Service-abonnement instellen van **waarschuwingen** en zelfs **schaalbaarheid** automatisch op basis van deze gegevens te controleren.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Informatie over quota en aan de doelstellingen

### <a name="quotas"></a>Quota 's

Ingesloten in een App Service-toepassingen zijn onderhevig aan bepaalde *limieten* voor de resources die zij kunnen gebruiken. De limieten zijn gedefinieerd door het **App-abonnement** dat is gekoppeld aan de app.

Als de toepassing wordt gehost in een **gratis** of **gedeeld** plan, worden klikt u vervolgens de beperkingen ten aanzien van de beschikking over de app resources gedefinieerd door **quota's**.

Als de toepassing wordt gehost in een **eenvoudige**, **standaard** - of de **Premium** -abonnement en vervolgens de limieten op zijn de resources die zij kunnen gebruiken door de **grootte** (klein, Gemiddeld, groot) en het **exemplaar tellen** (1, 2, 3,...) van het **abonnement dat is App**geconfigureerd.

**Quota** voor **gratis** of **gedeelde** apps zijn:

* **CPU(Short)**
   * Bedrag van de processor toegestaan voor deze toepassing in een interval 3 minuten. Dit quotum wordt opnieuw ingesteld voor elke 3 minuten.
* **CPU(Day)**
   * Totale bedrag van de processor toegestaan voor deze toepassing in een dag. Dit quotum wordt opnieuw ingesteld voor elke 24 uur middernacht UTC.
* **Geheugen**
   * Totale hoeveelheid geheugen die is toegestaan voor deze toepassing.
* **Bandbreedte**
   * Totale hoeveelheid uitgaande bandbreedte van deze toepassing in een dag.
   Dit quotum wordt opnieuw ingesteld voor elke 24 uur middernacht UTC.
* **Bestandssysteem**
   * Totale hoeveelheid opslagruimte toegestaan.

Het quotum voor alleen van toepassing op apps die worden gehost op **eenvoudige**, **Standard** en **Premium** -abonnement is **bestandssysteem**.

Meer informatie over de quota voor specifieke, grenzen en functies die beschikbaar zijn voor de verschillende App-Service SKU's vindt u hier: [Azure-abonnementen Service](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Quotum afdwingen

Als een toepassing in het gebruik ervan groter is dan de **processor (korte naam)**, wordt **processor (dag)**of **bandbreedte** quotum, klikt u vervolgens de toepassing gestopt totdat het quotum opnieuw ingesteld. Tijdens deze periode treden alle verzoeken voor oproepen een **HTTP 403**.
![][http403]

Als het quotum voor het **geheugen** van toepassing is overschreden, klikt u vervolgens de toepassing niet opnieuw worden gestart.

Als het **bestandssysteem** -limiet is overschreden, klikt u vervolgens schrijven een mislukt, inclusief schrijven naar Logboeken.

Quota's kunnen worden verhoogd of verwijderd uit uw app door een upgrade van uw App serviceplan.

### <a name="metrics"></a>Aan de doelstellingen

**Aan de doelstellingen** bevatten informatie over de app of gedrag van de App Service-abonnement.

Voor een **toepassing**zijn de doelstellingen van de beschikbaar:

* **Gemiddelde antwoord tijd**
   * De gemiddelde tijd voor de app aanvragen kunnen maken.
* **Gemiddelde geheugen werkset**
   * De gemiddelde hoeveelheid geheugen in MIB's die worden gebruikt door de app.
* **CPU-tijd**
   * Het bedrag van de processor in seconden dat door de app. Voor meer informatie over deze metrische Zie: [CPU tijd tegenover CPU percentage](#cpu-time-vs-cpu-percentage)
* **Gegevens In**
   * Het bedrag van binnenkomende bandbreedte dat door de app in MIB's.
* **Gegevens uit**
   * De hoeveelheid uitgaande bandbreedte dat door de app in MIB's.
* **HTTP-2xx**
   * Het aantal resulteert in een HTTP-statuscode > = 200 maar < 300.
* **HTTP-3xx**
   * Het aantal resulteert in een HTTP-statuscode > = 300 maar < 400.
* **HTTP 401**
   * Het aantal met HTTP 401 statuscode resultaat.
* **HTTP 403**
   * Het aantal met de statuscode HTTP 403 resultaat.
* **HTTP 404**
   * Het aantal met HTTP 404 statuscode resultaat.
* **HTTP 406**
   * Het aantal met de statuscode HTTP 406 resultaat.
* **HTTP-4xx**
   * Het aantal resulteert in een HTTP-statuscode > = 400 maar < 500.
* **HTTP-Server fouten**
   * Het aantal resulteert in een HTTP-statuscode > = 500 maar < 600.
* **Geheugenwerkset**
   * Huidige hoeveelheid geheugen die wordt gebruikt door de app in MIB's.
* **Aanvragen**
   * Totaal aantal aanvragen ongeacht hun resulterende HTTP-statuscode.

Voor een **App-abonnement**zijn de doelstellingen van de beschikbaar:

>[AZURE.NOTE] App-Service plannen doelstellingen zijn alleen beschikbaar voor abonnementen in **eenvoudige**, **Standard** en **Premium** SKU.

* **CPU-Percentage**
   * De gemiddelde CPU-gebruik in alle exemplaren van het abonnement.
* **Geheugenpercentage**
   * Het gemiddelde geheugen gebruikt in alle exemplaren van het abonnement.
* **Gegevens In**
   * De gemiddelde binnenkomende bandbreedte gebruikt in alle exemplaren van het abonnement.
* **Gegevens uit**
   * Het gemiddelde uitgaande bandbreedte die wordt gebruikt in alle exemplaren van het abonnement.
* **Lengte**
   * Het gemiddelde aantal zowel lezen en schrijven aanvragen die zijn in de wachtrij opslag. Een hoge lengte is een aanduiding van een toepassing die kan worden vertragen vanwege overtollige schijf I/O.
* **HTTP-wachtrij lengte**
   * Het gemiddelde aantal HTTP-aanvragen dat moet gaan zitten in de wachtrij voordat het wordt voldaan. De lengte van een hoge of toeneemt HTTP-wachtrij is een symptoom van een abonnement onder dik laden.

### <a name="cpu-time-vs-cpu-percentage"></a>CPU tijd tegenover CPU percentage
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Zijn er 2 van de doelstellingen die overeenkomen met CPU-gebruik. **CPU-tijd** en het **percentage van de processor**

**CPU-tijd** is handig voor apps die worden gehost op abonnementen die **gratis** of **gedeeld** aangezien een van de quota in CPU minuten die worden gebruikt door de app wordt gedefinieerd.

**CPU-percentage** is echter nuttig zijn voor apps die zijn ingesloten in een **eenvoudige**, **standard** en **premium** -abonnementen, aangezien ze kunnen worden vergroot af en deze metrisch een goede aanduiding voor de algehele gebruik in alle exemplaren is.

##<a name="metrics-granularity-and-retention-policy"></a>Aan de doelstellingen granulatie en bewaarbeleid

Aan de doelstellingen voor een toepassing en de app-abonnement zijn aangemeld en door de service met de volgende granulaties en bewaarbeleid samengevoegd:

 * **Minuut** granulatie doelstellingen blijven voor **48 uur** behouden
 * **Uur** granulatie doelstellingen worden **30** dagen bewaard
 * **Dag** granulatie doelstellingen worden **90** dagen bewaard

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Cmdlets voor controle quota en in de Portal van Azure aan de doelstellingen.

U kunt de status van de verschillende **quota** en **aan de doelstellingen** van dat dit gevolgen heeft een toepassing in de [Portal van Azure](https://portal.azure.com)bekijken.

![][quotas]
**Quota's** vindt u onder Instellingen >**quota's**. De UX kunt u bekijken: (1) de naam van de quota, (2) de interval opnieuw instellen, (3) de huidige limiet en (4) de huidige waarde.

![][metrics]
**Aan de doelstellingen** zijn access rechtstreeks vanuit het blad voor de resource. U kunt ook de grafiek aanpassen: (1) **klikt u op** op deze, en selecteer (2)- **grafiek bewerken**.
Hier kunt u het (3) **tijdsbereik**, (4) **grafiektype**en (5) van **de doelstellingen** weergeven.  

U kunt meer informatie over hier aan de doelstellingen: [de doelstellingen van de Monitor-service](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Waarschuwingen en automatisch schalen
Aan de doelstellingen meldingen voor een App of Service van de App-abonnement kan maximaal worden aangesloten, voor meer informatie over dit, raadpleegt u [meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

App Service-apps die zijn ingesloten in een eenvoudige, standaard of premium App Service abonnementen ondersteuning **automatisch schalen**. Hiermee kunt u voor het configureren van regels die de doelstellingen van de App Service-abonnement controleren en kunnen vergroten of verkleinen van het aantal exemplaar's bieden aanvullende informatie naar wens of opslaan geld wanneer de toepassing te veel inrichten. U kunt meer informatie over automatisch schalen hier: [hoe u de schaal](../monitoring-and-diagnostics/insights-how-to-scale.md) en hier [Aanbevolen procedures voor het beeldscherm Azure autoscaling](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er gewijzigd
* Zie voor een handleiding voor het wijzigen van Websites naar App Service: [Azure App-Service en de invloed op bestaande Azure-Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
