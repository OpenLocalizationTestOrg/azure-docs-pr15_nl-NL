<properties
    pageTitle="Een vertraging in logica apps toevoegen | Microsoft Azure"
    description="Overzicht van de vertraging en vertraging-totdat de acties en hoe u deze met een Azure logica-app."
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

# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Aan de slag met de vertraging en vertraging-totdat acties

Met behulp van de vertraging en "vertraging-totdat" acties, kunt u de werkstroom scenario's uitvoeren.

U kunt bijvoorbeeld het volgende doen:

- Wacht totdat een weekdag een taakstatus verzenden via e-mail.
- De werkstroom uitstellen totdat een HTTP-oproep tijd om te voltooien voordat u hervatten en het terughalen van het resultaat heeft.

Als u wilt beginnen met de actie vertraging in een app logica, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-delay-actions"></a>De vertraging acties gebruiken

Een actie is een bewerking die wordt uitgevoerd door de werkstroom die is gedefinieerd in een app logica. [Meer informatie over acties](connectors-overview.md).

Hier ziet u de volgorde van een voorbeeld van het gebruik van een stap vertraging in een app logica:

1. Na het toevoegen van een trigger, klikt u op **Nieuwe stap** als een actie wilt toevoegen.
2. Zoek naar **vertraging** om de vertraging acties weer te geven. In dit voorbeeld selecteren we **vertraging**.

    ![Vertraging acties](./media/connectors-native-delay/using-action-1.png)

3. Voer een van de eigenschappen van de actie voor het configureren van de vertraging.

    ![Vertraging zoekconfiguratie](./media/connectors-native-delay/using-action-2.png)

4. Klik op **Opslaan** als u wilt publiceren en de logica app activeren.


## <a name="action-details"></a>Actiedetails

De trigger terugkeerpatroon heeft de volgende eigenschappen die kunnen worden geconfigureerd.

### <a name="delay-action"></a>Vertraging actie

Deze actie vertraagd de uitvoeren voor een bepaalde periode.
A * houdt in dat een vereist veld.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|Aantal *|tellen|Het aantal tijdseenheden wachttijd|
|Eenheid *|eenheid|De tijdseenheid: `Second`, `Minute`, `Hour`, of`Day`|
<br>

### <a name="delay-until-action"></a>Vertraging-tot actie

Deze actie vertraagd de uitvoeren tot een opgegeven datum/tijd.
A * houdt in dat een vereist veld.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|Jaar *|tijdstempel|De jaar-tot-uitstellen tot (GMT)|
|Maand *|tijdstempel|De maand tot uitstellen tot (GMT)|
|Dag *|tijdstempel|De dag wachttijd tot (GMT)|
<br>


## <a name="next-steps"></a>Volgende stappen

Probeer nu het platform en [een logica-app maakt](../app-service-logic/app-service-logic-create-a-logic-app.md). Door te kijken onze [lijst API's](apis-list.md)vindt u de andere beschikbare connectors in logica-apps.
