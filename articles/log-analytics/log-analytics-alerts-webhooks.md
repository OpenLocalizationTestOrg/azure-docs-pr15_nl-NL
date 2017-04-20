<properties
   pageTitle="Log Analytics waarschuwing webhook voorbeeld"
   description="Een van de acties die u in antwoord op de melding voor een logboek Analytics uitvoeren kunt is een *webhook*, waarmee u een extern proces tot en met een enkele HTTP-aanvraag roepen. In dit artikel worden doorlopen een voorbeeld van een actie webhook maken in de melding voor een logboek analyses met marge."
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="webhooks-in-log-analytics-alerts"></a>Webhooks in Log Analytics-meldingen

Een van de acties die u in antwoord op een [melding voor een logboek Analytics uitvoeren kunt](log-analytics-alerts.md) is een *webhook*, waarmee u een extern proces tot en met een enkele HTTP-aanvraag roepen.  U kunt lezen over de details van waarschuwingen en webhooks in [waarschuwingen in Log Analytics](log-analytics-alerts.md)

In dit artikel wordt we begeleiden tot en met een voorbeeld van een actie webhook maken in de melding voor een logboek Analytics met marge dat wil een SMS-service zeggen.

>[AZURE.NOTE] U moet een vertraging-account om uit te voeren in dit voorbeeld hebben.  U kunt zich aanmelden voor een gratis account bij [slack.com](http://slack.com).

## <a name="step-1---enable-webhooks-in-slack"></a>Stap 1: webhooks inschakelen in marge
2.  Aanmelden bij een toegestane vertraging op [slack.com](http://slack.com).
3.  Een kanaal selecteren in de sectie **kanalen** in het linkerdeelvenster.  Dit is het kanaal dat het bericht wordt verzonden naar.  U kunt een van de standaard-kanalen zoals **Algemeen** of **willekeurig**selecteren.  In een productieschema maakt u waarschijnlijk een speciale kanaal zoals **criticalservicealerts**. <br>

    ![Toegestane vertraging kanalen](media/log-analytics-alerts-webhooks/oms-webhooks01.png)

3. Klik op **een app toevoegen of aangepaste integratie** om te openen van de App-gids.
3.  Typ *webhooks* in het zoekvak en selecteer vervolgens **Binnenkomende WebHooks**. <br>

    ![Toegestane vertraging kanalen](media/log-analytics-alerts-webhooks/oms-webhooks02.png)

4.  Klik op **installeren** naast de teamnaam van uw.
5.  Klik op **configuratie toevoegen**.
6.  Selecteer de het kanaal dat u wilt gebruiken voor dit voorbeeld en klik vervolgens op **de integratie van de binnenkomende WebHooks toevoegen**.  
6. Kopieer de **URL van de Webhook**.  U plakt dit in de waarschuwing configuratie. <br>

    ![Toegestane vertraging kanalen](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Stap 2: waarschuwingsregels in Log analyses maken
1.  [Een waarschuwing regel maken](log-analytics-alerts.md) met de volgende instellingen.
    - Query:```    Type=Event EventLevelName=error ```
    - Controleren op deze melding elke: 5 minuten
    - Het aantal resultaten is: groter is dan 10
    - Via deze tijdvenster: 60 minuten
    - Selecteer **Ja** **Webhook** en **Nee** voor de andere acties.
7. Plak de URL van de marge in het veld **Webhook-URL** .
8. Selecteer de optie voor het **opnemen van een aangepaste JSON-nettolading**.
9. Toegestane verwacht een nettolading opgemaakt in JSON met een parameter *tekst*met de naam.  Dit is de tekst die wordt weergegeven in het bericht dat wordt gemaakt.  U kunt een of meer van de waarschuwing parameters met de *#* symbool bijvoorbeeld zoals in het volgende voorbeeld.

    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```

    ![voorbeeld JSON nettolading](media/log-analytics-alerts-webhooks/oms-webhooks07.png)

9.  Klik op **Opslaan** om op te slaan van de huidige regel.

10. Wacht voldoende tijd vrij voor een melding in om te worden gemaakt en schakel vervolgens marge voor een bericht dat ongeveer als volgt uit.

    ![voorbeeld webhook in marge](media/log-analytics-alerts-webhooks/oms-webhooks08.png)


### <a name="advanced-webhook-payload-for-slack"></a>Geavanceerde webhook nettolading voor marge

U kunt binnenkomende berichten met marge uitgebreid aanpassen. Zie [Binnenkomende Webhooks](https://api.slack.com/incoming-webhooks) op de website van de toegestane vertraging voor meer informatie. Hier volgt een meer complexe nettolading een uitgebreide bericht maken met opmaak:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


Dit gegenereerd door een bericht in de toegestane vertraging van de volgende strekking.

![Voorbeeld van bericht in marge](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Overzicht

Met deze regel waarschuwing op hun plaats staan moet u een bericht verzonden naar marge telkens wanneer het criterium is voldaan.  

Dit is slechts één voorbeeld van een actie die u in antwoord op een melding maken kunt.  U kunt een webhook actie die een andere externe service u belt, een actie runbook te starten een runbook in Azure automatisering of een e-actie een e-mail te verzenden naar uzelf of aan andere geadresseerden maken.   

## <a name="next-steps"></a>Volgende stappen

- Leer meer over [waarschuwingen in Log analyses](log-analytics-alerts.md) , met inbegrip van andere acties.
- [Runbooks maken in Azure automatisering](../automation/automation-webhooks.md) die kunnen worden aangeroepen vanuit een webhook.
