<properties
    pageTitle="PowerShell gebruiken om u te maken van waarschuwingen voor Azure services | Microsoft Azure"
    description="PowerShell gebruiken om u te maken van Azure waarschuwingen, die kunnen worden geactiveerd meldingen of automatisering wanneer de opgegeven voorwaarden is voldaan."
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
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="use-powershell-to-create-alerts-for-azure-services"></a>PowerShell gebruiken om waarschuwingen voor Azure-services te maken

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Overzicht

In dit artikel leest u hoe u het instellen van Azure waarschuwingen via PowerShell.  

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


Voor aanvullende informatie, kunt u altijd typen ```get-help``` en klikt u vervolgens de PowerShell-opdracht op het gewenste help.

## <a name="create-alert-rules-in-powershell"></a>Waarschuwingsregels maken in PowerShell

1. Meld u aan bij Azure.   

    ```PowerShell
    Login-AzureRmAccount

    ```

2. Een lijst van de abonnementen waarover u beschikt. Controleer of u met het juiste abonnement werkt. Zo niet, stel deze in op het juiste account met de uitvoer van `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

3.  U kunt bestaande regels op een resourcegroep gebruiken de volgende opdracht uit:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

4. U kunt een regel maken, moet u eerst aantal belangrijke gegevens zijn. 
    - De **Resource-ID** voor de resource die u wilt een waarschuwing instellen voor
    - De **metrische definities** beschikbaar zijn voor de desbetreffende resource

    Een manier om toegang te krijgen van de Resource-ID is via de portal van Azure. Als dat de resource al is gemaakt, selecteert u deze in de portal. Selecteer vervolgens *Eigenschappen* in het volgende blad, onder de sectie *Instellingen* . De RESOURCE-ID is een veld in het volgende blad. Een andere manier is de [Azure Resource Explorer](https://resources.azure.com/)gebruiken.

    Een voorbeeld van de resource-id voor een web-app is

    ```
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    U kunt `Get-AzureRmMetricDefinition` om weer te geven van de lijst met alle metrische definities voor een specifieke resource.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id>
    ```

    Het volgende voorbeeld genereert een tabel met de naam van de meetwaarde en de eenheid voor die metrisch.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

    ```
    Een volledige lijst met beschikbare opties voor Get-AzureRmMetricDefinition is beschikbaar door Get-MetricDefinitions uit te voeren.


5. Het volgende voorbeeld is ingesteld van een waarschuwing voor een resource website. De waarschuwing triggers wanneer deze consistente al het verkeer ontvangt voor 5 minuten en opnieuw op wanneer deze geen verkeer ontvangt voor 5 minuten.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```

6. Als u wilt maken van webhook of e-mail verzenden als een waarschuwing geactiveerd, moet u eerst de e-mail-en/of webhooks maken. Direct maken de regel achteraf met de - acties-code en zoals wordt getoond in het volgende voorbeeld. U kunt geen koppelen webhook of e-mailberichten met regels via PowerShell al hebt gemaakt.


    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```


7. Als u wilt een waarschuwing die activeert maken van een specifieke voorwaarde in het gebeurtenissenlogboek, gebruikt u de opdrachten van de volgende formulier

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmLogAlertRule -Name myLogAlertRule -Location "East US" -ResourceGroup myresourcegroup -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup resourcegroupbeingmonitored -Actions $actionEmail, $actionWebhook
    ```

    De OperationName - overeenkomt met een type gebeurtenis in het gebeurtenissenlogboek. Voorbeelden hiervan zijn *Microsoft.Compute/virtualMachines/delete* en *microsoft.insights/diagnosticSettings/write*.

    U kunt de PowerShell-opdracht [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) gebruiken voor een lijst met mogelijke operationNames. U kunt ook kunt u de Azure-portal in het logboek activiteit zoeken en specifieke eerdere bewerkingen die u wilt maken van een waarschuwing voor zoeken. De bewerkingen die wordt weergegeven in de logboekweergave met afbeelding van de beschrijvende naam. Kijk in de JSON voor de invoer en de waarde OperationName uitlichten.   

8. Controleer of dat uw waarschuwingen correct zijn gemaakt door te kijken in de afzonderlijke regels.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

9. Uw waarschuwingen verwijderen. Deze opdrachten verwijderen de regels die eerder hebt gemaakt in dit artikel.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Volgende stappen

* [Een overzicht van Azure bewaken](monitoring-overview.md) met inbegrip van de soorten informatie kunt u verzamelen en bijhouden.
* Meer informatie over [configureren webhooks in waarschuwingen](insights-webhooks-alerts.md).
* Meer informatie over [Azure automatisering Runbooks](..\automation\automation-starting-a-runbook.md).
* Een [overzicht van de diagnostische logboeken verzamelen](monitoring-overview-of-diagnostic-logs.md) opvragen voor het verzamelen van gedetailleerde hoge frequentie afmetingen van uw service.
* Een [overzicht van de doelstellingen siteverzamelingen](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service is alleen beschikbaar en heeft gereageerd opvragen.
