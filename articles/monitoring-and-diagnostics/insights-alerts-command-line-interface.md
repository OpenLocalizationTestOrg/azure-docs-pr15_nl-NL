<properties
    pageTitle="De platforms Command Line Interface (CLI) gebruiken om te maken van waarschuwingen voor Azure services | Microsoft Azure"
    description="De opdracht lijn-interface gebruiken om te maken van Azure waarschuwingen, die kunnen worden geactiveerd meldingen of automatisering wanneer de opgegeven voorwaarden is voldaan."
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
    ms.date="10/24/2016"
    ms.author="robb"/>

# <a name="use-the-cross-platform-command-line-interface-cli-to-create-alerts-for-azure-services"></a>De platforms Command Line Interface (CLI) gebruiken om waarschuwingen voor Azure-services te maken

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Overzicht

In dit artikel leest u hoe u het instellen van Azure waarschuwingen met de opdracht lijn Interface (CLI).

>[AZURE.NOTE] Azure Monitor is de nieuwe naam voor het zogenoemde "Azure inzichten" tot 25e sep 2016. Echter bevatten de naamruimten en dus de onderstaande opdrachten nog steeds de "inzichten".

U kunt ontvangen een melding op basis van controleren van de doelstellingen voor of gebeurtenissen op uw Azure services.

- **Metrische waarden** - de waarschuwing triggers wanneer de waarde van een opgegeven meting kruist een drempel die u in de gewenste richting toewijst. Dat wil zeggen er beide wordt gegenereerd wanneer eerst aan de voorwaarde is voldaan, en klik vervolgens achteraf wanneer die-voorwaarde is niet meer wordt voldaan.    
- **Gebeurtenissen activiteit** - een waarschuwing kunt activeren op *elke* gebeurtenis of, alleen wanneer een bepaald aantal gebeurtenissen plaatsvinden.

U kunt een melding in om uit te voeren van de volgende handelingen uit als er wordt configureren:

- meldingen per e-mail verzenden naar de service-beheerder en CO-beheerders
- e-mail verzenden naar extra e-mailberichten die u opgeeft.
- een webhook bellen
- uitvoering van een Azure runbook (alleen van de Azure-portal op dit moment) starten

U kunt configureren en informatie over waarschuwingsregels gebruiken

- [Azure-portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [opdrachtregel-interface (CLI)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)


U kunt altijd help voor opdrachten ontvangen door een opdracht te typen en stellen - help aan het einde. Bijvoorbeeld:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a>Waarschuwingsregels met de CLI maken

1. De vereisten en meld u aan bij Azure uitvoeren. Zie [Azure Monitor CLI voorbeelden](insights-cli-samples.md). Kortom, de CLI installeren en uitvoeren van deze opdrachten. Ze krijgen u bent aangemeld, welk abonnement u gebruikt, en voorbereiden u uitvoeren van Azure Monitor opdrachten weergeven.


    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2.  U kunt bestaande regels op een resourcegroep gebruiken de volgende formulier **azure inzichten waarschuwingen regelsets lijst** *[opties] &lt;resourceGroup&gt; *

    ```console
    azure insights alerts rule list myresourcegroupname

    ```
3. U kunt een regel maken, moet u eerst aantal belangrijke gegevens zijn.
    - De **Resource-ID** voor de resource die u wilt een waarschuwing instellen voor
    - De **metrische definities** beschikbaar zijn voor de desbetreffende resource

    Een manier om toegang te krijgen van de Resource-ID is via de portal van Azure. Als dat de resource al is gemaakt, selecteert u deze in de portal. Selecteer vervolgens *Eigenschappen* in het volgende blad, onder de sectie *Instellingen* . De *RESOURCE-ID* is een veld in het volgende blad. Een andere manier is de [Azure Resource Explorer](https://resources.azure.com/)gebruiken.

    Een voorbeeld van de resource-id voor een web-app is

    ```console
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Als u een lijst met de beschikbare aan de doelstellingen en de eenheden die aan de doelstellingen voor het vorige voorbeeld van de resource, gebruikt u de volgende opdracht uit CLI:  

    ```console
    azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
    ```

    _PT1M_ is de granulatie van de beschikbare meting (1-minuut intervallen). Met verschillende granulaties, hebt u verschillende metrische opties.


4. Als u wilt een meetwaarde gebaseerde waarschuwingen regel maken door een opdracht van de volgende formulier te gebruiken:

    **Azure inzichten regel-meetwaarde voor meldingen instellen** *[opties] &lt;ruleName&gt; &lt;locatie&gt; &lt;resourceGroup&gt; &lt;venstergrootte&gt; &lt;operator&gt; &lt;drempel&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt; *

    Het volgende voorbeeld is ingesteld van een waarschuwing voor een resource website. De waarschuwing triggers wanneer deze consistente al het verkeer ontvangt voor 5 minuten en opnieuw op wanneer deze geen verkeer ontvangt voor 5 minuten.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```

5. Als u wilt maken van webhook of e-mail verzenden wanneer u een melding wordt uitgevoerd, moet u eerst de e-mail-en/of webhooks maken. Maak de regel direct achteraf. U kunt geen koppelen webhook of e-mailberichten met regels met de CLI al hebt gemaakt.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```


6. Als u wilt een waarschuwing die wordt uitgevoerd op een specifieke voorwaarde in het gebeurtenissenlogboek maken, gebruikt u het formulier:

    **Meld u inzichten waarschuwingen regel instellen** *[opties] &lt;ruleName&gt; &lt;locatie&gt; &lt;resourceGroup&gt; &lt;operationName&gt; *

    Bijvoorbeeld

    ```console
    azure insights alerts rule log set myActivityLogRule eastus myresourceGroupName Microsoft.Storage/storageAccounts/listKeys/action
    ```

    De operationName overeenkomt met een gebeurtenistype voor een vermelding in het gebeurtenissenlogboek. Voorbeelden hiervan zijn *Microsoft.Compute/virtualMachines/delete* en *microsoft.insights/diagnosticSettings/write*.

    U kunt de PowerShell-opdracht [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) gebruiken voor een lijst met mogelijke operationNames. U kunt ook kunt u de Azure-portal in het logboek activiteit zoeken en specifieke eerdere bewerkingen die u wilt maken van een waarschuwing voor zoeken. De bewerkingen die wordt weergegeven in de logboekweergave met afbeelding van de beschrijvende naam. Kijk in de JSON voor de invoer en de waarde OperationName uitlichten.   

7. U kunt controleren dat uw waarschuwingen correct zijn gemaakt door te kijken op een afzonderlijke regel.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```

8. Als u wilt verwijderen van regels, een opdracht aan het formulier te gebruiken:

    **inzichten waarschuwingen regel verwijderen** [opties] &lt;resourceGroup&gt; &lt;ruleName&gt;

    Deze opdrachten verwijderen de regels die eerder in dit artikel worden gemaakt.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```



## <a name="next-steps"></a>Volgende stappen

* [Een overzicht van Azure bewaken](monitoring-overview.md) met inbegrip van de soorten informatie kunt u verzamelen en bijhouden.
* Meer informatie over [configureren webhooks in waarschuwingen](insights-webhooks-alerts.md).
* Meer informatie over [Azure automatisering Runbooks](..\automation\automation-starting-a-runbook.md).
* Een [overzicht van de diagnostische logboeken verzamelen](monitoring-overview-of-diagnostic-logs.md) opvragen voor het verzamelen van gedetailleerde hoge frequentie afmetingen van uw service.
* Een [overzicht van de doelstellingen siteverzamelingen](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service is alleen beschikbaar en heeft gereageerd opvragen.
