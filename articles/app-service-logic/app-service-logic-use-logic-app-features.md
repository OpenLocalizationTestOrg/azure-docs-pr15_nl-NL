<properties 
    pageTitle="Het gebruik van functies van de logica-App | Microsoft Azure" 
    description="Informatie over het gebruik van geavanceerde functies van de logica apps." 
    authors="stepsic-microsoft-com" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/28/2016"
    ms.author="stepsic"/> 
    
# <a name="use-logic-apps-features"></a>Gebruik functies logica Apps

In het [vorige onderwerp](app-service-logic-create-a-logic-app.md), moet u uw eerste logica app gemaakt. Nu wordt we uitgelegd hoe u een gedetailleerde proces met App Services logica Apps maken. In dit onderwerp maakt u kennis met de volgende nieuwe logica Apps begrippen:

- Voorwaardelijke logica, waarmee een actie alleen wanneer een bepaalde voorwaarde is voldaan.
- Codeweergave bewerken van een bestaande logica-app.
- Opties voor het starten van een werkstroom.

Voordat u dit onderwerp uitvoert, moet u de stappen in [een nieuwe logica-app maken](app-service-logic-create-a-logic-app.md). Klik in de [portal van Azure]bladert u naar uw app logica en **Triggers en acties** op in het overzicht de definitie van de app logica bewerken.

## <a name="reference-material"></a>Referentiemateriaal

U mogelijk de volgende documenten nuttig vinden:

- [Management en runtime REST API's](https://msdn.microsoft.com/library/azure/mt643787.aspx) - waaronder over het rechtstreeks logica apps roepen
- [Naslaggids voor](https://msdn.microsoft.com/library/azure/mt643789.aspx) -een uitgebreide lijst met alle ondersteunde functies expressies
- [Typen trigger en actie](https://msdn.microsoft.com/library/azure/mt643939.aspx) - de verschillende soorten acties en de invoer die genomen worden
- [Overzicht van de Service van de App](../app-service/app-service-value-prop-what-is.md) - beschrijving van welke onderdelen om te kiezen wanneer u een oplossing bouwen

## <a name="adding-conditional-logic"></a>Voorwaardelijke logica toevoegen

Hoewel de oorspronkelijke stroom werkt, zijn er enkele gebieden die kunnen worden verbeterd. 


### <a name="conditional"></a>Voorwaardelijke
Deze app logica kan dit leiden tot u een groot aantal e-mailberichten aan. De volgende stappen toevoegen logica om ervoor te zorgen dat u een e-mailbericht alleen ontvangt wanneer de tweet afkomstig van iemand met een bepaald aantal Volgers is. 

1. Klik op het plusteken en Ga naar de actie *Gebruiker ophalen* voor Twitter.

2. In het veld **Tweeted door** doorgeven van de trigger om de gegevens over de Twitter-gebruiker.

    ![Gebruikers krijgen](./media/app-service-logic-use-logic-app-features/getuser.png)

3. Klikt u nogmaals op het plusteken, maar deze keer selecteert **Voorwaarde toevoegen**

4. Klik in het eerste vak op de **...** onder **Gebruiker krijgen** om het veld **Volgers tellen** te zoeken.

5. In de vervolgkeuzelijst, selecteert u **groter dan**

6. Typ in het tweede vak het aantal Volgers die u wilt dat gebruikers hebt.

    ![Voorwaardelijke](./media/app-service-logic-use-logic-app-features/conditional.png)

7.  Tot slot vak slepen en neerzetten het e-mailbericht in het vak **Indien Ja** . Dit betekent dat u krijgt alleen e-mailberichten wanneer het aantal pc-gebruiker is voldaan.

## <a name="repeating-over-a-list-with-foreach"></a>Herhalende over een lijst met forEach

De lus forEach geeft een matrix als u wilt herhalen via. Als deze niet een matrix wordt de stroom mislukt. Als u bijvoorbeeld als u action1 die uitvoer van een matrix van berichten hebt en u wilt verzenden van elk bericht, kunt u opnemen deze forEach-instructie in de eigenschappen van de actie: forEach:"@action('action1').outputs.messages"
 

## <a name="using-the-code-view-to-edit-a-logic-app"></a>Gebruik van de codeweergave bewerken van een App logica

U kunt de code die wordt als volgt gedefinieerd een logica-app rechtstreeks bewerken naast de ontwerpfunctie. 

1. Klik op de knop **codeweergave** in de opdrachtenbalk. 

    Hiermee opent u een volledige editor waarin de definitie die u zojuist hebt bewerkt.

    ![Codeweergave](./media/app-service-logic-use-logic-app-features/codeview.png)

    Met de teksteditor, kunt u kopiëren en plakken van een willekeurig aantal acties binnen de dezelfde logica-app of tussen logica apps. U kunt ook eenvoudig toevoegen of hele secties verwijderen uit de definitie en u kunt ook definities delen met anderen.

2. Nadat u uw wijzigingen in de codeweergave aanbrengt, klik op **Opslaan**. 

### <a name="parameters"></a>Parameters
Er zijn enkele mogelijkheden van logica-Apps die kan alleen worden gebruikt in de codeweergave. Een voorbeeld van de volgende is parameters. Parameters vergemakkelijkt waarden overal in uw app logica opnieuw gebruiken. Bijvoorbeeld als u een e-mailadres dat u gebruiken in diverse acties wilt hebt, moet u definiëren deze als een parameter.

De volgende bijgewerkt via uw bestaande logica-app te parameters gebruiken voor de queryterm.

1. Zoek in de codeweergave, de `parameters : {}` object en het volgende onderwerp object invoegen:

        "topic" : {
            "type" : "string",
            "defaultValue" : "MicrosoftAzure"
        }
    
2. Schuif naar de `twitterconnector` actie, de querywaarde zoeken en vervangen met `#@{parameters('topic')}`.
    U kunt ook de functie **samenvoegen** gebruiken om samen te voegen tekenreeksen met twee of meer, bijvoorbeeld: `@concat('#',parameters('topic'))` is gelijk aan het bovenstaande. 
 
Parameters zijn een goede manier om de waarden die u waarschijnlijk veel veranderen uitlichten. Ze zijn met name nuttig wanneer u moet overschrijven parameters in verschillende omgevingen. Zie voor meer informatie over de parameters op basis van de omgeving overschrijven onze [REST API-documentatie](https://msdn.microsoft.com/library/mt643787.aspx).

Nu, als u op **Opslaan**hebt geklikt, per uur krijgt u een nieuwe tweets die u meer dan 5 retweets afgeleverd in een map genaamd **tweets** in uw Dropbox hebt.

Meer informatie over de logica App definities, Zie [logica App definities van de auteur](app-service-logic-author-definitions.md).

## <a name="starting-a-logic-app-workflow"></a>Een werkstroom logica-app starten
Er zijn verschillende opties voor het starten van de werkstroom die is gedefinieerd in u logica app. Een werkstroom kan altijd worden gestart op aanvraag in de [portal van Azure].

### <a name="recurrence-triggers"></a>Terugkeerpatroon triggers
Een terugkeerpatroon-trigger wordt uitgevoerd met een tijdsinterval die u opgeeft. Wanneer de trigger voorwaardelijke logica bevat, bepaalt de trigger al dan niet de werkstroom moet worden uitgevoerd. Een trigger geeft aan dat deze moet worden uitgevoerd door te retourneren een `200` statuscode. Wanneer deze niet nodig heeft om uit te voeren, wordt een `202` statuscode.

### <a name="callback-using-rest-apis"></a>Terugbellen met REST API 's
Services kunnen bellen een eindpunt van de app logica om een werkstroom starten. Zie [logica apps als aanroepbare eindpunten](app-service-logic-connector-http.md) voor meer informatie. Als u wilt dat soort logica app op de aanvraag, klikt u op de knop **nu uitvoeren** op de balk met opdrachten. 

<!-- Shared links -->
[Azure-portal]: https://portal.azure.com 