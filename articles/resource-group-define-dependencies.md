<properties
   pageTitle="Afhankelijkheden in resourcemanager sjablonen | Microsoft Azure"
   description="Beschreven hoe u een resource instellen als afhankelijk is van een andere resource tijdens de implementatie om ervoor te zorgen resources zijn geïmplementeerd in de juiste volgorde."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="defining-dependencies-in-azure-resource-manager-templates"></a>Afhankelijkheden definiëren in resourcemanager Azure-sjablonen

Voor een bepaalde bron, kunnen er andere bronnen die u moeten bestaan voordat u de resource wordt geïmplementeerd. Bijvoorbeeld moet een SQL server bestaan voordat u probeert te implementeren van een SQL-database. U kunt deze relatie definiëren door te markeren van een resource als afhankelijk van de andere resource. Meestal u een afhankelijkheid met het element **dependsOn** definieert, maar u kunt het ook definiëren door middel van de functie **verwijzing** . 

Resourcemanager evalueert de afhankelijkheden tussen bronnen en deze implementeert in de afhankelijke volgorde. Wanneer u bronnen niet afhankelijk zijn van elkaar, worden deze door resourcemanager parallel implementeert.

## <a name="dependson"></a>dependsOn

Binnen uw sjabloon kunt het element dependsOn u een resource definiëren als afhankelijke op een of meer resources. De waarde kan een door komma's gescheiden lijst met resourcenamen zijn. 

Het volgende voorbeeld ziet u een virtuele machine schaal instellen die afhankelijk is van een taakverdeling, virtuele netwerk en een lus waarmee meerdere opslag-accounts. Deze andere resources worden niet weergegeven in het volgende voorbeeld, maar ze moeten bestaan ergens anders in de sjabloon.

    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "name": "[variables('namingInfix')]",
      "location": "[variables('location')]",
      "apiVersion": "2016-03-30",
      "tags": {
        "displayName": "VMScaleSet"
      },
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      ...
    }

Als u wilt een afhankelijkheid tussen een resource en resources die zijn gemaakt door een lus kopie definiëren, stelt u het element dependsOn op naam van de lus. Zie [meerdere exemplaren van resources in Azure resourcemanager maken](resource-group-create-multiple.md)voor een voorbeeld.

Terwijl u mogelijk worden onder een hoek met dependsOn relaties tussen uw resources toewijzen, is het belangrijk is voor meer informatie over waarom u doet dit omdat dit van invloed op de prestaties van uw implementatie zijn kan. Als u wilt documenteren hoe resources zijn onderling verbonden, bijvoorbeeld, is het dependsOn niet de juiste aanpak. U kunt geen indexquery welke resources zijn gedefinieerd in het hoofdelement dependsOn na implementatie. Met behulp van dependsOn u mogelijk van invloed zijn op implementatietijd omdat resourcemanager implementeert geen in parallelle twee resources die een afhankelijkheid hebben. Naar het document van relaties tussen resources, in plaats hiervan gebruik [resource koppelen](resource-group-link-resources.md).

## <a name="child-resources"></a>Onderliggende resources

De eigenschap resources kunt u opgeven onderliggende resources die zijn gerelateerd aan de resource wordt gedefinieerd. Onderliggende resources kunnen alleen worden voor gedefinieerde vijf niveaus. Het is belangrijk te weten dat een impliciete afhankelijkheid niet wordt gemaakt tussen de resource bovenliggende en onderliggende resources. Als u de onderliggende-resource kan worden geïmplementeerd na de bovenliggende resource nodig hebt, moet u deze afhankelijkheid met de eigenschap dependsOn expliciet vermelden. 

Elke resource bovenliggende accepteert alleen bepaalde resourcetypen als onderliggende resources. De geaccepteerde resourcetypen zijn opgegeven in het [sjabloon schema](https://github.com/Azure/azure-resource-manager-schemas) van de bovenliggende resource. De naam van de onderliggende brontype bevat de naam van het type van de bovenliggende resource, zoals **Microsoft.Web/sites/config** en **Microsoft.Web/sites/extensions** zijn beide onderliggende resources van het **Microsoft.Web/sites**.

Het volgende voorbeeld ziet u een SQL server en SQL-database. Zoals u ziet dat een expliciete afhankelijkheid is gedefinieerd tussen de SQL-database en SQL server, hoewel de database een subtitel van de server is.

    "resources": [
      {
        "name": "[variables('sqlserverName')]",
        "type": "Microsoft.Sql/servers",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "SqlServer"
        },
        "apiVersion": "2014-04-01-preview",
        "properties": {
          "administratorLogin": "[parameters('administratorLogin')]",
          "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
        },
        "resources": [
          {
            "name": "[parameters('databaseName')]",
            "type": "databases",
            "location": "[resourceGroup().location]",
            "tags": {
              "displayName": "Database"
            },
            "apiVersion": "2014-04-01-preview",
            "dependsOn": [
              "[variables('sqlserverName')]"
            ],
            "properties": {
              "edition": "[parameters('edition')]",
              "collation": "[parameters('collation')]",
              "maxSizeBytes": "[parameters('maxSizeBytes')]",
              "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
            }
          }
        ]
      }
    ]


## <a name="reference-function"></a>verwijzing, functie

De [verwijzing-functie](resource-group-template-functions.md#reference) kunt een expressie naar afgeleid van de waarde van de naam van andere JSON en waardeparen of runtime resources. Verwijzing expressies declareren impliciet dat een resource afhankelijk van een andere is. 

    reference('resourceName').propertyPath

U kunt met dit element of het element dependsOn afhankelijkheden opgeven, maar u hoeft niet te gebruiken voor dezelfde resource afhankelijke. Gebruik een impliciete verwijzing om te voorkomen dat per ongeluk het toevoegen van een onnodige afhankelijkheid waar mogelijk.

Zie [verwijzing, functie](resource-group-template-functions.md#reference)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het maken van Azure resourcemanager sjablonen, [ontwerpfuncties sjablonen](resource-group-authoring-templates.md). 
- Zie [functies van de sjabloon](resource-group-template-functions.md)voor een lijst met de beschikbare functies in een sjabloon.

