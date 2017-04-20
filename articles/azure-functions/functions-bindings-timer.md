<properties
    pageTitle="Azure functies timer trigger | Microsoft Azure"
    description="Meer informatie over het gebruik van timer triggers in Azure-functies."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure werkt, functies, verwerking van gebeurtenis, dynamische berekeningscluster, als u kiest architectuur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-timer-trigger"></a>Azure functies timer trigger

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In dit artikel wordt uitgelegd hoe timer triggers configureren in Azure-functies. Timer gebeurtenis gesprek functies op basis van een planning, één keer of terugkerende.  

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-timer-trigger"></a>Function.JSON voor timer-trigger

Het bestand *function.json* biedt een planning-expressie. Bijvoorbeeld de volgende planning wordt uitgevoerd de functie elke minuut:

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

De trigger timer omgaat met meerdere exemplaren schalen automatisch: slechts één exemplaar van een bepaalde timerfunctie wordt uitgevoerd in alle exemplaren.

## <a name="format-of-schedule-expression"></a>Opmaak van de expressie planning

De expressie planning is een [CRON expressie](http://en.wikipedia.org/wiki/Cron#CRON_expression) met 6 velden: `{second} {minute} {hour} {day} {month} {day of the week}`. 

Houd er rekening mee dat veel van de cron expressies u zoeken naar online het veld {tweede} weglaat, zodat als u vanuit een van die kopieert u hebt om aan te passen voor de extra veld. 

Hier volgen enkele andere planning voorbeelden van expressies:

Om te activeren om de 5 minuten:

```json
"schedule": "0 */5 * * * *"
```

Om te activeren één keer per uur Klik boven aan:

```json
"schedule": "0 0 * * * *",
```

Om te activeren om de twee uur:

```json
"schedule": "0 0 */2 * * *",
```

Om te activeren één keer per uur van 9 AM tot 5 PM:

```json
"schedule": "0 0 9-17 * * *",
```

Om te activeren om 9:30 AM elke dag:

```json
"schedule": "0 30 9 * * *",
```

Om te activeren om 9:30 AM elke weekdag:

```json
"schedule": "0 30 9 * * 1-5",
```

## <a name="timer-trigger-c-code-example"></a>Timer trigger C#-codevoorbeeld

In dit voorbeeld C#-code schrijft een één logboek telkens wanneer die de functie wordt geactiveerd.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
