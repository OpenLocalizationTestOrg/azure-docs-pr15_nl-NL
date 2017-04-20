<properties
   pageTitle="Sjablonen met resourcemanager gekoppeld | Microsoft Azure"
   description="Wordt beschreven hoe gekoppelde sjablonen gebruiken in een resourcemanager Azure-sjabloon om een sjabloonoplossing modulaire te maken. Ziet u hoe u geven parameterwaarden, Geef een parameterbestand en dynamisch gemaakte URL's."
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
   ms.date="09/02/2016"
   ms.author="tomfitz"/>

# <a name="using-linked-templates-with-azure-resource-manager"></a>Gekoppelde sjablonen met Azure Resource Manager gebruiken

Van binnen één resourcemanager Azure-sjabloon, kunt u koppelen aan een andere sjabloon, waarmee u om uw implementatie in een set gericht, doel-specifieke sjablonen. Net als met een toepassing in de verschillende codeklassen ontleedt, biedt uitgevouwen voordelen wat betreft testen, hergebruik en de leesbaarheid.  

U kunt parameters van een sjabloon belangrijkste doorgeven aan een gekoppelde sjabloon en deze parameters rechtstreeks kunnen toewijzen aan parameters of variabelen die worden aangeboden door de bellen sjabloon. De gekoppelde sjabloon kunt ook een uitvoervariabele doorgeven terug naar de bronsjabloon, zodat u een tweerichtingsvertrouwensrelatie tussen gegevensuitwisseling tussen sjablonen.

## <a name="linking-to-a-template"></a>Koppelen aan een sjabloon

U maken een koppeling tussen de twee sjablonen door toe te voegen een resource implementatie binnen de belangrijkste sjabloon die naar de gekoppelde sjabloon verwijst. U instellen de eigenschap **templateLink** met de URI van de gekoppelde sjabloon. U kunt opgeven parameterwaarden voor de gekoppelde sjabloon door het opgeven van de waarden rechtstreeks in uw sjabloon of door te koppelen aan een parameterbestand. Het volgende voorbeeld wordt de eigenschap **parameters** kunt u rechtstreeks een parameterwaarde opgeven.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

De service resourcemanager moet kunnen toegang tot de gekoppelde sjabloon. U kunt een lokaal bestand of een bestand dat is alleen beschikbaar op het lokale netwerk voor de gekoppelde sjabloon opgeven. U kunt alleen een URI-waarde met **http** of **https**opgeven. Eén optie is de gekoppelde sjabloon plaatsen in een opslag-account en de URI gebruiken voor het item, zoals weergegeven in het volgende voorbeeld.

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }

Hoewel de gekoppelde sjabloon moet extern beschikbaar zijn, hoeft het niet in het algemeen beschikbaar is voor het publiek. U kunt uw sjabloon toevoegen aan een account voor persoonlijke opslag die toegankelijk is voor alleen de eigenaar van de opslag-account. Maakt u een gedeelde handtekening (SA's) van het toegangstoken voor toegang tijdens de implementatie. U toevoegen die token SA's aan de URI voor de gekoppelde sjabloon. Zie voor instructies voor het instellen van een sjabloon in een opslag-account en een SA's token genereren, [Deploy resources met resourcemanager sjablonen en Azure PowerShell](resource-group-template-deploy.md) of [Deploy resources met resourcemanager sjablonen en Azure CLI](resource-group-template-deploy-cli.md). 

Het volgende voorbeeld ziet een sjabloon bovenliggende die verwijzen naar een andere sjabloon. De gekoppelde sjabloon wordt geopend met een token SA's dat als u een parameter wordt doorgegeven.

    "parameters": {
        "sasToken": { "type": "securestring" }
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "incremental",
              "templateLink": {
                "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
                "contentVersion": "1.0.0.0"
              }
            }
        }
    ],

Hoewel het token wordt doorgegeven als een tekenreeks met secure, is de URI van de gekoppelde sjabloon, de token SA's, inclusief de implementatie-bewerkingen voor die resourcegroep aangemeld. Als u wilt beperken weergeven, stelt u een verloopbeleid voor het token.

## <a name="linking-to-a-parameter-file"></a>Koppelen aan een parameterbestand

Het volgende voorbeeld wordt de eigenschap **parametersLink** wilt koppelen aan een parameterbestand.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

De waarde URI voor het parameterbestand gekoppelde een lokaal bestand kan niet worden en moet **http** of **https**bevatten. De parameterbestand kan ook worden beperkt tot toegang via een token SA's.

## <a name="using-variables-to-link-templates"></a>Variabelen gebruiken om te koppelen van sjablonen

De voorgaande column werd hard gecodeerde URL waarden voor de sjabloon koppelingen. Deze methode werkt mogelijk een eenvoudige sjabloon, maar deze werkt niet goed tijdens het werken met een groot aantal modulaire sjablonen. U kunt in plaats daarvan een statische variabele met een basis-URL voor de belangrijkste sjabloon maken en vervolgens de URL's voor de gekoppelde sjablonen van die basis-URL dynamisch maken. Het voordeel van deze methode is u eenvoudig kunt verplaatsen of de sjabloon zich splitsen, omdat u alleen moet de statische variabele in de belangrijkste sjabloon wijzigen. De belangrijkste sjabloon wordt doorgegeven dat de juiste URI's overal in de opgesplitste sjabloon.

Het volgende voorbeeld ziet u hoe u twee URL's voor gekoppelde sjablonen (**sharedTemplateUrl** en **vmTemplate**) maken met een basis-URL. 

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

U kunt ook [deployment()](resource-group-template-functions.md#deployment) gebruikt om de basis-URL voor de huidige sjabloon, en dat gebruiken om toegang te krijgen van de URL voor de andere sjablonen op dezelfde locatie. Deze methode is handig als de sjabloonlocatie wordt gewijzigd (misschien vanwege versiebeheer) of als u wilt voorkomen dat hard kleurcodering URL's in het sjabloonbestand. 

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## <a name="conditionally-linking-to-templates"></a>Voorwaardelijk koppelingen naar sjablonen

U kunt met doorgeven in een parameterwaarde die wordt gebruikt om de URI van de gekoppelde sjabloon te koppelen aan verschillende sjablonen. Deze methode werkt prima wanneer u nodig hebt om op te geven tijdens de implementatie die gekoppeld sjabloon waarmee u wilt gebruiken. U kunt bijvoorbeeld een sjabloon wilt gebruiken voor een bestaand opslag-account en een andere sjabloon wilt gebruiken voor een nieuw account voor de opslag opgeven.

Het volgende voorbeeld ziet u een parameter voor een opslagaccountnaam en een parameter Geef aan of de opslag-account nieuw of bestaand.

    "parameters": {
        "storageAccountName": {
            "type": "String"
        },
        "newOrExisting": {
            "type": "String",
            "allowedValues": [
                "new",
                "existing"
            ]
        }
    },

Maakt u een variabele voor de sjabloon URI die de waarde van de nieuwe of bestaande parameter bevat.

    "variables": {
        "templatelink": "[concat('https://raw.githubusercontent.com/exampleuser/templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
    },

U opgeven die variabele waarde voor de resource implementatie.

    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "StorageAccountName": {
                        "value": "[parameters('storageAccountName')]"
                    }
                }
            }
        }
    ],

De URI worden omgezet in een sjabloon met de naam **existingStorageAccount.json** of **newStorageAccount.json**. Sjablonen maken voor deze URI's.

Het volgende voorbeeld ziet u de sjabloon **existingStorageAccount.json** .

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "String"
        }
      },
      "variables": {},
      "resources": [],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

Het volgende voorbeeld ziet de sjabloon **newStorageAccount.json** . Zoals u ziet dat accountsjabloon het opslagobject-account zoals het bestaande opslag in de uitvoer van wordt geretourneerd. De basispagina sjabloon werkt met een van beide gekoppelde sjabloon.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('StorageAccountName')]",
          "apiVersion": "2016-01-01",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "properties": {
          }
        }
      ],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('StorageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

## <a name="complete-example"></a>Voltooid voorbeeld

Het volgende voorbeeld-sjablonen weergeven een eenvoudigere schikking van gekoppelde sjablonen om te worden enkele van de concepten in dit artikel. Het wordt ervan uitgegaan dat de sjablonen zijn toegevoegd aan de dezelfde container in een account opslagruimte met openbare toegang is uitgeschakeld. De gekoppelde sjabloon wordt een waarde doorgegeven terug naar de belangrijkste sjabloon in de sectie **uitvoer** .

Het bestand **parent.json** bestaat uit:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "containerSasToken": { "type": "string" }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "linkedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
              "contentVersion": "1.0.0.0"
            }
          }
        }
      ],
      "outputs": {
        "result": {
          "type": "object",
          "value": "[reference('linkedTemplate').outputs.result]"
        }
      }
    }

Het bestand **helloworld.json** bestaat uit:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "variables": {},
      "resources": [],
      "outputs": {
        "result": {
            "value": "Hello World",
            "type" : "string"
        }
      }
    }
    
In PowerShell, moet u een token krijgen voor de container en implementeren van de sjablonen met:

    Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
    $token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
    New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ("https://storagecontosotemplates.blob.core.windows.net/templates/parent.json" + $token) -containerSasToken $token

In Azure CLI, moet u een token krijgen voor de container en implementeren van de sjablonen met de volgende code. U moet een naam voor de implementatie op dit moment opgeven wanneer u een sjabloon URI waarin een token SA's gebruikt.  

    expiretime=$(date -I'minutes' --date "+30 minutes")  
    azure storage container sas create --container templates --permissions r --expiry $expiretime --json | jq ".sas" -r
    azure group deployment create -g ExampleGroup --template-uri "https://storagecontosotemplates.blob.core.windows.net/templates/parent.json?{token}" -n tokendeploy  

U wordt gevraagd of u kunt opgeven van het token SA's als een parameter. U moet het token met **?**beginnen.

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over het definiëren van de implementatie-volgorde voor uw resources, [afhankelijkheden in Azure resourcemanager sjablonen definiëren](resource-group-define-dependencies.md)
- Meer informatie over het definiëren van een resource, maar veel exemplaren van het maken, raadpleegt u [meerdere exemplaren van resources in Azure resourcemanager maken](resource-group-create-multiple.md)
