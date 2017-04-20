<properties
    pageTitle="Azure-portal gebruiken om te maken van waarschuwingen voor Azure services | Microsoft Azure"
    description="Gebruik de portal van Azure Azure waarschuwingen, die kunnen worden geactiveerd meldingen of automatisering wanneer de opgegeven voorwaarden is voldaan maken."
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
    ms.date="09/23/2016"
    ms.author="robb"/>

# <a name="use-azure-portal-to-create-alerts-for-azure-services"></a>Azure-portal gebruiken om waarschuwingen voor Azure-services te maken

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Overzicht

In dit artikel leest u het instellen van Azure waarschuwingen met behulp van de Azure portal.   

U kunt ontvangen een melding op basis van controleren van de doelstellingen voor of gebeurtenissen op uw Azure services.

- **Metrische waarden** - de waarschuwing triggers wanneer de waarde van een opgegeven meting kruist een drempel die u in de gewenste richting toewijst. Dat wil zeggen er beide wordt gegenereerd wanneer eerst aan de voorwaarde is voldaan, en klik vervolgens achteraf wanneer die-voorwaarde is niet meer wordt voldaan.    
- **Gebeurtenissen activiteit** - een waarschuwing kunt activeren op *elke* gebeurtenis of, alleen wanneer een bepaald aantal gebeurtenissen plaatsvinden.


U kunt een melding in om uit te voeren van de volgende handelingen uit als er wordt configureren:

- meldingen per e-mail verzenden naar de service-beheerder en CO-beheerders
- e-mail verzenden naar extra e-mailberichten die u opgeeft.
- een webhook bellen
- uitvoering van een Azure runbook (alleen in de portal Azure) starten

U kunt configureren en informatie over waarschuwingsregels gebruiken

- [Azure-portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [opdrachtregel-interface (CLI)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)


## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Een waarschuwing regel maken op een meting in de portal van Azure

1. Zoek de resource u geïnteresseerd bent in de cmdlets voor controle en selecteert u deze in de [portal](https://portal.azure.com/).

2. Selecteer **meldingen** of **waarschuwingsregels** onder de sectie controle. De tekst en het pictogram kunnen enigszins variëren voor verschillende bronnen.  

    ![Cmdlets voor controle](./media/insights-alerts-portal/AlertRulesButton.png)


3. Selecteer de opdracht **melding toevoegen** en vul de velden.

    ![Een waarschuwing toevoegen](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **De naam van** de melding voor een regel en kiest u een **Beschrijving**, die ook in e-mailberichten weergegeven.
5. Selecteer de **meetwaarde** die u wilt controleren en kies vervolgens een **voorwaarde** en een **drempelwaarde voor** de waarde voor de meetwaarde. Ook kiezen de **periode** waarin de metrische regel moet worden voldaan voordat de waarschuwing triggers. Als u de periode "PT5M" gebruikt en de waarschuwing Hiermee wordt gezocht naar CPU dan 80%, gebeurtenis de melding; wanneer de CPU consistente bovenstaande 80% gedurende 5 minuten is. Nadat de eerste trigger plaatsvindt, gebeurtenis deze opnieuw wanneer de CPU minder dan 80% voor 5 minuten blijft. De meting CPU doet zich voor elke 1 minuut.   

6. Schakel **e-eigenaars...** als u wilt beheerders en CO-beheerders aan e-mail worden verzonden wanneer de melding wordt uitgevoerd.

7. Desgewenst kunt u extra e-mailberichten naar een melding ontvangen wanneer de melding wordt uitgevoerd, kunt u deze in het veld **aanvullende beheerder email(s)** toevoegen. Meerdere e-mailberichten met een puntkomma - scheiden*email@contoso.com;email2@contoso.com*

8. In een geldige URI in het veld **Webhook** plaatsen als u wilt dat het aangeroepen als de melding wordt uitgevoerd.

9. Als u Azure automatisering gebruikt, kunt u een Runbook moet worden uitgevoerd wanneer de melding wordt uitgevoerd.

10. Selecteer **OK** wanneer u klaar bent om de waarschuwing maken.   

Binnen enkele minuten, de melding voor een actief is en activeert zoals eerder is beschreven.

## <a name="managing-your-alerts"></a>Uw waarschuwingen beheren

Als u een melding hebt gemaakt, kunt u deze selecteren en:

- Een grafiek met de drempelwaarde voor metrische en de werkelijke waarden uit de vorige dag weergeven.
- Bewerk of verwijder deze.
- **Uitschakelen** of **inschakelen** dit als u tijdelijk wilt stoppen of cv meldingen voor deze waarschuwing ontvangen.



## <a name="next-steps"></a>Volgende stappen

* [Een overzicht van Azure bewaken](monitoring-overview.md) met inbegrip van de soorten informatie kunt u verzamelen en bijhouden.
* Meer informatie over [configureren webhooks in waarschuwingen](insights-webhooks-alerts.md).
* Meer informatie over [Azure automatisering Runbooks](..\automation\automation-starting-a-runbook.md).
* Een [overzicht van de diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md) en gedetailleerde hoge frequentie afmetingen verzamelen van uw service.
* Een [overzicht van de doelstellingen siteverzamelingen](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service is alleen beschikbaar en heeft gereageerd opvragen.
