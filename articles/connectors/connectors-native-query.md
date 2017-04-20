<properties
    pageTitle="De actie OpenQuery in logica apps toevoegen | Microsoft Azure"
    description="Overzicht van de queryactie voor het uitvoeren van acties bijgehouden, zoals filter matrix."
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
   ms.date="07/20/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-query-action"></a>Aan de slag met de actie OpenQuery

Met de queryactie, kunt u werken met batches en matrices voor het uitvoeren van werkstromen aan:

- Een taak voor alle records van hoge prioriteit voor het maken van een database.
- Alle PDF-bijlagen voor e-mailberichten naar een Azure blob opslaan.

Als u wilt beginnen met de queryactie in een app logica, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-query-action"></a>De queryactie gebruiken

Een actie is een bewerking die wordt uitgevoerd door de werkstroom die is gedefinieerd in een app logica. [Meer informatie over acties](connectors-overview.md).  

De queryactie heeft momenteel één bewerking, de matrix filter, dat wordt weergegeven in de ontwerpfunctie voor genoemd. Hiermee kunt u een matrix query en een reeks gefilterde resultaten terug te keren.

Hier ziet u hoe u deze in een app logica kunt toevoegen:

1. Selecteer de knop **Nieuwe stap** .
2. Kies **een actie toevoegen**.
3. Typ in het zoekvak actie **filter** om de actie **Filter matrix** .

    ![Selecteer de actie OpenQuery](./media/connectors-native-query/using-action-1.png)

4. Selecteer een matrix als u wilt filteren. (De volgende schermafbeelding ziet u de matrix van resultaten van een zoekopdracht Twitter.)
5. Maak een voorwaarde voor elk item wordt berekend. (De volgende schermafbeelding filters tweets van gebruikers die meer dan 100 Volgers hebben).

    ![De queryactie voltooien](./media/connectors-native-query/using-action-2.png)

    De actie wordt uitvoer van een nieuwe matrix met alleen resultaten die voldoet aan de vereisten van het filter.
6. Klik op de linkerbovenhoek van de werkbalk om op te slaan en de logica-app wordt zowel opslaan en publiceren (activeren).

## <a name="query-action"></a>-Actie OpenQuery

Hier vindt u de details in voor de actie die ondersteuning biedt voor deze verbindingslijn. De verbindingslijn heeft een mogelijke actie.

|Actie|Beschrijving|
|---|---|
|Matrix filteren|Evalueert een voorwaarde voor elk item in een matrix en geeft als resultaat de resultaten|

## <a name="action-details"></a>Actiedetails

De queryactie wordt geleverd voor een mogelijke actie. De volgende tabellen worden de vereiste en optionele invoervelden voor de actie en de bijbehorende uitvoerdetails die zijn gekoppeld aan met de actie beschreven.

### <a name="filter-array"></a>Matrix filteren
Hier volgen invoervelden voor de actie, waardoor een uitgaande HTTP-verzoek.
A * houdt in dat een vereist veld.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|Uit *|Van|De matrix om te filteren|
|Voorwaarde *|waar|De voorwaarde voor elk item wordt berekend|
<br>

### <a name="output-details"></a>Gegevens voor uitvoer

Hier volgen uitvoerdetails voor het HTTP-antwoord.

|Naam van eigenschap|Gegevenstype|Beschrijving|
|---|---|---|
|Gefilterde matrix|matrix|Een matrix met een object voor elk gefilterde resultaat|

## <a name="next-steps"></a>Volgende stappen

Probeer nu het platform en [een logica-app maakt](../app-service-logic/app-service-logic-create-a-logic-app.md). Door te kijken onze [lijst API's](apis-list.md)vindt u de andere beschikbare connectors in logica-apps.
