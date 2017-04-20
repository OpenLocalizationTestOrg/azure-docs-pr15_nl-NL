<properties 
   pageTitle="Waarschuwingen in Log Analytics | Microsoft Azure"
   description="Waarschuwingen in Log Analytics kunnen belangrijke informatie in uw bibliotheek OMS identificeren en proactief een melding van problemen of roepen acties om te proberen te corrigeren.  Dit artikel wordt beschreven hoe u een regel voor waarschuwingen en details maakt de verschillende acties die kunnen nemen."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="bwren" />

# <a name="alerts-in-log-analytics"></a>Waarschuwingen in Log Analytics

Waarschuwingen in Log Analytics identificeren belangrijke informatie in uw bibliotheek OMS.  Waarschuwingsregels automatisch log zoeken op basis van een planning en maken van een waarschuwing record als de resultaten aan bepaalde criteria voldoen.  De regel kan een of meer acties om proactief een melding van de waarschuwing of een ander proces roepen vervolgens automatisch uitgevoerd.   

![Meld u aan analyses waarschuwingen](media/log-analytics-alerts/overview.png)

## <a name="creating-an-alert-rule"></a>Een waarschuwing regel maakt
Een waarschuwing als regel wilt maken, begint u met het maken van een zoekopdracht log voor de records die de waarschuwing moet roepen.  De knop **Waarschuwing** zijn beschikbaar zodat u kunt maken en configureren van de huidige regel.

1.  Klik op **Log zoeken**op de pagina OMS overzicht.
2.  Maak een nieuwe log-zoekopdracht of Selecteer een opgeslagen logboek zoeken. 
3.  Klik op een **Waarschuwing** boven aan de pagina om de **Waarschuwingsregel toevoegen** -venster te openen.
4. Raadpleeg de tabellen hieronder voor meer informatie over de opties voor het configureren van de melding.
5. Wanneer u het tijdvenster voor de waarschuwing regel opgeeft, wordt het nummer van bestaande records die voldoen aan de zoekcriteria voor die tijdvenster worden weergegeven.  Hiermee kunt u bepalen van de frequentie waarmee u het aantal resultaten dat u verwacht.
4.  Klik op **Opslaan** om te voltooien van de huidige regel.  Start het onmiddellijk uitvoeren.


![Waarschuwing regel toevoegen](media/log-analytics-alerts/add-alert-rule.png)

| Eigenschap | Beschrijving |
|:--|:--|
| **Informatie over waarschuwingen** | |
| Naam |  Unieke naam voor de huidige regel. |
| Ernst | Ernst van de waarschuwing die door deze regel wordt gemaakt. |
| Zoekquery | Selecteer **huidige zoekopdracht gebruiken** om te gebruiken van de huidige query of Selecteer een bestaande opgeslagen zoekactie in de lijst.  De querysyntaxis van de is opgegeven in het tekstvak waar u kunt deze desgewenst wijzigen.  |
| Tijdvenster | Geeft het tijdsbereik voor de query.  De query retourneert alleen de records die zijn gemaakt in dit bereik van de huidige tijd.  Dit is een waarde tussen 5 minuten en 24 uur.  Moet groter zijn dan of gelijk aan de frequentie van waarschuwingen.  <br><br> Bijvoorbeeld, als het tijdvenster is ingesteld op 60 minuten en de query wordt uitgevoerd bij 1:15 PM, alleen de records die zijn gemaakt tussen 12:15 PM en 1:15 PM geretourneerd. |
| **Planning** |     
| Drempelwaarde | Criteria voor wanneer maakt u een melding.  Een melding wordt gemaakt als het aantal records geretourneerd door de query aan deze criteria voldoet. |
| Frequentie van waarschuwingen | Hiermee geeft u op hoe vaak de query moet worden uitgevoerd.  Is een waarde tussen 5 minuten en 24 uur.  Moet gelijk is aan of kleiner is dan het tijdvenster. |
| Waarschuwingen onderdrukken | Wanneer u op onderdrukken voor de waarschuwing regel inschakelt, worden acties voor de regel voor een gedefinieerde tijdsduur na het maken van een melding voor een nieuwe uitgeschakeld.  De regel nog actief is en maakt waarschuwing records als het criterium is voldaan.  Dit is toe te staan dat u tijd kunt het probleem te verhelpen zonder u dubbele acties uitvoert. |
| **Acties** | |
| E-mailmelding | Geef **Ja** als u een e-mailbericht verzonden als de melding wordt geactiveerd. |
| Onderwerp    | Onderwerp in het e-mailbericht.  U kunt de tekst van het bericht niet wijzigen. |
| Geadresseerden | De adressen van alle e-mailgeadresseerden.  Als u meer dan één adres opgeeft, scheidt u de adressen met een puntkomma (;). |
| Webhook | Geef op **Ja** als u bellen een webhook wilt wanneer de melding wordt geactiveerd. |
| Webhook URL | De URL van de webhook. |
| Aangepaste JSON nettolading opnemen | Selecteer deze optie als u wilt de standaard-nettolading vervangen door een aangepaste nettolading. |
| Voer uw aangepaste JSON-nettolading | De aangepaste nettolading voor de webhook.  Raadpleeg het vorige gedeelte. |
| Runbook | Geef op **Ja** als u een runbook Azure automatisering starten wilt wanneer de melding wordt geactiveerd. |
| Selecteer een runbook | Selecteer het runbook starten vanuit het runbooks in het account automatisering is geconfigureerd in uw oplossing automatisering. |
| Klik op uitvoeren | Selecteer **Azure** het runbook uitvoeren in de cloud Azure.  Selecteer **Hybride werknemer** het runbook uitvoeren op een [Hybride Runbook werknemer](..\automation\automation-hybrid-runbook-worker.md) in uw lokale omgeving. |


## <a name="manage-alert-rules"></a>Waarschuwingsregels beheren
U kunt een lijst met alle waarschuwingsregels openen in het menu **waarschuwingen** in Log Analytics- **Instellingen**.  

![Waarschuwingen beheren](./media/log-analytics-alerts/configure.png)

1. Selecteer de tegel **-Instellingen** in de OMS-console.
2. Selecteer **meldingen**.

U kunt meerdere acties uitvoeren vanuit deze weergave.

- Een regel uitschakelen door in te schakelen **uit** ernaast.
- Een waarschuwing regel bewerken door te klikken op het potloodpictogram ernaast.
- Een waarschuwing regel verwijderen door te klikken op het pictogram **X** ernaast. 


## <a name="setting-time-windows"></a>Tijd windows instellen 

### <a name="event-alerts"></a>Gebeurtenis-meldingen

Gebeurtenissen bevatten gegevensbronnen zoals gebeurtenislogboeken van Windows, Syslog, en aangepaste logboeken.  U wilt maken van een melding wanneer een bepaalde foutgebeurtenis wordt gemaakt, of wanneer meerdere foutgebeurtenissen worden gemaakt voor een bepaald tijdstip-venster.

Om uzelf te waarschuwen bij een eenmalige gebeurtenis, stelt u het aantal resultaten op groter is dan 0 en de frequentie en tijdvenster op 5 minuten.  Of wordt de query uitvoert om de 5 minuten en controleer de vindplaats van één gebeurtenis die is gemaakt sinds de laatste keer dat de query is uitgevoerd.  De frequentie van een langer kunt uitstellen de tijd tussen de gebeurtenis die worden verzameld en de melding die wordt gemaakt.

Sommige toepassingen kunnen zich aanmelden voor een incidentele fout die niet mag een melding per se verhogen.  De toepassing kan bijvoorbeeld het proces opnieuw die de foutgebeurtenis hebt gemaakt en dan slagen van de volgende keer dat.  In dit geval u mogelijk niet wilt maken van de melding, tenzij meerdere gebeurtenissen worden gemaakt voor een bepaald tijdstip-venster.  

In sommige gevallen wilt u mogelijk een waarschuwing bij afwezigheid van een gebeurtenis maken.  Bijvoorbeeld een proces kan zich aanmelden normale gebeurtenissen om aan te geven dat deze correct werkt.  Als deze niet Meld u aan een van deze gebeurtenissen binnen een bepaald tijdstip-venster, moet klikt u vervolgens een melding worden gemaakt.  In dit geval stelt u de drempelwaarde voor *minder dan*1.

### <a name="performance-alerts"></a>Prestaties van meldingen

[Prestatiegegevens](log-analytics-data-sources-performance-counters.md) wordt opgeslagen als records in de bibliotheek OMS overeen met gebeurtenissen.  De waarde in elke record is het gemiddelde gemeten over de vorige 30 minuten.  Als u waarschuwen wilt wanneer een prestatie-item groter is dan een bepaalde drempel, moet deze drempel worden opgenomen in de query.

Stel dat u wilt weten wanneer de processor wordt uitgevoerd dan 90% gedurende 30 minuten, gebruikt u een query zoals *Type = Perf ObjectName Processor CounterName = "% processortijd" = tegenwaarde > 90* en de drempelwaarde voor de waarschuwing regel moet *groter zijn dan 0*.  

 Aangezien [prestaties records](log-analytics-data-sources-performance-counters.md) worden samengevoegd elke 30 minuten ongeacht de frequentie dat u elk item verzamelen, kan een kleiner is dan 30 minuten tijdvenster geen records retourneren.  Als u het tijdvenster op 30 minuten zorgt ervoor dat dat u één record ophalen voor elke verbonden bron die staat voor het gemiddelde op dat moment.

## <a name="alert-actions"></a>Meldingen

Naast een waarschuwing record maakt, kunt u de huidige regel automatisch uitgevoerd een of meer acties configureren.  Acties kunnen proactief een melding van de waarschuwing of aanroepen van een proces waarmee wordt geprobeerd het probleem dat is gedetecteerd worden gecorrigeerd.  De volgende secties worden de acties die momenteel beschikbaar zijn.

### <a name="email-actions"></a>E-mailacties
Een e-mailbericht met de details van de melding voor een verzenden e-mailacties naar een of meer geadresseerden.  Kunt u het onderwerp van het e-mailbericht, maar de inhoud is een standaardindeling door Log Analytics gemaakt.  Het bevat samenvatting van de gegevens zoals de naam van de waarschuwing naast details van maximaal tien records geretourneerd door de zoekopdracht log.  Deze bevat ook een koppeling naar een zoekopdracht log in Log Analytics waarmee de gehele set records van die query worden geretourneerd.   De afzender van het e-mailbericht is *Team van Microsoft bewerkingen Management Suite &lt; noreply@oms.microsoft.com *. 


### <a name="webhook-actions"></a>Webhook acties

Webhook acties kunnen u een extern proces tot en met een enkele HTTP POST-aanvraag roepen.  De service wordt aangeroepen moet ondersteuning webhooks en bepalen hoe elke nettolading wordt gebruikt deze ontvangt.  U kunt ook een REST API die niet specifiek webhooks ondersteunen, zolang de aanvraag is in een indeling die de API begrijpt oproepen.  Voorbeelden van het gebruik van een webhook in antwoord op een melding gebruikt een service zoals [toegestane vertraging van](http://slack.com) een bericht met de details van de waarschuwing of het maken van een incident in een service zoals [PagerDuty](http://pagerduty.com/)verzenden.  

Een volledige stapsgewijze instructies voor het maken van een regel voor waarschuwingen met een webhook om te bellen van een steekproef-service is beschikbaar op [Webhooks in Log Analytics waarschuwingen](log-analytics-alerts-webhooks.md).

Webhooks zijn een URL en een nettolading opgemaakt in JSON die de gegevens die zijn verzonden naar de externe service is.  Standaard bevat de nettolading de waarden in de volgende tabel.  U kunt deze nettolading vervangen door een aangepaste plattegrond van uw eigen.  In dat geval kunt u de variabelen in de tabel voor elk van de parameters hun waarde opnemen in uw aangepaste nettolading.


| Parameter | Variabele | Beschrijving |
|:--|:--|:--|
| AlertRuleName | #alertrulename | De naam van de waarschuwing regel. |
| AlertThresholdOperator | #thresholdoperator | Drempelwaarde voor de operator voor de huidige regel.  *Groter dan* of *kleiner dan*. |
| AlertThresholdValue | #thresholdvalue | Drempelwaarde voor de huidige regel. |
| LinkToSearchResults | #linktosearchresults | Koppeling naar Log Analytics log zoeken die resulteert in de records door de query die de melding hebt gemaakt. |
| ResultCount  | #searchresultcount | Het aantal records in de lijst met zoekresultaten. |
| SearchIntervalEndtimeUtc  | #searchintervalendtimeutc | Eindtijd voor de query in UTC-indeling. |
| SearchIntervalInSeconds | #searchinterval | Tijdvenster voor de huidige regel. |
| SearchIntervalStartTimeUtc  | #searchintervalstarttimeutc | Begintijd voor de query in UTC-indeling. |
| SearchQuery | #searchquery | Log zoekquery gebruikt door de huidige regel. |
| Zoekresultaten | Zie hieronder | Records die door de query in de indeling van JSON geretourneerd.  Beperkt tot de eerste 5000 records. |
| WorkspaceID | #workspaceid | ID van uw werkruimte OMS. |


U kunt de volgende aangepaste achter dat een enkele parameter genaamd *tekst*bevat bijvoorbeeld mogelijk opgeven.  De service die deze webhook belt zou worden verwachtte als deze parameter.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

In dit voorbeeld nettolading wilt omzetten in ongeveer als volgt te werk wanneer u naar de webhook verzonden.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Als u wilt opnemen zoekresultaten in een aangepaste nettolading, moet u de volgende regel toevoegen als een eigenschap van het hoogste niveau in de json-nettolading.  

    "IncludeSearchResults":true

Bijvoorbeeld als u wilt maken van een aangepaste achter dat alleen de naam van de waarschuwing en de lijst met zoekresultaten bevat, kunt u het volgende. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


U kunt een volledig voorbeeld van een waarschuwing regel maakt met een webhook starten van een externe service op [Log Analytics Waarschuw webhook voorbeeld](log-analytics-alerts-webhooks.md)doorlopen.

### <a name="runbook-actions"></a>Runbook acties

Een runbook starten Runbook acties in Azure automatisering.  Pas dit type actie gebruiken, moet u de [oplossing voor automatisering](log-analytics-add-solutions.md) geïnstalleerd en geconfigureerd in uw werkruimte OMS hebben.  Als u niet geïnstalleerd hebt wanneer u een nieuwe waarschuwing regel maakt, wordt een koppeling naar de installatie wordt weergegeven.  U kunt selecteren in de runbooks in het automatisering-account dat u in de oplossing automatisering hebt geconfigureerd.

Runbook acties start het runbook met een [webhook](../automation/automation-webhooks.md).  Wanneer u de huidige regel maakt, wordt deze automatisch een nieuwe webhook voor het runbook maken met de naam **OMS waarschuwing Remediation** gevolgd door een GUID.  

U kunt geen rechtstreeks vullen de parameters van het runbook, maar de [parameter $WebhookData](../automation/automation-webhooks.md) bevat de details van de melding, waaronder de resultaten van de zoekfunctie waarmee het is gemaakt.  Het runbook moet **$WebhookData** instelt als een parameter voor deze voor toegang tot de eigenschappen van de melding.  De waarschuwing gegevens is beschikbaar in de indeling van json in één eigenschap **zoekresultaten** in de eigenschap **RequestBody** van **$WebhookData**genoemd.  Dit heeft met de eigenschappen in de volgende tabel.


| Knooppunt | Beschrijving |
|:--|:--|
| ID         | Pad en de GUID van de zoekopdracht. |
| __metadata | Informatie over de waarschuwing waaronder het aantal records en status van de lijst met zoekresultaten. |
|  waarde     |  Apart item voor elke record in de lijst met zoekresultaten.  De details van de invoer komen overeen met de eigenschappen en waarden van de record.   |

Het volgende runbook zou bijvoorbeeld extraheren van de records die het resultaat van het logboek zoeken en andere eigenschappen op basis van het type van elke record toewijzen.  Houd er rekening mee dat het runbook begint met het converteren van **RequestBody** van json, zodat deze kan worden gewerkt met als een object in PowerShell.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value
    
    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer
        
        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }
        
        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="alert-records"></a>Waarschuwing records

Waarschuwing records die zijn gemaakt door waarschuwingsregels in Log Analytics hebben een **Type** van **een melding** en een **SourceSystem** van **OMS**.  Worden hebt de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Type          | *Waarschuwen* |
| SourceSystem  | *OMS* |
| AlertSeverity | Het prioriteitsniveau van de waarschuwing. |
| AlertName     | De naam van de waarschuwing. |
| Query         | Tekst van de query die is uitgevoerd.  |
| QueryExecutionEndTime   | Einde van het tijdsbereik voor de query. |
| QueryExecutionStartTime | Begin van het tijdsbereik voor de query.  |
| TimeGenerated | Datum en tijd waarop die de melding is gemaakt. |

Er zijn andere soorten waarschuwingen records die zijn gemaakt door de [Waarschuwing beheeroplossing](log-analytics-solution-alert-management.md) en [Hiermee exporteert u Power BI](log-analytics-powerbi.md).  Deze hebben allemaal een **Type** **Waarschuwing** , maar worden onderscheiden door hun **SourceSystem**.




## <a name="next-steps"></a>Volgende stappen

- Installeer de [Waarschuwing beheeroplossing](log-analytics-solution-alert-management.md) voor het analyseren van waarschuwingen die zijn gemaakt in Log Analytics samen met waarschuwingen die worden verzameld uit het systeem Center Operations Manager (SCOM).
- Lees meer over [log zoekopdrachten](log-analytics-log-searches.md) die waarschuwingen kunt genereren.
- Stapsgewijze instructies voor het [configureren van een webook](log-analytics-alerts-webhooks.md) met een waarschuwing regel te voltooien.  
- Leer hoe u het schrijven van [runbooks in Azure automatisering](https://azure.microsoft.com/documentation/services/automation) als u wilt een problemen die worden aangeduid met waarschuwingen te verhelpen.