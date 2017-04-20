<properties
pageTitle=" De toegestane vertraging verbindingslijn gebruiken in uw apps logica | Microsoft Azure"
description="Aan de slag met de verbindingslijn marge in uw Microsoft Azure App Service logica-apps"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-slack-connector"></a>Aan de slag met de toegestane vertraging verbindingslijn

Toegestane vertraging van een team communicatie hulpmiddel, die bij elkaar brengt is al uw team communicatie in één plaatsen, direct doorzoekbare en beschikbaar is waar u ook bent.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2015-08-01-schema voorbeeldversie.

Met de toegestane vertraging verbindingslijn, kunt u het volgende doen:

* Deze gebruiken om logica apps te bouwen

Als u wilt een bewerking in logica apps toevoegen, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="lets-talk-about-triggers-and-actions"></a>Computers nader bekeken triggers en acties

De toegestane vertraging verbindingslijn kan worden gebruikt als een actie; Er zijn geen triggers. Alle verbindingslijnen ondersteuning voor gegevens in JSON en XML-indelingen. 

 De toegestane vertraging verbindingslijn heeft de volgende of meerdere acties en/of trigger(s) beschikbaar:

### <a name="slack-actions"></a>Toegestane vertraging acties
U kunt deze of meerdere acties uitvoeren:

|Actie|Beschrijving|
|--- | ---|
|PostMessage|Een bericht naar een bepaald kanaal plaatsen.|
## <a name="create-a-connection-to-slack"></a>Een verbinding maken met toegestane vertraging
Als u wilt gebruiken de toegestane vertraging verbindingslijn, u maakt eerst een **verbinding** en geef de details voor deze eigenschappen: 

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|Token|Ja|Toegestane vertraging referenties opgeven|

Volg deze stappen om aan te melden bij de toegestane vertraging en voltooi de configuratie van de toegestane vertraging **verbinding** in uw app logica:

1. Selecteer **Terugkeerpatroon**
2. Selecteer een **frequentie** en voer een **Interval**
3. Selecteer **een actie toevoegen**  
![Toegestane vertraging configureren][1]  
4. Voer de toegestane vertraging in het zoekvak en wacht totdat de zoekopdracht tot alle items met toegestane vertraging in de naam te retourneren
5. **Marge - bericht bericht** selecteren
6. Selecteer **aanmelden bij een toegestane vertraging**:  
![Toegestane vertraging configureren][2]
7. Geef uw referenties in om aan te melden bij de toepassing Autoriseer vertraging    
![Toegestane vertraging configureren][3]  
8. U wordt omgeleid naar aanmelden pagina van uw organisatie. **Autoriseer** Toegestane vertraging om te communiceren met uw app logica:      
![Toegestane vertraging configureren][5] 
9. Nadat de autorisatie is voltooid wordt u omgeleid naar uw app logica om deze te voltooien door het configureren van de sectie **toegestane vertraging: alle berichten ontvangt** . Voeg andere triggers en acties die u nodig hebt.  
![Toegestane vertraging configureren][6]
10. Sla uw werk door te selecteren **Opslaan** op de menubalk hierboven.


>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

## <a name="slack-rest-api-reference"></a>Toegestane vertraging REST API-verwijzing
#### <a name="this-documentation-is-for-version-10"></a>Deze documentatie is bedoeld voor versie: 1.0


### <a name="post-a-message-to-a-specified-channel"></a>Een bericht naar een bepaald kanaal plaatsen.
**```POST: /chat.postMessage```** 



| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|kanaal|tekenreeks|Ja|query|geen|Kanaal, persoonlijke groep of IM-kanaal bericht wilt sturen. Een naam kan zijn (ex: #general) of een gecodeerd-ID.|
|tekst|tekenreeks|Ja|query|geen|Tekst van het bericht wilt verzenden. Zie voor opmaakopties, https://api.slack.com/docs/formatting.|
|gebruikersnaam|tekenreeks|geen|query|geen|Naam van de bots|
|as_user|Booleaanse waarde|geen|query|geen|Doorgeven waar het bericht als de geverifieerde gebruiker, in plaats van als een bots boeken|
|parseren|tekenreeks|geen|query|geen|Wijzigen hoe berichten worden behandeld. Zie https://api.slack.com/docs/formatting voor meer informatie.|
|link_names|geheel getal|geen|query|geen|Zoeken en koppeling kanaal namen en gebruikersnamen.|
|unfurl_links|Booleaanse waarde|geen|query|geen|Doorgeven True inschakelen unfurling hoofdzakelijk op tekst gebaseerde inhoud.|
|unfurl_media|Booleaanse waarde|geen|query|geen|Doorgeven ONWAAR om uit te schakelen unfurling van media-inhoud.|
|icon_url|tekenreeks|geen|query|geen|URL van een afbeelding wilt gebruiken als een pictogram voor dit bericht|
|icon_emoji|tekenreeks|geen|query|geen|Emoji wilt gebruiken als een pictogram voor dit bericht|


### <a name="here-are-the-possible-responses"></a>Dit zijn de mogelijke antwoorden:

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|408|Time-out van de aanvraag|
|429|Te veel aanvragen|
|500|Interne serverfout. Onbekende fout|
|503|Toegestane vertraging Service niet beschikbaar|
|504|Gateway-out|
|Standaard|Bewerking is mislukt.|
------



## <a name="object-definitions"></a>Object (s): 

 **Bericht**: Yammer-bericht

Vereiste eigenschappen voor bericht:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|ID|geheel getal|
|content_excerpt|tekenreeks|
|sender_id|geheel getal|
|replied_to_id|geheel getal|
|created_at|tekenreeks|
|network_id|geheel getal|
|message_type|tekenreeks|
|sender_type|tekenreeks|
|URL|tekenreeks|
|web_url|tekenreeks|
|group_id|geheel getal|
|hoofdtekst|niet gedefinieerd|
|thread_id|geheel getal|
|direct_message|Booleaanse waarde|
|client_type|tekenreeks|
|client_url|tekenreeks|
|taal|tekenreeks|
|notified_user_ids|matrix|
|privacy|tekenreeks|
|liked_by|niet gedefinieerd|
|system_message|Booleaanse waarde|



 **PostOperationRequest**: Hiermee geeft u een post-aanvraag voor Yammer verbindingslijn om te posten op yammer

Vereiste eigenschappen voor PostOperationRequest:

hoofdtekst

**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|hoofdtekst|tekenreeks|
|group_id|geheel getal|
|replied_to_id|geheel getal|
|direct_to_id|geheel getal|
|uitzending|Booleaanse waarde|
|Onderwerp1|tekenreeks|
|onderwerp2|tekenreeks|
|Topic3|tekenreeks|
|Topic4|tekenreeks|
|Topic5|tekenreeks|
|topic6|tekenreeks|
|Topic7|tekenreeks|
|Topic8|tekenreeks|
|topic9|tekenreeks|
|topic10|tekenreeks|
|topic11|tekenreeks|
|topic12|tekenreeks|
|topic13|tekenreeks|
|topic14|tekenreeks|
|topic15|tekenreeks|
|topic16|tekenreeks|
|topic17|tekenreeks|
|topic18|tekenreeks|
|topic19|tekenreeks|
|topic20|tekenreeks|



 **MessageList**: lijst met berichten

Vereiste eigenschappen voor MessageList:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|berichten|matrix|



 **MessageBody**: berichttekst

Vereiste eigenschappen voor MessageBody:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|geparseerde|tekenreeks|
|zonder opmaak|tekenreeks|
|uitgebreide|tekenreeks|



 **LikedBy**: leuk vindt door

Vereiste eigenschappen voor LikedBy:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|tellen|geheel getal|
|namen|matrix|



 **YammmerEntity**: leuk vindt door

Vereiste eigenschappen voor YammmerEntity:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|type|tekenreeks|
|ID|geheel getal|
|full_name|tekenreeks|


## <a name="next-steps"></a>Volgende stappen
[Een logica-app maken](../app-service-logic/app-service-logic-create-a-logic-app.md)
## <a name="object-definitions"></a>Object (s): 

 **WebResultModel**: zoekresultaten voor Bing web

Vereiste eigenschappen voor WebResultModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|Titel|tekenreeks|
|Beschrijving|tekenreeks|
|DisplayUrl|tekenreeks|
|ID|tekenreeks|
|FullUrl|tekenreeks|



 **VideoResultModel**: Bing video zoekresultaten

Vereiste eigenschappen voor VideoResultModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|Titel|tekenreeks|
|DisplayUrl|tekenreeks|
|ID|tekenreeks|
|MediaUrl|tekenreeks|
|Runtime|geheel getal|
|Miniatuur|niet gedefinieerd|



 **ThumbnailModel**: miniatuur van de eigenschappen van het multimedia element

Vereiste eigenschappen voor ThumbnailModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|MediaUrl|tekenreeks|
|ContentType|tekenreeks|
|Breedte|geheel getal|
|Hoogte|geheel getal|
|FileSize|geheel getal|



 **ImageResultModel**: zoekresultaten voor Bing image

Vereiste eigenschappen voor ImageResultModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|Titel|tekenreeks|
|DisplayUrl|tekenreeks|
|ID|tekenreeks|
|MediaUrl|tekenreeks|
|Vervolgens kunnen|tekenreeks|
|Miniatuur|niet gedefinieerd|



 **NewsResultModel**: zoekresultaten voor Bing nieuws

Vereiste eigenschappen voor NewsResultModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|Titel|tekenreeks|
|Beschrijving|tekenreeks|
|DisplayUrl|tekenreeks|
|ID|tekenreeks|
|Bron|tekenreeks|
|Datum|tekenreeks|



 **SpellResultModel**: Bing spelling suggesties resultaten

Vereiste eigenschappen voor SpellResultModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|ID|tekenreeks|
|Waarde|tekenreeks|



 **RelatedSearchResultModel**: Bing gerelateerde lijst met zoekresultaten

Vereiste eigenschappen voor RelatedSearchResultModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|Titel|tekenreeks|
|ID|tekenreeks|
|BingUrl|tekenreeks|



 **CompositeSearchResultModel**: Bing samengestelde zoekresultaten

Vereiste eigenschappen voor CompositeSearchResultModel:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|WebResultsTotal|geheel getal|
|ImageResultsTotal|geheel getal|
|VideoResultsTotal|geheel getal|
|NewsResultsTotal|geheel getal|
|SpellSuggestionsTotal|geheel getal|
|WebResults|matrix|
|ImageResults|matrix|
|VideoResults|matrix|
|NewsResults|matrix|
|SpellSuggestionResults|matrix|
|RelatedSearchResults|matrix|


## <a name="object-definitions"></a>Object (s): 

 **PostOperationResponse**: antwoord van post-bewerking van marge verbindingslijn vertegenwoordigt voor posten naar marge

Vereiste eigenschappen voor PostOperationResponse:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|OK|Booleaanse waarde|
|kanaal|tekenreeks|
|TS|tekenreeks|
|Bericht|niet gedefinieerd|
|Fout|tekenreeks|



 **MessageItem**: een kanaalbericht.

Vereiste eigenschappen voor MessageItem:


Geen van de eigenschappen zijn vereist. 


**Alle eigenschappen**: 


| Naam | Gegevenstype |
|---|---|
|tekst|tekenreeks|
|ID|tekenreeks|
|gebruiker|tekenreeks|
|gemaakt|geheel getal|
|is_user verwijderd|Booleaanse waarde|


## <a name="next-steps"></a>Volgende stappen
[Een logica-app maken](../app-service-logic/app-service-logic-create-a-logic-app.md)

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
