<properties
pageTitle="Outlook.com | Microsoft Azure"
description="Logica apps maken met de App Azure-service. Outlook.com-connector, kunt u voor het beheren van uw e-mail, agenda's en contactpersonen. U kunt verschillende bewerkingen zoals verzenden e-mailberichten, vergaderingen plannen, contactpersonen enzovoort toevoegen."
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

# <a name="get-started-with-the-outlookcom-connector"></a>Aan de slag met de Outlook.com-connector

Outlook.com-connector, kunt u voor het beheren van uw e-mail, agenda's en contactpersonen. U kunt verschillende bewerkingen zoals verzenden e-mailberichten, vergaderingen plannen, contactpersonen enzovoort toevoegen.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2015-08-01-schema voorbeeldversie. 

U kunt aan de slag met het maken van een app logica nu, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De verbindingslijn Outlook.com kan worden gebruikt als een actie; trigger(s) heeft. Alle verbindingslijnen ondersteuning voor gegevens in JSON en XML-indelingen. 

 De verbindingslijn Outlook.com heeft de volgende of meerdere acties en/of trigger(s) beschikbaar:

### <a name="outlookcom-actions"></a>Outlook.com-acties
U kunt deze of meerdere acties uitvoeren:

|Actie|Beschrijving|
|--- | ---|
|[GetEmails](connectors-create-api-outlook.md#GetEmails)|E-mailberichten opgehaald uit een map|
|[SendEmail](connectors-create-api-outlook.md#SendEmail)|Een e-mailbericht verzendt|
|[DeleteEmail](connectors-create-api-outlook.md#DeleteEmail)|Hiermee verwijdert u een e-mailbericht op id|
|[MarkAsRead](connectors-create-api-outlook.md#MarkAsRead)|Hiermee markeert u een e-mailbericht als gelezen|
|[ReplyTo](connectors-create-api-outlook.md#ReplyTo)|Hiermee beantwoordt u een e-mailbericht|
|[GetAttachment](connectors-create-api-outlook.md#GetAttachment)|Haalt e-mailbijlage op id|
|[SendMailWithOptions](connectors-create-api-outlook.md#SendMailWithOptions)|Stuur een e-mail met meerdere opties en wacht totdat de geadresseerde te beantwoorden met een van de opties|
|[SendApprovalMail](connectors-create-api-outlook.md#SendApprovalMail)|Stuur een e-mail goedkeuring en wacht op een reactie van de geadresseerde|
|[CalendarGetTables](connectors-create-api-outlook.md#CalendarGetTables)|Agenda's zijn opgehaald|
|[CalendarGetItems](connectors-create-api-outlook.md#CalendarGetItems)|Items opgehaald uit een agenda|
|[CalendarPostItem](connectors-create-api-outlook.md#CalendarPostItem)|Hiermee maakt u een nieuwe gebeurtenis|
|[CalendarGetItem](connectors-create-api-outlook.md#CalendarGetItem)|Een specifiek item opgehaald uit een agenda|
|[CalendarDeleteItem](connectors-create-api-outlook.md#CalendarDeleteItem)|Hiermee verwijdert u een agenda-item|
|[CalendarPatchItem](connectors-create-api-outlook.md#CalendarPatchItem)|Een agenda-item gedeeltelijk wordt bijgewerkt|
|[ContactGetTables](connectors-create-api-outlook.md#ContactGetTables)|Mappen met contactpersonen zijn opgehaald|
|[ContactGetItems](connectors-create-api-outlook.md#ContactGetItems)|Contactpersonen opgehaald uit een map met contactpersonen|
|[ContactPostItem](connectors-create-api-outlook.md#ContactPostItem)|Hiermee maakt u een nieuwe contactpersoon|
|[ContactGetItem](connectors-create-api-outlook.md#ContactGetItem)|Een specifieke contactpersoon opgehaald uit een map met contactpersonen|
|[ContactDeleteItem](connectors-create-api-outlook.md#ContactDeleteItem)|Hiermee verwijdert u een contactpersoon|
|[ContactPatchItem](connectors-create-api-outlook.md#ContactPatchItem)|Een contactpersoon gedeeltelijk wordt bijgewerkt|
### <a name="outlookcom-triggers"></a>Outlook.com-triggers
U kunt luisteren voor deze gebeurtenis(sen):

|Trigger | Beschrijving|
|--- | ---|
|Klik op gebeurtenis snel starten|Een stroom gebeurtenis wanneer een komende agenda-item wordt gestart|
|Klik op nieuwe e-mail|Een stroom activeert wanneer er een nieuwe e-mail binnenkomt|
|Klik op nieuwe items|Geactiveerd wanneer een nieuw agenda-item wordt gemaakt|
|Klik op de bijgewerkte items|Gestart wanneer een agenda-item is gewijzigd|


## <a name="create-a-connection-to-outlookcom"></a>Een verbinding maken met Outlook.com
Als u wilt logica apps maken met Outlook.com, moet u eerst een **verbinding** maken en de details bieden voor de volgende eigenschappen: 

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|Token|Ja|Outlook.com-referenties opgeven|
Nadat u de verbinding hebt gemaakt, kunt u deze uit te voeren van de acties en luisteren naar de triggers in dit artikel beschreven.

>[AZURE.INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)] 

>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.  

## <a name="reference-for-outlookcom"></a>Overzicht van Outlook.com
Dit geldt voor versie: 1.0

## <a name="onupcomingevents"></a>OnUpcomingEvents
Klik op gebeurtenis snel starten: een stroom gebeurtenis wanneer een komende agenda-item wordt gestart 

```GET: /Events/OnUpcomingEvents``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|query|geen|Unieke id van de agenda|
|lookAheadTimeInMinutes|geheel getal|geen|query|15|Tijd (in minuten) om te zoeken vooruit aanstaande evenementen|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|202|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


## <a name="getemails"></a>GetEmails
E-mailberichten ophalen: e-mailberichten opgehaald uit een map 

```GET: /Mail``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Mappad|tekenreeks|geen|query|Postvak in|Pad van de map om op te halen e-mailberichten (standaard: 'Postvak in')|
|Boven|geheel getal|geen|query|10|Aantal e-mailberichten om op te halen (standaard: 10)|
|fetchOnlyUnread|Booleaanse waarde|geen|query|waar|Alleen ongelezen e-mailberichten ophalen?|
|includeAttachments|Booleaanse waarde|geen|query|ONWAAR|Als u ook worden ingesteld op waar, bijlagen ingelezen samen met het e-mailbericht|
|searchQuery|tekenreeks|geen|query|geen|Zoekopdracht naar e-mailberichten filteren|
|overslaan|geheel getal|geen|query|0|Aantal e-mailberichten om over te slaan (standaard: 0)|
|skipToken|tekenreeks|geen|query|geen|Token gaat u verder met de nieuwe pagina ophalen|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


## <a name="sendemail"></a>SendEmail
E-mail verzenden: een e-mailbericht verzendt 

```POST: /Mail``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Er| |Ja|hoofdtekst|geen|E-mail|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


## <a name="deleteemail"></a>DeleteEmail
E-mail verwijderen: Hiermee verwijdert u een e-mailbericht op id 

```DELETE: /Mail/{messageId}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|messageId|tekenreeks|Ja|pad|geen|Id van het e-mailbericht verwijderen|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


## <a name="markasread"></a>MarkAsRead
Markeren als gelezen: Hiermee markeert u een e-mailbericht als gelezen 

```POST: /Mail/MarkAsRead/{messageId}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|messageId|tekenreeks|Ja|pad|geen|Id van het e-mailbericht worden gemarkeerd als gelezen|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


## <a name="replyto"></a>ReplyTo
Beantwoorden met e-mail: Hiermee beantwoordt u een e-mailbericht 

```POST: /Mail/ReplyTo/{messageId}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|messageId|tekenreeks|Ja|pad|geen|Id van het e-mailbericht beantwoorden|
|Opmerking|tekenreeks|Ja|query|geen|Opmerking beantwoorden|
|Allen beantwoorden|Booleaanse waarde|geen|query|ONWAAR|Reageren op alle geadresseerden|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


## <a name="getattachment"></a>GetAttachment
Bijlage ophalen: haalt e-mailbijlage op id 

```GET: /Mail/{messageId}/Attachments/{attachmentId}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|messageId|tekenreeks|Ja|pad|geen|Id van het e-mailbericht|
|attachmentId|tekenreeks|Ja|pad|geen|Id van de bijlage downloaden|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


## <a name="onnewemail"></a>OnNewEmail
Klik op nieuwe e-mail: een stroom activeert wanneer er een nieuwe e-mail binnenkomt 

```GET: /Mail/OnNewEmail``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Mappad|tekenreeks|geen|query|Postvak in|E-mailmap om op te halen (standaard: Postvak in)|
|Aan|tekenreeks|geen|query|geen|Geadresseerden e-mailadressen|
|Van|tekenreeks|geen|query|geen|Van adres|
|urgentie|tekenreeks|geen|query|Normaal|Belang van het e-mailbericht (hoog, normaal, laag) (standaard: normaal)|
|fetchOnlyWithAttachment|Booleaanse waarde|geen|query|ONWAAR|Alleen e-mailberichten met een bijlage ophalen|
|includeAttachments|Booleaanse waarde|geen|query|ONWAAR|Bijlagen|
|subjectFilter|tekenreeks|geen|query|geen|Tekenreeks moet worden gezocht in het onderwerp|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|202|Geaccepteerd|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


## <a name="sendmailwithoptions"></a>SendMailWithOptions
E-mailbericht met opties verzenden: stuur een e-mail met meerdere opties en wacht totdat de geadresseerde te beantwoorden met een van de opties 

```POST: /mailwithoptions/$subscriptions``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|optionsEmailSubscription| |Ja|hoofdtekst|geen|Abonnement verzoek voor de opties voor e-mail|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|201|Abonnement dat is gemaakt|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


## <a name="sendapprovalmail"></a>SendApprovalMail
Goedkeuring van e-mailbericht verzenden: een goedkeuringswerkstroom-e-mailbericht verzenden en wacht op een reactie van de geadresseerde 

```POST: /approvalmail/$subscriptions``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|approvalEmailSubscription| |Ja|hoofdtekst|geen|Abonnement verzoek om goedkeuring van e-mail|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|201|Abonnement dat is gemaakt|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


## <a name="calendargettables"></a>CalendarGetTables
Ophalen van agenda's: agenda's zijn opgehaald 

```GET: /datasets/calendars/tables``` 

Er zijn geen parameters voor dit gesprek
#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="calendargetitems"></a>CalendarGetItems
Gebeurtenissen ophalen: items opgehaald uit een agenda 

```GET: /datasets/calendars/tables/{table}/items``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|Unieke id van de agenda om op te halen|
|$filter|tekenreeks|geen|query|geen|Een query voor het filter van ODATA om het aantal items te beperken|
|$orderby|tekenreeks|geen|query|geen|Een query ODATA orderBy om aan te geven van de volgorde van items|
|$skip|geheel getal|geen|query|geen|Aantal vermeldingen om over te slaan (standaard = 0)|
|$top|geheel getal|geen|query|geen|Maximum aantal vermeldingen om op te halen (standaard = 256)|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="calendarpostitem"></a>CalendarPostItem
Gebeurtenis maken: Hiermee maakt u een nieuwe gebeurtenis 

```POST: /datasets/calendars/tables/{table}/items``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|Unieke id van een agenda|
|item| |Ja|hoofdtekst|geen|Agenda-item maken|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="calendargetitem"></a>CalendarGetItem
Gebeurtenis ophalen: een specifiek item opgehaald uit een agenda 

```GET: /datasets/calendars/tables/{table}/items/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|Unieke id van een agenda|
|ID|tekenreeks|Ja|pad|geen|Unieke id van een agenda-item om op te halen|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="calendardeleteitem"></a>CalendarDeleteItem
Gebeurtenis verwijderen: Hiermee verwijdert u een agenda-item 

```DELETE: /datasets/calendars/tables/{table}/items/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|Unieke id van een agenda|
|ID|tekenreeks|Ja|pad|geen|Unieke id van agenda-item verwijderen|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="calendarpatchitem"></a>CalendarPatchItem
Update gebeurtenis: een agenda-item gedeeltelijk wordt bijgewerkt 

```PATCH: /datasets/calendars/tables/{table}/items/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|Unieke id van een agenda|
|ID|tekenreeks|Ja|pad|geen|Unieke id van agenda-item wilt bijwerken|
|item| |Ja|hoofdtekst|geen|Agenda-item wilt bijwerken|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="calendargetonnewitems"></a>CalendarGetOnNewItems
Klik op nieuwe items: geactiveerd wanneer een nieuw agenda-item wordt gemaakt 

```GET: /datasets/calendars/tables/{table}/onnewitems``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|Unieke id van een agenda|
|$filter|tekenreeks|geen|query|geen|Een query voor het filter van ODATA om het aantal items te beperken|
|$orderby|tekenreeks|geen|query|geen|Een query ODATA orderBy om aan te geven van de volgorde van items|
|$skip|geheel getal|geen|query|geen|Aantal vermeldingen om over te slaan (standaard = 0)|
|$top|geheel getal|geen|query|geen|Maximum aantal vermeldingen om op te halen (standaard = 256)|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="calendargetonupdateditems"></a>CalendarGetOnUpdatedItems
Artikelen op bijgewerkt: gestart wanneer een agenda-item is gewijzigd 

```GET: /datasets/calendars/tables/{table}/onupdateditems``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|Unieke id van een agenda|
|$filter|tekenreeks|geen|query|geen|Een query voor het filter van ODATA om het aantal items te beperken|
|$orderby|tekenreeks|geen|query|geen|Een query ODATA orderBy om aan te geven van de volgorde van items|
|$skip|geheel getal|geen|query|geen|Aantal vermeldingen om over te slaan (standaard = 0)|
|$top|geheel getal|geen|query|geen|Maximum aantal vermeldingen om op te halen (standaard = 256)|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="contactgettables"></a>ContactGetTables
Mappen met contactpersonen ophalen: haalt mappen met contactpersonen 

```GET: /datasets/contacts/tables``` 

Er zijn geen parameters voor dit gesprek
#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="contactgetitems"></a>ContactGetItems
Contactpersonen ophalen: contactpersonen opgehaald uit een map met contactpersonen 

```GET: /datasets/contacts/tables/{table}/items``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|Unieke id van de map met contactpersonen om op te halen|
|$filter|tekenreeks|geen|query|geen|Een query voor het filter van ODATA om het aantal items te beperken|
|$orderby|tekenreeks|geen|query|geen|Een query ODATA orderBy om aan te geven van de volgorde van items|
|$skip|geheel getal|geen|query|geen|Aantal vermeldingen om over te slaan (standaard = 0)|
|$top|geheel getal|geen|query|geen|Maximum aantal vermeldingen om op te halen (standaard = 256)|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="contactpostitem"></a>ContactPostItem
Contactpersoon maken: Hiermee maakt u een nieuwe contactpersoon 

```POST: /datasets/contacts/tables/{table}/items``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|Unieke id van een map met contactpersonen|
|item| |Ja|hoofdtekst|geen|Contactpersoon maken|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="contactgetitem"></a>ContactGetItem
Krijgt contactpersoon: een specifieke contactpersoon opgehaald uit een map met contactpersonen 

```GET: /datasets/contacts/tables/{table}/items/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|Unieke id van een map met contactpersonen|
|ID|tekenreeks|Ja|pad|geen|Unieke id van een contactpersoon om op te halen|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="contactdeleteitem"></a>ContactDeleteItem
Contactpersoon verwijderen: Hiermee verwijdert u een contactpersoon 

```DELETE: /datasets/contacts/tables/{table}/items/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|Unieke id van een map met contactpersonen|
|ID|tekenreeks|Ja|pad|geen|Unieke id van contactpersoon verwijderen|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="contactpatchitem"></a>ContactPatchItem
Contactpersoon bijwerken: een contactpersoon gedeeltelijk wordt bijgewerkt 

```PATCH: /datasets/contacts/tables/{table}/items/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|tabel|tekenreeks|Ja|pad|geen|Unieke id van een map met contactpersonen|
|ID|tekenreeks|Ja|pad|geen|Unieke id van contactpersoon bijwerken|
|item| |Ja|hoofdtekst|geen|Neem contact op met item als u wilt bijwerken|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|Standaard|Bewerking is mislukt.|


## <a name="object-definitions"></a>Objectdefinities 

### <a name="triggerbatchresponseidictionarystringobject"></a>TriggerBatchResponse [IDictionary [tekenreeks, Object]]


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="object"></a>Object


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|



### <a name="sendmessage"></a>SendMessage


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Bijlagen|matrix|Nee |
|Van|tekenreeks|Nee |
|CC|tekenreeks|Nee |
|BCC|tekenreeks|Nee |
|Onderwerp|tekenreeks|Ja |
|Hoofdtekst|tekenreeks|Ja |
|Urgentie|tekenreeks|Nee |
|IsHtml|Booleaanse waarde|Nee |
|Aan|tekenreeks|Ja |



### <a name="sendattachment"></a>SendAttachment


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|@odata.type|tekenreeks|Nee |
|Naam|tekenreeks|Ja |
|ContentBytes|tekenreeks|Ja |



### <a name="receivemessage"></a>ReceiveMessage


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Nee |
|Isgelezen|Booleaanse waarde|Nee |
|Heeftbijlage|Booleaanse waarde|Nee |
|DateTimeReceived|tekenreeks|Nee |
|Bijlagen|matrix|Nee |
|Van|tekenreeks|Nee |
|CC|tekenreeks|Nee |
|BCC|tekenreeks|Nee |
|Onderwerp|tekenreeks|Ja |
|Hoofdtekst|tekenreeks|Ja |
|Urgentie|tekenreeks|Nee |
|IsHtml|Booleaanse waarde|Nee |
|Aan|tekenreeks|Ja |



### <a name="receiveattachment"></a>ReceiveAttachment


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Ja |
|ContentType|tekenreeks|Ja |
|@odata.type|tekenreeks|Nee |
|Naam|tekenreeks|Ja |
|ContentBytes|tekenreeks|Ja |



### <a name="digestmessage"></a>DigestMessage


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Onderwerp|tekenreeks|Ja |
|Hoofdtekst|tekenreeks|Nee |
|Urgentie|tekenreeks|Nee |
|Overzicht|matrix|Ja |
|Bijlagen|matrix|Nee |
|Aan|tekenreeks|Ja |



### <a name="triggerbatchresponsereceivemessage"></a>TriggerBatchResponse [ReceiveMessage]


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="datasetsmetadata"></a>DataSetsMetadata


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|in tabelvorm|niet gedefinieerd|Nee |
|BLOB|niet gedefinieerd|Nee |



### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|bron|tekenreeks|Nee |
|Weergavenaam|tekenreeks|Nee |
|urlEncoding|tekenreeks|Nee |
|tableDisplayName|tekenreeks|Nee |
|tablePluralName|tekenreeks|Nee |



### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|bron|tekenreeks|Nee |
|Weergavenaam|tekenreeks|Nee |
|urlEncoding|tekenreeks|Nee |



### <a name="tablemetadata"></a>TableMetadata


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|naam|tekenreeks|Nee |
|titel|tekenreeks|Nee |
|x-ms-machtigingen|tekenreeks|Nee |
|x-ms-mogelijkheden|niet gedefinieerd|Nee |
|schema|niet gedefinieerd|Nee |



### <a name="tablecapabilitiesmetadata"></a>TableCapabilitiesMetadata


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|sortRestrictions|niet gedefinieerd|Nee |
|filterRestrictions|niet gedefinieerd|Nee |
|filterFunctions|matrix|Nee |



### <a name="tablesortrestrictionsmetadata"></a>TableSortRestrictionsMetadata


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|sorteerbaar|Booleaanse waarde|Nee |
|unsortableProperties|matrix|Nee |
|ascendingOnlyProperties|matrix|Nee |



### <a name="tablefilterrestrictionsmetadata"></a>TableFilterRestrictionsMetadata


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|filterbare|Booleaanse waarde|Nee |
|nonFilterableProperties|matrix|Nee |
|requiredProperties|matrix|Nee |



### <a name="optionsemailsubscription"></a>OptionsEmailSubscription


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|NotificationUrl|tekenreeks|Nee |
|Bericht|niet gedefinieerd|Nee |



### <a name="messagewithoptions"></a>MessageWithOptions


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Onderwerp|tekenreeks|Ja |
|Opties|tekenreeks|Ja |
|Hoofdtekst|tekenreeks|Nee |
|Urgentie|tekenreeks|Nee |
|Bijlagen|matrix|Nee |
|Aan|tekenreeks|Ja |



### <a name="subscriptionresponse"></a>SubscriptionResponse


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Nee |
|resource|tekenreeks|Nee |
|notificationType|tekenreeks|Nee |
|notificationUrl|tekenreeks|Nee |



### <a name="approvalemailsubscription"></a>ApprovalEmailSubscription


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|NotificationUrl|tekenreeks|Nee |
|Bericht|niet gedefinieerd|Nee |



### <a name="approvalmessage"></a>ApprovalMessage


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Onderwerp|tekenreeks|Ja |
|Opties|tekenreeks|Ja |
|Hoofdtekst|tekenreeks|Nee |
|Urgentie|tekenreeks|Nee |
|Bijlagen|matrix|Nee |
|Aan|tekenreeks|Ja |



### <a name="approvalemailresponse"></a>ApprovalEmailResponse


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|SelectedOption|tekenreeks|Nee |



### <a name="tableslist"></a>TablesList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="table"></a>Tabel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Naam|tekenreeks|Nee |
|Weergavenaam|tekenreeks|Nee |



### <a name="item"></a>Item


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ItemInternalId|tekenreeks|Nee |



### <a name="calendaritemslist"></a>CalendarItemsList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="calendaritem"></a>CalendarItem


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ItemInternalId|tekenreeks|Nee |



### <a name="contactitemslist"></a>ContactItemsList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="contactitem"></a>ContactItem


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ItemInternalId|tekenreeks|Nee |



### <a name="datasetslist"></a>DataSetsList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="dataset"></a>Gegevensset


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Naam|tekenreeks|Nee |
|Weergavenaam|tekenreeks|Nee |


## <a name="next-steps"></a>Volgende stappen
[Een logica-app maken](../app-service-logic/app-service-logic-create-a-logic-app.md)