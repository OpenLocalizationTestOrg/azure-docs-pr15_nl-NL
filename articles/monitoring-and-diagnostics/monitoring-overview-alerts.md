<properties
    pageTitle="Overzicht van waarschuwingen in Microsoft Azure | Microsoft Azure"
    description="Waarschuwingen kunt u bewaken Azure resource aan de doelstellingen, gebeurtenissen of Logboeken en een melding ontvangen wanneer de opgegeven voorwaarde is voldaan."
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
    ms.date="09/24/2016"
    ms.author="robb"/>

# <a name="overview-of-alerts-in-microsoft-azure"></a>Overzicht van waarschuwingen in Microsoft Azure


In dit artikel wordt beschreven welke waarschuwingen zijn de voordelen, en hoe u aan de slag met gebruik van deze.  

## <a name="what-are-alerts"></a>Wat zijn waarschuwingen?
Waarschuwingen zijn een methode controleren Azure resource maatstaven, gebeurtenissen, of Logboeken en klik vervolgens wordt een melding ontvangen wanneer een bepaalde voorwaarde is voldaan.

U kunt op basis van meldingen ontvangen:

- **Metrische waarden**: deze melding wordt gegenereerd wanneer de waarde van een opgegeven meting kruist een drempel die u in de gewenste richting toewijst. Dat wil zeggen er beide wordt gegenereerd wanneer eerst aan de voorwaarde is voldaan, en klik daarna wanneer die-voorwaarde is niet meer wordt voldaan.
- **Activiteitsgebeurtenissen**: deze melding kunt activeren op elke gebeurtenis of alleen wanneer een bepaald aantal gebeurtenissen plaatsvinden.

## <a name="alerts-in-different-azure-services"></a>Waarschuwingen in de verschillende services van Azure

Waarschuwingen zijn beschikbaar op andere services, waaronder:

- **Toepassing inzichten**: Hiermee test en metrisch waarschuwingen met webonderdelen. Zie [waarschuwingen weergeven in de toepassing inzichten](../application-insights/app-insights-alerts.md) en [beschikbaarheid van de Monitor en serverreactie van een website](../application-insights/app-insights-monitor-web-app-availability.md).
- **Log Analytics (bewerkingen Management Suite)**: de routering van diagnostische logboeken naar Log Analytics is ingeschakeld. Bewerkingen Management Suite kunt metrisch, het logboek en andere typen waarschuwingen. Zie [waarschuwingen in Log Analytics](../log-analytics/log-analytics-alerts.md)voor meer informatie.  
- **Azure Monitor**: mogelijk maakt op basis van zowel metrische waarden en activiteitsgebeurtenissen waarschuwingen. Azure Monitor bevat de [Azure Monitor REST API](https://msdn.microsoft.com/library/dn931943.aspx).  Zie [werken met de portal van Azure, PowerShell, of de opdrachtregel waarschuwingen maken](insights-alerts-portal.md)voor meer informatie.

## <a name="alert-actions"></a>Meldingen
U kunt configureren dat een melding in om het volgende te doen:

- Meldingen per e-mail verzenden naar de service-beheerder op CO-beheerders, of extra e-mailadressen die u opgeeft.
- Bel een webhook, waarmee u aanvullende automatisering acties te starten.

 ![Uitleg over waarschuwingen](./media/monitoring-overview-alerts/AlertsOverviewResource3.png)


## <a name="next-steps"></a>Volgende stappen

Informatie ophalen over waarschuwingsregels en configureer deze via:

- [Azure-portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [Opdrachtregel-interface (CLI)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)
