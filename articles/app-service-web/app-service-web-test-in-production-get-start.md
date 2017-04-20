<properties
    pageTitle="Aan de slag met testen in voor de Web Apps"
    description="Meer informatie over de Test in productie (TiP)-functie van de Azure App Service Web Apps."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-test-in-production-for-web-apps"></a>Aan de slag met testen in voor de Web Apps

Testen in productie of live-testen van uw web-app met live klant-verkeer is toegestaan, wordt een test strategie die app ontwikkelaars steeds in hun werkwijze [agile ontwikkeling integreren](https://en.wikipedia.org/wiki/Agile_software_development) . U kunt de kwaliteit van uw apps gebruiken met live gebruiker-verkeer is toegestaan in uw productieomgeving, in tegenstelling tot kunstmatige gegevens in een testomgeving testen. Door de nieuwe app naar reële gebruikers weergeeft, kunt u geïnformeerd op de reële problemen die uw app computer zien kan wanneer deze wordt geïmplementeerd. U kunt de functionaliteit, prestaties en waarde van de updates van uw app tegen het volume, de migratiesnelheid en diverse reële gebruikersverkeer, waardoor u nooit in een testomgeving benaderen kunt verifiëren.

## <a name="traffic-routing-in-app-service-web-apps"></a>Verkeer-mailroutering in App Service WebApps

U kunt met de functie Routering van verkeer in [Azure-Service voor App](http://go.microsoft.com/fwlink/?LinkId=529714)een gedeelte van live gebruikersverkeer naar een of meer [implementatie sleuven](web-sites-staged-publishing.md)rechtstreekse en vervolgens uw app met [Azure toepassing inzichten](/services/application-insights/) of [Azure HDInsight](/services/hdinsight/)of een derde partij hulpmiddel zoals [Nieuwe Relic](/marketplace/partners/newrelic/newrelic/) voor het valideren van uw wijziging te analyseren. U kunt bijvoorbeeld de volgende scenario's implementeren met App-Service:

- Kennismaken met functionele bugs bij te werken of pinpoint prestaties problemen in uw updates voor de hele site-implementatie
- "Gecontroleerde test vluchten" van de wijzigingen door te meten usibility aan de doelstellingen op de bèta-app uitvoeren
- Geleidelijk aangeeft hoeveel naar een nieuwe update en zonder problemen weer omlaag naar de huidige versie als er een fout optreedt 
- Bedrijfsresultaten van uw app optimaliseren door te voeren [A / B getest](https://en.wikipedia.org/wiki/A/B_testing) of [multidimensionale tests](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) in meerdere implementatie sleuven

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>Vereisten voor het gebruik van verkeer mailroutering in Web-Apps

- Uw web-app moet uitvoeren in **standaard** of **Premium** laag, zoals dit vereist voor meerdere implementatie sleuven is.
- De goede werking verkeersroutering is vereist cookies zijn ingeschakeld in de browser van gebruikers. Cookies verkeer omleiden gebruikt om een opslaglocatie van de browser van een client naar een slot implementatie voor de duur de clientsessie te.
- Verkeer omleiden ondersteunt geavanceerde TiP-scenario's via Azure PowerShell-cmdlets.

## <a name="route-traffic-segment-to-a-deployment-slot"></a>Route-verkeer is toegestaan segment naar een slot implementatie

Op het niveau in een scenario voor elke TiP van eenvoudige, kunt u een vooraf gedefinieerde percentage van uw live verkeer doorsturen naar een niet-productieve implementatie slot. U doet dit door de volgende stappen uit te voeren:

>[AZURE.NOTE] De volgende stappen proberen wordt ervan uitgegaan dat u al een [niet-productieve implementatie slot](web-sites-staged-publishing.md) en dat de gewenste web app-inhoud al is [geïmplementeerd](web-sites-deploy.md) in deze.

1. Meld u aan bij de [Portal van Azure](https://portal.azure.com/).
2. Klik op **Instellingen**in uw web-app blade, > **Verkeer-mailroutering**.
  ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Selecteer de slot die u wilt doorsturen verkeer naar en het percentage van de totale verkeer u nodig hebben en klik op **Opslaan**.

    ![](./media/app-service-web-test-in-production/02-select-slot.png)

4. Ga naar de implementatie-slot blade. U ziet nu live verkeer naar deze locatie worden doorgestuurd.

    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

Zodra het verkeersroutering is geconfigureerd, wordt het opgegeven percentage van klanten willekeurig worden gestuurd naar uw niet-productieve slot. Het is echter Houd er rekening mee dat wanneer een client wordt automatisch doorgestuurd naar een specifieke slot, deze is '"aan die slot voor de duur van die clientsessie vastgemaakt wordt. Dit klaar een cookie om een opslaglocatie van de gebruikerssessie te gebruiken. Als u de HTTP-aanvragen controleren, vindt u een `TipMix` cookie in elke volgende aanvraag.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a>Dwingen clientaanvragen voor een specifieke slot

Naast het circulatielijst voor automatische verkeer kan App Service routeren aanvragen voor een specifieke slot. Dit is handig als u wilt dat uw gebruikers moeten kunnen opt-in- of opt-out van uw beta-app. Dit doet u gebruikt de `x-ms-routing-name` queryparameter.

Gebruikers kunnen een specifieke slot met worden omgeleid `x-ms-routing-name`, moet u ervoor zorgen dat de slot al is toegevoegd aan de lijst verkeer-mailroutering. Aangezien u doorsturen naar een slot expliciet wilt, kan het werkelijke routeren percentage dat u instellen maakt niet uit. Als u wilt, kunt u een 'beta koppeling' waarop gebruikers klikken kunnen om toegang tot de bèta-app Maak.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>Opt gebruikers af bij bèta-app

Als u wilt dat gebruikers afmelden bij uw beta-app, kunt u bijvoorbeeld deze koppeling in uw webpagina opnemen:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>

De tekenreeks `x-ms-routing-name=self` Hiermee geeft u de productie slot. Zodra de browser van de client toegang tot de koppeling, niet alleen deze omgeleid naar de slot productie, maar elke volgende aanvraag bevat de `x-ms-routing-name=self` cookie die de sessie naar de slot Productie kledingwinkelketen.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a>Opt-gebruikers in bij bèta-app

Als u wilt dat gebruikers deelnemen aan de bèta-app, stelt u de dezelfde queryparameter op de naam van de niet-productieve slot, bijvoorbeeld:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>Meer informatiebronnen ##

-   [Tijdelijke omgevingen voor web-apps in Azure-Service voor App instellen](web-sites-staged-publishing.md)
-   [Een complexe toepassing volgens plan in Azure implementeren](app-service-deploy-complex-application-predictably.md)
-   [Agile software development met Azure App-Service](app-service-agile-software-development.md)
-   [DevOps omgevingen effectief gebruiken voor uw web-apps](app-service-web-staged-publishing-realworld-scenarios.md)