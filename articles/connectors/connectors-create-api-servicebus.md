<properties
pageTitle="Leer hoe u met de busconnector van Azure-Service in uw apps logica | Microsoft Azure"
description="Logica apps maken met de App Azure-service. Verbinding maken met Azure Service Bus verzenden en ontvangen van berichten. U kunt acties zoals verzenden naar wachtrij, verzenden naar onderwerp, ontvangen uit wachtrij en u ontvangt van abonnement kunt uitvoeren."
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
ms.date="08/02/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-azure-service-bus-connector"></a>Aan de slag met de busconnector van Azure-Service

Verbinding maken met Azure Service Bus verzenden en ontvangen van berichten. U kunt acties zoals verzenden naar wachtrij, verzenden naar onderwerp, ontvangen uit wachtrij en u ontvangt van abonnement kunt uitvoeren.

Als u wilt gebruiken op [een verbindingslijn](./apis-list.md), moet u eerst een logica-app maakt. U kunt aan de slag door nu een logica-app te [maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-service-bus"></a>Verbinding maken met de Service Bus

Voordat uw app logica toegang elke service tot, moet u eerst een verbinding maken met de service. Een [verbinding](./connectors-overview.md) biedt connectiviteit tussen een logica-app en een andere service.  

>[AZURE.INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]

## <a name="use-a-service-bus-trigger"></a>Gebruik een Service Bus-trigger

Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een app logica te starten. [Meer informatie over activering](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]  

## <a name="use-a-service-bus-action"></a>Gebruik de actie van een Service Bus

Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een app logica. [Meer informatie over acties](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

[AZURE.INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]  

## <a name="technical-details"></a>Technische details

Hier vindt u de details van de triggers, acties en antwoorden die ondersteuning biedt voor deze verbinding.

### <a name="service-bus-triggers"></a>Service Bus triggers

Service Bus heeft de volgende triggers:  

|Trigger | Beschrijving|
|--- | ---|
|[Wanneer een bericht is ontvangen in een wachtrij](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-queue)|Deze bewerking gebeurtenis een stroom wanneer een bericht is ontvangen in een wachtrij.|
|[Wanneer een bericht is ontvangen in een onderwerp-abonnement](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-topic-subscription)|Deze bewerking gebeurtenis een stroom wanneer een bericht is ontvangen in een onderwerp-abonnement.|


### <a name="service-bus-actions"></a>Service Bus acties

Service Bus heeft de volgende bewerkingen uit:


|Actie|Beschrijving|
|--- | ---|
|[Bericht verzenden](connectors-create-api-servicebus.md#send-message)|Deze bewerking stuurt een bericht naar een wachtrij of onderwerp.|
### <a name="action-and-trigger-details"></a>Actie en de inwerkingtreding details

Hier vindt u informatie over de voor de acties en triggers voor deze verbindingslijn, samen met hun antwoorden.



#### <a name="send-message"></a>Bericht verzenden

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|ContentData *|Inhoud|De inhoud van het bericht.|
|ContentType|Inhoudstype|Inhoudstype van inhoud van het bericht.|
|Eigenschappen|Eigenschappen|Sleutel-waardeparen voor elk brokered eigenschap.|
|entityName *|Wachtrij/onderwerpnaam|Naam van de wachtrij of onderwerp.|

Deze geavanceerde parameters zijn ook beschikbaar:

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|MessageId|Bericht-Id|Een door de gebruiker gedefinieerde waarde die Service Bus identificeren dubbele berichten kunt als ingeschakeld.|
|Aan|Aan|Adres als u wilt verzenden naar.|
|ReplyTo|Beantwoorden|Adres van de wachtrij beantwoorden.|
|ReplyToSessionId|Antwoord op de sessie-Id|Id van de sessie beantwoorden.|
|Label|Label|Toepassingsspecifieke label.|
|ScheduledEnqueueTimeUtc|ScheduledEnqueueTimeUtc|Datum en tijd in UTC, wanneer het bericht, worden toegevoegd aan de wachtrij.|
|Sessie-id|Sessie-Id|Id van de sessie.|
|CorrelationId|Correlatie-Id|Id van de correlatie.|
|TimeToLive|Time To Live|De duur in tikken, dat een bericht geldig is. De duur wordt gestart uit wanneer het bericht wordt verzonden naar de Service Bus.|



Een * geeft aan dat een eigenschap vereist is.


#### <a name="when-a-message-is-received-in-a-queue"></a>Wanneer een bericht is ontvangen in een wachtrij

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|Wachtrijnaam *|De wachtrijnaam van de|De naam van de wachtrij.|


Een * geeft aan dat een eigenschap vereist is.


##### <a name="output-details"></a>Gegevens voor uitvoer

ServiceBusMessage: Dit object heeft de inhoud en de eigenschappen van een bericht Service Bus.


| Naam van eigenschap | Gegevenstype | Beschrijving |
|---|---|---|
|ContentData|tekenreeks|De inhoud van het bericht.|
|ContentType|tekenreeks|Inhoudstype van inhoud van het bericht.|
|Eigenschappen|object|Sleutel-waardeparen voor elk brokered eigenschap.|
|MessageId|tekenreeks|Een door de gebruiker gedefinieerde waarde die Service Bus identificeren dubbele berichten kunt als ingeschakeld.|
|Aan|tekenreeks|Verzenden naar adres.|
|ReplyTo|tekenreeks|Adres van de wachtrij beantwoorden.|
|ReplyToSessionId|tekenreeks|Id van de sessie beantwoorden.|
|Label|tekenreeks|Toepassingsspecifieke label.|
|ScheduledEnqueueTimeUtc|tekenreeks|Datum en tijd in UTC, wanneer het bericht, worden toegevoegd aan de wachtrij.|
|Sessie-id|tekenreeks|Id van de sessie.|
|CorrelationId|tekenreeks|Id van de correlatie.|
|TimeToLive|tekenreeks|De duur in tikken, dat een bericht geldig is. De duur wordt gestart uit wanneer het bericht wordt verzonden naar de Service Bus.|




#### <a name="when-a-message-is-received-in-a-topic-subscription"></a>Wanneer een bericht is ontvangen in een onderwerp-abonnement

|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|topicName *|De onderwerpnaam van het|De naam van het onderwerp.|
|subscriptionName *|Onderwerp de naam van abonnement|Naam van het abonnement dat onderwerp.|


Een * geeft aan dat een eigenschap vereist is.


##### <a name="output-details"></a>Gegevens voor uitvoer

ServiceBusMessage: Dit object heeft de inhoud en de eigenschappen van een bericht Service Bus.


| Naam van eigenschap | Gegevenstype | Beschrijving |
|---|---|---|
|ContentData|tekenreeks|De inhoud van het bericht.|
|ContentType|tekenreeks|Inhoudstype van inhoud van het bericht.|
|Eigenschappen|object|Sleutel-waardeparen voor elk brokered eigenschap.|
|MessageId|tekenreeks|Een door de gebruiker gedefinieerde waarde die Service Bus identificeren dubbele berichten kunt als ingeschakeld.|
|Aan|tekenreeks|Verzenden naar adres.|
|ReplyTo|tekenreeks|Adres van de wachtrij beantwoorden.|
|ReplyToSessionId|tekenreeks|Id van de sessie beantwoorden.|
|Label|tekenreeks|Toepassingsspecifieke label.|
|ScheduledEnqueueTimeUtc|tekenreeks|Datum en tijd in UTC, wanneer het bericht, worden toegevoegd aan de wachtrij.|
|Sessie-id|tekenreeks|Id van de sessie.|
|CorrelationId|tekenreeks|Id van de correlatie.|
|TimeToLive|tekenreeks|De duur in tikken, dat een bericht geldig is. De duur wordt gestart uit wanneer het bericht wordt verzonden naar de Service Bus.|



### <a name="http-responses"></a>HTTP-antwoorden

De voorgaande acties en triggers kunnen retourneren een of meer van de volgende codes van de HTTP-status:

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout opgetreden.|
|Standaard|Bewerking is mislukt.|

## <a name="next-steps"></a>Volgende stappen
[Een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).
