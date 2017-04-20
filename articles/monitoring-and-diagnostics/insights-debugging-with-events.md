<properties
    pageTitle="Gebeurtenissen weergeven en controlelogboeken"
    description="Leer hoe u ziet alle gebeurtenissen die in uw Azure-abonnement plaatsvinden."
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
    ms.date="04/28/2015"
    ms.author="robb"/>

# <a name="view-events-and-audit-logs"></a>Gebeurtenissen weergeven en controlelogboeken

Alle bewerkingen uitgevoerd op Azure resources worden volledig gecontroleerd door de Azure Resource Manager, van kunnen maken en verwijderingen aan toewijzen of intrekken van access. U kunt deze logboeken in de portal van Azure bladeren en u kunt ook de [REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx) of [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) gebruiken voor toegang via programmacode tot de volledige reeks gebeurtenissen.

## <a name="browse-the-events-impacting-your-azure-subscription"></a>Bladeren in de gebeurtenissen die invloed hebben op uw Azure-abonnement

1. Meld u aan bij de [Portal van Azure](https://portal.azure.com/).
2. Klik op **Bladeren** en selecteer **controlelogboeken bijhouden**.  
    ![Hub bladeren](./media/insights-debugging-with-events/Insights_Browse.png)
3. Hiermee opent u een blade met alle gebeurtenissen die u hebt een van uw abonnementen voor de afgelopen zeven dagen worden beïnvloed. Aan de bovenkant is een grafiek met gegevens met niveau en onder die de volledige lijst met Logboeken:  ![alle gebeurtenissen](./media/insights-debugging-with-events/Insights_AllEvents.png)

>[AZURE.NOTE] U kunt alleen de 500 meest recente gebeurtenissen voor een bepaald abonnement weergeven in de portal van Azure.

4. U kunt klikken op een vermelding om te zien van de gebeurtenissen die bestaat. Bijvoorbeeld wanneer u iets aan een resourcegroep implementeert, kunnen veel verschillende bronnen worden gemaakt of gewijzigd. U kunt voor elke vermelding zien:
    * Het **niveau** van de gebeurtenis - bijvoorbeeld deze kan worden alleen een proces voor het bijhouden van (**ter informatie**), of wanneer er iets is een fout opgetreden dat u moet weten over (**fout**).
    * De **Status** - de uiteindelijke status wordt in het algemeen zijn **geslaagd** of **mislukt**, maar mogelijk ook **geaccepteerde** voor langdurige bewerkingen.
    * *Wanneer* de gebeurtenis is opgetreden.
    * *Wie* de bewerking uitgevoerd als iedereen. Niet alle bewerkingen worden uitgevoerd door gebruikers, enkele worden uitgevoerd door de backend-services, zodat ze niet een **beller**zou hebben.
    * De **Correlatie-ID** van de gebeurtenis: dit is de unieke id voor deze reeks bewerkingen.

5. Hier kunt u Ga naar het blad details om de details van de gebeurtenis weer te geven.

    ![Resourcegroepen](./media/insights-debugging-with-events/Insights_EventDetails.png)

    Voor gebeurtenissen **mislukt** bevat deze pagina meestal een **Substatus** en een sectie **-Eigenschappen** die nuttige informatie voor foutopsporing bevatten.

## <a name="filter-to-specific-logs"></a>Filteren op specifieke Logboeken

Om te zien gebeurtenissen die op een specifieke entiteit of van een specifiek type toepassen, kunt u het controlelogboek logboeken blad filteren door te klikken op de opdracht **Filter** . U kunt ook het blad Filter gebruiken om te wijzigen van de **tijd tijdspanne** van het controlelogboek logboeken blad.

Nadat u op deze opdracht klikt, wordt een nieuwe blade geopend:

![Filter](./media/insights-debugging-with-events/Insights_EventFilter.png)

Er zijn vier typen filters:

1. Door abonnement
2. Door een **resourcegroep**
3. Door een **type Resource**
4. Door een bepaalde **Resource** - hiervoor moet u eerdere in de volledige *Resource-ID* van de resource waarin u geïnteresseerd bent

Bovendien kunt u ook gebeurtenissen filteren met wie de gebeurtenis uitgevoerd, of met het niveau van de gebeurtenis.

Wanneer u klaar bent met wat u wilt zien, klikt u op de knop **bijwerken** onderaan in het blad te kiezen.

## <a name="monitor-events-impacting-specific-resources"></a>Monitor gebeurtenissen die invloed hebben op specifieke resources

1. Klik op **Bladeren** om te vinden van de resource waarin u geïnteresseerd bent. U kunt alle de logboeken ook bekijken voor een volledige **resourcegroep**.
2. Klik op van de resource blade, schuif omlaag totdat u de tegel **gebeurtenissen** .  
    ![Gebeurtenissen-tegel](./media/insights-debugging-with-events/Insights_EventsTile.png)
3. Klik op deze tegel om te zien van gebeurtenissen die zijn gefilterd op alleen de resource die u hebt geselecteerd. De opdracht **filteren** kunt u het tijdsbereik wijzigen of meer informatie over filters toepassen.

## <a name="next-steps"></a>Volgende stappen

* [Meldingen ontvangen](insights-receive-alert-notifications.md) wanneer een gebeurtenis plaatsvindt.
* [De doelstellingen van de monitor-service](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service is alleen beschikbaar en heeft gereageerd.
* [Servicestatus bijhouden](insights-service-health.md) om vast te stellen wanneer Azure prestaties verslechtering van of service onderbrekingen is opgetreden.  
