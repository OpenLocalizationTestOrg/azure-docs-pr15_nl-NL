<properties
pageTitle="SendGrid | Microsoft Azure"
description="Logica apps maken met de App Azure-service. SendGrid verbindingsprovider kunt u e-mailbericht verzenden en beheren van adressenlijsten."
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

# <a name="get-started-with-the-sendgrid-connector"></a>Aan de slag met de verbindingslijn SendGrid

SendGrid verbindingsprovider kunt u e-mailbericht verzenden en beheren van adressenlijsten.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2015-08-01-schema voorbeeldversie. 

U kunt aan de slag met het maken van een app logica nu, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De verbindingslijn SendGrid kan worden gebruikt als een actie; trigger(s) heeft. Alle verbindingslijnen ondersteuning voor gegevens in JSON en XML-indelingen. 

 De verbindingslijn SendGrid heeft de volgende acties beschikbaar. Er zijn geen triggers.

### <a name="sendgrid-actions"></a>SendGrid acties
U kunt deze of meerdere acties uitvoeren:

|Actie|Beschrijving|
|--- | ---|
|[SendEmail](connectors-create-api-sendgrid.md#sendemail)|Stuurt een e-mailbericht SendGrid-API (beperkt tot 10.000 geadresseerden) gebruiken|
|[AddRecipientToList](connectors-create-api-sendgrid.md#addrecipienttolist)|Een individuele geadresseerde toevoegen aan een lijst met geadresseerden|


## <a name="create-a-connection-to-sendgrid"></a>Een verbinding maken met SendGrid
Als u wilt maken logica apps met SendGrid, moet u eerst een **verbinding** maken en de details bieden voor de volgende eigenschappen: 

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|ApiKey|Ja|Geef uw SendGrid Api-sleutel|
 

>[AZURE.INCLUDE [Steps to create a connection to SendGrid](../../includes/connectors-create-api-sendgrid.md)]

>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

Nadat u de verbinding hebt gemaakt, kunt u deze uit te voeren van de acties en luisteren naar de triggers in dit artikel beschreven.

## <a name="reference-for-sendgrid"></a>Overzicht van SendGrid
Dit geldt voor versie: 1.0

## <a name="sendemail"></a>SendEmail
E-mail verzenden: stuurt een e-mailbericht SendGrid-API (beperkt tot 10.000 geadresseerden) gebruiken 

```POST: /api/mail.send.json``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|aanvraag| |Ja|hoofdtekst|geen|E-mailbericht verzenden|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|429|Te veel aanvraag|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="addrecipienttolist"></a>AddRecipientToList
Geadresseerde toevoegen aan lijst: een individuele geadresseerde toevoegen aan een lijst met geadresseerden 

```POST: /v3/contactdb/lists/{listId}/recipients/{recipientId}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|listId|tekenreeks|Ja|pad|geen|Unieke id van de lijst met geadresseerden|
|recipientId|tekenreeks|Ja|pad|geen|Unieke id van de geadresseerde|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="object-definitions"></a>Objectdefinities 

### <a name="emailrequest"></a>EmailRequest


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Van|tekenreeks|Ja |
|FromName|tekenreeks|Nee |
|Aan|tekenreeks|Ja |
|aannaam|tekenreeks|Nee |
|Onderwerp|tekenreeks|Ja |
|hoofdtekst|tekenreeks|Ja |
|ishtml|Booleaanse waarde|Nee |
|CC|tekenreeks|Nee |
|ccnaam|tekenreeks|Nee |
|BCC|tekenreeks|Nee |
|bccnaam|tekenreeks|Nee |
|ReplyTo|tekenreeks|Nee |
|datum|tekenreeks|Nee |
|kopteksten|tekenreeks|Nee |
|bestanden|matrix|Nee |
|bestandsnamen|matrix|Nee |



### <a name="emailresponse"></a>EmailResponse


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Bericht|tekenreeks|Nee |



### <a name="recipientlists"></a>RecipientLists


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|lijsten|matrix|Nee |



### <a name="recipientlist"></a>RecipientList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|naam|tekenreeks|Nee |
|recipient_count|geheel getal|Nee |



### <a name="recipients"></a>Geadresseerden


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|geadresseerden|matrix|Nee |



### <a name="recipient"></a>Geadresseerde


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|E-mail|tekenreeks|Nee |
|Achternaam|tekenreeks|Nee |
|Voornaam|tekenreeks|Nee |
|ID|tekenreeks|Nee |


## <a name="next-steps"></a>Volgende stappen
[Een logica-app maken](../app-service-logic/app-service-logic-create-a-logic-app.md)