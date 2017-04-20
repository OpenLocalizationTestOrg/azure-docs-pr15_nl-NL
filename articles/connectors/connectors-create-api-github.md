<properties
pageTitle="GitHub | Microsoft Azure"
description="Logica apps maken met de App Azure-service. GitHub is een cijfer op het web opslagplaats hostingservice. Deze optie biedt de verdeelde revisie besturingselement en gegevensbron code management (SCM) functionaliteit van cijfer, evenals een eigen functies toevoegen."
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

# <a name="get-started-with-the-github-connector"></a>Aan de slag met de verbindingslijn GitHub

GitHub is een cijfer op het web opslagplaats hostingservice. Deze optie biedt de verdeelde revisie besturingselement en gegevensbron code management (SCM) functionaliteit van cijfer, evenals toevoegen van een eigen onderdelen.

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2015-08-01-schema voorbeeldversie. 

U kunt aan de slag met het maken van een app logica nu, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De verbindingslijn GitHub kan worden gebruikt als een actie; trigger(s) heeft. Alle verbindingslijnen ondersteuning voor gegevens in JSON en XML-indelingen. 

 De verbindingslijn GitHub heeft de volgende of meerdere acties en/of trigger(s) beschikbaar:

### <a name="github-actions"></a>GitHub acties
U kunt deze of meerdere acties uitvoeren:

|Actie|Beschrijving|
|--- | ---|
|[CreateIssue](connectors-create-api-github.md#createissue)|Hiermee maakt u een probleem|
### <a name="github-triggers"></a>GitHub-triggers
U kunt luisteren voor deze gebeurtenis(sen):

|Trigger | Beschrijving|
|--- | ---|
|Wanneer een probleem wordt geopend|Een probleem wordt geopend|
|Wanneer een probleem is gesloten|Een probleem is gesloten|
|Wanneer een probleem is toegewezen|Een probleem is toegewezen|


## <a name="create-a-connection-to-github"></a>Een verbinding maken met GitHub
Als u wilt maken logica apps met GitHub, moet u eerst een **verbinding** maken en de details bieden voor de volgende eigenschappen: 

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|Token|Ja|GitHub referenties opgeven|
Nadat u de verbinding hebt gemaakt, kunt u deze uit te voeren van de acties en luisteren naar de triggers in dit artikel beschreven. 

>[AZURE.INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]

>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

## <a name="reference-for-github"></a>Overzicht van GitHub
Dit geldt voor versie: 1.0

## <a name="createissue"></a>CreateIssue
Maken van een probleem: Hiermee maakt u een probleem 

```POST: /repos/{repositoryOwner}/{repositoryName}/issues``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|repositoryOwner|tekenreeks|Ja|pad|geen|De eigenaar van de bibliotheek|
|repositoryName|tekenreeks|Ja|pad|geen|De naam van de bibliotheek|
|issueBasicDetails| |Ja|hoofdtekst|geen|Actie-itemgegevens|

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


## <a name="issueopened"></a>IssueOpened
Wanneer een probleem wordt geopend: een probleem wordt geopend 

```GET: /trigger/issueOpened``` 

Er zijn geen parameters voor dit gesprek
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


## <a name="issueclosed"></a>IssueClosed
Wanneer een probleem is gesloten: een probleem is gesloten 

```GET: /trigger/issueClosed``` 

Er zijn geen parameters voor dit gesprek
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


## <a name="issueassigned"></a>IssueAssigned
Wanneer een probleem is toegewezen: een probleem is toegewezen 

```GET: /trigger/issueAssigned``` 

Er zijn geen parameters voor dit gesprek
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

### <a name="issuebasicdetailsmodel"></a>IssueBasicDetailsModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|titel|tekenreeks|Ja |
|hoofdtekst|tekenreeks|Ja |
|toegewezen gebruiker|tekenreeks|Ja |



### <a name="issuedetailsmodel"></a>IssueDetailsModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|titel|tekenreeks|Ja |
|hoofdtekst|tekenreeks|Ja |
|toegewezen gebruiker|tekenreeks|Ja |
|getal|tekenreeks|Nee |
|de staat|tekenreeks|Nee |
|created_at|tekenreeks|Nee |
|repository_url|tekenreeks|Nee |


## <a name="next-steps"></a>Volgende stappen
[Een logica-app maken](../app-service-logic/app-service-logic-create-a-logic-app.md)