<properties
pageTitle="Project Online | Microsoft Azure"
description="Logica apps maken met de App Azure-service. Project Online is een flexibele online oplossing voor projectportfoliobeheer (PPM) en dagelijkse werk van Microsoft. Afgeleverd via Office 365, Project Online kunnen organisaties snel aan de slag met krachtige mogelijkheden voor projectbeheer plannen en beheren van projecten en project portfolio investeringen prioriteit te bepalen, vanaf vrijwel elke locatie op vrijwel elk apparaat."
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

# <a name="get-started-with-the-projectonline-connector"></a>Aan de slag met de Project Online-connector

Project Online is een flexibele online oplossing voor projectportfoliobeheer (PPM) en dagelijkse werk van Microsoft. Afgeleverd via Office 365, Project Online kunnen organisaties snel aan de slag met krachtige mogelijkheden voor projectbeheer plannen en beheren van projecten en project portfolio investeringen prioriteit voorzien, vanaf vrijwel elke locatie op vrijwel elk apparaat.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2015-08-01-schema voorbeeldversie. 

U kunt aan de slag met het maken van een app logica nu, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De verbindingslijn Project Online kan worden gebruikt als een actie; trigger(s) heeft. Alle verbindingslijnen ondersteuning voor gegevens in JSON en XML-indelingen. 

 De verbindingslijn project online heeft de volgende of meerdere acties en/of trigger(s) beschikbaar:

### <a name="projectonline-actions"></a>Acties voor Project Online
U kunt deze of meerdere acties uitvoeren:

|Actie|Beschrijving|
|--- | ---|
|[ListProjects](connectors-create-api-projectonline.md#listprojects)|Lijsten van de projecten in uw project online-site|
|[CreateProject](connectors-create-api-projectonline.md#createproject)|Een nieuw project maakt in uw project online-site|
|[CreateTask](connectors-create-api-projectonline.md#createtask)|Hiermee maakt u een nieuwe taak in u project|
|[CreateResource](connectors-create-api-projectonline.md#createresource)|Hiermee maakt u een ondernemingsresources in uw project online-site|
|[ListTasks](connectors-create-api-projectonline.md#listtasks)|Lijsten van de gepubliceerde taken in een project|
|[CheckoutProject](connectors-create-api-projectonline.md#checkoutproject)|Een project in uw site uitgecheckt|
|[PublishProject](connectors-create-api-projectonline.md#publishproject)|Inchecken en publiceren en bestaande project op uw site|
### <a name="projectonline-triggers"></a>Project Online-triggers
U kunt luisteren voor deze gebeurtenis(sen):

|Trigger | Beschrijving|
|--- | ---|
|Wanneer een nieuw project is gemaakt|Een stroom activeert wanneer een nieuw project wordt gemaakt|
|Wanneer een nieuwe resource wordt gemaakt|Een nieuwe stroom activeert als een nieuwe resource wordt gemaakt|
|Wanneer een nieuwe taak is gemaakt|Een stroom activeert als een nieuwe taak wordt gemaakt|


## <a name="create-a-connection-to-projectonline"></a>Een verbinding maken met Project Online
Als wilt logica apps maken met Project Online, moet u eerst een **verbinding** maken en geef de details voor de volgende eigenschappen: 

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|Token|Ja|De referenties van de Project Online|

>[AZURE.INCLUDE [Steps to create a connection to ProjectOnline](../../includes/connectors-create-api-projectonline.md)]

>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

## <a name="reference-for-projectonline"></a>Overzicht van Project Online
Dit geldt voor versie: 1.0

## <a name="onnewproject"></a>OnNewProject
Wanneer een nieuw project is gemaakt: een stroom activeert wanneer een nieuw project wordt gemaakt 

```GET: /trigger/_api/ProjectData/Projects``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|Site-url van uw projectsite root (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewresource"></a>OnNewResource
Wanneer een nieuwe resource wordt gemaakt: een nieuwe stroom activeert als een nieuwe resource wordt gemaakt 

```GET: /trigger/_api/ProjectData/Resources``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|Site-url van uw projectsite root (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewtask"></a>OnNewTask
Wanneer een nieuwe taak is gemaakt: een stroom activeert als een nieuwe taak wordt gemaakt 

```GET: /trigger/_api/ProjectData/Tasks``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|Site-url van uw projectsite root (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="listprojects"></a>ListProjects
Een lijst met projecten: lijsten van de projecten in uw project online-site 

```GET: /_api/ProjectServer/Projects``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|Site-url van uw projectsite root (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="createproject"></a>CreateProject
Hiermee maakt u nieuwe project: een nieuw project maakt in uw project online-site 

```POST: /_api/ProjectServer/Projects``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|Site-url van uw projectsite root (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|
|Project| |Ja|hoofdtekst|geen|Nieuw project maken|

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


## <a name="createtask"></a>CreateTask
Hiermee maakt u nieuwe taak: Hiermee maakt u een nieuwe taak in u project 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Tasks/Add``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|Site-url van uw projectsite root (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|tekenreeks|Ja|pad|geen|Unieke ID van het project dat de taak wilt toevoegen|
|taak| |Ja|hoofdtekst|geen|Nieuwe taak toevoegen aan het project|

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


## <a name="createresource"></a>CreateResource
Nieuwe resource maakt: Hiermee maakt u een ondernemingsresources in uw project online-site 

```POST: /_api/ProjectServer/EnterpriseResources``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|Site-url van uw projectsite root (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|
|resource| |Ja|hoofdtekst|geen|Nieuwe ondernemingsresource om toe te voegen aan het project|

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


## <a name="listtasks"></a>ListTasks
Lijsten van taken: lijsten van de gepubliceerde taken in een project 

```GET: /_api/ProjectServer/Projects('{project_id}')/Tasks``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|Site-url van uw projectsite root (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|tekenreeks|Ja|pad|geen|Unieke ID van het project voor het ophalen van taken|

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


## <a name="checkoutproject"></a>CheckoutProject
Uitchecken een project: een project in uw site uitgecheckt 

```POST: /_api/ProjectServer/Projects('{project_id}')/checkOut``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|Site-url van uw projectsite root (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|tekenreeks|Ja|pad|geen|Unieke ID van het project dat de taak wilt toevoegen|

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


## <a name="publishproject"></a>PublishProject
Inchecken en project publiceren: inchecken en publiceren en bestaande project op uw site 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Publish(true)``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|siteUrl|tekenreeks|Ja|query|geen|Site-url van uw projectsite root (voorbeeld: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|tekenreeks|Ja|pad|geen|Unieke ID van het project naar inchecken|

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

### <a name="triggerprojectswrapper"></a>TriggerProjectsWrapper


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="triggerproject"></a>TriggerProject


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ProjectStartDate|tekenreeks|Nee |
|ProjectFinishDate|tekenreeks|Nee |
|ProjectCreatedDate|tekenreeks|Nee |
|ProjectId|tekenreeks|Nee |
|ProjectModifiedDate|tekenreeks|Nee |
|ProjectType|geheel getal|Nee |
|ProjectName|tekenreeks|Nee |



### <a name="triggerresourceswrapper"></a>TriggerResourcesWrapper


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="triggerresource"></a>TriggerResource


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ResourceId|tekenreeks|Nee |
|ResourceBaseCalendar|tekenreeks|Nee |
|ResourceBookingType|geheel getal|Nee |
|ResourceCanLevel|Booleaanse waarde|Nee |
|ResourceCostPerUse|getal|Nee |
|ResourceCreatedDate|tekenreeks|Nee |
|ResourceEarliestAvailableFrom|tekenreeks|Nee |
|ResourceEmail|tekenreeks|Nee |
|ResourceInitials|tekenreeks|Nee |
|ResourceIsActive|Booleaanse waarde|Nee |
|ResourceIsGeneric|Booleaanse waarde|Nee |
|ResourceLatestAvailableTo|tekenreeks|Nee |
|ResourceModifiedDate|tekenreeks|Nee |
|Bronnaam|tekenreeks|Nee |
|ResourceStatsuName|tekenreeks|Nee |
|Brontype|geheel getal|Nee |
|TypeDescription|tekenreeks|Nee |
|TypeName|tekenreeks|Nee |



### <a name="triggertaskswrapper"></a>TriggerTasksWrapper


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="triggertask"></a>TriggerTask


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ProjectId|tekenreeks|Nee |
|TaskId|tekenreeks|Nee |
|ProjectName|tekenreeks|Nee |
|Taaknaam|tekenreeks|Nee |
|TaskCreatedDate|tekenreeks|Nee |
|TaskModifieddate|tekenreeks|Nee |
|TaskStartDate|tekenreeks|Nee |
|TaskFinishDate|tekenreeks|Nee |
|TaskPriority|geheel getal|Nee |
|TaskIsActive|Booleaanse waarde|Nee |



### <a name="newproject"></a>Open


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Naam|tekenreeks|Ja |
|Beschrijving|tekenreeks|Nee |
|Starten|tekenreeks|Nee |



### <a name="newreource"></a>NewReource


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Naam|tekenreeks|Ja |
|IsBudget|Booleaanse waarde|Nee |
|IsGeneric|Booleaanse waarde|Nee |
|IsInactive|Booleaanse waarde|Nee |



### <a name="project"></a>Project


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ApprovedStart|tekenreeks|Nee |
|ApprovedEnd|tekenreeks|Nee |
|CheckedOutDate|tekenreeks|Nee |
|CheckOutDescription|tekenreeks|Nee |
|CheckOutId|tekenreeks|Nee |
|Voor CreatedDate|tekenreeks|Nee |
|ID|tekenreeks|Nee |
|IsCheckedOut|Booleaanse waarde|Nee |
|LastPublishedDate|tekenreeks|Nee |
|Aanmaakdatum|tekenreeks|Nee |
|OptimizerDecision|geheel getal|Nee |
|PlannerDecision|geheel getal|Nee |
|ProjectType|geheel getal|Nee |
|Naam|tekenreeks|Nee |
|WinprojVersion|tekenreeks|Nee |



### <a name="projectswrapper"></a>ProjectsWrapper


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="newtask"></a>Nieuwe taak


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|parameters|niet gedefinieerd|Ja |



### <a name="taskparameters"></a>TaskParameters


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Naam|tekenreeks|Ja |
|Notities|tekenreeks|Nee |
|Starten|tekenreeks|Nee |
|Duur|tekenreeks|Nee |



### <a name="enterpriseresource"></a>EnterpriseResource


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|CanLevel|Booleaanse waarde|Nee |
|Code|tekenreeks|Nee |
|CostAccrual|geheel getal|Nee |
|CostCenter|tekenreeks|Nee |
|Gemaakt|tekenreeks|Nee |
|DefaultBookingType|geheel getal|Nee |
|E-mail|tekenreeks|Nee |
|ExternalId|tekenreeks|Nee |
|Groep|tekenreeks|Nee |
|In dienst|tekenreeks|Nee |
|ID|tekenreeks|Nee |
|Initialen|tekenreeks|Nee |
|IsActive|Booleaanse waarde|Nee |
|IsBudget|Booleaanse waarde|Nee |
|IsCheckedOut|Booleaanse waarde|Nee |
|IsGeneric|Booleaanse waarde|Nee |
|IsTeam|Booleaanse waarde|Nee |
|MaterialLabel|tekenreeks|Nee |
|Gewijzigd|tekenreeks|Nee |
|Naam|tekenreeks|Nee |
|Fonetiek|tekenreeks|Nee |
|Brontype|geheel getal|Nee |
|TerminationDate|tekenreeks|Nee |



### <a name="taskswrapper"></a>TasksWrapper


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|waarde|matrix|Nee |



### <a name="task"></a>Taak


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|Gemaakt|tekenreeks|Nee |
|Gewijzigd|tekenreeks|Nee |
|Starten|tekenreeks|Nee |
|Voltooien|tekenreeks|Nee |
|Naam|tekenreeks|Nee |
|ID|tekenreeks|Nee |
|Prioriteit|geheel getal|Nee |
|PercentComplete|geheel getal|Nee |
|Notities|tekenreeks|Nee |
|Contactpersoon|tekenreeks|Nee |


## <a name="next-steps"></a>Volgende stappen
[Een logica-app maken](../app-service-logic/app-service-logic-create-a-logic-app.md)