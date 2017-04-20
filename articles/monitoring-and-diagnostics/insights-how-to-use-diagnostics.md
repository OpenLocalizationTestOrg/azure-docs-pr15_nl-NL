<properties
    pageTitle="Inschakelen cmdlets voor controle en diagnostische gegevens in Microsoft Azure | Microsoft Azure "
    description="Informatie over het instellen van diagnostische hulpprogramma's voor uw resources in Azure wordt aangegeven."
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

# <a name="enable-monitoring-and-diagnostics"></a>Cmdlets voor controle en diagnostische gegevens inschakelen

Klik in de [Portal van Azure](https://portal.azure.com)kunt u volwaardige, veelgebruikte, cmdlets voor controle en diagnostische gegevens over uw resources configureren. U kunt ook de [REST API](https://msdn.microsoft.com/library/azure/dn931932.aspx) of [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) gebruiken voor het configureren van diagnostische gegevens via programmacode.

Diagnostische hulpprogramma's, bewaken en metrisch gegevens in Azure wordt opgeslagen in een account opslag van uw keuze. Hiermee kunt u gebruik ongeacht tooling die u wilt de gegevens van een explorer opslag naar Power BI om tooling van derden worden gelezen.

## <a name="when-you-create-a-resource"></a>Wanneer u een resource maakt

De meeste services kunnen u diagnostische hulpprogramma's inschakelen wanneer u deze eerst in de [Portal van Azure](https://portal.azure.com)maakt.

1. Ga naar **Nieuw** en kiest u de resource waarin u geïnteresseerd bent.

2. Selecteer **optioneel configuratie**.
    ![Diagnostisch hulpprogramma blade](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)

3. Selecteer **diagnostische hulpprogramma's**en klikt u **op**. U moet kiezen de opslag-account dat u wilt dat diagnostische gegevens moeten worden opgeslagen. U moet betalen normale gegevens eenheidstarieven voor opslagruimte en transacties wanneer u diagnostische gegevens naar een opslag-account verzendt.

4. Klik op **OK** en maken van de resource.

## <a name="change-settings-for-an-existing-resource"></a>Instellingen wijzigen voor een bestaande bron

Als u een resource al hebt gemaakt en u wijzigen van de instellingen van de diagnostische gegevens wilt (om het niveau van de gegevensverzameling, bijvoorbeeld wijzigen), kunt u deze rechtstreeks in de Portal Azure kunt doen.

1. Ga naar de resource en klik op de opdracht **Instellingen** .

2. **Diagnostische hulpprogramma's**selecteren

3. Het **hulpprogramma's voor diagnose** blad bevat alle mogelijke diagnostisch hulpprogramma en controlegegevens siteverzameling voor de desbetreffende resource. Voor sommige resources kunt u ook **een bewaarbeleid voor gegevens, om op te schonen van uw account opslagruimte** kiezen.
    ![Opslag diagnostische gegevens](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)

4. Als u uw instellingen hebt gekozen, klikt u op de opdracht **Opslaan** . Het duurt iets terwijl voor om te worden weergegeven als u deze voor de eerste keer inschakelen wilt-gegevens bewaken.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Categorieën van gegevens verzamelen voor virtuele machines
Voor virtuele machines wordt alle doelstellingen en Logboeken, vastgelegd met één minuut tussenpozen, zodat u kunt altijd de meest recente informatie over uw computer hebt.

- **Eenvoudige aan de doelstellingen** : status statistieken over uw VM zoals processor en geheugen
- **Aan de doelstellingen netwerk en Internet** : aan de doelstellingen over uw netwerkverbindingen en webservices
- **Aan de doelstellingen .NET** : aan de doelstellingen over de .NET en ASP.NET-toepassingen op uw computer virtuele
- **Aan de doelstellingen SQL** : als u Microsoft SQL-Service, de prestatiegegevens uitvoert
- **Windows-gebeurtenislogboeken toepassing** : Windows-gebeurtenissen die zijn verzonden naar het kanaal toepassing
- **Windows-systeem van gebeurtenislogboeken** : Windows-gebeurtenissen die zijn verzonden naar het systeemkanaal. Dit bevat ook alle gebeurtenissen van [Microsoft Antimalware](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409).
- **Windows-gebeurtenislogboeken beveiliging** : Windows-gebeurtenissen die zijn verzonden naar het beveiligingskanaal
- **Diagnostisch hulpprogramma infrastructuur logboeken** : informatie over de infrastructuur van de siteverzameling diagnostische gegevens vastleggen
- **IIS-logboeken** : logboeken over uw IIS-server

Houd er rekening mee dat op dit moment bepaalde onderzoeken Linux worden niet ondersteund, en, de Gast-Agent moet zijn geïnstalleerd op de virtuele machine.

## <a name="next-steps"></a>Volgende stappen

* Cross een drempelwaarde voor [meldingen ontvangen](insights-receive-alert-notifications.md) wanneer operationele gebeurtenissen plaatsvinden of aan de doelstellingen.
* [De doelstellingen van de monitor-service](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service is alleen beschikbaar en heeft gereageerd.
* [Automatisch schalen dat exemplaar tellen](insights-how-to-scale.md) om ervoor te zorgen dat uw service schaal op basis van de aanvraag.
* [Prestaties van de toepassing controleren](../application-insights/app-insights-azure-web-apps.md) als u wilt begrijpen precies hoe uw code wordt uitgevoerd in de cloud.
* [Gebeurtenissen weergeven en controlelogboeken](insights-debugging-with-events.md) voor meer informatie over alles die wijzigingen in uw service heeft plaatsgevonden.
* [Servicestatus bijhouden](insights-service-health.md) om vast te stellen wanneer Azure prestaties verslechtering van of service onderbrekingen is opgetreden.
