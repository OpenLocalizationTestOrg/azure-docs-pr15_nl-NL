<properties
    pageTitle="De Facebook-connector in uw Apps logica toevoegen | Microsoft Azure"
    description="Overzicht van de Facebook-connector met REST API parameters"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-facebook-connector"></a>Aan de slag met de Facebook-connector
Verbinding maken met Facebook en publiceren naar een tijdlijn, krijgen een pagina-feed en meer. 

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2015-08-01-schema voorbeeldversie.


Met Facebook, kunt u het volgende doen:

- Bouw uw zakelijke flow op basis van de gegevens die u van Facebook ontvangt. 
- Gebruik een trigger wanneer een nieuw bericht is ontvangen.
- Gebruik acties die posten naar de tijdlijn, krijgen een pagina-feed en meer. Deze acties bent u een reactie en klikt u vervolgens de uitvoer beschikbaar maken voor andere acties. Bijvoorbeeld wanneer er een nieuw bericht op de tijdlijn, kunt u dat bericht nemen en doorgeven aan uw Twitter-feed. 



Als u wilt een bewerking in logica apps toevoegen, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties
De Facebook-connector bevat de volgende trigger en acties. 

| Triggers | Acties|
| --- | --- |
| <ul><li>Als er een nieuw bericht op de tijdlijn</li></ul> |<ul><li>Krijg feed vanuit mijn tijdlijn</li><li>Posten naar mijn tijdlijn</li><li>Als er een nieuw bericht op de tijdlijn</li><li>Pagina-feed ophalen</li><li>Gebruiker tijdlijn ophalen</li><li>Posten naar pagina</li></ul>

Alle verbindingslijnen ondersteuning voor gegevens in JSON en XML-indelingen.

## <a name="create-a-connection-to-facebook"></a>Een verbinding maken met Facebook
Wanneer u deze connector aan uw apps logica toevoegt, moet u verbinding maakt met uw Facebook-logica apps machtigen.

1. Meld u aan bij uw Facebook-account
2. Selecteer **autoriseren**en laat uw apps logica verbinding maken en gebruiken van uw Facebook. 

>[AZURE.INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]

>[AZURE.TIP] U kunt deze dezelfde Facebook-verbinding gebruiken in andere apps logica.

## <a name="swagger-rest-api-reference"></a>Swagger REST API verwijzing
Dit geldt voor versie: 1.0.

### <a name="get-feed-from-my-timeline"></a>Krijg feed vanuit mijn tijdlijn
Krijgt de feeds uit de tijdlijn van de gebruiker is aangemeld.  
```GET: /me/feed```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|velden|tekenreeks|geen|query|geen |De velden die u als resultaat wilt geven. Voorbeeld (-id, naam, afbeelding).|
|limiet|geheel getal|geen|query| geen|Maximum aantal berichten moeten worden opgehaald|
|met|tekenreeks|geen|query| geen|De lijst met berichten beperken tot alleen de locatie die zijn gekoppeld.|
|filter|tekenreeks|geen|query| geen|Hiermee kunt u alleen de berichten die overeenkomen met een bepaalde stream filter ophalen.|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


### <a name="post-to-my-timeline"></a>Posten naar mijn tijdlijn
Een statusbericht aan de gebruiker is aangemeld tijdlijn plaatsen.  
```POST: /me/feed```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Verzenden|tekenreeks |Ja|hoofdtekst|geen |Nieuw bericht moeten worden geboekt|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


### <a name="when-there-is-a-new-post-on-my-timeline"></a>Als er een nieuw bericht op de tijdlijn
Een nieuwe stroom gebeurtenis wanneer er een nieuw bericht op de gebruiker is aangemeld tijdlijn wordt.  
```GET: /trigger/me/feed```

Er zijn geen parameters. 

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


### <a name="get-page-feed"></a>Pagina-feed ophalen
Berichten krijgen van de feed van een bepaalde pagina.  
```GET: /{pageId}/feed```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|pageId|tekenreeks|Ja|pad| geen|Id van de pagina waaruit berichten moeten worden opgehaald.|
|limiet|geheel getal|geen|query| geen|Maximum aantal berichten moeten worden opgehaald|
|include_hidden|Booleaanse waarde|geen|query|geen |Al dan niet om op te nemen alle berichten die door de pagina zijn verborgen|
|velden|tekenreeks|geen|query|geen |De velden die u als resultaat wilt geven. Voorbeeld (-id, naam, afbeelding).|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


### <a name="get-user-timeline"></a>Gebruiker tijdlijn ophalen
Berichten krijgen van de tijdlijn van een gebruiker.  
```GET: /{userId}/feed```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|gebruikers-id|tekenreeks|Ja|pad|geen |Id van de gebruiker waarvan tijdlijn hebt moeten worden opgehaald.|
|limiet|geheel getal|geen|query|geen |Maximum aantal berichten moeten worden opgehaald|
|met|tekenreeks|geen|query|geen |De lijst met berichten beperken tot alleen de locatie die zijn gekoppeld.|
|filter|tekenreeks|geen|query| geen|Hiermee kunt u alleen de berichten die overeenkomen met een bepaalde stream filter ophalen.|
|velden|tekenreeks|geen|query| geen|De velden die u als resultaat wilt geven. Voorbeeld (-id, naam, afbeelding).|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


### <a name="post-to-page"></a>Posten naar pagina
Een bericht naar een Facebook-Page plaatsen als de gebruiker is aangemeld.  
```POST: /{pageId}/feed```

| Naam|Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|pageId|tekenreeks|Ja|pad|geen |Id van de pagina om het te plaatsen.|
|Verzenden|veel |Ja|hoofdtekst|geen |Nieuwe berichten wordt geplaatst.|

#### <a name="response"></a>Antwoord
|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


## <a name="object-definitions"></a>Objectdefinities

#### <a name="getfeedresponse"></a>GetFeedResponse

|Naam van eigenschap | Gegevenstype | Vereist|
|---|---|---|
|gegevens|matrix|geen|

#### <a name="triggerfeedresponse"></a>TriggerFeedResponse

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|gegevens|matrix|geen|

#### <a name="postitem-a-single-entry-in-a-profiles-feed"></a>PostItem: Één vermelding in een profiel bevindt zich feed
Het profiel is mogelijk een gebruiker, een pagina, een app of een groep. 

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|admin_creator|matrix|geen|
|bijschrift|tekenreeks|geen|
|created_time|tekenreeks|geen|
|Beschrijving|tekenreeks|geen|
|feed_targeting|niet gedefinieerd|geen|
|Van|niet gedefinieerd|geen|
|pictogram|tekenreeks|geen|
|is_hidden|Booleaanse waarde|geen|
|is_published|Booleaanse waarde|geen|
|koppeling|tekenreeks|geen|
|Bericht|tekenreeks|geen|
|naam|tekenreeks|geen|
|object_id|tekenreeks|geen|
|afbeelding|tekenreeks|geen|
|plaatsen|niet gedefinieerd|geen|
|privacy|niet gedefinieerd|geen|
|Eigenschappen|matrix|geen|
|bron|tekenreeks|geen|
|status_type|tekenreeks|geen|
|verhaal|tekenreeks|geen|
|doelgroepen|niet gedefinieerd|geen|
|Aan|matrix|geen|
|type|tekenreeks|geen|
|updated_time|tekenreeks|geen|
|with_tags|niet gedefinieerd|geen|

#### <a name="triggeritem-a-single-entry-in-a-profiles-feed"></a>TriggerItem: Één vermelding in een profiel bevindt zich feed
Het profiel is mogelijk een gebruiker, een pagina, een app of een groep.

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|created_time|tekenreeks|geen|
|Van|niet gedefinieerd|geen|
|Bericht|tekenreeks|geen|
|type|tekenreeks|geen|

#### <a name="adminitem"></a>AdminItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|koppeling|tekenreeks|geen|

#### <a name="propertyitem"></a>PropertyItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|naam|tekenreeks|geen|
|tekst|tekenreeks|geen|
|href|tekenreeks|geen|

#### <a name="userpostfeedrequest"></a>UserPostFeedRequest

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|Bericht|tekenreeks|Ja|
|koppeling|tekenreeks|geen|
|afbeelding|tekenreeks|geen|
|naam|tekenreeks|geen|
|bijschrift|tekenreeks|geen|
|Beschrijving|tekenreeks|geen|
|plaatsen|tekenreeks|geen|
|labels|tekenreeks|geen|
|privacy|niet gedefinieerd|geen|
|object_attachment|tekenreeks|geen|

#### <a name="pagepostfeedrequest"></a>PagePostFeedRequest

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|Bericht|tekenreeks|Ja|
|koppeling|tekenreeks|geen|
|afbeelding|tekenreeks|geen|
|naam|tekenreeks|geen|
|bijschrift|tekenreeks|geen|
|Beschrijving|tekenreeks|geen|
|acties|matrix|geen|
|plaatsen|tekenreeks|geen|
|labels|tekenreeks|geen|
|object_attachment|tekenreeks|geen|
|doelgroepen|niet gedefinieerd|geen|
|feed_targeting|niet gedefinieerd|geen|
|gepubliceerd|Booleaanse waarde|geen|
|scheduled_publish_time|tekenreeks|geen|
|backdated_time|tekenreeks|geen|
|backdated_time_granularity|tekenreeks|geen|
|child_attachments|matrix|geen|
|multi_share_end_card|Booleaanse waarde|geen|

#### <a name="postfeedresponse"></a>PostFeedResponse

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|

#### <a name="profilecollection"></a>ProfileCollection

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|gegevens|matrix|geen|

#### <a name="useritem"></a>UserItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|Voornaam|tekenreeks|geen|
|Achternaam|tekenreeks|geen|
|naam|tekenreeks|geen|
|geslacht|tekenreeks|geen|
|over|tekenreeks|geen|

#### <a name="actionitem"></a>ActionItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|naam|tekenreeks|geen|
|koppeling|tekenreeks|geen|

#### <a name="targetitem"></a>TargetItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|landen|matrix|geen|
|Landinstellingen|matrix|geen|
|regio 's|matrix|geen|
|steden|matrix|geen|

#### <a name="feedtargetitem-object-that-controls-news-feed-targeting-for-this-post"></a>FeedTargetItem: Object waarmee wordt bepaald nieuws feed doelgroepen voor dit bericht
Iedereen in deze groepen wordt vaker dit bericht ziet, anderen minder snel zijn. Alleen van toepassing op pagina's.

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|landen|matrix|geen|
|regio 's|matrix|geen|
|steden|matrix|geen|
|age_min|geheel getal|geen|
|age_max|geheel getal|geen|
|geslachten|matrix|geen|
|relationship_statuses|matrix|geen|
|interested_in|matrix|geen|
|college_years|matrix|geen|
|interesses|matrix|geen|
|relevant_until|geheel getal|geen|
|education_statuses|matrix|geen|
|Landinstellingen|matrix|geen|

#### <a name="placeitem"></a>PlaceItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|naam|tekenreeks|geen|
|overall_rating|getal|geen|
|locatie|niet gedefinieerd|geen|

#### <a name="locationitem"></a>LocationItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|plaats|tekenreeks|geen|
|land|tekenreeks|geen|
|breedtegraad|getal|geen|
|located_in|tekenreeks|geen|
|lengtegegevens|getal|geen|
|naam|tekenreeks|geen|
|regio|tekenreeks|geen|
|de staat|tekenreeks|geen|
|straat|tekenreeks|geen|
|Postcode|tekenreeks|geen|

#### <a name="privacyitem"></a>PrivacyItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|Beschrijving|tekenreeks|geen|
|waarde|tekenreeks|Ja|
|toestaan|tekenreeks|geen|
|weigeren|tekenreeks|geen|
|vrienden|tekenreeks|geen|

#### <a name="childattachmentsitem"></a>ChildAttachmentsItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|koppeling|tekenreeks|geen|
|afbeelding|tekenreeks|geen|
|image_hash|tekenreeks|geen|
|naam|tekenreeks|geen|
|Beschrijving|tekenreeks|geen|

#### <a name="postphotorequest"></a>PostPhotoRequest

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|URL|tekenreeks|Ja|
|bijschrift|tekenreeks|geen|

#### <a name="postphotoresponse"></a>PostPhotoResponse

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|Ja|
|post_id|tekenreeks|Ja|

#### <a name="postvideorequest"></a>PostVideoRequest

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|videoData|tekenreeks|Ja|
|Beschrijving|tekenreeks|Ja|
|titel|tekenreeks|Ja|
|uploadedVideoName|tekenreeks|geen|

#### <a name="getphotoresponse"></a>GetPhotoResponse

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|gegevens|niet gedefinieerd|Ja|

#### <a name="getphotoresponseitem"></a>GetPhotoResponseItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|URL|tekenreeks|Ja|
|is_silhouette|Booleaanse waarde|Ja|
|hoogte|tekenreeks|geen|
|Breedte|tekenreeks|geen|

#### <a name="geteventresponse"></a>GetEventResponse

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|gegevens|matrix|Ja|

#### <a name="geteventresponseitem"></a>GetEventResponseItem

|Naam van eigenschap | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|Ja|
|naam|tekenreeks|Ja|
|start_time|tekenreeks|geen|
|end_time|tekenreeks|geen|
|tijdzone|tekenreeks|geen|
|locatie|tekenreeks|geen|
|Beschrijving|tekenreeks|geen|
|ticket_uri|tekenreeks|geen|
|rsvp_status|tekenreeks|Ja|


## <a name="next-steps"></a>Volgende stappen

[Een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).
