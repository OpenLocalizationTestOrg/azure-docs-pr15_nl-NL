<properties
pageTitle="De verbindingslijn Office 365 Video gebruiken in uw apps logica | Microsoft Azure"
description="Aan de slag met de Office 365 Video-connector in uw Microsoft Azure App service logica-apps"
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

# <a name="get-started-with-the-office365-video-connector"></a>Aan de slag met de Office 365 Video-connector
Verbinding maken met Office 365 Video u kunt informatie over een Office 365 video een lijst met video's en meer. De verbindingslijn Office 365 Video kan worden gebruikt vanaf:

- Logica apps 

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2015-08-01-schema voorbeeldversie. Deze verbindingslijn wordt niet ondersteund in eerdere versies schema.

Met Office 365 Video, kunt u het volgende doen:

- De bedrijfswerkstroom van uw op basis van de gegevens die u van Office 365 Video ontvangt maken. 
- Acties die de video portal status controleren, een lijst met alle video in een kanaal en meer gebruiken. Deze acties bent u een reactie en klikt u vervolgens de uitvoer beschikbaar maken voor andere acties. U kunt bijvoorbeeld de zoekfunctie voor Bing-connector gebruiken om te zoeken voor Office 365-video's en gebruikt u de Office 365 video-connector voor informatie over deze video. Als de video aan uw vereisten voldoet, kunt u deze video posten op Facebook. 

Als u wilt een bewerking in logica apps toevoegen, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De Office 365 Video-connector heeft de volgende acties beschikbaar. Er zijn geen triggers.

| Triggers | Acties|
| --- | --- |
| Geen | <ul><li>Controles voor video-portal status</li><li>Alle zichtbare kanalen ophalen</li><li>Afspelen-url van het manifest Azure Media Services ophalen voor een video</li><li>De dragertoken toegang kunt krijgen tot de video ontsleutelen ophalen</li><li>Informatie ophalen over een bepaalde Office 365 video</li><li>Lijsten die alle Office 365-video's in een kanaal aanbevelen presenteren</li></ul>

Alle verbindingslijnen ondersteuning voor gegevens in JSON en XML-indelingen. 

## <a name="create-a-connection-to-office365-video-connector"></a>Een verbinding maken met Office 365 Video-connector
Wanneer u deze connector aan uw apps logica toevoegt, moet u aanmelden bij uw Office 365 Video-account en toestaan logica apps verbinding maken met uw account.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Nadat u de verbinding hebt gemaakt, u voert u de Office 365 video-eigenschappen, zoals de naam van de tenant of kanaal ID. De **REST API verwijzing** in dit onderwerp worden deze eigenschappen.

>[AZURE.TIP] U kunt deze dezelfde verbinding met Office 365 Video gebruiken in andere apps logica.

## <a name="swagger-rest-api-reference"></a>Swagger REST API verwijzing
Dit geldt voor versie: 1.0.

### <a name="checks-video-portal-status"></a>Controles voor video-portal status 
Hiermee wordt gecontroleerd op de video portal-status om te zien als video services zijn ingeschakeld.  
```GET: /{tenant}/IsEnabled``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tenant|tekenreeks|Ja|pad|geen|De naam van de tenant voor de map de gebruiker maakt deel uit van|


#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|404|Niet gevonden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|



### <a name="get-all-viewable-channels"></a>Alle zichtbare kanalen ophalen 
Hiermee haalt u alle kanalen die de gebruiker heeft toegang tot weergeven.  
```GET: /{tenant}/Channels``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tenant|tekenreeks|Ja|pad|geen|De naam van de tenant voor de map de gebruiker maakt deel uit van|


#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|404|Niet gevonden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|




### <a name="lists-all-the-office365-videos-present-in-a-channel"></a>Lijsten die alle Office 365-video's in een kanaal aanbevelen presenteren 
Lijsten die alle Office 365-video's in een kanaal presenteren.  
```GET: /{tenant}/Channels/{channelId}/Videos``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tenant|tekenreeks|Ja|pad|geen|De naam van de tenant voor de map de gebruiker maakt deel uit van|
|channelId|tekenreeks|Ja|pad|geen|De id van de kanaal waarin de video's moeten worden opgehaald|


#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|404|Niet gevonden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|




### <a name="gets-information-about-a-particular-office365-video"></a>Informatie ophalen over een bepaalde Office 365 video 
Informatie over een bepaalde Office 365 krijgt video.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tenant|tekenreeks|Ja|pad|geen|De naam van de tenant voor de map de gebruiker maakt deel uit van|
|channelId|tekenreeks|Ja|pad|geen|De kanaal-id|
|videoId|tekenreeks|Ja|pad|geen|De video-id|


#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|404|Niet gevonden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|




### <a name="get-playback-url-of-the-azure-media-services-manifest-for-a-video"></a>Afspelen-url van het manifest Azure Media Services ophalen voor een video 
Afspelen-url van het manifest Azure Media Services krijgen voor een video.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/playbackurl``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tenant|tekenreeks|Ja|pad|geen|De naam van de tenant voor de map de gebruiker maakt deel uit van|
|channelId|tekenreeks|Ja|pad|geen|De kanaal-id|
|videoId|tekenreeks|Ja|pad|geen|De video-id|
|streamingFormatType|tekenreeks|Ja|query|geen|Streaming indelingstype. 1 - vloeiend Streaming of MPEG-streepje. 0 - HLS Streaming|


#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|404|Niet gevonden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|




### <a name="get-the-bearer-token-to-get-access-to-decrypt-the-video"></a>De dragertoken toegang kunt krijgen tot de video ontsleutelen ophalen 
Krijgen de dragertoken toegang kunt krijgen tot de video ontsleutelen.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/token```

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tenant|tekenreeks|Ja|pad|geen|De naam van de tenant voor de map de gebruiker maakt deel uit van|
|channelId|tekenreeks|Ja|pad|geen|De kanaal-id|
|videoId|tekenreeks|Ja|pad|geen|De video-id|


#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|404|Niet gevonden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


## <a name="object-definitions"></a>Objectdefinities

#### <a name="channel-channel-class"></a>Kanaal: Kanaal class

| Naam | Gegevenstype | Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|Titel|tekenreeks|geen|
|Beschrijving|tekenreeks|geen|


#### <a name="video"></a>Video 

| Naam | Gegevenstype |Vereist|
|---|---|---|
|ID|tekenreeks|geen|
|Titel|tekenreeks|geen|
|Beschrijving|tekenreeks|geen|
|CreationDate|tekenreeks|geen|
|Eigenaar|tekenreeks|geen|
|ThumbnailUrl|tekenreeks|geen|
|VideoUrl|tekenreeks|geen|
|VideoDuration|geheel getal|geen|
|VideoProcessingStatus|geheel getal|geen|
|ViewCount|geheel getal|geen|


## <a name="next-steps"></a>Volgende stappen
[Een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).
