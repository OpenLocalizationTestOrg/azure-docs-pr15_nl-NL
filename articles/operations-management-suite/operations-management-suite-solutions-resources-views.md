<properties
   pageTitle="Weergaven in bewerkingen Management Suite Kantoorbeheersysteem managementoplossingen | Microsoft Azure"
   description="Oplossingen in bewerkingen Management Suite Kantoorbeheersysteem bevat meestal een of meer weergaven om gegevens te visualiseren.  In dit artikel wordt beschreven hoe u wilt exporteren van een weergave die is gemaakt door de ontwerpfunctie voor weergaven en opnemen in een beheeroplossing. "
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
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Weergaven in bewerkingen Management Suite Kantoorbeheersysteem managementoplossingen (Preview)

>[AZURE.NOTE]Dit is voorlopige documentatie voor het maken van oplossingen in OMS die momenteel beschikbaar in de Preview-versie zijn. Een schema hieronder beschreven kan worden gewijzigd.    

[Oplossingen in bewerkingen Management Suite Kantoorbeheersysteem](operations-management-suite-solutions.md) bevat meestal een of meer weergaven om gegevens te visualiseren.  In dit artikel wordt beschreven hoe u wilt exporteren van een weergave die is gemaakt door de [Ontwerpfunctie voor weergaven](../log-analytics/log-analytics-view-designer.md) en opnemen in een beheeroplossing.  

>[AZURE.NOTE]In de voorbeelden in dit artikel parameters en variabelen die zijn vereist of gemeenschappelijke oplossingen en die worden beschreven in [oplossingen maken in bewerkingen Management Suite Kantoorbeheersysteem](operations-management-suite-solutions-creating.md) gebruiken 


## <a name="prerequisites"></a>Vereisten voor
In dit artikel wordt ervan uitgegaan dat u al bekend met het [maken van een oplossing voor beheer](operations-management-suite-solutions-creating.md) en de structuur van een oplossingbestand bent.


## <a name="overview"></a>Overzicht

Als u wilt een weergave in een beheeroplossing opnemen, maakt u een **resource** voor deze in het [oplossingsbestand](operations-management-suite-solutions-creating.md).  De JSON waarmee gedetailleerde configuratie van de weergave wordt beschreven is meestal complexe wel en niet iets dat de auteur van een typisch oplossing zou kunnen handmatig maken.  De meest gebruikte methode is de weergave met de [Ontwerpfunctie voor weergaven](../log-analytics/log-analytics-view-designer.md)maken, deze exporteren en voegt u de gedetailleerde configuratie toe aan de oplossing. 

De basisstappen beschreven om een weergave toevoegen aan een oplossing zijn als volgt.  Elke stap wordt nader in de secties hierna beschreven.

1. De weergave exporteren naar een bestand.
2. De resource weergave maken in de oplossing.
3. De details bekijken toevoegen.

## <a name="export-the-view-to-a-file"></a>De weergave exporteren naar een bestand
Volg de instructies op [Log Analytics weergave Designer](../log-analytics/log-analytics-view-designer.md) een weergave exporteren naar een bestand.  Het geëxporteerde bestand worden weergegeven in de indeling van JSON met dezelfde [elementen als het oplossingsbestand](operations-management-suite-solutions-creating.md#management-solution-files).  

Het element **resources** van het weergavebestand heeft een resource met een soort **Microsoft.OperationalInsights/workspaces** die de werkruimte OMS vertegenwoordigt.  Met dit element heeft een subelement met een soort **weergaven** die staat voor de weergave en de gedetailleerde configuratie bevat.  U de details van dit element kopiëren en deze vervolgens in uw oplossing te kopiëren.


## <a name="create-the-view-resource-in-the-solution"></a>De resource weergave maken in de oplossing
De volgende informatiebron voor de weergave toevoegen aan het element **resources** van uw oplossingsbestand.  Deze maakt gebruik van variabelen die hieronder worden beschreven dat moet u ook toevoegen.  Houd er rekening mee dat de eigenschappen van het **Dashboard** en **OverviewTile** tijdelijke aanduidingen die u door de corresponderende eigenschappen van het bestand geëxporteerde weergave overschreven zijn.
 
    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile": 
        }
    }

De volgende variabelen toevoegen aan het element [variabelen](operations-management-suite-solutions-creating.md#variables) van het oplossingsbestand en vervang de waarden met die van uw oplossing.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."


Houd er rekening mee dat u de hele weergave resource uit uw geëxporteerde weergave-bestand kopiëren kan, maar u moet de volgende wijzigingen werkt in uw oplossing.  

- Het **type** voor de resource weergave moet worden gewijzigd van **weergaven** in **Microsoft.OperationalInsights/workspaces**.
- De eigenschap **naam** voor de resource weergave moet worden gewijzigd en bevat de naam van de werkruimte.
- De afhankelijkheid van de werkruimte moet worden verwijderd omdat de werkruimte resource wordt niet gedefinieerd in de oplossing.
- **Weergavenaam** eigenschap moet worden toegevoegd aan de weergave.  De **Id**, de **naam**en de **weergavenaam** moeten alle overeenkomen.
- Parameternamen moeten worden gewijzigd zodat deze overeenkomen met de vereiste set parameters.
- Variabelen moeten worden gedefinieerd in de oplossing en gebruikt in de gewenste eigenschappen.

## <a name="add-the-view-details"></a>De details bekijken toevoegen
De bron weergeven in het bestand geëxporteerde weergave bevat twee elementen in het **Dashboard** en **OverviewTile** genoemde **Eigenschappen** -element waarin de gedetailleerde configuratie van de weergave.  Kopieer deze twee elementen en de inhoud naar het element **Eigenschappen** van de resource weergeven in uw oplossingsbestand. 

## <a name="example"></a>Voorbeeld
In het onderstaande voorbeeld ziet u bijvoorbeeld een eenvoudige oplossing-bestand met een weergave.  Drie puntjes (...) voor de inhoud van het **Dashboard** en **OverviewTile** om ruimte redenen worden weergegeven.


    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
        ]
    }




## <a name="next-steps"></a>Volgende stappen

- Informatie over de complete informatie over het [van beheeroplossingen](operations-management-suite-solutions-creating.md)te maken.
- [Automatisering runbooks in uw beheeroplossing voor](operations-management-suite-solutions-resources-automation.md)opnemen.