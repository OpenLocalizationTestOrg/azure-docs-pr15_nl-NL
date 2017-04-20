<properties
   pageTitle="Automatisering resources in OMS oplossingen | Microsoft Azure"
   description="Oplossingen in OMS bevatten meestal runbooks in Azure automatisering processen, zoals het verzamelen en verwerken van controlegegevens automatiseren.  In dit artikel wordt beschreven hoe runbooks en hun verwante resources opnemen in een oplossing."
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
   ms.date="10/19/2016"
   ms.author="bwren" />

# <a name="automation-resources-in-oms-solutions-preview"></a>Automatisering resources in OMS oplossingen (Preview)

>[AZURE.NOTE]Dit is voorlopige documentatie voor het maken van oplossingen in OMS die momenteel beschikbaar in de Preview-versie zijn. Een schema hieronder beschreven kan worden gewijzigd.   

[Oplossingen in OMS](operations-management-suite-solutions.md) bevatten meestal runbooks in Azure automatisering processen, zoals het verzamelen en verwerken van controlegegevens automatiseren.  Naast het runbooks bevat automatisering accounts activa, zoals variabelen en planningen die ondersteuning bieden voor de runbooks gebruikt in de oplossing.  In dit artikel wordt beschreven hoe runbooks en hun verwante resources opnemen in een oplossing.

>[AZURE.NOTE]In de voorbeelden in dit artikel parameters en variabelen die zijn vereist of gemeenschappelijke oplossingen en die worden beschreven in [oplossingen maken in bewerkingen Management Suite Kantoorbeheersysteem](operations-management-suite-solutions-creating.md) gebruiken 


## <a name="prerequisites"></a>Vereisten voor
In dit artikel wordt ervan uitgegaan dat u al bekend met het [maken van een oplossing voor beheer](operations-management-suite-solutions-creating.md) en de structuur van een oplossingbestand bent.

## <a name="samples"></a>Voorbeelden
U kunt resourcemanager voorbeeldsjablonen voor automatisering resources krijgen van de [QuickStart sjablonen in GitHub](https://github.com/azureautomation/automation-packs/tree/master/101-sample-automation-resource-templates).

## <a name="automation-account"></a>Automatisering-account
Alle resources in Azure automatisering bevinden zich in een [automatisering-account](../automation/automation-security-overview.md#automation-account-overview).  Zoals beschreven in [de werkruimte OMS en automatisering account](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account) het account dat automatisering geen deel uitmaakt van de oplossing management, maar moet bestaan voordat de oplossing is geïnstalleerd.  Als deze niet beschikbaar is, klikt u vervolgens mislukt de oplossing installatie.

De naam van het account van hun automatisering is naam van elke resource automatisering.  Dit is in de oplossing met de parameter **accountnaam** zoals in het volgende voorbeeld van een resource runbook.
    
    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooks
[Azure automatisering runbook](../automation/automation-runbook-types.md) resources zijn een soort **Microsoft.Automation/automationAccounts/runbooks** en hebt de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| runbookType | Hiermee geeft u de typen van het runbook. <br><br> Script - PowerShell-script <br>PowerShell - PowerShell-werkstroom <br> GraphPowerShell - runbook voor grafische PowerShell-script <br> GraphPowerShellWorkflow - grafische PowerShell werkstroom runbook   |
| logProgress | Geeft aan of [voortgang records](../automation/automation-runbook-output-and-messages.md) moeten worden gegenereerd voor het runbook. |
| logVerbose  | Geeft aan of [uitgebreide records](../automation/automation-runbook-output-and-messages.md) moeten worden gegenereerd voor het runbook. |
| Beschrijving | Optionele beschrijving voor het runbook. |
| publishContentLink | Hiermee geeft u de inhoud van het runbook. <br><br>URI - Uri tot de inhoud van het runbook.  Dit is een bestand .ps1 voor PowerShell en Script runbooks en een geëxporteerde grafische runbook-bestand voor een grafiek runbook.  <br> versie - versie van het runbook voor uw eigen bijhouden. |

Er is een voorbeeld van een resource runbook hieronder.  In dit geval deze het runbook opgehaald uit de [Galerie met PowerShell](https://www.powershellgallery.com).

    "name": "[concat(parameters('accountName'), '/Start-AzureV2VMs'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]"
    ],
    "tags": { },
    "properties": {
        "runbookType": "PowerShell",
        "logProgress": "true",
        "logVerbose": "true",
        "description": "",
        "publishContentLink": {
            "uri": "https://www.powershellgallery.com/api/v2/items/psscript/package/Update-ModulesInAutomationToLatestVersion/1.0/Start-AzureV2VMs.ps1",
            "version": "1.0"
        }
    }

### <a name="starting-a-runbook"></a>Een runbook starten
Er zijn twee manieren een runbook starten in een beheeroplossing.

- Als u wilt starten het runbook wanneer de oplossing is geïnstalleerd, door een [taak, resource](#automation-jobs) te maken, zoals hieronder beschreven.
- Als u wilt het runbook volgens een schema, door een [resource planning](#schedules) te maken, zoals hieronder beschreven. 


## <a name="automation-jobs"></a>Automatisering taken
Om te starten een runbook wanneer de beheeroplossing is geïnstalleerd, kunt u de resource van een **taak** maken.  Taak resources zijn een soort **Microsoft.Automation/automationAccounts/jobs** en hebt de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| runbook    | Één **naam** entiteit met de naam van het runbook.  |
| parameters | Entiteit voor elke parameter die is vereist door het runbook. |


De taak bevat de naam van de runbook en eventuele parameterwaarden worden verzonden naar het runbook.  De taak moet [zijn afhankelijk van](operations-management-suite-solutions-creating.md#resources) het runbook dat deze wordt gestart sinds het runbook voordat u de taak moet worden gemaakt.  U kunt ook afhankelijkheden maken op andere taken voor runbooks die moet worden voltooid voordat de huidige rij.

De naam van de resource van een taak moet bevatten een GUID die meestal door een parameter is toegewezen.  U vindt meer informatie over GUID parameters in [oplossingen maken in bewerkingen Management Suite Kantoorbeheersysteem](operations-management-suite-solutions-creating.md#parameters).  

Hier volgt een voorbeeld van een taak resource waarmee een runbook kan worden gestart wanneer de beheeroplossing is geïnstalleerd.  Een andere runbooks moeten worden uitgevoerd voordat u dit een wordt gestart, zodat deze afhankelijk van de taken voor die runbook is.  De details in voor de taak zijn opgenomen in parameters en variabelen plaats rechtstreeks worden opgegeven.

    {
        "name": "[concat(parameters('accountName'), '/', parameters('startVmsRunbookGuid'))]",
        "type": "Microsoft.Automation/automationAccounts/jobs",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "location": "[parameters('regionId')]",
        "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]",
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/jobs/', parameters('initialPrepRunbookGuid'))]"
        ],
        "tags": { },
        "properties": {
            "runbook": {
                "name": "[variables('startVmsRunbookName')]"
            },
            "parameters": {
                "AzureConnectionAssetName": "[variables('AzureConnectionAssetName')]",
                "ResourceGroupName": "[variables('ResourceGroupName')]"
            }
        }
    }


## <a name="certificates"></a>Certificaten
[Azure automatisering certificaten](../automation/automation-certificates.md) een soort **Microsoft.Automation/automationAccounts/certificates** en hebt de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| base64Value   | Basiskalender 64 waarde voor het certificaat. |
| vingerafdruk    | Blauwdruk voor het certificaat. |

Er is een voorbeeld van een resource certificaat onder.

    "name": "[concat(parameters('accountName'), '/MyCertificate')]",
    "type": "certificates",
    "apiVersion": "2015-01-01-preview",
    "location": "[parameters('regionId')]",
    "tags": {},
    "dependsOn": [
    ],
    "properties": {
        "base64Value": "MIIC1jCCAA8gAwIAVgIYJY4wXCXH/YAHMtALh7qEFzANAgkqhkiG5w0AAQUFGDAUMRIwEBYDVQQDEwlsA3NhAGevc3QqHhcNMTZwNjI5MHQxMjAsWhcNOjEwNjI5NKWwMDAwWjAURIwEAYDVQQBEwlsA2NhAGhvc3QwggEiMA0GCSqGSIA3DQEAAQUAA4IADwAwggEKAoIAAQDIyzv2A0RUg1/AAryI9W1DGAHAqqGdlFfTkUSDfv+hEZTAwKv0p8daqY6GroT8Du7ctQmrxJsy8JxIpDWxUaWwXtvv1kR9eG9Vs5dw8gqhjtOwgXvkOcFdKdQwA82PkcXoHlo+NlAiiPPgmHSELGvcL1uOgl3v+UFiiD1ro4qYqR0ITNhSlq5v2QJIPnka8FshFyPHhVtjtKfQkc9G/xDePW8dHwAhfi8VYRmVMmJAEOLCAJzRjnsgAfznP8CZ/QUczPF8LuTZ/WA/RaK1/Arj6VAo1VwHFY4AZXAolz7xs2sTuHplVO7FL8X58UvF7nlxq48W1Vu0l8oDi2HjvAgMAAAGjJDAiMAsGA1UdDwREAwIEsDATAgNVHSUEDDAKAggrAgEFNQcDATANAgkqhkiG9w0AAQUFAAOCAQEAk8ak2A5Ug4Iay2v0uXAk95qdAthJQN5qIVA13Qay8p4MG/S5+aXOVz4uMXGt18QjGds1A7Q8KDV4Slnwn95sVgA5EP7akvoGXhgAp8Dm90sac3+aSG4fo1V7Y/FYgAgpEy4C/5mKFD1ATeyyhy3PmF0+ZQRJ7aLDPAXioh98LrzMZr1ijzlAAKfJxzwZhpJamAwjZCYqiNZ54r4C4wA4QgX9sVfQKd5e/gQnUM8gTQIjQ8G2973jqxaVNw9lZnVKW3C8/QyLit20pNoqX2qQedwsqg3WCUcPRUUqZ4NpQeHL/AvKIrt158zAfU903yElAEm2Zr3oOUR4WfYQ==",
        "thumbprint": "F485CBE5569F7A5019CB68D7G6D987AC85124B4C"
    }

## <a name="credentials"></a>Referenties
[Azure automatisering referenties](../automation/automation-credentials.md) zijn een soort **Microsoft.Automation/automationAccounts/credentials** en hebt de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Gebruikersnaam   | Gebruikersnaam voor de referentie. |
| wachtwoord   | Wachtwoord voor de referentie. |

Er is een voorbeeld van een bron referentie hieronder.

    "name": "[concat(parameters('accountName'), '/', variables('credentialName'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks/credentials",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "userName": "User01",
        "password": "password"
    }


## <a name="schedules"></a>Schema 's
[Azure automatisering planningen](../automation/automation-schedules.md) een soort **Microsoft.Automation/automationAccounts/schedules** en hebt de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Beschrijving | Optionele beschrijving voor de planning. |
| Starttijd   | Hiermee geeft u de begintijd van een planning als een DateTime-object. Een tekenreeks kan worden verstrekt als deze kan worden geconverteerd naar een geldige DateTime. |
| isEnabled   | Geeft aan of de planning is ingeschakeld. |
| interval    | Het type interval voor de planning.<br><br>dag<br>uur |
| frequentie   | De frequentie die de planning moet worden geactiveerd in hoeveel dagen of uren. |

Een voorbeeld van een resource planning is hieronder.

    "name": "[concat(parameters('accountName'), '/', variables('variableName'))]",
    "type": "microsoft.automation/automationAccounts/schedules",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Schedule for StartByResourceGroup runbook",
        "startTime": "10/30/2016 12:00:00",
        "isEnabled": "true",
        "interval": "day",
        "frequency": "1"
    }


## <a name="variables"></a>Variabelen
[Azure automatisering variabelen](../automation/automation-variables.md) een soort **Microsoft.Automation/automationAccounts/variables** en hebt de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Beschrijving | Optionele beschrijving voor de variabele. |
| isEncrypted | Geeft aan of de variabele moet worden gecodeerd. |
| type        | Het gegevenstype voor de variabele. |
| waarde       | De waarde voor de variabele. |

Er is een voorbeeld van een variabele resource onder.

    "name": "[concat(parameters('accountName'), '/', variables('StartTargetResourceGroupsAssetName')) ]",
    "type": "microsoft.automation/automationAccounts/variables",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Description for the variable.",
        "isEncrypted": "true",
        "type": "string",
        "value": "'This is a string value.'"
    }


## <a name="modules"></a>Modules
Uw beheeroplossing hoeft niet te definiëren die worden gebruikt door uw runbooks, omdat ze altijd beschikbaar voor [globale modules](../automation/automation-integration-modules.md) .  U moet een resource voor alle andere module die wordt gebruikt door uw runbooks opnemen en het runbook moet zijn afhankelijk van de module resource om ervoor te zorgen dat deze wordt gemaakt voordat het runbook.

[Integratie van modules](../automation/automation-integration-modules.md) een soort **Microsoft.Automation/automationAccounts/modules** en hebt de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| contentLink | Hiermee geeft u de inhoud van de module. <br><br>URI - Uri tot de inhoud van het runbook.  Dit is een bestand .ps1 voor PowerShell en Script runbooks en een geëxporteerde grafische runbook-bestand voor een grafiek runbook.  <br> versie - versie van het runbook voor uw eigen bijhouden. |

Er is een voorbeeld van een resource module hieronder.

    {       
        "name": "[concat(parameters('accountName'), '/', variables('OMSIngestionModuleName'))]",
        "type": "Microsoft.Automation/automationAccounts/modules",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "properties": {
            "contentLink": {
                "uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
            }
        }
    }

### <a name="updating-modules"></a>Modules bijwerken
Als u een oplossing die een runbook met een schema bevat bijwerken en de nieuwe versie van uw oplossing een nieuwe module die wordt gebruikt door deze runbook heeft, kan het runbook de oude versie van de module gebruiken.  U moet de volgende runbooks opnemen in uw oplossing en een taak te laat u ze voordat u een andere runbooks definiëren.  Dit zorgt ervoor dat dat alle modules worden bijgewerkt als vereist voordat de runbooks worden geladen.

- [Update-ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) zorgt ervoor dat alle modules die worden gebruikt door runbooks in uw oplossing zijn de meest recente versie.  
- [ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) worden alle bronnen planning om ervoor te zorgen dat de runbooks gekoppeld aan deze met gebruiken de meest recente modules registreren.


Hier volgt een voorbeeld van de vereiste elementen van een oplossing voor de ondersteuning van de module-update.
    
    "parameters": {
        "ModuleImportGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        },
        "ReRegisterRunbookGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        }
    },

    "variables": {
        "ModuleImportRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ModuleImportRunbookUri": "https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/Content/Update-ModulesInAutomationToLatestVersion.ps1",
        "ModuleImportRunbookDescription": "Imports modules and also updates all Azure dependent modules that are in the same Automation Account.",
        "ReRegisterSchedulesRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ReRegisterSchedulesRunbookUri": "https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/Content/ReRegisterAutomationSchedule-MS-Mgmt.ps1",
        "ReRegisterSchedulesRunbookDescription": "Reregisters schedules to ensure that they use latest modules."
    },

    "resources": [
        {
            "name": "[concat(parameters('accountName'), '/', variables('ModuleImportRunbookName'))]",
            "type": "Microsoft.Automation/automationAccounts/runbooks",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "dependsOn": [
            ],
            "location": "[parameters('regionId')]",
            "tags": { },
            "properties": {
                "runbookType": "PowerShell",
                "logProgress": "true",
                "logVerbose": "true",
                "description": "[variables('ModuleImportRunbookDescription')]",
                "publishContentLink": {
                    "uri": "[variables('ModuleImportRunbookUri')]",
                    "version": "1.0.0.0"
                }
            }
        },
        {
            "name": "[concat(parameters('accountName'), '/', parameters('ModuleImportGuid'))]",
            "type": "Microsoft.Automation/automationAccounts/jobs",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "location": "[parameters('regionId')]",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ModuleImportRunbookName'))]"
            ],
            "tags": { },
            "properties": {
                "runbook": {
                    "name": "[variables('ModuleImportRunbookName')]"
                },
                "parameters": {
                    "ResourceGroupName": "[resourceGroup().name]",
                    "AutomationAccountName": "[parameters('accountName')]",
                    "NewModuleName": "AzureRM.Insights"
                }
            }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('ReRegisterSchedulesRunbookName'))]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "PowerShell",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('ReRegisterSchedulesRunbookDescription')]",
            "publishContentLink": {
              "uri": "[variables('ReRegisterSchedulesRunbookUri')]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', parameters('reRegisterSchedulesGuid'))]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ReRegisterSchedulesRunbookName'))]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('ReRegisterSchedulesRunbookName')]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
        }
    ]


## <a name="next-steps"></a>Volgende stappen

- [Een weergave toevoegen aan uw oplossing](operations-management-suite-solutions-resources-views.md) verzamelde gegevens kunt visualiseren.