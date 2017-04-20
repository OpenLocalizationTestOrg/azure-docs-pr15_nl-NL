<properties
    pageTitle="De trigger terugkeerpatroon in logica apps toevoegen | Microsoft Azure"
    description="Overzicht van de trigger terugkeerpatroon en hoe u dit product gebruiken met een Azure logica-app."
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-recurrence-trigger"></a>Aan de slag met de trigger terugkeerpatroon

Met behulp van het terugkeerpatroon-trigger, kunt u krachtige werkstromen maken in de cloud.

U kunt bijvoorbeeld het volgende doen:

- Een werkstroom voor het uitvoeren van een opgeslagen SQL-procedure elke dag plannen.
- Een overzicht van alle tweets e-mail in de laatste week over een bepaalde hashtag.

Als u wilt beginnen met het terugkeerpatroon-trigger in een app logica, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Gebruik een terugkeerpatroon-trigger

Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een app logica te starten. [Meer informatie over activering](connectors-overview.md).

Hier ziet u de volgorde van een voorbeeld van het instellen van een trigger terugkeerpatroon in een app logica:

1. De trigger **Terugkeerpatroon** als de eerste stap in een app logica toevoegen.
2. Vul de parameters in voor het terugkeerpatroon.

De logica-app wordt nu een uitvoeren na elke tijdsinterval gestart.

![HTTP-trigger](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Trigger details

De trigger terugkeerpatroon heeft de volgende eigenschappen die u kunt configureren.

Deze gebeurtenis wordt gestart een app logica na een bepaald tijdsinterval.
A * houdt in dat een vereist veld.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|Frequentie *|frequentie|The unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.|
|Interval *|interval|Het interval van de opgegeven interval voor het terugkeerpatroon.|
|Tijdzone|Tijdzone|Als een begintijd wordt geleverd zonder een UTC-tijd, kunt u deze tijdzone wordt gebruikt.|
|Begintijd|Starttijd|De begintijd in [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).|
<br>


## <a name="next-steps"></a>Volgende stappen

Probeer nu het platform en [een logica-app maakt](../app-service-logic/app-service-logic-create-a-logic-app.md). Door te kijken onze [lijst API's](apis-list.md)vindt u de andere beschikbare connectors in logica-apps.
