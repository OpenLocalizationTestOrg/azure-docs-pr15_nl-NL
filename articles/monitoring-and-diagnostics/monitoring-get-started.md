<properties
    pageTitle="Aan de slag met Azure Monitor | Microsoft Azure"
    description="Aan de slag met Azure Monitor inzicht in de werking van uw resources en onderneem eventueel actie die zijn gebaseerd op gegevens."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-azure-monitor"></a>Aan de slag met Azure Monitor

Azure Monitor is het platform-service die één bron biedt voor het controleren van Azure resources. Met Azure Monitor, u kunt visualiseren, query, te routeren, archiveren en de aan de doelstellingen en logboeken die afkomstig zijn uit bronnen in Azure actie ondernemen. U kunt werken met deze gegevens met het beeldscherm portal blad, [Monitor PowerShell-Cmdlets](./insights-powershell-samples.md), [Platforms CLI](insights-cli-samples.md)of [Azure Monitor REST API's](https://msdn.microsoft.com/library/dn931943.aspx). In dit artikel doorlopen we een paar van de belangrijkste onderdelen van Azure Monitor, met behulp van de portal voor demonstratie.

1. Ga naar **meer services** en de optie **Monitor** vinden in de portal. Klik op het sterpictogram als u wilt deze optie toevoegen aan de lijst Favorieten, zodat deze altijd gemakkelijk toegankelijk zijn vanuit de linkernavigatiebalk.

    ![Controleren in de lijst met services](./media/monitoring-get-started/monitor-more-services.png)

2. Klik op de optie **Monitor** om het blad **Monitor** te openen. Deze blade bij elkaar brengt uw controle-instellingen en gegevens in één geconsolideerde weergave. Eerst wordt geopend naar de sectie **logboek** .

    ![Monitor blade navigatie](./media/monitoring-get-started/monitor-blade-nav.png)

    > [AZURE.WARNING] De **servicemeldingen** en **melding groepen** bovenstaande opties wordt alleen weergegeven voor de personen die hebben deelgenomen aan het privé voorbeeld van deze functies.

    Azure beeldscherm heeft drie eenvoudige categorieën-gegevens bewaken: het **logboek**, **aan de doelstellingen**en **Diagnostische logboeken**.

3. Klik op **activiteit Meld u aan** om ervoor te zorgen dat de activiteit log sectie wordt weergegeven.

    ![Activiteit Log blade](./media/monitoring-get-started/monitor-act-log-blade.png)

    Het [**gebeurtenissenlogboek**](./monitoring-overview-activity-logs.md) worden alle bewerkingen die zijn uitgevoerd voor bronnen in uw abonnement. Het logboek met, kunt u bepalen de ' wat, wie, en wanneer ' voor een maken, bijwerken of verwijderen van bewerkingen voor bronnen in uw abonnement. Bijvoorbeeld weergegeven het gebeurtenissenlogboek wanneer een web-app is gestopt en wie deze gestopt. Activiteitsgebeurtenissen zijn opgeslagen in het platform en beschikbaar zijn om te zoeken op 90 dagen duurt.
   
    U kunt maken en opslaan van query's voor algemene filters, waarna de belangrijkste query's aan een portal dashboard vastmaken zodat u altijd weet als gebeurtenissen die aan uw criteria voldoen zijn opgetreden.

4. De weergave aan een resourcegroep filteren in de laatste week en klik op de knop **Opslaan** .

    ![Activiteit log query opslaan](./media/monitoring-get-started/monitor-act-log-save.png)

5. Klik nu op de knop **vastmaken** .

    ![Klik op de pincode voor logboek](./media/monitoring-get-started/monitor-act-log-pin.png)

    De meeste van de weergaven in dit scenario kunnen worden gekoppeld aan een dashboard. Hiermee maakt u één bron van informatie voor operationele gegevens op uw services. 

6. Ga terug naar uw dashboard. Nu kunt u zien dat de query (en het aantal resultaten) wordt weergegeven in uw dashboard. Dit is handig als u wilt snel zien hoogwaardig bewerkingen die zijn opgetreden onlangs in uw abonnement, bijvoorbeeld. een nieuwe rol is toegewezen of een VM is verwijderd.

    ![Logboek die zijn vastgemaakt aan dashboard](./media/monitoring-get-started/monitor-act-log-db.png)

7. Ga terug naar de **Monitor** -tegel en klikt u op de sectie **aan de doelstellingen** . U moet eerst een resource selecteren door te filteren en het gebruik van de vervolgkeuzelijst opties boven aan het blad te selecteren.

    ![Filter resources aan de doelstellingen](./media/monitoring-get-started/monitor-met-filter.png)

    Alle Azure resources verzenden [**aan de doelstellingen**](./monitoring-overview-metrics.md). Deze weergave worden samengebracht alle parameters in een één venster zodat u eenvoudig kan begrijpen hoe uw resources uitvoert.

8. Als u een resource hebt geselecteerd, worden alle beschikbare maatstelsel weergegeven aan de linkerkant van het blad. U kunt meerdere aan de doelstellingen in één keer een grafiek door te selecteren van de doelstellingen en het type en tijdbereik op grafiek wijzigen. U kunt ook alle metrische waarschuwingen instellen voor deze resource weergeven.

    ![Metrische blade](./media/monitoring-get-started/monitor-metric-blade.png)

    > [AZURE.NOTE] Sommige van de doelstellingen zijn alleen beschikbaar doordat [Toepassing inzichten](../application-insights/app-insights-overview.md) en/of Windows of Linux Azure diagnostische gegevens van uw resources.

9. Wanneer u tevreden met uw grafiek bent, kunt u de knop **vastmaken** aan het vastmaken aan uw dashboard.

10. Ga terug naar het **Beeldscherm** blad en klikt u op **Diagnostische logboeken**.

    ![Diagnostische logboeken blade](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    [**Diagnostische logboeken**](monitoring-overview-of-diagnostic-logs.md) zijn logboeken gegenereerd *door* een resource die gegevens over de werking van die resource. Netwerk beveiliging groep regel items en logica App werkstroom logboeken zijn bijvoorbeeld beide soorten diagnostische logboeken. Deze logboeken kunnen worden opgeslagen in een opslag-account, streamen op een gebeurtenis-Hub en/of verzonden naar [Log Analytics](../log-analytics/log-analytics-overview.md). Log Analytics is Microsofts operationele intelligence product voor Geavanceerd zoeken en waarschuwingen.
   
    U kunt in de portal weergeven en een lijst met alle resources in uw abonnement om te bepalen wanneer ze diagnostische logboeken ingeschakeld beschikt over filteren.

11. Klik op een resource in het blad diagnostische logboeken. Als de diagnostische logboeken zijn wordt opgeslagen in een opslag-account, ziet u een lijst met per uur logboeken die u rechtstreeks kunt downloaden.

    ![Diagnostische logboeken voor een resource](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    U kunt ook klikken op **Diagnostische instellingen**waarmee u instellen of wijzigen van uw instellingen voor archiveren met een account opslag, streaming met Hubs gebeurtenis of het verzenden naar een werkruimte Log Analytics.

    ![Diagnostische logboeken inschakelen](./media/monitoring-get-started/monitor-diaglogs-enable.png)

    Als u diagnostische logboeken naar Log Analytics hebt ingesteld, kunt u deze vervolgens in de sectie **Log zoeken** van de portal zoeken.

12. Navigeer naar de sectie **waarschuwingen** van het blad Monitor.

    ![waarschuwingen blade voor openbare](./media/monitoring-get-started/monitor-alerts-nopp.png)

    Hier kunt u alle [**meldingen**](./monitoring-overview-alerts.md) op uw Azure bronnen beheren. Dit geldt ook voor waarschuwingen op maatstaven, activiteitsgebeurtenissen (in privé preview), toepassing inzichten web tests (locaties) en toepassing inzichten proactief diagnostische gegevens. Waarschuwingen kunnen resulteren in een e-mailbericht te kunnen verzenden of een HTTP-bericht naar een webhook-URL.
   
13. Klik op **metrische waarschuwing toevoegen** als u wilt een waarschuwing maken.

    ![een waarschuwing voor metrische toevoegen](./media/monitoring-get-started/monitor-alerts-add.png)

    U kunt een waarschuwing vervolgens vastmaken aan uw dashboard zodat u eenvoudig de status op elk gewenst moment zien.

14. De sectie Monitor bevat ook koppelingen naar de [Toepassing inzichten](../application-insights/app-insights-overview.md) toepassingen en [Log Analytics](../log-analytics/log-analytics-overview.md) managementoplossingen. Deze andere Microsoft-producten hebt naadloze integratie met Azure Monitor.

15. Als u niet-toepassing inzichten of Log Analytics gebruikt, is de kans groot dat Azure Monitor verbonden met uw huidige controleren, logboekregistratie en waarschuwingsmethoden producten is. Zie onze [partnerpagina](./monitoring-partners.md) voor een volledige lijst en instructies voor het integreren.

Door deze stappen te volgen en losmaken van alle relevante tegels aan een dashboard, kunt u volledig weergaven van uw toepassingen en de infrastructuur wilt maken:

![Azure Monitor dashboard](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Volgende stappen
- Lees het [overzicht van Azure Monitor](./monitoring-overview.md)
