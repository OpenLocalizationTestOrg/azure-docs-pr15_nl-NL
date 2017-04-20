<properties
   pageTitle="Logica apps als aanroepbare eindpunten"
   description="Het maken en configureren eindpunten activeren en gebruiken in een app logica in Azure App-Service"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>


# <a name="logic-apps-as-callable-endpoints"></a>Logica apps als aanroepbare eindpunten

Logica Apps kan een synchroon HTTP-eindpunt native blootgesteld als een trigger.  U kunt ook het patroon van aanroepbare eindpunten gebruiken om aan te roepen logica Apps als een geneste werkstroom tot en met de actie 'werkstroom' in een App logica.

Er zijn 3 soorten triggers die aanvragen kunnen ontvangen:

* Aanvragen
* ApiConnectionWebhook
* HttpWebhook

Voor de rest van het artikel, we **verzoek** gebruiken als in het voorbeeld, maar alle van de beginselen identiek toepassen op de andere triggers 2 typen.

## <a name="adding-a-trigger-to-your-definition"></a>Een trigger toevoegen aan de definitie van uw
De eerste stap is een trigger toevoegen aan de definitie van uw logica-app die verzoeken voor oproepen kan ontvangen.  U kunt zoeken in de ontwerpfunctie voor "HTTP-aanvraag' de trigger-kaart wilt toevoegen. U kunt de hoofdtekst van een aanvraag JSON Schema definiëren en de ontwerpfunctie tokens waarmee u parseert en doorgeven van gegevens uit de handmatige trigger via de werkstroom wordt gegenereerd.  Ik kan een hulpmiddel zoals [jsonschema.net](http://jsonschema.net) gebruiken om te genereren van een schema JSON van een steekproef hoofdtekst nettolading aanbevelen.

![Trigger kaart aanvragen][2]

Nadat u de definitie van uw App logica opslaat, wordt de URL van een terugbellen lijkt op deze worden gegenereerd:
 
``` text
https://prod-03.eastus.logic.azure.com:443/workflows/080cb66c52ea4e9cabe0abf4e197deff/triggers/myendpointtrigger?*signature*...
```

Deze URL bestaat uit een sleutel SA's in de queryparameters gebruikt voor verificatie.

U kunt dit eindpunt ook openen in de portal van Azure:

![][1]

Of, door te bellen:

``` text
POST https://management.azure.com/{resourceID of your logic app}/triggers/myendpointtrigger/listCallbackURL?api-version=2015-08-01-preview
```

### <a name="security-for-the-trigger-url"></a>Beveiliging voor de URL van de trigger

Logica App terugbellen URL's worden gegenereerd veilig met een Access gedeeld handtekening.  De handtekening wordt doorgegeven als queryparameter en moet worden gevalideerd voordat de logica-app wordt uitgevoerd.  Het wordt gegenereerd door een unieke combinatie van een geheime sleutel per app logica, de naam van de inwerkingtreding en de bewerking wordt uitgevoerd.  Als iemand toegang tot de geheime logica app-toets heeft, zou ze zijn niet beschikbaar om een geldige handtekening te genereren.

## <a name="calling-the-logic-app-triggers-endpoint"></a>De logica app-trigger eindpunt bellen

Nadat u het eindpunt voor uw trigger hebt gemaakt, kunt u het activeren via een `POST` naar de volledige URL. U kunt extra kopteksten en alle inhoud opnemen in de hoofdtekst.

Als het inhoudstype is `application/json` en vervolgens is mogelijk om te verwijzen naar de eigenschappen van binnen het verzoek. Deze worden, anders behandeld als een binaire eenheid die kan worden doorgegeven aan andere API's, maar kan niet worden verwezen in de werkstroom zonder te converteren van de inhoud.  Als u bijvoorbeeld `application/xml` inhoud kunt u `@xpath()` moet een extractie xpath of `@json()` omzetten van XML-JSON.  Meer informatie over het werken met inhoud typt [kan worden gevonden hier](app-service-logic-content-type.md)

Bovendien kunt u een schema JSON opgeven in de definitie. Hierdoor wordt de ontwerpfunctie voor het genereren van tokens die u vervolgens in stappen doorgeven kunt.  Bijvoorbeeld de volgende handelingen uit, waarmee u een `title` en `name` token beschikbaar in de ontwerpfunctie voor:

```
{
    "properties":{
        "title": {
            "type": "string"
        },
        "name": {
            "type": "string"
        }
    },
    "required": [
        "title",
        "name"
    ],
    "type": "object"
}
```

## <a name="referencing-the-content-of-the-incoming-request"></a>Verwijst naar de inhoud van de binnenkomende aanvraag

De `@triggerOutputs()` functie wordt de inhoud van de binnenkomende aanvraag uitvoer. Deze er bijvoorbeeld als:

```
{
    "headers" : {
        "content-type" : "application/json"
    },
    "body" : {
        "myprop" : "a value"
    }
}
```

U kunt de `@triggerBody()` snelkoppeling voor toegang tot de `body` eigenschap specifiek. 

## <a name="responding-to-the-request"></a>Reageren op verzoek

Voor bepaalde aanvragen die een logica-app starten, is het raadzaam om te reageren met bepaalde inhoud op de beller. Er is een nieuwe actietype genoemd **antwoord** dat kan worden gebruikt om de statuscode, de hoofdtekst en de kopteksten voor uw antwoord te maken. Opmerking dat als er geen shape **antwoord** aanwezig is, het eindpunt van de app logica wordt *onmiddellijk* beantwoorden met **202 geaccepteerde**.

![HTTP-antwoord actie][3]

``` json
"Response": {
            "conditions": [],
            "inputs": {
                "body": {
                    "name": "@{triggerBody()['name']}",
                    "title": "@{triggerBody()['title']}"
                },
                "headers": {
                    "content-type": "application/json"
                },
                "statusCode": 200
            },
            "type": "Response"
        }
```

Antwoorden hebt het volgende:

| Eigenschap | Beschrijving |
| -------- | ----------- |
| statusCode | De HTTP-statuscode reageren op binnenkomende verzoek. Elke geldige statuscode die met 2xx, 4xx of 5xx begint u kunt werken. 3xx statuscodes zijn niet toegestaan. | 
| hoofdtekst | Een object hoofdtekst die een tekenreeks, een JSON-object of zelfs binaire inhoud waarnaar wordt verwezen vanuit een vorige stap. | 
| kopteksten | U kunt een willekeurig aantal kopteksten moeten worden opgenomen in de reactie definiëren | 

Alle stappen in de app logica die vereist voor het antwoord zijn moet uitvoeren binnen *de 60 seconden* voor de oorspronkelijke aanvraag voor het ontvangen van het antwoord **tenzij de werkstroom wordt aangeroepen als een geneste logica-App**. Als u geen actie antwoord is bereikt binnen de 60 seconden en vervolgens de binnenkomende aanvraag wordt time-out en ontvangt een antwoord **408 Client time-out** HTTP.  Voor geneste logica-Apps, de bovenliggende logica App blijft moet wachten om een antwoord totdat voltooid, ongeacht hoe lang duurt.

## <a name="advanced-endpoint-configuration"></a>Geavanceerde eindpuntconfiguratie

Logica-apps hebt gemaakt in ondersteuning voor het eindpunt directe toegang en gebruik altijd de `POST` methode een uitvoeren van de logica-app starten. De app **HTTP luisteraar ervan af** API eerder ook ondersteund wijzigen van de segmenten URL en de HTTP-methode. U kunt zelfs instellen extra beveiliging of een aangepast domein toevoegen aan de host van API-app (de Web-app die de API-app gehost). 

Deze functionaliteit is beschikbaar via de **API management**:
* [De methode van de aanvraag wijzigen](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod)
* [De URL-segmenten van het verzoek wijzigen](https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL)
* Uw API management domeinen op het tabblad **configureren** in de klassieke Azure-portal instellen
* Beleid instellen om te controleren voor basisverificatie (**koppeling nodig**)

## <a name="summary-of-migration-from-2014-12-01-preview"></a>Overzicht van de migratie van 2014-12-01-voorbeeld

|  2014-12-01-voorbeeld | 2016-01-06 |
|---------------------|--------------------|
| Klik op **Http-luisteraar ervan af** API-app | Klik op **handmatig trigger** (geen API-app vereist) |
| HTTP luisteraar ervan af instellen "*stuurt de reactie automatisch*" | Een actie van een **antwoord** opnemen of niet in de werkstroomdefinitie |
| Basic of OAuth verificatie configureren | via beheer van de API |
| HTTP-methode configureren | via beheer van de API |
| Relatieve pad configureren | via beheer van de API |
| Overzicht van de binnenkomende hoofdtekst via`@triggerOutputs().body.Content` | Verwijzing via`@triggerOutputs().body` |
| De actie **verzenden HTTP-antwoord** op de HTTP luisteraar ervan af | Klik op het **reageren op HTTP-aanvraag** (geen API-app vereist)


[1]: ./media/app-service-logic-http-endpoint/manualtriggerurl.png
[2]: ./media/app-service-logic-http-endpoint/manualtrigger.png
[3]: ./media/app-service-logic-http-endpoint/response.png
