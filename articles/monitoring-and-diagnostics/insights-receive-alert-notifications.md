<properties
    pageTitle="Waarschuwingen voor Azure services ontvangen | Microsoft Azure"
    description="Een melding ontvangen wanneer waarschuwingsregels voorwaarden is voldaan."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="receive-alert-notifications"></a>Meldingen ontvangen

U kunt ontvangen een melding op basis van controleren van de doelstellingen voor of gebeurtenissen op uw Azure services.

Een waarschuwing regel op een waarde, wanneer de waarde van een opgegeven meting een drempelwaarde die is toegewezen kruist, wordt de huidige regel actief en voor een melding kunt verzenden. Voor een regel voor waarschuwingen over alle gebeurtenissen, een regel kan een melding op *elke* gebeurtenis of verzenden, alleen wanneer een bepaald aantal gebeurtenissen plaatsvinden.

Wanneer u een waarschuwing regel maakt, kunt u opties voor het verzenden per e-mail een melding naar de service-beheerder en CO-beheerders of naar een andere beheerder die u kunt opgeven. Wanneer de regel wordt geactiveerd, en wanneer een waarschuwing voorwaarde is opgelost, wordt de e-mail van een melding gestuurd.

U kunt de [REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx) of [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) configureren en informatie over waarschuwingsregels via een programma kunt ophalen.

## <a name="create-an-alert-rule"></a>Een waarschuwing regel maken

1. Klik op **Bladeren**en klik vervolgens op een resource waarin u bent geïnteresseerd cmdlets voor controle in de [portal](https://portal.azure.com/).

2. Klik op de tegel **waarschuwingsregels** in de lens voor **bewerkingen** .

3. Klik op de opdracht **melding toevoegen** .

    ![Een waarschuwing toevoegen](./media/insights-receive-alert-notifications/Insights_AddAlert.png)

4. U kunt de waarschuwing regel een naam en kies een beschrijving die wordt weergegeven in het e-mailbericht voor de melding.

5. Wanneer u **aan de doelstellingen** selecteert kiest u een voorwaarde en een drempel waarde voor de meetwaarde. Dit is de periode waarin Azure voor beeldscherm en uitgezet waarschuwing activiteit wordt gebruikt.

    ![Voorwaarde en een drempelwaarde](./media/insights-receive-alert-notifications/Insights_ConditionAndThreshold.png)

6. U kunt ook Kies **evenementen**en krijgt een melding wanneer een bepaalde gebeurtenis plaatsvindt.

    ![Gebeurtenissen](./media/insights-receive-alert-notifications/Insights_Events.png)

7. Tot slot kunt u kiezen voor het verzenden van e-mailmelding voor beheerders die verantwoordelijk is.

Nadat u op **Opslaan**, binnen enkele minuten zult u op de hoogte wanneer de meetwaarde die u kiest groter is dan de drempelwaarde voor.

## <a name="managing-your-alert-rules"></a>Uw waarschuwingsregels beheren

Als u een waarschuwing regel hebt gemaakt, kunt u een voorbeeld van de waarschuwing bekijken drempelwaarde voor de meetwaarde van de vorige dag vergeleken.

![Gebeurtenissen](./media/insights-receive-alert-notifications/Insights_EditAlert.png)


Uiteraard kunt u **deze waarschuwingsregels, en- of **uitschakelen** deze desgewenst kunt u tijdelijk niet langer meldingen over deze** bewerken.

## <a name="next-steps"></a>Volgende stappen

* [Webhooks configureren op uw waarschuwingen](insights-webhooks-alerts.md) naar meldingen doorsturen naar verschillende kanalen
* [De doelstellingen van de monitor-service](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service is alleen beschikbaar en heeft gereageerd.
* [Schakel controle en diagnostische hulpprogramma's](insights-how-to-use-diagnostics.md) voor het verzamelen van gedetailleerde hoge frequentie afmetingen van uw service.
* [Beschikbaarheid van de monitor en serverreactie van een webpagina](../application-insights/app-insights-monitor-web-app-availability.md) met de toepassing inzichten zodat u als de pagina vinden kan is niet beschikbaar.
* [Prestaties van de toepassing controleren](../application-insights/app-insights-azure-web-apps.md) als u wilt begrijpen precies hoe uw code wordt uitgevoerd in de cloud.
* [Gebeurtenissen weergeven en controlelogboeken](insights-debugging-with-events.md) voor meer informatie over alles die wijzigingen in uw service heeft plaatsgevonden.
* [Servicestatus bijhouden](insights-service-health.md) om vast te stellen wanneer Azure prestaties verslechtering van of service onderbrekingen is opgetreden.
