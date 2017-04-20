<properties
    pageTitle="Verticaal schalen Azure virtuele machines schaal sets | Microsoft Azure"
    description="Hoe u een virtuele Machine verticaal de schaal in antwoord op waarschuwingen met Azure automatisering bewaken"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="madhana"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="guybo"/>

# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Hiermee stelt u verticale automatisch schalen met VM schaal

In dit artikel wordt beschreven hoe verticaal schalen Azure [Virtuele machines schaal Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/) met of zonder reprovisioning. Raadpleeg [verticaal schalen Azure virtuele machines met Azure automatisering](../virtual-machines/virtual-machines-windows-vertical-scaling-automation.md)voor verticale schaling van VMs die niet in schaal sets.

Verticaal schalen, ook bekend als _vergroten_ en _verkleinen_, betekent dat er oplopende of aflopende volgorde VM (VM) grootte in antwoord op een werkbelasting. Vergelijk deze met [horizontale schaalbaarheid](./virtual-machine-scale-sets-autoscale-overview.md), ook wel genoemd _schaal af_ en _schaal in_waar het aantal VMs afhankelijk van de werklast wordt gewijzigd.

Reprovisioning betekent dat er een bestaande VM verwijderen en vervangen door een nieuwe record. Wanneer u vergroten of verkleinen van de grootte van VMs in een VM schaal instellen, in sommige gevallen wilt u vergroten of verkleinen bestaande VMs en bewaren van uw gegevens gebruikt, terwijl in andere gevallen moet u nieuwe VMs van de nieuwe grootte implementeren. In dit document behandelt beide gevallen.

Verticale schaalbaarheid is handig wanneer:

- Een service die is gebaseerd op virtuele machines is onder gebruikt (bijvoorbeeld in het weekend). VM kleiner kunt verlagen maandelijkse.
- VM vergroten omgaan met grotere aanvraag zonder extra VMs maken.

U kunt instellen verticaal schalen moeten geactiveerde op basis van metrieke waarschuwingen op basis van uw VM schaal instellen. Wanneer de melding wordt geactiveerd deze wordt uitgevoerd een webhook die triggers een runbook die upgrades schaal instellen omhoog of omlaag. Verticale schaalbaarheid kan worden geconfigureerd door deze stappen uit:

1. Maak een account Azure automatisering met starten als de mogelijkheid.
2. Azure automatisering verticale schaal runbooks voor VM schaal Sets importeren in uw abonnement.
3. Een webhook toevoegen aan uw runbook.
4. Een waarschuwing toevoegen aan uw VM schaal instellen met een melding webhook.

> [AZURE.NOTE] Verticale autoscaling kan alleen worden beheerd in een bepaalde reeks VM grootte. De specificaties van elke grootte voordat u besluit aan de nieuwe schaal uit een andere vergelijken (hoger aantal geeft niet altijd aan grotere VM). U kunt aan de nieuwe schaal tussen de volgende paren van formaten:

>| VM die groter zijn schaal koppelen |   |
|---|---|
|  Standard_A0 | Standard_A11 |
|  Standard_D1 |  Standard_D14 |
|  Standard_DS1 |  Standard_DS14 |
|  Standard_D1v2 |  Standard_D15v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Een Azure automatisering-Account maken met de mogelijkheid starten als

Het eerste wat dat u moet doen is een Azure automatisering account maken die wordt gehost in het runbooks kon u de schaal van de exemplaren VM schaal instellen. [Azure automatisering](https://azure.microsoft.com/services/automation/) geïntroduceerd onlangs de "Als account uitvoeren"-functie waardoor u de instelling van de Service hoofdsom voor het automatisch uitvoeren van de runbooks van een gebruiker namens heel eenvoudig. U vindt meer informatie over dit in het artikel hieronder:

* [Runbooks verifiëren met Azure uitvoeren als account](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Azure automatisering verticale schaal runbooks importeren in uw abonnement

De runbooks die nodig zijn voor uw VM schaal Sets verticaal schalen zijn al gepubliceerd in de galerie met Azure automatisering Runbook. Als u wilt importeren Volg in uw abonnement de stappen in dit artikel:

* [Runbook en module galerieën voor het automatiseren van Azure](../automation/automation-runbook-gallery.md)

Kies de optie Bladeren galerie in het menu Runbooks:

![Runbooks te importeren][runbooks]

De runbooks die moeten worden geïmporteerd worden weergegeven. Selecteer het runbook op basis van of u wilt dat verticale schaling met of zonder reprovisioning:

![Galerie met Runbooks][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Een webhook toevoegen aan uw runbook

Nadat u hebt geïmporteerd toevoegen de runbooks u moet een webhook aan het runbook, zodat deze kan worden veroorzaakt door een melding van een VM schaal instellen. De details van het maken van een webhook voor uw Runbook worden beschreven in dit artikel:

* [Azure automatisering webhooks](../automation/automation-webhooks.md)

> [AZURE.NOTE] Zorg ervoor dat u de webhook URI kopiëren voordat u het dialoogvenster webhook afsluit, zoals u dit in het volgende gedeelte moet.

## <a name="add-an-alert-to-your-vm-scale-set"></a>Een waarschuwing toevoegen aan uw VM schaal instellen

Hieronder vindt u een PowerShell-script waarin wordt getoond hoe u een waarschuwing toevoegen aan een VM schaal instellen. Raadpleeg het volgende artikel om de naam van de meetwaarde gestart, de melding op: [Azure Monitor autoscaling algemene doelstellingen](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [AZURE.NOTE] Het wordt aanbevolen voor het configureren van een redelijk tijdvenster voor de melding om te voorkomen activeert verticale schalen en alle bijbehorende serviceonderbreking, te vaak. Houd rekening met een venster van het laagste 20-30 minuten of langer. Houd rekening met horizontale schaalbaarheid als u wilt vermijden.

Raadpleeg de volgende artikelen voor meer informatie over het maken van waarschuwingen:

* [Voorbeelden van Azure Monitor PowerShell aan de slag](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Voorbeelden van Azure Monitor platforms CLI snel aan de slag](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Overzicht

In dit artikel werd eenvoudige verticale schaal voorbeelden. U kunt verbinding maken met een uitgebreide reeks gebeurtenissen met een aangepaste reeks acties met deze bouwstenen - automatisering-account, runbooks, webhooks, waarschuwingen.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
