<properties
   pageTitle="Azure resourcemanager sjablonen Authoring | Microsoft Azure"
   description="Azure resourcemanager sjablonen met declaratieve JSON-syntaxis voor de implementatie van Azure-toepassingen maken."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="authoring-azure-resource-manager-templates"></a>Een presentatie resourcemanager Azure-sjablonen

In dit onderwerp worden de structuur van een sjabloon resourcemanager Azure. Daaraan de verschillende secties van een sjabloon en de eigenschappen die beschikbaar in deze secties zijn. De sjabloon bestaat uit JSON en expressies die u gebruiken kunt om samen te stellen van de waarden voor de implementatie. 

De sjabloon voor resources die u al hebt geïmplementeerd, Zie [exporteren van een sjabloon Azure resourcemanager aan de bestaande bronnen](resource-manager-export-template.md). Voor hulp bij het maken van een sjabloon, raadpleegt u [Stapsgewijze instructies resourcemanager-sjabloon](resource-manager-template-walkthrough.md). Zie [Aanbevolen procedures voor het maken van Azure resourcemanager sjablonen](resource-manager-template-best-practices.md)voor aanbevelingen over het maken van sjablonen.

Een goede JSON-editor kunt u de taak voor het maken van sjablonen van vereenvoudigen. Zie voor informatie over het gebruik van Visual Studio met uw sjablonen [maken en implementeren van Azure resourcegroepen tot en met Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). Zie [werken met Azure resourcemanager sjablonen in Visual Studio-Code](resource-manager-vs-code.md)voor informatie over het gebruik van de VS Code.

De grootte van de sjabloon wilt 1 MB en elke parameterbestand 64 kB beperken. De limiet van 1 MB is van toepassing op de laatste staat van de sjabloon nadat deze is uitgevouwen met iteratieve resource definities en waarden voor de variabelen en parameters. 

## <a name="template-format"></a>Sjabloon opmaken

Een sjabloon bevat in de eenvoudigste structuur, de volgende elementen:

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| De elementnaam van het   | Vereist | Beschrijving
| :------------: | :------: | :----------
| $schema        |   Ja    | Locatie van de JSON-schemabestand waarmee de versie van de sjabloontaal wordt beschreven. De URL die wordt weergegeven in het voorgaande voorbeeld gebruiken.
| contentVersion |   Ja    | Versie van de sjabloon (zoals 1.0.0.0). Voor dit element kunt u elke waarde opgeven. Wanneer u resources met de sjabloon implementeert, kan deze waarde kan worden gebruikt om ervoor te zorgen dat de juiste sjabloon waarmee wordt gebruikt.
| parameters     |   Nee     | Waarden die beschikbaar zijn wanneer de implementatie wordt uitgevoerd om aan te passen resource-implementatie.
| variabelen      |   Nee     | Waarden die worden gebruikt als JSON fragmenten in de sjabloon voor de sjabloon taal expressies te vereenvoudigen.
| resources      |   Ja    | Resourcetypen die zijn geïmplementeerd of bijgewerkt in een resourcegroep.
| uitvoer        |   Nee     | Waarden die worden geretourneerd na implementatie.

We bestudeert u de secties van de sjabloon uitgebreider verderop in dit onderwerp. Nu wordt besproken enkele van de syntaxis van de waaruit de sjabloon.

## <a name="expressions-and-functions"></a>Expressies en functies

De syntaxis van de sjabloon is in principe JSON. Expressies en functies uitbreiden echter de JSON die beschikbaar is in de sjabloon. Met expressies maakt u de waarden die niet strikte letterlijke waarden zijn. Expressies zijn ingesloten met vierkante haken [en], en worden geëvalueerd als de sjabloon wordt geïmplementeerd. Expressies kunnen plaatsen in een tekenreekswaarde JSON en altijd terugkeren een andere JSON-waarde. Als u wilt gebruiken een letterlijke tekenreeks die met een haak openen begint [, moet u twee vierkante haken [[.

Normaal gesproken kunt u expressies met de functies worden bewerkingen uitgevoerd voor het configureren van de implementatie. Net als in JavaScript, zijn functie oproepen opgemaakt als **functionName(arg1,arg2,arg3)**. U verwijzen naar eigenschappen met behulp van de stip en [index] operatoren opnemen.

Het volgende voorbeeld ziet het gebruik van de verschillende functies wanneer het bouwen van waarden:
 
    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

Zie [Azure resourcemanager sjabloon functies](resource-group-template-functions.md)voor de volledige lijst met functies van de sjabloon. 


## <a name="parameters"></a>Parameters

U opgeven welke waarden u kunt invoer bij het distribueren van de resources in de sectie parameters van de sjabloon. Deze parameterwaarden kunnen u de implementatie aanpassen door te geven van waarden die zijn aangepast voor een bepaalde omgeving (zoals ontwikkelaar, test- en). U hoeft niet te bieden van parameters in uw sjabloon, maar zonder parameters uw sjabloon altijd dezelfde resources met dezelfde namen, locaties en eigenschappen wilt implementeren.

U kunt deze parameterwaarden overal in de sjabloon waarden voor de geïmplementeerd resources instellen. Alleen parameters die zijn gedefinieerd in de sectie parameters kunnen worden gebruikt in andere secties van de sjabloon.

U definiëren parameters met de volgende structuur:

    "parameters": {
       "<parameter-name>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<default-value-of-parameter>",
         "allowedValues": [ "<array-of-allowed-values>" ],
         "minValue": <minimum-value-for-int>,
         "maxValue": <maximum-value-for-int>,
         "minLength": <minimum-length-for-string-or-array>,
         "maxLength": <maximum-length-for-string-or-array-parameters>,
         "metadata": {
             "description": "<description-of-the parameter>" 
         }
       }
    }

| De elementnaam van het   | Vereist | Beschrijving
| :------------: | :------: | :----------
| parameterName  |   Ja    | De naam van de parameter. Moet u een geldige JavaScript-id.
| type           |   Ja    | Type de parameterwaarde op te geven. Zie de onderstaande lijst met toegestane typen.
| Standaardwaarde   |   Nee     | De standaardwaarde voor de parameter, als u geen waarde is opgegeven voor de parameter.
| allowedValues  |   Nee     | Matrix met toegestane waarden voor de parameter om ervoor te zorgen dat de juiste waarde is opgegeven.
| minValue       |   Nee     | De minimumwaarde voor int typeparameters, deze waarde is inclusief.
| maxValue       |   Nee     | De maximale waarde voor int typeparameters, deze waarde is inclusief.
| minLength      |   Nee     | De minimumlengte voor tekenreeks, secureString en matrix typeparameters, deze waarde is inclusief.
| maxLength      |   Nee     | De maximumlengte voor tekenreeks, secureString en matrix typeparameters, deze waarde is inclusief.
| Beschrijving    |   Nee     | Beschrijving van de parameter, dat wordt weergegeven aan gebruikers van de sjabloon die u via de portal aangepaste sjabloon-interface.

De toegestane typen en waarden zijn:

- **tekenreeks**
- **secureString**
- **int**
- **BOOL**
- **object** 
- **secureObject**
- **matrix**

Als u een parameter als optionele, vindt u een defaultValue (een lege tekenreeks steekt nogal). 

Als u een parameternaam die overeenkomt met een van de parameters in de opdracht om te implementeren van de sjabloon opgeeft, wordt u gevraagd om een waarde voor een parameter met de postfix **FromTemplate**. Bijvoorbeeld als u een parameter met de naam **ResourceGroupName** in uw sjabloon opnemen die is hetzelfde als de parameter **ResourceGroupName** in de [Nieuwe AzureRmResourceGroupDeployment] [ deployment2cmdlet] cmdlet, moet u een waarde opgeven voor **ResourceGroupNameFromTemplate**. In het algemeen, voorkomt u deze verwarring door de naam van parameters niet met dezelfde naam als parameters gebruikt voor implementatie bewerkingen.

>[AZURE.NOTE] Alle wachtwoorden, sleutels en andere geheimen moeten het type **secureString** gebruiken. Parameters van sjabloon met het type secureString kunnen niet worden gelezen na implementatie van de resource. 

Het volgende voorbeeld ziet u hoe om parameters te definiëren:

    "parameters": {
      "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
      },
      "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
      },
      "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
      },
      "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
      }
    }

Hoe u invoer van de betreffende parameterwaarden tijdens de implementatie, raadpleegt u [een toepassing met Azure resourcemanager sjabloon Deploy](resource-group-template-deploy.md#parameter-file). 

## <a name="variables"></a>Variabelen

Klik in de sectie variabelen kunt u waarden die kunnen worden gebruikt samenstellen overal in uw sjabloon. Meestal variabelen op basis van de waarden uit de parameters die zijn ingevoerd. U hoeft niet te definiëren variabelen, maar ze vaak uw sjabloon vereenvoudigen doordat complexe expressies.

U definiëren variabelen met de volgende structuur:

    "variables": {
       "<variable-name>": "<variable-value>",
       "<variable-name>": { 
           <variable-complex-type-value> 
       }
    }

Het volgende voorbeeld ziet hoe u een variabele die is gebaseerd op twee parameterwaarden definiëren:

     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

Het volgende voorbeeld ziet u een variabele die is van een complexe JSON-type en variabelen die zijn gebouwd als van andere variabelen:

    "parameters": {
       "environmentName": {
         "type": "string",
         "allowedValues": [
           "test",
           "prod"
         ]
       }
    },
    "variables": {
       "environmentSettings": {
         "test": {
           "instancesSize": "Small",
           "instancesCount": 1
         },
         "prod": {
           "instancesSize": "Large",
           "instancesCount": 4
         }
       },
       "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
       "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
       "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
    }

## <a name="resources"></a>Resources

In de sectie bronnen definieert u de resources die zijn geïmplementeerd of bijgewerkt. In dit gedeelte krijgt ingewikkeld omdat u de typen die u implementeren wilt om de juiste waarden moet kennen. 

U definiëren resources met de volgende structuur:

    "resources": [
       {
         "apiVersion": "<api-version-of-resource>",
         "type": "<resource-provider-namespace/resource-type-name>",
         "name": "<name-of-the-resource>",
         "location": "<location-of-resource>",
         "tags": "<name-value-pairs-for-resource-tagging>",
         "comments": "<your-reference-notes>",
         "dependsOn": [
           "<array-of-related-resource-names>"
         ],
         "properties": "<settings-for-the-resource>",
         "copy": {
           "name": "<name-of-copy-loop>",
           "count": "<number-of-iterations>"
         }
         "resources": [
           "<array-of-child-resources>"
         ]
       }
    ]

| De elementnaam van het             | Vereist | Beschrijving
| :----------------------: | :------: | :----------
| apiVersion               |   Ja    | Versie van de REST API gebruiken voor het maken van de resource.
| type                     |   Ja    | Type van de resource. Deze waarde is een combinatie van de naamruimte van de resource-provider en het resourcetype (zoals **Microsoft.Storage/storageAccounts**).
| naam                     |   Ja    | De naam van de resource. De naam moet URI onderdeel beperkingen zijn gedefinieerd in RFC3986 volgen. Daarnaast Azure services die de naam van de resource voor het valideren van de naam om te controleren of deze buiten partijen tonen is niet een poging naar een andere identiteit nabootsen. Zie [Resourcenaam controleren](https://msdn.microsoft.com/library/azure/mt219035.aspx).
| locatie                 |   Varieert  | Ondersteunde geografische-locaties van de meegeleverde resource. U kunt een van de beschikbare locaties selecteren, maar gewoonlijk is het verstandig om te kiezen dat voldoet aan uw gebruikers. Meestal is het ook verstandig om te plaatsen van resources die met elkaar in dezelfde regio werken. De meeste resourcetypen vereist een locatie, maar sommige typen (zoals een roltoewijzing) is niet vereist een locatie.
| labels                     |   Nee     | Labels die zijn gekoppeld aan de resource.
| opmerkingen                 |   Nee     | Uw notities voor de resources in uw sjabloon documenteert
| dependsOn                |   Nee     | Resources die afhankelijk van de resource wordt gedefinieerd. De afhankelijkheden tussen resources worden geëvalueerd en resources zijn geïmplementeerd in hun afhankelijke volgorde. Wanneer u bronnen niet afhankelijk zijn van elkaar, worden ze parallel geïmplementeerd. De waarde kan een door komma's gescheiden lijst met een resource zijn namen of unieke id's voor een resource.
| Eigenschappen               |   Nee     | Resource-specifieke configuratie-instellingen. De waarden voor de eigenschappen zijn hetzelfde als de waarden die u in het hoofdgedeelte van de aanvraag voor de REST API-bewerking (opslag methode opgeeft) om de bron te maken. Zie voor koppelingen naar resource schemadocumentatie of REST API, [resourcemanager providers, regio's, API-versies en schema's](resource-manager-supported-services.md).
| kopiëren                     |   Nee     | Als meer dan één exemplaar is vereist, het aantal bronnen die u wilt maken. Zie [meerdere exemplaren van resources in Azure resourcemanager maken](resource-group-create-multiple.md)voor meer informatie. |
| resources                |   Nee     | Onderliggende resources die afhankelijk zijn van de resource wordt gedefinieerd. U kunt alleen resourcetypen die zijn toegestaan door het schema van de resource bovenliggende opgeeft. De volledig gekwalificeerde naam van het kind brontype bevat het type van een bovenliggende resource, zoals **Microsoft.Web/sites/extensions**. Afhankelijkheid van de bovenliggende resource wordt niet geïmpliceerd; u moet expliciet die afhankelijkheid definiëren. 

U weet welke waarden u moet opgeven voor **apiVersion**, is **type**, en de **locatie** niet onmiddellijk duidelijk. Gelukkig kunt u deze waarden via Azure PowerShell of Azure CLI bepalen.

Als u alle resource-providers met **PowerShell**, gebruikt u:

    Get-AzureRmResourceProvider -ListAvailable

Zoek de resource-providers waarin u geïnteresseerd bent in de resulterende lijst. Als u de resourcetypen voor een resource-provider (zoals opslag), gebruikt u:

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes

Als u de API-versies voor een resourcetype (zoals opslag-accounts), gebruikt u:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).ApiVersions

Als u ondersteunde locaties voor een resourcetype, gebruikt u:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).Locations

Als u alle resource-providers met **Azure CLI**, gebruikt u:

    azure provider list

Zoek de resource-providers waarin u geïnteresseerd bent in de resulterende lijst. Als u de resourcetypen voor een resource-provider (zoals opslag), gebruikt u:

    azure provider show Microsoft.Storage

Als u ondersteunde locaties en API-versies, gebruikt u:

    azure provider show Microsoft.Storage --details --json

Zie voor meer informatie over providers van de resource, [resourcemanager providers, regio's, API-versies en schema's](resource-manager-supported-services.md).

De sectie resources bevat een matrix van de bronnen om te implementeren. Binnen elke resource, kunt u ook een matrix van de onderliggende resources definiëren. De sectie van uw resources hebt daardoor, kan een structuur zoals:

    "resources": [
       {
           "name": "resourceA",
       },
       {
           "name": "resourceB",
           "resources": [
               {
                   "name": "firstChildResourceB",
               },
               {   
                   "name": "secondChildResourceB",
               }
           ]
       },
       {
           "name": "resourceC",
       }
    ]


Het volgende voorbeeld ziet u een resource **Microsoft.Web/serverfarms** en een resource **Microsoft.Web/sites** met een onderliggend **extensies** resource. Zoals u ziet dat de site is gemarkeerd als afhankelijk van de server-farm, omdat de server-farm moet bestaan voordat de site kan worden geïmplementeerd. U ziet ook dat de resource **extensies** onderliggend item van de site.

    "resources": [
      {
        "apiVersion": "2015-08-01",
        "name": "[parameters('hostingPlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "HostingPlan"
        },
        "sku": {
          "name": "[parameters('skuName')]",
          "capacity": "[parameters('skuCapacity')]"
        },
        "properties": {
          "name": "[parameters('hostingPlanName')]",
          "numberOfWorkers": 1
        }
      },
      {
        "apiVersion": "2015-08-01",
        "type": "Microsoft.Web/sites",
        "name": "[parameters('siteName')]",
        "location": "[resourceGroup().location]",
        "tags": {
          "environment": "test",
          "team": "Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Web/serverFarms/', parameters('hostingPlanName'))]"
        ],
        "properties": {
          "name": "[parameters('siteName')]",
          "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
        },
        "resources": [
          {
            "apiVersion": "2015-08-01",
            "type": "extensions",
            "name": "MSDeploy",
            "dependsOn": [
              "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
            ],
            "properties": {
              "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
              "dbType": "None",
              "connectionString": "",
              "setParameters": {
                "Application Path": "[parameters('siteName')]"
              }
            }
          }
        ]
      }
    ]


## <a name="outputs"></a>Uitvoer

Klik in de sectie uitvoer van u waarden die worden geretourneerd uit implementatie. U kunt bijvoorbeeld de URI voor toegang tot een uitgevouwen resource retourneren.

Het volgende voorbeeld ziet de structuur van de definitie van een uitvoer:

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>"
       }
    }

| De elementnaam van het   | Vereist | Beschrijving
| :------------: | :------: | :----------
| outputName     |   Ja    | Naam van de uitvoerwaarde. Moet u een geldige JavaScript-id.
| type           |   Ja    | Type de uitvoerwaarde. Uitvoerwaarden ondersteuning voor dezelfde typen als sjabloon invoerparameters weergegeven.
| waarde          |   Ja    | Sjabloon taal-expressie die wordt geëvalueerd en die wordt geretourneerd als de waarde voor uitvoer.


De volgende voorbeeldformule wordt een waarde die wordt geretourneerd in de sectie uitvoer.

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

Zie voor meer informatie over het werken met uitvoer [delen staat in de resourcemanager Azure-sjablonen](best-practices-resource-manager-state.md).

## <a name="next-steps"></a>Volgende stappen
- Zie de [Azure Quickstart sjablonen](https://azure.microsoft.com/documentation/templates/)voltooid sjablonen voor groot aantal verschillende grafiektypen oplossingen.
- Zie [Azure resourcemanager sjabloon functies](resource-group-template-functions.md)voor meer informatie over de functies die u via binnen een sjabloon gebruiken kunt.
- Als u wilt combineren meerdere sjablonen tijdens de implementatie, Zie [gekoppelde sjablonen met Azure Resource Manager gebruiken](resource-group-linked-templates.md).
- Mogelijk moet u de resources die zijn opgeslagen in een andere resourcegroep gebruiken. Dit scenario geldt tijdens het werken met opslag accounts of virtuele netwerken die door meerdere resourcegroepen worden gedeeld. Zie de [resourceId functie](resource-group-template-functions.md#resourceid)voor meer informatie.


[deployment2cmdlet]: https://msdn.microsoft.com/library/mt740620(v=azure.200).aspx
