<properties
   pageTitle="Logica apps inhoud Typ afhandeling | Microsoft Azure"
   description="Meer informatie over hoe logica Apps omgaat met inhoudstypen op ontwerp en runtime"
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
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-content-type-handling"></a>Logica Apps inhoudstype afhandeling

Er zijn verschillende soorten inhoud die door een App logica - inclusief JSON, XML-bestanden en binaire gegevens kan stromen.  Tijdens het alle inhoudstypen worden ondersteund, enkele native worden begrepen door de logica Apps-Engine en anderen kunnen vereisen toewijzing of conversies naar wens.  Het volgende artikel wordt beschreven hoe de engine omgaat met verschillende inhoudstypen en hoe ze kunnen worden correct verwerkt naar wens.

## <a name="content-type-header"></a>Koptekst van inhoudstype

Als u wilt beginnen eenvoudige, eens kijken naar de twee `Content-Types` die geen nodig eventuele conversie of een toewijzing gebruik binnen een App logica - `application/json` en `text/plain`.

### <a name="applicationjson"></a>Toepassing/json

De werkstroom-engine is afhankelijk van de `Content-Type` van de koptekst van HTTP waarmee de juiste afhandeling belt.  Een aanvraag met het inhoudstype `application/json` worden opgeslagen en verwerkt als een Object JSON.  Bovendien kan JSON inhoud al dan niet standaard worden geparseerd zonder een toewijzing.  Dus een aanvraag met de kop van het inhoudstype `application/json ` als volgt uitziet:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

kan worden geparseerd in een werkstroom met een expressie zoals `@body('myAction')['foo'][0]` om een waarde (in dit geval `bar`).  Geen extra toewijzing nodig is.  Als u met gegevens dat de JSON is maar heeft een header die is opgegeven werkt, u kunt handmatig cast deze over het gebruik van JSON de `@json()` functie (bijvoorbeeld: `@json(triggerBody())['foo']`).

### <a name="textplain"></a>Tekst/zonder opmaak

Dezelfde `application/json`, HTTP-berichten ontvangen met de `Content-Type` koptekst van `text/plain` worden opgeslagen in de onbewerkte formulier.  Bovendien als opgenomen in een latere acties zonder een toewijzing het verzoek gaat af met een `Content-Type`: `text/plain` koptekst.  Als het werken met een plat bestand wordt u bijvoorbeeld de volgende HTTP-inhoud:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

as `text/plain`.  Als in de volgende actie u het hebt verzonden als de hoofdtekst van een andere aanvraag kunt invullen (`@body('flatfile')`), het verzoek moet een `text/plain` inhoudstype koptekst.  Als u met gegevens die is tekst zonder opmaak maar heeft een header die is opgegeven werkt, u kunt handmatig cast deze naar tekst met de `@string()` functie (bijvoorbeeld: `@string(triggerBody())`)

### <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Toepassing/xml Application/octet-stream en conversieprogramma functies

De logica App-Engine behoudt altijd de `Content-Type` die op een antwoord of de HTTP-aanvraag is ontvangen.  Wat betekent dit dat is als een inhoud wordt ontvangen met `Content-Type` van `application/octet-stream`, inclusief die in een latere actie met geen toewijzing zijn ingevoegd om een uitgaande aanvraag met `Content-Type`: `application/octet-stream`.  Op deze manier de engine kunt guaruntee gegevens worden niet verloren als u deze overal in de werkstroom hebt verplaatst.  De actiestatus (invoer en uitvoer) worden echter opgeslagen in een object JSON zoals loopt overal in de werkstroom.  Dit betekent om te behouden van bepaalde gegevenstypen, de engine de inhoud wordt geconverteerd naar een binair base64-gecodeerde tekenreeks met de juiste metagegevens waarbij beide `$content` en `$content-type` -die automatisch worden geconverteerd.  U kunt ook handmatig converteren tussen inhoudstypen ingebouwd in de conversieprogramma functies gebruiken:

* `@json()`-gegevens naar cast`application/json`
* `@xml()`-gegevens naar cast`application/xml`
* `@binary()`-gegevens naar cast`application/octet-stream`
* `@string()`-gegevens naar cast`text/plain`
* `@base64()`-inhoud converteert naar een base64-tekenreeks
* `@base64toString()`-Converteert een tekenreeks base64 codering`text/plain`
* `@base64toBinary()`-Converteert een tekenreeks base64 codering`application/octet-stream`
* `@encodeDataUri()`-gecodeerd van een tekenreeks als dataUri byte-matrix
* `@decodeDataUri()`-een dataUri decoderen in een matrix van bytes

Als u een HTTP-aanvraag met ontvangen bijvoorbeeld `Content-Type`: `application/xml` van:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Ik kan beschouwd en later gebruiken met een ander nummer zoals `@xml(triggerBody())`, of binnen andere functies zoals `@xpath(xml(triggerBody()), '/CustomerName')`.

### <a name="other-content-types"></a>Andere inhoudstypen

Andere inhoudstypen worden ondersteund en werkt met een logica-App, maar mogelijk nodig handmatig ophalen van de berichttekst door decoderen de `$content`.  Als ik uitschakelen van zijn activeert bijvoorbeeld een `application/x-www-url-formencoded` aanvraag die vroeger van de volgende handelingen uit:

```
CustomerName=Frank&Address=123+Avenue
```

omdat dit een geen tekst zonder opmaak of JSON deze moet worden opgeslagen in de actie als:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Waar `$content` is de nettolading gecodeerd als een base64-tekenreeks om alle gegevens te bewaren.  Aangezien er geen momenteel een systeemeigen functie voor formulier-gegevens, kan ik deze gegevens binnen een werkstroom nog steeds gebruiken door het handmatig toegang tot de gegevens met een functie zoals `@string(body('formdataAction'))`.  Als ik mijn uitgaande verzoek om ook de `application/x-www-url-formencoded` inhoudstype koptekst, ik kan alleen toevoegen aan de hoofdtekst actie zonder een toewijzing zoals `@body('formdataAction')`.  Echter dit werkt alleen als hoofdtekst is de enige parameter in het `body` invoer.  Als u probeert te doen `@body('formdataAction')` binnen van een `application/json` aanvragen krijgt u een runtimefout terwijl de gecodeerde hoofdtekst wordt verzonden.
