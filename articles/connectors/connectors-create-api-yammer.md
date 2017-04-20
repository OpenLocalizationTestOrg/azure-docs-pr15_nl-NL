<properties
pageTitle="De verbindingslijn Yammer in uw Apps logica toevoegen | Microsoft Azure"
description="Overzicht van de verbindingslijn Yammer met REST API parameters"
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

# <a name="get-started-with-the-yammer-connector"></a>Aan de slag met de Yammer-connector

Verbinding maken met Yammer aan access gesprekken in uw bedrijfsnetwerk.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2015-08-01-schema voorbeeldversie.

Met Yammer, kunt u het volgende doen:

- De bedrijfswerkstroom van uw op basis van de gegevens die u van Yammer ontvangt maken. 
- Gebruik de gebeurtenis voor wanneer er een nieuw bericht in een groep of een feed uw volgende is.
- Acties gebruiken om een bericht plaatsen, alle berichten en informatie te vinden. Deze acties bent u een reactie en klikt u vervolgens de uitvoer beschikbaar maken voor andere acties. Wanneer een nieuw bericht wordt weergegeven, kunt u bijvoorbeeld een e-mailbericht met behulp van Office 365 verzenden.

Als u wilt een bewerking in logica apps toevoegen, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties
Yammer bevat de volgende triggers en acties. 

Trigger | Acties
--- | ---
<ul><li>Als er een nieuw bericht in een groep</li><li>Als er feed een nieuw bericht in mijn volgende</li></ul>| <ul><li>Alle berichten ontvangt</li><li>Berichten ontvangt in een groep</li><li>Haalt de berichten van mijn volgende feed</li><li>Post-bericht</li><li>Als er een nieuw bericht in een groep</li><li>Als er feed een nieuw bericht in mijn volgende</li></ul>

Alle verbindingslijnen ondersteuning voor gegevens in JSON en XML-indelingen. 

## <a name="create-a-connection-to-yammer"></a>Een verbinding maken met Yammer
Als u wilt gebruiken de Yammer-connector, u maakt eerst een **verbinding** en geef de details voor deze eigenschappen: 

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|Token|Ja|Yammer-referenties opgeven|

>[AZURE.INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]


>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

## <a name="yammer-rest-api-reference"></a>Yammer-REST API-verwijzing
Deze documentatie is bedoeld voor versie: 1.0


### <a name="get-all-public-messages-in-the-logged-in-users-yammer-network"></a>Alle openbare berichten ontvangt in Yammer-netwerk van de gebruiker is aangemeld
Komt overeen met 'Alle' gesprekken in de web-interface voor Yammer.  
```GET: /messages.json```

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|older_then|geheel getal|geen|query|geen|Geeft als resultaat berichten die ouder zijn dan de bericht-ID die is opgegeven als een reeks cijfers. Dit is handig voor pagineren van berichten. Als u momenteel weergeeft 20 berichten en het oudste getal is bijvoorbeeld 2912, kunt u kan toevoegen '? older_than = 2912″ op uw aanvraag om de 20 berichten vóór deze die u ziet.|
|newer_then|geheel getal|geen|query|geen|Retourneert berichten nieuwer dan de bericht-ID die is opgegeven als een reeks cijfers. Dit moet worden gebruikt als polling voor nieuwe berichten. Als u berichten bekijkt en de meest recente bericht 3516 waarde wordt geretourneerd, kunt u een nieuw vergaderverzoek met de parameter "? newer_than = 3516″ om ervoor te zorgen dat er dubbele kopieën van berichten al op uw pagina.|
|limiet|geheel getal|geen|query|geen|Retourneert het opgegeven aantal berichten.|
|pagina|geheel getal|geen|query|geen|Krijgen van de pagina die is opgegeven. Als de geretourneerde gegevens groter is dan de limiet, kunt u dit veld gebruiken voor toegang tot de volgende pagina 's|


### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|408|Time-out van de aanvraag|
|429|Te veel aanvragen|
|500|Interne serverfout. Onbekende fout|
|503|Yammer-Service niet beschikbaar|
|504|Gateway-out|
|Standaard|Bewerking is mislukt.|


### <a name="post-a-message-to-a-group-or-all-company-feed"></a>Een bericht posten naar een groep of gehele bedrijf Feed
Als groeps-ID wordt opgegeven, wordt de opgegeven groep anders die wordt geboekt in alle bedrijf Feed bericht geboekt.    
```POST: /messages.json``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|invoer| |Ja|hoofdtekst|geen|Verzoek om bericht te posten|


### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|201|Gemaakt|



## <a name="object-definitions"></a>Objectdefinities

#### <a name="message-yammer-message"></a>: Bericht Yammer

| Naam | Gegevenstype | Vereist |
|---|---| --- | 
|ID|geheel getal|geen|
|content_excerpt|tekenreeks|geen|
|sender_id|geheel getal|geen|
|replied_to_id|geheel getal|geen|
|created_at|tekenreeks|geen|
|network_id|geheel getal|geen|
|message_type|tekenreeks|geen|
|sender_type|tekenreeks|geen|
|URL|tekenreeks|geen|
|web_url|tekenreeks|geen|
|group_id|geheel getal|geen|
|hoofdtekst|niet gedefinieerd|geen|
|thread_id|geheel getal|geen|
|direct_message|Booleaanse waarde|geen|
|client_type|tekenreeks|geen|
|client_url|tekenreeks|geen|
|taal|tekenreeks|geen|
|notified_user_ids|matrix|geen|
|privacy|tekenreeks|geen|
|liked_by|niet gedefinieerd|geen|
|system_message|Booleaanse waarde|geen|

#### <a name="postoperationrequest-represents-a-post-request-for-yammer-connector-to-post-to-yammer"></a>PostOperationRequest: Hiermee geeft u een post-aanvraag voor Yammer verbindingslijn om te posten op yammer

| Naam | Gegevenstype | Vereist |
|---|---| --- | 
|hoofdtekst|tekenreeks|Ja|
|group_id|geheel getal|geen|
|replied_to_id|geheel getal|geen|
|direct_to_id|geheel getal|geen|
|uitzending|Booleaanse waarde|geen|
|Onderwerp1|tekenreeks|geen|
|onderwerp2|tekenreeks|geen|
|Topic3|tekenreeks|geen|
|Topic4|tekenreeks|geen|
|Topic5|tekenreeks|geen|
|topic6|tekenreeks|geen|
|Topic7|tekenreeks|geen|
|Topic8|tekenreeks|geen|
|topic9|tekenreeks|geen|
|topic10|tekenreeks|geen|
|topic11|tekenreeks|geen|
|topic12|tekenreeks|geen|
|topic13|tekenreeks|geen|
|topic14|tekenreeks|geen|
|topic15|tekenreeks|geen|
|topic16|tekenreeks|geen|
|topic17|tekenreeks|geen|
|topic18|tekenreeks|geen|
|topic19|tekenreeks|geen|
|topic20|tekenreeks|geen|

#### <a name="messagelist-list-of-messages"></a>MessageList: Lijst met berichten

| Naam | Gegevenstype | Vereist |
|---|---| --- | 
|berichten|matrix|geen|


#### <a name="messagebody-message-body"></a>MessageBody: Berichttekst

| Naam | Gegevenstype | Vereist |
|---|---| --- | 
|geparseerde|tekenreeks|geen|
|zonder opmaak|tekenreeks|geen|
|uitgebreide|tekenreeks|geen|

#### <a name="likedby-liked-by"></a>LikedBy: Leuk vindt door

| Naam | Gegevenstype | Vereist |
|---|---| --- | 
|tellen|geheel getal|geen|
|namen|matrix|geen|

#### <a name="yammmerentity-liked-by"></a>YammmerEntity: Leuk vindt door

| Naam | Gegevenstype | Vereist |
|---|---| --- | 
|type|tekenreeks|geen|
|ID|geheel getal|geen|
|full_name|tekenreeks|geen|


## <a name="next-steps"></a>Volgende stappen
[Een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

[1]: ./media/connectors-create-api-yammer/connectionconfig1.png
[2]: ./media/connectors-create-api-yammer/connectionconfig2.png 
[3]: ./media/connectors-create-api-yammer/connectionconfig3.png
[4]: ./media/connectors-create-api-yammer/connectionconfig4.png
[5]: ./media/connectors-create-api-yammer/connectionconfig5.png
