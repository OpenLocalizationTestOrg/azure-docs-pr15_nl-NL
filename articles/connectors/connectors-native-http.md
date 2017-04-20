<properties
    pageTitle="De actie HTTP in logica apps toevoegen | Microsoft Azure"
    description="Overzicht van de actie HTTP met eigenschappen"
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
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http-action"></a>Aan de slag met de HTTP-actie

U kunt met de actie HTTP, werkstromen voor uw organisatie uitbreiden en naar een eindpunt communiceren via HTTP.

U kunt:

- Hiermee maakt u logica app werkstromen die (trigger) activeren wanneer een website die u beheert uitvalt.
- Een eindpunt delen via HTTP voor het uitbreiden van uw werkstromen in andere services.

Als u wilt beginnen met de actie HTTP in een app logica, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-trigger"></a>Gebruik de HTTP-trigger

Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een app logica te starten. [Meer informatie over activering](connectors-overview.md).

Hier ziet u de volgorde van een voorbeeld van het instellen van de HTTP-trigger in de ontwerpfunctie voor logica-App.

1. De HTTP-trigger in uw app logica toevoegen.
2. Vul de parameters voor het HTTP-eindpunt dat u wilt controleren.
3. Wijzig het interval voor het terugkeerpatroon op hoe vaak deze moet controleren.
4. De logica-app wordt nu uitgevoerd met de inhoud die wordt geretourneerd tijdens elke controle.

![HTTP-trigger](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a>De werking van de HTTP-trigger

De HTTP-trigger belt in bij een HTTP-eindpunt op een terugkerend interval. Geen antwoord HTTP code standaard minder dan 300 resultaten in een logica-app uitvoeren. U kunt een voorwaarde toevoegen in de weergave van een code die wordt geëvalueerd na de HTTP-oproep om te bepalen als de logica-app moet worden geactiveerd. Hier volgt een voorbeeld van een HTTP-trigger die wordt uitgevoerd wanneer de geretourneerde statuscode groter is dan of gelijk is aan `400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

Uitgebreide informatie over de HTTP-trigger parameters zijn beschikbaar op [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).

## <a name="use-the-http-action"></a>Gebruik de HTTP-actie

Een actie is een bewerking die wordt uitgevoerd door de werkstroom die is gedefinieerd in een app logica. [Meer informatie over acties](connectors-overview.md).

1. Selecteer de knop **Nieuwe stap** .
2. Kies **een actie toevoegen**.
3. Typ in het zoekvak actie **http** u kunt de HTTP-actie.

    ![Selecteer de HTTP-actie](./media/connectors-native-http/using-action-1.png)

4. In alle parameters die vereist voor de HTTP-oproep zijn toevoegen.

    ![De actie HTTP voltooien](./media/connectors-native-http/using-action-2.png)

5. Klik op de linkerbovenhoek van de werkbalk om op te slaan. Uw app logica wordt zowel opslaan en publiceren (activeren).

## <a name="http-trigger"></a>HTTP-trigger

Hier vindt u de details in voor de trigger die ondersteuning biedt voor deze verbindingslijn. De verbindingslijn HTTP heeft één trigger.

|Trigger|Beschrijving|
|---|---|
|HTTP|Een HTTP-gesprek wordt en geeft als resultaat de inhoud van de reactie.|

## <a name="http-action"></a>HTTP-actie

Hier vindt u de details in voor de actie die ondersteuning biedt voor deze verbindingslijn. De verbindingslijn HTTP heeft een mogelijke actie.

|Actie|Beschrijving|
|---|---|
|HTTP|Een HTTP-gesprek wordt en geeft als resultaat de inhoud van de reactie.|

## <a name="http-details"></a>HTTP-details

De volgende tabellen worden de vereiste en optionele invoervelden voor de actie en de bijbehorende uitvoerdetails die zijn gekoppeld aan met de actie beschreven.


#### <a name="http-request"></a>HTTP-aanvraag
Hier volgen invoervelden voor de actie, waardoor een uitgaande HTTP-verzoek.
A * houdt in dat een vereist veld.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|Methode *|methode|De HTTP-woord gebruiken|
|URI *|URI|De URI voor de HTTP-aanvraag|
|Kopteksten|kopteksten|Een object JSON HTTP-headers om op te nemen|
|Hoofdtekst|hoofdtekst|Het hoofdgedeelte van de aanvraag|
|Verificatie|verificatie|Gegevens in de sectie [verificatie](#authentication)|
<br>

#### <a name="output-details"></a>Gegevens voor uitvoer

Hier volgen uitvoerdetails voor het HTTP-antwoord.

|Naam van eigenschap|Gegevenstype|Beschrijving|
|---|---|---|
|Kopteksten|object|Antwoord kopteksten|
|Hoofdtekst|object|Antwoordobject|
|Statuscode|int|HTTP-statuscode|

## <a name="authentication"></a>Verificatie

De functie logica Apps van Azure-Service voor App kunt u het gebruik van verschillende soorten verificatie HTTP-eindpunten. U kunt deze verificatie gebruiken met de verbindingslijnen **HTTP**, **[HTTP + Swagger](./connectors-native-http-swagger.md)**en **[HTTP Webhook](./connectors-native-webhook.md)** . De volgende soorten verificatie worden geconfigureerd:

* [Basisverificatie](#basic-authentication)
* [Verificatie via clientcertificaat](#client-certificate-authentication)
* [Azure Active Directory (Azure AD) OAuth-verificatie](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Basisverificatie

Het volgende object voor de verificatie is vereist voor basisverificatie.
A * houdt in dat een vereist veld.

|Naam van eigenschap|Gegevenstype|Beschrijving|
|---|---|---|
|Type *|type|Type verificatie (moet `Basic` voor basisverificatie)|
|Gebruikersnaam *|gebruikersnaam|Gebruikersnaam in te voeren om te verifiëren|
|Wachtwoord *|wachtwoord|Wachtwoord om te verifiëren|

>[AZURE.TIP] Als u wilt een wachtwoord gebruiken dat kan niet worden opgehaald uit de definitie, gebruik een `securestring` parameter en de `@parameters()` [werkstroom definitie functie](http://aka.ms/logicappdocs).

Zo maakt u een object als volgt in het veld verificatie:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Verificatie via clientcertificaat

Het volgende object voor de verificatie is vereist voor verificatie via clientcertificaat. A * houdt in dat een vereist veld.

|Naam van eigenschap|Gegevenstype|Beschrijving|
|---|---|---|
|Type *|type|Het type verificatie (moet `ClientCertificate` voor SSL-certificaten voor client)|
|PFX *|PFX|De inhoud Base64-codering van het bestand persoonlijke informatie Exchange (PFX)|
|Wachtwoord *|wachtwoord|Het wachtwoord voor toegang tot het PFX-bestand|

>[AZURE.TIP] U kunt een `securestring` parameter en de `@parameters()` [werkstroom definitie functie](http://aka.ms/logicappdocs) gebruik van een parameter weer die niet in de definitie na het opslaan van de app logica leesbaar.

Bijvoorbeeld:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Azure AD OAuth-verificatie

Het volgende object voor de verificatie is vereist voor Azure AD OAuth-verificatie. A * houdt in dat een vereist veld.

|Naam van eigenschap|Gegevenstype|Beschrijving|
|---|---|---|
|Type *|type|Het type verificatie (moet `ActiveDirectoryOAuth` voor Azure AD OAuth)|
|Tenant *|tenant|De tenant-id voor de Azure AD-tenant|
|Publiek *|publiek|Stel op`https://management.core.windows.net/`|
|Client -ID *|clientId|De client-id voor de Azure AD-toepassing|
|Geheim *|geheim|Het geheim van de client die het token aanvraagt|

>[AZURE.TIP] U kunt een `securestring` parameter en de `@parameters()` [werkstroom definitie functie](http://aka.ms/logicappdocs) gebruik van een parameter weer die niet leesbaar in de definitie na het opslaan.

Bijvoorbeeld:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Volgende stappen

Probeer nu het platform en [een logica-app maakt](../app-service-logic/app-service-logic-create-a-logic-app.md). Door te kijken onze [lijst API's](apis-list.md)vindt u de andere beschikbare connectors in logica-Apps.
