<properties
    pageTitle="Functionaliteit toevoegen aan uw eerste web-app"
    description="Leuke functies toevoegen aan uw eerste web-app in een paar minuten."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/12/2016"
    ms.author="cephalin"
/>

# <a name="add-functionality-to-your-first-web-app"></a>Functionaliteit toevoegen aan uw eerste web-app

In [Deploy Azure in vijf minuten uw eerste WebApp](app-service-web-get-started.md), kunt u een steekproef web-app geïmplementeerd in [Azure App-Service](../app-service/app-service-value-prop-what-is.md). In dit artikel kunt u snel enkele handige functies toevoegen aan uw geïmplementeerd web-app. In een paar minuten, zal u het volgende doen:

- verificatie voor uw gebruikers afdwingen
- uw app automatisch schaal
- meldingen ontvangen op de prestaties van uw app

Ongeacht welke steekproef app u geïmplementeerd in het vorige artikel, kunt u meelezen in deze zelfstudie.

De drie activiteiten in deze zelfstudie zijn alleen een paar voorbeelden van de groot aantal handige functies wanneer u uw web-app in de App Service hebt opgeslagen. Veel functies zijn beschikbaar in de laag **gratis** (die is wat uw eerste web-app wordt uitgevoerd op), en u kunt uw proefabonnement tegoeden functies waarvoor hoger prijzen lagen uit te proberen. Ervan op uw web-app blijven staan in **vrije** laag, tenzij u expliciet wordt dit omgezet in een andere prijzen laag.

>[AZURE.NOTE] De web-app die u hebt gemaakt met Azure CLI wordt uitgevoerd in **vrije** laag, zodat alleen één gedeelde VM exemplaar met quota voor de resource. Zie voor meer informatie over wat u met **gratis** laag, [App Service limieten](../azure-subscription-service-limits.md#app-service-limits).

## <a name="authenticate-your-users"></a>Verificatie van uw gebruikers

Nu, laten we eens kijken hoe makkelijk het is verificatie toevoegen aan uw app (verder lezen op [App Service verificatie/autorisatie](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)).

1. Klik in het portal blad voor de app die u net hebt geopend, klikt u op **Instellingen** > **verificatie / autorisatie**.  
    ![Verificatie - instellingen blade](./media/app-service-web-get-started/aad-login-settings.png)

2. Klik **op** verificatie inschakelen.  

4. Klik in **Verificatieproviders**op **Azure Active Directory**.  
    ![Verificatie - Schakel Azure AD](./media/app-service-web-get-started/aad-login-config.png)

5. Klik op **Express**in het blad **Azure Active Directory-instellingen** en klik op **OK**. De standaardinstellingen Maak een nieuwe Azure AD-toepassing in het telefoonboek van uw standaard.  
 ![Verificatie - configuratie express](./media/app-service-web-get-started/aad-login-express.png)

6. Klik op **Opslaan**.  
    ![Verificatie - configuratie opslaan](./media/app-service-web-get-started/aad-login-save.png)

    Zodra de wijziging geslaagd is, ziet u de melding Bel groen, samen met een beschrijvende bericht.

7. Terug in de portal blad van uw app, klikt u op de **URL** -koppeling (of **Blader** op de menubalk). De koppeling is een HTTP-adres.  
    ![Verificatie - Blader naar de URL](./media/app-service-web-get-started/aad-login-browse-click.png)  
    Maar nadat dit is de app in een nieuw tabblad geopend, de URL vak omleidingen enkele malen en eindigt op uw app met een HTTPS-adres. Wat u ziet is dat u al bent aangemeld bij uw Azure-abonnement en u automatisch in de app bent geverifieerd.  
    ![Verificatie - aangemeld](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    Dus als u nu een niet-geverifieerde sessie in een andere browser opent, u een aanmeldingsvenster ziet wanneer u naar dezelfde URL navigeren.  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
   Als u andere items met Azure Active Directory nooit hebt gedaan, zijn uw standaardmap mogelijk geen Azure AD-gebruikers. In dat geval is waarschijnlijk de enige account in hier het Microsoft-account aan uw Azure-abonnement. Daarom heb ik u automatisch bij de app in dezelfde browser eerder zijn aangemeld.
   U kunt die hetzelfde Microsoft-account op deze pagina login ook aanmelden.

Gefeliciteerd, u bij het verifiëren van al het verkeer naar uw web-app.

Zoals u hebt gezien, de **verificatie / autorisatie** blade die u nog veel meer,, zoals uitvoeren kunt:

- Sociale aanmelding inschakelen
- Meerdere login opties inschakelen
- Het standaardgedrag wijzigen wanneer mensen eerst naar uw app navigeren

App-Service biedt dat een oplossing inschakelen-toets voor een deel van de algemene verificatie nodig heeft, zodat u niet hoeft te leveren van de logica verificatie zelf.
Zie [App Service verificatie/autorisatie](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)voor meer informatie.

## <a name="scale-your-app-automatically-based-on-demand"></a>De schaal van de app automatisch op basis van de aanvraag aanpassen

Volgende, laten we automatisch schalen uw app zodat deze automatisch kunt u deze capaciteit om te reageren op gebruiker aanvraag aanpassen wordt (verder lezen op [schaal van de app in Azure wordt aangegeven](web-sites-scale.md) en [schaal exemplaar tellen handmatig of automatisch](../monitoring-and-diagnostics/insights-how-to-scale.md)).

Kortom, u de schaal van uw web-app op twee manieren:

- [Schalen](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): meer CPU, geheugen, schijfruimte en extra functies beschikbaar, zoals speciale VMs, aangepaste domeinen en certificaten, tijdelijke sleuven, autoscaling en meer. U vergroten door te wijzigen van de prijzen laag van het abonnement dat App uw app behoort.
- [Schaal af](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): het aantal VM met groter wordende exemplaren waarop uw app wordt uitgevoerd.
U kunt schalen af in maximaal 50 exemplaren, afhankelijk van uw prijzen laag.

Zonder verder ado, kunt u autoscaling we instellen.

1. Eerst, breiden we autoscaling inschakelen. Klik op **Instellingen**in de portal blad van uw app, > **Schaal omhoog (App Service abonnement)**.  
    ![Schalen - instellingen blade](./media/app-service-web-get-started/scale-up-settings.png)

2. Schuif en selecteert u de **Standaard S1** laag, de laagste laag die ondersteuning biedt voor autoscaling (omcirkeld in schermafbeelding) en klik vervolgens op **selecteren**.  
    ![Schalen: Kies Laag](./media/app-service-web-get-started/scale-up-select.png)

    U bent klaar schaal omhoog.

    >[AZURE.IMPORTANT] Deze laag expends uw gratis proefabonnement tegoeden. Als u een betaald per gebruik-account hebt, bijhoudt deze kosten met uw account.

3. Vervolgens we configureren autoscaling. Klik op **Instellingen**in de portal blad van uw app, > **Schaal af (App Service abonnement)**.  
    ![Schaal uit - instellingen blade](./media/app-service-web-get-started/scale-out-settings.png)

4. **CPU-Percentage** **schaal door te** wijzigen. De schuifregelaars onder de vervolgkeuzelijst bijwerken dienovereenkomstig gewijzigd. Kies vervolgens een bereik **exemplaren** tussen **1** en **2** en een **doelbereik** tussen **40** en **80**definiëren. Voer deze door te typen in de vakken of door de schuifregelaars te bewegen.  
 ![Schaal - autoscaling configureren](./media/app-service-web-get-started/scale-out-configure.png)

    Op basis van deze configuratie, uw app automatisch aangepast af wanneer CPU-gebruik groter dan 80 is % en schalen wanneer CPU-gebruik minder dan 40%.

5. Klik op **Opslaan** op de menubalk.

Gefeliciteerd, uw app autoscaling is.

Zoals u hebt gezien, in het blad **Schaalinstellingen** die u nog veel meer,, zoals uitvoeren kunt:

- Handmatig op een bepaald aantal exemplaren schaal
- Met andere prestatiegegevens, zoals het percentage of schijf wachtrij geheugen wilt verkleinen
- Schaal gedrag aanpassen wanneer een regel prestaties wordt geactiveerd
- Automatisch schalen volgens een schema
- Autoscaling gedrag voor een toekomstige gebeurtenis instellen

Zie voor meer informatie over de schaalbaarheid van uw app, de [schaal van de app in Azure wordt aangegeven](../app-service-web/web-sites-scale.md). Voor meer informatie over het schalen, Zie [schalen dat exemplaar tellen handmatig of automatisch](../monitoring-and-diagnostics/insights-how-to-scale.md).

## <a name="receive-alerts-for-your-app"></a>Meldingen voor uw app

Nu dat uw app wordt uitgevoerd autoscaling, wat gebeurt er wanneer de maximale exemplaar-telling (2) worden bereikt en processor zich boven de gewenste gebruik (80%)?
U kunt een waarschuwing (verder lezen op [waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)) instellen zodat u deze situatie zodat u kunt uw app omhoog/out verder bijvoorbeeld schalen. Laten we snel een waarschuwing instellen voor dit scenario.

1. Klik in de portal blad van uw app, op **Hulpmiddelen voor** > **waarschuwingen**.  
    ![Waarschuwingen - instellingen blade](./media/app-service-web-get-started/alert-settings.png)

2. Klik op **melding toevoegen**. Selecteer vervolgens in het vak **resources** de resource die op **(serverfarms eindigt)**. Dit is uw App serviceplan.  
    ![Waarschuwingen - toevoegen waarschuwing voor App-abonnement](./media/app-service-web-get-started/alert-add.png)

3. Geef **de naam** als `CPU Maxed`, **Metrisch** als **Percentage van de processor**en **drempel** als `90`, **e-eigenaren, inzenders, en lezers**te selecteren en klik vervolgens op **OK**.   
 ![Waarschuwingen - configureren melding](./media/app-service-web-get-started/alert-configure.png)

    Wanneer Azure gemaakt van de melding is, ziet u deze in het blad **waarschuwingen** .  
    ![Waarschuwingen - klaar weergave](./media/app-service-web-get-started/alert-done.png)

Gefeliciteerd, u krijgt nu waarschuwingen.

Deze instelling voor waarschuwingen controles CPU-gebruik elke vijf minuten. Als dit nummer 90% of hoger gaat, ontvangt u een e-mailwaarschuwing, samen met iemand die gemachtigd is. Als u wilt zien iedereen die gemachtigd is om de waarschuwingen te ontvangen, gaat u terug naar de portal blad van uw app en klik op de knop **Access** .  
![Zien wie er waarschuwingen](./media/app-service-web-get-started/alert-rbac.png)

U ziet dat **abonnement beheerders** al zijn de **eigenaar** van de app. Deze groep zou u als u de accountbeheerder van het van uw Azure abonnement (bijvoorbeeld uw proefabonnement) bevatten. Zie [Toegangsbeheer voor Azure Role-Based](../active-directory/role-based-access-control-configure.md)voor meer informatie over Azure Rolgebaseerd toegangsbeheer.

> [AZURE.NOTE] Waarschuwingsregels is een Azure functie. Zie [meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Klik op uw manier voor het configureren van de melding, misschien u opgevallen een uitgebreide set hulpmiddelen voor in het blad **Hulpmiddelen voor** . Hier kunt u oplossen problemen monitor prestaties testen op problemen, resources beheren, communiceren met de VM-console en nuttige extensies toevoegen. We nodigen u kunt op elk item deze tools te vinden van de eenvoudige, maar krachtige hulpmiddelen die uw vinger tips klikt.

Ontdek hoe u meer doen met uw geïmplementeerd-app. Hier volgt alleen een gedeeltelijke lijst:

- [Kopen en configureren van een aangepaste domeinnaam](custom-dns-web-site-buydomains-web-app.md) - kopen van een fraaie domein voor uw web-app in plaats van de *. azurewebsites.net domein. Of een domein dat u al hebt gebruikt.
- [Instellen tijdelijke omgevingen](web-sites-staged-publishing.md) - Implementeer de app naar de URL van een tijdelijk opslaan voordat u deze in bedrijf plaatst. Werk uw live web-app probleemloos. Een uitgebreide DevOps-oplossing met meerdere implementatie sleuven instellen.
- [Instellen van continue implementatie](app-service-continuous-deployment.md) - app-implementatie integreren met uw systeem voor bronbeheer gebruikt. Dashboard implementeren naar Azure met elke doorvoeren.
- [Toegang tot on-premises implementatie resources](web-sites-hybrid-connection-get-started.md) - Access een bestaande on-premises implementatie-database of CRM-systeem.
- Een [back-up van uw app](web-sites-backup.md) - instellen van back up en herstellen voor uw web-app. Voorbereiden voor onverwachte fouten en hiertegen herstellen.
- [Diagnostische logboeken inschakelen](web-sites-enable-diagnostic-log.md) - lezen de IIS-logboeken Azure of toepassing sporen. Lezen in een stream, kunt u deze downloaden of deze poort in [Toepassing inzichten](../application-insights/app-insights-overview.md) voor inschakelen-toets analyse.
- [Scan uw app voor problemen](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
scannen van uw web-app tegen moderne threats service van [Folie beveiliging](https://www.tinfoilsecurity.com/)gebruiken.
- [Achtergrondtaken uitvoeren](../azure-functions/functions-overview.md) - klaar-taken voor gegevensverwerking rapportage, enzovoort.
- [Informatie over de werking van de App-Service](../app-service/app-service-how-works-readme.md)
