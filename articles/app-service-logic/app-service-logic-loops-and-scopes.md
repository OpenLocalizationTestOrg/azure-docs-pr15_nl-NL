<properties
   pageTitle="Logica Apps lussen, -bereiken en Debatching | Microsoft Azure"
   description="Logica App lus, scope vaststellen en debatching concepten"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/14/2016"
   ms.author="jehollan"/>
   
# <a name="logic-apps-loops-scopes-and-debatching"></a>Logica Apps lussen, -bereiken en Debatching
  
>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica Apps 2016-04-01-preview schema en hoger.  Concepten voor oudere schema's lijken, maar bereiken zijn alleen beschikbaar voor dit schema en hoger.
  
## <a name="foreach-loop-and-arrays"></a>Een loopback ForEach en matrices
  
Logica Apps kunt u een reeks gegevens doorlopen en een actie uitvoeren voor elk item.  Dit is mogelijk via de `foreach` actie.  Klik in de ontwerpfunctie kunt u opgeven om toe te voegen een voor elke lus.  Nadat u selecteert de matrix die u wilt doorlopen, kunt u beginnen met het toevoegen van acties.  Of u momenteel zijn beperkt tot slechts één actie per foreach lus, maar deze beperking zullen worden opgeheven weken.  De lus kunt u één keer om op te geven wat er moet gebeuren bij elke waarde van de matrix.

Als codeweergave gebruikt, kunt u een voor elke lus zoals hieronder.  Dit is een voorbeeld van een voor elke lus die een e-mailbericht voor elke e-mailadres met 'microsoft.com' verzendt:

```
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  A `foreach` actie kunt sequentieel over matrixen maximaal 5.000 rijen.  Elke iteratie kunt uitvoeren in parallel, zodat deze berichten toevoegen aan een wachtrij wellicht als stroom besturingselement is vereist.
  
## <a name="until-loop"></a>Tot herhalen
  
  U kunt een actie of een reeks acties uitvoeren totdat een voorwaarde is voldaan.  De meeste gevallen hiervoor is een eindpunt bellen totdat u het antwoord dat u zoekt.  Klik in de ontwerpfunctie kunt u opgeven om toe te voegen een tot herhalen.  Nadat u acties binnen de lus toevoegt, kunt u de voorwaarde afsluiten, evenals de lus instellen limieten.  Er is een vertraging 1 minuut tussen lus maal.
  
  Als codeweergave gebruikt, kunt u een totdat lus zoals hieronder.  Dit is een voorbeeld van het bellen van een HTTP-eindpunt totdat de hoofdtekst van het antwoord de waarde heeft 'Voltooid'.  Er wordt uitgevoerd wanneer een van beide 
  
  * HTTP-antwoord heeft status 'Voltooid'
  * Deze heeft geprobeerd voor 1 uur
  * Dit is 100 keer herhaald
  
  ```
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed'),
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn en Debatching

Soms wordt een trigger een matrix van items die u wilt debatch en begint u een werkstroom per item.  Dit kunt u doen de `spliton` opdracht.  Als uw swagger trigger Hiermee geeft u een pakket dat een matrix, is een `spliton` wordt toegevoegd en begint u een uitvoeren per item.  SplitOn kunnen alleen worden toegevoegd aan een trigger.  Dit kan handmatig worden geconfigureerd of overschreven in definitie-codeweergave.  Momenteel SplitOn kunt debatch matrixen maximaal 5.000 items.  U kunt geen een `spliton` en het patroon van de reactie syncronous ook implementeren.  Een werkstroom die genoemd heeft een `response` actie naast `spliton` moet asyncronously uitvoeren en verzend een direct `202 Accepted` antwoord.  

SplitOn kan worden opgegeven in de weergave van de code als het volgende voorbeeld.  In dit krijgen een matrix van items en debatches voor elke rij.

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequency": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Bereiken

Het is mogelijk om te groeperen van een reeks acties samenwerken met een bereik.  Dit is vooral handig voor de uitvoering van afhandeling van uitzonderingen.  U kunt in de ontwerpfunctie voor toevoegen van een nieuwe scope en beginnen met het toevoegen van bewerkingen erin.  U kunt bereiken definiëren in de weergave-code als volgt uit:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
