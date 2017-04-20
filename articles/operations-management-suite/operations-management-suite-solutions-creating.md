<properties
   pageTitle="Managementoplossingen te maken in bewerkingen Management Suite Kantoorbeheersysteem | Microsoft Azure"
   description="De functionaliteit van bewerkingen Management Suite Kantoorbeheersysteem uitbreiden managementoplossingen doordat verpakt management-scenario's die klanten aan hun OMS-werkruimte toevoegen kunnen.  In dit artikel bevat informatie over hoe u de oplossingen moet worden gebruikt in uw eigen omgeving kunt maken of beschikbaar worden gemaakt naar uw klanten."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="creating-management-solutions-in-operations-management-suite-oms-preview"></a>Managementoplossingen te maken in bewerkingen Management Suite Kantoorbeheersysteem (Preview)

>[AZURE.NOTE]Dit is voorlopige documentatie voor het maken van oplossingen in OMS die momenteel beschikbaar in de Preview-versie zijn. Een schema hieronder beschreven kan worden gewijzigd.  

De functionaliteit van bewerkingen Management Suite Kantoorbeheersysteem uitbreiden managementoplossingen doordat verpakt management-scenario's die klanten aan hun OMS-werkruimte toevoegen kunnen.  In dit artikel bevat informatie over het maken van uw eigen managementoplossingen die u kunt in uw eigen omgeving gebruiken of beschikbaar voor klanten via de community maken.

## <a name="planning-your-management-solution"></a>Uw beheeroplossing voor planning
Oplossingen in OMS bevatten meerdere resources ondersteunende van een scenario voor bepaalde management.  Wanneer u uw oplossing plant, moet u zich richten op het scenario dat u probeert te bereiken en alle vereiste resources te ondersteunen.  Elke oplossing moet worden opgenomen en definiëren van elke resource die deze nodig heeft, zelfs als een of meer resources ook zijn gedefinieerd door andere oplossingen.  Wanneer een beheeroplossing is geïnstalleerd, wordt elke resource gemaakt, tenzij deze al bestaat en u wat gebeurt er met de resources definiëren kunt wanneer een oplossing wordt verwijderd.  

Een oplossing kan bijvoorbeeld een [Azure automatisering runbook](../automation/automation-intro.md) waarmee gegevens worden verzameld naar het logboek Analytics-bibliotheek met een [schema](../automation/automation-schedules.md) en een [weergave](../log-analytics/log-analytics-view-designer.md) vindt u verschillende visualisaties van de verzamelde gegevens bevatten.  Hetzelfde schema kan worden gebruikt door een andere oplossing.  U zou doen als de auteur van de oplossing management alle drie resources definiëren maar opgeven dat de runbook en de weergave moeten worden automatisch verwijderd wanneer de oplossing wordt verwijderd.    U wordt ook de planning definiëren, maar u kunt opgeven dat deze op hun plaats staan blijven moet als de oplossing zijn als deze nog steeds in gebruik is verwijderd door de andere oplossing.

## <a name="management-solution-files"></a>Bestanden van de oplossing Management
Oplossingen worden geïmplementeerd als [resourcebeheer sjablonen](../resource-manager-template-walkthrough.md).  De belangrijkste taak bij het leren hoe u de oplossingen voor beheer van de auteur is leren hoe u [de auteur van een sjabloon](../resource-group-authoring-templates.md).  In dit artikel vindt unieke details van sjablonen die wordt gebruikt voor oplossingen en het definiëren van de normale oplossing resources.

De basisstructuur van een bestand van de oplossing management is hetzelfde als [Resourcemanager sjabloon](resource-group-authoring-templates.md#template-format) die u als volgt.  Elk van de volgende secties worden de elementen van het hoogste niveau en en de inhoud in een oplossing.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parameters

[Parameters](../resource-group-authoring-templates.md#parameters) zijn waarden die u nodig hebt van de gebruiker bij de installatie van de oplossing.  Er zijn standaard parameters die alle oplossingen heeft en u kunt aanvullende parameters zoals vereist voor uw specifieke oplossing toevoegen.  Gebruikers, hoe parameterwaarden vindt u bij de installatie van uw oplossing hangt voor de desbetreffende parameter en hoe de oplossing wordt geïnstalleerd.


Wanneer een gebruiker uw beheeroplossing via de [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-solutions) of [Azure QuickStart sjablonen installeert](operations-management-suite-solutions.md#finding-and-installing-solutions) zijn hierom wordt gevraagd om te selecteren van een [werkruimte OMS en automatisering-account](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account).  Deze worden gebruikt om te vullen de waarden van elk van de standaard parameters.  De gebruiker wordt niet gevraagd rechtstreeks verstrekken waarden voor de standaard parameters, maar ze worden gevraagd waarden op te geven voor eventuele aanvullende parameters.

Wanneer de gebruiker de oplossing [een andere methode](operations-management-suite-solutions.md#finding-and-installing-solutions)installeert, moeten deze een waarde opgeven voor alle standaard parameters en alle aanvullende parameters.

Hieronder vindt u een voorbeeld-parameter.

    "Daily Start Time": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

De volgende tabel beschrijft de kenmerken van een parameter.

| Kenmerk | Beschrijving |
|:--|:--|
| type        | Het gegevenstype voor de parameter. De invoer weergegeven voor de gebruiker, is afhankelijk van het gegevenstype.<br><br>BOOL - vervolgkeuzelijst<br>tekenreeks - tekstvak<br>Integer - tekstvak<br>SecureString - veld voor wachtwoord<br> |
| categorie    | Optionele categorie voor de parameter.  Parameters in dezelfde categorie worden gegroepeerd. |
| besturingselement     | Extra functionaliteit voor tekenreeksparameters.<br><br>DateTime - Datetime-besturingselement wordt weergegeven.<br>GUID - Guid waarde wordt automatisch gegenereerd en de parameter niet wordt weergegeven. |
| Beschrijving | Optionele beschrijving voor de parameter.  Weergegeven in een tekstballon informatie naast de parameter. |


### <a name="standard-parameters"></a>Standaard parameters
De volgende tabel bevat de standaard parameters voor alle managementoplossingen.  Deze waarden worden ingevuld voor de gebruiker in plaats van waarin u wordt gevraagd deze als uw oplossing vanuit de Azure Marketplace of Quickstart sjablonen is geïnstalleerd.  De gebruiker moet waarden opgeven voor hen als de oplossing is geïnstalleerd met een andere methode.

>[AZURE.NOTE]De gebruikersinterface van de Azure Marketplace en Quickstart sjablonen verwacht de parameternamen in de tabel.  Als u verschillende parameternamen gebruikt wordt de gebruiker gevraagd deze en ze wordt niet automatisch ingevuld.


| Parameter | Type | Beschrijving |
|:--|:--|:--|
| Accountnaam       | tekenreeks |  Azure automatisering accountnaam. |
| pricingTier       | tekenreeks | Prijzen laag zowel Log Analytics-werkruimte en automatisering Azure-account. |
| regionId          | tekenreeks | De regio van de Azure automatisering-account. |
| Naam oplossing      | tekenreeks | Naam van de oplossing. |
| Werkruimtenaam     | tekenreeks | Meld u aan de naam van de werkruimte Analytics. |
| workspaceRegionId | tekenreeks | De regio van de werkruimte Log Analytics. |





### <a name="sample"></a>Voorbeeld
Hier volgt een voorbeeld parameter-item naar een oplossing.  Dit omvat alle de standaard parameters en twee aanvullende parameters in dezelfde categorie.

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "A valid Azure Automation account name"
            }
        },
        "workspaceRegionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        },
        "jobIdGuid": {
        "type": "string",
            "metadata": {
                "description": "GUID for a runbook job",
                "control": "guid",
                "category": "Schedule"
            }
        },
        "startTime": {
            "type": "string",
            "metadata": {
                "description": "Time for starting the runbook.",
                "control": "datetime",
                "category": "Schedule"
            }
        }


U verwijzen naar parameterwaarden in andere elementen van de oplossing met de syntaxis van de **parameters (parameternaam)**.  Voor toegang tot de naam van de werkruimte, zou u bijvoorbeeld **parameters('workspaceName')** gebruiken 

## <a name="variables"></a>Variabelen

Het element **variabelen** bevat de waarden die u wilt gebruiken in de rest van de oplossing.  Deze waarden worden niet blootgesteld aan de gebruiker de oplossing installeert.  Ze zijn bedoeld om te leveren van de auteur met één enkele locatie waar deze waarden die meerdere keren kunnen worden gebruikt in de oplossing kunnen beheren. U moet een waarden specifieke plaatsen naar de oplossing in variabelen niet hardcoding ze in het element **resources** .  Hiermee maakt de code beter leesbaar en kunt u gemakkelijk wijzigen deze waarden in nieuwere versies.

Hier volgt een voorbeeld van een element **variabelen** met normale parameters in oplossingen gebruikt.

    "variables": { 
        "SolutionVersion": "1.1", 
        "SolutionPublisher": "Contoso", 
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

U verwijzen naar de variabele waarden van de oplossing met de syntaxis van de **variabelen ('variabelennaam')**.  Voor toegang tot de variabele naam oplossing, zou u bijvoorbeeld **variables('solutionName')** gebruiken 


## <a name="resources"></a>Resources

Het element **resources** bepaalt de verschillende bronnen opgenomen in uw beheeroplossing voor.  Dit is het grootste en meest complexe deel van de sjabloon.  Resources worden gedefinieerd met behulp van de volgende structuur.  

    "resources": [
        {
            "name": "<name-of-the-resource>",           
            "apiVersion": "<api-version-of-resource>",
            "type": "<resource-provider-namespace/resource-type-name>",     
            "location": "<location-of-resource>",
            "tags": "<name-value-pairs-for-resource-tagging>",
            "comments": "<your-reference-notes>",
            "dependsOn": [
                "<array-of-related-resource-names>"
            ],
            "properties": "<unique-settings-for-the-resource>",
            "resources": [
                "<array-of-child-resources>"
            ]
        }
    ]

### <a name="dependencies"></a>Afhankelijkheden
De elementen **dependsOn** bevat een [afhankelijkheid](../resource-group-define-dependencies.md) op een andere resource.  Wanneer de oplossing is geïnstalleerd, wordt een resource is niet gemaakt totdat alle bijbehorende afhankelijkheden zijn gemaakt.  Bijvoorbeeld uw oplossing mogelijk [een runbook starten](operations-management-suite-solutions-resources-automation.md#runbooks) wanneer dit geïnstalleerd met behulp van een [taak, resource](operations-management-suite-solutions-resources-automation.md#automation-jobs).  De resource taak, zijn afhankelijk van de resource runbook om ervoor te zorgen dat het runbook is gemaakt voordat de taak is gemaakt.

### <a name="oms-workspace-and-automation-account"></a>OMS werkruimte en automatisering-account
Oplossingen is vereist voor een [werkruimte OMS](../log-analytics/log-analytics-manage-access.md) naar weergaven bevatten en een [automatisering account](../automation/automation-security-overview.md#automation-account-overview) runbooks en verwante resources bevat.  Deze moeten beschikbaar zijn voordat de resources in de oplossing worden gemaakt en niet in de oplossing zelf moeten worden gedefinieerd.  De gebruiker wordt [een werkruimte en account opgeven](operations-management-suite-solutions.md#oms-workspace-and-automation-account) wanneer ze uw-oplossing implementeert, maar als auteur kunt u overwegen de volgende punten.


## <a name="solution-resource"></a>Oplossing resource
Elke oplossing is vereist voor de invoer in een resource in het element **resources** die de oplossing zelf definieert.  Dit heeft een type **Microsoft.OperationsManagement/solutions**.  Hier volgt een voorbeeld van een resource oplossing.  De verschillende elementen worden beschreven in de onderstaande secties.

    "name": "[concat(variables('SolutionName'), '[ ' ,parameters('workspacename'), ' ]')]",
    "location": "[parameters('workspaceRegionId')]",
    "tags": { },
    "type": "Microsoft.OperationsManagement/solutions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]",
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
    ]
    "properties": {
        "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
        "referencedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]"
        ],
        "containedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
        ]
    },
    "plan": {
        "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
        "Version": "[variables('SolutionVersion')]",
        "product": "AzureSQLAnalyticSolution",
        "publisher": "[variables('SolutionPublisher')]",
        "promotionCode": ""
    }

### <a name="solution-name"></a>De oplossingsnaam van de
Naam van de oplossing moet uniek zijn in uw Azure-abonnement. De aanbevolen waarde moet worden gebruikt, is het volgende.  Hiermee wordt een variabele met de naam van de **naam oplossing** voor de naam en de parameter **Werkruimtenaam** gebruikt om ervoor te zorgen dat de naam uniek is.

    [concat(variables('SolutionName'), ' [' ,parameters('workspaceName'), ']')]

Dit wilt omzetten in een naam als volgt uit.

    My Solution Name [MyWorkspace]
 

### <a name="dependencies"></a>Afhankelijkheden
De resource oplossing moet een [afhankelijkheid](../resource-group-define-dependencies.md) op elke andere bron in de oplossing hebben, aangezien ze bestaan moeten voordat de oplossing kan worden gemaakt.  U doen dit door het toevoegen van een vermelding voor elke resource in het element **dependsOn** .


### <a name="properties"></a>Eigenschappen
De resource oplossing heeft de eigenschappen in de volgende tabel.  Dit geldt ook voor de resources waarnaar wordt verwezen en opgenomen door de oplossing waarin de manier waarop de resource wordt beheerd nadat de oplossing is geïnstalleerd.  Elke resource in de oplossing moet worden vermeld in de **referencedResources** of de eigenschap **containedResources** .

| Eigenschap | Beschrijving |
|:--|:--|
| workspaceResourceId | ID van de werkruimte OMS in het formulier * <Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<de naam van de werkruimte\>*. |
| referencedResources   | Lijst met bronnen in de oplossing aan die niet moeten worden verwijderd wanneer de oplossing wordt verwijderd. |
| containedResources   | Lijst met bronnen in de oplossing aan die moet worden verwijderd wanneer de oplossing is verwijderd. |

Het bovenstaande voorbeeld is voor een oplossing met een runbook, een planning en weergave.  De planning en runbook zijn *waarnaar wordt verwezen* in het element **Eigenschappen** , zodat ze niet worden verwijderd wanneer de oplossing wordt verwijderd.  De weergave is *opgenomen* , zodat deze wordt verwijderd wanneer de oplossing wordt verwijderd.


### <a name="plan"></a>Plannen
De entiteit **abonnement** van de resource oplossing heeft de eigenschappen in de volgende tabel. 

| Eigenschap | Beschrijving |
|:--|:--|
| naam          | Naam van de oplossing. |
| Versie       | Versie van de oplossing bepaald door de auteur. |
| product       | Unieke tekenreeks aan te geven van de oplossing. |
| Publisher     | Uitgever van de oplossing. |


## <a name="other-resources"></a>Overige informatiebronnen
U kunt de details en voorbeelden van resources die voor managementoplossingen in de volgende artikelen gelden.

- [Weergaven en dashboards](operations-management-suite-solutions-resources-views.md)
- [Automatisering resources](operations-management-suite-solutions-resources-automation.md)

## <a name="testing-a-management-solution"></a>Een beheeroplossing testen
Het wordt aanbevolen dat u deze via [Test-AzureRmResourceGroupDeployment](../resource-group-template-deploy.md#deploy-with-powershell)testen voordat u uw beheeroplossing wordt geïmplementeerd.  Hiermee wordt uw oplossing bestands- en help die u problemen ondervindt identificeren voordat u probeert te implementeren valideren.



## <a name="next-steps"></a>Volgende stappen

- Informatie over de details van [Azure resourcemanager Authoring sjablonen](../resource-group-authoring-templates.md).
- Zoeken [Azure Quickstart sjablonen](https://azure.microsoft.com/documentation/templates) voor voorbeelden van verschillende resourcemanager sjablonen.
- De details voor het [toevoegen van weergaven die u wilt een beheeroplossing](operations-management-suite-solutions-resources-views.md)weergeven.
- De details weergeven voor [automatisering resources aan een beheeroplossing toe te voegen](operations-management-suite-solutions-resources-automation.md).
 