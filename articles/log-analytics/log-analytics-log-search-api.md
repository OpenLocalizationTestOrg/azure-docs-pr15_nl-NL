<properties
    pageTitle="Log Analytics Meld zoeken REST API | Microsoft Azure"
    description="Deze handleiding bevat een eenvoudige zelfstudie waarin wordt beschreven hoe u kunt het logboek Analytics zoeken REST API in de bewerkingen Management Suite Kantoorbeheersysteem en deze bevat voorbeelden waarin u het gebruik van de opdrachten kunt zien."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="log-analytics-log-search-rest-api"></a>Log Analytics log zoeken REST API

Deze handleiding bevat een eenvoudige zelfstudie waarin wordt beschreven hoe u de beschikking over de Log Analytics zoeken REST API in de bewerkingen Management Suite Kantoorbeheersysteem en deze bevat voorbeelden waarin u het gebruik van de opdrachten kunt zien. Enkele voorbeelden in dit artikel verwijzingen maken naar operationele inzichten, namelijk de naam van de vorige versie van Log Analytics.

## <a name="overview-of-the-log-search-rest-api"></a>Overzicht van het logboek zoeken REST API

Het logboek Analytics zoeken REST API RESTful is en kunnen worden geraadpleegd via de Azure resourcemanager-API. In dit document vindt u voorbeelden waarin de API wordt geopend met de [ARMClient](https://github.com/projectkudu/ARMClient), een bron openen opdrachtregel hulpmiddel waardoor het eenvoudiger wordt aanroepen van de Azure resourcemanager-API. Het gebruik van ARMClient en PowerShell is een van de vele opties voor toegang tot de API Log Analytics zoeken. Er is een andere optie voor het gebruik van de Azure PowerShell-module voor OperationalInsights waaronder cmdlets voor het werken met zoeken. U kunt de RESTful Azure resourcemanager API om te bellen OMS werkruimten en uitvoeren van de opdracht zoekopdracht daarin gebruiken met deze hulpmiddelen. De API uitvoer zoekresultaten voor u in de indeling van JSON, zodat u kunt de lijst met zoekresultaten via een programma op veel verschillende manieren gebruiken.

De Manager van Azure Resource kan worden gebruikt via een [bibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) , evenals een [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx). Bekijk de gekoppelde webpagina's voor meer informatie.

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Eenvoudige Log Analytics zoeken REST API zelfstudie

### <a name="to-use-the-arm-client"></a>Gebruik van de ARM-Client

1. Installatie [Chocolatey](https://chocolatey.org/), dat wil het pakket van een Open bronbeheer voor Windows zeggen. Open een opdrachtpromptvenster als beheerder en voer de volgende opdracht:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```

2. Installeer de ARMClient door de volgende opdracht uit te voeren:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-simple-search-using-the-armclient"></a>Een eenvoudige zoekprogramma's via de ARMClient uitvoeren

1. Aanmelden bij uw Microsoft- of OrgID account:

    ```
    armclient login
    ```

    Een succesvolle aanmelding bevat alle abonnementen die zijn gekoppeld aan de opgegeven account:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```

2. De werkruimten bewerkingen Management Suite ophalen:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Een geslaagde Get-aanroep zou alle werkruimten die zijn gekoppeld aan het abonnement uitvoeren:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Maak uw variabele zoeken:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Zoeken met behulp van de variabele van uw nieuwe zoeken:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Meld u aan analyses zoeken REST API verwijzing voorbeelden
De volgende voorbeelden wordt u hoe u de zoeken-API kunt gebruiken.

### <a name="search---actionread"></a>Zoeken - actie/gelezen

**Voorbeeld-Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Vraag:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
De volgende tabel beschrijft de eigenschappen die beschikbaar zijn.

|**Eigenschap**|**Beschrijving**|
|---|---|
|Boven|Het maximum aantal weer te geven resultaten.|
|markeren|Vóór en na parameters, meestal gebruikt voor het markeren van overeenkomende velden bevat|
|Pre|De opgegeven tekenreeks aan de overeenkomende velden toe als voorvoegsel toegevoegd.|
|Verzenden|Hiermee voegt u de opgegeven tekenreeks toe aan de overeenkomende velden toe.|
|query|De query die wordt gebruikt voor het verzamelen en resultaten worden geretourneerd.|
|starten|Het begin van het tijdvenster die u wilt dat de resultaten worden gevonden uit.|
|einde|Het einde van het tijdvenster die u wilt dat de resultaten worden gevonden uit.|


**Antwoord:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Zoeken / {ID} - actie/gelezen

**Vraag de inhoud van een zoekopdracht opgeslagen:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

>[AZURE.NOTE] Als de zoekactie retourneert met status 'In behandeling', kan klikt u vervolgens de bijgewerkte resultaten polling worden uitgevoerd via deze API. Na afloop van 6 min, het resultaat van de zoekopdracht wordt verwijderd uit de cache en HTTP verdwenen wordt geretourneerd. Als de eerste zoekopdracht wordt onmiddellijk een 'Geslaagd' status geretourneerd, wordt deze niet worden toegevoegd aan de cache veroorzaakt door deze API om terug te keren HTTP verdwenen als een query wordt uitgevoerd. De inhoud van het resultaat van een HTTP 200 worden weergegeven in dezelfde opmaak als de eerste zoekopdracht alleen met de bijgewerkte waarden.

### <a name="saved-searches---rest-only"></a>Opgeslagen zoekopdrachten - REST alleen

**Lijst met opgeslagen zoekopdrachten aanvragen:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Ondersteunde methoden: GET plaatsen verwijderen

Ondersteunde methoden: ophalen

De volgende tabel beschrijft de eigenschappen die beschikbaar zijn.

|Eigenschap|Beschrijving|
|---|---|
|ID|De unieke id.|
|ETag|**Vereist voor Patch**. In elke schrijven door server bijgewerkt. Waarde moet gelijk is aan de huidige waarde van de opgeslagen of ' *' om te werken. 409 geretourneerd voor oude/ongeldig waarden.|
|Properties.query|**Vereist**. De query.|
|properties.displayName|**Vereist**. De gebruiker gedefinieerde weergavenaam van de query. Als gezien als een Azure resource, zou dit een Tag.|
|Properties.Category|**Vereist**. De gebruiker gedefinieerde categorie van de query. Als gezien als een Azure resource dit een Tag is.|

>[AZURE.NOTE] De zoeken-API Log Analytics retourneert momenteel gebruiker gemaakte opgeslagen zoekopdrachten wanneer doorzocht op opgeslagen zoekopdrachten in een werkruimte. De API retourneert opgeslagen zoekopdrachten verstrekt door oplossingen op dit moment niet. Deze functionaliteit wordt toegevoegd op een later tijdstip.

### <a name="create-saved-searches"></a>Opgeslagen zoekopdrachten maken

**Vraag:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="delete-saved-searches"></a>Zoekopdrachten verwijderen die zijn opgeslagen

**Vraag:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Update opgeslagen zoekopdrachten

 **Vraag:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metagegevens - JSON alleen

Hier is een manier om de velden voor alle log typen voor de gegevens die worden verzameld in uw werkruimte te bekijken. Bijvoorbeeld als u dat u weten wilt als het type gebeurtenis een veld met de naam Computer heeft, is dit een manier om te zoeken en te bevestigen.

**Verzoek om velden:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Antwoord:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

De volgende tabel beschrijft de eigenschappen die beschikbaar zijn.

|**Eigenschap**|**Beschrijving**|
|---|---|
|naam|Veldnaam.|
|Weergavenaam|De weergavenaam van het veld.|
|type|Het Type van de veldwaarde.|
|facetable|Combinatie van huidige 'geïndexeerde', ' opgeslagen ' en 'facet' Eigenschappen.|
|weergeven|Huidige 'weergeven' eigenschap. Waar als het veld wordt weergegeven in zoekresultaten.|
|voor ownerType|Beperkte aan alleen typen worden gebruikt die deel uitmaken van de onboarded IP.|


## <a name="optional-parameters"></a>Optionele parameters
De volgende informatie beschrijft optionele parameters die beschikbaar zijn.

### <a name="highlighting"></a>Markeren

De parameter "Markeren" is een optionele parameter die u kunt de zoeken-subsysteem aanvragen bevat een aantal markeringen in de reactie.

Deze markeringen geven aan het begin en eindig gemarkeerde tekst die overeenkomt met de voorwaarden die beschikbaar zijn in uw query.
U kunt de begin- en -markeringen die worden gebruikt door zoeken om te laten teruglopen van de gemarkeerde term opgeven.

**Voorbeeld zoekquery**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Resultaat van voorbeeld:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Zoals u ziet het resultaat bovenstaande een foutbericht dat is het voorvoegsel en toegevoegd.

## <a name="computer-groups"></a>Computergroepen

Computergroepen zijn speciale opgeslagen zoekopdrachten waarmee een set computers wordt geretourneerd.  U kunt een computergroep in een andere query om de resultaten met de computers in de groep te beperken.  Een computergroep wordt geïmplementeerd als een opgeslagen zoekactie met een tag groep met een waarde van de Computer.

Hier volgt een voorbeeld-antwoord voor een computergroep.

    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
        }],
    "Version": 1
    }

### <a name="retrieving-computer-groups"></a>Computergroepen ophalen

Gebruik de methode ophalen met de groeps-ID om op te halen van een computergroep.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Maken of een computergroep bijwerken

De opslag methode gebruiken met een unieke opgeslagen-ID zoeken naar een nieuwe computergroep maken. Als u een bestaande computer groeps-ID gebruikt, wordt dat een worden gewijzigd. Wanneer u een computergroep in de OMS-console maakt, wordt de ID van de groep en de naam gemaakt.

De query die wordt gebruikt voor de definitie van de moet een set computers voor de groep goed te retourneren.  Het wordt aanbevolen dat u uw query met *beëindigen | DISTINCT Computer* om ervoor te zorgen de juiste gegevens als resultaat gegeven.

De definitie van de opgeslagen zoekactie moet een tag met de naam groep met een waarde van de Computer voor het zoeken worden geclassificeerd als een computergroep bevatten.

    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson

### <a name="deleting-computer-groups"></a>Computergroepen verwijderen

Gebruik de methode verwijderen met de groeps-ID aan een computergroep verwijderen.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [zoekopdrachten in het logboek](log-analytics-log-searches.md) maken van query's met aangepaste velden voor de criteria.
