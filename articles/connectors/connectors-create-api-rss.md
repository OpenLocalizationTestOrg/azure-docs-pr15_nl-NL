<properties
pageTitle="RSS | Microsoft Azure"
description="Logica apps maken met de App Azure-service. RSS-connector kan de gebruikers publiceren en feeds items op te halen. Deze kan ook de gebruikers bewerkingen activeren wanneer een nieuw item wordt gepubliceerd naar de feed."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-rss-connector"></a>Aan de slag met de RSS-connector
RSS is een populaire syndicatie webindeling gebruikt voor het publiceren van vaak bijgewerkte inhoud – zoals blogberichten en nieuws.  Veel uitgevers bieden een RSS-feed, zodat gebruikers zich kunnen abonneren op deze.  Gebruik de RSS-verbindingslijn om op te halen feeds informatie en de inwerkingtreding loopt wanneer nieuwe items zijn gepubliceerd in een RSS-feed.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2015-08-01-schema voorbeeldversie. 

U kunt aan de slag met het maken van een app logica nu, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De RSS-connector kan worden gebruikt als een actie; trigger(s) heeft. Alle verbindingslijnen ondersteuning voor gegevens in JSON en XML-indelingen. 

 De RSS-connector heeft de volgende of meerdere acties en/of trigger(s) beschikbaar:

### <a name="rss-actions"></a>RSS-acties
U kunt deze of meerdere acties uitvoeren:

|Actie|Beschrijving|
|--- | ---|
|[ListFeedItems](connectors-create-api-rss.md#listfeeditems)|Alle RSS-feed items krijgen.|
### <a name="rss-triggers"></a>RSS-triggers
U kunt luisteren voor deze gebeurtenis(sen):

|Trigger | Beschrijving|
|--- | ---|
|Wanneer een nieuw item in de feed gepubliceerd|Een werkstroom voor de gebeurtenis wanneer een nieuwe feed wordt gepubliceerd|


## <a name="create-a-connection-to-rss"></a>Een verbinding maken met RSS

>[AZURE.INCLUDE [Steps to create a connection to an RSS feed](../../includes/connectors-create-api-rss.md)]

>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

## <a name="reference-for-rss"></a>Overzicht van RSS
Dit geldt voor versie: 1.0

## <a name="onnewfeed"></a>OnNewFeed
Wanneer een nieuw item in de feed is gepubliceerd: een werkstroom voor de gebeurtenis wanneer een nieuwe feed wordt gepubliceerd 

```GET: /OnNewFeed``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|feedUrl|tekenreeks|Ja|query|geen|Url-feed|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="listfeeditems"></a>ListFeedItems
Lijst van alle RSS-feed items.: alle RSS-feed items ophalen. 

```GET: /ListFeedItems``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|feedUrl|tekenreeks|Ja|query|geen|Url-feed|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="object-definitions"></a>Objectdefinities 

### <a name="triggerbatchresponsefeeditem"></a>TriggerBatchResponse [FeedItem]


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="feeditem"></a>FeedItem


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Ja |
|titel|tekenreeks|Ja |
|inhoud|tekenreeks|Ja |
|koppelingen|matrix|Nee |
|updatedOn|tekenreeks|Nee |


## <a name="next-steps"></a>Volgende stappen
[Een logica-app maken](../app-service-logic/app-service-logic-create-a-logic-app.md)