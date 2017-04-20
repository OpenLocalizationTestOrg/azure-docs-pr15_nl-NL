<properties
    pageTitle="Vergaderverzoeken en antwoorden acties gebruiken | Microsoft Azure"
    description="Overzicht van de vergaderverzoeken en antwoorden inwerkingtreding en de actie in een Azure logica-app"
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

# <a name="get-started-with-the-request-and-response-components"></a>Aan de slag met de onderdelen vergaderverzoeken en antwoorden

Met de vergaderverzoeken en antwoorden onderdelen in een app logica, kunt u in realtime reageren op gebeurtenissen.

U kunt bijvoorbeeld het volgende doen:

- Reageren op een HTTP-aanvraag met gegevens van een on-premises implementatie-database via een logica-app.
- Een app logica uit een externe webhook gebeurtenis activeren.
- Een app logica met een actie vergaderverzoeken en antwoorden uit in een andere logica app belt.

Als u wilt beginnen met de acties vergaderverzoeken en antwoorden in een app logica, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-request-trigger"></a>Gebruik de HTTP-aanvraag-trigger

Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een app logica te starten. [Meer informatie over activering](connectors-overview.md).

Hier ziet u de volgorde van een voorbeeld van het instellen van een HTTP-aanvraag in de ontwerpfunctie voor logica-App.

1. De trigger **aanvraag - wanneer een HTTP-aanvraag wordt ontvangen** in uw app logica toevoegen. U kunt desgewenst een schema JSON (met behulp van een hulpmiddel zoals [JSONSchema.net](http://jsonschema.net)) voor het hoofdgedeelte van de aanvraag. Hiermee kunt de ontwerpfunctie voor het genereren van tokens voor eigenschappen in het HTTP-verzoek.
2. Een andere actie toevoegen, zodat u kunt de logica-app opslaan.
3. Na het opslaan van de logica-app, kunt u de URL van de aanvraag HTTP krijgen van de kaart.
4. Een HTTP POST (u kunt een hulpmiddel zoals [Postman](https://www.getpostman.com/)) naar de URL de gebeurtenis de logica-app.

>[AZURE.NOTE] Als u niet de actie van een antwoord, definieert een `202 ACCEPTED` antwoord direct naar de beller wordt geretourneerd. U kunt de actie antwoord een reactie aanpassen.

![Antwoord-trigger](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a>Gebruik de actie HTTP-antwoord

De HTTP-antwoord actie is alleen geldig wanneer u deze gebruikt in een werkstroom die door een HTTP-aanvraag is geactiveerd. Als u niet de actie van een antwoord, definieert een `202 ACCEPTED` antwoord direct naar de beller wordt geretourneerd.  U kunt de actie van een antwoord bij elke stap in de werkstroom toevoegen. De app logica alleen zorgt ervoor dat de binnenkomende aanvraag openen voor een minuut voor een reactie.  Na een minuut, als er geen antwoord van de werkstroom is verzonden (en een actie antwoord in de definitie bestaat), een `504 GATEWAY TIMEOUT` is geretourneerd naar de beller.

Hier volgt een HTTP-antwoord actie toevoegen:

1. Selecteer de knop **Nieuwe stap** .
2. Kies **een actie toevoegen**.
3. Typ in het zoekvak actie **antwoord** u kunt de actie antwoord.

    ![Selecteer de actie antwoord](./media/connectors-native-reqres/using-action-1.png)

4. In alle parameters die vereist voor het antwoordbericht HTTP zijn toevoegen.

    ![De actie antwoord voltooien](./media/connectors-native-reqres/using-action-2.png)

5. Klik op de linkerbovenhoek van de werkbalk om op te slaan en de logica-app wordt zowel opslaan en publiceren (activeren).

## <a name="request-trigger"></a>Trigger aanvragen

Hier vindt u de details in voor de trigger die ondersteuning biedt voor deze verbindingslijn. Er is een enkele aanvraag-trigger.

|Trigger|Beschrijving|
|---|---|
|Aanvragen|Deze gebeurtenis vindt plaats wanneer een HTTP-aanvraag wordt ontvangen|

## <a name="response-action"></a>Antwoord actie

Hier vindt u de details in voor de actie die ondersteuning biedt voor deze verbindingslijn. Er is een actie één antwoord dat alleen kan worden gebruikt wanneer deze wordt geleverd met een verzoek-trigger.

|Actie|Beschrijving|
|---|---|
|Antwoord|Geeft als resultaat een antwoord op de gecorreleerde HTTP-aanvraag|

### <a name="trigger-and-action-details"></a>Details van inwerkingtreding en een actie

De volgende tabellen beschrijven de invoervelden voor de inwerkingtreding en de actie en de bijbehorende details weergeven.

#### <a name="request-trigger"></a>Trigger aanvragen
Hierna volgt een invoer veld voor de trigger uit een binnenkomende HTTP-aanvraag.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|JSON Schema|schema|Het schema JSON van het hoofdgedeelte van de aanvraag|
<br>

**Gegevens voor uitvoer**

Hier volgen uitvoerdetails voor het verzoek.

|Naam van eigenschap|Gegevenstype|Beschrijving|
|---|---|---|
|Kopteksten|object|Kopteksten aanvragen|
|Hoofdtekst|object|Object aanvragen|

#### <a name="response-action"></a>Antwoord actie

Hier volgen invoervelden voor de HTTP-antwoord actie. A * houdt in dat een vereist veld.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|Status Code *|statusCode|De HTTP-statuscode|
|Kopteksten|kopteksten|Een object JSON van eventuele antwoordheaders om op te nemen|
|Hoofdtekst|hoofdtekst|De hoofdtekst van het antwoord|

## <a name="next-steps"></a>Volgende stappen

Probeer nu het platform en [een logica-app maakt](../app-service-logic/app-service-logic-create-a-logic-app.md). Door te kijken onze [lijst API's](apis-list.md)vindt u de andere beschikbare connectors in logica-apps.
