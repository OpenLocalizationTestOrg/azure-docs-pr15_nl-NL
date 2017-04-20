<properties
pageTitle="Wunderlist | Microsoft Azure"
description="Logica apps maken met de App Azure-service. Wunderlist vindt u een taak lijst en taak manager zodat anderen kunnen hun zaken voor elkaar krijgt.  Of u een boodschappenlijst met een hun naam deelt, kunt werken aan een project of een vakantie, plant Wunderlist u gemakkelijk kunt vastleggen, delen en uw to¬dos voltooien. Wunderlist direct worden gesynchroniseerd tussen uw telefoon, tablet en computer, zodat u toegang hebt tot alle taken vanaf elke locatie."
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

# <a name="get-started-with-the-wunderlist-connector"></a>Aan de slag met de verbindingslijn Wunderlist

Wunderlist vindt u een taak lijst en taak manager zodat anderen kunnen hun zaken voor elkaar krijgt.  Of u een boodschappenlijst met een hun naam deelt, kunt werken aan een project of een vakantie, plant Wunderlist u gemakkelijk kunt vastleggen, delen en uw to¬dos voltooien. Wunderlist direct worden gesynchroniseerd tussen uw telefoon, tablet en computer, zodat u toegang hebt tot alle taken vanaf elke locatie.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2015-08-01-schema voorbeeldversie. 

U kunt aan de slag met het maken van een app logica nu, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De verbindingslijn Wunderlist kan worden gebruikt als een actie; trigger(s) heeft. Alle verbindingslijnen ondersteuning voor gegevens in JSON en XML-indelingen. 

 De verbindingslijn Wunderlist heeft de volgende of meerdere acties en/of trigger(s) beschikbaar:

### <a name="wunderlist-actions"></a>Wunderlist acties
U kunt deze of meerdere acties uitvoeren:

|Actie|Beschrijving|
|--- | ---|
|[RetrieveLists](connectors-create-api-wunderlist.md#retrievelists)|Hiermee kunt u de lijsten die zijn gekoppeld aan uw account ophalen.|
|[CreateList](connectors-create-api-wunderlist.md#createlist)|Een lijst maken.|
|[ListTasks](connectors-create-api-wunderlist.md#listtasks)|Taken worden opgehaald uit een bepaalde lijst.|
|[CreateTask](connectors-create-api-wunderlist.md#createtask)|Een taak maken|
|[ListSubTasks](connectors-create-api-wunderlist.md#listsubtasks)|Subtaken ophalen vanuit een specifieke lijst of een specifieke taak.|
|[CreateSubTask](connectors-create-api-wunderlist.md#createsubtask)|Een subtaak binnen een bepaalde taak maken|
|[ListNotes](connectors-create-api-wunderlist.md#listnotes)|Notities voor een specifieke lijst of een bepaalde taak ophalen.|
|[CreateNote](connectors-create-api-wunderlist.md#createnote)|Een notitie toevoegen aan een specifieke taak|
|[ListComments](connectors-create-api-wunderlist.md#listcomments)|Taak opmerkingen voor een specifieke lijst of een bepaalde taak ophalen.|
|[CreateComment](connectors-create-api-wunderlist.md#createcomment)|Een opmerking toevoegen aan een specifieke taak|
|[RetrieveReminders](connectors-create-api-wunderlist.md#retrievereminders)|Hiermee kunt u herinneringen voor een specifieke lijst of een bepaalde taak ophalen.|
|[CreateReminder](connectors-create-api-wunderlist.md#createreminder)|Een herinnering instellen.|
|[RetrieveFiles](connectors-create-api-wunderlist.md#retrievefiles)|Bestanden voor een specifieke lijst of een bepaalde taak ophalen.|
|[GetList](connectors-create-api-wunderlist.md#getlist)|Een specifieke lijst haalt|
|[DeleteList](connectors-create-api-wunderlist.md#deletelist)|Hiermee verwijdert u een lijst|
|[Een UpdateList](connectors-create-api-wunderlist.md#updatelist)|Een specifieke lijst bijwerken|
|[GetTask](connectors-create-api-wunderlist.md#gettask)|Een specifieke taak haalt|
|[UpdateTask](connectors-create-api-wunderlist.md#updatetask)|Een specifieke taak bijwerkt|
|[DeleteTask](connectors-create-api-wunderlist.md#deletetask)|Hiermee verwijdert u een specifieke taak|
|[GetSubTask](connectors-create-api-wunderlist.md#getsubtask)|Een specifieke subtaak haalt|
|[UpdateSubTask](connectors-create-api-wunderlist.md#updatesubtask)|Een specifieke subtaak wordt bijgewerkt|
|[DeleteSubTask](connectors-create-api-wunderlist.md#deletesubtask)|Hiermee verwijdert u een specifieke subtaak|
|[GetNote](connectors-create-api-wunderlist.md#getnote)|Een specifieke notitie ophalen|
|[UpdateNote](connectors-create-api-wunderlist.md#updatenote)|Een specifieke notitie bijwerken|
|[DeleteNote](connectors-create-api-wunderlist.md#deletenote)|Een specifieke notitie verwijderen|
|[GetComment](connectors-create-api-wunderlist.md#getcomment)|Een specifieke taak Opmerking ophalen|
|[UpdateReminder](connectors-create-api-wunderlist.md#updatereminder)|Een specifieke herinnering bijwerken|
|[DeleteReminder](connectors-create-api-wunderlist.md#deletereminder)|Een specifieke herinnering verwijderen|
### <a name="wunderlist-triggers"></a>Wunderlist-triggers
U kunt luisteren voor deze gebeurtenis(sen):

|Trigger | Beschrijving|
|--- | ---|
|Wanneer een taak moet zijn voltooid|Een nieuwe stroom gebeurtenis wanneer een taak in de lijst moet voltooid zijn|
|Wanneer een nieuwe taak is gemaakt|Een nieuwe stroom activeert als een nieuwe taak in de lijst wordt gemaakt|
|Wanneer een herinnering|Een nieuwe stroom activeert als een herinnering|


## <a name="create-a-connection-to-wunderlist"></a>Een verbinding maken met Wunderlist
Als u wilt maken logica apps met Wunderlist, moet u eerst een **verbinding** maken en de details bieden voor de volgende eigenschappen: 

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|Token|Ja|Wunderlist referenties opgeven|
Nadat u de verbinding hebt gemaakt, kunt u deze uit te voeren van de acties en luisteren naar de triggers in dit artikel beschreven. 


>[AZURE.INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)] 


>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

## <a name="reference-for-wunderlist"></a>Overzicht van Wunderlist
Dit geldt voor versie: 1.0

## <a name="triggertaskdue"></a>TriggerTaskDue
Wanneer een taak moet zijn voltooid: de stroom van een nieuwe gebeurtenis wanneer een taak in de lijst moet voltooid zijn 

```GET: /trigger/tasksdue``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst ID|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|


## <a name="triggertasknew"></a>TriggerTaskNew
Wanneer een nieuwe taak is gemaakt: een nieuwe stroom activeert als een nieuwe taak in de lijst wordt gemaakt 

```GET: /trigger/tasksnew``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst ID|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|


## <a name="triggerreminder"></a>TriggerReminder
Wanneer een herinnering: een nieuwe stroom activeert als een herinnering 

```GET: /trigger/reminders``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst ID|
|TASK_ID|geheel getal|geen|query|geen|Taak-ID|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|


## <a name="retrievelists"></a>RetrieveLists
Lijsten ophalen: ophalen van de lijsten die zijn gekoppeld aan uw account. 

```GET: /lists``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|Ongeldige aanvraag|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="createlist"></a>CreateList
Een lijst maken: een lijst maken. 

```POST: /lists``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Verzenden| |Ja|hoofdtekst|geen|Nieuwe lijst wilt maken|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|Standaard|Bewerking is mislukt.|


## <a name="listtasks"></a>ListTasks
Taken ophalen: taken ophalen uit een bepaalde lijst. 

```GET: /tasks``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst ID|
|voltooid|Booleaanse waarde|geen|query|geen|Voltooid|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|Ongeldige aanvraag|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="createtask"></a>CreateTask
Een taak maken: een taak maken 

```POST: /tasks``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Verzenden| |Ja|hoofdtekst|geen|Nieuwe taak maken|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|201|Gemaakt|


## <a name="listsubtasks"></a>ListSubTasks
Subtaken: subtaken ophalen uit een bepaalde lijst of van een specifieke taak. 

```GET: /subtasks``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst ID|
|TASK_ID|geheel getal|geen|query|geen|Taak-ID|
|voltooid|Booleaanse waarde|geen|query|geen|Voltooid|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|Ongeldige aanvraag|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="createsubtask"></a>CreateSubTask
Een subtaak maken: een subtaak binnen een bepaalde taak maken 

```POST: /subtasks``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Verzenden| |Ja|hoofdtekst|geen|Nieuwe subtaak moet worden gemaakt|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|201|Gemaakt|


## <a name="listnotes"></a>ListNotes
Notities ophalen: ophalen van notities voor een specifieke lijst of een specifieke taak. 

```GET: /notes``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst ID|
|TASK_ID|geheel getal|geen|query|geen|Taak-ID|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|Ongeldige aanvraag|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="createnote"></a>CreateNote
Een notitie maken: een notitie toevoegen aan een specifieke taak 

```POST: /notes``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Verzenden| |Ja|hoofdtekst|geen|Nieuwe notitie te maken|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|201|Gemaakt|


## <a name="listcomments"></a>ListComments
Taak opmerkingen: ophalen van de taak opmerkingen voor een specifieke lijst of een specifieke taak. 

```GET: /task_comments``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst ID|
|TASK_ID|geheel getal|geen|query|geen|Taak-ID|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|Ongeldige aanvraag|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="createcomment"></a>CreateComment
Een opmerking toevoegen aan een taak: een opmerking toevoegen aan een specifieke taak 

```POST: /task_comments``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Verzenden| |Ja|hoofdtekst|geen|Nieuwe taak opmerking moet worden gemaakt|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|201|Gemaakt|


## <a name="retrievereminders"></a>RetrieveReminders
Herinneringen ophalen: ophalen herinneringen voor een specifieke lijst of een specifieke taak. 

```GET: /reminders``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst ID|
|TASK_ID|geheel getal|geen|query|geen|Taak-ID|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|Ongeldige aanvraag|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="createreminder"></a>CreateReminder
Een herinnering instellen: een herinnering instellen. 

```POST: /reminders``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|Verzenden| |Ja|hoofdtekst|geen|Nieuwe herinnering moet worden gemaakt|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|Standaard|Bewerking is mislukt.|


## <a name="retrievefiles"></a>RetrieveFiles
Bestanden ophalen: ophalen van bestanden voor een specifieke lijst of een specifieke taak. 

```GET: /files``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst ID|
|TASK_ID|geheel getal|geen|query|geen|Taak-ID|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|400|Ongeldige aanvraag|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="getlist"></a>GetList
Lijst opvragen: haalt een specifieke lijst 

```GET: /lists/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad|geen|Lijst ID|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="deletelist"></a>DeleteList
Lijst verwijderen: Hiermee verwijdert u een lijst 

```DELETE: /lists/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|Lijst ID|
|revisie|geheel getal|Ja|query|geen|Revisie|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|204|Geen inhoud|


## <a name="updatelist"></a>Een UpdateList
Een lijst bijwerken: een specifieke lijst bijwerken 

```PATCH: /lists/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|Lijst ID|
|Verzenden| |Ja|hoofdtekst|geen|Lijstdetails|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="gettask"></a>GetTask
Taak: haalt van een bepaalde taak 

```GET: /tasks/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst ID|
|ID|geheel getal|Ja|pad|geen|Taak-ID|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="updatetask"></a>UpdateTask
Een taak bijwerken: een bepaalde taak wordt bijgewerkt 

```PATCH: /tasks/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst ID|
|ID|geheel getal|Ja|pad|geen|Taak-ID|
|Verzenden| |Ja|hoofdtekst|geen|Taakgegevens|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="deletetask"></a>DeleteTask
Taak verwijderen: Hiermee verwijdert u een specifieke taak 

```DELETE: /tasks/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|geheel getal|Ja|query|geen|Lijst ID|
|ID|geheel getal|Ja|pad|geen|Taak-ID|
|revisie|geheel getal|Ja|query|geen|Revisie|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|204|Geen inhoud|


## <a name="getsubtask"></a>GetSubTask
Subtaak ophalen: haalt u een specifieke subtaak 

```GET: /subtasks/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad|geen|Subtaak-ID|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="updatesubtask"></a>UpdateSubTask
Een subtaak bijwerken: een specifieke subtaak wordt bijgewerkt 

```PATCH: /subtasks/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|Subtaak-ID|
|Verzenden| |Ja|hoofdtekst|geen|Subtaak details|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="deletesubtask"></a>DeleteSubTask
Een subtaak verwijderen: Hiermee verwijdert u een specifieke subtaak 

```DELETE: /subtasks/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|Subtaak-ID|
|revisie|geheel getal|Ja|query|geen|Revisie|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|204|Geen inhoud|


## <a name="getnote"></a>GetNote
Een notitie ophalen: een specifieke notitie ophalen 

```GET: /notes/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad|geen|Notitie-ID|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="updatenote"></a>UpdateNote
Een notitie bijwerken: een specifieke notitie bijwerken 

```PATCH: /notes/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|Notitie-ID|
|Verzenden| |Ja|hoofdtekst|geen|Notitie-details|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="deletenote"></a>DeleteNote
Een notitie verwijderen: een specifieke notitie verwijderen 

```DELETE: /notes/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|Notitie-ID|
|revisie|geheel getal|Ja|query|geen|Revisie|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|204|Geen inhoud|


## <a name="getcomment"></a>GetComment
Taak Opmerking ophalen: ophalen van een bepaalde taak-Opmerking 

```GET: /task_comments/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|tekenreeks|Ja|pad|geen|Opmerking-ID|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="updatereminder"></a>UpdateReminder
Een herinnering bijwerken: een specifieke herinnering bijwerken 

```PATCH: /reminders/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|Herinnering-ID|
|Verzenden| |Ja|hoofdtekst|geen|Details van de herinnering|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|


## <a name="deletereminder"></a>DeleteReminder
Een herinnering verwijderen: een specifieke herinnering verwijderen 

```DELETE: /reminders/{id}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|ID|geheel getal|Ja|pad|geen|ID van de herinnering.|
|revisie|geheel getal|Ja|query|geen|Revisie|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|204|Geen inhoud|


## <a name="object-definitions"></a>Objectdefinities 

### <a name="list"></a>Lijst


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|created_at|tekenreeks|Nee |
|titel|tekenreeks|Nee |
|list_type|tekenreeks|Nee |
|type|tekenreeks|Nee |
|revisie|geheel getal|Nee |



### <a name="createdlist"></a>CreatedList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|created_at|tekenreeks|Nee |
|titel|tekenreeks|Nee |
|revisie|geheel getal|Nee |
|type|tekenreeks|Nee |



### <a name="task"></a>Taak


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|assignee_id|geheel getal|Nee |
|assigner_id|geheel getal|Nee |
|created_at|tekenreeks|Nee |
|created_by_id|geheel getal|Nee |
|due_date|tekenreeks|Nee |
|list_id|geheel getal|Nee |
|revisie|geheel getal|Nee |
|starred|Booleaanse waarde|Nee |
|titel|tekenreeks|Nee |



### <a name="subtask"></a>Subtaak


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|TASK_ID|geheel getal|Nee |
|created_at|tekenreeks|Nee |
|created_by_id|geheel getal|Nee |
|revisie|tekenreeks|Nee |
|titel|tekenreeks|Nee |



### <a name="note"></a>Opmerking


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|TASK_ID|geheel getal|Nee |
|inhoud|tekenreeks|Nee |
|created_at|tekenreeks|Nee |
|updated_at|tekenreeks|Nee |
|revisie|geheel getal|Nee |



### <a name="comment"></a>Opmerking


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|TASK_ID|geheel getal|Nee |
|revisie|geheel getal|Nee |
|tekst|tekenreeks|Nee |
|type|tekenreeks|Nee |
|created_at|tekenreeks|Nee |



### <a name="reminder"></a>Herinnering


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|datum|tekenreeks|Nee |
|TASK_ID|geheel getal|Nee |
|revisie|geheel getal|Nee |
|type|tekenreeks|Nee |
|created_at|tekenreeks|Nee |
|updated_at|tekenreeks|Nee |



### <a name="createdreminder"></a>CreatedReminder


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|datum|tekenreeks|Nee |
|TASK_ID|geheel getal|Nee |
|revisie|geheel getal|Nee |
|created_at|tekenreeks|Nee |
|updated_at|tekenreeks|Nee |



### <a name="file"></a>Bestand


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|geheel getal|Nee |
|URL|tekenreeks|Nee |
|TASK_ID|geheel getal|Nee |
|list_id|geheel getal|Nee |
|USER_ID|geheel getal|Nee |
|bestandsnaam|tekenreeks|Nee |
|CONTENT_TYPE|tekenreeks|Nee |
|file_size|geheel getal|Nee |
|local_created_at|tekenreeks|Nee |
|created_at|tekenreeks|Nee |
|updated_at|tekenreeks|Nee |
|type|tekenreeks|Nee |
|revisie|geheel getal|Nee |



### <a name="newtask"></a>Nieuwe taak


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|list_id|geheel getal|Ja |
|titel|tekenreeks|Ja |
|assignee_id|geheel getal|Nee |
|voltooid|Booleaanse waarde|Nee |
|recurrence_type|tekenreeks|Nee |
|recurrence_count|geheel getal|Nee |
|due_date|tekenreeks|Nee |
|starred|Booleaanse waarde|Nee |



### <a name="newlist"></a>NewList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|titel|tekenreeks|Ja |



### <a name="newsubtask"></a>NewSubtask


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|list_id|geheel getal|Ja |
|TASK_ID|geheel getal|Ja |
|titel|tekenreeks|Ja |
|voltooid|Booleaanse waarde|Nee |



### <a name="newnote"></a>NewNote


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|list_id|geheel getal|Ja |
|TASK_ID|geheel getal|Ja |
|inhoud|tekenreeks|Ja |



### <a name="newcomment"></a>NieuweOpmerking


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|list_id|geheel getal|Ja |
|TASK_ID|geheel getal|Ja |
|tekst|tekenreeks|Ja |



### <a name="newreminder"></a>NewReminder


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|list_id|geheel getal|Ja |
|TASK_ID|geheel getal|Ja |
|datum|tekenreeks|Ja |



### <a name="updatetask"></a>UpdateTask


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|revisie|geheel getal|Nee |
|titel|tekenreeks|Nee |
|assignee_id|geheel getal|Nee |
|voltooid|Booleaanse waarde|Nee |
|recurrence_type|tekenreeks|Nee |
|recurrence_count|geheel getal|Nee |
|due_date|tekenreeks|Nee |
|starred|Booleaanse waarde|Nee |



### <a name="updatelist"></a>Een UpdateList


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|revisie|geheel getal|Nee |
|titel|tekenreeks|Nee |



### <a name="updatesubtask"></a>UpdateSubtask


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|revisie|geheel getal|Nee |
|titel|tekenreeks|Nee |
|voltooid|Booleaanse waarde|Nee |



### <a name="updatenote"></a>UpdateNote


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|revisie|geheel getal|Nee |
|inhoud|tekenreeks|Nee |



### <a name="updatereminder"></a>UpdateReminder


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|datum|tekenreeks|Nee |
|revisie|geheel getal|Nee |


## <a name="next-steps"></a>Volgende stappen
[Een logica-app maken](../app-service-logic/app-service-logic-create-a-logic-app.md)